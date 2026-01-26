# NewCool Architecture - Pokemon Style

> Documentación arquitectónica del ecosistema NewCool aplicando los patrones de ingeniería de Pokémon Red/Blue

---

## Vista General del Ecosistema

```
newcool/
├── home/                    # T01-NUCLEUS (siempre cargado)
│   ├── newcool-auth/        # Sistema de autenticación central
│   ├── newcool-hub/         # Centro de localización
│   ├── newcool-sso/         # Single Sign-On
│   └── newcool-analytics/   # Métricas base
│
├── bank01/                  # T02-ATLAS (Browser & AI)
│   ├── newcool-atlas-v4/    # Motor cognitivo
│   ├── newcool-search-v3/   # Motor de búsqueda Rust
│   ├── chromium/            # Browser rebrandeado
│   └── newcool-neo/         # Monitor de build AI
│
├── bank02/                  # T03-EDUCATION (12 módulos)
│   ├── newcool-math/
│   ├── newcool-english/
│   ├── newcool-science/
│   └── ...
│
├── bank03/                  # T04-STREAMING (Música & Video)
│   ├── newcool-streaming-platform/
│   ├── newcool-music-pipeline/
│   └── newcool-video/
│
├── bank04/                  # T05-INDUSTRIAL (SCADA)
├── bank05/                  # T06-MARKETPLACE (FixMatch)
├── bank06/                  # T07-GOVERNMENT (11 módulos)
├── bank07/                  # T08-PROFESSIONAL (6 módulos)
├── bank08/                  # T09-LIFESTYLE (6 módulos)
├── bank09/                  # T10-CREATIVE (AI Factory)
├── bank10/                  # T11-PREMIUM (Mind OS)
├── bank11/                  # T12-COMMUNITY (8 módulos)
├── bank12/                  # T13-APEX LEARNING
│
├── engine/                  # Lógica compartida
│   ├── state-machine/       # Máquina de estados global
│   ├── event-bus/           # Sistema de eventos T12
│   └── scheduler/           # V-Blank equivalente
│
├── data/                    # Definiciones estáticas
│   ├── modules.json         # Catálogo de 143 módulos
│   ├── pricing.json         # Tabla de precios
│   ├── permissions.json     # Matriz de permisos
│   └── locales/             # Traducciones (5 países)
│
├── gfx/                     # Assets visuales
│   └── universo-newcool-assets/
│
└── audio/                   # Assets de audio
    └── music-pipeline/      # 4,734 canciones
```

---

## Patrón 1: Bank Switching (Carga de Módulos)

### Concepto Pokémon
En Pokémon, la ROM de 2MB se divide en bancos de 16KB. Solo un banco puede estar activo a la vez.

### Aplicación NewCool

```rust
// newcool-hub/src/module_loader.rs

/// Equivalente al Bank Switching de Game Boy
/// Solo los módulos necesarios se cargan en memoria
pub struct ModuleBank {
    /// Módulos siempre cargados (HOME bank)
    pub nucleus: Vec<Module>,
    /// Banco actualmente cargado
    pub active_bank: Option<BankId>,
    /// Cache de módulos recientes (LRU)
    pub cache: LruCache<BankId, Vec<Module>>,
}

impl ModuleBank {
    /// Switch de banco - equivalente a `bankswitch` en asm
    pub async fn switch_bank(&mut self, bank_id: BankId) -> Result<()> {
        // 1. Guardar estado del banco actual (si existe)
        if let Some(current) = &self.active_bank {
            self.save_bank_state(current).await?;
        }

        // 2. Cargar nuevo banco
        let modules = if let Some(cached) = self.cache.get(&bank_id) {
            cached.clone()
        } else {
            self.load_bank_from_storage(bank_id).await?
        };

        // 3. Activar banco
        self.active_bank = Some(bank_id);

        Ok(())
    }
}

/// Mapeo de Teams a Bancos
pub enum BankId {
    Nucleus,      // T01 - Siempre cargado
    Atlas,        // T02
    Education,    // T03
    Streaming,    // T04
    Industrial,   // T05
    Marketplace,  // T06
    Government,   // T07
    Professional, // T08
    Lifestyle,    // T09
    Creative,     // T10
    Premium,      // T11
    Community,    // T12
    ApexLearning, // T13
}
```

### Beneficios
- **Memoria**: Solo 1-2 bancos en RAM a la vez
- **Cold Start**: Carga inicial rápida (solo nucleus)
- **Lazy Loading**: Módulos se cargan bajo demanda

---

## Patrón 2: Data-Driven Design

### Concepto Pokémon
Los 151 Pokémon se definen en tablas de datos, no en código hardcodeado.

### Aplicación NewCool

```json
// data/modules.json - Equivalente a BaseStats en Pokémon
{
  "modules": [
    {
      "id": "newcool-math",
      "bank": "education",
      "tier": "free",
      "stats": {
        "complexity": 3,
        "dependencies": ["newcool-auth", "newcool-quiz"],
        "age_range": [6, 18],
        "subjects": ["arithmetic", "algebra", "geometry"]
      },
      "evolution": {
        "next": "newcool-datascience",
        "condition": "complete_all_levels"
      }
    },
    {
      "id": "newcool-english",
      "bank": "education",
      "tier": "free",
      "stats": {
        "complexity": 2,
        "dependencies": ["newcool-auth"],
        "languages": ["es", "pt", "en"]
      }
    }
  ]
}
```

```rust
// newcool-hub/src/module_registry.rs

#[derive(Debug, Deserialize)]
pub struct ModuleDefinition {
    pub id: String,
    pub bank: BankId,
    pub tier: Tier,
    pub stats: ModuleStats,
    pub evolution: Option<Evolution>,
}

/// Cargar módulos desde JSON (como Pokémon carga BaseStats)
pub fn load_module_registry() -> HashMap<String, ModuleDefinition> {
    let data = include_str!("../data/modules.json");
    serde_json::from_str(data).expect("Invalid module registry")
}

/// Buscar módulo por ID (como GetMonHeader)
pub fn get_module(id: &str) -> Option<&ModuleDefinition> {
    MODULE_REGISTRY.get(id)
}
```

### Catálogo de 143 Módulos por Tipo

| Tipo | Cantidad | Equivalente Pokémon |
|------|----------|---------------------|
| Servicio Público | 37 | Normal type |
| Educación | 21 | Psychic type |
| Profesional | 11 | Fighting type |
| Lifestyle | 13 | Fairy type |
| Industrial | 9 | Steel type |
| API | 18 | Electric type |
| Creative | 14 | Fire type |
| Community | 9 | Water type |
| AI Generation | 5 | Dragon type |
| Premium | 1 | Legendary |
| Marketplace | 2 | Dark type |

---

## Patrón 3: State Machine (Navegación Global)

### Concepto Pokémon
El juego tiene estados claros: Overworld, Battle, Menu, Dialog.

### Aplicación NewCool

```rust
// engine/state-machine/src/lib.rs

/// Estados globales del ecosistema NewCool
/// Equivalente a wGameState en Pokémon
#[derive(Debug, Clone, PartialEq)]
pub enum NewCoolState {
    // === Estados de Autenticación ===
    Anonymous,
    Authenticating,
    Authenticated(UserId),

    // === Estados de Navegación ===
    Hub {
        active_bank: BankId,
        active_module: Option<ModuleId>,
    },

    // === Estados de Contenido ===
    Learning {
        course_id: String,
        lesson_id: String,
        progress: f32,
    },
    Playing {
        game_id: String,
        score: u32,
        quiz_pending: bool,
    },
    Streaming {
        track_id: String,
        position: Duration,
        karaoke_mode: bool,
    },

    // === Estados de Transacción ===
    Checkout {
        items: Vec<CartItem>,
        payment_method: Option<PaymentMethod>,
    },

    // === Estados Especiales ===
    MindOS {
        mode: CognitiveMode,
        session_start: Instant,
    },

    // === Estados de Error ===
    Error(ErrorKind),
    Maintenance,
}

/// Transiciones válidas (como el grafo de estados de Pokémon)
impl NewCoolState {
    pub fn can_transition_to(&self, next: &NewCoolState) -> bool {
        match (self, next) {
            // Anonymous puede ir a Authenticating
            (NewCoolState::Anonymous, NewCoolState::Authenticating) => true,

            // Authenticating puede ir a Authenticated o Error
            (NewCoolState::Authenticating, NewCoolState::Authenticated(_)) => true,
            (NewCoolState::Authenticating, NewCoolState::Error(_)) => true,

            // Authenticated puede ir a cualquier estado de contenido
            (NewCoolState::Authenticated(_), NewCoolState::Hub { .. }) => true,
            (NewCoolState::Authenticated(_), NewCoolState::Learning { .. }) => true,
            (NewCoolState::Authenticated(_), NewCoolState::Playing { .. }) => true,
            (NewCoolState::Authenticated(_), NewCoolState::Streaming { .. }) => true,

            // Desde Hub se puede ir a cualquier módulo
            (NewCoolState::Hub { .. }, NewCoolState::Learning { .. }) => true,
            (NewCoolState::Hub { .. }, NewCoolState::Playing { .. }) => true,
            (NewCoolState::Hub { .. }, NewCoolState::Streaming { .. }) => true,
            (NewCoolState::Hub { .. }, NewCoolState::MindOS { .. }) => true,

            // Cualquier estado puede ir a Error
            (_, NewCoolState::Error(_)) => true,

            _ => false,
        }
    }
}
```

### Diagrama de Estados

```
                    ┌─────────────────┐
                    │    Anonymous    │
                    └────────┬────────┘
                             │ login
                             ▼
                    ┌─────────────────┐
                    │ Authenticating  │
                    └────────┬────────┘
                             │ success
                             ▼
                    ┌─────────────────┐
                    │  Authenticated  │
                    └────────┬────────┘
                             │
            ┌────────────────┼────────────────┐
            │                │                │
            ▼                ▼                ▼
    ┌───────────┐    ┌───────────┐    ┌───────────┐
    │    Hub    │◄──►│  Learning │    │ Streaming │
    └─────┬─────┘    └───────────┘    └───────────┘
          │
          ├──────────────────┐
          │                  │
          ▼                  ▼
    ┌───────────┐    ┌───────────┐
    │  Playing  │    │  MindOS   │
    └───────────┘    └───────────┘
```

---

## Patrón 4: V-Blank Scheduling (Event Loop)

### Concepto Pokémon
Todo el procesamiento ocurre en el período V-Blank (modo 1) del LCD.

### Aplicación NewCool

```rust
// engine/scheduler/src/lib.rs

/// Frame budget equivalente al V-Blank
/// En web: 16.67ms para 60fps
/// En servidor: configurable por prioridad
pub struct FrameScheduler {
    frame_budget: Duration,
    tasks: PriorityQueue<ScheduledTask>,
    metrics: FrameMetrics,
}

/// Prioridades de tareas (como sprites vs background en GB)
#[derive(Debug, PartialEq, Eq, PartialOrd, Ord)]
pub enum TaskPriority {
    Critical,  // Auth, seguridad - siempre ejecutar
    High,      // UI updates, audio sync
    Normal,    // Búsquedas, carga de datos
    Low,       // Analytics, prefetch
    Idle,      // Garbage collection, cache cleanup
}

impl FrameScheduler {
    /// Ejecutar tareas dentro del frame budget
    /// Equivalente al loop V-Blank de Game Boy
    pub async fn run_frame(&mut self) {
        let frame_start = Instant::now();

        while frame_start.elapsed() < self.frame_budget {
            if let Some(task) = self.tasks.pop() {
                let task_start = Instant::now();

                // Ejecutar tarea
                task.execute().await;

                // Métricas
                self.metrics.record_task(
                    task.priority,
                    task_start.elapsed(),
                );

                // Si queda poco tiempo, solo tareas críticas
                let remaining = self.frame_budget - frame_start.elapsed();
                if remaining < Duration::from_millis(2) {
                    self.tasks.retain(|t| t.priority == TaskPriority::Critical);
                }
            } else {
                break;
            }
        }

        // Reportar frame overrun
        if frame_start.elapsed() > self.frame_budget {
            self.metrics.record_overrun(frame_start.elapsed());
        }
    }
}
```

### Budget por Contexto

| Contexto | Frame Budget | Prioridades |
|----------|--------------|-------------|
| Web Frontend | 16.67ms | UI > Audio > Network |
| API Server | 50ms | Auth > Query > Analytics |
| Streaming | 10ms | Audio > Sync > Buffer |
| Game (WASM) | 16.67ms | Input > Logic > Render |

---

## Patrón 5: Metatile System (Component Hierarchy)

### Concepto Pokémon
Los mapas usan metatiles de 32x32 compuestos de 4 tiles de 8x8.

### Aplicación NewCool

```typescript
// engine/ui-system/src/metatiles.ts

/**
 * Sistema de componentes jerárquico estilo Metatile
 * Atoms → Molecules → Organisms → Templates → Pages
 */

// === NIVEL 1: Atoms (8x8 tiles) ===
const atoms = {
  Icon: ({ name }: { name: string }) => <LucideIcon name={name} />,
  Text: ({ children }: { children: string }) => <span>{children}</span>,
  Button: ({ onClick }: { onClick: () => void }) => <button onClick={onClick} />,
};

// === NIVEL 2: Molecules (16x16 - 2x2 atoms) ===
const molecules = {
  IconButton: () => (
    <div className="icon-button">
      <atoms.Icon name="play" />
      <atoms.Text>Play</atoms.Text>
    </div>
  ),
  ScoreDisplay: ({ score }: { score: number }) => (
    <div className="score">
      <atoms.Icon name="star" />
      <atoms.Text>{formatBCD(score)}</atoms.Text>
    </div>
  ),
};

// === NIVEL 3: Organisms (32x32 - Metatile completo) ===
const organisms = {
  PlayerControls: () => (
    <div className="player-controls">
      <molecules.IconButton icon="prev" />
      <molecules.IconButton icon="play" />
      <molecules.IconButton icon="next" />
      <molecules.ScoreDisplay score={currentScore} />
    </div>
  ),
  CourseCard: ({ course }) => (
    <div className="course-card">
      <atoms.Image src={course.thumbnail} />
      <atoms.Text>{course.title}</atoms.Text>
      <molecules.ProgressBar value={course.progress} />
      <molecules.IconButton icon="start" />
    </div>
  ),
};

// === NIVEL 4: Templates (Mapa completo) ===
const templates = {
  LearningTemplate: ({ courses, currentLesson }) => (
    <div className="learning-template">
      <header>
        <organisms.Navigation />
      </header>
      <aside>
        <organisms.CourseList courses={courses} />
      </aside>
      <main>
        <organisms.LessonViewer lesson={currentLesson} />
      </main>
    </div>
  ),
};

// === NIVEL 5: Pages (Route completa) ===
const pages = {
  MathPage: () => {
    const { courses } = useEducationBank('math');
    const { currentLesson } = useLearningState();

    return (
      <templates.LearningTemplate
        courses={courses}
        currentLesson={currentLesson}
      />
    );
  },
};
```

### Mapeo de Componentes

```
┌─────────────────────────────────────────────────┐
│                    Page                          │
│  ┌─────────────────────────────────────────┐    │
│  │              Template                    │    │
│  │  ┌─────────────┐  ┌─────────────────┐   │    │
│  │  │  Organism   │  │    Organism     │   │    │
│  │  │ ┌───┬───┐   │  │  ┌───┬───┬───┐  │   │    │
│  │  │ │Mol│Mol│   │  │  │Mol│Mol│Mol│  │   │    │
│  │  │ │┌─┐│┌─┐│   │  │  │┌─┐│┌─┐│┌─┐│  │   │    │
│  │  │ ││A│││A││   │  │  ││A│││A│││A││  │   │    │
│  │  │ │└─┘│└─┘│   │  │  │└─┘│└─┘│└─┘│  │   │    │
│  │  │ └───┴───┘   │  │  └───┴───┴───┘  │   │    │
│  │  └─────────────┘  └─────────────────┘   │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

---

## Patrón 6: Lookup Tables (Recomendaciones)

### Concepto Pokémon
El Trainer AI usa tablas de lookup para decidir movimientos.

### Aplicación NewCool

```rust
// engine/recommendations/src/lib.rs

/// Sistema de recomendaciones basado en lookup tables
/// Como el AI de entrenadores en Pokémon

/// Tabla de compatibilidad módulo-usuario
pub static COMPATIBILITY_TABLE: &[CompatibilityEntry] = &[
    // (user_tier, age_range, module_id, base_score)
    CompatibilityEntry::new(Tier::Free, 6..12, "newcool-math", 100),
    CompatibilityEntry::new(Tier::Free, 6..12, "newcool-english", 90),
    CompatibilityEntry::new(Tier::Free, 13..18, "newcool-code", 95),
    CompatibilityEntry::new(Tier::Pro, 18..99, "newcool-mind-os", 100),
    // ...143 entradas más
];

/// Tabla de progresión de aprendizaje
pub static LEARNING_PATH_TABLE: &[LearningPath] = &[
    LearningPath {
        start: "newcool-math",
        next: "newcool-datascience",
        condition: Condition::CompleteAllLessons,
    },
    LearningPath {
        start: "newcool-code",
        next: "newcool-ai",
        condition: Condition::ScoreAbove(80),
    },
    // ...
];

/// Función de recomendación (como GetAIMove)
pub fn get_recommendations(user: &User, context: &Context) -> Vec<ModuleId> {
    let mut scores: Vec<(ModuleId, u32)> = Vec::new();

    for entry in COMPATIBILITY_TABLE {
        if entry.tier <= user.tier
           && entry.age_range.contains(&user.age)
        {
            let mut score = entry.base_score;

            // Bonus por historial
            if user.completed.contains(&entry.module_id) {
                score += 20; // Ya le gustó algo similar
            }

            // Bonus por contexto
            if context.time_of_day.is_evening() && entry.module_id.contains("relax") {
                score += 15;
            }

            // Bonus por secuencia de aprendizaje
            for path in LEARNING_PATH_TABLE {
                if user.completed.contains(&path.start)
                   && !user.completed.contains(&path.next)
                   && path.next == entry.module_id
                {
                    score += 50; // Siguiente paso natural
                }
            }

            scores.push((entry.module_id.to_string(), score));
        }
    }

    // Ordenar por score y tomar top 5
    scores.sort_by(|a, b| b.1.cmp(&a.1));
    scores.into_iter().take(5).map(|(id, _)| id).collect()
}
```

### Tablas de Lookup del Sistema

| Tabla | Propósito | Entradas |
|-------|-----------|----------|
| `COMPATIBILITY_TABLE` | Módulo → Usuario | 143 × 5 tiers |
| `LEARNING_PATH_TABLE` | Progresión | ~200 paths |
| `PRICING_TABLE` | Precios regionales | 5 países × 143 |
| `PERMISSION_TABLE` | Acceso a features | 143 × 4 tiers |

---

## Patrón 7: BCD Encoding (Scores & Progress)

### Concepto Pokémon
Los números se almacenan en Binary-Coded Decimal para display fácil.

### Aplicación NewCool

```rust
// engine/scoring/src/bcd.rs

/// Progreso de usuario en formato BCD
/// Permite display instantáneo sin conversión
#[derive(Debug, Clone, Copy)]
pub struct BCDProgress {
    /// 3 dígitos: 0-999 (ej: 087 = 87%)
    pub percent: [u8; 3],
    /// 5 dígitos: 0-99999 (ej: 04734 = 4734 canciones)
    pub total: [u8; 5],
    /// 4 dígitos: 0-9999 (ej: 0182 = 182 tests pasados)
    pub score: [u8; 4],
}

impl BCDProgress {
    /// Incrementar score (como sumar puntos en Tetris)
    pub fn add_score(&mut self, points: u16) {
        let mut carry = points;
        for i in (0..4).rev() {
            let digit = self.score[i] as u16 + (carry % 10);
            self.score[i] = (digit % 10) as u8;
            carry = carry / 10 + digit / 10;
        }
    }

    /// Convertir a string para display instantáneo
    pub fn display(&self) -> String {
        format!(
            "{}{}{}% - Score: {}{}{}{}",
            self.percent[0], self.percent[1], self.percent[2],
            self.score[0], self.score[1], self.score[2], self.score[3]
        )
    }
}

/// Estadísticas del ecosistema en BCD
pub struct EcosystemStats {
    pub repositories: BCDNumber<3>,  // 122 repos
    pub modules: BCDNumber<3>,       // 143 módulos
    pub songs: BCDNumber<5>,         // 4,734 canciones
    pub recipes: BCDNumber<3>,       // 436+ recetas
    pub guides: BCDNumber<3>,        // 677+ guías
    pub tests_atlas: BCDNumber<3>,   // 182 tests
    pub tests_apex: BCDNumber<2>,    // 52 tests
}
```

---

## Patrón 8: Bit Flags (Permisos & Features)

### Concepto Pokémon
Los eventos y badges se almacenan como bits individuales.

### Aplicación NewCool

```rust
// engine/permissions/src/flags.rs

/// Feature flags como bits
/// Cada bit = una feature o permiso
bitflags! {
    pub struct UserPermissions: u64 {
        // === Tier Flags (bits 0-3) ===
        const FREE       = 0b0000_0001;
        const BASIC      = 0b0000_0010;
        const PRO        = 0b0000_0100;
        const ENTERPRISE = 0b0000_1000;

        // === Module Access (bits 4-15) ===
        const EDUCATION  = 0b0000_0000_0001_0000;
        const STREAMING  = 0b0000_0000_0010_0000;
        const GOVERNMENT = 0b0000_0000_0100_0000;
        const CREATIVE   = 0b0000_0000_1000_0000;
        const MINDOS     = 0b0000_0001_0000_0000;
        const INDUSTRIAL = 0b0000_0010_0000_0000;
        const COMMUNITY  = 0b0000_0100_0000_0000;

        // === Special Flags (bits 16-23) ===
        const BETA_TESTER    = 0b0000_0001_0000_0000_0000_0000;
        const CONTENT_CREATOR = 0b0000_0010_0000_0000_0000_0000;
        const MODERATOR      = 0b0000_0100_0000_0000_0000_0000;
        const ADMIN          = 0b0000_1000_0000_0000_0000_0000;

        // === Achievements (bits 24-31) ===
        const COMPLETED_MATH    = 0b0001_0000_0000_0000_0000_0000_0000_0000;
        const COMPLETED_ENGLISH = 0b0010_0000_0000_0000_0000_0000_0000_0000;
        const COMPLETED_CODE    = 0b0100_0000_0000_0000_0000_0000_0000_0000;
        const POWER_USER        = 0b1000_0000_0000_0000_0000_0000_0000_0000;
    }
}

impl UserPermissions {
    /// Check si puede acceder a módulo
    pub fn can_access(&self, module: &str) -> bool {
        match module {
            // Gobierno siempre gratis
            m if m.starts_with("newcool-sernac") => true,
            m if m.starts_with("newcool-fonasa") => true,
            m if m.starts_with("newcool-sii") => true,

            // Educación gratis
            "newcool-math" | "newcool-english" => true,

            // Mind OS requiere PRO
            "newcool-mind-os" => self.contains(Self::PRO | Self::MINDOS),

            // Industrial requiere ENTERPRISE
            "newcool-scada" => self.contains(Self::ENTERPRISE | Self::INDUSTRIAL),

            _ => true, // Default: acceso libre
        }
    }
}
```

### Mapa de Bits

```
┌─────────────────────────────────────────────────────────────────┐
│ Bit   63                    32                     0            │
│       │                      │                     │            │
│       ▼                      ▼                     ▼            │
│ ┌─────────────┬─────────────┬─────────────┬─────────────────┐   │
│ │  Reserved   │ Achievements│   Special   │ Modules │ Tier  │   │
│ │   32 bits   │   8 bits    │   8 bits    │ 12 bits │4 bits │   │
│ └─────────────┴─────────────┴─────────────┴─────────────────┘   │
│                                                                  │
│ Ejemplo: Usuario PRO con Mind OS y Math completado               │
│ 0b...0001_0000_0000_0001_0000_0000_0001_0000_0100                │
│       ^                 ^                 ^    ^                 │
│       │                 │                 │    └── PRO tier      │
│       │                 │                 └─────── MINDOS access │
│       │                 └───────────────────────── Mind OS mode  │
│       └─────────────────────────────────────────── Math complete │
└─────────────────────────────────────────────────────────────────┘
```

---

## Patrón 9: Pointer Tables (Navegación Dinámica)

### Concepto Pokémon
Las tablas de punteros permiten saltar a cualquier dato/código.

### Aplicación NewCool

```rust
// engine/routing/src/pointer_table.rs

/// Tabla de rutas del ecosistema
/// Como las Pointer Tables de Pokémon pero para navegación web
pub static ROUTE_TABLE: &[RouteEntry] = &[
    // === Hub Routes ===
    RouteEntry::new("/", Page::Hub),
    RouteEntry::new("/search", Page::Search),
    RouteEntry::new("/profile", Page::Profile),

    // === Education Routes (bank02) ===
    RouteEntry::new("/learn", Page::LearnHub),
    RouteEntry::new("/learn/math", Page::Module("newcool-math")),
    RouteEntry::new("/learn/english", Page::Module("newcool-english")),
    RouteEntry::new("/learn/science", Page::Module("newcool-science")),
    RouteEntry::new("/learn/:module/:lesson", Page::Lesson),

    // === Streaming Routes (bank03) ===
    RouteEntry::new("/music", Page::MusicHub),
    RouteEntry::new("/music/:track", Page::Track),
    RouteEntry::new("/karaoke/:track", Page::Karaoke),

    // === Government Routes (bank06) ===
    RouteEntry::new("/gov", Page::GovHub),
    RouteEntry::new("/gov/fonasa", Page::Module("newcool-fonasa")),
    RouteEntry::new("/gov/sii", Page::Module("newcool-sii")),
    RouteEntry::new("/gov/:service/:tramite", Page::Tramite),

    // === Premium Routes (bank10) ===
    RouteEntry::new("/mindos", Page::MindOS),
    RouteEntry::new("/mindos/:mode", Page::CognitiveMode),
];

/// Resolver ruta a página (como GetMapPointer)
pub fn resolve_route(path: &str) -> Option<Page> {
    for entry in ROUTE_TABLE {
        if entry.matches(path) {
            return Some(entry.page.clone());
        }
    }
    None
}

/// Obtener todas las rutas de un banco
pub fn get_bank_routes(bank: BankId) -> Vec<&'static RouteEntry> {
    ROUTE_TABLE
        .iter()
        .filter(|r| r.bank() == bank)
        .collect()
}
```

---

## Patrón 10: Event Bus (Sistema de Mensajes)

### Concepto Pokémon
Los eventos del juego se procesan en un loop central.

### Aplicación NewCool

```typescript
// engine/event-bus/src/index.ts

/**
 * EventBus del ecosistema NewCool
 * Implementado con BroadcastChannel API (como T12)
 */
class NewCoolEventBus {
  private channel: BroadcastChannel;
  private handlers: Map<string, Set<EventHandler>>;

  constructor() {
    this.channel = new BroadcastChannel('newcool-ecosystem');
    this.handlers = new Map();

    this.channel.onmessage = (event) => {
      this.dispatch(event.data);
    };
  }

  /**
   * Eventos del ecosistema (como wEventFlags en Pokémon)
   */
  emit(event: NewCoolEvent): void {
    this.channel.postMessage(event);
    this.dispatch(event);
  }

  private dispatch(event: NewCoolEvent): void {
    const handlers = this.handlers.get(event.type);
    if (handlers) {
      handlers.forEach(handler => handler(event));
    }
  }
}

// Tipos de eventos del ecosistema
type NewCoolEvent =
  // Auth events
  | { type: 'user.login'; userId: string }
  | { type: 'user.logout'; userId: string }

  // Learning events
  | { type: 'lesson.started'; lessonId: string; moduleId: string }
  | { type: 'lesson.completed'; lessonId: string; score: number }
  | { type: 'quiz.submitted'; quizId: string; correct: number; total: number }

  // Streaming events
  | { type: 'track.play'; trackId: string }
  | { type: 'track.complete'; trackId: string; duration: number }
  | { type: 'karaoke.started'; trackId: string }

  // Gamification events
  | { type: 'achievement.unlocked'; achievementId: string }
  | { type: 'level.up'; newLevel: number }
  | { type: 'points.earned'; points: number; reason: string }

  // System events
  | { type: 'module.loaded'; moduleId: string; bank: BankId }
  | { type: 'error.occurred'; error: Error; context: string };

// Uso
const bus = new NewCoolEventBus();

bus.on('lesson.completed', (event) => {
  // Actualizar gamificación
  bus.emit({ type: 'points.earned', points: 100, reason: 'lesson_complete' });

  // Verificar logros
  checkAchievements(event.lessonId);
});
```

---

## Integración: NewCool Search V3 Estilo Pokémon

```rust
// newcool-search-v3/src/lib.rs

/// Motor de búsqueda inspirado en la arquitectura de Pokémon
///
/// Bank Switching: Índices se cargan bajo demanda
/// Data-Driven: Configuración en JSON
/// V-Blank: Búsquedas en batches para no bloquear
/// Pointer Tables: Routing de queries

pub struct PokemonStyleSearch {
    /// Banco de índices (como ROM banks)
    index_bank: IndexBank,

    /// Tabla de tipos de búsqueda (como type effectiveness)
    query_types: HashMap<QueryType, QueryHandler>,

    /// Cache LRU (como wram en GB)
    cache: LruCache<QueryHash, SearchResults>,

    /// Scheduler estilo V-Blank
    scheduler: FrameScheduler,
}

impl PokemonStyleSearch {
    /// Búsqueda principal
    pub async fn search(&mut self, query: &str, options: SearchOptions) -> SearchResults {
        // 1. Determinar tipo de query (como calcular type effectiveness)
        let query_type = self.classify_query(query);

        // 2. Cargar banco de índice apropiado
        self.index_bank.switch_to(query_type.required_bank()).await?;

        // 3. Ejecutar en frame budget
        self.scheduler.schedule(TaskPriority::High, async {
            let handler = self.query_types.get(&query_type).unwrap();
            handler.execute(query, &self.index_bank, options).await
        }).await
    }

    /// Clasificar query (como GetMonType)
    fn classify_query(&self, query: &str) -> QueryType {
        // Lookup table de patrones
        for (pattern, query_type) in QUERY_PATTERNS {
            if pattern.is_match(query) {
                return *query_type;
            }
        }
        QueryType::General
    }
}

/// Tipos de búsqueda (como tipos de Pokémon)
pub enum QueryType {
    Music,       // Búsqueda de canciones
    Education,   // Búsqueda de cursos/lecciones
    Government,  // Búsqueda de trámites
    Code,        // Búsqueda de código/docs
    General,     // Búsqueda general
}

/// Tabla de patrones (como type chart)
static QUERY_PATTERNS: &[(Regex, QueryType)] = &[
    (regex!(r"canción|música|artista"), QueryType::Music),
    (regex!(r"curso|lección|aprender"), QueryType::Education),
    (regex!(r"trámite|fonasa|sii|rut"), QueryType::Government),
    (regex!(r"código|función|api|rust"), QueryType::Code),
];
```

---

## Resumen: Patrones Pokémon en NewCool

| Patrón Pokémon | Aplicación NewCool | Beneficio |
|----------------|--------------------| ----------|
| Bank Switching | Module Loading | Memoria eficiente |
| Data-Driven | modules.json | Fácil mantenimiento |
| State Machine | Navegación global | UX predecible |
| V-Blank | Frame Scheduler | No bloquea UI |
| Metatiles | Component System | Reutilización |
| Lookup Tables | Recomendaciones | O(1) decisions |
| BCD | Scores/Progress | Display instantáneo |
| Bit Flags | Permisos | Storage compacto |
| Pointer Tables | Routing | Navegación flexible |
| Event Loop | EventBus T12 | Desacoplamiento |

---

## Próximos Pasos

1. **Implementar ModuleBank** en newcool-hub
2. **Migrar modules.json** a formato data-driven
3. **Integrar FrameScheduler** en APIs críticas
4. **Unificar EventBus** en T12 con tipos estrictos

---

*"Gotta catch 'em all... modules!"*

*NewCool Architecture - Pokemon Style*
*Enero 2026*
