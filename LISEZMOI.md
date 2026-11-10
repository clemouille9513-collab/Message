# Bulle — messagerie de téléphone à téléphone (sans serveur)

Messages privés illimités, emojis, photos, vidéos et position en direct. Pas de compte, pas de serveur : les deux téléphones se connectent directement (WebRTC).

## Mise en ligne (une seule fois, GitHub Pages)
1. Dépôt GitHub public avec ces fichiers : `index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`.
2. **Settings → Pages** → Source : *Deploy from a branch* → branche `main`, dossier `/ (root)` → Save.
3. Ton adresse : `https://TON-PSEUDO.github.io/NOM-DU-DEPOT/`.

## Installer sur le téléphone
Ouvre l'adresse dans Chrome → menu ⋮ → **Installer l'application**. Ton ami fait pareil.
Pour en faire un vrai APK : https://www.pwabuilder.com → colle l'adresse → **Package for Android**.

## Se connecter à un ami
Vous devez être **tous les deux en ligne**.
1. Toi : **＋ → 1 · Je crée un code**, puis envoie le code à ton ami (SMS, WhatsApp…).
2. Ton ami : **＋ → 2 · J'ai reçu un code**, colle le code, puis lui renvoie le code de réponse.
3. Toi : colle sa réponse dans « 2 · Colle sa réponse » et appuie sur **Connecter**.

Une fois connectés, vous pouvez discuter, envoyer emojis, photos, vidéos (30 Mo max) et votre position (onglet **Carte**).

## À savoir
- **La connexion est à refaire à chaque fois** que l'un de vous ferme l'appli ou perd le réseau : sans serveur, personne ne peut « retrouver » l'autre tout seul. L'historique, lui, reste sur le téléphone.
- **Pas de message hors ligne** : si l'autre n'est pas connecté, le message ne part pas.
- **Pas de notifications** quand l'appli est fermée. Active « Garder l'écran allumé » dans ⚙️ pour éviter les coupures.
- **Position** : envoyée seulement pendant que vous êtes connectés et que l'appli est ouverte.
- **Réseaux 4G stricts** : la connexion directe peut échouer sur certains réseaux. Essayez en Wi-Fi.
- **Confidentialité** : messages et médias ne passent par aucun serveur, ils sont chiffrés entre les deux téléphones et stockés uniquement sur eux. Ne partage tes codes qu'avec la personne concernée.
