# NEWCOOL MASTER PROMPT

> **Version:** 2.0
> **Fecha:** Enero 2026
> **Ecosistema:** 122 repos GitHub + 141 módulos locales
> **Claude Code:** Referencia completa CLI

---

# PARTE I: ECOSISTEMA NEWCOOL

---

## Arquitectura del Ecosistema

### Departamentos (13 Teams)

| Team | Nombre | Módulos | Estado | Prioridad |
|------|--------|---------|--------|-----------|
| T01 | Infrastructure Core | newcool-auth, newcool-hub, newcool-cloud, newcool-analytics, newcool-sso | Production | P0 |
| T02 | Browser & AI | newcool-atlas-v4, chromium, newcool-search-v3, newcool-neo | Development | P0 |
| T03 | Educational Modules | newcool-math, newcool-english, newcool-science, newcool-history, newcool-geography, newcool-civics, newcool-ai, newcool-datascience, newcool-code, newcool-reading, newcool-teacher, newcool-quiz | Production | P1 |
| T04 | Music & Video | newcool-streaming-platform, newcool-music-pipeline, newcool-video, newcool-podcast | Production | P1 |
| T05 | SCADA & Automation | newcool-scada, newcool-robotics | Production | P2 |
| T06 | FixMatch Platform | fixmatch, fixmatch-launch | Production | P1 |
| T07 | Public Services | newcool-sernac, newcool-sii-data, newcool-impuestos, newcool-fonasa, newcool-previred, newcool-direccion-trabajo, newcool-trabajo, newcool-beneficios, newcool-comisaria-virtual, newcool-registro-civil, newcool-tgr, newcool-caj | Production | P1 |
| T08 | Business Tools | newcool-contabilidad, newcool-rrhh, newcool-finiquito, newcool-emprendimiento, newcool-marketing, newcool-mantencion | Production | P1 |
| T09 | Personal Tools | newcool-cocina, newcool-mascota, newcool-hogar, newcool-horoscopo, newcool-fortuna, newcool-finance | Production | P2 |
| T10 | AI Factory & Assets | newcool-ai-factory-os, universo-newcool-assets, newcool-anime-factory, newcool-motion, newcool-art | Production | P1 |
| T11 | Premium Products | newcool-mind-os | Development | P1 |
| T12 | Community Platform | newcool-community, newcool-cerebro, newcool-feedback, newcool-notifications, newcool-gamification, newcool-moderation, newcool-events, newcool-t12-shared | Production | P1 |
| T13 | Ethical Learning | newcool-apex-learning | Development | P1 |

---

## Stack Tecnológico

### Frontend

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Next.js | 16 | Framework principal |
| React | 19 | UI Library |
| TypeScript | 5.7+ | Lenguaje |
| Tailwind CSS | 4.0 | Styling |
| Zustand | 5.x | State management |
| Framer Motion | 11-12 | Animaciones |
| Radix UI | Latest | Componentes accesibles |
| Lucide React | Latest | Iconos |
| Recharts | 2.x | Gráficos |
| React Hook Form + Zod | Latest | Formularios + validación |

### Backend

| Tecnología | Versión | Uso |
|------------|---------|-----|
| Node.js | 20+ | Runtime |
| Express.js | Latest | Web framework |
| Next.js API Routes | 16 | API endpoints |
| Supabase Auth + JWT | Latest | Autenticación |
| Zod | Latest | Validación |
| Helmet + CORS | Latest | Seguridad |

### Rust Services

| Proyecto | Tecnologías | Tests |
|----------|-------------|-------|
| newcool-search-v3 | Tantivy, Axum, Tokio, Qdrant, Redis | - |
| newcool-apex-learning | DDD, serde, thiserror | 52 tests |
| newcool-hub | NAPI-RS, Tantivy BM25 | - |

### Python Services

| Proyecto | Tecnologías |
|----------|-------------|
| newcool-neo | Python 3.11+, asyncio, aiosqlite, Click, Rich |
| chromium-asset-generator | Pillow, rsvg-convert |

### Mobile (Flutter)

| Tecnología | Versión |
|------------|---------|
| Flutter | 3.x |
| Dart | Latest |
| Provider | 6.x |
| sqflite | Latest |
| audioplayers | 5.x |
| Platforms | iOS, Android, macOS, Web, Windows, Linux |

### AI Providers

| Tipo | Primary | Fallback |
|------|---------|----------|
| Image | Replicate (Flux/SDXL) | FAL.ai, OpenAI DALL-E 3 |
| Video | Runway Gen-3 Alpha | Hailuo MiniMax, Veo 3 |
| Audio/Music | Suno AI | - |
| LLM | Anthropic Claude | OpenAI GPT-4 |

### Databases

| Tipo | Tecnología |
|------|------------|
| Primary | PostgreSQL (Supabase) |
| Cache | Redis |
| Local | SQLite |
| Vector | Qdrant |
| File Storage | JSON, FileStore |

### Infrastructure

| Servicio | Provider |
|----------|----------|
| Hosting | Vercel |
| CDN | AWS S3 (us-east-2) |
| Auth | Supabase |
| Queue | BullMQ + Redis |
| Container | Docker + Docker Compose |

### Browser (Chromium)

| Aspecto | Valor |
|---------|-------|
| Base | Chromium 145.0.7560 |
| Build System | GN + Ninja |
| Languages | 81 |
| Platforms | macOS, Windows, Linux, Android |

### Testing

| Runtime | Framework | Coverage |
|---------|-----------|----------|
| Node.js | node:test (native) | atlas_v4: 182 tests |
| Rust | cargo test | apex_learning: 52 tests |
| Python | pytest + pytest-asyncio | neo: 126 tests |

---

## Integraciones

### Search + Atlas

```javascript
// V3Integration (non-intrusive)
from: "newcool-atlas-v4"
to: "newcool-search-v3"
apis: ["search", "index", "getDocument", "getAnalytics", "getStatus"]
```

### Factory + Atlas

```javascript
// FactoryOSBridge
from: "newcool-atlas-v4"
to: "newcool-ai-factory-os"
features: ["image_generation", "video_generation", "audio_generation"]
```

### T12 EventBus

```javascript
// BroadcastChannel API
channel: "t12-community"
modules: ["cerebro", "feedback", "notifications", "gamification", "moderation", "events"]
```

### Streaming CDN

```javascript
provider: "AWS S3"
bucket: "newcool-streaming-platform-cdn"
region: "us-east-2"
```

---

## Modelo de Negocio

### Clasificación de Módulos (143 total)

| Categoría | Cantidad | Modelo de Ingreso |
|-----------|----------|-------------------|
| Servicio Público | 37 | 100% Gratuito |
| Educación | 21 | Gratuito + Donaciones |
| API/Infrastructure | 18 | Pay-per-use |
| Creatividad | 14 | Gratuito/Freemium |
| Herramienta Personal | 13 | Gratuito + Donaciones |
| Herramienta Profesional | 11 | Freemium |
| Industrial | 9 | Pricing Custom |
| Community | 9 | Gratuito |
| AI Generation | 5 | Pay-per-use |
| Marketplace | 2 | 20% Comisión |
| Sostenibilidad | 2 | Gratuito |
| Premium | 1 | Suscripción |

### Potencial de Ingresos

| Tipo | Cantidad | Porcentaje |
|------|----------|------------|
| Módulos gratuitos | 82 | 57% |
| Módulos freemium | 25 | 17% |
| Módulos pagados | 34 | 24% |
| Custom/Enterprise | 2 | 1% |

---

## Módulos por Categoría

### Servicio Público (37 módulos)

```
newcool-sernac, newcool-sii-data, newcool-impuestos, newcool-fonasa,
newcool-previred, newcool-direccion-trabajo, newcool-trabajo,
newcool-beneficios, newcool-comisaria-virtual, newcool-registro-civil,
newcool-tgr, newcool-caj, newcool-aduanas, newcool-afp, newcool-agricultura,
newcool-agua, newcool-certs, newcool-ciudadania, newcool-civil,
newcool-conservador, newcool-consumidor, newcool-contraloria,
newcool-defensoria, newcool-derechos, newcool-emergencias, newcool-familia,
newcool-forestal, newcool-migracion, newcool-mineria, newcool-ministerio-publico,
newcool-municipalidades, newcool-normas, newcool-notarias, newcool-pesca,
newcool-poder-judicial, newcool-seguridad, newcool-telecomunicaciones
```

### Educación (21 módulos)

```
newcool-math, newcool-english, newcool-science, newcool-history,
newcool-geography, newcool-civics, newcool-reading, newcool-writing,
newcool-teacher, newcool-quiz, newcool-ai, newcool-datascience,
newcool-code, newcool-desaprender, newcool-ciencia, newcool-cultura,
newcooltura, newcool-informada, newcool-leadership, newcool-translate,
newcool-music-edu
```

### Herramienta Personal (13 módulos)

```
newcool-cocina, newcool-mascota, newcool-mascotas, newcool-hogar,
newcool-horoscopo, newcool-fortuna, newcool-finance, newcool-personalidad,
newcool-salud, newcool-deporte, newcool-turismo, newcool-vivienda,
newcool-transporte
```

### Herramienta Profesional (11 módulos)

```
newcool-contabilidad, newcool-rrhh, newcool-finiquito, newcool-emprendimiento,
newcool-marketing, newcool-mantencion, newcool-startup, newcool-ecommerce,
newcool-nocode, newcool-product, newcool-impact
```

### Industrial (9 módulos)

```
newcool-scada, newcool-robotics, newcool-robots, newcool-biotech,
newcool-energia, newcool-energy, newcool-cybersecurity, newcool-devops,
newcool-blockchain
```

### API/Infrastructure (18 módulos)

```
newcool-auth, newcool-hub, newcool-cloud, newcool-analytics, newcool-sso,
newcool-search-v3, newcool-indexing, newcool-notifications, newcool-offline,
newcool-ethical-framework, newcool-content-uploader, newcool-assets-pipeline,
newcool-atlas-v4, newcool-atlas-patches, chromium, chromium-asset-generator,
newcool-neo, newcool-cerebro
```

### Creatividad (14 módulos)

```
newcool-art, newcool-motion, newcool-video, newcool-photography,
newcool-podcast, newcool-music-pipeline, newcool-music-engine-clean,
newcool-drama, newcool-terror, newcool-farandula, newcool-memes,
newcool-streaming, newcool-streaming-platform, newcool-3dvr
```

### AI Generation (5 módulos)

```
newcool-ai-factory-os, newcool-anime-factory, universo-newcool-assets,
newcool-astral, newcool-gamedev
```

### Community (9 módulos)

```
newcool-community, newcool-feedback, newcool-gamification, newcool-moderation,
newcool-events, newcool-t12-shared, newcool-medioambiente, newcool-planet,
newcool-circular
```

### Premium (1 módulo)

```
newcool-mind-os
```

### Marketplace (2 módulos)

```
fixmatch, fixmatch-launch
```

---

## Sistemas Principales

### Atlas V4 (Browser AI)

- **21 módulos** con 182 tests
- Chromium 145.0.7560 rebrand
- Integración con Search V3 y AI Factory
- Soporte para 81 idiomas

### Search V3 (Rust)

- Motor híbrido BM25 + semantic search
- Tantivy como base
- Qdrant para vectores
- Redis para cache

### Mind OS (Premium)

- Flutter multi-plataforma
- 10 modos cognitivos (3 gratis, 7 Pro)
- Suscripción mensual/anual

### AI Factory OS

- Generación de imágenes (Flux/SDXL)
- Generación de video (Runway)
- Generación de audio (Suno)

### APEX Learning (Rust)

- Motor de aprendizaje ético
- Domain-Driven Design
- 52 tests

### T12 Community

- EventBus via BroadcastChannel API
- 8 módulos integrados
- Gamificación y moderación

### Universo NewCool Assets

- 1,040 archivos
- 7 personajes principales
- 7 mundos
- LoRA training configs

---

## Expansión Geográfica

### Chile (Base)

- Dominio: newcool.cl
- Moneda: CLP
- Regulaciones: Ley de Protección de Datos
- Gateway: Transbank, MercadoPago

### España

- Dominio: newcool.es
- Moneda: EUR
- Regulaciones: RGPD
- Gateway: Stripe, Redsys

### México

- Dominio: newcool.mx
- Moneda: MXN
- Regulaciones: LFPDPPP
- Gateway: Conekta, MercadoPago

### Estados Unidos

- Dominio: newcool.io
- Moneda: USD
- Regulaciones: CCPA/CPRA, FTC, COPPA
- Gateway: Stripe, PayPal

### Brasil

- Dominio: newcool.com.br
- Moneda: BRL
- Regulaciones: LGPD
- Gateway: MercadoPago, PagSeguro, PIX

---

## Precios Sugeridos

### Chile (CLP)

| Producto | Mensual | Anual |
|----------|---------|-------|
| Donación sugerida | $1.000 - $5.000 | $10.000 - $50.000 |
| Profesional Básico | $9.990 | $99.900 |
| Profesional Pro | $19.990 - $29.990 | $199.900 |
| Mind OS Premium | $9.990/mes | $79.900/año |

### USA (USD)

| Producto | Mensual | Anual |
|----------|---------|-------|
| Suggested Donation | $1 - $5 | $10 - $50 |
| Professional Basic | $9.99 | $99 |
| Professional Pro | $19.99 - $29.99 | $199 |
| Mind OS Premium | $9.99/month | $79/year |

### Brasil (BRL)

| Producto | Mensual | Anual |
|----------|---------|-------|
| Doação sugerida | R$ 5 - R$ 25 | R$ 50 - R$ 250 |
| Profissional Básico | R$ 49,90 | R$ 499 |
| Profissional Pro | R$ 99,90 - R$ 149,90 | R$ 999 |
| Mind OS Premium | R$ 49,90/mês | R$ 399/ano |

---

## MCP Servers Disponibles

### newcool-assets

```json
{
  "mcpServers": {
    "newcool-assets": {
      "command": "node",
      "args": ["/path/to/newcool-assets-mcp/index.js"],
      "env": {
        "NEWCOOL_ASSETS_PATH": "/Users/newcool/universo-newcool-assets"
      }
    }
  }
}
```

**Tools disponibles:**
- `list_asset_categories` - Listar categorías
- `get_assets` - Obtener assets por categoría
- `get_asset_url` - URL de asset específico
- `generate_asset` - Generar asset con AI
- `generate_batch` - Generar múltiples assets
- `search_free_images` - Buscar en Unsplash
- `list_ecosystem_modules` - Listar módulos NewCool
- `get_module_assets` - Assets por módulo
- `list_visual_styles` - Estilos disponibles
- `list_asset_templates` - Templates disponibles

---

## Comandos Frecuentes

### Desarrollo

```bash
npm install          # Instalar dependencias
npm run dev          # Desarrollo local
npm run build        # Build
npm test             # Tests
npm run lint         # Lint
```

### Git

```bash
git checkout -b feature/nombre              # Feature branch
git commit -m "feat(modulo): descripcion"   # Commit
git push origin feature/nombre              # Push
```

### Rust

```bash
cargo build --release   # Build
cargo test              # Tests
cargo clippy            # Lint
cargo fmt               # Format
```

### Flutter

```bash
flutter run             # Run
flutter build apk       # Build Android
flutter build ios       # Build iOS
flutter test            # Tests
```

### Docker & Deploy

```bash
docker build -t newcool-app .   # Build
docker-compose up -d            # Run
vercel --prod                   # Deploy Vercel
```

---

# PARTE II: CLAUDE CODE CLI

---

## Moods (Thinking Verbs)

Lista completa de los estados/moods que Claude Code muestra mientras trabaja.

> **Fuente oficial:** Pallav Agarwal (Anthropic) + código fuente Claude Code
> **Total:** 90 verbos documentados

### Lista Oficial (56 verbos)

| # | Mood | Traducción | Categoría |
|---|------|------------|-----------|
| 1 | Accomplishing... | Logrando | Acción |
| 2 | Actioning... | Accionando | Acción |
| 3 | Actualizing... | Actualizando | Acción |
| 4 | Baking... | Horneando | Cocina |
| 5 | Brewing... | Preparando | Cocina |
| 6 | Calculating... | Calculando | Técnico |
| 7 | Cerebrating... | Cerebrando | Pensamiento |
| 8 | Churning... | Batiendo | Cocina |
| 9 | **Clauding...** | Claudeando | Especial |
| 10 | Coalescing... | Fusionando | Abstracto |
| 11 | Cogitating... | Cogitando | Pensamiento |
| 12 | Computing... | Computando | Técnico |
| 13 | Conjuring... | Conjurando | Abstracto |
| 14 | Considering... | Considerando | Pensamiento |
| 15 | Cooking... | Cocinando | Cocina |
| 16 | Crafting... | Elaborando | Creación |
| 17 | Creating... | Creando | Creación |
| 18 | Crunching... | Procesando | Técnico |
| 19 | Deliberating... | Deliberando | Pensamiento |
| 20 | Determining... | Determinando | Pensamiento |
| 21 | Doing... | Haciendo | Acción |
| 22 | Effecting... | Efectuando | Acción |
| 23 | Finagling... | Maniobrado | Informal |
| 24 | Forging... | Forjando | Creación |
| 25 | Forming... | Formando | Creación |
| 26 | Generating... | Generando | Creación |
| 27 | Hatching... | Incubando | Gestación |
| 28 | Herding... | Pastoreando | Informal |
| 29 | **Honking...** | Graznando | Informal |
| 30 | Hustling... | Apurando | Informal |
| 31 | Ideating... | Ideando | Pensamiento |
| 32 | Inferring... | Infiriendo | Pensamiento |
| 33 | Manifesting... | Manifestando | Abstracto |
| 34 | Marinating... | Marinando | Cocina |
| 35 | Moseying... | Paseando | Informal |
| 36 | Mulling... | Reflexionando | Pensamiento |
| 37 | Mustering... | Reuniendo | Acción |
| 38 | Musing... | Meditando | Pensamiento |
| 39 | Noodling... | Garabateando | Informal |
| 40 | Percolating... | Filtrando | Cocina |
| 41 | Pondering... | Ponderando | Pensamiento |
| 42 | Processing... | Procesando | Técnico |
| 43 | Puttering... | Trasteando | Informal |
| 44 | **Reticulating...** | Reticulando | Abstracto |
| 45 | Ruminating... | Rumiando | Pensamiento |
| 46 | Schlepping... | Arrastrando | Informal |
| 47 | Shucking... | Desgranando | Informal |
| 48 | Simmering... | Hirviendo | Cocina |
| 49 | Smooshing... | Aplastando | Informal |
| 50 | Spinning... | Girando | Abstracto |
| 51 | Stewing... | Guisando | Cocina |
| 52 | Synthesizing... | Sintetizando | Técnico |
| 53 | Thinking... | Pensando | Pensamiento |
| 54 | Transmuting... | Transmutando | Abstracto |
| 55 | **Vibing...** | Vibrando | Especial |
| 56 | Working... | Trabajando | Acción |

### Lista Extendida (34 verbos adicionales - comunidad)

| # | Mood | Traducción | Categoría |
|---|------|------------|-----------|
| 57 | Booping... | Picoteando | Informal |
| 58 | Channelling... | Canalizando | Abstracto |
| 59 | Combobulating... | Combobulando | Informal |
| 60 | Concocting... | Maquinando | Cocina |
| 61 | Contemplating... | Contemplando | Pensamiento |
| 62 | Deciphering... | Descifrando | Técnico |
| 63 | Discombobulating... | Descombobulando | Informal |
| 64 | Divining... | Adivinando | Abstracto |
| 65 | Elucidating... | Elucidando | Pensamiento |
| 66 | Enchanting... | Encantando | Abstracto |
| 67 | Envisioning... | Visionando | Pensamiento |
| 68 | Flibbertigibbeting... | Charloteando | Informal |
| 69 | Frolicking... | Retozando | Informal |
| 70 | Germinating... | Germinando | Gestación |
| 71 | Imagining... | Imaginando | Pensamiento |
| 72 | Incubating... | Incubando | Gestación |
| 73 | Jiving... | Jiveando | Informal |
| 74 | Meandering... | Serpenteando | Exploración |
| 75 | Perusing... | Hojeando | Exploración |
| 76 | Philosophising... | Filosofando | Pensamiento |
| 77 | Pontificating... | Pontificando | Pensamiento |
| 78 | Puzzling... | Acertijando | Pensamiento |
| 79 | Scheming... | Tramando | Pensamiento |
| 80 | Shimmying... | Meneando | Informal |
| 81 | Spelunking... | Espeleando | Exploración |
| 82 | Sussing... | Detectando | Exploración |
| 83 | Tinkering... | Cacharreando | Creación |
| 84 | Unfurling... | Desplegando | Abstracto |
| 85 | Unravelling... | Desenredando | Técnico |
| 86 | Wandering... | Vagando | Exploración |
| 87 | Whirring... | Zumbando | Técnico |
| 88 | **Wibbling...** | Wibbleando | Especial |
| 89 | Wizarding... | Maguificando | Abstracto |
| 90 | Wrangling... | Domando | Acción |

### Categorías de Moods

| Categoría | Icono | Cantidad |
|-----------|-------|----------|
| Pensamiento | 🧠 | 19 |
| Informal | 🎪 | 15 |
| Abstracto | ✨ | 12 |
| Cocina | 🍳 | 9 |
| Acción | 🎯 | 8 |
| Técnico | ⚙️ | 8 |
| Creación | 🔨 | 6 |
| Exploración | 🧭 | 5 |
| Gestación | 🥚 | 3 |
| Especial | ⭐ | 4 |

---

## Extended Thinking

| Trigger | Budget (tokens) | Nivel | Uso |
|---------|-----------------|-------|-----|
| `think` | ~4,000 | Bajo | Preguntas simples |
| `think hard` | ~10,000 | Medio | Problemas moderados |
| `megathink` | ~10,000 | Medio | Alias de think hard |
| `think harder` | ~31,999 | Máximo | Problemas complejos |
| `ultrathink` | ~31,999 | Máximo | Alias de think harder |

```bash
# Ejemplos
> think about how to optimize this function
> think hard about the architecture for this feature
> ultrathink about the edge cases in this algorithm
```

---

## Atajos de Teclado

### Navegación

| Tecla | Función |
|-------|---------|
| `Tab` | Toggle thinking mode on/off |
| `Ctrl+O` | Ver verbose/thinking expandido |
| `Ctrl+T` | Mostrar todos los procesos |
| `Ctrl+C` | Cancelar operación actual |
| `Esc` | Interrumpir |
| `Esc` x2 | Volver en historial |
| `Up/Down` | Navegar historial de comandos |

### Edición

| Tecla | Función |
|-------|---------|
| `Ctrl+A` | Ir al inicio de línea |
| `Ctrl+E` | Ir al final de línea |
| `Ctrl+W` | Borrar palabra anterior |
| `Ctrl+U` | Borrar línea completa |
| `Ctrl+K` | Borrar hasta el final |

### Modelo

| Atajo | Función |
|-------|---------|
| `Option+P` (macOS) | Cambiar modelo sin limpiar prompt |
| `Alt+P` (Win/Linux) | Cambiar modelo sin limpiar prompt |

---

## Agents (Subagents)

Agents son asistentes AI especializados a los que Claude puede delegar tareas.

### Agents Built-in

| Agente | Modelo | Tools | Uso |
|--------|--------|-------|-----|
| **General-Purpose** | Sonnet | Todos | Tareas complejas multi-step |
| **Plan** | Sonnet | Read, Glob, Grep, Bash (read-only) | Research en plan mode |
| **Explore** | Haiku | Glob, Grep, Read, Bash (read-only) | Buscar/entender codebase |

### Ubicación de Agents

| Scope | Path | Prioridad |
|-------|------|-----------|
| Project | `.claude/agents/name.md` | Alta |
| CLI | `--agents '{...}'` | Media |
| User | `~/.claude/agents/name.md` | Baja |
| Plugin | `agents/` en plugin | Baja |

### AGENT.md Format

```markdown
---
name: agent-name
description: When this agent should be invoked
tools: Read, Grep, Glob, Bash
model: sonnet
permissionMode: default
skills: skill1, skill2
---

Your agent's system prompt goes here.
```

### Permission Modes

| Mode | Comportamiento |
|------|----------------|
| `default` | Verificaciones normales |
| `acceptEdits` | Auto-aprobar ediciones |
| `bypassPermissions` | Auto-aprobar TODO |
| `plan` | Read-only, presenta plan |

### Agent via CLI

```bash
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use after code changes.",
    "prompt": "You are a senior code reviewer...",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  }
}'
```

---

## Skills (Agent Skills)

Skills son archivos markdown que enseñan a Claude cómo hacer algo específico.

### Ubicación de Skills

| Scope | Path | Aplica a |
|-------|------|----------|
| Enterprise | managed settings | Toda la organización |
| Personal | `~/.claude/skills/` | Tú, todos los proyectos |
| Project | `.claude/skills/` | Cualquiera en el repo |
| Plugin | `skills/` en plugin | Quien instale el plugin |

### SKILL.md Format

```yaml
---
name: skill-name
description: What this Skill does and when to use it
allowed-tools: Read, Grep, Glob
model: claude-sonnet-4-20250514
---

# Skill Name

## Instructions
Step-by-step guidance for Claude.

## Examples
Concrete examples of using this Skill.
```

### Skills vs Slash Commands

| Aspecto | Slash Commands | Skills |
|---------|----------------|--------|
| **Complejidad** | Simple prompts | Capacidades complejas |
| **Estructura** | Un archivo .md | Directorio con recursos |
| **Discovery** | Explícito (`/command`) | Automático (contexto) |
| **Invocación** | Tú escribes `/command` | Claude decide cuándo |

---

## Plugins

Plugins extienden Claude Code con funcionalidad custom.

### Componentes de Plugin

| Componente | Descripción |
|------------|-------------|
| **Slash commands** | Archivos `.md` en `commands/` |
| **Agents** | Subagentes para tareas específicas |
| **Skills** | Capacidades invocadas por el modelo |
| **Hooks** | Event handlers para automatización |
| **MCP servers** | Integraciones de herramientas externas |
| **LSP servers** | Code intelligence y soporte de lenguajes |

### Comandos de Plugin

```bash
/plugin                              # UI interactiva
claude plugin install name@market    # Instalar
claude plugin uninstall name         # Desinstalar
claude plugin enable/disable name    # Toggle
claude plugin update name            # Actualizar

# Marketplaces
/plugin marketplace add owner/repo
claude plugin marketplace list

# Testing local
claude --plugin-dir ./my-plugin
```

### Estructura de Plugin

```
my-plugin/
├── .claude-plugin/
│   └── plugin.json          # Manifest (requerido)
├── commands/                 # Slash commands
├── agents/                   # Subagentes
├── skills/                   # Agent Skills
├── hooks/                    # Event handlers
├── .mcp.json                 # MCP servers
└── .lsp.json                 # LSP servers
```

---

## Settings (Configuración)

### Ubicación de Archivos

| Scope | Ubicación | Compartido |
|-------|-----------|------------|
| Enterprise | `/etc/claude-code/managed-settings.json` | IT-deployed |
| User | `~/.claude/settings.json` | No |
| Project | `.claude/settings.json` | Sí (git) |
| Local | `.claude/settings.local.json` | No (gitignored) |

### Precedencia (Mayor a Menor)

1. Enterprise (managed-settings.json)
2. Command-line arguments
3. Local project (.claude/settings.local.json)
4. Shared project (.claude/settings.json)
5. User (~/.claude/settings.json)

### Settings Principales

```json
{
  "model": "sonnet",
  "permissions": {
    "allow": ["Bash(npm run:*)", "Read", "Edit(/src/**)"],
    "deny": ["Read(.env)", "Bash(curl:*)"],
    "defaultMode": "acceptEdits"
  },
  "sandbox": {
    "enabled": true,
    "autoAllowBashIfSandboxed": true
  }
}
```

### Permission Modes

| Mode | Descripción |
|------|-------------|
| `default` | Pedir confirmación para cada tool |
| `acceptEdits` | Auto-aceptar ediciones de archivos |
| `plan` | Solo lectura, sin comandos ni ediciones |
| `bypassPermissions` | Saltar todos los prompts |

---

## CLAUDE.md (Memory Files)

CLAUDE.md es el sistema de memoria persistente de Claude Code.

### Jerarquía de Archivos (5 niveles)

| Prioridad | Tipo | Ubicación | Compartido |
|-----------|------|-----------|------------|
| 1 (Mayor) | Enterprise | `/etc/claude-code/CLAUDE.md` | Organización |
| 2 | Proyecto | `./CLAUDE.md` o `./.claude/CLAUDE.md` | Equipo (git) |
| 3 | Reglas proyecto | `./.claude/rules/*.md` | Equipo (git) |
| 4 | Usuario | `~/.claude/CLAUDE.md` | Solo tú |
| 5 (Menor) | Local proyecto | `./CLAUDE.local.md` | Solo tú (gitignored) |

### Sintaxis de Imports

```markdown
# Importar archivos con @
See @README.md for project overview
Follow @docs/API_DESIGN.md for standards
Check @~/.claude/personal.md for my preferences
```

### Comandos

```bash
/init     # Inicializar CLAUDE.md nuevo
/memory   # Editar archivos de memoria
```

---

## MCP (Model Context Protocol)

MCP es un estándar open-source para integraciones AI-herramientas.

### Componentes

| Componente | Descripción |
|------------|-------------|
| **MCP Servers** | Servicios que exponen tools, resources, prompts |
| **Tools** | Funciones que Claude puede llamar |
| **Resources** | Datos referenciables con @ mentions |
| **Prompts** | Templates que se convierten en slash commands |
| **Transport** | `http`, `sse`, o `stdio` |

### Comandos CLI

```bash
# Agregar servidores
claude mcp add --transport http github https://api.githubcopilot.com/mcp/
claude mcp add --transport stdio db -- npx -y @bytebase/dbhub --dsn "postgresql://..."

# Gestión
claude mcp list
claude mcp get <name>
claude mcp remove <name>

# Importar desde Claude Desktop
claude mcp add-from-claude-desktop
```

### Formato .mcp.json

```json
{
  "mcpServers": {
    "server-name": {
      "type": "http",
      "url": "https://api.example.com/mcp",
      "headers": {
        "Authorization": "Bearer ${API_KEY}"
      }
    }
  }
}
```

---

## Hooks System

Los hooks son comandos shell que se ejecutan en respuesta a eventos.

### Tipos de Eventos (10 tipos)

| Evento | Descripción | Puede bloquear |
|--------|-------------|----------------|
| `PreToolUse` | Antes de ejecutar tool | Sí |
| `PostToolUse` | Después de completar tool | No |
| `PermissionRequest` | Al mostrar diálogo de permiso | Sí |
| `Notification` | Cuando envía notificaciones | No |
| `UserPromptSubmit` | Al enviar prompt | Sí |
| `Stop` | Cuando el agente principal termina | Sí |
| `SubagentStop` | Cuando un subagente termina | Sí |
| `PreCompact` | Antes de compactar contexto | No |
| `SessionStart` | Al iniciar o reanudar sesión | No |
| `SessionEnd` | Al terminar sesión | No |

### Estructura Básica

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.command' >> ~/.claude/bash-log.txt"
          }
        ]
      }
    ]
  }
}
```

### Exit Codes

| Code | Efecto |
|------|--------|
| `0` | Éxito, stdout parseado como JSON |
| `2` | Error bloqueante, stderr mostrado |
| Otro | Error no-bloqueante, continúa |

---

## CLI Flags

### Comandos Base

| Comando | Descripción |
|---------|-------------|
| `claude` | Iniciar REPL interactivo |
| `claude "query"` | REPL con prompt inicial |
| `claude -p "query"` | Query via SDK, luego salir |
| `cat file \| claude -p` | Procesar contenido piped |
| `claude -c` | Continuar conversación reciente |
| `claude -r "session"` | Reanudar sesión por ID/nombre |
| `claude update` | Actualizar a última versión |

### Flags Principales

| Flag | Corto | Descripción |
|------|-------|-------------|
| `--print` | `-p` | Imprimir respuesta sin modo interactivo |
| `--continue` | `-c` | Cargar conversación más reciente |
| `--resume` | `-r` | Reanudar sesión específica |
| `--model` | | Modelo con alias o nombre completo |
| `--output-format` | | Formato: `text`, `json`, `stream-json` |
| `--permission-mode` | | Modo de permisos inicial |
| `--tools` | | Tools disponibles |
| `--allowedTools` | | Tools sin prompt de permiso |
| `--append-system-prompt` | | Agregar texto al prompt default |
| `--debug` | | Modo debug con filtro |

### Aliases de Modelo

| Alias | Modelo completo |
|-------|-----------------|
| `sonnet` | claude-sonnet-4-5-20250929 |
| `opus` | claude-opus-4-5-20250929 |
| `haiku` | claude-3-5-haiku-20241022 |

---

## Slash Commands (37 built-in)

| Comando | Descripción |
|---------|-------------|
| `/add-dir` | Agregar directorios de trabajo |
| `/agents` | Gestionar subagentes AI |
| `/bashes` | Listar tareas en background |
| `/bug` | Reportar bugs |
| `/clear` | Limpiar historial |
| `/compact` | Compactar conversación |
| `/config` | Abrir Settings |
| `/context` | Visualizar uso de contexto |
| `/cost` | Mostrar estadísticas de tokens |
| `/doctor` | Verificar instalación |
| `/exit` | Salir del REPL |
| `/export` | Exportar conversación |
| `/help` | Obtener ayuda |
| `/hooks` | Gestionar hooks |
| `/ide` | Gestionar integraciones IDE |
| `/init` | Inicializar CLAUDE.md |
| `/login` | Cambiar cuenta |
| `/logout` | Cerrar sesión |
| `/mcp` | Gestionar MCP |
| `/memory` | Editar archivos CLAUDE.md |
| `/model` | Cambiar modelo AI |
| `/permissions` | Ver/actualizar permisos |
| `/plugin` | Gestionar plugins |
| `/pr-comments` | Ver comentarios PR |
| `/rename` | Renombrar sesión |
| `/resume` | Reanudar conversación |
| `/review` | Solicitar revisión de código |
| `/rewind` | Retroceder conversación |
| `/sandbox` | Habilitar bash aislado |
| `/security-review` | Revisión de seguridad |
| `/stats` | Visualizar uso |
| `/status` | Ver status |
| `/statusline` | Configurar status line |
| `/terminal-setup` | Instalar Shift+Enter |
| `/todos` | Listar items TODO |
| `/usage` | Mostrar límites de plan |
| `/vim` | Entrar en modo vim |

### Quick Prefixes

| Prefijo | Descripción | Ejemplo |
|---------|-------------|---------|
| `/` | Slash command | `/help` |
| `#` | Memory shortcut | `# Este proyecto usa React` |
| `!` | Bash mode | `! npm test` |
| `@` | File path mention | `@src/index.ts` |

### Input Multilínea

| Método | Atajo |
|--------|-------|
| Quick escape | `\` + `Enter` |
| macOS default | `Option+Enter` |
| Terminal setup | `Shift+Enter` |

---

## Easter Eggs

| Mood | Referencia |
|------|------------|
| **Reticulating...** | Referencia a "Reticulating Splines" de SimCity/Maxis |
| **Clauding...** | Auto-referencia a Claude |
| **Honking...** | Posible referencia a Untitled Goose Game |
| **Wibbling...** | Término británico informal |
| **Vibing...** | Slang moderno de internet |

---

# PARTE III: REFERENCIAS

---

## Referencias Claude Code

### Moods / Thinking Verbs
- [Pallav Agarwal (Anthropic) - Lista oficial](https://x.com/pallavmac/status/1897491460636778693)
- [GitHub - anthropics/claude-code](https://github.com/anthropics/claude-code)
- [GitHub - Piebald-AI/tweakcc](https://github.com/Piebald-AI/tweakcc)

### Documentación Oficial
- [Claude Code Docs - Slash Commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands)
- [Claude Code Docs - CLI Reference](https://docs.anthropic.com/en/docs/claude-code/cli-reference)
- [Claude Code Docs - Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks)
- [Claude Code Docs - MCP](https://docs.anthropic.com/en/docs/claude-code/mcp)
- [Claude Code Docs - Memory](https://docs.anthropic.com/en/docs/claude-code/memory)
- [Claude Code Docs - Settings](https://docs.anthropic.com/en/docs/claude-code/settings)
- [Claude Code Docs - Plugins](https://docs.anthropic.com/en/docs/claude-code/plugins)
- [Claude Code Docs - Skills](https://docs.anthropic.com/en/docs/claude-code/skills)
- [Claude Code Docs - Agents](https://docs.anthropic.com/en/docs/claude-code/sub-agents)

### MCP
- [Model Context Protocol](https://modelcontextprotocol.io/introduction)
- [MCP Servers Registry](https://github.com/modelcontextprotocol/servers)

### Settings Schema
- [Settings JSON Schema](https://json.schemastore.org/claude-code-settings.json)

---

## Referencias NewCool

- [Documentación Técnica](/docs)
- [API Reference](/api)
- [GitHub Organization](https://github.com/newcool)
- [Status Page](https://status.newcool.io)

---

## Contacto y Soporte

### Chile
- Email: soporte@newcool.cl
- Horario: Lunes a Viernes, 9:00 - 18:00 (CLT)

### USA
- Email: support@newcool.io
- Hours: Monday - Friday, 9 AM - 6 PM (EST)

### Brasil
- Email: suporte@newcool.com.br
- WhatsApp: +55 11 XXXX-XXXX
- Horário: Segunda a Sexta, 9h - 18h (BRT)

---

## Resumen del Documento

### Ecosistema NewCool

| Métrica | Valor |
|---------|-------|
| Repositorios GitHub | 122 |
| Módulos Locales | 141 |
| Módulos Clasificados | 143 |
| Teams | 13 |
| Países | 5 |
| Tests documentados | 360 |

### Claude Code CLI

| Sección | Cantidad |
|---------|----------|
| Moods oficiales | 56 |
| Moods extendidos | 34 |
| **Total moods** | **90** |
| Agents built-in | 3 |
| Skills scopes | 4 |
| Plugin components | 6 |
| Settings scopes | 4 |
| CLAUDE.md niveles | 5 |
| MCP transports | 3 |
| Hook events | 10 |
| CLI flags | 35+ |
| Slash commands | 37 |
| Thinking triggers | 5 |
| Permission modes | 4 |

---

---

# PARTE IV: PLAN DE ACCIÓN

---

## Filosofía del Progreso

```
No medimos CUÁNDO llegas.
Medimos CUÁNTO has avanzado.

0% ════════════════════════════════════ 100%
   Cada paso cuenta. Sin prisa, sin pausa.
```

> Metodología NewCool: Progreso medible, no presión temporal
> "Cabeza fría, corazón encendido, respeto en cada pase"

### Reglas del Sistema

1. **Secuencia importa, tiempo no** - Fase 1 antes de Fase 2, pero a tu ritmo
2. **Cada checkbox = progreso real** - No hay tareas "medio hechas"
3. **Bloqueadores se documentan** - Si algo frena, se anota y se resuelve
4. **Celebrar cada 25%** - Micro-victorias mantienen momentum

---

## Mapa de Progreso General

```
ECOSISTEMA NEWCOOL - ACTIVACIÓN COMPLETA

[░░░░░░░░░░░░░░░░░░░░] 0%   ← Inicio
[████░░░░░░░░░░░░░░░░] 20%  ← Fundación lista
[████████░░░░░░░░░░░░] 40%  ← Núcleo operativo
[████████████░░░░░░░░] 60%  ← Productos activos
[████████████████░░░░] 80%  ← Todo integrado
[████████████████████] 100% ← Ecosistema sincronizado
```

---

## FASE 0: FUNDACIÓN
### Progreso: 0% → 15%

```
Meta: Cimientos sólidos para construir encima
Desbloquea: Todo lo demás
```

#### Checkpoint 0.1 — Documentación (0% → 5%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 0.1.1 | Crear repo `newcool-docs` | ⬜ | 1% |
| 0.1.2 | Mover 14 archivos de output | ⬜ | 1% |
| 0.1.3 | Estructura de carpetas definida | ⬜ | 1% |
| 0.1.4 | README del repo | ⬜ | 1% |
| 0.1.5 | Primer commit organizado | ⬜ | 1% |

#### Checkpoint 0.2 — Claude Code Global (5% → 10%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 0.2.1 | Copiar MASTER_PROMPT a `~/.claude/CLAUDE.md` | ⬜ | 2% |
| 0.2.2 | Crear `~/.claude/rules/` | ⬜ | 1% |
| 0.2.3 | Regla: monetizacion.md | ⬜ | 1% |
| 0.2.4 | Regla: codigo.md | ⬜ | 0.5% |
| 0.2.5 | Regla: letras.md | ⬜ | 0.5% |

#### Checkpoint 0.3 — NewCool ID Diseño (10% → 15%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 0.3.1 | Elegir auth provider | ⬜ | 1% |
| 0.3.2 | Schema de usuario definido | ⬜ | 2% |
| 0.3.3 | Tabla de permisos por producto | ⬜ | 1% |
| 0.3.4 | Documento de arquitectura auth | ⬜ | 1% |

```
FASE 0 COMPLETADA = 15%
[███░░░░░░░░░░░░░░░░░]
```

---

## FASE 1: NÚCLEO
### Progreso: 15% → 35%

```
Meta: El corazón del ecosistema latiendo
Requiere: Fase 0 completa
Desbloquea: Productos
```

#### Checkpoint 1.1 — API Central (15% → 22%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 1.1.1 | Proyecto API iniciado (Rust/Node) | ⬜ | 1% |
| 1.1.2 | Endpoint `/health` | ⬜ | 1% |
| 1.1.3 | Endpoint `/auth` | ⬜ | 2% |
| 1.1.4 | Endpoint `/users` | ⬜ | 1% |
| 1.1.5 | Endpoint `/search` | ⬜ | 1% |
| 1.1.6 | Endpoint `/music` | ⬜ | 1% |

#### Checkpoint 1.2 — Base de Datos (22% → 27%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 1.2.1 | PostgreSQL configurado | ⬜ | 1% |
| 1.2.2 | Schema de usuarios creado | ⬜ | 1% |
| 1.2.3 | Schema de módulos creado | ⬜ | 1% |
| 1.2.4 | Redis para cache | ⬜ | 1% |
| 1.2.5 | Conexión S3 verificada | ⬜ | 1% |

#### Checkpoint 1.3 — MCP Servers (27% → 35%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 1.3.1 | MCP: newcool-search | ⬜ | 2% |
| 1.3.2 | MCP: newcool-music | ⬜ | 2% |
| 1.3.3 | MCP: newcool-modules | ⬜ | 2% |
| 1.3.4 | MCP: mind-os | ⬜ | 2% |

```
FASE 1 COMPLETADA = 35%
[███████░░░░░░░░░░░░░]
```

---

## FASE 2: PRODUCTOS
### Progreso: 35% → 60%

```
Meta: Productos funcionando y generando valor
Requiere: Fase 1 completa
Desbloquea: Integración
```

#### Checkpoint 2.1 — Search V3 Pro Producción (35% → 40%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 2.1.1 | Deploy a producción | ⬜ | 2% |
| 2.1.2 | Índice de música cargado | ⬜ | 1% |
| 2.1.3 | Índice de cursos cargado | ⬜ | 1% |
| 2.1.4 | API pública documentada | ⬜ | 1% |

#### Checkpoint 2.2 — NewCool Music (40% → 47%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 2.2.1 | Conectar a API central | ⬜ | 2% |
| 2.2.2 | Integrar Search V3 | ⬜ | 1% |
| 2.2.3 | Player funcionando | ⬜ | 1% |
| 2.2.4 | Catálogo 4,734 canciones visible | ⬜ | 2% |
| 2.2.5 | NewCool ID login | ⬜ | 1% |

#### Checkpoint 2.3 — TuneStream Activación (47% → 54%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 2.3.1 | Modo TuneStream en app Music | ⬜ | 2% |
| 2.3.2 | Skin violeta/magenta | ⬜ | 1% |
| 2.3.3 | Estructura de cursos | ⬜ | 1% |
| 2.3.4 | Sistema de progreso | ⬜ | 2% |
| 2.3.5 | Primer curso completo | ⬜ | 1% |

#### Checkpoint 2.4 — Mind OS Beta (54% → 60%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 2.4.1 | Landing page | ⬜ | 1% |
| 2.4.2 | Sistema de pagos (Stripe) | ⬜ | 2% |
| 2.4.3 | 10 modos cognitivos activos | ⬜ | 2% |
| 2.4.4 | Beta con 10 usuarios | ⬜ | 1% |

```
FASE 2 COMPLETADA = 60%
[████████████░░░░░░░░]
```

---

## FASE 3: INTEGRACIÓN
### Progreso: 60% → 80%

```
Meta: Productos hablando entre sí
Requiere: Fase 2 completa
Desbloquea: Automatización
```

#### Checkpoint 3.1 — Cross-linking (60% → 67%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 3.1.1 | Búsqueda sugiere contenido cruzado | ⬜ | 2% |
| 3.1.2 | Music → TuneStream links | ⬜ | 2% |
| 3.1.3 | Deep links funcionando | ⬜ | 2% |
| 3.1.4 | Compartir entre apps | ⬜ | 1% |

#### Checkpoint 3.2 — EventBus (67% → 74%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 3.2.1 | EventBus implementado | ⬜ | 2% |
| 3.2.2 | Evento: user.created | ⬜ | 1% |
| 3.2.3 | Evento: content.indexed | ⬜ | 1% |
| 3.2.4 | Evento: course.completed | ⬜ | 1% |
| 3.2.5 | Evento: payment.received | ⬜ | 1% |
| 3.2.6 | Webhooks externos | ⬜ | 1% |

#### Checkpoint 3.3 — NewCool Hub (74% → 80%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 3.3.1 | Dashboard básico | ⬜ | 2% |
| 3.3.2 | Métricas en tiempo real | ⬜ | 1% |
| 3.3.3 | Gestión de usuarios | ⬜ | 1% |
| 3.3.4 | Subir contenido | ⬜ | 1% |
| 3.3.5 | Ver ingresos | ⬜ | 1% |

```
FASE 3 COMPLETADA = 80%
[████████████████░░░░]
```

---

## FASE 4: AUTOMATIZACIÓN
### Progreso: 80% → 95%

```
Meta: El sistema corre solo
Requiere: Fase 3 completa
Desbloquea: Escala
```

#### Checkpoint 4.1 — CI/CD (80% → 87%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 4.1.1 | GitHub Actions configurado | ⬜ | 2% |
| 4.1.2 | Deploy automático en push | ⬜ | 2% |
| 4.1.3 | Tests corren en PR | ⬜ | 1% |
| 4.1.4 | Rollback automático si falla | ⬜ | 1% |
| 4.1.5 | Notificación post-deploy | ⬜ | 1% |

#### Checkpoint 4.2 — Monitoreo (87% → 92%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 4.2.1 | Health checks cada 5 min | ⬜ | 1% |
| 4.2.2 | Alertas configuradas | ⬜ | 1% |
| 4.2.3 | Logs centralizados | ⬜ | 1% |
| 4.2.4 | Error tracking (Sentry) | ⬜ | 1% |
| 4.2.5 | Status page pública | ⬜ | 1% |

#### Checkpoint 4.3 — Hooks Claude Code (92% → 95%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 4.3.1 | Hook: post_commit | ⬜ | 1% |
| 4.3.2 | Hook: on_error | ⬜ | 1% |
| 4.3.3 | Auto-clasificación módulos | ⬜ | 1% |

```
FASE 4 COMPLETADA = 95%
[███████████████████░]
```

---

## FASE 5: ESCALA
### Progreso: 95% → 100%

```
Meta: Ecosistema completo y expandiendo
Requiere: Fase 4 completa
Resultado: Reloj sincronizado ⏰
```

#### Checkpoint 5.1 — Internacionalización (95% → 98%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 5.1.1 | Chile activo | ✅ | 0% (ya existe) |
| 5.1.2 | España activado | ⬜ | 0.75% |
| 5.1.3 | México activado | ⬜ | 0.75% |
| 5.1.4 | USA activado | ⬜ | 0.75% |
| 5.1.5 | Brasil activado | ⬜ | 0.75% |

#### Checkpoint 5.2 — Ecosistema Completo (98% → 100%)

| # | Tarea | Estado | Peso |
|---|-------|--------|------|
| 5.2.1 | 143 módulos documentados | ⬜ | 0.5% |
| 5.2.2 | Todos los MCP servers activos | ⬜ | 0.5% |
| 5.2.3 | Monetización fluyendo | ⬜ | 0.5% |
| 5.2.4 | Métricas de éxito alcanzadas | ⬜ | 0.5% |

```
FASE 5 COMPLETADA = 100%
[████████████████████] 🎉
```

---

## Resumen Ejecutivo del Plan

| Fase | Rango | Checkpoints | Tareas |
|------|-------|-------------|--------|
| 0 - Fundación | 0-15% | 3 | 14 |
| 1 - Núcleo | 15-35% | 3 | 15 |
| 2 - Productos | 35-60% | 4 | 18 |
| 3 - Integración | 60-80% | 3 | 15 |
| 4 - Automatización | 80-95% | 3 | 13 |
| 5 - Escala | 95-100% | 2 | 9 |
| **TOTAL** | **0-100%** | **18** | **84** |

---

## Tracker de Progreso

### Actualizar cada vez que completes una tarea:

```
PROGRESO ACTUAL: ____%

[░░░░░░░░░░░░░░░░░░░░] 0%
[██░░░░░░░░░░░░░░░░░░] 10%
[████░░░░░░░░░░░░░░░░] 20%
[██████░░░░░░░░░░░░░░] 30%
[████████░░░░░░░░░░░░] 40%
[██████████░░░░░░░░░░] 50%
[████████████░░░░░░░░] 60%
[██████████████░░░░░░] 70%
[████████████████░░░░] 80%
[██████████████████░░] 90%
[████████████████████] 100% 🎉
```

### Celebraciones:

- **25%** → Fundación + Núcleo iniciado 🌱
- **50%** → Productos tomando forma 🚀
- **75%** → Sistema integrado 🔗
- **100%** → Ecosistema sincronizado ⏰🎉

---

## Cómo Usar Este Documento

### Diario:

```
1. Abrir plan
2. Buscar siguiente tarea ⬜
3. Completarla
4. Marcar ✅
5. Actualizar % total
```

### Si hay bloqueador:

```
1. Marcar tarea con 🔴
2. Anotar bloqueador abajo
3. Buscar otra tarea que SÍ puedas hacer
4. Volver cuando se resuelva
```

### Bloqueadores actuales:

| Tarea | Bloqueador | Fecha | Resuelto |
|-------|------------|-------|----------|
| - | - | - | - |

---

## Dependencias (Secuencia Obligatoria)

```
FASE 0 ──► FASE 1 ──► FASE 2 ──► FASE 3 ──► FASE 4 ──► FASE 5
  │          │          │          │          │          │
  ▼          ▼          ▼          ▼          ▼          ▼
 15%        35%        60%        80%        95%       100%

NO puedes saltar fases.
SÍ puedes ir a tu ritmo dentro de cada fase.
```

---

## Principios NewCool

1. **Progreso > Perfección** — Mejor 1% hoy que 100% nunca
2. **Secuencia > Velocidad** — El orden importa, el tiempo no
3. **Documentar > Memorizar** — Si no está escrito, no existe
4. **Celebrar > Criticar** — Cada checkbox es una victoria
5. **Constancia > Intensidad** — Todos los días un poco

---

*"El reloj no tiene prisa, pero nunca se detiene"*

**Estado actual: 0%**
**Siguiente tarea: 0.1.1 — Crear repo newcool-docs**

---

*NEWCOOL MASTER PROMPT - Enero 2026*
*Ecosistema NewCool + Claude Code CLI + Plan de Acción*
*Versión unificada 2.0*

NewCool® 2026 🌍😎
