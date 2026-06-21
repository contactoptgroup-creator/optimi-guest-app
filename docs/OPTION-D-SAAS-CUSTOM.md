# Option D - SaaS Custom OPT IM GROUP

## Vision

Créer **OPTIMI Guest** — une plateforme SaaS complète de Guest Experience que OPT IM GROUP peut:
1. Utiliser pour sa propre flotte
2. Vendre/licencier à d'autres conciergeries
3. White-labeler pour des partenaires

---

## Proposition de valeur

### Pour OPT IM GROUP
- Zéro frais par listing (économie ~400€/mois vs Duve)
- Contrôle total sur l'expérience
- Différenciation concurrentielle
- Asset technologique valorisable

### Pour autres conciergeries (clients potentiels)
- Solution moins chère que Duve/Guesty
- Support francophone
- Adapté au marché français
- Personnalisation poussée

---

## Architecture Technique

### Stack recommandé

```
┌─────────────────────────────────────────────────────────────────┐
│                         FRONTEND                                 │
├─────────────────────────────────────────────────────────────────┤
│  Guest App (PWA)          │  Admin Dashboard    │  Super Admin  │
│  Next.js / Nuxt.js        │  React / Vue        │  React        │
│  Tailwind CSS             │  Tailwind CSS       │  Tailwind CSS │
│  PWA + Offline            │  Charts.js          │  Multi-tenant │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                         BACKEND                                  │
├─────────────────────────────────────────────────────────────────┤
│  API                      │  Auth               │  Storage       │
│  Node.js + Express        │  NextAuth / Clerk   │  Cloudflare R2 │
│  ou Supabase Edge         │  ou Supabase Auth   │  ou Supabase   │
│  Functions                │                     │                │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                        DATABASE                                  │
├─────────────────────────────────────────────────────────────────┤
│  PostgreSQL (Supabase)    │  Redis (optionnel)                  │
│  - Multi-tenant           │  - Cache                            │
│  - Row Level Security     │  - Sessions                         │
│  - Realtime               │  - Rate limiting                    │
└─────────────────────────────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│                       INTÉGRATIONS                               │
├─────────────────────────────────────────────────────────────────┤
│  Airbnb API    │  Booking.com   │  Stripe      │  Twilio/WhatsApp│
│  (via Hostaway │  API           │  Payments    │  Messaging      │
│  ou direct)    │                │              │                 │
└─────────────────────────────────────────────────────────────────┘
```

### Multi-tenancy

```
organization (tenant)
├── id
├── name (ex: "OPT IM GROUP", "SuperHote", etc.)
├── slug (ex: "optimi", "superhote")
├── plan (free, pro, enterprise)
├── custom_domain (ex: guest.optimigroup.fr)
├── branding (logo, colors, etc.)
└── settings
    ├── apartments[]
    ├── users[]
    ├── services[]
    └── analytics[]
```

---

## Fonctionnalités MVP

### Phase 1 - Guest App (2-4 semaines)

| Feature | Priorité | Complexité |
|---------|----------|------------|
| Page d'accueil appartement | P1 | ⭐ |
| WiFi (copie en 1 clic) | P1 | ⭐ |
| Instructions check-in/out | P1 | ⭐ |
| Règlement intérieur | P1 | ⭐ |
| Infos quartier | P1 | ⭐⭐ |
| Numéros d'urgence | P1 | ⭐ |
| Multi-langues (FR/EN) | P1 | ⭐⭐ |
| PWA installable | P1 | ⭐⭐ |
| Lien réservation externe | P1 | ⭐ |
| QR code par appartement | P1 | ⭐ |

### Phase 2 - Admin Dashboard (2-4 semaines)

| Feature | Priorité | Complexité |
|---------|----------|------------|
| Auth admin (email/password) | P1 | ⭐⭐ |
| CRUD appartements | P1 | ⭐⭐ |
| Upload photos | P1 | ⭐⭐ |
| Génération QR codes | P1 | ⭐ |
| Preview Guest App | P2 | ⭐⭐ |
| Analytics basiques | P2 | ⭐⭐ |
| Export données | P3 | ⭐⭐ |

### Phase 3 - Services & Upselling (2-4 semaines)

| Feature | Priorité | Complexité |
|---------|----------|------------|
| Catalogue services | P1 | ⭐⭐ |
| Demande de service | P1 | ⭐⭐ |
| Notifications (email/WhatsApp) | P1 | ⭐⭐⭐ |
| Paiement intégré (Stripe) | P2 | ⭐⭐⭐ |
| Calendrier disponibilité services | P3 | ⭐⭐⭐ |

### Phase 4 - Réservation Directe (4-6 semaines)

| Feature | Priorité | Complexité |
|---------|----------|------------|
| Widget réservation | P1 | ⭐⭐⭐ |
| Calendrier disponibilités | P1 | ⭐⭐⭐ |
| Sync Airbnb/Booking (iCal) | P1 | ⭐⭐⭐ |
| Paiement réservation | P1 | ⭐⭐⭐ |
| Confirmation automatique | P2 | ⭐⭐ |
| Channel Manager basique | P3 | ⭐⭐⭐⭐ |

### Phase 5 - Multi-tenant SaaS (4-6 semaines)

| Feature | Priorité | Complexité |
|---------|----------|------------|
| Inscription organisation | P1 | ⭐⭐⭐ |
| Gestion plans (free/pro) | P1 | ⭐⭐⭐ |
| Facturation Stripe | P1 | ⭐⭐⭐ |
| Custom domains | P2 | ⭐⭐⭐ |
| White-label complet | P2 | ⭐⭐⭐ |
| Super Admin dashboard | P2 | ⭐⭐⭐ |
| API publique | P3 | ⭐⭐⭐⭐ |

---

## Modèle économique SaaS

### Plans tarifaires proposés

| Plan | Prix/mois | Listings | Features |
|------|-----------|----------|----------|
| **Starter** | Gratuit | 1 | Guest App basique |
| **Pro** | 29€ | 10 | + Admin + Services + Analytics |
| **Business** | 79€ | 30 | + Réservation directe + Support |
| **Enterprise** | 199€ | Illimité | + White-label + API + SLA |

### Projection revenus

Si OPT IM GROUP acquiert 50 clients payants en 1 an:
- 20 × Pro (29€) = 580€/mois
- 20 × Business (79€) = 1,580€/mois
- 10 × Enterprise (199€) = 1,990€/mois
- **Total: ~4,150€/mois = ~50,000€/an**

---

## Coûts de développement et maintenance

### Développement initial

| Composant | Temps | Coût (si externe) |
|-----------|-------|-------------------|
| Guest App MVP | 2-3 sem | ~3,000€ |
| Admin Dashboard | 2-3 sem | ~3,000€ |
| Backend + DB | 2 sem | ~2,000€ |
| Services & Paiement | 2 sem | ~2,000€ |
| Réservation directe | 4 sem | ~5,000€ |
| Multi-tenant | 4 sem | ~5,000€ |
| **Total** | **12-16 sem** | **~20,000€** |

*Note: Avec Claude Code, le développement peut être fait en interne à coût quasi-nul.*

### Coûts récurrents

| Service | Coût/mois |
|---------|-----------|
| Supabase Pro | 25€ |
| Vercel Pro | 20€ |
| Domaine(s) | ~2€ |
| Email (Resend) | 0-20€ |
| Stripe fees | 1.4% + 0.25€/tx |
| **Total** | **~50-70€/mois** |

---

## Roadmap

### Q3 2026 (Juillet - Septembre)
- [x] Prototype Guest App (Option A) ✅
- [ ] Option B avec Supabase
- [ ] Admin Dashboard basique
- [ ] Déploiement OPT IM GROUP interne

### Q4 2026 (Octobre - Décembre)
- [ ] Services et upselling
- [ ] Paiement Stripe
- [ ] Multi-langues complet (10 langues)
- [ ] Beta avec 2-3 conciergeries partenaires

### Q1 2027 (Janvier - Mars)
- [ ] Réservation directe
- [ ] Sync calendriers (iCal)
- [ ] Multi-tenant
- [ ] Lancement public SaaS

### Q2 2027 (Avril - Juin)
- [ ] Channel Manager
- [ ] White-label complet
- [ ] API publique
- [ ] Marketing & acquisition

---

## Avantages compétitifs

### vs Duve
- ✅ Prix plus bas (pas de coût par listing)
- ✅ Support francophone natif
- ✅ Adapté marché français (taxe séjour, etc.)
- ✅ Personnalisation totale

### vs Chekin
- ✅ Guest App plus riche
- ✅ Réservation directe intégrée
- ✅ Upselling plus poussé

### vs développement from scratch
- ✅ Basé sur templates éprouvés
- ✅ Design system prêt (Jack HTML Generator)
- ✅ Stack moderne et maintenable

---

## Risques et mitigations

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Temps de développement | Moyen | Utiliser Claude Code, approche MVP |
| Concurrence (Duve, etc.) | Élevé | Focus niche (conciergeries FR) |
| Acquisition clients | Moyen | Réseau SuperHote, coaching |
| Maintenance technique | Faible | Stack simple, Supabase managed |

---

## Prochaines étapes immédiates

1. **Valider l'Option B** (Supabase) comme base technique
2. **Créer le projet Supabase** et la structure DB
3. **Développer l'Admin Dashboard** minimal
4. **Migrer les données** du JSON vers Supabase
5. **Déployer sur un domaine** (ex: guest.optimigroup.fr)
6. **Tester avec 3 appartements** OPT IM GROUP
7. **Itérer** selon les retours

---

## Conclusion

L'Option D représente un investissement initial (~2-3 mois de développement) mais offre:
- **Économies** de ~5,000€/an vs SaaS externe
- **Revenus potentiels** de 50,000€+/an en vendant à d'autres
- **Différenciation** sur le marché
- **Autonomie** totale

C'est la stratégie recommandée pour une conciergerie qui veut être **novatrice et indépendante**.
