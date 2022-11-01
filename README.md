# Stratégie de Sécurisation d'une application Web (Mission Locale)

# Sommaire

- <a href="#introduction">Introduction</a>
- <a href="#defense_profondeur">Défense en profondeur</a>
- <a href="#reduction_surface">Réduction surface d'attaque</a>
- <a href="#moindre_privileges">Moindre Privilèges </a>
- <a href="#rbac">RBAC</a>
- <a href="#secure_protocols">Protocoles Sécurisés</a>
    - <a href="#https">HTTPS</a>
    - <a href="#tls">TLS </a>
    - <a href="#hsts">HSTS </a>
    - <a href="#hsts_preload">HSTS Preload </a>
- <a href="#nav_secu">Sécurité Navigateur </a>
    - <a href="#sop">SOP </a>
    - <a href="#cors">CORS </a>
    - <a href="#csp">CSP </a>
    - <a href="#sri">SRI </a>
    - <a href="#sanitisation">Sanitisation </a>
- <a href="#menaces_rependues">Menaces les plus répandues </a>
    - <a href="#xss">XSS </a>
        - <a href="#clickjacking">Clickjacking </a>
        - <a href="#quiet_requests">Requêtes silencieuses </a>
    - <a href="#csrf">CSRF </a>
    - <a href="#sqli">SQLi </a>
        - <a href="#orm">ORM </a>
    - <a href="#waterholing">WaterHoling (Point d'eau) </a>
- <a href="#password_policy">Politique des mots de passe </a>
- <a href="#auth_security">Sécurisation Authentification </a>
    - <a href="#cookie">Cookie </a>
    - <a href="#token">Token </a>
    - <a href="#jwt">JWT </a>
    - <a href="#session">Session </a>
    - <a href="#uuid">UUID </a>
- <a href="#api_security">Sécurisation API </a>
- <a href="#journalisation">Journalisation </a>
- <a href="#sha256">SHA256 </a>
    - <a href="#hashage">Hashage </a>
    - <a href="#salage">Salage </a>
- <a href="#save_strat">Stratégie de Sauvegarde </a>
- <a href="#rgpd">RGPD </a>
- <a href="#bug_bounty">Bug Bounty</a>
- <a href="#audit">Audit</a>
- <a href="#sources">Sources</a>

# Introduction <a id='introduction'></a>

This present documentation is a securisation strategy for the Mission Locale project,<br>
we will see what's adapted strategy that i chosed to secure this Web Site.<br>

# Défense en profondeur <a id='defense_profondeur'></a>

Le principe de défense en profondeur consiste simplement à ne pas protéger une seul point d'entrée<br>
mais bien l'ensemble des éléments consistuant un système ou une application afin que chacun<br>
de ces éléments participent activement à la sécurité générale du système ou de l'application.<br>

# Réduction de la surface d'attaque<a id='reduction_surface'></a>

La réduction de la surface d'attaque est un principe de sécurité selon lequel toutes les parties d'une application <br>
ou d'un système non necessairement exposables seront isolées.<br>
C'est à dire que chaque partie du système ou de l'application dont l'exposition peut être réduite doit l'être.<br>

Dans le cadre de ce projet, l'exposition réseau pourrait se limiter à : <br>

- Port 80 : Port utilisé pour l'échange d'information en HTTPS
- Port 443 : Port utilisé pour l'échange d'information en HTTPS
- Port 20 : Port utilisé pour l'échange de fichier en FTP
- Port 21 : Port utilisé pour l'échange de fichier en FTP
- Port 22 : Port utilisé pour le protocol SSH

# Moidre privilèges<a id='moindre_privileges'></a>

Le principe de moindre privilèges consiste à restreindre les permissions d'un élément aux permissions <br>
strictement nécessaires à son bon fonctionnement.<br>
Par exemple, si un composant doit simplement lire un flux de donnée, nous autoriserons ce composant à lire mais sans lui permettre d'écrire dans les flux qu'il sera amené à lire.<br>

# RBAC (Role Based Access Control)<a id='rbac'></a>

La mise en place d'une stratégie basée sur RBAC (Controle d'Accès Basé sur les Rôles)<br>
découle directement du principe de moindre privilèges, en effet grace à la stratégie RBAC,<br>
il est possible de définir des rôles afin d'octroyer certaines permissions à certains utilisateur.<br>

Dans le cadre de ce projet, les rôles suivants seront potentiellement mis en place :<br>

- Visiteur : 
    - Peut visiter le site
- Jeune :
    - Toutes permissions des rôles inférieurs
    - Demander un rendez-vous à son référent
- Rédacteur :
    - Toutes permissions des rôles inférieurs
    - Apporter des suggestions de modification aux différents contenus du site
    - Ajouter des contenus au site par le biais d'accord préalable d'un membre de l'équipe de communication
- Community Manager : 
    - Toutes permissions des rôles inférieurs
    - Approuver l'ajout de contenu au site.
    - Suppression de contenu du site.
- Administrateur : 
    - Toutes permissiosn des rôles inférieurs
    - Accès à un panel d'administration complet du site

# Protocoles Sécurisés<a id='secure_protocols'></a>

# HTTPS<a id='https'></a>

Le protocol HTTPS est une version améliorée du protocol HTTP en terme de sécurité.<br>
HTTPS signifie HyperText Transfer Protcol Secure ou en français Protocol Sécurisé de Transfert d'HyperText.<br>
Ce protocol se repose en réalité sur un autre protocol afin de sécuriser l'échange de données, le protocol TLS.<br>
Dans le cadre de ce projet, le protocol que nous utiliserons sera le HTTPS<br>

# TLS <a id='tls'></a>

Le protocol TLS est un protocol visant à encapsuler l'information<br> 
et à la chiffrer par le biais d'opérations cryptographiques.<br>
TLS signifie Transport Layer Security ou en français Sécurité de la Couche de Transport.<br>
TLS permet de chiffrer les données émises dans un contexte d'échange type Client -> Serveur afin d'empêcher la compromission des données transitantes sur la couche réseau.<br><br>
Dans le cadre de l'utilisation de TLS le serveur est nécessairement authentifié, alors qu'il existe certaines fonctions permettant l'identification du client si besoin est.<br><br>
Notons aussi que TLS est l'une des solutions préférées dans la protection de flux réseau.<br>
Lors de l'utilsation de TLS, les messages sont généralement transmis par l'intermediaire du protocol TCP.<br>
<a href="https://www.ssi.gouv.fr/uploads/2020/03/anssi-guide-recommandations_de_securite_relatives_a_tls-v1.2.pdf">Afin d'en apprendre beaucoup plus sur TLS</a>

# HSTS<a id='hsts'></a>

HSTS est un méchanisme visant à effectuer des redirections de requêtes utilisant le protocol HTTP vers un protocol sécurisé tel que HTTPS afin d'éviter la communication par le biais d'un protocol non sécurisé tel qu'HTTP.<br><br>
HSTS signifie HTTP Strict Transport Security.<br>
Cependant en cas d'utilisation d'HSTS il est fortement recommandé de vérifier les dates d'expirations des certificats SSL/TLS fournissant l'accès au protocol HTTPS, en effet, en cas d'oubli de renouvellement du certificat, l'utilisation d'HSTS empêchera les visiteurs de consulter le site.<br><br>
Aussi il existe une vulnérabilité lors de la première visite d'un site utilisant HSTS, en effet, HSTS ne prends pas en compte la première visite d'un utilisateur, c'est à dire qu'il existe un risque minime qu'un utilisateur soit victime d'une attaque par MITM, il existe cependant un moyen de rémédier à cette vulnérabilité.<br>

Dans le cadre du projet de la Mission Locale ce mechanisme sera mis en place.<br>

# HSTS Preload<a id='hsts_preload'></a>

HSTS Preload est enfaite un registre contenant une liste de domaine,<br> 
permettant aux navigateurs d'enregistrer les domaines au HSTS préalablement à la première visite d'un utilisateur<br>
de ce fait, tout domaine enregistré dans le Preload est garanti d'être connu du navigateur<br>
et par la même occasion de ne plus souffrir de cette vulnérabilité lors de la première visite d'un utilisateur.<br>

Dans le cadre du projet de la Mission Locale, le domaine sera enregistré au Preload.<br>

# Sécurité côté Navigateur<a id='nav_secu'></a>

# SOP (Same Origin Policy)<a id='sop'></a>

La Politique de Même Origine est une sécurité mise en place par défaut par le navigateur,<br>
cette politique permet de définir que l'origine sur laquelle vous vous trouvez sera en quelque sorte cloisonnée,<br>
et sera hermétique aux autres origines se trouvant sur le Web,<br>
avec SOP votre origine ne pourra pas échanger de ressources
avec les autres origines se trouvant sur le Web, il existe cependant quelques exceptions telles que les images et les iframes.<br>

Dans le cadre de ce projet, SOP ne sera pas utilisé puis-ce qu'il semble nécessaire d'échanger des ressources avec les partenaires de la Mission Locale.<br>

# CORS (Cross Origin Ressources Sharing)<a id='cors'></a>

Cross Origin Ressources Sharing (CORS) ou Partage de Ressources Inter-Origin est une sécurité permettant d'échanger des ressources<br>
entre différentes origines, de cette façons, il sera possible de récupérer des informationss sur des sites partenaires, cependant grace à CORS il est possible d'établir une liste de domaine avec lesquels échanger des informations.<br>
Pour qu'un échange de donnée entre origines puisse avoir lieu avec CORS, il est nécessaire que les 2 origines acceptent l'échange l'un avec l'autre, sans quoi l'échange de donnée n'aura pas lieu.<br>

Dans le cadre du projet de la Mission Locale, CORS sera mis en place afin d'échanger des contenus provennant de partenaires.<br>

# CSP (Content Security Policy)<a id='csp'></a>

La Politique de Sécurité du Contenu (CSP) est une politique mise en place par le navigateur,<br>
grace à CSP il est possible de dresser une liste de ressources dont l'exécution sera authoriser,<br>
à l'inverse si une ressource importée d'un autre domaine par le biais de CORS ne se trouve pas dans la liste de CSP,<br>
cette dernière verrait son importation ou son exécution bloquée par le navigateur afin de prévenir l'injection de code par exemple.<br>
De ce fait, CSP permet d'endiguer en partie les failles de type XSS, cependant, CSP ne représente pas une contre-mesure suffisante à ce type de faille et ne doit donc pas se substituer à de bonnes pratiques en matière de développement.<br>

Dans le cadre de ce projet, CSP sera mis en place et configuré de façons à être le plus sécurisant possible.<br>

# SRI (SubRessources Integrity)<a id='sri'></a>

SRI est un système de vérification de l'Intégrité des Sous-Ressources, c'est à dire que grace à SRI il est possible de vérifier si une ressource est intègre ou non.<br>

Lors ce que nous allons importer des ressources, il peut-être intéressant d'avoir un moyen de vérifier que les dites ressources correspondent effectivement à ce pour quoi elles ont étées prévues.<br>
Par exemple, lors d'import de librairies externes telles que Bootstrap, il est utile de vérifier que la librairie bootstrap importée correspond effectivement à ce dont elle doit correspondre.<br>
La vérification de l'intégrité d'une ressource se fait par le biais du Hashage, le principe est simple :<br>

Les développeurs de Bootstrap développent une nouvelle version.<br>
Ils hashent le fichier bootsrap.min.css, ils récupérent leur version du code sous forme de hash.<br>
Ils postent la nouvelle version de boostrap.min.css puis postent le hash.<br>
Lors de l'import de bootsrap un développeur importe la nouvelle version afin de l'utiliser sur son site.<br>
Et par la même occasion se munit du hash de cette dernière afin de vérifier que le code correspond bien au hash.<br>
Si c'est le cas, pas de soucis, la version est bonne et n'a subie aucune modification, sinon, la ressource ne correspond pas
au contenu importé, on n'utilisera pas cette ressource puis-ce que potentiellement dangereuse.<br>

Dans le cadre de ce projet, SRI sera utilisé afin de controler l'intégrité des librairies externes que nous utiliserons.<br>

# Sanitisation<a id='sanitisation'></a>

La sanitisation est une solution permettant de nettoyer la donnée entrée par l'utilisateur, afin de ne pas permettre à un utilisateur d'entrer de chiffre dans un champ prévu pour un nom par exemple.<br>
Il existe une sanitisation côté front-end et côté back-end, la solution de sanitisation côté front-end est un premier rempart mais trop peu suffisant, avec la sanitisation côté front-end, il est possible de nettoyer l'information entrée par un utilisateur lambda, mais pour quelqu'un d'experimenté, il est assez aisé de supprimer la sanitisation côte front-end.<br>
C'est pourquoi la sanitisation côté back-end doit être mise en place, afin d'être assuré que les informations entrées sont conformes aux attentes et seront traitables.<br>

La sanitisation est un point essentiel, c'est pourquoi elle sera mise en place dans ce projet.<br>

# Menaces les plus répandues <a id='menaces_rependues'></a>

# La faille XSS (Cross Site Scripting)<a id='xss'></a>

La faille XSS est une faille majeure, l'utilisation de faille XSS consiste à injecter du code malveillant d'un site vers un autre afin de récupérer les informations de connexion de l'utilisateur par exemple.<br>
L'utilisation de faille XSS peut-être dévastatrice et de nombreuses pratiques frauduleuses peuvent en découler.<br>
Prenons un exemple d'utilisation de faille XSS: <br>

Un utilisateur nommé Gérard reçoit un mail provenant de sa banque lui indiquant un besoin de rectification,<br>
Gérard clique alors sur ce lien, malheureusement, il est déjà trop tard.<br>
L'emetteur de ce mail frauduleux avait glissé du code malveillant dans ce lien par le biais d'une faille XSS inconnue sur le site de la banque de Gérard, l'attaquant a alors été en mesure de faire exécuter du code malveillant au site de la banque de Gérard, l'attaquant à redirigé Gérard par le biais d'un lien récupérant ses cookies de connexion à sa banque, l'attaquant a récupéré le cookie de connexion de Gérard et a maintenant la possibilité de se connecter au compte de ce dernier.<br>

La plupart du temps, les attaques XSS si elles sont bien faites ne sont même pas visibles par la vicitime.<br>

Il existe des possibilités infinies quant à l'exploitation de faille XSS telles que :

- <a id='quiet_requests'>Requêtes silencieuses</a><br>
    Les requêtes silencieuses sont un moyen d'appeler certaines pages sur le Web sans qu'un utilisateur s'en apercoivent.<br>
    Exemple d'exploitation de requêtes silencieuses, nous reprendrons notre ami Gérard :<br>
        L'attaquant à placer du code effectuant une requête silencieuse afin de demander à une page de tracage d'effectuer un tracage sur la page actuelle,<br> la page récupére alors le cookie de connexion de Gérard à sa banque et l'attaquant n'a plus qu'à récupérer ce cookie,<br> Gérard ne s'est rendu compte de rien, la requête étant silencieuse, <br>il peut continuer sa navigation sans le moindre soucis mais surtout sans s'être rendu compte qu'il venait de donner l'accès à son compte bancaire à un tiers.

- <a id='clickjacking'>Le clickjacking</a><br>
    Le clickjacking est une pratique consistant à remplacer le contenu d'un site Web légitime par un contenu frauduleux.<br>
    Exemple d'exploitation de clickjacking, nous reprendrons notre ami Gérard :<br>
        Gérard est assez chanceux, sa banque à mis en place un système d'authentification par JWT et donc l'attaquant n'a qu'une partie de son jeton d'authentification, il n'ira pas bien loin avec ceci.<br>
        Cependant, l'attaquant à placer du code malveillant afin de remplacer le formulaire de connexion au site de la banque de Gérard ressemblant trait pour trait,<br> Gérard entre ses informations de connexion et les valides, le formulaire envoi un mail à l'attaquant avec les informations de connexion de Gérard et Gérard et reconduit sur la page de son compte en banque.

Il est très important de développer avec précaution afin d'éviter les failles de type XSS (Cette faille mériterait elle aussi une documentation à part entière).<br>

# L'attaque par CSRF (Cross Site Request Forgery)<a id='csrf'></a>

L'attaque par CSRF ou Forge de Requête par site Interposés est un type d'attaque semblable à une attaque par XSS, le but de l'attaque par CSRF est enfaite d'exécuter des actions sur un site Web légitime à l'insu de l'utilisateur, poster un message sur un forum par exemple, il est aussi possible de récupérer les informations de connexion de l'utilisateur par le biais d'attaque CSRF.<br>

# La faille SQLi (Injection SQL)<a id='sqli'></a>

La faille SQLi (pour SQL Injection) est un type de faille permettant d'exécuter des requêtes SQL non préevues.<br>
Prenons un exemple d'exploitation de faille SQLi :<br>
Un attaquant se trouve sur une page de connexion d'un site Web vulnérable à l'injection SQL<br>
Il tape un nom d'utilisateur aléatoire et tape un mot de passe tel que celui-ci : ```SQL ' OR 1=1 ``` .<br>
Le serveur voit simplement le mot de passe et l'interprete comme suit : ```.. WHERE USERAME='nom utilisateur bidon' AND PASSWORD='' OR 1=1 ```.<br>
Le serveur exécute la requête SQL et donne l'accès au premier compte contenu dans la base de donnée, l'attaquant est donc connecté sur le compte d'un utilisateur qui est dans 99% des cas le compte un compte à privilèges.<br>

Il existe des déclinaisons à l'injection SQL, comme la suppression de toutes les données contenues de la base de données par l'interposition d'un ';' afin de faire exécuter toutes les requêtes envisageables à la base de donnée.<br>
Afin de palier à ce type de faille, il existe des outils tels que des ORM permettant d'endiguer ce type de faille.<br>

# ORM (Object Relational Mapping)<a id='orm'></a>

Un ORM peut permettre de préparer des paternes de requêtes et de préparer des requêtes lors de leur exécution en base de données, ce qui permet d'empêcher l'insertion de caractère d'échappement dans les requêtes, de cette façons, chaque informations sera écrite AS DATA et non plus AS LITTERAL et ne permettra plus l'exécution de code SQL arbitraire.<br>
Dans le pire des cas avec un ORM bien configuré mais sans la gestion d'erreur relative, la réponse de la base de donnée sera une succession d'erreur et le script plantera.<br>

Dans le cadre de ce projet, un ORM sera utilisé afin d'éviter au maximum les attaques par SQLi et de ce fait endiguer la compromission des données utilisateurs.<br>

# Principe du point d'eau<a id='waterholing'></a>

Le principe d'attaque par point d'eau consiste à infecter des machines par voie de RAT (Remote Admin Tool / Remote Access Trojan) par le biais de failles diverses telles que peuvent l'être des failles XSS dans le cadre d'un point d'eau dont l'infiltration se ferait pas le Web.<br>
Le but du point d'eau est d'attendre qu'une proie morde à l'hameçon afin de l'infecter et d'infecter tout son entourage numérique par le biais de diverses moyens, mails, infiltration au niveau du réseau, le but étant bien evidemment d'infecter un maximum de machines afin de récupérer de l'information ou bien simplement d'ériger un BotNet, il existe tout un tas de déclinaison d'utilisation de point d'eau, de façons de faire, de raison d'utilisation.<br>

# Politique des mots de passe<a id='password_policy'></a>

Une politique de mot de passe est un ensemble de recommandations à suivre afin d'établir une certaines sécurié concernant les mots de passe possiblement entrés par les utilisateurs.<br>
Dans le cadre de cette strategie, la politique des mots de passes sera la suivante : 

- Une longueur comprise entre 8 et 80 caractères.
- Une composition (A-z, 0-9, caractères spéciaux sans exception) des mots de passe.
- La vérification de la robustesse du mot de passe entré en temps réel par la vérification des facteurs ci-dessus.
- Un temps d'attente de 2 minutes par tranche de 3 tentatives de connexions échouées.
- Une stratégie de recouvrement par le biais d'un lien envoyé par mail afin de regénérer un mot de passe.
 
# Sécurisation de l'authentification<a id='auth_security'></a>

# Cookies<a id='cookie'></a>

Les cookies sont connotés de façon négatives par la plupart des utilisateurs car souvent présentés sous-forme de pop-ups génantes, demandant à l'utilisateur l'autorisation de sauvegarder les préférences utilisateurs.<br>
Cependant les cookies sont très utiles pour stocker des informations concernant le compte d'un utilisateur, afin de ne pas forcer l'utilisateur à se reconnecter au site à chaque visite.<br>
Cependant, il existe des problèmes au niveau de la sécurité concernant les cookies, en effet, les failles de type CSRF permettent la récupération des cookies contenues sur une page Web, c'est pourquoi il devient très déconseillé d'utiliser simplement les cookies en guise de preuve de confiance.<br>
Dans la plupart des cas où la sécurité prime, le cookie n'est enfaite qu'une partie du stockage de la session d'un utilisateur.<br>
Il existe des alternatives afin de ne pas rendre le compte d'un utilisateur complétement vulnérable en cas d'attaque par le biais de CSRF.<brs>

Dans le cadre de ce projet les cookies seront utilisés de façon partielle.<br>

# Token (Jeton)<a id='token'></a>

Le Token ou Jeton est enfaite un moyen d'authentifier un utilisateur au sein d'un système.<br>
Il existe 2 types de Token :

- Le hardware Token (Jeton Materiel)

Le jeton materiel est un jeton tangible permettant l'authentification d'un utilisateur sur un système, il peut-être représenté par une clé USB ou tout autre peripherique permettant le stockage d'informations.<br>

- Le software Token (Jeton Logiciel)

Le jeton logiciel quant à lui est un jeton non physique pouvant être stocké sur n'importe quel appareil tel qu'un ordinateur, un téléphone portable et bien d'autres encore.

# JWT (JSON Web Token)<a id='jwt'></a>

Le JWT est tout simplement un Token sous forme de JSON permettant l'authentification d'un utilisateur sur un site Web.<br>
De part sa structure, il est préférable à l'utilisation des cookies par exemple, en effet, il est possible de découper le Jeton JWT en plusieurs parties afin de répartir les différentes informations en plusieurs endroit, et c'est en ça que le JWT est préférable à la simple utilisation d'un cookie.<br>
Le cookie étant lui très vulnérable aux attaques CSRF, un attaquant pourrait facilement récupérer le cookie de connexion d'un utilisateur et usurper l'accès au compte récupérer.<br>
Avec le JWT ceci n'est plus possible si la mise en place du stockage du jeton suit quelques principes.<br>
La répartition du JWT dans les cookies et dans le localStorage.<br>
En effet, il est possible de séparer le payload du JWT et le JWT en lui même, de cette façon, on peut imaginer le scénario suivant, selon lequel le payload sera stocké dans le localStorage et le JWT dans les cookies, de façon à ce qu'un attaquant récupérant les cookies n'aura qu'une partie du Jeton mais pas l'autre et inversement.<br>

Dans le cadre de ce projet, le JWT sera utilisé en raison de son apport en terme de sécurité.<br>

# Les sessions<a id='session'></a>

Une session n'est ni plus ni moins régie par le temps d'expiration d'un cookie ou d'un token dans notre cas, une session va être utile dans le cas ou l'on veut qu'un utilisateur n'ait pas besoin de se reconnecter à notre site.<br>
Lors de la connexion d'un utilisateur, un jeton avec une date d'expiration lui est transmis, l'utilisateur pourra quitter et revenir sur le site sans avoir besoin de se connecter tant que son jeton n'a pas expiré, cependant, une fois le jeton révolu, l'utilisateur devra se ré-authentifier afin d'obtenir un nouveau jeton.<br>

Dans le cadre de ce projet, les sessions seront à temps limité, les sessions expireront après 1 heure.

# UUID (Unique User ID)<a id='uuid'></a>

Un UUID est un identifiant unique délivré par le serveur pour chaque utilisateur ayant besoin d'un enregistrement en base de donnée.<br>
A l'inverse d'un simple ID pouvant être composé de caractères numériques allant de 0 à 9, l'UUID se compose d'un ensemble de caractère litteraux et numérique comme suit : <strong>863cebfd-7875-45af-9e83-cd1e43aa1be4</strong> <br>
De cette façon, il n'est pas envisageable qu'un attaquant puisse faire appelle à la base de donnée afin d'en récupérer les informations par le biais du champ 'ID' puis-ce qu'il faudrait être incroyablement chanceux pour tomber sur un UUID identique.<br>

Dans le cadre de ce projet et en raison du niveau de sécurité qu'apporte la solution UUID comparativement à sa non-uitlisation, UUID sera utilisé.<br>

# Sécurisation de l'API (Application Programmation Interface)<a id='api_security'></a>

Dans le cadre de ce projet, la sécurisatibon de l'API se fera par les biais suivants :

- L'utilisation de JWT afin de restreindre l'accès à l'API aux utilisateurs possédant un Token
- L'utilisation du protocol sécurisé HTTPS afin de chiffrer les informations transitantes
- La restriction du nombre d'appel à l'API par tranche de temps, 60 appels possibles par utilisateur par tranche d'1 minute

# La journalisation<a id='journalisation'></a>

La journalisation est un système de rapport de tout événements survenus sur un système, de cette façons, l'analyse de bug, de tentative d'intrusion ou tout autres événements est rendu plus facile.<br>

Dans le cadre de ce projet, un système de journalisation sera mis en place.<br>

# Algoritme de chiffrement (SHA256)<a id='sha256'></a>

Un algoritme de chiffrement est utile lorsque l'on veut qu'une information ne soit pas découverte, on insère alors cette information dans un algoritme de chiffrement afin que cette information ne soit plus compréhensible en l'état.<br>
Dans le cadre de ce projet, l'algoritme de chiffrement retenu pour chiffrer les mots de passes des utilisateurs sera le SHA256.<br>

# Hashage<a id='hashage'></a>

Le SHA256 est un algoritme de chiffrement dit de "Hashage", lorsque l'on parle de hashage, on parle de méthodologie de chiffrement découpant l'information en une quantité de tronçons de même taille, chaque tronçon aura une influence sur le prochain tronçon jusqu'à ce qu'il n'y est plus de tronçon influencable, aussi, une information hashé par un algoritme tel que SHA256 aura toujours un ordre de taille donné : (32 bytes / octets) et (64 bytes / octets) en hexadécimal.<br>
Cependant, le hashage en lui même ne représente pas une sécurité suffisante en elle-même, puis-ce qu'un algoritme de chiffrement est une simple opération mathématique applicable en fonction de certains facteur.<br>
Pour palier à cette vulnérabilité, nous couplerons donc le Hashage avec le Salage.<br>

# Salage<a id='salage'></a>

Le salage est simplement le fait de rajouter une information (le sel) avant ou après l'information à hasher.<br>
Le sel peut être une phrase telle que 'Je suis le sel, et je suis utilisé dans le but de rendre indéchiffrable le hash auquel je vais être appliqué' sur laquelle un traitement aléatoire de mélange sera appliqué afin de rendre chaque sel unique et par la même occasion chaque empreinte de hashage différente.<br>

Dans le cadre de ce projet, nous utiliserons le SHA256 avec un sel généré aléatoirement.<br>

# Stratégie de sauvegarde<a id='save_strat'></a>

Une stratégie de sauvegarde est une stratégie selon laquelle les informations stockées en base de données seront sauvegardées à intervales régulier afin de prévenir le risque de perte d'informations en cas d'attaque type RansomWare ou tout autre type de problèmes.<br>

Dans le cadre de ce projet, une sauvegarde complète des données sera effectuée durant la nuit afin de s'assurer que les données ne soient pas perdues en cas de problème.<br>

# RGPD (Règlement Général sur la Protection des Données)<a id='rgpd'></a>

Le RGPD est un règlement en vigueur au sein de l'Union Européenne, grace au RGPD les utilisateurs dont les informations sont stockées sur un système afin d'assurer un service voient la protection de leurs données respectées.<br>

Le RGPD traitent certains sujets tels que les suivants :

- Un traitement de donnée (Récupération, Modification, Exctraction, etc) ne peut pas être effectué sans but précis.
- Les données d'un utilisateur ne peuvent pas être stockées indéfiniment, il faut que les données soient supprimées à un moment.
- Un utilisateur dont les données sont stockées sur un serveur doit avoir un droit de suppression, modification, regard sur ces dernières.

Dans le cadre de ce projet, le RGPD sera mis en oeuvre.<br>

# Bug Bounty <a id='bug_bounty'></a>

Le Bug Bounty est une pratique consistant à permettre à des hackers éthiques de venir tester la sécurité d'une application ou simplement de tester d'éventuels bugs sur votre système ou votre application, en échange d'une prime en fonction de la criticité des vulnérabilités trouvées.<br>

Dans le cadre de ce projet, un Bug Bounty est envisagé.<br>

# Audit (PASSI) (Prestataires d'Audit de la Sécurité des Systèmes d'Information)<a id='audit'></a>

Lorsque l'on parle d'audit PASSI, on parle d'une certification attestant que votre système correspond à des critères d'excellence.<br>

La qualification PASSI identifie 5 domaines d'intervention :

- Audit organisationnel et physique
- Audit d'architecture
- Audit de configuration
- Test d'intrusion sur un système 
- Audit de code

Il est donc possible de chercher à obtenir les 5 qualifications PASSI.<br>

Dans le cadre de ce projet, un Audit ne semble pas strictement nécessaire et n'est donc pas envisagé.<br>

# Sources<a id='sources'></a>

- <a href='https://www.ssi.gouv.fr/uploads/2013/05/anssi-guide-recommandations_mise_en_oeuvre_site_web_maitriser_standards_securite_cote_navigateur-v2.0.pdf'>Recommandation ANSSI sur la sécurité côté navigateur</a>
- <a href='https://developer.mozilla.org/fr/docs/Glossary/CSRF'>Attaque CSRF</a>
- <a href='https://www.base-de-donnees.com/orm/'>ORM</a>
- <a href='https://www.futura-sciences.com/tech/definitions/securite-attaque-point-eau-16369/'>WaterHoling</a>
- <a href='https://www.ssi.gouv.fr/guide/recommandations-relatives-a-lauthentification-multifacteur-et-aux-mots-de-passe/'>Politique des mots de passe</a>
- <a href='https://www.ionos.fr/digitalguide/sites-internet/developpement-web/json-web-token-jwt/'>JWT</a>
- <a href='https://www.cnil.fr/fr/la-cnil-publie-une-recommandation-relative-aux-mesures-de-journalisation'>Journalisation</a>
- <a href='https://www.cnil.fr/fr/rgpd-de-quoi-parle-t-on'>RGPD</a>
- <a href='https://www.activus-group.fr/news/359-cybersecurite.html'>Audit PASSI</a>