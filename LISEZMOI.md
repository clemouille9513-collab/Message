# Bulle — installation (≈ 15 minutes, tout se fait depuis le téléphone)

Bulle est une appli web installable : messages privés illimités, emojis, photos, vidéos et position des contacts.
Elle a besoin d'un serveur : on utilise **Firebase** (gratuit chez Google).

## 1. Créer le projet Firebase
1. Va sur https://console.firebase.google.com et connecte-toi avec ton compte Google.
2. **Créer un projet** → nom au choix (ex. `bulle`) → tu peux refuser Google Analytics.

## 2. Activer les comptes
1. Menu **Build → Authentication → Commencer**.
2. Onglet **Méthode de connexion** → **E-mail/Mot de passe** → active → Enregistrer.

## 3. Créer la base de données
1. Menu **Build → Realtime Database → Créer une base de données**.
2. Région : Belgique (europe-west1). Mode : **verrouillé**.
3. Onglet **Règles** → efface tout, colle ce qui suit, puis **Publier** :

```json
{
  "rules": {
    "usernames": {
      "$n": { ".read": "auth != null", ".write": "auth != null && !data.exists() && newData.val() === auth.uid" }
    },
    "users": {
      "$uid": { ".read": "auth != null", ".write": "auth != null && auth.uid === $uid" }
    },
    "userChats": {
      "$uid": {
        ".read": "auth != null && auth.uid === $uid",
        "$chatId": { ".write": "auth != null && (auth.uid === $uid || newData.child('with').val() === auth.uid)" }
      }
    },
    "chats": {
      "$chatId": {
        ".read": "auth != null && $chatId.contains(auth.uid)",
        ".write": "auth != null && $chatId.contains(auth.uid)",
        "messages": { "$m": { ".validate": "newData.child('from').val() === auth.uid" } }
      }
    },
    "media": {
      "$chatId": {
        ".read": "auth != null && $chatId.contains(auth.uid)",
        ".write": "auth != null && $chatId.contains(auth.uid)"
      }
    },
    "locations": {
      "$uid": {
        ".read": "auth != null && (auth.uid === $uid || root.child('shares').child($uid).child(auth.uid).val() === true)",
        ".write": "auth != null && auth.uid === $uid"
      }
    },
    "shares": {
      "$owner": {
        ".read": "auth != null && auth.uid === $owner",
        ".write": "auth != null && auth.uid === $owner",
        "$viewer": { ".read": "auth != null && auth.uid === $viewer" }
      }
    }
  }
}
```

## 4. Récupérer la config
1. Roue ⚙️ → **Paramètres du projet** → section **Vos applications** → icône **</>** (Web).
2. Donne un nom, **Enregistrer**. Firebase affiche un bloc `firebaseConfig = { ... }`.
3. **Envoie-moi ce bloc** dans la conversation : je te renvoie `index.html` déjà rempli.
   (Ou remplace toi-même les `COLLE_ICI` en haut du script de `index.html`.)
   Vérifie que la ligne `databaseURL` est bien présente.

## 5. Mettre l'appli en ligne (GitHub Pages, gratuit)
L'appli doit être en `https://` pour que la position et l'installation fonctionnent.
1. Crée un compte sur https://github.com puis un nouveau dépôt **public** nommé `bulle`.
2. **Add file → Upload files** : envoie les 5 fichiers (`index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`) → Commit.
3. **Settings → Pages** → Source : *Deploy from a branch* → branche `main`, dossier `/ (root)` → Save.
4. Après ~1 minute, ton adresse est `https://TON-PSEUDO.github.io/bulle/`.

## 6. Installer sur le S22
Ouvre l'adresse dans Chrome → menu ⋮ → **Installer l'application** (ou *Ajouter à l'écran d'accueil*).
Tes amis font pareil, puis chacun crée son compte avec un pseudo. Pour discuter : **＋** puis le pseudo de l'autre.
Pour la position : onglet **Carte** → active « Partager ma position en direct » et coche les personnes qui peuvent te voir.

## À savoir
- **Position** : elle n'est envoyée que quand l'appli est ouverte (une appli web ne peut pas suivre en arrière-plan).
- **Pas de notifications** quand l'appli est fermée.
- **Vidéos** : 6 Mo max par envoi. Les photos sont réduites automatiquement.
- **« Illimité »** : aucune limite dans l'appli, mais l'offre gratuite de Firebase est limitée (1 Go stockés, 10 Go téléchargés par mois). Les photos et vidéos en consomment le plus.
- **Confidentialité** : les messages ne sont visibles que par les deux personnes de la discussion dans l'appli, mais ils ne sont pas chiffrés de bout en bout : le propriétaire du projet Firebase (toi) peut les lire dans la console.
- **APK** : plus tard, on pourra transformer cette appli web en vrai APK avec un outil gratuit (ex. PWABuilder), une fois qu'elle est en ligne.
