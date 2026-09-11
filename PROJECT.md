# NIM Drop — Nimiq Mini Apps Competition (Cycle II)

## Concept
Enveloppe rouge porte-bonheur (style "hongbao"/red envelope), réinventée pour Nimiq Pay :
un créateur met un montant en NIM dans un "drop", fixe le nombre de parts (égales ou
aléatoires/surprise), partage un lien/QR, et chaque ami qui ouvre le drop reçoit un envoi
NIM direct, instantané et sans frais.

Pourquoi ce concept : le pattern "red envelope" est déjà un mécanisme viral éprouvé
(WeChat hongbao, bots Telegram/Discord de tips) mais devient pénible sur la plupart des
chaînes à cause des frais de gas sur les micro-paiements groupés. Nimiq (feeless,
finalité ~1s) supprime exactement ce frein — c'est l'argument produit central de la démo.

## Ce qui est déjà construit
- `index.html` — page marketing + démo interactive complète (single file, vanilla JS,
  aucune dépendance backend), publiée en Artifact Claude :
  https://claude.ai/code/artifact/2348310d-d6ce-47ed-aa0a-cc0df413bcca
- Simulation complète du flux "Créer → Partager → Ouvrir" avec algorithme de répartition
  façon enveloppe rouge (`luckySplit`) et répartition égale.
- Détection progressive du vrai environnement Nimiq Pay (`window.nimiqPay`) : si présent,
  tente de charger `@nimiq/mini-app-sdk` (via jsDelivr ESM), se connecte avec `init()` et
  `listAccounts()`. Hors de Nimiq Pay, reste en mode démo (clairement affiché).
- Panneau de code montrant les vrais appels SDK utilisés (`listAccounts`,
  `sendBasicTransactionWithData`) pour la crédibilité technique.

## Ce qui manque pour une vraie soumission
1. **Backend/coordination réelle des claims** : dans la démo, la personne qui "ouvre"
   simule côté client. En prod, il faut soit :
   - garder le créateur en ligne et faire l'envoi direct pair-à-pair au moment du claim
     (le plus simple, zéro custody, mais demande que le créateur ait l'app ouverte), ou
   - un petit service de coordination (éviter double-claim) — à héberger séparément.
2. **Hébergement public HTTPS** requis pour charger la mini app dans Nimiq Pay
   (`nimiqpay://miniapp?url=...` ou `https://nimpay.app/miniapps/open/...`).
3. **Vrais QR codes / liens de partage** (actuellement un simple code à 6 caractères
   cosmétique).
4. **Test réel dans Nimiq Pay** via "Load a local Mini App" pour valider les appels SDK
   en conditions réelles (adresses, confirmations natives, fee handling).
5. Soumission via la page "Submit" (sign-in GitHub → PR automatique) avant le
   18 septembre 2026, 23:59 UTC.

## Stack
- HTML/CSS/JS vanilla, aucune build step.
- Polices : Fraunces (display) + Sora (UI), via Google Fonts.
- `@nimiq/mini-app-sdk` chargé dynamiquement depuis jsDelivr, uniquement si
  `window.nimiqPay` est détecté.
- Aucune donnée personnelle, aucune clé privée gérée côté app — toutes les opérations
  sensibles passent par les dialogues natifs de Nimiq Pay.
