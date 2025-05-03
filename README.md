Je développe actuellement ce projet dans le but de créer un espace automatisé et boosté à l'IA pour les entrepreneurs, TPE et PME.
j'ajouterais des worklows au fil du temps


---

# Automatisations n8n pour Discord : LinkedIn + Répondeur Email

Ce projet regroupe deux workflows automatisés sous n8n, connectés à un serveur Discord. Ils visent à **optimiser la communication professionnelle** à travers deux axes complémentaires :
   🔹 la **publication stratégique sur LinkedIn**,
   🔹 la **gestion intelligente des emails**.

D'autre fonctionnalités sont à venir : 
• Gestion des rendez vous
• Création de factures

---

## Outils et APIs utilisées

- **n8n** (outil no-code d'automatisation)
- **Discord Bot API** – pour les notifications et validations
- **OpenAI (GPT-3.5 / GPT-4o-mini)** – pour la génération de contenu
- **Google Sheets & Airtable** – gestion de planning éditorial
- **LinkedIn API** – publication automatique
- **IMAP / SMTP** – réception et réponse aux emails

---

# 📌 Workflow 1 — Ghostwritter

### 🎯 Objectif
Automatiser la création, validation et publication de posts LinkedIn à partir d’un planning éditorial externe (Google Sheets ou Airtable), avec interaction humaine via Discord.

### 🔁 Fonctionnement

1. ⏱ Déclencheur toutes les 12h
2. 📚 Lecture du sujet dans Airtable / Google Sheets (statut = "GO")
3. ✍️ Génération du post via GPT-3.5-Turbo avec un prompt copywriting
4. 📩 Notification sur Discord avec bouton de validation
5. 🧠 Décision :
   - ✅ Si **validé** → Publication LinkedIn + MAJ du statut (`GO` → `DONE`)
   - ❌ Si **refusé** → Notification de non-publication

### 🧩 Modules clés

| Étape                        | Description                                                |
|-----------------------------|------------------------------------------------------------|
| `Déclencheur`               | Démarrage du processus (toutes les 12h)                     |
| `Planning de publications`  | Connexion à Airtable/Google Sheets                         |
| `Création du contenu`       | Génération IA via OpenAI                                   |
| `Discord - Notification`    | Message avec boutons de validation                         |
| `Condition`                 | Si approuvé, publication LinkedIn                          |
| `Mise à jour`               | Statut mis à jour dans Airtable et Google Sheets           |

---

# 📬 Workflow 2 — Emailing

### 🎯 Objectif
Notifier des nouveaux mails et générer automatiquement des réponses professionnelles aux emails reçus, avec contrôle humain via Discord avant envoi.

### 🔁 Fonctionnement

1. 📥 Réception d’un nouvel email (via IMAP)
2. 📩 Notification sur Discord du contenu du message
3. 🗂 Insertion automatique des infos (prix, horaires, lien Calendly…)
4. 🧠 Génération de réponse avec un **agent IA spécialisé OccitAI**
5. 📤 Proposition de réponse sur Discord :
   - ✅ Si **validé** → Envoi de l’email via SMTP
   - ❌ Si **refusé** → Notification d’annulation

### 🧩 Modules clés

| Étape                      | Description                                               |
|---------------------------|-----------------------------------------------------------|
| `Attente email`           | Surveillance boîte mail                                   |
| `Notification Discord`    | Récap de l’email reçu                                     |
| `Formulaire informations` | Ajout des données nécessaires à la réponse                |
| `AI Agent`                | Rédaction par l’IA (OpenAI GPT-4o-mini)                   |
| `Validation Discord`      | Question avec boutons de choix                            |
| `Envoi email / Annulation`| Envoi ou refus selon la décision                          |

---

## 🔐 Accès API requis

- `OpenAI API Key`
- `Discord Bot Token` 
- `LinkedIn OAuth2`
- `Airtable Token` ou `Google Sheets OAuth2`
- `IMAP / SMTP credentials`

---

## ✅ Bénéfices

- **Gain de temps** considérable dans la gestion de contenu et d’emails
- **Contrôle humain intégré**, garantissant la qualité des réponses
- **Centralisation sur Discord**, évitant les outils multiples
- **Scalabilité** : facilement duplicable pour d'autres cas d’usage

---

## 📎 À propos

Ces workflows sont développés pour une utilisation interne, mais peuvent être adaptés à d'autres structures souhaitant allier **automatisation intelligente** et **contrôle éditorial humain**.

---

## 📬 Me contacter / En savoir plus

Si ce cockpit t'inspire ou si tu souhaites mettre en place une solution similaire pour ton entreprise, je suis joignable ici :

- 💼 [LinkedIn](https://www.linkedin.com/in/liamsdd)  
- 🐦 [Twitter / X](https://twitter.com/liam4chill)  
- 📖 [Lire l’article complet sur mon blog](https://liam4chill.fr/n8n)

---

🛠️ *Dernière mise à jour : Mai 2025*
