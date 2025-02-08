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
7. [Utilisation de la chaine CI/CD](#Utilisation-de-la-chaine-CI/CD)
8. [Développement en local](#)
9. [Contacts](#Contacts)

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

| Fonctionnalité          | Technologie                 | 
| :---------------------- | :-------------------------- |
| Orchestration           | Saagie                      |
| CI/CD                   | SaagieApi                   |
| Stockage                | PostgreSQL                  |
| Layer Ingestion         | DLT > S3                    |    
| Layer Transformation    | DBT > DuckDB                |
| Layer Exposition        | Datamart PostgreSQL - MinIO |


## Roadmap

[En construction]

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
- **Indentation** :
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
**Typage explicite** : nous utilisons le module `typing` pour tous les arguments et retours de fonction :
```python
def process_data(data: pd.DataFrame) -> pd.DataFrame:
```

### Design
* Les fonctionnalités sont développées de manière modulables et avec pour objectif d'être réutilisables. C'est pourquoi, **chaque fonctionnalité est packagée** afin d'être importée dans n'importe quel projet sans devoir être réécrite. Vous pouvez retrouver l'ensemble des packages [ici](https://github.com/DNUM-SocialGouv/sdh-pyetl)
* Conformément à la logique objet, l'utilisation des `class` est à privilégier
* Chaque fonction doit être limitée à 1 usage

### Environnement 
Le SDH utilise [Poetry](https://python-poetry.org/) pour gérer les environnements virtuels. 
* Pour installer poetry : `pip install poetry`
* Un envrionnement virtuel est créé pour chaque projet. 
* Les packages et versions installées pour chaque projet sont stockés dans `poetry.lock` à la racine du répertoire du projet.
* Pour installer les dépendances à partir de ce fichier, voici la commande : `poetry install`.

### Commit
Tout commit doit être formalisé ainsi `[<type>] <module> : <Descriptif court>`
* **[feat]** pour l'ajout d'une fonctionnalité
* **[fix]** pour la correction d'une anomalie
* **[docs]** pour la modification de la docuemntation
* **[refactor]** pour le refacto du code
* **[test]** pour l'ajout ou la modification des tests
* **[ci]** pour la modification de la CI

### Logging

Nous utilisons `logging` pour administrer les logs : 
```python
import logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.info("Processing started.")s
```

### Erreurs 
Les exceptions spécifiées explicitement sont préférées : 
```python
try:
    result = 1 / 0
except ZeroDivisionError:
    print("Division par zéro.")
```

### Tests
Nous imposons un taux de couverture de tests unitaires >= 80%.
* Le framework [pytest](https://docs.pytest.org/en/stable/) est utilisé [décorateurs standards à décrire]
* Avec la librairie [great_expectations](https://github.com/great-expectations/great_expectations), les données doivent être validées au niveau de chaque layer : schéma, type, règle métier [standards à compléter]

### Sécurité

Toutes les informations sensibles doivent être stockées dans des variables d'environnement. 
* Sur chaque repository github, il faut veiller à configuer un secret par variable
* [chiffrement des variables à prévoir avec cryptography ?]

### Documentation 

Chaque projet fait l'objet d'une documentation : 
* un `README.md` avec a minima :
    * Une courte description du projet et de ses fonctionnalités
    * un tutoriel d'installation
    * une description de l'organisation du projet
* [compléter avec la doc métier : specs, MCD]

### Meta données 

[à construire]

## Architecture

### Architecture d'un projet Social Data Hub

.github \
cicd_saagie_tool \
Saagie \
|_ envs \
|_____ dev.json \
|_____ prod.json \
|_ jobs \
|_____ ingestion.json \
|_____ transformation.json \
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
Pour initialiser un projet : 
1) Se rendre sur https://dnum-workspace.pcv.saagie.io, sélectionnez l'environnement souhaité et créez un nouveau projet
2) Créer les variables d'environnement pour la CI/CD : 
    - `$SAAGIE_PWD`
    - `$SAAGIE_REALM`
    - `$SAAGIE_URL`
    - `$SAAGIE_USER`

### Initialisation de la CI
1) Créer un fork du repository SocialDataHub, règle de nommage : "SocialDataHub_[NomDuProjet]"
2) Configurer les secrets dans `Settings` > `Actions secrets and variables`: 
    - `$SAAGIE_PWD`
    - `$SAAGIE_REALM`
    - `$SAAGIE_URL`
    - `$SAAGIE_USER`
3) Cloner le repository ainsi créé
4) Dans `saagie/envs/dev.json`, saisir le `platform_id` correspondant à l'environnement et le `project_id` ainsi que le `project_name` créé dans l'UI Saagie. 

### Création d'un job
1) Sur https://dnum-workspace.pcv.saagie.io, créer un nouveau job. Convention de nommage : execute_nom_du_job càd verbe + objet, ex : print_hello_world
2) Dans `saagie/jobs/[job_name].json`, saisir le `job_id` correspondant au job créé : 
* `id` : id propre au fichier yaml. Bonne pratique : copiez l'id du job.
* `next_nodes` : id du job à exécuter après si nécessaire.
Sur la plateforme, chaque job est exécuté dans un conteneur ce qui signifie que le stockage doit être effectué sur les services partagés de la plateforme pour être résilient : S3 ou PostreSQL. 

### Création d'un pipeline
1) Sur https://dnum-workspace.pcv.saagie.io, créer un nouveau pipeline. 
2) Dans `saagie/pipelines/[pipeline_name].json`, saisir le `pipeline_id` correspondant au job créé : 
* `id` : id propre au fichier yaml. Bonne pratique : copiez l'id du pipeline. 

### Bonne pratique de gestion des erreurs
Il n'y a pas de serveur SMTP sur la plateforme. En cas d'erreur, la seule possibilité d'envoyer un email de notification est de passer par la fonctionnalité ad hoc du produit Saagie. Cette fonctionnalité est liée au job et ne s'applique que lorsque le job entier est en erreur. 
* Stocker les erreur dans une liste `err_list = []`
    ```python
    if len(error_list) != 0:
        for error in error_list:
            print(error)
        raise Exception("Errors during the processing of the files")

### Bonne pratique de gestion des settings
Il existe 2 types de settings : 
* `variables.json` : paramètres communs aux jobs, ce fichier étant situé à la racine du projet, il s'agit des paramètres sur les éléments résilients.
* `[nom_du_parametrage].json` : paramètres propres à chaque job. 

## Utilisation de la chaine CI/CD

Nous utilisons l'API Saagie afin d'automatiser la chaine CI/CD. L'usage de l'API est documenté [ici](https://saagieapi.readthedocs.io/en/latest/). 

## Développement en local

Les postes Ministères sociaux ne disposant pas des droits d'admin, voici un guide d'installation des outils les plus utiles pour développer en local. 
- **Python**
    - Télécharger Python sur le site officiel, version "avec exécutable"
    - Choisir l'installation custom
    - Décocher toutes les options "for all users"
    - À chaque utilisation de python, par ex 3.11 ici, il faut : `set PATH=%PATH%;C:\Users\[username]\AppData\Local\Programs\Python\Python311` remplacer [username] par votre nom d'utilisateur sans crochet
- **Poetry**
  - Installer pipx `py -m pip install --user pipx`
  - À chaque session de command line, il faut ajouter le path ainsi : `set PATH=%PATH%;C:\Users\[username]\AppData\Roaming\Python\Python311\Scripts`
  - `pipx install poetry`
  - À chaque session de command line, il faut ajouter le path ainsi : `set PATH=%PATH%;C:\Users\[username]\.local\bin`
  - À chaque session de command line, il faut ajouter le path ainsi : `set PATH=%PATH%;C:\Users\[username]\.local\bin`
- **Pyenv**
  - À chaque session de command line, il faut ajouter le path ainsi : `set PATH=%PATH%;C:\Users\[username]\.pyenv\pyenv-win\bin`
- **git**
  - Comme pour Python, il faut installer git seulement pour son user
  - À chaque session de command line, il faut ajouter le path ainsi : ` set PATH=%PATH%;C:\Users\beatrice.daniel\AppData\Local\Programs\Git\cmd`

## Contacts

Béatrice DANIEL, tech lead data : beatrice.daniel@sg.social.gouv.fr