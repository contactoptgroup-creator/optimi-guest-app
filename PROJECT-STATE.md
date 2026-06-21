# OPT IM GROUP - Guest App Project

**Dernière mise à jour:** 2026-06-21 23:30
**Statut:** MVP Option A terminé - Prêt pour déploiement

---

## 📋 Vue d'ensemble du projet

**Objectif:** Créer une Guest App (livret d'accueil digital) pour toute la flotte d'appartements en conciergerie OPT IM GROUP, inspirée de Tastycloud Room Directory.

**Fonctionnalités principales:**
- Accès QR code par appartement
- WiFi (copie en 1 clic)
- Instructions check-in/checkout
- Services additionnels (early/late, ménage, transfert)
- Infos quartier
- Numéros d'urgence
- **Moteur de réservation intégré** (lien personnalisé par appart)
- Support multi-langues (FR/EN)
- PWA (Progressive Web App)

---

## 🗂️ Structure du projet

```
optimi-guest-app/
├── index.html              ✅ CRÉÉ - App principale (Option A)
├── PROJECT-STATE.md        ✅ CRÉÉ - Ce fichier
├── manifest.json           ⏳ À créer - PWA manifest
├── sw.js                   ⏳ À créer - Service Worker
├── css/
│   └── style.css           ⏳ À créer (optionnel, CSS inline actuellement)
├── js/
│   └── app.js              ⏳ À créer (optionnel, JS inline actuellement)
├── data/
│   └── apartments.json     ⏳ À créer - Config par appartement
├── assets/
│   └── images/             📁 Dossier créé
├── admin/
│   └── dashboard.html      ⏳ À créer - Dashboard admin
└── docs/
    ├── OPTION-A.md         ⏳ À créer - Documentation Option A
    ├── OPTION-B.md         ⏳ À créer - Documentation Option B (Supabase)
    ├── OPTION-C.md         ⏳ À créer - Benchmark SaaS
    └── OPTION-D-SAAS.md    ⏳ À créer - Architecture SaaS custom
```

---

## ✅ Progression

### Étape 1: Structure du projet ✅ TERMINÉ
- [x] Créer dossiers
- [x] Créer index.html (prototype complet)
- [x] Créer PROJECT-STATE.md (mémoire)

### Étape 2: Option A - Solution statique ✅ TERMINÉ
- [x] HTML principal avec design complet
- [x] Système de navigation
- [x] Modals pour chaque section (9 modals)
- [x] Support multi-langues (FR/EN)
- [x] Safe-area pour iPhone
- [x] manifest.json (PWA)
- [x] sw.js (Service Worker offline)
- [x] offline.html (page hors-ligne)
- [x] apartments.json (données par appart)
- [ ] QR codes par appartement
- [ ] Déploiement Vercel

### Étape 3: Option B - Supabase 📄 DOCUMENTÉ
- [x] Documentation complète (docs/OPTION-B-SUPABASE.md)
- [x] Schéma base de données SQL
- [x] Architecture détaillée
- [ ] Implémentation (à faire)

### Étape 4: Option C - Benchmark SaaS ✅ TERMINÉ
- [x] Analyser Duve
- [x] Analyser Chekin
- [x] Analyser Hostaway
- [x] Analyser Guesty
- [x] Analyser Your Porter
- [x] Analyser Tastycloud
- [x] Tableau comparatif (docs/OPTION-C-BENCHMARK-SAAS.md)

### Étape 5: Option D - SaaS OPT IM GROUP 📄 DOCUMENTÉ
- [x] Architecture technique complète
- [x] Multi-tenant design
- [x] Roadmap détaillée
- [x] Modèle économique
- [x] Documentation (docs/OPTION-D-SAAS-CUSTOM.md)
- [ ] Implémentation (à faire)

### Étape 6: Moteur de réservation 🔗 INTÉGRÉ
- [x] Lien réservation externe (bookingUrl par appart)
- [x] Bouton CTA "Réserver maintenant"
- [ ] Widget de réservation directe (Phase 4 SaaS)
- [ ] Calendrier disponibilités (Phase 4 SaaS)

### Étape 7: Déploiement ⏳ EN COURS
- [ ] GitHub repo
- [ ] Vercel deployment
- [ ] Domaine custom
- [ ] QR codes générés

---

## 🔧 Configuration actuelle (Option A - index.html)

### Données d'exemple (à remplacer)
```javascript
const apartmentData = {
    id: 'suffren-001',
    name: 'Appartement Tour Eiffel',
    address: '21 Avenue de Suffren, Paris 7e',
    wifi: {
        name: 'OPTIMI_Suffren',
        password: 'Welcome2024!'
    },
    bookingUrl: 'https://airbnb.com/rooms/xxxxx',
    // ... autres données
};
```

### Comment ajouter un appartement
1. Copier `index.html`
2. Modifier `apartmentData` avec les infos du bien
3. Générer QR code vers l'URL
4. Déployer sur Vercel

---

## 📊 Options de développement

| Option | Description | Complexité | Coût | Temps |
|--------|-------------|------------|------|-------|
| **A** | HTML statique (1 fichier/appart) | ⭐ | Gratuit | 1-2j |
| **B** | Supabase + Admin Dashboard | ⭐⭐⭐ | Gratuit (free tier) | 1 sem |
| **C** | SaaS externe (Duve, Chekin...) | ⭐ | 30-100€/mois | 1j |
| **D** | SaaS custom OPT IM GROUP | ⭐⭐⭐⭐⭐ | Dev + hosting | 1-2 mois |

---

## 🔗 Liens de référence

- **Inspiration:** https://mercure-lyon-centre-beaux-arts.tastycloud.menu/
- **Tastycloud docs:** https://www.tastycloud.fr/solution-room-directory-hotel/
- **Design system:** Jack HTML Generator palette (dark mode)
- **Icônes:** Emojis natifs

---

## 📝 Notes importantes

1. **Moteur de réservation:** Chaque appart aura son propre `bookingUrl` (Airbnb, Booking, ou direct)
2. **QR Codes:** Format URL: `https://guest.optimigroup.fr/?apt=suffren-001`
3. **Multi-langues:** Détection auto + switch manuel (FR/EN implémenté, extensible)
4. **PWA:** Installable sur l'écran d'accueil du téléphone

---

## 🚀 Pour reprendre le travail

Si la session s'arrête, voici comment reprendre:

1. Lire ce fichier `PROJECT-STATE.md`
2. Vérifier les ✅ et ⏳ dans la progression
3. Continuer avec les tâches marquées ⏳

**Commande pour voir l'état:**
```bash
cd "C:\Users\taouo\OneDrive\Bureau\2 - Airbnb Coaching Program\OPT IM GROUP ACADEMY\Apparts Conciergerie OPT IM Group\optimi-guest-app"
cat PROJECT-STATE.md
```

---

## 📞 Contact projet

- **Owner:** Jean-Marc Taouo
- **Email:** contactoptgroup@gmail.com
- **WhatsApp:** +33 6 12 78 49 32
