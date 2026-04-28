# 🌀 Whispr — Application de messagerie anonyme

> Partage ton lien, reçois des messages anonymes, réponds en chat privé.

---

## 📁 Structure du projet

```
whispr/
├── index.html              ← Toute l'interface (HTML/CSS/JS)
├── package.json            ← Dépendances Capacitor
├── capacitor.config.json   ← Config de l'app Android
├── www/                    ← Généré automatiquement (copie de index.html)
├── android/                ← Généré par Capacitor
└── .github/
    └── workflows/
        └── build.yml       ← Build automatique de l'APK
```

---

## 🚀 Étapes pour générer ton APK

### 1. Préparer Firebase

1. Va sur [console.firebase.google.com](https://console.firebase.google.com)
2. Crée un nouveau projet
3. Active **Authentication** (Email/Password)
4. Active **Firestore Database**
5. Copie ta config Firebase dans `index.html` à l'endroit marqué `VOTRE_...`

---

### 2. Créer le repo GitHub

```bash
# Sur ton ordinateur
git init
git add .
git commit -m "Initial commit Whispr"
git branch -M main
git remote add origin https://github.com/TON_USERNAME/whispr.git
git push -u origin main
```

---

### 3. Générer l'APK via GitHub Actions

1. Va dans ton repo GitHub
2. Clique sur l'onglet **Actions**
3. Tu verras le workflow **"Build APK Whispr"** se lancer automatiquement
4. Attends ~5 minutes ⏳
5. Clique sur le workflow terminé → **Artifacts** → télécharge **whispr-debug-apk**
6. Installe le fichier `.apk` sur ton téléphone Android !

> 💡 **Important** : Active "Sources inconnues" sur Android pour installer un APK hors Play Store.  
> Paramètres → Sécurité → Installer des apps inconnues

---

### 4. Lancer manuellement un build

1. Onglet **Actions** dans GitHub
2. Clique sur **"Build APK Whispr"**
3. Clique **"Run workflow"** → **"Run workflow"**

---

## 🔥 Règles Firestore à configurer

Dans Firebase Console → Firestore → Règles, colle ces règles :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Profils utilisateurs
    match /users/{userId} {
      allow read: if true;
      allow write: if request.auth.uid == userId;

      // Messages — lecture/écriture pour le propriétaire
      match /messages/{messageId} {
        allow read, update, delete: if request.auth.uid == userId;
        allow create: if true; // N'importe qui peut envoyer un message
      }
    }
  }
}
```

---

## 📱 Fonctionnalités V1

- ✅ Inscription / Connexion (Firebase Auth)
- ✅ Lien anonyme unique par utilisateur
- ✅ Envoi de messages anonymes
- ✅ Chat privé (réponse au message)
- ✅ Marquage lu / non lu
- ✅ Suppression de message

## 🔮 Fonctionnalités V2 (Premium)

- 🔒 Messages illimités par jour
- 🎨 Thèmes personnalisés
- 👁 Voir si quelqu'un a vu ton profil
- 🔔 Notifications push
- 📊 Statistiques

---

## 🛠 Tech Stack

| Couche | Technologie |
|--------|------------|
| Frontend | HTML / CSS / Vanilla JS |
| Backend | Firebase (Auth + Firestore) |
| Mobile | Capacitor 5 |
| CI/CD | GitHub Actions |
| Hébergement | Firebase Hosting (optionnel) |
