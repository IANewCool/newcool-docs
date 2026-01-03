# CLAUDE.md - NewCool Ecosystem

> **Version:** 2.0
> **Fecha:** Enero 2026
> **Repositorios GitHub:** 122
> **Modulos Locales:** 141

---

## Arquitectura del Ecosistema

### Departamentos (13 Teams)

| Team | Nombre | Modulos | Estado | Prioridad |
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

## Stack Tecnologico

### Frontend

| Tecnologia | Version | Uso |
|------------|---------|-----|
| Next.js | 16 | Framework principal |
| React | 19 | UI Library |
| TypeScript | 5.7+ | Lenguaje |
| Tailwind CSS | 4.0 | Styling |
| Zustand | 5.x | State management |
| Framer Motion | 11-12 | Animaciones |
| Radix UI | Latest | Componentes accesibles |
| Lucide React | Latest | Iconos |
| Recharts | 2.x | Graficos |
| React Hook Form + Zod | Latest | Formularios + validacion |

### Backend

| Tecnologia | Version | Uso |
|------------|---------|-----|
| Node.js | 20+ | Runtime |
| Express.js | Latest | Web framework |
| Next.js API Routes | 16 | API endpoints |
| Supabase Auth + JWT | Latest | Autenticacion |
| Zod | Latest | Validacion |
| Helmet + CORS | Latest | Seguridad |

### Rust Services

| Proyecto | Tecnologias | Tests |
|----------|-------------|-------|
| newcool-search-v3 | Tantivy, Axum, Tokio, Qdrant, Redis | - |
| newcool-apex-learning | DDD, serde, thiserror | 52 tests |
| newcool-hub | NAPI-RS, Tantivy BM25 | - |

### Python Services

| Proyecto | Tecnologias |
|----------|-------------|
| newcool-neo | Python 3.11+, asyncio, aiosqlite, Click, Rich |
| chromium-asset-generator | Pillow, rsvg-convert |

### Mobile (Flutter)

| Tecnologia | Version |
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

| Tipo | Tecnologia |
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

### Clasificacion de Modulos (143 total)

| Categoria | Cantidad | Modelo de Ingreso |
|-----------|----------|-------------------|
| Servicio Publico | 37 | 100% Gratuito |
| Educacion | 21 | Gratuito + Donaciones |
| API/Infrastructure | 18 | Pay-per-use |
| Creatividad | 14 | Gratuito/Freemium |
| Herramienta Personal | 13 | Gratuito + Donaciones |
| Herramienta Profesional | 11 | Freemium |
| Industrial | 9 | Pricing Custom |
| Community | 9 | Gratuito |
| AI Generation | 5 | Pay-per-use |
| Marketplace | 2 | 20% Comision |
| Sostenibilidad | 2 | Gratuito |
| Premium | 1 | Suscripcion |

### Potencial de Ingresos

| Tipo | Cantidad | Porcentaje |
|------|----------|------------|
| Modulos gratuitos | 82 | 57% |
| Modulos freemium | 25 | 17% |
| Modulos pagados | 34 | 24% |
| Custom/Enterprise | 2 | 1% |

---

## Modulos por Categoria

### Servicio Publico (37 modulos)

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

### Educacion (21 modulos)

```
newcool-math, newcool-english, newcool-science, newcool-history,
newcool-geography, newcool-civics, newcool-reading, newcool-writing,
newcool-teacher, newcool-quiz, newcool-ai, newcool-datascience,
newcool-code, newcool-desaprender, newcool-ciencia, newcool-cultura,
newcooltura, newcool-informada, newcool-leadership, newcool-translate,
newcool-music-edu
```

### Herramienta Personal (13 modulos)

```
newcool-cocina, newcool-mascota, newcool-mascotas, newcool-hogar,
newcool-horoscopo, newcool-fortuna, newcool-finance, newcool-personalidad,
newcool-salud, newcool-deporte, newcool-turismo, newcool-vivienda,
newcool-transporte
```

### Herramienta Profesional (11 modulos)

```
newcool-contabilidad, newcool-rrhh, newcool-finiquito, newcool-emprendimiento,
newcool-marketing, newcool-mantencion, newcool-startup, newcool-ecommerce,
newcool-nocode, newcool-product, newcool-impact
```

### Industrial (9 modulos)

```
newcool-scada, newcool-robotics, newcool-robots, newcool-biotech,
newcool-energia, newcool-energy, newcool-cybersecurity, newcool-devops,
newcool-blockchain
```

### API/Infrastructure (18 modulos)

```
newcool-auth, newcool-hub, newcool-cloud, newcool-analytics, newcool-sso,
newcool-search-v3, newcool-indexing, newcool-notifications, newcool-offline,
newcool-ethical-framework, newcool-content-uploader, newcool-assets-pipeline,
newcool-atlas-v4, newcool-atlas-patches, chromium, chromium-asset-generator,
newcool-neo, newcool-cerebro
```

### Creatividad (14 modulos)

```
newcool-art, newcool-motion, newcool-video, newcool-photography,
newcool-podcast, newcool-music-pipeline, newcool-music-engine-clean,
newcool-drama, newcool-terror, newcool-farandula, newcool-memes,
newcool-streaming, newcool-streaming-platform, newcool-3dvr
```

### AI Generation (5 modulos)

```
newcool-ai-factory-os, newcool-anime-factory, universo-newcool-assets,
newcool-astral, newcool-gamedev
```

### Community (9 modulos)

```
newcool-community, newcool-feedback, newcool-gamification, newcool-moderation,
newcool-events, newcool-t12-shared, newcool-medioambiente, newcool-planet,
newcool-circular
```

### Premium (1 modulo)

```
newcool-mind-os
```

### Marketplace (2 modulos)

```
fixmatch, fixmatch-launch
```

### Sostenibilidad (2 modulos)

```
newcool-greentech, newcool-uxui
```

---

## Sistemas Principales

### Atlas V4 (Browser AI)

- **21 modulos** con 182 tests
- Chromium 145.0.7560 rebrand
- Integracion con Search V3 y AI Factory
- Soporte para 81 idiomas

### Search V3 (Rust)

- Motor hibrido BM25 + semantic search
- Tantivy como base
- Qdrant para vectores
- Redis para cache

### Mind OS (Premium)

- Flutter multi-plataforma
- 10 modos cognitivos (3 gratis, 7 Pro)
- Suscripcion mensual/anual

### AI Factory OS

- Generacion de imagenes (Flux/SDXL)
- Generacion de video (Runway)
- Generacion de audio (Suno)

### APEX Learning (Rust)

- Motor de aprendizaje etico
- Domain-Driven Design
- 52 tests

### T12 Community

- EventBus via BroadcastChannel API
- 8 modulos integrados
- Gamificacion y moderacion

### Universo NewCool Assets

- 1,040 archivos
- 7 personajes principales
- 7 mundos
- LoRA training configs

---

## Expansion Geografica

### Chile (Base)

- Dominio: newcool.cl
- Moneda: CLP
- Regulaciones: Ley de Proteccion de Datos
- Gateway: Transbank, MercadoPago

### Espana

- Dominio: newcool.es
- Moneda: EUR
- Regulaciones: RGPD
- Gateway: Stripe, Redsys

### Mexico

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
| Donacion sugerida | $1.000 - $5.000 | $10.000 - $50.000 |
| Profesional Basico | $9.990 | $99.900 |
| Profesional Pro | $19.990 - $29.990 | $199.900 |
| Mind OS Premium | $9.990/mes | $79.900/ano |

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
| Donacao sugerida | R$ 5 - R$ 25 | R$ 50 - R$ 250 |
| Profissional Basico | R$ 49,90 | R$ 499 |
| Profissional Pro | R$ 99,90 - R$ 149,90 | R$ 999 |
| Mind OS Premium | R$ 49,90/mes | R$ 399/ano |

---

## Comandos Frecuentes

### Desarrollo

```bash
# Instalar dependencias
npm install

# Desarrollo local
npm run dev

# Build
npm run build

# Tests
npm test
node --test tests/

# Lint
npm run lint
```

### Git

```bash
# Feature branch
git checkout -b feature/nombre

# Commit con conventional commits
git commit -m "feat(modulo): descripcion"

# Push
git push origin feature/nombre
```

### Rust

```bash
# Build
cargo build --release

# Tests
cargo test

# Lint
cargo clippy
cargo fmt
```

### Flutter

```bash
# Run
flutter run

# Build
flutter build apk
flutter build ios

# Tests
flutter test
```

### Docker

```bash
# Build
docker build -t newcool-app .

# Run
docker-compose up -d
```

### Vercel

```bash
# Deploy
vercel --prod

# Preview
vercel
```

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
- `list_asset_categories` - Listar categorias
- `get_assets` - Obtener assets por categoria
- `get_asset_url` - URL de asset especifico
- `generate_asset` - Generar asset con AI
- `generate_batch` - Generar multiples assets
- `search_free_images` - Buscar en Unsplash
- `list_ecosystem_modules` - Listar modulos NewCool
- `get_module_assets` - Assets por modulo
- `list_visual_styles` - Estilos disponibles
- `list_asset_templates` - Templates disponibles

---

## Contacto y Soporte

### Chile

- Email: soporte@newcool.cl
- Horario: Lunes a Viernes, 9:00 - 18:00 (CLT)

### USA

- Email: support@newcool.io
- Phone: +1 (XXX) XXX-XXXX
- Hours: Monday - Friday, 9 AM - 6 PM (EST)

### Brasil

- Email: suporte@newcool.com.br
- WhatsApp: +55 11 XXXX-XXXX
- Horario: Segunda a Sexta, 9h - 18h (BRT)

---

## Referencias

- [Documentacion Tecnica](/docs)
- [API Reference](/api)
- [GitHub Organization](https://github.com/newcool)
- [Status Page](https://status.newcool.io)

---

*NewCool Ecosystem - Enero 2026*
*122 repos GitHub + 141 modulos locales = 143 modulos clasificados*
