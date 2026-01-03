# Reglas de Código NewCool

## Stack Preferido

### Frontend
- **Framework:** Next.js 16 + React 19
- **Lenguaje:** TypeScript 5.7+ (strict mode)
- **Styling:** Tailwind CSS 4.0
- **State:** Zustand 5.x
- **Componentes:** Radix UI + Lucide icons
- **Forms:** React Hook Form + Zod

### Backend
- **Runtime:** Node.js 20+
- **API:** Next.js API Routes o Express
- **Auth:** Supabase Auth + JWT
- **Validation:** Zod everywhere

### Rust (para servicios de alto rendimiento)
- **Web:** Axum + Tokio
- **Search:** Tantivy
- **Serialization:** serde

### Mobile
- **Framework:** Flutter 3.x
- **State:** Provider 6.x

## Convenciones

### Nombres
```typescript
// Variables y funciones: camelCase
const userName = "test";
function getUserById(id: string) {}

// Tipos e interfaces: PascalCase
interface UserProfile {}
type AuthState = "loading" | "authenticated";

// Constantes: SCREAMING_SNAKE_CASE
const MAX_RETRIES = 3;
const API_BASE_URL = "https://api.newcool.io";

// Archivos: kebab-case
// user-profile.tsx, auth-service.ts
```

### Estructura de Proyecto
```
src/
├── components/     # Componentes React
├── hooks/          # Custom hooks
├── lib/            # Utilidades
├── services/       # API calls
├── stores/         # Zustand stores
├── types/          # TypeScript types
└── app/            # Next.js App Router
```

### Commits
```bash
# Conventional Commits
feat(module): add new feature
fix(module): fix bug
docs(module): update documentation
refactor(module): refactor code
test(module): add tests
chore(module): maintenance
```

## Prácticas

1. **TypeScript strict** - No `any`, no `as` innecesarios
2. **Zod para validación** - Runtime + compile time safety
3. **Error handling** - Try/catch con tipos de error
4. **Tests** - node:test para Node, cargo test para Rust
5. **No console.log en producción** - Usar logger apropiado

## Prohibiciones

- ❌ `any` type sin justificación
- ❌ `// @ts-ignore` sin comentario explicativo
- ❌ Secrets hardcodeados
- ❌ console.log en código de producción
- ❌ Dependencias sin lockfile

## Seguridad

- Validar TODA entrada de usuario con Zod
- Sanitizar output HTML
- Usar prepared statements para SQL
- HTTPS everywhere
- CORS configurado correctamente
