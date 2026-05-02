# Lab MobSF Dynamic Analysis avec DIVA Android

## 1. Introduction

Ce laboratoire a pour objectif de réaliser une analyse statique et dynamique d’une application Android vulnérable à l’aide de MobSF.

L’application utilisée est **DIVA** pour *Damn Insecure and Vulnerable App*. Il s’agit d’une application Android volontairement vulnérable conçue pour l’apprentissage de la sécurité mobile.

Dans ce lab, l’analyse est réalisée avec MobSF sur un émulateur Android. L’objectif est d’observer le comportement de l’application, d’identifier les vulnérabilités présentes dans DIVA et de comprendre les mauvaises pratiques de sécurité Android.

---

## 2. Objectifs du lab

Les objectifs principaux de ce laboratoire sont :

- Comprendre le fonctionnement de l’analyse dynamique Android.
- Utiliser MobSF pour analyser une application Android vulnérable.
- Importer et analyser l’APK DIVA.
- Réaliser une analyse statique de l’application.
- Lancer l’analyse dynamique avec MobSF.
- Tester les différents challenges proposés par DIVA.
- Identifier les secrets codés en dur.
- Détecter les problèmes de stockage local non sécurisé.
- Tester les problèmes de validation des entrées.
- Identifier les faiblesses liées au contrôle d’accès.
- Interpréter les résultats obtenus à partir des captures.

---

## 3. Environnement utilisé

| Élément | Description |
|---|---|
| Système hôte | Windows |
| Terminal | PowerShell |
| Émulateur | Android Emulator |
| Outil d’analyse | MobSF |
| Application analysée | DIVA APK |
| Package Android | `jakhar.aseem.diva` |
| Outils complémentaires | ADB, MobSF Dynamic Analyzer |

---

## 4. Préparation de l’environnement

Avant de commencer l’analyse avec MobSF, l’émulateur Android a été lancé puis vérifié avec ADB.

La commande `adb devices` montre que l’émulateur est bien détecté par la machine hôte. Dans ce lab, l’émulateur utilisé est identifié par `emulator-5554`.

<img width="1047" height="87" alt="01_adb_devices" src="https://github.com/user-attachments/assets/014530e2-ead8-4866-9406-d7a282be8e25" />


Cette étape est importante car MobSF doit pouvoir communiquer avec l’émulateur pour installer l’application et lancer l’analyse dynamique.

---

## 5. Accès à MobSF

MobSF a été lancé puis ouvert depuis le navigateur web.

L’accès à l’interface se fait avec les identifiants par défaut :

| Champ | Valeur |
|---|---|
| Username | `mobsf` |
| Password | `mobsf` |

<p align="center">
  <img width="832" height="448" alt="02_mobsf_login" src="https://github.com/user-attachments/assets/e1b89a20-233e-4443-9220-5a697f5653cc" />
</p>

Après la connexion, MobSF permet d’importer l’APK à analyser et d’accéder aux modules d’analyse statique et dynamique.

---

## 6. Analyse statique de DIVA

L’APK DIVA a été importé dans MobSF. Après l’importation, MobSF a généré automatiquement un rapport d’analyse statique.

Ce rapport contient plusieurs informations importantes :

- le nom de l’application ;
- le nom du package ;
- l’activité principale ;
- les versions SDK ;
- le score de sécurité ;
- les composants exportés ;
- les informations du fichier APK ;
- les permissions et éléments déclarés dans le Manifest.

<p align="center">
  <img width="647" height="530" alt="03_static_analysis_dashboard" src="https://github.com/user-attachments/assets/e19cb66e-7a32-48f2-a8e3-4e899148713e" />
</p>

L’analyse statique montre que l’application possède plusieurs composants Android déclarés. Certains composants sont exportés, ce qui peut représenter un risque si aucun contrôle d’accès n’est appliqué.

Exemple d’informations observées :

| Élément | Résultat observé |
|---|---|
| Exported Activities | 2 / 17 |
| Exported Services | 0 / 0 |
| Exported Receivers | 0 / 0 |
| Exported Providers | 1 / 1 |

---

## 7. Démarrage de l’analyse dynamique

Après l’analyse statique, l’analyse dynamique a été lancée depuis MobSF.

MobSF affiche l’application DIVA dans la liste des applications disponibles. L’option `Start Dynamic Analysis` permet de lancer l’analyse runtime dans l’émulateur.

<p align="center">
  <img width="687" height="218" alt="04_start_dynamic_analysis" src="https://github.com/user-attachments/assets/7a46bd66-c47c-4f06-834b-3411bb7e5471" />
</p>

Cette étape permet de passer de l’analyse statique à l’observation du comportement réel de l’application pendant son exécution.

---

## 8. Interface Dynamic Analyzer

L’interface Dynamic Analyzer de MobSF fournit plusieurs fonctionnalités utiles pendant l’analyse dynamique.

Parmi les fonctionnalités disponibles, on trouve :

- affichage de l’écran de l’émulateur ;
- test TLS/SSL ;
- test des activités exportées ;
- test des activités de l’application ;
- capture d’écran ;
- Logcat ;
- génération du rapport final.

<p align="center">
  <img width="686" height="635" alt="05_dynamic_analyzer_interface" src="https://github.com/user-attachments/assets/0ee56bc9-584d-4cfa-b4a4-cd60ec9a7899" />
</p>

Cette interface centralise les outils nécessaires pour observer et tester l’application Android en temps réel.

---

## 9. Menu principal de DIVA

Après le lancement de l’application, le menu principal de DIVA affiche plusieurs challenges de sécurité Android.

Les challenges visibles sont :

1. Insecure Logging
2. Hardcoding Issues - Part 1
3. Insecure Data Storage - Part 1
4. Insecure Data Storage - Part 2
5. Insecure Data Storage - Part 3
6. Insecure Data Storage - Part 4
7. Input Validation Issues - Part 1
8. Input Validation Issues - Part 2
9. Access Control Issues - Part 1
10. Access Control Issues - Part 2
11. Access Control Issues - Part 3
12. Hardcoding Issues - Part 2
13. Input Validation Issues - Part 3

<p align="center">
  <img width="320" alt="06_diva_main_menu" src="https://github.com/user-attachments/assets/c3f1f789-e308-4ca3-b2de-a5ae20947093" />

</p>


Les tests suivants sont présentés selon l’ordre du menu de l’application DIVA.

---

## 10. Test : Hardcoding Issues - Part 1

Le challenge `Hardcoding Issues - Part 1` permet de tester la présence de secrets codés en dur dans l’application.

Une valeur secrète a été saisie dans le champ prévu. L’application a accepté la valeur `vendorsecretkey`, ce qui montre que cette information est présente dans l’application.

<p align="center">
 <img width="320" alt="07_hardcoding_part1" src="https://github.com/user-attachments/assets/905fd91c-41fc-4f33-b5c8-f1faa52f376b" />

</p>


### Observation

L’application affiche un message indiquant que l’accès est accordé.

### Analyse

Cette vulnérabilité montre qu’un secret est intégré directement dans l’application. Un attaquant peut extraire l’APK, analyser le code ou les chaînes de caractères, puis retrouver cette valeur.

### Risque

Les secrets codés en dur peuvent exposer :

- des mots de passe ;
- des clés API ;
- des tokens ;
- des identifiants internes ;
- des informations confidentielles.

---

## 11. Test : Insecure Data Storage - Part 1

Le challenge `Insecure Data Storage - Part 1` teste le stockage local des identifiants.

Un nom d’utilisateur et un mot de passe ont été saisis dans l’application, puis enregistrés.

<p align="center">
  <img width="320" alt="08_insecure_storage_part1" src="https://github.com/user-attachments/assets/81cdbb8c-48ab-4289-a223-33ffc49e2dcb" />

</p>


Après l’enregistrement, les données ont été retrouvées dans un fichier XML situé dans les préférences partagées de l’application.

<p align="center">
  <img width="1047" height="200" alt="09_shared_prefs_cleartext" src="https://github.com/user-attachments/assets/c7e60fc7-5379-4445-a2d7-d3c374b3fa98" />

</p>


### Observation

Le fichier contient les valeurs suivantes en clair :

```xml
<string name="password">test123</string>
<string name="user">test</string>
```

### Analyse

L’application stocke les identifiants dans `SharedPreferences` sans chiffrement. Ce comportement est dangereux car les données peuvent être récupérées si un attaquant accède au système de fichiers de l’application.

### Risque

Le stockage en clair peut provoquer :

- la fuite d’identifiants ;
- la récupération de mots de passe ;
- l’exposition de données sensibles ;
- l’exploitation en cas d’appareil compromis ou rooté.

---

## 12. Test : Insecure Data Storage - Part 2

Le challenge `Insecure Data Storage - Part 2` teste le stockage dans une base de données SQLite.

Des identifiants ont été saisis puis sauvegardés dans l’application.

<p align="center">
  <img width="320" alt="10_insecure_storage_part2" src="https://github.com/user-attachments/assets/e86f02fa-03b8-42e6-b7ce-4dc103f19e14" />

</p>


Les données ont ensuite été retrouvées dans la base SQLite de l’application.

<p align="center">
  <img width="1047" height="100"  alt="11_sqlite_cleartext" src="https://github.com/user-attachments/assets/f26238f5-90d4-456d-98df-7469a7f48831" />

</p>


### Observation

La requête SQLite affiche les données sauvegardées en clair.

Exemple observé :

```text
test2|test123
```

### Analyse

Les données sensibles sont stockées dans une base SQLite sans chiffrement. SQLite est un mécanisme courant de stockage local, mais il ne protège pas automatiquement les données.

### Risque

Un attaquant peut extraire la base de données et consulter son contenu hors ligne.

### Recommandation

Il est recommandé de :

- ne pas stocker les mots de passe en clair ;
- chiffrer les données sensibles ;
- limiter les informations sauvegardées localement ;
- utiliser un mécanisme de stockage sécurisé.

---

## 13. Test : Insecure Data Storage - Part 3

Le challenge `Insecure Data Storage - Part 3` montre un autre cas de stockage local non sécurisé.

Des identifiants ont été saisis puis enregistrés dans l’application.

<p align="center">
  <img width="320" alt="12_insecure_storage_part3" src="https://github.com/user-attachments/assets/e61ec2ec-c567-48c9-a7c8-457a1a002cf7" />

</p>


Une recherche dans le dossier de données de l’application permet de retrouver les informations en clair.

<p align="center">
  <img width="1047" height="100"  alt="13_temp_file_cleartext" src="https://github.com/user-attachments/assets/fa57790e-9c57-4fd6-a3ee-1a66ad7720c1" />

</p>


### Observation

Les données sont retrouvées dans un fichier temporaire situé dans le répertoire de l’application.

### Analyse

Même si le fichier n’est pas visible depuis l’interface utilisateur, il reste accessible dans le système de fichiers de l’application.

### Risque

Les fichiers temporaires peuvent contenir des données sensibles oubliées par les développeurs. Cela peut mener à une fuite d’informations.

---

## 14. Test : Insecure Data Storage - Part 4

Le challenge `Insecure Data Storage - Part 4` concerne le stockage externe.

Des identifiants ont été saisis dans l’application puis sauvegardés.

<p align="center">
  <img width="320" alt="14_insecure_storage_part4" src="https://github.com/user-attachments/assets/57a64a1f-6720-466d-afd5-5e786d1335aa" />


Les données ont été retrouvées dans un fichier situé dans le stockage externe de l’émulateur.

<p align="center">
  <img width="1047" height="100"  alt="15_external_storage_cleartext" src="https://github.com/user-attachments/assets/cd73dccb-7020-4759-b9aa-131b4c8fa73e" />

</p>


### Observation

Les informations sont stockées dans un fichier accessible depuis le stockage externe.

Exemple de chemin observé :

```text
/storage/emulated/0/.uinfo.txt
```

### Analyse

Le stockage externe est moins sécurisé que le stockage interne de l’application. Les fichiers présents dans cet espace peuvent être accessibles à d’autres applications ou à un utilisateur ayant accès au stockage.

### Risque

Le stockage externe ne doit pas être utilisé pour conserver des données sensibles comme :

- des identifiants ;
- des mots de passe ;
- des tokens ;
- des clés API ;
- des informations personnelles.

---

## 15. Test : Input Validation Issues - Part 1

Le challenge `Input Validation Issues - Part 1` permet de tester une injection SQL.

Une entrée de type injection a été saisie :

```text
' OR '1'='1
```

<p align="center">
  <img width="320" alt="16_input_validation_sql_injection" src="https://github.com/user-attachments/assets/b04acc58-2bbf-4253-83f8-b5af83b73563" />

</p>


### Observation

L’application retourne plusieurs utilisateurs avec leurs mots de passe et leurs numéros de carte.

### Analyse

Cela montre que l’entrée utilisateur n’est pas correctement filtrée avant d’être utilisée dans une requête SQL.

### Risque

Une injection SQL peut permettre à un attaquant de :

- contourner une authentification ;
- afficher des données confidentielles ;
- modifier des données ;
- supprimer des données ;
- compromettre la base locale.

### Recommandation

Il faut utiliser des requêtes paramétrées et valider strictement les entrées utilisateur.

---

## 16. Test : Input Validation Issues - Part 2

Le challenge `Input Validation Issues - Part 2` teste l’utilisation d’une entrée utilisateur pour charger une URL dans une WebView.

L’URL suivante a été saisie :

```text
https://example.com
```

<p align="center">
  <img width="320" alt="17_input_validation_webview" src="https://github.com/user-attachments/assets/2ea4572a-9d09-47fa-b6b9-9d51250470ab" />

</p>


### Observation

L’application charge le contenu de la page demandée dans une WebView.

### Analyse

Si l’application accepte n’importe quelle URL sans validation, elle peut exposer l’utilisateur à du contenu externe non contrôlé.

### Risque

Une mauvaise validation des URL peut entraîner :

- l’ouverture de pages malveillantes ;
- l’exposition à du phishing ;
- l’exécution de contenu non fiable ;
- des attaques liées à WebView.

### Recommandation

Il faut limiter les domaines autorisés et contrôler strictement les entrées provenant de l’utilisateur.

---

## 17. Test : Access Control Issues - Part 1

Le challenge `Access Control Issues - Part 1` concerne l’accès à des identifiants API Vendor.

Après l’exécution du challenge, les identifiants API sont affichés par l’application.

<p align="center">
  <img width="320" alt="18_access_control_vendor_api" src="https://github.com/user-attachments/assets/2f4d5ba6-0778-4eb1-9b59-49eab7e8f27e" />

</p>


### Observation

Les informations suivantes sont visibles :

```text
API Key
API User name
API Password
```

### Analyse

L’application affiche des identifiants sensibles sans protection suffisante.

### Risque

Un attaquant pourrait récupérer ces informations et les utiliser pour accéder à une API ou à un service externe.

### Recommandation

Les identifiants API ne doivent pas être exposés directement dans l’application cliente. Ils doivent être protégés côté serveur ou gérés par un mécanisme sécurisé.

---

## 18. Test : Access Control Issues - Part 2

Le challenge `Access Control Issues - Part 2` concerne l’accès à des identifiants API Tweeter.

Après l’exécution du challenge, les identifiants API sont affichés par l’application.

<p align="center">
  <img width="320" alt="19_access_control_tweeter_api" src="https://github.com/user-attachments/assets/5f508a4b-e6b0-4994-bd69-e5ee72779c9e" />

</p>


### Observation

L’écran affiche :

```text
API Key
API User name
API Password
```

### Analyse

Ces données ne devraient pas être exposées directement dans l’application. Cela montre une faiblesse de contrôle d’accès et une mauvaise gestion des secrets.

### Risque

L’exposition de clés API ou d’identifiants peut permettre :

- l’usurpation d’identité applicative ;
- l’utilisation abusive d’un service ;
- la fuite d’informations ;
- des accès non autorisés.

---

## 19. Test : Access Control Issues - Part 3

Le challenge `Access Control Issues - Part 3` concerne l’accès à des notes privées protégées par un PIN.

Après la création ou l’utilisation du PIN, les notes privées deviennent accessibles.

<p align="center">
  <img width="320" alt="20_access_control_private_notes" src="https://github.com/user-attachments/assets/4fefb4c1-6a2d-4c30-9956-77f18d66238c" />

</p>


### Observation

Des notes privées sont affichées dans l’application.

### Analyse

Ces informations ne devraient être accessibles qu’après un contrôle d’accès correct. Le fait qu’elles soient visibles montre que l’application ne protège pas suffisamment certaines données sensibles.

### Risque

Une mauvaise gestion du contrôle d’accès peut entraîner :

- la fuite d’informations privées ;
- l’accès à des données confidentielles ;
- le contournement de la logique normale de l’application.

---

## 20. Test : Hardcoding Issues - Part 2

Le challenge `Hardcoding Issues - Part 2` concerne également une valeur codée en dur.

Une valeur a été saisie dans l’application et l’accès a été accordé.

<p align="center">
  <img width="320" alt="21_hardcoding_part2" src="https://github.com/user-attachments/assets/108d6440-1b39-4cbd-bf61-6bc0f1b23d62" />

</p>


### Observation

L’application affiche le message :

```text
Access granted! See you on the other side :)
```

### Analyse

Cela confirme que l’application contient une logique de validation basée sur une valeur présente dans le code ou dans une partie accessible de l’application.

### Risque

Ce type de protection est faible car il peut être contourné par reverse engineering.

### Recommandation

Les secrets ne doivent jamais être stockés directement dans le code source ou dans l’APK.

---

## 21. Test : Input Validation Issues - Part 3

Le challenge `Input Validation Issues - Part 3` teste le comportement de l’application face à une entrée anormale ou trop longue.

Une longue chaîne de caractères a été saisie dans le champ de l’application.

<p align="center">
  <img width="320" alt="22_input_validation_part3" src="https://github.com/user-attachments/assets/e181ac20-e0a9-4e0f-8733-7ed4ee0dc7c1" />
</p>


### Observation

L’application accepte une entrée longue avant l’exécution de l’action.

### Analyse

Ce type de test permet de vérifier si l’application valide correctement la taille et le contenu des entrées utilisateur.

### Risque

Une mauvaise validation peut provoquer :

- un crash de l’application ;
- un comportement inattendu ;
- une instabilité ;
- une possible exploitation si le traitement interne est vulnérable.

### Recommandation

L’application doit contrôler la longueur, le format et le type des entrées avant de les traiter.

---

## 22. Synthèse des vulnérabilités observées

| Catégorie | Vulnérabilité observée | Preuve |
|---|---|---|
| Hardcoding | Secrets codés en dur dans l’application | Accès accordé avec des valeurs spécifiques |
| SharedPreferences | Données stockées en clair | Fichier XML contenant user/password |
| SQLite | Données sensibles stockées en clair | Requête SQLite affichant les identifiants |
| Fichiers temporaires | Informations sensibles dans un fichier temporaire | Recherche dans `/data/data` |
| Stockage externe | Données sensibles dans le stockage externe | Fichier lisible dans `/storage/emulated/0` |
| Input Validation | Injection SQL réussie | Affichage de plusieurs utilisateurs |
| WebView | Chargement d’une URL externe | Page web affichée dans l’application |
| Access Control | Identifiants API exposés | Vendor/Tweeter API Credentials |
| Données privées | Notes privées accessibles | Affichage de Diva Private Notes |

---

## 23. Conclusion

Ce laboratoire a permis de réaliser une analyse de l’application DIVA avec MobSF.

L’analyse statique a permis d’identifier les informations générales de l’application, ses composants Android et certains risques potentiels. L’analyse dynamique a ensuite permis d’observer le comportement réel de l’application pendant son exécution.

Les tests réalisés ont mis en évidence plusieurs vulnérabilités Android classiques, notamment les secrets codés en dur, le stockage local non sécurisé, l’exposition de données dans SQLite, le stockage externe non protégé, les problèmes de validation des entrées et les faiblesses de contrôle d’accès.

Ce lab montre l’importance d’utiliser des outils comme MobSF pour analyser les applications Android et identifier les mauvaises pratiques de sécurité avant leur mise en production.

---

## 24. Auteure

Réalisé par : Ikram Laabouki

Module : Sécurité des Applications Mobiles

Établissement : ENSA Marrakech
