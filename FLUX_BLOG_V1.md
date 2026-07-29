# Flux Blog — Version Définitive V1

> **Validé le** : 28 mars 2026
> **Statut** : VALIDÉ — Prêt pour transmission lundi
> **Scope** : Articles de blog organisés en Topic Clusters
> **Pré-requis** : Flux Pages Web terminé (idéal) OU Fiche Entreprise + Structure Site disponibles

---

## Vue d'ensemble

```
PRÉ-REQUIS                 ÉTAPE B1                      ÉTAPE B2                    ÉTAPE B3                    ÉTAPE B4
(si pas de pages)           Préparation Cluster           Stratégie MC Cluster        Rédaction Articles          Audit Cohérence
                            (keyword-strategy adapté)     (keyword-strategy adapté)   (redaction-blog)            (audit-coherence-blog)
                                    │                             │                           │                          │
Pages publiées ?            Input : page pilier           Scoring multicritère        Validation inputs           Session fraîche
   │                        (scraping ou sujet fourni)    + P1/P2/P3 par article            │                          │
  Non──► intelligence-      + données GKP existantes      + Anti-cannibalisation      Mini-briefs/article         Vérification
         client                     │                             │                          │                     croisée cluster
         + scraping Chrome   Expansion mots-clés blog     HTML interactif             ⏸ PAUSE B4                       │
         (structure site)    10K+ par cluster                    │                    Validation briefs           Rapport OK
   │                                │                     ⏸ PAUSE B3                        │                    ou corrections
  Oui──► Continuer          CSV → GKP                    Client/opérateur valide     Rédaction                        │
                                    │                            │                          │                    Boucle si
                            ⏸ PAUSE B1                   Stratégie MC Blog           PDF + MD + Strapi          corrections
                            Opérateur import/export       (MD de production)          (par article)              nécessaires
                                    │                                                       │
                            Propositions articles                                    Auto-check (niveau 1)
                            (priorisées)                                                    │
                                    │                                                ⏸ PAUSE B5
                            ⏸ PAUSE B2                                              Validation client
                            Validation plan articles                                        │
                                                                                     Boucle corrections
                                                                                     → audit → validation
                                                                                            │
                                                                                      PUBLICATION
```

**4 étapes + 5 pauses humaines — Skills : `redaction-blog` (pipeline) + `audit-coherence-blog` + `writing-standards` + `web-optimization` (support)**

---

## Pré-requis — Contexte client

Le flux blog a besoin de contexte client pour rédiger des articles de qualité (ton, preuves, maillage interne). Deux scénarios :

### Scénario A — Pages publiées (idéal)

Les documents suivants existent déjà (produits par le flux pages web) :

- Fiche Entreprise (`.md`)
- Structure du Site Finale (`.md`) — avec les pages publiées et leurs URLs définitives
- Stratégie de Mots-Clés (`.md`) — avec les P1 des pages pour l'anti-cannibalisation

→ On passe directement à l'Étape B1.

### Scénario B — Pas de pages produites

Il faut reconstituer le contexte avant de commencer :

1. **Lancer `intelligence-client`** pour générer la Fiche Entreprise (formulaire → validation client → JSON → documents de production)
2. **Scraper le site existant** via Chrome MCP pour obtenir la Structure du Site (pages, URLs, arborescence, maillage existant)
3. Si des données GKP existent déjà (import/export précédents), les récupérer

→ Ces documents deviennent les inputs de l'Étape B1.

**Règle** : ne jamais démarrer la rédaction d'articles sans Fiche Entreprise ET Structure du Site. Le maillage interne et le ton dépendent de ces documents.

---

## ÉTAPE B1 — Préparation Cluster

**Objectif** : Identifier le champ sémantique d'un cluster rattaché à une page pilier, générer une vision exhaustive des mots-clés, et proposer un plan d'articles priorisé.

### Inputs

- **Option 1** : Page pilier existante (URL) → Claude scrappe via Chrome MCP (contenu, H1, H2, meta, mots-clés couverts)
- **Option 2** : Sujet, thème ou champ lexical fourni par l'opérateur (si la page n'existe pas encore)
- Données GKP existantes si disponibles (CSV retour du flux pages)
- Fiche Entreprise (`.md`)
- Structure du Site Finale (`.md`)
- Stratégie de Mots-Clés des pages (`.md`, si disponible — pour connaître les P1 pages et éviter la cannibalisation)

### Ce que Claude fait

**Phase 1 — Analyse de la page pilier** :
- Si URL fournie : scraping complet (contenu, P1, H2, meta, liens internes existants)
- Si sujet fourni : cadrage du champ sémantique (thème central, sous-thèmes, intent principal)
- Identification des angles informationnels couverts vs non couverts par la page pilier

**Phase 2 — Extraction seeds** :
- Depuis la page pilier : sous-thèmes, questions implicites, termes techniques
- Depuis la Fiche Entreprise : expertise, certifications, services liés, pain points clients
- Depuis les données GKP existantes : mots-clés informationnels déjà scorés (réutilisation)
- Depuis la concurrence : scraping Chrome des blogs concurrents sur le même sujet (titres d'articles, H1, meta)

**Phase 3 — Expansion massive** :
- WebSearch + génération combinatoire Python (sets in-memory)
- 14+ angles obligatoires (identiques au flux pages, adaptés à l'intent informationnel) :
  1. Questions PAA (comment, pourquoi, qu'est-ce que)
  2. Guides/Tutoriels (étapes, conseils, checklist, guide complet)
  3. Erreurs/Pièges (erreurs à éviter, problèmes courants)
  4. Comparatifs (X vs Y, meilleur, alternatives)
  5. Listes (top, meilleurs, outils, exemples)
  6. Définitions/Glossaire (c'est quoi, définition, signification)
  7. Tendances (2026, IA, futur, prédictions)
  8. Études de cas (exemple, retour d'expérience, succès)
  9. Débutant/Avancé (pour débutants, avancé, expert)
  10. Problème-solution (résoudre, corriger, débloquer)
  11. ROI/Chiffres (statistiques, benchmark, études)
  12. Intégrations/Outils (outils, logiciels, stack technique)
  13. GEO/IA (questions ChatGPT, Perplexity, AI overview)
  14. Longue traîne (combinaisons sujet × cible × contexte × techno)

**Objectif minimum : 10 000 mots-clés candidats par cluster, en plus des données GKP existantes. Viser 20-30K+.** Maximiser toujours — le minimum est un seuil de qualité (en dessous, l'expansion n'a pas été assez poussée), pas une cible. Ne jamais brider l'expansion.

**Phase 4 — Génération CSV pour Google Keyword Planner** :

Règles techniques GKP (identiques au flux pages) :
- Format CSV UTF-8, header `Keyword` en première ligne
- Max 9 999 mots-clés par fichier (= 1 import GKP)
- Max 10 mots par mot-clé (contrainte technique GKP)
- Caractères interdits GKP : `! @ % , *` — les supprimer systématiquement
- Nommage : `[CLIENT]_GKP_BLOG_[Cluster]_[FR|EN]_Lot[N].csv`
- Ne PAS inclure les mots-clés déjà scorés dans les données GKP existantes (éviter les doublons)

### Output intermédiaire

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_GKP_BLOG_[Cluster]_[FR\|EN]_Lot[N].csv` | CSV UTF-8 | Mots-clés blog à importer dans GKP |

### ⏸ PAUSE B1 — Import/Export Google Keyword Planner

L'opérateur :
1. Va sur Google Keyword Planner (ads.google.com)
2. Importe chaque fichier CSV un par un
3. Exporte les résultats en CSV pour chaque lot
4. Renvoie tous les CSV exportés à Claude

### Au retour des données GKP

**Phase 5 — Scoring multicritère** (mêmes poids que le flux pages) :

| Critère | Poids |
|---------|-------|
| Volume de recherche | 25% |
| Pertinence business | 25% |
| Intention informationnelle (≠ flux pages qui utilise "intention commerciale") | 20% |
| Faisabilité / Concurrence | 15% |
| CPC indicatif | 10% |
| CTR potentiel | 5% |

**Phase 6 — Propositions d'articles** :

Claude produit un plan du cluster avec :
- Liste d'articles proposés, classés du plus important au moins important
- Pour chaque article : P1 cible, intent (informationnel/navigationnel), sous-type (GUIDE, HOW-TO, LISTE, ANALYSE, COMPARATIF, ETUDE-CAS, GLOSSAIRE, ACTUALITE), angle éditorial, volume P1 estimé
- Anti-cannibalisation : vérification que chaque P1 blog est différent des P1 pages (intent informationnel vs commercial)
- Maillage prévu : lien vers la page pilier (≥2 mentions), liens inter-articles du cluster
- Priorité de production argumentée (volume × faisabilité × valeur business)

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Plan_Cluster_[Nom].md` | Markdown | Plan du cluster : articles priorisés avec P1, intent, angle, sous-type |

### ⏸ PAUSE B2 — Validation du plan d'articles

L'opérateur/client valide :
- Quels articles produire (peut en supprimer, en ajouter, en réordonner)
- Les P1 cibles de chaque article
- Les sous-types et angles proposés
- L'ordre de production

---

## ÉTAPE B2 — Stratégie de Mots-Clés Cluster

**Objectif** : Construire la stratégie de mots-clés détaillée pour chaque article validé du cluster, la faire valider, et en extraire les documents de production.

### Inputs

- Plan du Cluster validé (`.md`, depuis Étape B1)
- Données GKP scorées (depuis Étape B1)
- Fiche Entreprise (`.md`)
- Structure du Site Finale (`.md`)
- Stratégie de Mots-Clés des pages (`.md`, si disponible — pour l'anti-cannibalisation)

### Ce que Claude fait

**Phase 1 — Assignation P1/P2/P3 par article avec anti-cannibalisation intégrée** :

- P1 : 1 seul par article, jamais dupliqué (ni entre articles du cluster, ni avec les pages)
- P2 : 2-5 par article, variantes sémantiques du P1
- P3 : 3-10 par article, longue traîne et questions
- Vérification anti-cannibalisation PENDANT l'assignation :
  - Blog P1 ≠ Pages P1 (intent informationnel vs commercial)
  - Blog P1 ≠ Autre blog P1 (même cluster ou autre cluster)
  - Hiérarchie de priorité : Pages prioritaires (accueil, services principaux) > Pages secondaires (à propos, contact, portfolio) > Pages tertiaires > Blog existant > Nouveau blog

**Phase 2 — Enrichissement par article** :

Pour chaque article :
- Termes à citer obligatoirement (entités nommées, termes techniques, marques)
- Enrichissements sémantiques (synonymes, termes LSI)
- Questions PAA / Featured Snippet cibles avec format prédéfini (paragraphe, liste, tableau)
- H1 proposé, meta title, meta description
- Maillage : liens vers page pilier (≥2), liens inter-articles, liens vers pages services

**Phase 3 — Production du HTML interactif** :

### HTML Stratégie de Mots-Clés Blog

Contient TOUT dans un seul document interactif :

- **Par article** : P1/P2/P3 avec métriques (volume, CPC, concurrence), termes à citer, entités nommées, enrichissements sémantiques
- **Vue cluster** : anti-cannibalisation visualisée, maillage bidirectionnel (articles ↔ page pilier)
- **H1 proposés, meta titles, meta descriptions** pour chaque article
- **Choix stratégiques** : arbitrages effectués, opportunités identifiées

**Fonctionnalités du HTML** :
- Autonome, branding NUKLEAR light mode
- Visualisation interactive par article
- Le client/opérateur peut valider, modifier, corriger
- **Sauvegarde locale** (reprendre plus tard)
- **Export JSON/CSV** (transmettre les données validées)
- **Import JSON/CSV** (recharger un fichier sauvegardé)

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Strategie_MC_Blog_[Cluster].html` | HTML interactif | Interface client/opérateur — validation stratégie blog |

### ⏸ PAUSE B3 — Validation stratégie avant rédaction

**Go/No-Go critique.** Le client/opérateur valide dans le HTML :
- P1/P2/P3 de chaque article
- H1 proposés
- Meta titles et meta descriptions
- Maillage prévu
- Anti-cannibalisation OK

Quand c'est validé, export JSON/CSV.

### Au retour de l'export validé

Claude génère le document de production :

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Strategie_MC_Blog_[Cluster].md` | Markdown | Document de production enrichi — utilisé par Claude pour la rédaction |

### Stockage

```
./clients/[CLIENT]/04_Blog/[Cluster-Name]/
├── [CLIENT]_Plan_Cluster_[Nom].md
├── [CLIENT]_Strategie_MC_Blog_[Cluster].html
├── [CLIENT]_Strategie_MC_Blog_[Cluster].md
├── GKP-Import/
│   └── [CLIENT]_GKP_BLOG_[Cluster]_[FR|EN]_Lot[N].csv
└── GKP-Export/
    └── (CSV retour Google Keyword Planner)
```

---

## ÉTAPE B3 — Rédaction Articles

**Skill** : `redaction-blog`

**Pré-requis** : Lire `writing-standards` (socle) + `web-optimization` (32 disciplines).

**Objectif** : Rédiger chaque article du cluster selon le plan validé et la stratégie de mots-clés, livrer en PDF + MD + Strapi.

### Inputs

- Fiche Entreprise (`.md`)
- Structure du Site Finale (`.md`)
- Stratégie de Mots-Clés Blog du Cluster (`.md`)
- Plan du Cluster validé (`.md`)
- Si besoin de plus de détails sur un terme → consulter les CSV GKP-Export

### Ce que Claude fait

**Phase 1 — Validation cohérence** :
- Chaque article du plan a un P1 dans la stratégie ?
- Les intents sont bien informationnels (pas commerciaux — ça, c'est les pages) ?
- Le maillage vers la page pilier est cohérent avec la Structure du Site ?
- **Signale les incohérences AVANT de rédiger** et demande validation à l'opérateur.

**Phase 2 — Mini-briefs par article** :

Pour chaque article du cluster, Claude produit un brief contenant :
- Titre de l'article, slug proposé
- Sous-type (GUIDE, HOW-TO, LISTE, ANALYSE, COMPARATIF, ETUDE-CAS, GLOSSAIRE, ACTUALITE)
- P1 (mot-clé, volume, placements)
- P2 et P3 (avec placements)
- Questions GEO/AEO cibles + format Featured Snippet (paragraphe, liste, tableau)
- Maillage : liens vers page pilier (≥2), liens inter-articles, liens vers pages services
- Preuves à utiliser (depuis Fiche Entreprise)
- Persona ciblé
- CTA principal + CTA secondaire
- Longueur cible (selon sous-type)
- Angle éditorial et promesse de valeur
- Sources à citer

**Longueurs cibles par sous-type** :

| Sous-type | Longueur cible | Spécificités |
|-----------|---------------|--------------|
| **GUIDE** | 2 500 - 3 500 mots | Vue d'ensemble exhaustive, sections détaillées |
| **HOW-TO** | 1 500 - 2 500 mots | Pas-à-pas numéroté, prérequis, résultat attendu |
| **LISTE** | 1 500 - 2 500 mots | Items classés, critères de sélection, verdict |
| **ANALYSE** | 2 000 - 3 000 mots | Données, graphiques, méthodologie, conclusions |
| **COMPARATIF** | 2 000 - 3 000 mots | Critères pondérés, tableau comparatif, verdict argumenté |
| **ETUDE-CAS** | 1 500 - 2 500 mots | Contexte, défi, solution, résultats, leçons |
| **GLOSSAIRE** | 1 000 - 2 000 mots | Définitions structurées, liens croisés, exemples |
| **ACTUALITE** | 800 - 1 500 mots | Fait, analyse, impact, que faire, sources |

Tous les briefs sont rassemblés dans un seul document pour vision d'ensemble.

### Output briefs

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_[Cluster]_Briefs.md` | Markdown | Tous les mini-briefs du cluster — vision d'ensemble pour validation |

### ⏸ PAUSE B4 — Validation des mini-briefs

L'opérateur valide ou ajuste les briefs (sous-type, longueur, angle, preuves, CTA, maillage) avant que Claude rédige.

### Rédaction

**Phase 3 — Rédaction article par article** :

Le skill fonctionne en **2 phases internes** avec pause optionnelle :

**Phase 3a — Contenu (Parties 1-4)** :
- P1 : Fiche technique SEO (identité article, P1/P2/P3, anti-cannibalisation, meta, structure Hn)
- P2 : Answer Capsule AEO/GEO (50-70 mots — plus courte que les pages qui sont à ~150 mots, car l'article lui-même est la réponse détaillée) + formats Featured Snippet : paragraphe/liste/tableau
- P3 : Contenu rédigé (article complet selon sous-type et framework)
- P4 : FAQ (questions/réponses exploitant P3 et questions GEO)

*Pause optionnelle : l'opérateur peut valider le contenu avant de générer la partie technique. Par défaut, Claude enchaîne automatiquement.*

**Phase 3b — Technique (Parties 5-7)** :
- P5 : Maillage interne (≥2 liens vers page pilier + liens inter-articles du cluster + liens pages services)
- P6 : Données structurées JSON-LD Article (BlogPosting avec auteur E-E-A-T)
- P7 : KPIs & Tracking (métriques, événements GA4, benchmarks SEO)

**Règles de rédaction** (identiques au flux pages, adaptées au blog) :
- 32 disciplines d'optimisation (`web-optimization`)
- Ton et DO/DON'T de la Fiche Entreprise
- Blacklist IA (`writing-standards`)
- Mots/expressions interdits du client
- P1 en densité 1-2%, P2 dans les H2, P3 dans le corps
- Réponse directe à l'intent dans les 100 premiers mots (pour GEO/AEO)
- Featured Snippet optimisé par article (format prédéfini dans le brief)
- Au moins 1 CTA actionnable par article (vers page pilier ou contact)
- Rythme varié, zéro remplissage
- Sources citées et vérifiées

**Phase 4 — Auto-check intégré (Niveau 1)** :

Avant de livrer, le skill vérifie automatiquement :
- P1 bien placé dans H1, meta title et intro
- Densité de mots-clés correcte
- Maillage vers page pilier présent (≥2 liens)
- Pas de termes blacklist IA
- Mots/expressions interdits du client respectés
- Structure Hn correcte (pas de saut de niveau)
- Longueur dans la fourchette cible du sous-type
- Featured Snippet présent et formaté
- FAQ pertinente (pas de remplissage)
- Si problème détecté → Claude corrige AVANT de livrer

**Phase 5 — Génération des sorties** :

Pour chaque article, 3 fichiers sont produits :

| Fichier | Format | Contenu |
|---------|--------|---------|
| `[CLIENT]_[Cluster]_Art[N]_Blog.md` | Markdown | Brief Éditorial + Article complet + Version Newsletter + FAQ + Sources + Articles connexes |
| `[CLIENT]_[Cluster]_Art[N]_Blog.pdf` | PDF brandé NUKLEAR | Même contenu, mis en page avec branding |
| `[CLIENT]_[Cluster]_Art[N]_Strapi.md` | Markdown | 14 champs mappés pour import CMS Strapi |

### Structure du Blog MD/PDF (format Sortie 3)

Le fichier Blog MD/PDF est structuré en sections :

**BRIEF ÉDITORIAL** :
- Titre, angle éditorial, sous-type
- Intention de recherche & persona cible
- Promesse de valeur
- Structure H2/H3 avec notes éditoriales
- CTAs positionnés
- Métriques clés (P1, volume, longueur cible)

**ARTICLE COMPLET** :
- Meta title, meta description, slug, auteur, temps de lecture
- Article rédigé (contenu visible)
- FAQ

**VERSION NEWSLETTER** :
- Subject line options (3 propositions avec longueur)
- Preview text
- Email body (version courte pour newsletter)
- CTA principal + secondaire avec URLs
- Distribution recommandée (segment, timing, A/B test)

**PIED DE PAGE** :
- Sources utilisées (avec liens)
- Articles connexes du cluster (avec liens)

### Champs Strapi (fichier séparé)

| Champ | Contenu |
|-------|---------|
| `title` | Titre de l'article |
| `slug` | URL path |
| `author` | Nom + titre (E-E-A-T) |
| `cover_image` | (à ajouter manuellement) |
| `published_date` | Date de publication |
| `category` | Catégorie blog |
| `article_type` | Sous-type (GUIDE, HOW-TO, etc.) |
| `content` | Article rédigé (HTML ou MD) |
| `answer_capsule` | Answer Capsule AEO/GEO (50-70 mots) |
| `summary` | Résumé 2-3 phrases |
| `reading_time` | Temps de lecture en minutes |
| `meta` | { title, description } |
| `faq` | [ { question, answer }, ... ] |
| `related_articles` | [ slugs des articles connexes ] |

### ⏸ PAUSE B5 — Validation articles par le client

Présentation des articles au client. Boucle de validation identique au flux pages.

### Gestion multi-sessions

Claude surveille activement sa charge de contexte. Au-delà de 2 articles rédigés ou dès qu'il détecte une baisse de qualité (réponses plus courtes, formulations répétitives), il propose **proactivement** de :
1. Sauvegarder tout le travail fait
2. Générer un **brief de continuité** (`.md`)
3. Continuer dans une nouvelle session

Le brief de continuité contient : client, cluster, articles faits (avec statut), articles restants (avec priorité), chemins des documents de référence, ton calibré, corrections appliquées.

### Stockage

```
./clients/[CLIENT]/04_Blog/[Cluster-Name]/
├── [CLIENT]_[Cluster]_Briefs.md
└── [Article-Name]/
    ├── [CLIENT]_[Cluster]_Art[N]_Blog.md
    ├── [CLIENT]_[Cluster]_Art[N]_Blog.pdf
    └── [CLIENT]_[Cluster]_Art[N]_Strapi.md

./clients/[CLIENT]/_Claude/
└── [CLIENT]_Brief_Continuite_Blog.md
```

---

## ÉTAPE B4 — Audit Cohérence Cluster

**Skill** : `audit-coherence-blog`

**Toujours lancé dans une NOUVELLE session** pour un contexte frais, sans biais de rédaction.

### Inputs

- Tous les articles du cluster (MD)
- Stratégie de Mots-Clés Blog du Cluster (`.md`)
- Plan du Cluster validé (`.md`)
- Structure du Site Finale (`.md`)
- Stratégie de Mots-Clés des pages (`.md`, si disponible)

### Vérifications

**Par article** :
- P1 bien placé dans H1, meta title et intro
- Aucun P1 dupliqué entre articles
- H1 tous distincts
- Meta titles tous distincts
- Intents informationnels (pas commerciaux)
- 32 disciplines appliquées
- Densité de mots-clés correcte
- Ton conforme à la Fiche Entreprise
- Zéro terme de la blacklist IA
- Mots/expressions interdits du client respectés
- Featured Snippet présent et optimisé
- Sources citées et vérifiables
- Longueur dans la fourchette du sous-type

**Par cluster (vérifications croisées)** :
- Anti-cannibalisation inter-articles : pas de P1 dupliqués, pas d'angles redondants
- Anti-cannibalisation blog vs pages : P1 blog ≠ P1 pages (intent différent)
- Maillage bidirectionnel : chaque article → page pilier (≥2 liens), page pilier → articles
- Maillage inter-articles cohérent
- Couverture du champ sémantique : les articles couvrent les sous-thèmes prévus dans le plan
- Cohérence du ton entre articles du même cluster
- Progression logique des articles (du plus général au plus spécifique, ou du plus important au moins important)

### Output

| Fichier | Format | Rôle |
|---------|--------|------|
| `[CLIENT]_Audit_Blog_[Cluster]_[Date].md` | Markdown | Rapport : OK ou corrections (par article + vision cluster) |

### Boucle de validation

```
Articles rédigés (Étape B3)
        │
        ▼
┌─── BOUCLE DE VALIDATION ───┐
│                              │
│  Audit Cohérence Cluster     │
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

**Règle** : chaque correction significative repasse par l'audit en session fraîche. Corrections mineures (typos, reformulations légères) → validation directe client sans re-audit.

### Stockage

```
./clients/[CLIENT]/05_Audits/
└── [CLIENT]_Audit_Blog_[Cluster]_[Date].md
```

---

## 8 sous-types d'articles

| Sous-type | Intent | Usage | Modules spécifiques |
|-----------|--------|-------|---------------------|
| **GUIDE** | Informationnel | Vue d'ensemble exhaustive d'un sujet | Table des matières, sections détaillées, TL;DR |
| **HOW-TO** | Informationnel | Tutoriel pas-à-pas actionnable | Prérequis, étapes numérotées, résultat attendu, pièges |
| **LISTE** | Informationnel | Scan rapide, curation d'items | Critères de sélection, classement argumenté, verdict |
| **ANALYSE** | Informationnel | Étude approfondie avec données | Méthodologie, données/graphiques, conclusions, limites |
| **COMPARATIF** | Informationnel/Transactionnel | X vs Y vs Z | Critères pondérés, tableau comparatif, verdict, "pour qui" |
| **ETUDE-CAS** | Informationnel | Retour d'expérience concret | Contexte, défi, solution, résultats chiffrés, leçons |
| **GLOSSAIRE** | Informationnel | Définitions structurées | Entrées alphabétiques, liens croisés, exemples concrets |
| **ACTUALITE** | Informationnel | Réaction à un événement/tendance | Fait, analyse, impact, que faire maintenant, sources |

---

## Organisation en Topic Clusters

### Principe

Chaque cluster est rattaché à une **page pilier** (page service ou page structurelle). Le cluster complète la page pilier avec du contenu informationnel qui attire du trafic organique et renforce l'autorité thématique.

### Règles de maillage

- **Article → Page pilier** : ≥2 liens par article vers la page pilier (ancrage contextuel, pas forcé)
- **Page pilier → Articles** : la page pilier renvoie vers les articles du cluster (section "En savoir plus" ou liens contextuels)
- **Article → Article** : liens inter-articles quand le contexte le justifie (pas de maillage forcé entre tous les articles)
- **Article → Pages services** : liens vers les pages services pertinentes (CTA contextuels)

### Anti-cannibalisation stricte

- Blog P1 ≠ Page P1 : l'intent est différent (informationnel vs commercial)
- Si un blog P1 est trop proche d'un page P1, changer l'angle (ajouter "comment", "guide", "erreurs à éviter")
- Aucun P1 dupliqué entre articles du même cluster OU de clusters différents
- Vérification systématique à chaque étape (plan, stratégie, audit)

---

## Formats de fichiers figés — Blog

| Document | Format | Extension | Rôle |
|----------|--------|-----------|------|
| Plan du Cluster | Markdown | `.md` | Articles priorisés avec P1, intent, angle, sous-type |
| CSV GKP Import Blog | CSV UTF-8, header `Keyword`, max 9 999/fichier | `.csv` | Import GKP par cluster |
| CSV GKP Export Blog | CSV (retour Google) | `.csv` | Données volumes/CPC/concurrence |
| Stratégie MC Blog | HTML interactif (sauvegarde locale + export/import JSON-CSV) | `.html` | Interface validation stratégie blog |
| Stratégie MC Blog | Markdown | `.md` | Document de production (dérivé de l'export validé) |
| Mini-briefs cluster | Markdown | `.md` | Tous les briefs du cluster, vision d'ensemble |
| Article Blog | Markdown | `.md` | Livrable : Brief + Article + Newsletter + FAQ + Sources |
| Article Blog | PDF brandé | `.pdf` | Même contenu, mis en page |
| Export Strapi | Markdown | `.md` | 14 champs mappés pour import CMS |
| Audit cohérence blog | Markdown | `.md` | Rapport qualité par cluster |
| Brief de continuité | Markdown | `.md` | Gestion sessions Claude |

---

## Arborescence de stockage — Blog

```
./clients/[CLIENT]/04_Blog/
├── [Cluster-Name]/
│   ├── [CLIENT]_Plan_Cluster_[Nom].md
│   ├── [CLIENT]_Strategie_MC_Blog_[Cluster].html
│   ├── [CLIENT]_Strategie_MC_Blog_[Cluster].md
│   ├── [CLIENT]_[Cluster]_Briefs.md
│   ├── GKP-Import/
│   │   └── [CLIENT]_GKP_BLOG_[Cluster]_[FR|EN]_Lot[N].csv
│   ├── GKP-Export/
│   │   └── (CSV retour Google Keyword Planner)
│   ├── [Article-Name]/
│   │   ├── [CLIENT]_[Cluster]_Art[N]_Blog.md
│   │   ├── [CLIENT]_[Cluster]_Art[N]_Blog.pdf
│   │   └── [CLIENT]_[Cluster]_Art[N]_Strapi.md
│   └── [Article-Name]/
│       └── ...
└── [Autre-Cluster]/
    └── ...

./clients/[CLIENT]/05_Audits/
└── [CLIENT]_Audit_Blog_[Cluster]_[Date].md

./clients/[CLIENT]/_Claude/
└── [CLIENT]_Brief_Continuite_Blog.md
```

---

## Skills du flux blog

### Skills pipeline

| # | Skill | Rôle |
|---|-------|------|
| B1-B2 | `keyword-strategy` (adapté) | Préparation cluster + stratégie mots-clés blog |
| B3 | `redaction-blog` *(à construire)* | Rédaction articles, 2 phases internes, auto-check, 3 sorties |
| B4 | `audit-coherence-blog` *(à construire)* | Audit croisé cluster en session fraîche |

### Skills support (partagés avec le flux pages)

| Skill | Rôle |
|-------|------|
| `writing-standards` (ex `nuklear-writing`) | Socle qualité : blacklist IA, humanisation, styles de tons, frameworks rédactionnels |
| `web-optimization` (ex `nuklear-web`) | 32 disciplines SEO/GEO/AEO/CRO/E-E-A-T |
| `nuklear-brand` | Charte NUKLEAR (light theme, logo SVG fixe) — livrables NUKLEAR uniquement |

---

## Différences clés Blog vs Pages

| Aspect | Pages | Blog |
|--------|-------|------|
| Intent | Commercial | Informationnel |
| Organisation | Par page | Par cluster (rattaché à une page pilier) |
| Maillage | Inter-pages | Bidirectionnel article ↔ pilier + inter-articles |
| Schemas JSON-LD | WebPage, Organization, Service | BlogPosting, Article avec auteur E-E-A-T |
| Featured Snippet | Optionnel | Obligatoire par article (format prédéfini) |
| Format livrable | DOCX V10.2 (7 parties) | PDF + MD (Brief + Article + Newsletter) + Strapi MD |
| Audit | Par lot de pages | Par cluster |
| Mots-clés | 50K+ minimum pour le site, viser 100K+ | 10K+ minimum par cluster, viser 20-30K+ |
| Sous-types | Frameworks (Hub, PASTOR, AIDA, PAS, StoryBrand, FAQ-first) | 8 sous-types (GUIDE, HOW-TO, LISTE, ANALYSE, COMPARATIF, ETUDE-CAS, GLOSSAIRE, ACTUALITE) |
| Newsletter | Non | Oui, intégrée dans le livrable |
| Export CMS | Non | Oui, Strapi (14 champs) |

---

## Prochaines étapes

1. ~~Valider le flux blog~~ ✅
2. Mettre à jour `keyword-strategy` pour supporter le mode blog (Étapes B1-B2)
3. Créer le skill `redaction-blog` (Étape B3)
4. Créer le skill `audit-coherence-blog` (Étape B4)
5. Nettoyer et réorganiser les réalisations blog NUKLEAR (Art1-Art5 + Art7)
6. Intégrer le flux blog dans le dossier de transmission + présentation
