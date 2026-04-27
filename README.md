# LAB-18-FireStorm
Introduction

Ce laboratoire consiste à étudier l’application Android FireStorm dans le but de comprendre son fonctionnement interne et d’en extraire les informations nécessaires pour résoudre le challenge proposé.

L’approche adoptée repose sur plusieurs techniques complémentaires, notamment l’analyse statique, l’observation dynamique du comportement de l’application, ainsi que l’exploitation de services externes comme Firebase.

#Étude du mécanisme de génération du mot de passe

L’analyse du code a permis d’identifier une fonction clé nommée Password().
Cette fonction est responsable de la construction du mot de passe utilisé par l’application.

Le mot de passe final est obtenu en combinant plusieurs éléments :

des chaînes de caractères statiques définies dans le fichier strings.xml
une partie dynamique générée via une fonction native appelée generateRandomStrings, issue de la bibliothèque libfirestorm.so

Par ailleurs, le fichier de ressources contient des informations sensibles, notamment :

une adresse email utilisée pour l’authentification
les paramètres de configuration Firebase (clé API, URL de la base de données, etc.)

Un point notable est que cette fonction de génération n’est pas directement déclenchée par l’interface utilisateur, ce qui rend son exploitation plus complexe et nécessite une approche dynamique.

<img width="923" height="238" alt="image" src="https://github.com/user-attachments/assets/b30cee93-8c67-446b-bc0e-25854986f055" />


#Mise en place de l’instrumentation dynamique

Afin d’observer le comportement de l’application en exécution, l’utilisation de Frida s’impose.

Frida permet d’injecter du code dans l’application en cours d’exécution, ce qui donne la possibilité d’intercepter et d’analyser les appels de fonctions internes.

Cette technique est particulièrement utile lorsque certaines parties du code ne sont pas accessibles directement via l’interface.
<img width="1376" height="768" alt="PceNmN9d" src="https://github.com/user-attachments/assets/a0a7f0e5-5239-4316-8108-867a477200bc" />

<img width="902" height="73" alt="image" src="https://github.com/user-attachments/assets/8163a08e-ce88-4424-800a-0888368bdb03" />


#Récupération du mot de passe

Un script Frida a été développé pour cibler la fonction responsable de la génération du mot de passe.

Grâce à cette instrumentation, il a été possible d’intercepter la valeur retournée par cette fonction et d’obtenir le mot de passe complet :

C7_dotpsC7t7f_._In_i.IdttpaofoaIIdIdnndIfC

Cette étape constitue un point clé dans la progression du challenge.
Une fois les identifiants récupérés, un script Python utilisant Pyrebase a été utilisé pour se connecter au service Firebase de l’application.

Cette connexion permet d’accéder aux données stockées dans la base et d’extraire les informations nécessaires, notamment le flag du challenge.


<img width="1376" height="768" alt="R4C6dJus" src="https://github.com/user-attachments/assets/e6c26f00-db9f-498b-9a23-39fc5d08ca29" />


