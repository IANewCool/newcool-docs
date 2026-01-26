# Patrones de Arquitectura: Pokémon Red/Blue

> Análisis de ingeniería del juego más exitoso de Game Boy
> Aplicable a: newcool-gameboy, newcool-atlas, newcool-search-v3

---

## 1. Contexto y Restricciones

### Hardware Game Boy (1996)

| Recurso | Límite | Implicación |
|---------|--------|-------------|
| CPU | Sharp LR35902 @ 4.19 MHz | Cada ciclo cuenta |
| RAM | 8 KB | Estado mínimo |
| VRAM | 8 KB | Tiles compartidos |
| ROM | 2 MB (cartridge) | Compresión obligatoria |
| Sprites | 40 total, 10 por scanline | Planificación visual |
| Tiles | 384 (8x8 px cada uno) | Reutilización |
| Colores | 4 tonos de verde | Contraste crítico |

### Equipo Game Freak

- **5 programadores** para todo el juego
- **2.5 años** de desarrollo adicional para versión occidental
- **Sin frameworks** - Assembly Z80 puro

---

## 2. Estructura del Código

Basado en [pret/pokered](https://github.com/pret/pokered):

```
pokered/
├── home/           → Código residente (siempre accesible)
│   ├── init.asm
│   ├── vblank.asm
│   ├── audio.asm
│   └── text.asm
│
├── engine/         → Lógica del juego (bank-switched)
│   ├── battle/
│   │   ├── core.asm
│   │   ├── trainer_ai.asm
│   │   └── animations.asm
│   ├── overworld/
│   │   ├── movement.asm
│   │   └── sprite_collisions.asm
│   ├── pokemon/
│   │   └── load_mon_data.asm
│   ├── menus/
│   ├── events/
│   └── gfx/
│
├── data/           → Datos estáticos
│   ├── pokemon/
│   │   ├── base_stats/
│   │   ├── evos_moves.asm
│   │   └── cry_data.asm
│   ├── moves/
│   │   ├── moves.asm
│   │   └── animations.asm
│   ├── maps/
│   └── items/
│
├── gfx/            → Assets gráficos (comprimidos)
│   ├── pokemon/
│   ├── trainers/
│   ├── tilesets/
│   └── sprites/
│
├── audio/          → Música y SFX
│   ├── music/
│   └── sfx/
│
├── maps/           → Definiciones de mapas
│   ├── PalletTown.asm
│   └── ...
│
├── scripts/        → Eventos y diálogos
├── text/           → Strings localizados
├── constants/      → Constantes globales
├── macros/         → Macros de assembly
└── ram/            → Layout de memoria
    ├── wram.asm
    ├── sram.asm
    └── hram.asm
```

---

## 3. Patrones de Arquitectura

### 3.1 Bank Switching (Memory Paging)

**Problema:** ROM de 2MB pero CPU solo ve 64KB

**Solución:** MBC1 (Memory Bank Controller)

```
┌─────────────────────────────────────────────────────────┐
│                    MAPA DE MEMORIA                       │
├─────────────────────────────────────────────────────────┤
│ 0x0000 - 0x3FFF │ Bank 0 (FIJO)     │ home/, core      │
│ 0x4000 - 0x7FFF │ Bank 1-127 (SWAP) │ engine/, data/   │
│ 0x8000 - 0x9FFF │ VRAM              │ Tiles, Sprites   │
│ 0xA000 - 0xBFFF │ SRAM (cartridge)  │ Save data        │
│ 0xC000 - 0xDFFF │ WRAM              │ Estado del juego │
│ 0xFF00 - 0xFFFF │ HRAM              │ Variables rápidas│
└─────────────────────────────────────────────────────────┘
```

**Fórmula de conversión:**
```
ROM_address = (internal_address - 0x4000) + bank * 0x4000
```

**Aplicación NewCool:**
```rust
// newcool-search-v3: Índices como "banks"
pub struct IndexBank {
    id: u8,
    loaded: bool,
    data: Option<IndexData>,
}

impl SearchEngine {
    fn switch_index(&mut self, bank_id: u8) {
        if self.current_bank != bank_id {
            self.unload_current();
            self.load_bank(bank_id);
        }
    }
}
```

---

### 3.2 Data-Driven Design

**Problema:** 151 Pokémon con stats, moves, sprites, cries únicos

**Solución:** Tablas de datos + punteros

```asm
; data/pokemon/base_stats/pikachu.asm
PikachuBaseStats:
    db PIKACHU        ; species
    db 35             ; hp
    db 55             ; attack
    db 30             ; defense
    db 90             ; speed
    db 50             ; special
    db ELECTRIC       ; type1
    db ELECTRIC       ; type2
    db 190            ; catch rate
    db 82             ; base exp
    ; ...
```

**Patrón:** Estructura fija + lookup por índice

```rust
// Aplicación NewCool: Definición de módulos
#[derive(Debug, Clone)]
pub struct ModuleDefinition {
    pub id: &'static str,
    pub name: &'static str,
    pub department: Department,
    pub category: Category,
    pub status: Status,
    pub pricing: PricingModel,
    // Estructura fija, datos variables
}

// Lookup O(1) por índice
static MODULES: &[ModuleDefinition] = &[
    ModuleDefinition { id: "mind-os", ... },
    ModuleDefinition { id: "search-v3", ... },
    // 143 módulos
];
```

---

### 3.3 Sprite Compression (RLE + Delta)

**Problema:** Sprites de 56x56 px × 151 Pokémon = demasiado grande

**Solución:** Compresión en dos fases

```
┌─────────────────────────────────────────────────────────┐
│              PIPELINE DE COMPRESIÓN                      │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Sprite Original (56x56)                                 │
│         │                                                │
│         ▼                                                │
│  ┌─────────────────┐                                    │
│  │ Delta Encoding  │  → Diferencias entre scanlines     │
│  └────────┬────────┘                                    │
│           │                                              │
│           ▼                                              │
│  ┌─────────────────┐                                    │
│  │ RLE (Run-Length)│  → Secuencias de ceros             │
│  └────────┬────────┘                                    │
│           │                                              │
│           ▼                                              │
│  Datos Comprimidos (~40% tamaño original)               │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Aplicación NewCool:**
```rust
// newcool-search-v3: Compresión de índices
pub fn compress_index(data: &[u8]) -> Vec<u8> {
    let delta = delta_encode(data);      // Fase 1
    let compressed = rle_encode(&delta); // Fase 2
    compressed
}

pub fn decompress_index(data: &[u8]) -> Vec<u8> {
    let delta = rle_decode(data);        // Fase 2 inversa
    let original = delta_decode(&delta); // Fase 1 inversa
    original
}
```

---

### 3.4 State Machine para Batallas

**Problema:** Sistema de combate complejo con múltiples estados

**Solución:** Máquina de estados jerárquica

```
┌─────────────────────────────────────────────────────────┐
│              BATTLE STATE MACHINE                        │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  ┌──────────┐                                           │
│  │  INIT    │ → Cargar datos, animación entrada         │
│  └────┬─────┘                                           │
│       │                                                  │
│       ▼                                                  │
│  ┌──────────┐     ┌──────────┐                         │
│  │ PLAYER   │────▶│ EXECUTE  │                         │
│  │ CHOICE   │     │  TURN    │                         │
│  └──────────┘     └────┬─────┘                         │
│       ▲                │                                │
│       │    ┌───────────┼───────────┐                   │
│       │    ▼           ▼           ▼                   │
│       │ ┌──────┐  ┌──────────┐  ┌──────┐              │
│       │ │DAMAGE│  │ SWITCH   │  │ ITEM │              │
│       │ │CALC  │  │ POKEMON  │  │ USE  │              │
│       │ └──┬───┘  └────┬─────┘  └──┬───┘              │
│       │    │           │           │                   │
│       │    └───────────┴───────────┘                   │
│       │                │                                │
│       │                ▼                                │
│       │         ┌──────────┐                           │
│       │         │ CHECK    │                           │
│       └─────────│ VICTORY  │                           │
│                 └────┬─────┘                           │
│                      │                                  │
│           ┌──────────┼──────────┐                      │
│           ▼          ▼          ▼                      │
│      ┌────────┐ ┌────────┐ ┌────────┐                 │
│      │PLAYER  │ │OPPONENT│ │CONTINUE│                 │
│      │  WIN   │ │  WIN   │ │ BATTLE │                 │
│      └────────┘ └────────┘ └────────┘                 │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Aplicación NewCool (Quiz System):**
```rust
#[derive(Debug, Clone)]
pub enum QuizState {
    Init,
    ShowQuestion { question_id: usize },
    WaitingAnswer,
    ProcessAnswer { answer: u8 },
    ShowFeedback { correct: bool },
    NextQuestion,
    Complete { score: f64 },
}

impl QuizSession {
    pub fn transition(&mut self, event: QuizEvent) -> Option<QuizAction> {
        match (&self.state, event) {
            (QuizState::Init, QuizEvent::Start) => {
                self.state = QuizState::ShowQuestion { question_id: 0 };
                Some(QuizAction::DisplayQuestion(0))
            }
            (QuizState::WaitingAnswer, QuizEvent::Answer(a)) => {
                self.state = QuizState::ProcessAnswer { answer: a };
                Some(QuizAction::CheckAnswer(a))
            }
            // ... más transiciones
            _ => None,
        }
    }
}
```

---

### 3.5 V-Blank Scheduling

**Problema:** Solo se puede actualizar pantalla durante V-Blank (~1.1ms)

**Solución:** Cola de trabajo priorizada

```
┌─────────────────────────────────────────────────────────┐
│              V-BLANK WORK QUEUE                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Frame N                    V-Blank (~1.1ms)            │
│  ────────────────────────── │ ──────────────            │
│  │ Gameplay Logic         │ │ 1. OAM DMA (160B)       │
│  │ Input Processing       │ │ 2. Upload tiles (max 8) │
│  │ AI Calculation         │ │ 3. Scroll registers     │
│  │ Prepare next tiles     │ │ 4. Palette update       │
│  │                        │ │ 5. Audio tick           │
│  ────────────────────────── │ ──────────────            │
│                                                          │
│  Si excede V-Blank → Flicker/Corrupción                 │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Aplicación NewCool (Event Loop):**
```rust
// Frame budget: 16.67ms para 60fps
pub struct FrameScheduler {
    budget_ms: f64,
    high_priority: VecDeque<Task>,  // Audio, input
    normal_priority: VecDeque<Task>, // Render, logic
    low_priority: VecDeque<Task>,    // Analytics, prefetch
}

impl FrameScheduler {
    pub fn tick(&mut self, delta_ms: f64) {
        let deadline = Instant::now() + Duration::from_millis(self.budget_ms as u64);

        // Siempre procesar alta prioridad
        while let Some(task) = self.high_priority.pop_front() {
            task.execute();
        }

        // Normal hasta deadline
        while Instant::now() < deadline {
            if let Some(task) = self.normal_priority.pop_front() {
                task.execute();
            } else {
                break;
            }
        }

        // Low priority si hay tiempo
        if Instant::now() < deadline {
            if let Some(task) = self.low_priority.pop_front() {
                task.execute();
            }
        }
    }
}
```

---

### 3.6 Metatile System

**Problema:** Mapas grandes con pocos tiles disponibles

**Solución:** Tiles → Bloques → Mapas

```
┌─────────────────────────────────────────────────────────┐
│              JERARQUÍA DE TILES                          │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Nivel 1: Tiles (8x8 px)                                │
│  ┌───┬───┬───┬───┐                                      │
│  │ A │ B │ C │ D │  (384 tiles máximo en VRAM)          │
│  └───┴───┴───┴───┘                                      │
│                                                          │
│  Nivel 2: Blocks/Metatiles (32x32 px = 4x4 tiles)       │
│  ┌───────────┐                                          │
│  │ A │ A │ B │  Bloque "césped"                         │
│  │ A │ C │ B │  = combinación de tiles base             │
│  │ D │ D │ D │                                          │
│  └───────────┘                                          │
│                                                          │
│  Nivel 3: Map (array de block IDs)                      │
│  ┌───────────────────────┐                              │
│  │ 01 │ 02 │ 02 │ 03 │   │  Pallet Town                 │
│  │ 04 │ 05 │ 05 │ 06 │   │  = 20x18 blocks              │
│  │ 04 │ 07 │ 08 │ 06 │   │  = 640x576 px                │
│  └───────────────────────┘                              │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

**Aplicación NewCool (Component System):**
```rust
// Nivel 1: Primitivos
pub enum Primitive {
    Text(String),
    Icon(IconId),
    Color(RGB),
}

// Nivel 2: Componentes (combinación de primitivos)
pub struct Component {
    id: ComponentId,
    primitives: Vec<Primitive>,
    layout: Layout,
}

// Nivel 3: Pantallas (array de componentes)
pub struct Screen {
    id: ScreenId,
    components: Vec<ComponentId>,
    // Referencia por ID, no copia
}

// Resultado: Reutilización masiva
// 10 primitivos → 50 componentes → 100 pantallas
```

---

### 3.7 Trainer AI (Lookup Tables)

**Problema:** IA que parezca inteligente pero sea predecible

**Solución:** Tablas de comportamiento + random ponderado

```asm
; engine/battle/trainer_ai.asm
TrainerClassAI:
    ; Tabla de probabilidades por clase de entrenador
    db $00  ; YOUNGSTER: Sin IA especial
    db $01  ; BUG CATCHER: Prefiere tipo
    db $03  ; LASS: Healing priority
    db $07  ; SAILOR: Aggressive
    ; ...

AIScoring:
    ; Cada bit activa un comportamiento
    ; Bit 0: Considera tipos
    ; Bit 1: Considera HP
    ; Bit 2: Considera status
    ; Bit 3: Usa items
```

**Aplicación NewCool (Recommendation Engine):**
```rust
pub struct RecommendationAI {
    // Tabla de comportamientos
    behaviors: HashMap<UserArchetype, BehaviorWeights>,
}

#[derive(Debug)]
pub struct BehaviorWeights {
    pub prefer_visual: f32,      // Bit 0
    pub prefer_audio: f32,       // Bit 1
    pub prefer_interactive: f32, // Bit 2
    pub prefer_challenge: f32,   // Bit 3
}

impl RecommendationAI {
    pub fn score_content(&self, user: &User, content: &Content) -> f32 {
        let weights = self.behaviors.get(&user.archetype).unwrap_or_default();

        let mut score = 0.0;
        score += weights.prefer_visual * content.visual_score;
        score += weights.prefer_audio * content.audio_score;
        score += weights.prefer_interactive * content.interactive_score;
        score += weights.prefer_challenge * content.difficulty;

        // Random ponderado para variedad
        score *= 0.9 + (random::<f32>() * 0.2);
        score
    }
}
```

---

## 4. Patrones de Datos

### 4.1 BCD (Binary-Coded Decimal)

**Uso:** Scores, dinero, stats para display fácil

```rust
// Convertir BCD a decimal
pub fn bcd_to_decimal(bcd: &[u8]) -> u32 {
    let mut result = 0u32;
    for byte in bcd {
        let high = (byte >> 4) & 0x0F;
        let low = byte & 0x0F;
        result = result * 100 + (high as u32 * 10) + low as u32;
    }
    result
}

// Ejemplo: [0x12, 0x34] → 1234
```

### 4.2 Bit Flags

**Uso:** Estado compacto (badges, eventos, pokédex)

```rust
pub struct GameFlags {
    badges: u8,      // 8 badges en 1 byte
    events: [u8; 32], // 256 eventos en 32 bytes
    pokedex: [u8; 19], // 151 pokémon en 19 bytes
}

impl GameFlags {
    pub fn has_badge(&self, n: u8) -> bool {
        self.badges & (1 << n) != 0
    }

    pub fn caught_pokemon(&self, n: u8) -> bool {
        let byte = (n / 8) as usize;
        let bit = n % 8;
        self.pokedex[byte] & (1 << bit) != 0
    }
}
```

### 4.3 Pointer Tables

**Uso:** Acceso O(1) a datos variables

```rust
// Tabla de punteros a datos de Pokémon
static POKEMON_DATA_PTRS: [fn() -> &'static PokemonData; 151] = [
    || &BULBASAUR_DATA,
    || &IVYSAUR_DATA,
    || &VENUSAUR_DATA,
    // ...
];

pub fn get_pokemon(id: u8) -> &'static PokemonData {
    POKEMON_DATA_PTRS[id as usize]()
}
```

---

## 5. Aplicación a NewCool

### Mapeo de Patrones

| Patrón Pokémon | Aplicación NewCool |
|----------------|-------------------|
| Bank Switching | Carga lazy de módulos |
| Data-Driven | Definición de 143 módulos |
| Sprite Compression | Compresión de índices |
| State Machine | Quiz flow, auth flow |
| V-Blank Scheduling | Frame budget en UI |
| Metatiles | Component system |
| Trainer AI | Recommendation engine |
| BCD | Display de scores/stats |
| Bit Flags | Permisos, features |
| Pointer Tables | Module registry |

### Ejemplo Integrado

```rust
// newcool-core: Aplicando todos los patrones

pub struct NewCoolEngine {
    // Bank Switching
    loaded_modules: HashMap<ModuleId, Box<dyn Module>>,
    current_module: Option<ModuleId>,

    // Data-Driven
    module_registry: &'static [ModuleDefinition],

    // State Machine
    state: AppState,

    // Scheduler
    frame_scheduler: FrameScheduler,

    // Flags
    user_permissions: PermissionFlags,
    feature_flags: FeatureFlags,
}

impl NewCoolEngine {
    pub fn switch_module(&mut self, id: ModuleId) -> Result<()> {
        // Bank switching pattern
        if Some(id) != self.current_module {
            if let Some(old) = self.current_module.take() {
                self.loaded_modules.remove(&old);
            }
            let module = self.load_module(id)?;
            self.loaded_modules.insert(id, module);
            self.current_module = Some(id);
        }
        Ok(())
    }

    pub fn tick(&mut self, delta_ms: f64) {
        // V-Blank scheduling pattern
        self.frame_scheduler.tick(delta_ms);

        // State machine pattern
        if let Some(event) = self.poll_events() {
            self.state = self.state.transition(event);
        }
    }
}
```

---

## 6. Lecciones Clave

### Para newcool-gameboy

1. **Memory watches** ya implementados correctamente
2. **Event detection** sigue el patrón de Pokémon
3. Agregar: **Compression** para save states

### Para newcool-search-v3

1. **Bank switching** → Index sharding
2. **Pointer tables** → Document lookup
3. **V-Blank scheduling** → Query timeout budget

### Para newcool-atlas-v4

1. **Metatile system** → Component hierarchy
2. **State machine** → Navigation flow
3. **Trainer AI** → Tab suggestions

### Para newcool-api

1. **Data-driven** → Module definitions
2. **Bit flags** → Permission system
3. **BCD** → Price formatting

---

## Referencias

- [pret/pokered](https://github.com/pret/pokered) - Disassembly oficial
- [RetroReversing](https://www.retroreversing.com/pokemonredblue) - Análisis técnico
- [Data Crystal](https://datacrystal.tcrf.net/wiki/Pokémon_Red_and_Blue/Notes) - RAM maps
- [Glitch City Wiki](https://glitchcity.wiki/wiki/Sprite_compression_(Generation_I)) - Compresión
- [Medium: How Pokémon Red/Blue Were Built](https://medium.com/@fulton_shaun/how-pok%C3%A9mon-red-blue-were-built-bf9a4d42f86b)

---

*Documentado para NewCool - Enero 2026*
*"5 programadores, 2MB de ROM, 151 Pokémon, infinitas lecciones"*
