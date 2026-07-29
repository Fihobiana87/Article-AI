# Flux Pages Web — Version Définitive V1

> **Validé le** : 28 mars 2026
> **Statut** : VALIDÉ — Prêt pour transmission lundi
> **Scope** : Pages structurelles uniquement (blog = flux séparé à définir)

---

## Vue d'ensemble

```
ÉTAPE 1                    ÉTAPE 2                      ÉTAPE 3                    ÉTAPE 4
Intelligence Client        Stratégie Mots-Clés          Rédaction Pages            Audit Cohérence
(intelligence-client)      (keyword-strategy)           (redaction-pages)          (à construire)
        │                          │                            │                         │
   Scan sources              Seeds + Expansion           Validation inputs          Session fraîche
        │                    + Analyse concurrence              │                         │
   Pré-remplir HTML          50K+ mots-clés              Mini-briefs/page          Vérification
        │                          │                            │                    croisée
   ⏸ PAUSE 1                 CSV → GKP                   ⏸ PAUSE 4                     │
   Client valide                   │                     Validation briefs          Rapport OK
        │                    ⏸ PAUSE 2                         │                    ou corrections
   Export JSON               Opérateur import/export      Rédaction                      │
        │                          │                            │                   2 passes :
   Fiche Entreprise          Scoring + P1/P2/P3          DOCX V10.2                pré-client
   + Structure Site          + Anti-cannibalisation       (7 parties)              post-corrections
     Initiale                      │                            │
                             HTML interactif              Brief continuité
                                   │                     si multi-sessions
                             ⏸ PAUSE 3
                             Client valide
                                   │
                             Stratégie Mots-Clés
                             + Structure Site Finale
```

**4 étapes + 4 pauses humaines — 6 skills (3 pipeline + 3 support)**

---

## Skills du pipeline

| # | Skill | Rôle |
|---|-------|------|
| 1 | `intelligence-client` | Collecte données client, formulaire interactif, Fiche Entreprise, Structure Site |
| 2 | `keyword-strategy` | Expansion mots-clés, GKP, scoring, stratégie P1/P2/P3 |
| 3 | `redaction-pages` | Rédaction contenu pages, SEO, Answer Capsule, JSON-LD, DOCX V10.2 |
| 4 | `audit-coherence` *(à construire)* | Audit cohérence — vérification croisée en contexte frais |

## Skills support (génériques)

| Skill | Rôle |
|-------|------|
| `writing-standards` (ex `nuklear-writing`) | Socle qualité : blacklist IA, humanisation, styles de tons, frameworks rédactionnels (PASTOR, AIDA, PAS, StoryBrand...), véracité, légalité |
| `web-optimization` (ex `nuklear-web`) | 32 disciplines SEO/GEO/AEO/CRO/E-E-A-T |
| `nuklear-brand` | Charte NUKLEAR (light theme, logo SVG fixe) — livrables NUKLEAR uniquement |

---

## ÉTAPE 1 — Intelligence Client

**Skill** : `intelligence-client`

**Objectif** : Scanner toutes les sources du client, pré-remplir un formulaire HTML standardisé, récupérer la validation client, et en extraire les documents de production.

### Inputs

- URL du site existant (optionnel mais très utile)
- Documents client : PDF, DOCX, plaquettes, briefs, charte graphique (optionnel)
- Nom du client (minimum requis)

### Ce que Claude fait

1. **Scrape le site** via Chrome MCP (pages clés : accueil, à propos, services, contact, réalisations, blog)
2. **Exécute le script JS** d'extraction technique sur la page d'accueil (technos/CMS, couleurs hex exactes via computed styles, polices, logos/favicons, meta tags complets)
3. **Lit tous les documents** fournis (Read tool pour PDF/DOCX/MD)
4. **Pré-remplit le Formulaire HTML** standardisé

### Formulaire HTML interactif

Le formulaire couvre 57+ questions organisées par section :

- Identité & Positionnement (nom, forme juridique, secteur, positionnement, USP)
- Services & Offres (détail par service, technologies, méthodologies)
- Cibles & Marché (personas, pain points, vocabulaire client, zones géographiques)
- Preuves & Crédibilité (chiffres, témoignages, réalisations, certifications)
- Équipe & Expertise
- Histoire & Culture (genèse, mission, vision, valeurs)
- Ton de Voix (DO / DON'T, signatures verbales, pronom)
- Présence Digitale & Contact
- Projet Web (pages souhaitées, mots-clés importants, concurrents, objectifs)
- Charte Graphique (couleurs, polices, logo, style graphique)
- **Mots/expressions interdits** (termes à ne jamais utiliser)
- **Sujets à éviter** (messages interdits, sujets sensibles)

**Fonctionnalités du HTML** :
- Autonome (un seul fichier, zéro dépendance externe)
- Branding NUKLEAR light mode (fond blanc #FFFFFF, accent rouge #E23013, police Inter)
- Champs pré-remplis avec badge "pré-rempli" (données issues de l'analyse)
- Compteur de progression (questions remplies / restantes)
- **Sauvegarde locale** (enregistre l'état dans le navigateur pour reprendre plus tard)
- **Export JSON** (télécharge les réponses validées)
- **Import JSON** (recharge un fichier sauvegardé précédemment)
- Responsive (utilisable sur mobile)

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Formulaire.html` | HTML interactif | Interface client — outil de collecte et validation |

### ⏸ PAUSE 1 — Validation client

Le client remplit/valide le formulaire dans son navigateur. Il peut sauvegarder localement et reprendre plus tard. Quand c'est prêt, il exporte le JSON et le renvoie à l'opérateur.

### Au retour du JSON validé

Claude importe le JSON et génère les documents de production :

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Formulaire_Valide.json` | JSON | Source de vérité — export du client |
| `[CLIENT]_Fiche_Entreprise.md` | Markdown | Document de production : 10 sections (identité, services, cibles, preuves, équipe, histoire, ton, digital, contraintes, lacunes) |
| `[CLIENT]_Structure_Site_Initiale.md` | Markdown | Document de production : pages, arborescence, URLs provisoires, P1 provisoires, maillage |

**Règle** : Le JSON client fait foi. Si Claude avait extrait une info différente lors du scan, c'est la version validée par le client qui prime. Tous les documents sont **définitifs** à ce stade.

### Stockage

```
./clients/[CLIENT]/01_Fondations/
├── [CLIENT]_Formulaire.html
├── [CLIENT]_Formulaire_Valide.json
├── [CLIENT]_Fiche_Entreprise.md
└── [CLIENT]_Structure_Site_Initiale.md
```

---

## ÉTAPE 2 — Stratégie de Mots-Clés

**Skill** : `keyword-strategy`

**Pré-requis** : Lire `writing-standards` (socle d'écriture générique).

**Objectif** : Construire une vision exhaustive du marché de mots-clés, la faire valider, et en extraire les documents de production pour la rédaction.

### Inputs

- Fiche Entreprise (`.md`, depuis Étape 1)
- Structure du Site Initiale (`.md`, depuis Étape 1)

### Ce que Claude fait

**Phase 1 — Extraction seeds** : Extraction systématique depuis la Fiche Entreprise (services, cibles, problèmes clients, technologies, certifications, mots-clés importants du client) et la Structure du Site (P1 provisoires, clusters thématiques).

**Phase 1b — Analyse concurrence** : Scraping via Chrome des meta titles, H1, meta descriptions des sites concurrents identifiés dans la Fiche Entreprise. Enrichissement des seeds.

**Phase 2 — Expansion massive** : WebSearch + génération combinatoire Python (sets in-memory, pas de JSON intermédiaire).

14+ angles obligatoires :
1. Transactionnel (prix, devis, tarif)
2. Comparatif (vs, meilleur, avis)
3. Questions PAA (comment, pourquoi, qu'est-ce que)
4. Longue traîne (service + cible + géo + techno)
5. Problème-solution (douleur client sans mentionner la solution)
6. Concurrence (termes concurrents, alternatives)
7. Tendances (2026, IA, termes émergents)
8. White label / B2B (sous-traitance, outsource)
9. Prix/tarifs (TJM, budget, ROI)
10. Géographique (villes × services × patterns)
11. Guides/Tutoriels (étapes, conseils, checklist)
12. Erreurs/Pièges (erreurs à éviter)
13. PAA Google (questions réelles)
14. GEO/IA (optimisation ChatGPT, Perplexity, AI overview)

**Objectif minimum : 50 000 mots-clés candidats, même pour un petit site. Viser 100 000+.** Maximiser toujours — l'objectif est la vision globale la plus exhaustive possible. Le minimum est un seuil de qualité (en dessous, l'expansion n'a pas été assez poussée), pas une cible. Ne jamais brider l'expansion pour rester à un chiffre rond.

**Phase 3 — Génération CSV pour Google Keyword Planner.**

Règles techniques GKP :
- Format CSV UTF-8, header `Keyword` en première ligne
- Max 9 999 mots-clés par fichier (= 1 import GKP)
- Max 10 mots par mot-clé (contrainte technique GKP — au-delà, GKP rejette silencieusement)
- Caractères interdits GKP : `! @ % , *` — les supprimer systématiquement
- Séparer SITE vs BLOG et FR vs EN (marchés séparés dans GKP)
- Nommage : `[CLIENT]_GKP_[SITE|BLOG]_[FR|EN]_Lot[N].csv`

### Output intermédiaire

| Fichier | Format | Règles |
|---------|--------|--------|
| `[CLIENT]_GKP_SITE_[FR\|EN]_Lot[N].csv` | CSV UTF-8 | Pages structurelles, par langue, par lot |
| `[CLIENT]_GKP_BLOG_[FR\|EN]_Lot[N].csv` | CSV UTF-8 | Articles blog, par langue, par lot |

### ⏸ PAUSE 2 — Import/Export Google Keyword Planner

L'opérateur :
1. Va sur Google Keyword Planner (ads.google.com)
2. Clique "Obtenir le volume de recherche et les prévisions"
3. Importe chaque fichier CSV un par un
4. Exporte les résultats en CSV pour chaque lot
5. Renvoie tous les CSV exportés à Claude

### Au retour des données GKP

**Phase 4 — Scoring multicritère** :

| Critère | Poids |
|---------|-------|
| Volume de recherche | 25% |
| Pertinence business | 25% |
| Intention commerciale | 20% |
| Faisabilité / Concurrence | 15% |
| CPC indicatif | 10% |
| CTR potentiel | 5% |

**Phase 5 — Assignation P1/P2/P3 avec vérification anti-cannibalisation intégrée** :

- P1 : 1 seul par page, jamais dupliqué
- P2 : 2-5 par page, variantes sémantiques du P1
- P3 : 3-10 par page, longue traîne et questions
- Vérification anti-cannibalisation PENDANT l'assignation (pas en phase séparée)
- Hiérarchie de priorité : Pages prioritaires (accueil, services principaux) > Pages secondaires (à propos, contact, portfolio) > Pages tertiaires > Blog

Production du HTML interactif.

### HTML Stratégie de Mots-Clés

Contient TOUT dans un seul document interactif :

- **Par page** : P1/P2/P3 avec métriques (volume, CPC, concurrence), termes à citer (ex : "JavaScript", "React"), entités nommées, termes techniques, enrichissements sémantiques
- **Structure du Site Finale** : pages, URLs définitives, arborescence, maillage interne
- **Choix stratégiques** : arbitrages effectués, opportunités identifiées
- **H1 proposés, meta titles, meta descriptions** pour chaque page

**Fonctionnalités du HTML** :
- Autonome, branding NUKLEAR light mode
- Visualisation interactive par page
- Le client/opérateur peut valider, modifier, corriger
- **Sauvegarde locale** (reprendre plus tard)
- **Export JSON/CSV** (transmettre les données validées)
- **Import JSON/CSV** (recharger un fichier sauvegardé)

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Strategie_Mots_Cles.html` | HTML interactif | Interface client/opérateur — validation de la stratégie |

### ⏸ PAUSE 3 — Validation stratégie avant rédaction

**Go/No-Go critique.** Le client/opérateur valide dans le HTML :
- P1/P2/P3 de chaque page
- Structure du Site Finale (pages, arborescence)
- URLs définitives
- H1 proposés
- Meta titles et meta descriptions

Quand c'est validé, export JSON/CSV.

### Au retour de l'export validé

Claude génère les documents de production :

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Strategie_Mots_Cles.md` | Markdown | Document de production enrichi (dérivé de l'export validé) — utilisé par Claude pour la rédaction |
| `[CLIENT]_Structure_Site_Finale.md` | Markdown | Document de production (dérivé de l'export validé) — P1 définitifs, URLs définitives |

### Stockage

```
./clients/[CLIENT]/01_Fondations/
└── [CLIENT]_Structure_Site_Finale.md          ← ajouté ici

./clients/[CLIENT]/02_Mots-Cles/
├── [CLIENT]_Strategie_Mots_Cles.html
├── [CLIENT]_Strategie_Mots_Cles.md
├── GKP-Import/
│   └── [CLIENT]_GKP_[SITE|BLOG]_[FR|EN]_Lot[N].csv
└── GKP-Export/
    └── (CSV retour Google Keyword Planner)
```

---

## ÉTAPE 3 — Rédaction Pages

**Skill** : `redaction-pages`

**Pré-requis** : Lire `writing-standards` (socle) + `web-optimization` (32 disciplines).

**Objectif** : Rédiger le contenu complet de chaque page structurelle et livrer en DOCX V10.2.

### Inputs

- Fiche Entreprise (`.md`)
- Structure du Site Finale (`.md`)
- Stratégie de Mots-Clés (`.md`)
- Si besoin de plus de détails sur un terme → consulter les CSV GKP-Export

### Ce que Claude fait

**Phase 1 — Validation cohérence** :
- Chaque page du Structure Site a un P1 dans la Stratégie ?
- Les intents correspondent ?
- Les services Fiche Entreprise couvrent toutes les pages services ?
- **Signale les incohérences AVANT de rédiger** et demande validation à l'opérateur.

**Phase 2 — Mini-briefs par page** :

Pour chaque page, Claude produit un brief contenant :
- Nom, URL, type de page
- Framework recommandé
- P1 (mot-clé, volume, placements)
- P2 et P3 (avec placements)
- Questions GEO/AEO cibles
- Maillage (liens vers quelles pages)
- Preuves à utiliser (depuis Fiche Entreprise)
- Persona ciblé
- CTA principal
- Longueur cible

**Frameworks disponibles** :

| Framework | Usage | Longueur cible |
|-----------|-------|---------------|
| **Hub** | Page d'accueil — positionnement + distribution trafic | 800-1 200 mots |
| **PASTOR** (invisible) | Pages services — problème→solution→offre→CTA | 1 200-1 800 mots |
| **Narrative E-E-A-T** | Page à propos — prouver Experience, Expertise, Authority, Trust | 800-1 200 mots |
| **Utilitaire** | Page contact — court, direct, fonctionnel | 200-400 mots |
| **Case Study** | Portfolio/réalisations | 600-1 000 mots |
| **AIDA** | Alternative landing — Attention, Interest, Desire, Action | 1 000-1 500 mots |
| **PAS** | Alternative problème-solution — Problem, Agitate, Solve | 1 000-1 500 mots |
| **StoryBrand** | Alternative centrée parcours client comme héros | 1 000-1 500 mots |
| **FAQ-first** | Pages à fort potentiel GEO/AEO | 800-1 200 mots |

Si l'opérateur ou le client n'est pas satisfait du framework recommandé, Claude propose les alternatives avec avantages/inconvénients pour le cas spécifique.

### ⏸ PAUSE 4 — Validation des mini-briefs

L'opérateur valide ou ajuste les briefs (framework, longueur, preuves, CTA) avant que Claude rédige.

### Rédaction

**Phase 3 — Rédaction page par page** selon le framework validé, en respectant :
- 32 disciplines d'optimisation (`web-optimization`)
- Ton et DO/DON'T de la Fiche Entreprise
- Blacklist IA (`writing-standards`)
- Maillage interne de la Structure du Site Finale
- Mots/expressions interdits du client
- P1 en densité 1-2%, P2 dans les H2, P3 dans le corps
- Réponse directe à l'intent dans les 100 premiers mots (pour GEO/AEO)
- Au moins 1 CTA actionnable par page
- Rythme varié, PASTOR invisible, zéro remplissage

**Phase 4 — Éléments techniques** :
- Fiche SEO (meta title ≤60 car. avec P1, meta description ≤155 car., H1, densité)
- Answer Capsule AEO/GEO (~150 mots, JSON-LD WebPage + Speakable — invisible sur le site)
- Schemas JSON-LD complets (Organization, Service, FAQPage, BreadcrumbList, etc.)

**Phase 5 — Vérification croisée** :
- Anti-cannibalisation (P1 uniques, H1 distincts, meta titles distincts)
- Cohérence contenu ↔ stratégie
- Checklist 32 disciplines
- Qualité rédactionnelle (ton, pronom, vocabulaire persona, zéro remplissage)
- Livraison

### Output — DOCX V10.2 (7 parties)

1 fichier DOCX par page, structuré en 7 parties :

| Partie | Contenu | Destinataire |
|--------|---------|--------------|
| P1 | Fiche technique SEO (mots-clés P1/P2/P3, meta, structure Hn, densité, anti-cannibalisation) | Équipe technique |
| P2 | Answer Capsule AEO/GEO (JSON-LD WebPage + Speakable) — ⚠ texte invisible sur le site, injecté dans le `<head>` | Équipe technique |
| P3 | Contenu rédigé (texte visible, section par section, CTAs et liens notés séparément) | Client + équipe |
| P4 | FAQ (questions/réponses exploitant P3 et questions GEO) | Client + équipe |
| P5 | Maillage interne (tableau : texte d'ancrage → URL cible → section) | Équipe technique |
| P6 | Données structurées JSON-LD complètes (Organization, Service, FAQPage, BreadcrumbList, etc.) | Équipe technique |
| P7 | Rapport technique (compteurs de validation, KPIs SEO, checklist 32 disciplines) | Équipe technique |

**Nommage** : `[CLIENT]_[NomPage]_[Lang]_V[X].docx`

### Gestion multi-sessions

Claude surveille activement sa charge de contexte. Au-delà de 3 pages rédigées ou dès qu'il détecte une baisse de qualité (réponses plus courtes, formulations répétitives), il propose **proactivement** de :
1. Sauvegarder tout le travail fait
2. Générer un **brief de continuité** (`.md`)
3. Continuer dans une nouvelle session

Le brief de continuité contient : client, pages faites (avec statut), pages restantes (avec priorité), chemins des documents de référence, ton calibré, corrections appliquées. Il est copié-collé dans la nouvelle conversation pour reprendre sans perte d'information.

### Stockage

```
./clients/[CLIENT]/03_Contenu_Pages/
└── [CLIENT]_[NomPage]_[Lang]_V[X].docx
```

---

## ÉTAPE 4 — Audit Cohérence

**Skill** : `audit-coherence` *(à construire)*

**Toujours lancé dans une NOUVELLE session** pour un contexte frais, sans biais de rédaction.

### Inputs

- Tous les DOCX V10.2 produits en Étape 3
- Stratégie de Mots-Clés (`.md`)
- Structure du Site Finale (`.md`)

### Vérifications

- P1 bien placés dans H1, meta title et intro de chaque page
- Aucun P1 dupliqué entre pages
- H1 tous distincts
- Meta titles tous distincts
- Intents différenciés par page
- 32 disciplines appliquées
- Maillage interne cohérent avec Structure du Site
- Densité de mots-clés correcte
- Ton conforme à la Fiche Entreprise
- Zéro terme de la blacklist IA
- Mots/expressions interdits du client respectés

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Audit_Coherence_[Date].md` | Markdown | Rapport : OK ou corrections nécessaires (avec détail par page) |

### Usage

Tourne **2 fois** :
1. **Pré-client** : avant de présenter les contenus au client
2. **Post-corrections** : après intégration des modifications demandées par le client

### Stockage

```
./clients/[CLIENT]/05_Audits/
└── [CLIENT]_Audit_Coherence_[Date].md
```

---

## BOUCLE DE VALIDATION — Post-production

**Principe** : les validations sont des BOUCLES. On ne passe à l'étape suivante (publication) que quand le client ET l'audit valident conjointement. Il n'y a pas de limite au nombre d'itérations.

```
Étape 3 (Rédaction) terminée
        │
        ▼
┌─── BOUCLE DE VALIDATION ───┐
│                              │
│  Audit Cohérence pré-client  │
│  (session fraîche)           │
│        │                     │
│   OK ? ──Non──► Corrections  │
│        │        puis retour  │
│       Oui       audit        │
│        │                     │
│  Présentation client         │
│        │                     │
│  Client OK ? ──Non──►        │
│        │     Corrections     │
│       Oui    puis retour     │
│        │     audit           │
│        ▼                     │
│   PUBLICATION                │
└──────────────────────────────┘
```

**Détail du cycle** :

1. **Audit pré-client** : audit cohérence en session fraîche (Étape 4). Si problèmes → corrections → re-audit.
2. **Validation client** : présentation des contenus au client. Si corrections demandées → Claude intègre → retour audit cohérence (session fraîche) → si OK → retour client pour re-validation.
3. **Publication** : uniquement quand audit OK + client OK.

**Règle** : chaque correction significative repasse par l'audit cohérence en session fraîche. Les corrections mineures (typos, reformulations légères) peuvent être validées directement par le client sans re-audit.

---

## Formats de fichiers figés

| Document | Format | Extension | Rôle |
|----------|--------|-----------|------|
| Formulaire client | HTML interactif (sauvegarde locale + export/import JSON) | `.html` | Interface client Étape 1 |
| Formulaire validé | JSON (export du client) | `.json` | Source de vérité Étape 1 |
| Fiche Entreprise | Markdown | `.md` | Document de production (dérivé du JSON validé) |
| Structure du Site Initiale | Markdown | `.md` | Document de production (dérivé du JSON validé) |
| CSV GKP Import | CSV UTF-8, header `Keyword`, max 9 999/fichier | `.csv` | Import Google Keyword Planner |
| CSV GKP Export | CSV (retour Google) | `.csv` | Données volumes/CPC/concurrence |
| Stratégie Mots-Clés | HTML interactif (sauvegarde locale + export/import JSON-CSV) | `.html` | Interface client/opérateur Étape 2 |
| Stratégie Mots-Clés | Markdown | `.md` | Document de production (dérivé de l'export validé) |
| Structure du Site Finale | Markdown | `.md` | Document de production (dérivé de l'export validé) |
| Contenu pages | Word DOCX V10.2 (7 parties) | `.docx` | Livrable final |
| Audit cohérence | Markdown | `.md` | Rapport qualité |
| Brief de continuité | Markdown | `.md` | Gestion sessions Claude |

---

## Arborescence de stockage

```
./clients/[CLIENT]/
├── 01_Fondations/
│   ├── [CLIENT]_Formulaire.html
│   ├── [CLIENT]_Formulaire_Valide.json
│   ├── [CLIENT]_Fiche_Entreprise.md
│   ├── [CLIENT]_Structure_Site_Initiale.md
│   └── [CLIENT]_Structure_Site_Finale.md
├── 02_Mots-Cles/
│   ├── [CLIENT]_Strategie_Mots_Cles.html
│   ├── [CLIENT]_Strategie_Mots_Cles.md
│   ├── GKP-Import/
│   │   └── [CLIENT]_GKP_SITE_[FR|EN]_Lot[N].csv
│   └── GKP-Export/
│       └── (CSV retour Google Keyword Planner)
├── 03_Contenu_Pages/
│   └── [CLIENT]_[NomPage]_[Lang]_V[X].docx
├── 04_Blog/                              ← Voir FLUX_BLOG_V1.md pour le détail
│   └── [Cluster-Name]/
│       ├── [CLIENT]_Plan_Cluster_[Nom].md
│       ├── [CLIENT]_Strategie_MC_Blog_[Cluster].html
│       ├── [CLIENT]_Strategie_MC_Blog_[Cluster].md
│       ├── [CLIENT]_[Cluster]_Briefs.md
│       ├── GKP-Import/
│       │   └── [CLIENT]_GKP_BLOG_[Cluster]_[FR|EN]_Lot[N].csv
│       ├── GKP-Export/
│       │   └── (CSV retour Google)
│       └── [Article-Name]/
│           ├── [CLIENT]_[Cluster]_Art[N]_Blog.md
│           ├── [CLIENT]_[Cluster]_Art[N]_Blog.pdf
│           └── [CLIENT]_[Cluster]_Art[N]_Strapi.md
├── 05_Audits/
│   └── [CLIENT]_Audit_Coherence_[Date].md
└── _Claude/
    └── [CLIENT]_Brief_Continuite.md
```

---

## Prochaines étapes

1. ~~Valider le flux pages web~~ ✅
2. ~~Définir le flux blog~~ ✅ → Voir `FLUX_BLOG_V1.md`
3. Nettoyer et restructurer les fichiers existants
4. Mettre à jour les skills (`intelligence-client`, `keyword-strategy`, `redaction-pages`)
5. Renommer les skills support (`writing-standards`, `web-optimization`)
6. Préparer le dossier NUKLEAR (réalisations à date)
7. Créer la présentation pour lundi
8. Construire le Skill 4 (audit cohérence)
9. Construire les skills blog (`redaction-blog`, `audit-coherence-blog`)
