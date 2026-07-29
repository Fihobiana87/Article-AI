# Architecture Skills — Générateur de Pages Web Optimisées

> **Version** : 1.0 — 12 Mars 2026
> **Statut** : VALIDÉ — Décisions actées avec Thibaud
> **Contexte** : Refonte du système de prompts existant (11 prompts chaînés, V2.2) en skills Cowork automatisés

---

## 1. DÉCISION : 3 Skills Indépendants en Pipeline

Le système monolithique (11 prompts manuels) est remplacé par **3 skills Cowork** chaînables mais utilisables séparément.

```
┌─────────────────────┐     ┌─────────────────────┐     ┌─────────────────────┐
│  SKILL 1             │     │  SKILL 2             │     │  SKILL 3             │
│  web-discovery       │────▶│  web-keywords        │────▶│  web-content         │
│                      │     │                      │     │                      │
│  Intelligence &      │     │  Stratégie           │     │  Génération de       │
│  Fondations          │     │  Mots-Clés           │     │  Contenu             │
│                      │     │                      │     │                      │
│  Output:             │     │  Output:             │     │  Output V1:          │
│  D-02 (Factsheet)    │     │  D-04 × N pages      │     │  Pack Validation     │
│  D-03 (Structure)    │     │  (Fiches Keywords)   │     │  Client (textes)     │
│  Form HTML (trous)   │     │                      │     │                      │
└─────────────────────┘     └─────────────────────┘     │  Output V2:          │
                                                         │  Pack Dev            │
                                                         │  Pack WordPress      │
                                                         │  Pack Designer       │
                                                         └─────────────────────┘
```

### Pourquoi 3 et pas 1

| Raison | Détail |
|--------|--------|
| **Temporalité** | Chaque skill correspond à un moment différent du projet (kickoff → recherche → production) |
| **Qualité** | Un prompt plus court et focalisé = meilleur output qu'un prompt géant |
| **Business** | Chaque phase vendable séparément |
| **Réutilisabilité** | Un client avec un site existant peut skip une partie du Skill 1 |

---

## 2. DÉCISIONS TECHNIQUES

### Formulaire client
- **Format** : HTML interactif avec export JSON
- **Workflow** : Claude génère le HTML pré-rempli → client remplit dans navigateur → export JSON → Claude le lit dans la conversation ou via le dossier
- **Pas de dépendance externe** (pas de Google Forms, pas de backend)
- **Dynamique** : seules les questions où Claude n'a pas pu déduire la réponse sont posées

### Scraping site existant
- **Méthode** : Chrome (navigation réelle via Claude in Chrome)
- **Raison** : gère les sites JS/SPA, contenu dynamique, meilleure extraction

### Qualité préservée
- Les **32 disciplines SEO/GEO/AEO/CRO** du système V2.2 sont embarquées dans le Skill 3
- Le **workflow 2 passes** (contenu puis technique) est conservé dans le Skill 3
- L'**anti-cannibalisation** est automatisée (plus de check manuel)
- La **blacklist IA** et les règles d'humanisation sont intégrées

---

## 3. FICHIERS DE RÉFÉRENCE DU SYSTÈME EXISTANT

Le système V2.2 se trouve dans : `PROJETS/Generateur de pages Web Optimisées/Système/`

### Fichiers clés à réutiliser dans les skills

| Fichier existant | Réutilisé dans | Comment |
|---|---|---|
| D-01 (Formulaire Intake) | Skill 1 | Base pour les questions du form HTML dynamique |
| D-02 (Template Factsheet) | Skill 1 | Format de sortie de la Fiche Entreprise |
| D-03 (Template Structure) | Skill 1 | Format de sortie de la Structure Site |
| P-01 (Prompt Fiche Entreprise) | Skill 1 | Logique de traitement des données client |
| P-02 (Prompt Structure Site) | Skill 1 | Logique de création d'arborescence |
| P-03 (Prompt Liste Keywords) | Skill 2 | Logique de génération keywords bruts |
| P-04 (Prompt Fiche KW/Page) | Skill 2 | Format D-04, traitement GKP |
| P-05_CORE (32 disciplines) | Skill 3 | Cœur du système de génération |
| P-05_SEO_TECH (Passe 2) | Skill 3 | Référentiel technique partagé |
| P-05_VALIDATION | Skill 3 | Auto-validation intégrée |
| P-06 (Extraction Vues) | Skill 3 | Génération des packs Dev/Designer/WP |
| P-07 (Audit Cross-Pages) | Skill 3 | Audit automatique en fin de batch |

### Fichiers obsolètes après migration

| Fichier | Raison |
|---|---|
| P-05_GUIDE_OPERATEUR | Plus d'opérateur humain — Claude gère |
| P-05_CHECKLIST | Intégré dans l'auto-validation du Skill 3 |
| Google Form Links.txt | Remplacé par form HTML dynamique |

---

## 4. ORDRE DE CONSTRUCTION

| # | Skill | Priorité | Statut |
|---|---|---|---|
| 1 | `web-discovery` | 🔴 Premier | 🔴 À construire |
| 2 | `web-keywords` | 🟡 Après test Skill 1 | ⬜ En attente |
| 3 | `web-content` | 🟡 Après test Skill 2 | ⬜ En attente |

### Flux B (Blog) — À planifier plus tard

Les skills blog (équivalent P-08, P-09, P-10) seront construits après validation du Flux A.
L'architecture sera similaire : un skill `blog-strategy` + un skill `blog-content`.

---

## 5. CHANGELOG

| Version | Date | Modification |
|---------|------|-------------|
| 1.0 | 2026-03-12 | Création — Décisions d'architecture validées avec Thibaud |

---

*Document d'architecture — NUKLEAR — Générateur de Pages Web — V1.0*
