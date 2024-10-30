# SocialDataHub

## Architecture d'un projet SocialDataHub

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
|_ input_verification \
|_ datawarehouse \
|_ datamart \
|_ export \

## Guetting started

### Initialisation du projet
Pour initialiser un projet, rendez vous sur https://dnum-workspace.pcv.saagie.io, sélectionnez l'environnement souhaité et créez un nouveau projet. 

### Lancement de la CI
Dans `saagie/envs/dev.json`, saisissez le `platform_id` correspondant à l'environnement et le `project_id` créé dans l'UI Saagie. 

### Création d'un job
Sur https://dnum-workspace.pcv.saagie.io, créez un nouveau job. Dans `saagie/jobs/[job_name].json`, saisissez le `job_id` correspondant au job créé : 
* `id` : id propre au fichier yaml. Bonne pratique : copiez l'id du job.
* `next_nodes` : id du éjob à exécuter après si nécessaire.
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
