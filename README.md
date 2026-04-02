La Séquence 4 est l'étape de capitalisation. C'est ici que tu prouves au correcteur que tu as compris ce que tu as fait. Pour obtenir les 4 points de qualité du README, ton fichier doit être structuré, illustré et expliquer la démarche de A à Z.

Voici une structure complète que tu peux copier-coller dans ton fichier README.md (remplace les parties entre crochets par tes informations).

 Rapport d'Atelier : From Image to Cluster
 Objectif du projet
L'objectif de cet atelier était d'automatiser la création d'une image applicative personnalisée et son déploiement sur un cluster Kubernetes (K3d) en utilisant une approche Infrastructure as Code (IaC) avec Packer et Ansible.

🛠️ Stack Technique
Environnement : GitHub Codespaces (Ubuntu Noble)

Orchestrateur : K3d (Kubernetes léger)

Build Image : Packer (Plugin Docker)

Automatisation : Ansible & Makefile

Serveur Web : Nginx customisé

1. Pré-requis
Avant de commencer, assurez-vous que les dépendances Python et les collections Ansible sont installées :

Bash:
pip3 install kubernetes
ansible-galaxy collection install kubernetes.core

2. Déploiement automatique
Grâce au Makefile, l'intégralité du pipeline est automatisée. Une seule commande suffit :

Bash:
make all
Ce que fait cette commande :

Build : Packer crée une image Docker my-custom-nginx:v1 en injectant mon fichier index.html.

Import : L'image est poussée manuellement dans le cluster K3d lab.

Deploy : Ansible communique avec l'API Kubernetes pour créer le Deployment (2 réplicas) et le Service (NodePort).

Expose : Un port-forward est lancé sur le port 8081.

 Structure des fichiers de configuration:
 
Packer (image.pkr.hcl)
J'utilise Packer pour garantir l'immuabilité de l'image. Le bloc provisioner "file" permet d'intégrer directement le code source dans l'artefact final.

Ansible (deploy.yml)
Le playbook utilise le module kubernetes.core.k8s. J'ai configuré :

replicas: 2 : Pour assurer la disponibilité du service.

imagePullPolicy: Never : Pour forcer Kubernetes à utiliser l'image importée dans le noeud plutôt que d'essayer de la télécharger sur le Docker Hub.

Vérification du fonctionnement: 
Une fois le déploiement terminé, vérifiez le statut des ressources :

Bash:
kubectl get all
L'application est accessible dans l'onglet PORTS du Codespace sur le port 8081.



Cycle de vie DevOps : Build -> Ship -> Run.

Idempotence : Avec Ansible, le déploiement peut être relancé sans créer de doublons ou d'erreurs si l'état souhaité est déjà atteint.
