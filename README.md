# Stratégie de Sécurisation d'une application

# Table des matières

- Principes de sécurité
    - Défense en profondeur 
    - Réduction surface d'attaque 
    - Moindre privileges 
    - Politique sécurité des mots de passe 
- Protocoles
    - HTTP
        - Faille relatives : 
            - MITM (Man In The Middle)
    - HTTPS
        - TLS
        - SSL 
    - HSTS 
- Sécurité navigateur
    - SOP 
    - CORS 
    - CSP 
    - SRI 
    - Sanitisation 
    - Cookies 
    - Tokens 
    - Session 
    - Authentification 
        - Failles relatives :
            - XSS 
                - Clickjacking 
                - Requêtes silencieuses 
            - CSRF 
            - SQLI 
            - Point d'eau 
- Sécurité backend
    - RBAC 
    - DDOS (Deni de service) 
    - Autorisation 
    - Journalisation 
    - Sécurisation des mots de passe 
        - SHA256 
            - Hashage
            - Salage  
    - Sécurisation authentification 
    - Sécurisation API 
    - Stratégie de sauvegarde 
    - Strict mode 
    - Failles relatives : 
        - SSRF 
- Annexe
    - RGPD 
    - Audit 
    - Bug Bounty 

# Principes de sécurité

## Défense en profondeur

Le principe de défense en profondeur consiste simplement à ne pas protéger une seul point d'entrée<br>
mais bien l'ensemble des éléments consistuant un système ou une application afin que chacun<br>
de ces éléments participent activement à la sécurité générale du système ou de l'application.<br>

## Réduction de la surface d'attaque

La réduction de la surface d'attaque est un principe de sécurité selon lequel toutes les parties d'une application ou d'un système non necessairement exposables seront isolés.<br>
C'est à dire que chaque partie du système ou de l'application dont l'exposition peut-être réduite doit l'être.<br>
<br>
Par exemple : <br>
Lors-ce que nous développerons pour le Web, nous pourrions réduire la surface d'attaque en n'exposant que les ports<br>
80, 443, 20 ( ou 21) afin que le système ne soit exposés sur le réseau que par le biais de ces ports.

## Moidre privilèges

Le principe de moindre privilèges consiste à restreindre les permissions d'un élément aux permissions strictement nécessaire à son bon fonctionnement.<br>
Par exemple, si un composant doit simplement lire un flux d'informations, nous autoriserons ce composant à lire mais sans lui permettre d'écrire dans les flux qu'il sera amené à lire.<br>

## Politique des mots de passe

Une politique de mot de passe est un ensemble de recommandations à suivre afin d'établir une certaines sécurié concernant les mots de passe possiblement entrés par les utilisateurs.<br>
Dans le cadre de cette strategie, la politique des mots de passes sera la suivante : 

- Une longueur comprise entre 8 et 80 caractères.
- Une composition (A-z, 0-9, caractères spéciaux sans exception) des mots de passe.
- La vérification de la robustesse du mot de passe entré en temps réel par la vérification des facteurs ci-dessus.
- Un tente d'attente de 2 minutes par tranche de 3 tentatives de connexions échouées.
- Une stratégie de recouvrement par le biais d'un lien envoyé par mail afin de regénerer un mot de passe.
 
# Sécurité par les protocoles

## HTTP

Le protocol HTTP est un protocol d'échange de donnée s'insérant sur la couche réseau du modèle OSI.<br>
HTTP signifie HyperText Transfer Protocol ou en français Protocol de Transfert d'HyperText.<br>
Nous n'utiliserons pas ce protocol en raison de son manque de protection lros du transfer d'information.<br>
Ce protocol étant soumis à une faille de sécurité majeure (MITM) il sera préférable d'utiliser le protocol HTTPS.<br>

### MITM (Man In The Middle)

L'attaque dites de Man In The Middle ou literallement L'homme du millieu est une attaque qui consiste à se placer entre les deux machines communiquantes entre elles tel qu'un Client et un Serveur par exemple afin de récupérer les échanges d'informations émises et reçues.<br><br>
Prenons un exemple concret: 
![](Images/MITM_Basic.png)<br>
Dans cet exemple l'utilisateur ne s'est même pas rendu compte qu'un attaquant avait récupéré ses informations de connexion à son site favori.<br>
Tout comme le serveur ne s'est rendu compte de rien, l'attaquant n'étant qu'un émetteur de plus à qui répondre.<br>
Avec l'attaque par MITM il est possible d'aller bien plus loin, par exemple falsifier le contenu renvoyé par le site Web afin de faire éxécuter à l'utilisateur du code malveillant pouvant avoir des impacts sur d'autres sites par le biais d'ajout de requêtes silencieuses par exemple.<br>
L'attaque par MITM est réellement dangereuse et mériterait une documentation à part entière comme de nombreuses failles dont nous allons parler dans cette documentation.