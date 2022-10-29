# Stratégie de Sécurisation d'une application

# Table des matières

- Principes de sécurité
    - Défense en profondeur 
    - Réduction surface d'attaque 
    - Moindre privileges 
    - Politique sécurité des mots de passe 
- Protocoles
    - HTTP
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

# Principes de sécurités

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
