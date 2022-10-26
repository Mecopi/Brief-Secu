# Stratégie de Sécurisation d'une application (Mission Locale)

## Sommaire

- Front End
- - Définition du terme ``` origin ```
- - SOP
- - CORS
- - CSP
- - SRI

## Introduction

Ce document fait office de documentation des sécurités de bases applicatives.<br>
Ainsi que de présentation d'une stratégie de sécurisation d'une application.<br>

## Définition du temre ``` origin ```

Le terme ``` origin ``` représente enfaite un domaine.<br>
Un domaine peut-être comparé à une maison.<br>
Prenons un exemple : 

Internet se rapporterai au monde.<br>
Le Web se rapporterai à un quartier.<br>
Un domaine se rapporterai à une maison.<br>

## SOP (Same Origin Policy)

La politique de même origine est une sécurité présente par défaut sur tous les navigateurs.<br>
Cette politique permet de définir que votre domaine ne partagera aucune informations avec d'autres
domaines extérieurs.<br>

Reprenons un exemple : 

Si un domaine est comparable à une maison, alors<br>
SOP serait comme fermer à clef toutes les portes<br>
et fenêtres de votre maison, de cette façons, aucun<br>
de vos voisins ne serait apte à intéragir avec vous<br>

Il est cependant possible de récupérer des informations tels que des images ou des iframes depuis
d'autres origins, mais SOP imposera quelques contraintes à leur incorporation au sein de votre site.<br>
Par exemple, il ne sera pas possible d'accèder au contenu d'une iframe par scripting puis-ce que celà se rapporterai à du ``` Cross-Origin ``` ou Origines Croisées, nous y reviendrons plus tard.<br>

Il n'est pas envisagé d'utiliser SOP dans le cadre de la stratégie de sécurisation de l'application de la Mission Locale puis ce qu'il semblerait qu'il sera nécessaire de récupérer des informations provenants de partenaires de cette dernière.<br>

## CORS (Cross Origin Ressources Sharing)

Le standart de Partage de Ressources Inter-Domaines permet de s'ouvrir au partage de données avec d'autres domaines sur le Web à contrario de SOP (Politique de même origine).<br>
Cepedant, avec CORS il est nécessaire d'établir une liste de domaines avec lesquels vous pourrez
échanger des données.<br>
Il est aussi nécessaire que les deux domaines acceptent le partage de données entre eux, si l'un d'eux n'est pas d'accord pour partager ses données avec l'autre, alors l'échange de données ne sera pas permis.<br>
L'utilisation de CORS se fait par le biais d'insertion d'en-tête lors de requêtes HTTP<br>

Admettons que notre domaine soit 'missionlocale-jeunes-du-valenciennois.com' et que nous cherchions à partager nos données avec 'pole-emploi.fr' ainsi que de récupérer ses données<br>

```
Access-Control-Allow-Origin: https://pole-emploi.fr
```

## CSP (Content Security Policy)

CSP (Politique de Sécurité du Contenu) est une stratégie applicable dans le cadre du controle d'accès aux ressources atteignables.<br>
Il est enfaite question de définir une liste de ressources que le navigateur sera en mesure de
charger, toutes les ressources ne se trouvant pas dans la CSP seront bloqués ou simplement non executés.<br>
Il s'agit simplement de restreindre l'accès aux ressources dans l'optique de ne pas récupérer et éxecuter un script frauduleux.