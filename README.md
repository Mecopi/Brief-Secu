# Stratégie de Sécurisation d'une application (Mission Locale)

## Sommaire

- Front End
- - CORS
- - CSP

## CORS (Cross Origin Ressources Sharing)

CORS est un standart permettant de s'ouvrir au partage de données avec d'autres domaines sur le Web à contrario de SOP (Same Origin Policy).<br>
Cepedant, avec CORS il est nécessaire d'établir une liste de domaines avec lesquels vous pourrez
échanger des données.<br>
Il est aussi nécessaire que les deux domaines acceptent le partage de données entre eux, si l'un d'eux n'est pas d'accord pour partager ses données avec l'autre, alors l'échange de données ne sera pas permis.<br>
L'utilisation de CORS se fait par le biais d'insertion d'en-tête lors de requêtes HTTP<br>

Admettons que notre domaine soit 'missionlocale-jeunes-du-valenciennois.com' et que nous cherchions à partager nos données avec 'pole-emploi.fr' ainsi que de récupérer ses données<br>

```
Access-Control-Allow-Origin: https://pole-emploi.fr
```

Serait une entête correcte afin d'effectuer un échange d'information entre ces 2 domaines.<br>

Dans le cadre du projet pour la Mission Locale, il semble être nécessaire d'utiliser CORS afin
de permettre la récupération de contenu de partenaire.

## CSP (Content Security Policy)

CSP est une stratégie applicable dans le cadre du controle d'accès aux ressources atteignables.<br>
Il est enfaite question d'une liste de 