# NewCool ID - Arquitectura de Autenticación

> Sistema de identidad unificado para el ecosistema NewCool
> Versión: 1.0 | Enero 2026

---

## 1. Auth Provider Seleccionado

### Decisión: **Supabase Auth**

| Criterio | Supabase | Auth0 | Firebase | Clerk |
|----------|----------|-------|----------|-------|
| Costo inicial | Gratis | $23/mo | Gratis | $25/mo |
| SSO incluido | ✅ | ❌ (addon) | ❌ | ✅ |
| Self-host option | ✅ | ❌ | ❌ | ❌ |
| PostgreSQL nativo | ✅ | ❌ | ❌ | ❌ |
| Row Level Security | ✅ | ❌ | ❌ | ❌ |
| Latam friendly | ✅ | ✅ | ✅ | ❌ |
| **Puntuación** | **10/10** | 7/10 | 6/10 | 6/10 |

### Razones de Elección

1. **Integración nativa con PostgreSQL** - Ya usamos Supabase como DB
2. **Row Level Security** - Permisos a nivel de fila sin código extra
3. **Costo predecible** - Free tier generoso, pricing transparente
4. **Self-host posible** - Podemos migrar si es necesario
5. **JWT estándar** - Compatible con cualquier backend

---

## 2. Schema de Usuario

### Tabla: `users` (extends auth.users)

```sql
-- Supabase crea auth.users automáticamente
-- Extendemos con tabla pública

CREATE TABLE public.profiles (
  -- Identity
  id UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,

  -- Basic Info
  username TEXT UNIQUE NOT NULL,
  display_name TEXT,
  avatar_url TEXT,

  -- Contact
  email TEXT NOT NULL,
  phone TEXT,

  -- Location
  country_code TEXT DEFAULT 'CL',  -- ISO 3166-1 alpha-2
  timezone TEXT DEFAULT 'America/Santiago',
  locale TEXT DEFAULT 'es-CL',

  -- Subscription
  tier TEXT DEFAULT 'free' CHECK (tier IN ('free', 'pro', 'enterprise')),
  tier_expires_at TIMESTAMPTZ,

  -- Preferences
  preferences JSONB DEFAULT '{
    "theme": "system",
    "notifications": true,
    "newsletter": false
  }'::jsonb,

  -- Metadata
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW(),
  last_seen_at TIMESTAMPTZ,

  -- Flags
  is_verified BOOLEAN DEFAULT FALSE,
  is_creator BOOLEAN DEFAULT FALSE,
  is_admin BOOLEAN DEFAULT FALSE
);

-- Índices
CREATE INDEX idx_profiles_username ON public.profiles(username);
CREATE INDEX idx_profiles_country ON public.profiles(country_code);
CREATE INDEX idx_profiles_tier ON public.profiles(tier);

-- Trigger para updated_at
CREATE OR REPLACE FUNCTION update_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at = NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER profiles_updated_at
  BEFORE UPDATE ON public.profiles
  FOR EACH ROW
  EXECUTE FUNCTION update_updated_at();
```

### Tabla: `user_modules` (permisos por módulo)

```sql
CREATE TABLE public.user_modules (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,
  module_id TEXT NOT NULL,  -- e.g., 'mind-os', 'fixmatch', 'search-v3'

  -- Access
  access_level TEXT DEFAULT 'free' CHECK (access_level IN ('free', 'basic', 'pro', 'unlimited')),
  granted_at TIMESTAMPTZ DEFAULT NOW(),
  expires_at TIMESTAMPTZ,

  -- Usage
  usage_count INTEGER DEFAULT 0,
  usage_limit INTEGER,  -- NULL = unlimited
  last_used_at TIMESTAMPTZ,

  -- Billing
  subscription_id TEXT,  -- Stripe subscription ID

  UNIQUE(user_id, module_id)
);

CREATE INDEX idx_user_modules_user ON public.user_modules(user_id);
CREATE INDEX idx_user_modules_module ON public.user_modules(module_id);
```

### Tabla: `sessions` (tracking)

```sql
CREATE TABLE public.sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES public.profiles(id) ON DELETE CASCADE,

  -- Session Info
  device_type TEXT,  -- 'desktop', 'mobile', 'tablet'
  browser TEXT,
  os TEXT,
  ip_address INET,

  -- Location
  country TEXT,
  city TEXT,

  -- Timestamps
  created_at TIMESTAMPTZ DEFAULT NOW(),
  last_active_at TIMESTAMPTZ DEFAULT NOW(),
  expires_at TIMESTAMPTZ
);

CREATE INDEX idx_sessions_user ON public.sessions(user_id);
CREATE INDEX idx_sessions_active ON public.sessions(last_active_at);
```

---

## 3. Tabla de Permisos por Producto

### Matriz de Acceso

| Módulo | Free | Basic | Pro | Enterprise |
|--------|------|-------|-----|------------|
| **T01 - Infrastructure** |
| newcool-auth | ✅ | ✅ | ✅ | ✅ |
| newcool-hub | View | Edit | Admin | Full |
| newcool-analytics | Basic | Full | Full | Custom |
| **T02 - Browser & AI** |
| newcool-atlas-v4 | ✅ | ✅ | ✅ | ✅ |
| newcool-search-v3 | 100/día | 1K/día | 10K/día | Unlimited |
| **T03 - Education** |
| All modules | ✅ | ✅ | ✅ | ✅ |
| **T04 - Streaming** |
| newcool-music | Ads | No-ads | Offline | API |
| **T06 - FixMatch** |
| Consumer | ✅ | ✅ | ✅ | ✅ |
| Provider | ❌ | Basic | Full | Multi-account |
| **T07 - Government** |
| All modules | ✅ | ✅ | ✅ | ✅ |
| **T08 - Professional** |
| Tools | Limited | Basic | Full | Custom |
| **T10 - AI Factory** |
| Image gen | 10/día | 50/día | 200/día | Unlimited |
| Video gen | ❌ | 5/día | 20/día | Unlimited |
| **T11 - Mind OS** |
| Modes 1-3 | ✅ | ✅ | ✅ | ✅ |
| Modes 4-10 | ❌ | ❌ | ✅ | ✅ |

### Definición de Tiers

```typescript
interface TierDefinition {
  id: 'free' | 'basic' | 'pro' | 'enterprise';
  name: string;
  price: {
    CLP: number;
    USD: number;
    EUR: number;
    BRL: number;
    MXN: number;
  };
  billing: 'monthly' | 'yearly' | 'custom';
  features: string[];
}

const TIERS: TierDefinition[] = [
  {
    id: 'free',
    name: 'Gratuito',
    price: { CLP: 0, USD: 0, EUR: 0, BRL: 0, MXN: 0 },
    billing: 'monthly',
    features: [
      'Acceso a educación completa',
      'Servicios públicos ilimitados',
      'Búsqueda básica',
      'Mind OS modos 1-3'
    ]
  },
  {
    id: 'basic',
    name: 'Básico',
    price: { CLP: 4990, USD: 4.99, EUR: 4.99, BRL: 24.90, MXN: 89 },
    billing: 'monthly',
    features: [
      'Todo en Free',
      'Sin publicidad',
      'Herramientas profesionales básicas',
      'Soporte prioritario'
    ]
  },
  {
    id: 'pro',
    name: 'Pro',
    price: { CLP: 9990, USD: 9.99, EUR: 9.99, BRL: 49.90, MXN: 179 },
    billing: 'monthly',
    features: [
      'Todo en Basic',
      'Mind OS completo',
      'AI Generation aumentado',
      'FixMatch Provider',
      'Offline mode'
    ]
  },
  {
    id: 'enterprise',
    name: 'Enterprise',
    price: { CLP: 0, USD: 0, EUR: 0, BRL: 0, MXN: 0 }, // Custom
    billing: 'custom',
    features: [
      'Todo en Pro',
      'API access',
      'SSO/SAML',
      'SLA garantizado',
      'Account manager'
    ]
  }
];
```

---

## 4. Arquitectura de Autenticación

### Flujo de Login

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Cliente   │────▶│  Supabase   │────▶│  PostgreSQL │
│  (Next.js)  │◀────│    Auth     │◀────│   (Perfiles)│
└─────────────┘     └─────────────┘     └─────────────┘
       │                   │
       │                   ▼
       │            ┌─────────────┐
       │            │    JWT      │
       │            │   Token     │
       │            └─────────────┘
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│   API       │◀────│   Verify    │
│  Routes     │     │    JWT      │
└─────────────┘     └─────────────┘
```

### Métodos de Autenticación

| Método | Prioridad | Implementación |
|--------|-----------|----------------|
| Email + Password | P0 | Supabase native |
| Magic Link | P0 | Supabase native |
| Google OAuth | P1 | Supabase OAuth |
| GitHub OAuth | P1 | Supabase OAuth |
| Apple Sign In | P2 | Supabase OAuth |
| Phone OTP | P2 | Supabase + Twilio |

### JWT Structure

```typescript
interface NewCoolJWT {
  // Standard claims
  sub: string;        // user_id (UUID)
  aud: string;        // 'authenticated'
  exp: number;        // Expiration timestamp
  iat: number;        // Issued at

  // Supabase claims
  email: string;
  phone?: string;
  app_metadata: {
    provider: string;
    providers: string[];
  };

  // NewCool custom claims
  user_metadata: {
    username: string;
    display_name: string;
    avatar_url?: string;
    country_code: string;
    tier: 'free' | 'basic' | 'pro' | 'enterprise';
    is_verified: boolean;
    is_creator: boolean;
    is_admin: boolean;
  };
}
```

### Row Level Security Policies

```sql
-- Profiles: usuarios ven solo su perfil
CREATE POLICY "Users can view own profile"
  ON public.profiles FOR SELECT
  USING (auth.uid() = id);

CREATE POLICY "Users can update own profile"
  ON public.profiles FOR UPDATE
  USING (auth.uid() = id);

-- Profiles públicos (username, avatar)
CREATE POLICY "Public profiles are viewable"
  ON public.profiles FOR SELECT
  USING (true);  -- Solo campos públicos via view

-- User modules: solo propietario
CREATE POLICY "Users can view own modules"
  ON public.user_modules FOR SELECT
  USING (auth.uid() = user_id);

-- Sessions: solo propietario
CREATE POLICY "Users can view own sessions"
  ON public.sessions FOR SELECT
  USING (auth.uid() = user_id);
```

---

## 5. API Endpoints

### Auth Routes

```typescript
// POST /api/auth/signup
interface SignupRequest {
  email: string;
  password: string;
  username: string;
  country_code?: string;
}

// POST /api/auth/login
interface LoginRequest {
  email: string;
  password: string;
}

// POST /api/auth/logout
// No body required

// POST /api/auth/magic-link
interface MagicLinkRequest {
  email: string;
}

// POST /api/auth/refresh
// Uses refresh token from cookie

// GET /api/auth/me
// Returns current user profile

// PATCH /api/auth/me
interface UpdateProfileRequest {
  display_name?: string;
  avatar_url?: string;
  preferences?: object;
}
```

### Permission Checking

```typescript
// Middleware para verificar permisos
async function checkModuleAccess(
  userId: string,
  moduleId: string,
  requiredLevel: 'free' | 'basic' | 'pro' | 'unlimited'
): Promise<boolean> {
  const { data } = await supabase
    .from('user_modules')
    .select('access_level, expires_at, usage_count, usage_limit')
    .eq('user_id', userId)
    .eq('module_id', moduleId)
    .single();

  if (!data) return requiredLevel === 'free';

  // Check expiration
  if (data.expires_at && new Date(data.expires_at) < new Date()) {
    return false;
  }

  // Check usage limit
  if (data.usage_limit && data.usage_count >= data.usage_limit) {
    return false;
  }

  // Check level
  const levels = ['free', 'basic', 'pro', 'unlimited'];
  return levels.indexOf(data.access_level) >= levels.indexOf(requiredLevel);
}
```

---

## 6. Integración con Productos

### SDK de Autenticación

```typescript
// @newcool/auth - SDK compartido

import { createClient } from '@supabase/supabase-js';

export class NewCoolAuth {
  private supabase;

  constructor(url: string, key: string) {
    this.supabase = createClient(url, key);
  }

  async signUp(email: string, password: string, username: string) {
    const { data, error } = await this.supabase.auth.signUp({
      email,
      password,
      options: {
        data: { username }
      }
    });
    return { data, error };
  }

  async signIn(email: string, password: string) {
    return this.supabase.auth.signInWithPassword({ email, password });
  }

  async signOut() {
    return this.supabase.auth.signOut();
  }

  async getUser() {
    return this.supabase.auth.getUser();
  }

  async getProfile(userId: string) {
    return this.supabase
      .from('profiles')
      .select('*')
      .eq('id', userId)
      .single();
  }

  async checkAccess(moduleId: string, level: string) {
    const { data: { user } } = await this.getUser();
    if (!user) return false;

    return checkModuleAccess(user.id, moduleId, level);
  }
}

export const auth = new NewCoolAuth(
  process.env.SUPABASE_URL!,
  process.env.SUPABASE_ANON_KEY!
);
```

---

## 7. Seguridad

### Checklist

- [ ] HTTPS everywhere
- [ ] Tokens in httpOnly cookies
- [ ] CSRF protection
- [ ] Rate limiting (100 req/min login)
- [ ] Brute force protection
- [ ] Password requirements (8+ chars, mixed)
- [ ] Email verification required
- [ ] Session invalidation on password change
- [ ] Audit logging

### Variables de Entorno

```bash
# .env.local
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=eyJ...
SUPABASE_SERVICE_KEY=eyJ... # Solo backend

# Opcional
GOOGLE_CLIENT_ID=xxx
GOOGLE_CLIENT_SECRET=xxx
GITHUB_CLIENT_ID=xxx
GITHUB_CLIENT_SECRET=xxx
```

---

## 8. Roadmap

| Fase | Features | Estado |
|------|----------|--------|
| MVP | Email/Password, Magic Link, Profiles | 🔄 |
| v1.1 | Google/GitHub OAuth | ⏳ |
| v1.2 | Phone OTP, Apple Sign In | ⏳ |
| v2.0 | SSO/SAML Enterprise | ⏳ |
| v2.1 | Passkeys/WebAuthn | ⏳ |

---

*NewCool ID - "Una identidad, todo el ecosistema"*
*Enero 2026*
