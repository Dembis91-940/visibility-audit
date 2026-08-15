# AI Visibility Audit — Micro-SaaS de conformité IA

**Le diagnostic de visibilité des usages d'IA pour les organisations réglementées** (santé, finance, juridique).

Personne ne sait quels outils d'IA sont utilisés, où, avec quelles données. Ce site propose un parcours commercial complet pour répondre à ce problème : un questionnaire d'audit réel qui génère un rapport d'alerte immédiat, une offre de rapport complet, et un audit annuel avec lettre de conformité.

---

## 📦 Contenu du livrable

| Fichier | Rôle |
|---|---|
| `index.html` | Landing page institutionnelle : hero sobre, badges de conformité, méthode, sections sectorielles (santé / finance / juridique), 3 offres tarifées, témoignages anonymisés, FAQ, formulaire de contact EmailJS. |
| `questionnaire.html` | **Outil réel** : 20 questions en 6 catégories → rapport d'alerte généré localement dans le navigateur (inventaire, 6 catégories de risque sur 4 niveaux, alertes détaillées, recommandations P1/P2/P3, score global), bouton imprimer/PDF. |
| `exemple-rapport.html` | Exemple de rapport d'alerte anonymisé et fictif (cabinet médical), identique en structure au rapport généré. |
| `README.md` | Ce document. |

Ouverture : double-clic sur `index.html` — aucun serveur ni dépendance requis (hors CDN EmailJS pour le formulaire de contact).

---

## 🎨 Identité visuelle

Architecture **institutionnelle / confiance**, volontairement opposée aux sites « startup » (pas de fond sombre cyan/violet, pas d'animations lourdes) :

- Fond très clair `#f8fafc`, surfaces blanches
- Accent bleu `#1d4ed8`, touches gris acier `#475569`, encres `#0f172a` / `#334155` / `#64748b`
- Bordures fines `#e2e8f0`, tableaux propres, bandeaux de mise en garde ambre, badges de conformité
- Typographie système (sans serif), aucune dépendance de police externe

## 💶 Offres affichées

| Offre | Prix | Contenu |
|---|---|---|
| Audit de démarrage | **0 €** | Questionnaire 15 min → rapport d'alerte automatique (inventaire + risques + recommandations) |
| Rapport complet | **149 € TTC** | Analyse experte, entretiens, vérification des conditions d'utilisation, registre des traitements IA, plan d'action priorisé |
| Audit annuel | **299 € TTC / an** | Rapport + suivi trimestriel + veille réglementaire + lettre de conformité |

Mentions : prix TTC, TVA non applicable (art. 293 B du CGI), devis avant facturation.

## ⚙️ Intégration EmailJS (formulaire de contact)

Réelle, configurée dans `index.html` :

```js
emailjs.init('8Pui4ZEqxW2jRVF7h');
emailjs.send('service_cy1ytdb', 'template_xpo58cv', {
  site:     /* organisation */,
  name:     /* nom           */,
  email:    /* email         */,
  question: /* demande       */
});
```

- SDK chargé depuis le CDN officiel : `https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js`
- Le template reçoit exactement les 4 champs attendus : `{site, name, email, question}`
- États gérés : envoi en cours, succès, échec, service indisponible (hors-ligne)
- Case de consentement RGPD obligatoire avant envoi

## 🔍 Comment fonctionne le questionnaire (zéro simulateur)

Le rapport n'est pas pré-rempli : il est **calculé à partir des réponses** de l'utilisateur.

1. **20 questions** réparties en 6 catégories : inventaire des usages (5), données traitées (4), accès & circulation (3), stockage & traitement (3), consentement & conformité (3), formation & gouvernance (2).
2. Chaque option porte un **poids de risque** (0 à 4). Les cases à cocher sont plafonnées à un maximum par question.
3. **Score par catégorie** = somme des poids / maximum possible × 100, classé sur **4 niveaux** :
   - 0–24 **Faible** · 25–49 **Modéré** · 50–74 **Élevé** · 75–100 **Critique**
4. **Score global** = moyenne des 6 catégories.
5. Le rapport assemble :
   - l'**inventaire des usages déclarés** (outils, proportion d'utilisateurs, services, finalités, types de données, comptes, conservation) ;
   - les **6 niveaux de risque** avec barres et commentaires par catégorie ;
   - les **alertes détaillées** (une par question risquée, avec sévérité) ;
   - les **recommandations priorisées P1 / P2 / P3** issues des réponses risquées, des seuils par catégorie, du secteur choisi et de bonnes pratiques permanentes ;
   - les **prochaines étapes** commerciales.
6. Le secteur (santé / finance / juridique / autre) **adapte les recommandations** (HDS, DORA/ACPR/LCB-FT, secret professionnel).

### Confidentialité

Le calcul s'exécute **entièrement dans le navigateur** : aucune réponse n'est envoyée sur un serveur. Les réponses sont persistées en `localStorage` (clé `ava_answers_v1`) uniquement pour permettre de reprendre le questionnaire ; un bouton « Recommencer » permet de tout effacer. C'est un argument commercial affiché sur le site.

### Ajouter ou modifier une question

Les données sont dans le tableau `QUESTIONS` du script de `questionnaire.html`. Chaque question a : `id`, `cat` (l'une des 6 catégories), `type` (`radio` / `check`), `title`, `help`, `max` (plafond de score, optionnel), et `options` (avec `label`, `score`, et éventuellement `alert` / `rec`). Les `SECTOR_RECS` et `catRecs` permettent d'ajouter des recommandations sectorielles et par catégorie.

## 🧪 Vérifications effectuées

- Ouverture des 3 pages dans un navigateur, zéro erreur console
- Parcours complet du questionnaire automatisé (20 réponses simulées) → rapport généré, score calculé
- Envoi EmailJS testé avec les identifiants réels (voir section précédente pour la config)
- CSS d'impression (`@media print`) validé pour le rapport et l'exemple

## ⚠️ Limites et honnêteté juridique

- Le rapport d'alerte est un **diagnostic déclaratif** : il reflète les réponses fournies, pas nécessairement la réalité complète des usages.
- Le site affiche explicitement que le rapport ne constitue ni un avis juridique, ni une certification, et que la lettre de conformité annuelle est un document d'appui (elle ne remplace pas l'avis d'un avocat ou d'un DPO).
- Les références réglementaires (RGPD art. 9/13/14/30, HDS, DORA, ACPR, LCB-FT, secret professionnel) sont présentées de manière générale, sans garantie d'exhaustivité — à faire valider par un professionnel du droit avant toute communication publique.

## 🔧 Personnalisation rapide

- **Couleurs** : variables CSS en tête de chaque fichier (`--blue`, `--bg`, etc.)
- **Offres / tarifs** : sections `#offres` (index) et `next-steps` (questionnaire, exemple)
- **EmailJS** : constantes `SERVICE_ID`, `TEMPLATE_ID`, `PUBLIC_KEY` dans le script de `index.html`
- **Secteurs** : `SECTOR_RECS` dans `questionnaire.html` ; blocs de la section `#secteurs` dans `index.html`

---

© 2026 AI Visibility Audit — livrable de démonstration. Ne pas publier sans avoir validé les mentions juridiques avec un professionnel.
