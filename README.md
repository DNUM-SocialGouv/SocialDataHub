# Social Data Hub

Le Social Data Hub (SDH) est la plateforme data de la Direction du Numérique des Ministères sociaux. 
La plateforme repose sur [Saagie](https://www.saagie.com/fr/) et est hébergée sur un cloud public. 

## Table des matières

1. [Manifeste](#Manifeste)
2. [Fonctionnalités](#Fonctionnalités)
3. [Roadmap](#Roadmap)
4. [Standards](#Standards)
5. [Architecture](#Architecture)
6. [Guetting started](#guetting-started)
7. [Contacts](#Contacts)

## Manifeste

Le SDH a été lancé afin d'accélérer, fiabiliser et optimiser la fabrication de produits de données en :
* **réduisant les irritants techniques** : infrastructure, DevOps, CI/CD ;
* **augmentant la couche d'abstraction** pour les développeurs et les product managers ;
* **diminuant les coûts** grâce à la mutualisation de l'infrastructure et de la stack.

Ainsi, la construction de cette plateforme s'articule autour de 3 valeurs coeur :

* **réutilisation** : la plateforme doit être composée de briques permettant leur réutilisation dans un autre contexte technique et fonctionnel. 
* **interopérabilité** : que ce soit pour l'ingestion, la transformation ou l'exposition des données, la plateforme a pour vocation de maintenir sa compatibilité vers un maximum de types et formats de données différents. 
* **accessibilité** : la plateforme s'impose une gestion de la documentation, des meta données et des standards de développement permettant une accessibilité réelle aux données. 


## Fonctionnalités

| Fonctionnalité    | Technologie     | 
| :---------------- | :-------------- |
| Orchestration     | Saagie          |
| CI/CD             | SaagieApi       |
| Stockage          | PostgreSQL      |
| Ingestion         | Pandas / S3     |    
| Transformation    | DBT / DuckDB    |
| Exposition        | MinIO           |


## Roadmap

[En construction]
* Anonymisation
* Ingestion et exposition : mise à disposition des utilisateur d'une UI pour télécharger / déposer des fichiers
* Pipeline IA

## Standards

### Code

 Le SDH respecte les conventions de style Python **PEP 8**. Les outils suivants peuvent être utilisés afin de faciliter le respect de ces normes : [flake8](https://flake8.pycqa.org) ou [black](https://github.com/psf/black). 
- **Nommage** :
    * **Variables et fonctions** : snake_case (ex. : process_data, user_name).
    * **Classes** : CamelCase (ex. : DataProcessor, UserManager).
    * **Constantes** : UPPER_CASE_WITH_UNDERSCORES (ex. : MAX_RETRIES, API_KEY).
    * **Arguments privés ou protégés** : Préfixez avec un underscore _ (ex. : _internal_var).
- **Longueur des lignes** :
    * limitée à 79 caractères et 72 pour les commentaires
    * Pour cela, nous utilisons la fonctionnalité de continuation implicite ou explicite de Python et SQL. Ex : 
     ```python
     total = (first_variable
           + second_variable
           - third_variable)
- **Identation** :
    * L'indentation se fait par bloc de 4 espaces
    * Ne jamais mélanger tabulations et espaces
    * 2 lignes entre fonctions
    * Les blocs conditionnels doivent être bien alignés :
    ```python
    if condition1:
        do_something()
    elif condition2:
        do_something_else()
    else:
        handle_default_case()
- **Espacement** :
    * Un espace autour des opérateurs ` y = ax + b `, `if x == 1:`
    * Pas d'espace autour des parenthèses/brackets `my_list[0]`, `func(arg)`.
- **Import** :
    * L'import se fait toujours en haut des fichiers
    * L'import se fait dans l'ordre suivant : bibliothèques standards > bibliothèques externes > modules internes
    * L'import explicite est préféré aux imports globaux : 
    ```python
    # Correct
    from math import sqrt
    # A éviter
    from math import *
- **Commentaires et docstrings** :
    * Commentaires : les commentaires # sont placés sur une ligne séparée ou infinie si courts
    * Docstrings : Les modules, classes et fonctions sont documentées entre """ avec un style de type NumPy :
    ```python
    def add_numbers(a: int, b: int) -> int:
        """
        Add two integers and return the result.

        Args:
            a (int): The first integer.
            b (int): The second integer.

        Returns:
            int: The sum of the integers.
        """
        return a + b
- **Exceptions** : Les exceptions spécifiées explicitement sont préférées : 
    ```python
    try:
        result = 1 / 0
    except ZeroDivisionError:
        print("Division par zéro.")
**Typage explicite** : nous utilisons le module `typing` pour tous les arguments et retours de fonctions.
```python
from typing import List

def process_data(data: List[str]) -> List[str]:
    return [d.upper() for d in data]

```
### Commit
aaa

### Environnement 

### Logging

### Erreurs 

### Tests

### Sécurité

### Documentation 

### Meta données 

## Architecture

### Architecture d'un projet SocialDataHub

.github \
cicd_saagie_tool \
Saagie \
|_ envs \
|_____ dev.json \
|_____ prod.json \
|_ jobs \
|_____ input_verification.json \
|_____ datawarehouse.json \
|_____ datamart.json \
|_____ export.json \
Code \
|_ Ingestion \
|_ Transformation \
|_ Export 

## Guetting started

### Prérequis
* Disposer d'un compte sur https://dnum-workspace.pcv.saagie.io
* Avoir créé un projet sur https://dnum-workspace.pcv.saagie.io
* Disposer d'un compte Github et d'un accès en écriture sur https://github.com/DNUM-SocialGouv/
* Avoir installé les librairies de `cicd_saagie_tool/requirements.txt`

### Initialisation du projet
Pour initialiser un projet, rendez vous sur https://dnum-workspace.pcv.saagie.io, sélectionnez l'environnement souhaité et créez un nouveau projet. 

### Initialisation de la CI
1) Créer un fork du repository SocialDataHub, règle de nommage : "SocialDataHub_[NomDuProjet]"
2) Configurer les secrets dans `Settings` > `Actions secrets and variables`
3) Cloner le repository ainsi créé
4) Dans `saagie/envs/dev.json`, saisir le `platform_id` correspondant à l'environnement et le `project_id` ainsi que le `project_name` créé dans l'UI Saagie. 

### Création d'un job
1) Sur https://dnum-workspace.pcv.saagie.io, créer un nouveau job. Convention de nommage : execute_nom_du_job càd verbe + objet, ex : print_hello_world
2) Dans `saagie/jobs/[job_name].json`, saisir le `job_id` correspondant au job créé : 
* `id` : id propre au fichier yaml. Bonne pratique : copiez l'id du job.
* `next_nodes` : id du job à exécuter après si nécessaire.
Sur la plateforme, chaque job est exécuté dans un conteneur ce qui signifie que le stockage doit être effectué sur les services partagés de la plateforme pour être résilient : S3 ou PostreSQL. 

### Bonne pratique de gestion des erreurs
Il n'y a pas de serveur SMTP sur la plateforme. En cas d'erreur, la seule possibilité d'envoyer un email de notification est de passer par la fonctionnalité ad hoc du produit Saagie. Cette fonctionnalité est liée au job et ne s'applique que lorsque le job entier est en erreur. 
* Stocker les erreur dans une liste `err_list = []`
* ```if len(error_list) != 0:
        for error in error_list:
            print(error)
        raise Exception("Errors during the processing of the files")```

### Bonne pratique de gestion des settings
Il existe 2 types de settings : 
* `variables.json` : paramètres communs aux jobs, ce fichier étant situé à la racine du projet, il s'agit des paramètres sur les éléments résilients.
* `[nom_du_parametrage].json` : paramètres propres à chaque job. 

## Contacts