# LAB-14-Bypass-Root-Detection-sur-Android-Techniques-Dynamiques-avec-Frida-Objection-et-Hooks-Natif

# 1. Présentation du laboratoire et environnement de travail

Cette première étape présente l’environnement utilisé pour réaliser les manipulations d’analyse dynamique Android.

Le laboratoire est exécuté dans un environnement contrôlé comprenant :

* Windows PowerShell ;
* Android Studio Emulator ;
* ADB ;
* Frida ;
* Objection ;
* Medusa.

L’objectif principal est de préparer une plateforme permettant l’instrumentation dynamique et l’analyse des mécanismes de détection root sur Android.

**Capture associée :**

<img width="322" height="623" alt="image" src="https://github.com/user-attachments/assets/764a326e-c36e-4449-9606-3839a9ac3122" />


---

# 2. Vérification de Python, Pip et Frida

Avant de commencer les manipulations, il est nécessaire de vérifier que les outils indispensables sont correctement installés sur le système.

Les vérifications portent principalement sur :

* Python ;
* Pip ;
* Frida.

Exemples de commandes utilisées :

```bash id="w4m8pv"
python --version
```

```bash id="r9u1xa"
pip --version
```

```bash id="n7d2ke"
frida --version
```

Cette étape permet de confirmer que l’environnement logiciel est prêt pour les opérations d’instrumentation dynamique.

**Captures associées :**

<img width="1441" height="331" alt="image" src="https://github.com/user-attachments/assets/fbf6261c-c6eb-4947-9d77-a19e1681d8d3" />


---

# 3. Vérification de la connexion ADB avec l’émulateur Android

La communication entre le poste de travail et l’émulateur Android doit être vérifiée avant toute manipulation.

Commande utilisée :

```bash id="q5p7zb"
adb devices
```

Cette commande affiche les appareils Android détectés par ADB.

Le résultat doit montrer l’émulateur Android actif ainsi que son identifiant.

Cette étape confirme que l’émulateur est correctement reconnu par le système.

**Capture associée :**

<img width="977" height="575" alt="image" src="https://github.com/user-attachments/assets/ae7d0d52-6a13-46f6-8cd2-929ef0887448" />


---

# 4. Démarrage de frida-server et détection des applications Android

Après la connexion ADB, le service `frida-server` est lancé sur l’émulateur Android.

Cette étape permet à Frida de communiquer avec les processus Android en cours d’exécution.

Une fois le serveur démarré, les applications installées peuvent être listées avec :

```bash id="d3x8hn"
frida-ps -Uai
```

### Explication

| Option | Fonction                            |
| ------ | ----------------------------------- |
| `-U`   | Connexion USB / émulateur Android   |
| `-a`   | Affiche les applications            |
| `-i`   | Affiche les informations détaillées |

Cette étape confirme que Frida fonctionne correctement avec l’environnement Android.

**Capture associée :**
<img width="1257" height="597" alt="image" src="https://github.com/user-attachments/assets/5a76a62a-fb6b-4618-86d2-5a34cf27f318" />


---

# 5. Test d’injection Frida avec le script `hello.js`

Afin de vérifier le bon fonctionnement de l’instrumentation dynamique, un premier script Frida nommé `hello.js` est injecté dans l’application cible.

Exemple de commande :

```bash id="k8z1qw"
frida -U -f com.example.app -l hello.js
```

Le script permet de tester :

* l’injection dynamique ;
* l’exécution du code JavaScript ;
* la communication entre Frida et l’application Android.

Cette étape valide le fonctionnement global de l’environnement Frida.

**Capture associée :**

<img width="1153" height="438" alt="image" src="https://github.com/user-attachments/assets/2002a6ad-a2fb-4d7b-b59e-23d82a6eccca" />


---

# 6. Bypass des vérifications Root Java avec `bypass_root_basic.js`

Après le test initial, un second script Frida est utilisé afin de contourner certaines vérifications root réalisées au niveau Java.

Le script :

```text id="m9y4bt"
bypass_root_basic.js
```

intercepte certaines fonctions utilisées par l’application pour :

* détecter le root ;
* rechercher le binaire `su` ;
* identifier des applications root ;
* vérifier des propriétés système Android.

L’objectif est de modifier dynamiquement les réponses retournées à l’application afin de simuler un environnement non rooté.

**Capture associée :**

<img width="1137" height="516" alt="image" src="https://github.com/user-attachments/assets/355a3def-d6bd-4c0f-a351-9282f6ffe3a1" />


---

# 7. Contournement de la détection Root avec Objection

L’outil **Objection** permet d’automatiser plusieurs techniques de bypass root grâce à Frida.

Exemple de lancement :

```bash id="h2w5jc"
objection -g com.example.app explore
```

Puis :

```bash id="v6q9ap"
android root disable
```

Cette commande applique automatiquement différents hooks destinés à masquer les indicateurs de root.

L’application Android peut alors continuer son exécution sans détecter l’environnement rooté.

**Capture associée :**

<img width="1348" height="586" alt="image" src="https://github.com/user-attachments/assets/b5ae2c2a-b559-4b7e-9a45-e50dee8baa95" />


---

# 8. Exécution du module Root Bypass avec Medusa

Le framework **Medusa** est ensuite utilisé afin d’appliquer un module spécialisé de bypass root.

Exemple de commande :

```bash id="z7u3rd"
python medusa.py -p com.example.app -r universal_root_detection_bypass
```

Ce module utilise Frida afin d’instrumenter dynamiquement l’application Android et neutraliser certaines vérifications de sécurité.

Cette étape permet de comparer différentes approches de bypass root :

* scripts Frida personnalisés ;
* Objection ;
* Medusa.

**Capture associée :**

<img width="1246" height="742" alt="image" src="https://github.com/user-attachments/assets/bf564b38-f3ff-4da6-bdea-c9ee5f0f647f" />


---

# 9. Conclusion du laboratoire

Ce laboratoire a permis de :

* préparer un environnement Android d’analyse dynamique ;
* vérifier la communication entre ADB et l’émulateur Android ;
* démarrer et utiliser `frida-server` ;
* injecter des scripts Frida dans une application Android ;
* contourner plusieurs mécanismes de détection root ;
* utiliser différents outils d’instrumentation dynamique comme Frida, Objection et Medusa.

L’ensemble des manipulations a été réalisé dans un environnement pédagogique et contrôlé afin d’étudier les mécanismes de sécurité des applications mobiles Android.
