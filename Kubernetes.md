# Fondamentaux de Kubernetes

## Aperçu des chapitres

Kubernetes est une plate-forme d'orchestration de conteneurs open source très populaire qui peut être utilisée pour automatiser le déploiement, la mise à l'échelle et la gestion des charges de travail conteneurisées. Dans ce chapitre, nous allons découvrir l'architecture de base d'un cluster Kubernetes et ses composants. Pour en savoir encore plus sur les bases de Kubernetes, vous pouvez suivre le cours gratuit Introduction à Kubernetes (LFS158x) de la Fondation Linux sur edX.

Conçu et développé à l'origine par Google, Kubernetes est devenu open source en 2014, et le long de la version v1.0, Kubernetes a été donné à la nouvelle Cloud Native Computing Foundation en tant que tout premier projet. De nombreuses technologies cloud natives évoluent autour de Kubernetes, qu'il s'agisse d'outils de bas niveau tels que les runtimes de conteneurs, la surveillance ou les outils de livraison d'applications.

Les professionnels ayant des compétences Kubernetes, qu'il s'agisse d'administration ou de développement, sont très recherchés car Kubernetes est devenu la pièce maîtresse de nombreuses plates-formes cloud modernes.

## Objectifs d'apprentissage

À la fin de ce chapitre, vous devriez être en mesure de :

* Discuter de l'architecture de base de Kubernetes.
* Expliquer les différents composants du plan de contrôle et des nœuds de travail.
* Comprendre comment démarrer avec la configuration de Kubernetes.
* Discutez de la manière dont Kubernetes exécute les conteneurs.
* Discuter des concepts de planification de Kubernetes.

## Architecture Kubernetes

Kubernetes est souvent utilisé en tant que cluster, ce qui signifie qu'il est réparti sur plusieurs serveurs qui travaillent sur différentes tâches et répartissent la charge d'un système. Il s'agit d'une décision de conception initiale basée sur les exigences de Google, où des milliards de conteneurs sont lancés chaque semaine. Compte tenu de la grande évolutivité verticale de Kubernetes, il est possible d'avoir des clusters avec des milliers de nœuds de serveur, dans plusieurs centres de données et régions.

D'un point de vue général, les clusters Kubernetes se composent de deux types de nœuds de serveur différents qui constituent un cluster :

* Nœud(s) du plan de contrôle

Ce sont les cerveaux de l'opération. Les nœuds du plan de contrôle contiennent divers composants qui gèrent le cluster et contrôlent diverses tâches telles que le déploiement, la planification et l'auto-réparation des charges de travail conteneurisées.

* Nœuds de travail

Les noeuds worker sont là où les applications s'exécutent dans votre cluster. C'est le seul travail des nœuds de travail et ils n'ont aucune autre logique implémentée. Leur comportement, comme s'ils devaient démarrer un conteneur, est entièrement contrôlé par le nœud du plan de contrôle.

![Architecture Kubernetes](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/lrc7lcf1ayk1-Kubernetesarchitecture.png)

Semblable à une architecture de microservices que vous choisiriez pour votre propre application, Kubernetes intègre plusieurs services plus petits qui doivent être installés sur les nœuds.

### Les nœuds du plan de contrôle hébergent généralement les services suivants...

**kube-apiserver**
C'est la pièce maîtresse de Kubernetes. Tous les autres composants interagissent avec le serveur API et c'est là que les utilisateurs accèdent au cluster.

**etcd**
Une base de données qui contient l'état du cluster. etcd est un projet autonome et non une partie officielle de Kubernetes.

**planificateur de kube**
Lorsqu'une nouvelle charge de travail doit être planifiée, le kube-scheduler choisit un nœud de travail qui pourrait convenir, en fonction de différentes propriétés telles que le processeur et la mémoire.

**kube-controller-manager**
Contient différentes boucles de contrôle sans terminaison qui gèrent l'état du cluster. Par exemple, l'une de ces boucles de contrôle peut s'assurer qu'un nombre souhaité de votre application est disponible en permanence.

**cloud-controller-manager (facultatif)**
Peut être utilisé pour interagir avec l'API des fournisseurs de cloud, pour créer des ressources externes comme des équilibreurs de charge, des groupes de stockage ou de sécurité.

### Composants des nœuds de travail

**durée d'exécution du conteneur**
L'environnement d'exécution du conteneur est responsable de l'exécution des conteneurs sur le nœud de travail. Pendant longtemps, Docker était le choix le plus populaire, mais il est maintenant remplacé par d'autres runtimes comme containerd.

**kubelet**
Un petit agent qui s'exécute sur chaque noeud worker du cluster. Le kubelet communique avec le serveur API et le runtime du conteneur pour gérer la dernière étape du démarrage des conteneurs.

**kube-proxy**
Un proxy réseau qui gère les communications internes et externes de votre cluster. Au lieu de gérer le flux de trafic par lui-même, le kube-proxy essaie de s'appuyer sur les capacités de mise en réseau du système d'exploitation sous-jacent si possible.

Il est important de noter que cette conception permet aux applications déjà démarrées sur un noeud worker de continuer à s'exécuter même si le plan de contrôle n'est pas disponible. Bien que de nombreuses fonctionnalités importantes telles que la mise à l'échelle, la planification de nouvelles applications, etc., ne soient pas possibles lorsque le plan de contrôle est hors ligne.

Kubernetes a également un concept d'espaces de noms, qui ne doivent pas être confondus avec les espaces de noms du noyau qui sont utilisés pour isoler les conteneurs. Un espace de noms Kubernetes peut être utilisé pour diviser un cluster en plusieurs clusters virtuels, qui peuvent être utilisés pour la multilocation lorsque plusieurs équipes partagent un cluster. Veuillez noter que les espaces de noms Kubernetes ne conviennent pas à une isolation renforcée et doivent plutôt être considérés comme un répertoire sur un ordinateur où vous pouvez organiser des objets et gérer quel utilisateur a accès à quel dossier.

## Configuration de Kubernetes

La configuration d'un cluster Kubernetes peut être réalisée avec de nombreuses méthodes différentes. Créer un "cluster" de test peut être très facile avec les bons outils :

* Minikube
* gentil
* MicroK8

Si vous souhaitez configurer un cluster de niveau production sur votre propre matériel ou machines virtuelles, vous pouvez choisir l'un des différents programmes d'installation :

* kubeadm
* cops
* kubespray

Quelques fournisseurs ont commencé à regrouper Kubernetes dans une distribution et proposent même un support commercial :

* Éleveur
* k3s
* OpenShift
* VMWare Tanzu

Les distributions choisissent souvent une approche opiniâtre et offrent des outils supplémentaires tout en utilisant Kubernetes comme élément central de leur cadre.

Si vous ne souhaitez pas l'installer et le gérer vous-même, vous pouvez le consommer auprès d'un fournisseur de cloud :

*Amazon (EKS)
*Google (GKE)
* Microsoft (AKS)
* Digital Ocean (DOKS)

## Tutoriel interactif - Créer un cluster

Vous pouvez apprendre à configurer votre propre cluster Kubernetes avec Minikube dans ce [tutoriel interactif](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/).

## API Kubernetes

L'API Kubernetes est le composant le plus important d'un cluster Kubernetes. Sans lui, la communication avec le cluster n'est pas possible, chaque utilisateur et chaque composant du cluster lui-même a besoin du serveur API.

![Aperçu du contrôle d'accès](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/3qfnpfcpccts-AccessControlOverview.png)

Présentation du contrôle d'accès, extrait de la [documentation Kubernetes](https://kubernetes.io/docs/concepts/security/controlling-access/)

Avant qu'une requête ne soit traitée par Kubernetes, elle doit passer par trois étapes :

**Authentification**

Le demandeur doit présenter un moyen d'identité pour s'authentifier auprès de l'API. Généralement effectué avec un certificat signé numériquement (X.509) ou avec un système de gestion d'identité externe. Les utilisateurs de Kubernetes sont toujours gérés en externe. Les [comptes de service](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/) peuvent être utilisés pour authentifier les utilisateurs techniques.

**Autorisation**

Il est décidé ce que le demandeur est autorisé à faire. Dans Kubernetes, cela peut être fait avec [Role Based Access Control (RBAC)](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

**Controle d'admission**

Dans la dernière étape, les [contrôleurs d'admission](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/) peuvent être utilisés pour modifier ou valider la demande. Par exemple, si un utilisateur essaie d'utiliser une image de conteneur à partir d'un registre non fiable, un contrôleur d'admission pourrait bloquer cette demande. Des outils tels que [Open Policy Agent](https://www.openpolicyagent.org/) peuvent être utilisés pour gérer le contrôle d'admission en externe.

Comme de nombreuses autres API, l'API Kubernetes est implémentée en tant qu'interface RESTful exposée via HTTPS. Grâce à l'API, un utilisateur ou un service peut créer, modifier, supprimer ou récupérer des ressources qui résident dans Kubernetes.

## Exécuter des conteneurs sur Kubernetes

En quoi l'exécution d'un conteneur sur votre ordinateur local diffère-t-elle de l'exécution de conteneurs dans Kubernetes ? Dans Kubernetes, au lieu de démarrer directement les conteneurs, vous définissez les pods comme la plus petite unité de calcul et Kubernetes le traduit en un conteneur en cours d'exécution. Nous en apprendrons plus sur les pods plus tard, pour l'instant imaginez-le comme un emballage autour d'un conteneur.

Lorsque vous créez un objet Pod dans Kubernetes, plusieurs composants sont impliqués dans ce processus, jusqu'à ce que vous obteniez des conteneurs exécutant un nœud.

Voici un exemple utilisant Docker :

![Exécution de conteneurs dans Kubermetes](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ej86a4r4qgt9-ContainersinKubernetes.png)

Exécution de conteneurs dans Kubermetes

Afin de permettre l'utilisation d'autres environnements d'exécution de conteneur que Docker, Kubernetes a introduit l'interface d'exécution de conteneur (CRI) en 2016.

### Exécutions de conteneurs

**conteneur**

[containerd](https://containerd.io/) est une implémentation légère et performante pour exécuter des conteneurs. Sans doute le runtime de conteneur le plus populaire actuellement. Il est utilisé par tous les principaux fournisseurs de cloud pour les produits Kubernetes As A Service.

**CRI-O**

[CRI-O](https://cri-o.io/) a été créé par Red Hat et avec une base de code similaire étroitement liée à podman et buildah.

**Docker**

Le standard depuis longtemps, mais jamais vraiment fait pour l'orchestration de conteneurs. L'utilisation de Docker comme environnement d'exécution pour Kubernetes est obsolète et sera supprimée dans Kubernetes 1.23. Kubernetes a un excellent [article de blog](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/) qui répond à toutes les questions sur le sujet.

![La création de conteneurs avec containerd peut être beaucoup plus simple qu'avec Docker](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/7akir1v64za9-ContainercreationwithcontainerdcanbemuchsimplerthanwithDocker.png)

La création de conteneurs avec containerd peut être beaucoup plus simple qu'avec Docker

L'idée de containerd et de CRI-O était très simple : fournir un environnement d'exécution qui ne contient que l'essentiel pour exécuter des conteneurs. Néanmoins, ils disposent de fonctionnalités supplémentaires, telles que la possibilité de s'intégrer aux outils de bac à sable d'exécution des conteneurs. Ces outils tentent de résoudre le problème de sécurité lié au partage du noyau entre plusieurs conteneurs. Les outils les plus courants actuellement sont :

**[gvisor](https://github.com/google/gvisor)**

Fabriqué par Google, fournit un noyau d'application qui se situe entre le processus conteneurisé et le noyau hôte.

**[Kata Containers] (https://katacontainers.io/)**
Un environnement d'exécution sécurisé qui fournit une machine virtuelle légère, mais se comporte comme un conteneur.

## La mise en réseau

La mise en réseau Kubernetes peut être très compliquée et difficile à comprendre. Un grand nombre de ces concepts ne sont pas liés à Kubernetes et font parti de l'orchestration des conteneurs. Encore une fois, nous devons faire face au problème que de nombreux conteneurs doivent communiquer à travers de nombreux nœuds. Kubernetes fait la distinction entre quatre problèmes de réseau différents qui doivent être résolus :

1. Communications de conteneur à conteneur

Cela peut être résolu par le concept Pod comme nous l'apprendrons plus tard.

2. Communications Pod à Pod

Cela peut être résolu avec un réseau superposé.

3. Communications pod à service

Il est implémenté par le kube-proxy et le filtre de paquets sur le nœud.

4. Communications externes au service

Il est implémenté par le kube-proxy et le filtre de paquets sur le nœud.

Il existe différentes manières de mettre en œuvre la mise en réseau dans Kubernetes, mais également trois exigences importantes :

* Tous les pods peuvent communiquer entre eux à travers les nœuds.
* Tous les nœuds peuvent communiquer avec tous les pods.
* Pas de traduction d'adresse réseau (NAT).

Pour mettre en œuvre la mise en réseau, vous pouvez choisir parmi une variété de fournisseurs de réseau tels que :

* [Projet Calico](https://www.tigera.io/project-calico/)
* [Weave](https://www.weave.works/oss/net/)
* [Cilium](https://cilium.io/)
*
Dans Kubernetes, chaque pod obtient sa propre adresse IP, il n'y a donc aucune configuration manuelle impliquée. De plus, la plupart des configurations Kubernetes incluent un module complémentaire de serveur DNS appelé [core-dns](https://kubernetes.io/docs/tasks/administer-cluster/coredns/), qui peut fournir la découverte de services et la résolution de noms à l'intérieur du cluster.

## Planification

Dans sa forme la plus élémentaire, la planification est une sous-catégorie de l'orchestration de conteneurs et décrit le processus de sélection automatique du bon nœud (worker) sur lequel exécuter une charge de travail conteneurisée. Dans le passé, la planification était davantage une tâche manuelle où un administrateur système choisissait le bon serveur pour une application en gardant une trace des serveurs disponibles, de leur capacité et d'autres propriétés comme leur emplacement.

Dans un cluster Kubernetes, le kube-scheduler est le composant qui prend la décision de planification, mais n'est pas responsable du démarrage réel de la charge de travail. Le processus de planification dans Kubernetes démarre toujours lorsqu'un nouvel objet Pod est créé. N'oubliez pas que Kubernetes utilise une approche déclarative, où le pod n'est décrit qu'en premier, puis le planificateur sélectionne un nœud où le pod sera réellement démarré par le kubelet et l'exécution du conteneur.

Une fausse idée courante à propos de Kubernetes est qu'il possède une forme d'"intelligence artificielle" analysant la charge de travail et déplaçant les pods en fonction de la consommation de ressources, du type de charge de travail et d'autres facteurs. La vérité est qu'un utilisateur doit donner des informations sur les exigences de l'application, y compris les demandes de CPU et de mémoire et les propriétés d'un nœud. Par exemple, un utilisateur pourrait demander que son application nécessite deux cœurs de processeur, quatre gigaoctets de mémoire et devrait de préférence être planifiée sur un nœud avec des disques rapides.

Le planificateur utilisera ces informations pour filtrer tous les nœuds qui répondent à ces exigences. Si plusieurs nœuds répondent également aux exigences, Kubernetes programmera le pod sur le nœud avec le moins de pods. Il s'agit également du comportement par défaut si un utilisateur n'a spécifié aucune autre exigence.

Il est possible que l'état souhaité ne puisse pas être établi, par exemple parce que les noeuds worker ne disposent pas de ressources suffisantes pour exécuter votre application. Dans ce cas, le planificateur réessaiera de trouver un nœud approprié jusqu'à ce que l'état puisse être établi.

## Ressources additionnelles

En savoir plus sur...

* From Google to the world: The Kubernetes origin story, by Craig McLuckie (2016)
* Large-scale cluster management at Google with Borg, by Abhishek Verma, Luis Pedrosa, Madhukar R. Korupolu, David Oppenheimer, Eric Tune, John Wilkes (2015)

Kubernetes Architecture

* Kubernetes Architecture explained | Kubernetes Tutorial 15

RBAC

* Demystifying RBAC in Kubernetes, by Kaitlyn Barnard

Container Runtime Interface

* Introducing Container Runtime Interface (CRI) in Kubernetes (2016)

Kubernetes networking and CNI

* What is Kubernetes networking?

Internals of Kubernetes Scheduling

* A Deep Dive into Kubernetes Scheduling, by Ron Sobol (2020)

Kubernetes Security Tools

* Popeye
* kubeaudit
* kube-bench

Kubernetes Playground

* Play with Kubernetes

