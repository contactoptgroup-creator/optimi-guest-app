# Option B - Solution Supabase + Admin Dashboard

## Vue d'ensemble

Cette option utilise **Supabase** (alternative open-source à Firebase) pour:
- Stocker les données des appartements
- Gérer les utilisateurs admin
- Dashboard de gestion
- API temps réel

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         FRONTEND                                 │
├─────────────────────────────────────────────────────────────────┤
│  Guest App (PWA)          │  Admin Dashboard                    │
│  - index.html             │  - admin/index.html                 │
│  - Chargement dynamique   │  - CRUD appartements                │
│  - Multi-langues          │  - Stats & analytics                │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                        SUPABASE                                  │
├─────────────────────────────────────────────────────────────────┤
│  Database (PostgreSQL)    │  Auth          │  Storage           │
│  - apartments             │  - Admin users │  - Photos apparts  │
│  - bookings               │  - JWT tokens  │  - Documents       │
│  - services               │                │                    │
│  - analytics              │                │                    │
└─────────────────────────────────────────────────────────────────┘
```

## Schéma Base de Données

### Table: apartments
```sql
CREATE TABLE apartments (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    slug VARCHAR(100) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    address TEXT NOT NULL,
    type VARCHAR(20),
    surface INTEGER,
    bedrooms INTEGER,
    bathrooms INTEGER,
    max_guests INTEGER,
    image_url TEXT,
    images JSONB DEFAULT '[]',
    wifi_name VARCHAR(100),
    wifi_password VARCHAR(100),
    host_name VARCHAR(100),
    host_phone VARCHAR(20),
    host_email VARCHAR(100),
    booking_airbnb_url TEXT,
    booking_direct_url TEXT,
    review_url TEXT,
    checkin_time TIME DEFAULT '15:00',
    checkin_instructions JSONB DEFAULT '[]',
    access_codes JSONB DEFAULT '{}',
    checkout_time TIME DEFAULT '11:00',
    checkout_instructions JSONB DEFAULT '[]',
    rules JSONB DEFAULT '[]',
    neighborhood_description TEXT,
    neighborhood_places JSONB DEFAULT '[]',
    appliances JSONB DEFAULT '[]',
    services JSONB DEFAULT '{}',
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Index pour recherche rapide
CREATE INDEX idx_apartments_slug ON apartments(slug);
CREATE INDEX idx_apartments_status ON apartments(status);
```

### Table: services_requests
```sql
CREATE TABLE services_requests (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    apartment_id UUID REFERENCES apartments(id),
    service_type VARCHAR(50) NOT NULL,
    guest_name VARCHAR(100),
    guest_phone VARCHAR(20),
    guest_email VARCHAR(100),
    requested_date DATE,
    notes TEXT,
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```

### Table: analytics
```sql
CREATE TABLE analytics (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    apartment_id UUID REFERENCES apartments(id),
    event_type VARCHAR(50) NOT NULL,
    event_data JSONB DEFAULT '{}',
    user_agent TEXT,
    ip_address INET,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Index pour reporting
CREATE INDEX idx_analytics_apartment ON analytics(apartment_id);
CREATE INDEX idx_analytics_date ON analytics(created_at);
```

## Configuration Supabase

### 1. Créer un projet Supabase

1. Aller sur https://supabase.com
2. Créer un nouveau projet
3. Noter les credentials:
   - Project URL: `https://xxxxx.supabase.co`
   - Anon Key: `eyJhbGciOi...`
   - Service Role Key: `eyJhbGciOi...` (pour admin)

### 2. Variables d'environnement

```javascript
// config.js
const SUPABASE_URL = 'https://xxxxx.supabase.co';
const SUPABASE_ANON_KEY = 'eyJhbGciOi...';
```

### 3. Row Level Security (RLS)

```sql
-- Lecture publique des appartements actifs
CREATE POLICY "Public read active apartments" ON apartments
    FOR SELECT USING (status = 'active');

-- Admin peut tout faire
CREATE POLICY "Admin full access" ON apartments
    FOR ALL USING (auth.role() = 'authenticated');

-- Analytics: insertion publique, lecture admin only
CREATE POLICY "Public insert analytics" ON analytics
    FOR INSERT WITH CHECK (true);

CREATE POLICY "Admin read analytics" ON analytics
    FOR SELECT USING (auth.role() = 'authenticated');
```

## Code Frontend (Guest App)

### Chargement dynamique des données

```javascript
// js/supabase-client.js
import { createClient } from 'https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2/+esm';

const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY);

// Charger un appartement par slug
async function loadApartment(slug) {
    const { data, error } = await supabase
        .from('apartments')
        .select('*')
        .eq('slug', slug)
        .single();

    if (error) {
        console.error('Error loading apartment:', error);
        return null;
    }

    return data;
}

// Tracker un événement
async function trackEvent(apartmentId, eventType, eventData = {}) {
    await supabase.from('analytics').insert({
        apartment_id: apartmentId,
        event_type: eventType,
        event_data: eventData,
        user_agent: navigator.userAgent
    });
}

// Demander un service
async function requestService(apartmentId, serviceType, guestInfo) {
    const { data, error } = await supabase
        .from('services_requests')
        .insert({
            apartment_id: apartmentId,
            service_type: serviceType,
            guest_name: guestInfo.name,
            guest_phone: guestInfo.phone,
            guest_email: guestInfo.email,
            notes: guestInfo.notes
        });

    return { data, error };
}

export { supabase, loadApartment, trackEvent, requestService };
```

### URL Schema

```
https://guest.optimigroup.fr/tour-eiffel-suffren
https://guest.optimigroup.fr/bastille-moreau
https://guest.optimigroup.fr/?apt=suffren-001
```

## Admin Dashboard

### Fonctionnalités

1. **Liste des appartements**
   - Vue tableau avec filtres
   - Statut (actif/inactif)
   - Actions rapides

2. **Édition appartement**
   - Formulaire complet
   - Upload photos
   - Preview en temps réel

3. **Demandes de services**
   - Liste des demandes
   - Statuts (pending/confirmed/cancelled)
   - Notifications email

4. **Analytics**
   - Vues par appartement
   - Services les plus demandés
   - Heures de pointe

### Structure Admin Dashboard

```
admin/
├── index.html          # Dashboard principal
├── apartments.html     # Liste appartements
├── apartment-edit.html # Édition appartement
├── services.html       # Demandes de services
├── analytics.html      # Statistiques
└── js/
    ├── admin-auth.js   # Authentification
    ├── admin-api.js    # API calls
    └── admin-ui.js     # UI components
```

## Coûts

### Supabase Free Tier
- ✅ 500 MB database
- ✅ 1 GB file storage
- ✅ 50,000 monthly active users
- ✅ 2 GB bandwidth
- ✅ **Suffisant pour 50-100 appartements**

### Supabase Pro ($25/mois)
- 8 GB database
- 100 GB file storage
- Unlimited API requests
- Daily backups
- **Recommandé pour 100+ appartements**

## Avantages

- ✅ Modification instantanée sans redéploiement
- ✅ Dashboard admin complet
- ✅ Analytics intégrés
- ✅ Scalable
- ✅ API temps réel
- ✅ Authentification sécurisée

## Inconvénients

- ⚠️ Dépendance à un service tiers
- ⚠️ Complexité accrue
- ⚠️ Nécessite maintenance

## Prochaines étapes

1. [ ] Créer projet Supabase
2. [ ] Exécuter scripts SQL
3. [ ] Configurer RLS
4. [ ] Adapter index.html pour chargement dynamique
5. [ ] Créer admin dashboard
6. [ ] Migrer données apartments.json vers Supabase
7. [ ] Tests et déploiement
