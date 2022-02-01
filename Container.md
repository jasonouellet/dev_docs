# Container

## Aperçu des chapitres

Dans ce chapitre, nous découvrirons les défis et les opportunités de l'orchestration de conteneurs et pourquoi elle a des exigences particulières en matière de mise en réseau et de stockage.

Avant de commencer à parler d'orchestration des conteneurs, revenons un peu en arrière et apprenons ce qu'est un conteneur et ce qui le rend si utile dans la création et l'exploitation d'applications.

## Objectifs d'apprentissage

À la fin de ce chapitre, vous devriez être en mesure de :

* Expliquez ce qu'est un conteneur et en quoi il diffère des machines virtuelles.
* Décrire les principes fondamentaux de l'orchestration des conteneurs.
* Discutez des défis de la mise en réseau et du stockage des conteneurs.
* Expliquer les avantages d'un maillage de services.

## Utilisation de conteneurs

L'histoire du développement d'applications est également l'histoire de l'emballage de ces applications pour différentes plates-formes et systèmes d'exploitation.

Prenons comme exemple une application Web simple écrite dans le langage de programmation populaire Python. Pour exécuter une application sur un serveur ou sur votre machine locale, le système doit généralement répondre à des exigences spécifiques telles que :

* Installer et configurer le système d'exploitation de base
* Installez les packages Python de base pour exécuter le programme
* Installez les extensions Python que votre programme utilise
* Configurer la mise en réseau de votre système
* Connectez-vous à des systèmes tiers comme une base de données, un cache ou un stockage.

Bien que le développeur connaisse le mieux son application et ses dépendances, c'est généralement un administrateur système qui fournit l'infrastructure, installe toutes les dépendances et configure le système sur lequel l'application s'exécute. Ce processus peut être très sujet aux erreurs et difficile à maintenir, de sorte que les serveurs ne sont configurés que dans un seul but, comme l'exécution d'une base de données ou d'un serveur d'applications, puis sont connectés par le réseau.

Pour obtenir une utilisation plus efficace du matériel du serveur, les machines virtuelles peuvent être utilisées pour émuler un serveur complet avec le processeur, la mémoire, le stockage, la mise en réseau, le système d'exploitation et le logiciel en plus. Cela permet d'exécuter plusieurs serveurs isolés sur le même matériel.

Avant l'adoption généralisée des conteneurs, la virtualisation des serveurs était le moyen le plus efficace d'exécuter des applications isolées et faciles à gérer, mais comme vous devez exécuter un système d'exploitation complet, y compris le noyau, cela entraîne toujours une surcharge si vous devez exécuter beaucoup de serveurs.

Les conteneurs peuvent être utilisés pour résoudre ces deux problèmes, en gérant les dépendances d'une application et en s'exécutant beaucoup plus efficacement que de faire tourner un grand nombre de machines virtuelles.

## Principes de base du conteneur

Contrairement à la croyance populaire, les technologies de conteneurs sont beaucoup plus anciennes qu'on ne le pense. L'un des premiers ancêtres des technologies de conteneurs modernes est la commande chroot qui a été introduite dans la version 7 d'Unix en 1979. La commande chroot peut être utilisée pour isoler un processus du système de fichiers racine et essentiellement "cacher" les fichiers du processus et simuler un nouveau répertoire racine. L'environnement isolé est ce qu'on appelle une prison chroot, où les fichiers ne sont pas accessibles par le processus, mais sont toujours présents sur le système.

|[Les répertoires chroot peuvent être créés à différents endroits du système de fichiers](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/zlgs2h3aa6bl-chrootdirectoriescanbecreatedonvariousplacesinthefilesystem.png)

Bien que le chroot soit une technologie assez ancienne, il est encore utilisé aujourd'hui par certains projets logiciels populaires. Les technologies de conteneurs que nous avons aujourd'hui incarnent toujours ce concept même, mais dans une version modernisée et avec de nombreuses fonctionnalités en plus.

Pour isoler un processus encore plus que ne peut le faire chroot, les noyaux Linux actuels fournissent des fonctionnalités telles que les espaces de noms et les cgroups.

Les espaces de noms sont utilisés pour isoler diverses ressources, par exemple le réseau. Un espace de noms réseau peut être utilisé pour fournir une abstraction complète des interfaces réseau et des tables de routage. Cela permet à un processus d'avoir sa propre adresse IP. Le noyau Linux 5.6 fournit actuellement 8 espaces de noms :

* pid - ID de processus fournit à un processus son propre ensemble d'ID de processus.
* net - network permet aux processus d'avoir leur propre pile réseau, y compris l'adresse IP.
* mnt - mount résume la vue du système de fichiers et gère les points de montage.
* ipc - la communication inter-processus permet de séparer les segments de mémoire partagée nommés.
* utilisateur - fournit au processus son propre ensemble d'ID utilisateur et d'ID de groupe.
* uts - Le partage de temps Unix permet aux processus d'avoir leur propre nom d'hôte et nom de domaine.
* cgroup - un espace de noms plus récent qui permet à un processus d'avoir son propre ensemble de répertoires racine de cgroup.
* time - l'espace de noms le plus récent peut être utilisé pour virtualiser l'horloge du système.
* Les cgroups sont utilisés pour organiser les processus en groupes hiérarchiques et leur attribuer des ressources telles que la mémoire et le processeur. Lorsque vous souhaitez limiter votre conteneur d'applications à, disons, 4 Go de mémoire, des groupes de contrôle sont utilisés sous le capot pour garantir ces limites.

Lancé en 2013, Docker est devenu synonyme de construction et de gestion de conteneurs. Bien que Docker n'ait pas inventé les technologies utilisées pour exécuter les conteneurs, ils ont assemblé les technologies existantes de manière intelligente pour rendre les conteneurs plus conviviaux et accessibles.

À première vue, les conteneurs semblent être très similaires aux machines virtuelles, mais il est crucial de comprendre qu'ils sont très différents. Alors que les machines virtuelles émulent une machine complète, y compris le système d'exploitation et un noyau, les conteneurs partagent le noyau de la machine hôte et, comme expliqué, ne sont que des processus isolés.

Les machines virtuelles entraînent des frais généraux, qu'il s'agisse du temps de démarrage, de la taille ou de l'utilisation des ressources pour exécuter le système d'exploitation. Les conteneurs, d'autre part, sont littéralement des processus, comme le navigateur que vous pouvez démarrer sur votre machine, ils démarrent donc beaucoup plus rapidement et ont une empreinte plus petite.

![Déploiement traditionnel vs déploiement virtualisé vs déploiement de conteneurs](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/zlgs2h3aa6bl-chrootdirectoriescanbecreatedonvariousplacesinthefilesystem.png)

Dans de nombreux cas, il ne s'agit pas de savoir si vous utilisez des conteneurs ou des machines virtuelles, vous utilisez plutôt les deux technologies pour bénéficier de l'efficacité des conteneurs tout en utilisant les avantages de sécurité que la plus grande isolation des machines virtuelles apporte à la table.

## Conteneurs en cours d'exécution

Pour exécuter des conteneurs aux normes de l'industrie, vous n'avez pas besoin d'utiliser Docker ; vous pouvez simplement suivre la norme OCI [runtime-spec](https://github.com/opencontainers/runtime-spec) à la place. L'Open Container Initiative gère également une implémentation de référence d'exécution de conteneur appelée [runC](https://github.com/opencontainers/runc). Ce runtime de bas niveau est utilisé dans une variété d'outils pour démarrer des conteneurs, y compris Docker lui-même.

Si vous êtes développeur et que vous connaissez la programmation orientée objet, vous pouvez imaginer la relation entre l'image du conteneur et le conteneur en cours d'exécution comme une classe et l'instanciation de cette classe.

Avec Docker installé, vous pouvez démarrer des conteneurs comme ceci :

```bash
docker run nginx
```

La spécification d'exécution va de pair avec la spécification d'image, que nous aborderons dans le chapitre suivant, car elle décrit comment décompresser une image de conteneur, puis gérer le cycle de vie complet du conteneur, de la création de l'environnement du conteneur au démarrage du processus. , en l'arrêtant et en le supprimant.

Pour votre machine locale, vous avez le choix entre de nombreuses alternatives, dont certaines ne servent qu'à créer des images comme [buildah](https://github.com/opencontainers/runc) ou [kaniko](https://github.com/GoogleContainerTools/kaniko), tandis que d'autres s'alignent comme des alternatives complètes à Docker, comme le fait [podman](https://podman.io/).

Podman fournit une API similaire à Docker et peut être utilisé en remplacement. De plus, il est livré avec quelques fonctionnalités supplémentaires comme l'exécution de conteneurs sans privilèges root et l'utilisation du concept Pod que nous découvrirons plus tard.

## Construire des images de conteneurs

Pourquoi un conteneur est-il appelé conteneur en premier lieu ? La métaphore utilisée ici vise l'utilisation de conteneurs maritimes normalisés selon la norme [ISO 668](https://en.wikipedia.org/wiki/ISO_668). Le format standard d'un conteneur maritime permet de l'empiler facilement sur un porte-conteneurs, de le décharger facilement avec une grue et sur un camion quoi qu'il arrive. à l'intérieur. Vous verrez que de nombreux termes dans le monde natif des conteneurs et du cloud suivent ce thème nautique.

Docker a réutilisé tous les composants pour isoler les processus tels que les espaces de noms et les groupes de contrôle, mais une pièce cruciale du puzzle qui a aidé les conteneurs à réaliser leur percée est l'introduction d'images de conteneurs.

Les images de conteneurs sont ce qui rend les conteneurs portables et faciles à réutiliser sur une variété de systèmes. [Docker](https://www.docker.com/resources/what-container) décrit une image de conteneur comme suit :

"Une image de conteneur Docker est un package logiciel léger, autonome et exécutable qui comprend tout le nécessaire pour exécuter une application : code, environnement d'exécution, outils système, bibliothèques système et paramètres."

En 2015, le format d'image rendu populaire par Docker a été donné à la nouvelle Open Container Initiative et est également connu sous le nom de [OCI image-spec](https://github.com/opencontainers/image-spec) qui peut être trouvée sur GitHub. Les images se composent d'un ensemble de systèmes de fichiers et de métadonnées.

![Images de conteneurs](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/695pmh5f8y49-Containerimages.png)

Les images peuvent être construites en lisant les instructions d'un fichier de construction appelé Dockerfile. Les instructions sont presque les mêmes que celles que l'on utiliserait pour installer une application sur un serveur. Voici un exemple de Dockerfile qui conteneurise un script Python :

```dockerfile
# Chaque image de conteneur commence par une image de base.
# Cela pourrait être votre distribution Linux préférée
FROM ubuntu:20.04

# Exécutez des commandes pour ajouter des logiciels et des bibliothèques à votre image
# Ici, nous installons python3 et le gestionnaire de paquets pip
RUN apt-get update && \
    apt-get -y install python3 python3-pip

# La commande de copie peut être utilisée pour copier votre code dans l'image
# Ici, nous copions un script appelé "mon-app.py" dans le système de fichiers des conteneurs
COPY mon-app.py /app/

# Définit le workdir dans lequel l'application s'exécute
# À partir de ce moment, tout sera exécuté dans /app
WORKSPACE /app

# Le processus qui doit être démarré lorsque le conteneur s'exécute
# Dans ce cas, nous démarrons notre application python "mon-app.py"
CMD ["python3","mon-app.py"]
```

Si vous avez installé Docker sur votre machine, vous pouvez créer l'image avec la commande suivante :

```shell
docker build -t mon-image-python -f Dockerfile
```

Avec les paramètres -t my-python-image, vous pouvez spécifier une balise de nom pour votre image, et avec -f Dockerfile, vous spécifiez où trouver votre Dockerfile. Cela donne aux développeurs la possibilité de gérer toutes les dépendances de leur application et de la préparer à l'exécution au lieu de laisser cette tâche à une autre personne ou équipe.

Pour distribuer ces images, vous pouvez utiliser un registre de conteneurs. Ce n'est rien de plus qu'un serveur Web sur lequel vous pouvez télécharger et télécharger des images. De manière pratique, Docker intègre les commandes push et pull :

```shell
docker push mon-registre.com/mon-image-python
docker pull mon-registre.com/mon-image-python
```

## Sécurité

Il est important de comprendre que les conteneurs ont des exigences de sécurité différentes de celles des machines virtuelles. Beaucoup de gens comptent sur les propriétés d'isolation des conteneurs, mais cela peut être très dangereux.

Lorsque les conteneurs sont démarrés sur une machine, ils partagent toujours le même noyau, ce qui devient alors un risque pour l'ensemble du système, si les conteneurs sont autorisés à appeler des fonctions du noyau comme par exemple tuer d'autres processus ou modifier le réseau hôte en créant des règles de routage. Vous pouvez en savoir plus sur les fonctionnalités du noyau dans la documentation Docker.

L'un des plus grands risques de sécurité, et pas seulement dans la zone des conteneurs, est l'exécution de processus avec trop de privilèges, en particulier le démarrage de processus en tant que root ou administrateur. Malheureusement, c'est un problème qui a été souvent ignoré dans le passé et il existe de nombreux conteneurs qui s'exécutent en tant qu'utilisateurs root.

Une surface d'attaque relativement nouvelle qui a été introduite avec les conteneurs est l'utilisation d'images publiques. Deux des registres d'images publiques les plus populaires sont [Docker Hub](https://hub.docker.com/) et [Quay](https://quay.io/) et bien qu'il soit formidable qu'ils fournissent des images accessibles au public, vous devez vous assurer que ces images n'ont pas été modifiées pour inclure des logiciels malveillants.

Sysdig a un excellent [article de blog sur la façon d'éviter de nombreux problèmes de sécurité et de créer des images de conteneurs sécurisées](https://sysdig.com/blog/dockerfile-best-practices/).

La sécurité en général n'est pas quelque chose qui ne peut être atteint qu'au niveau du conteneur. C'est un processus continu qui doit être adapté en permanence. Les 4C de la sécurité Cloud Native peuvent donner une idée approximative des couches qui doivent être protégées si vous utilisez des conteneurs. Assurez-vous de couvrir chaque couche car elle protège efficacement la couche à l'intérieur. La [documentation Kubernetes](https://kubernetes.io/docs/concepts/security/overview/) est un bon point de départ pour comprendre les couches.

![Les 4C de Cloud Native Security, extraits de la documentation de Kubernetes](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/8c2fwjtsjomf-4CsofCloudNativesecurity.png)

## Fondamentaux de l'orchestration de conteneurs

Il est assez facile d'exécuter quelques conteneurs sur votre machine locale ou sur un seul serveur, mais la façon dont les conteneurs sont utilisés introduit de nouveaux défis concernant les opérations des conteneurs. La grande efficacité du concept a entraîné des applications et des services de plus en plus petits et vous constaterez que les applications modernes peuvent être constituées de nombreux conteneurs.

Avoir beaucoup de petits conteneurs faiblement couplés, isolés et indépendants est la base des architectures dites de microservices. Ces petits conteneurs sont de petites parties autonomes de la logique métier qui font partie d'une application plus grande.

Si vous devez gérer et déployer de grandes quantités de conteneurs, vous arrivez rapidement au point où vous avez besoin d'un système qui aide à la gestion de ces conteneurs. Les problèmes à résoudre peuvent inclure :

* Fournir des ressources de calcul telles que des machines virtuelles sur lesquelles les conteneurs peuvent s'exécuter
* Planifiez des conteneurs sur des serveurs de manière efficace
* Allouer des ressources telles que le processeur et la mémoire aux conteneurs
* Gérer la disponibilité des conteneurs et les remplacer en cas de défaillance
* Mettre à l'échelle les conteneurs si la charge augmente
* Fournir une mise en réseau pour connecter les conteneurs entre eux
* Provisionnez le stockage si les conteneurs doivent conserver les données.

Les systèmes d'orchestration de conteneurs permettent de créer un cluster de plusieurs serveurs et d'héberger les conteneurs au-dessus. La plupart des systèmes d'orchestration de conteneurs se composent de deux parties : un plan de contrôle qui est responsable de la gestion des conteneurs et des nœuds de travail qui hébergent réellement les conteneurs.

Au fil des ans, plusieurs systèmes ont pu être utilisés pour l'orchestration, mais la plupart d'entre eux n'ont plus une grande importance aujourd'hui et l'industrie a choisi Kubernetes comme système standard pour orchestrer les conteneurs.

## La mise en réseau

L'architecture des microservices dépend fortement de la communication réseau. Contrairement aux applications monolithiques, un microservice implémente une interface qui peut être appelée pour faire une requête. Par exemple, vous pourriez avoir un service qui répond avec une liste de produits dans une application de commerce électronique.

Les espaces de noms réseau permettent à chaque conteneur d'avoir sa propre adresse IP unique, donc plusieurs applications peuvent ouvrir le même port réseau ; par exemple, vous pouvez avoir plusieurs serveurs Web conteneurisés qui ouvrent tous le port 8080.

Pour rendre l'application accessible depuis l'extérieur du système hôte, les conteneurs ont la possibilité de mapper un port du conteneur à un port du système hôte.

Pour permettre la communication entre les conteneurs à travers les hôtes, nous pouvons utiliser un réseau superposé qui les place dans un réseau virtuel qui s'étend sur les systèmes hôtes.

Cela permet aux conteneurs de communiquer très facilement entre eux tandis que les administrateurs système n'ont pas à configurer une mise en réseau et un routage complexes entre les hôtes et les conteneurs.

La plupart des réseaux superposés prennent également en charge la gestion des adresses IP, ce qui représenterait beaucoup de travail s'il était mis en œuvre manuellement. Dans ce cas, le réseau superposé gère quel conteneur obtient quelle adresse IP et comment le trafic doit circuler pour accéder aux conteneurs individuels.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/assg25dwbiz3-Routingbetweenhostsandcontainers.png)

La plupart des implémentations modernes de mise en réseau de conteneurs sont basées sur le [Container network interface (CNI)](https://github.com/containernetworking/cni). CNI est une norme qui peut être utilisée pour écrire ou configurer des plugins réseau et facilite l'échange de différents plugins dans diverses plates-formes d'orchestration de conteneurs.

## Découverte de services et DNS

Pendant longtemps, la gestion des serveurs dans les centres de données traditionnels était gérable. De nombreux administrateurs système se souvenaient même de toutes les adresses IP des systèmes importants avec lesquels ils devaient travailler. De grandes listes de serveurs, leurs noms d'hôte, adresses IP et objectifs - tous maintenus manuellement - étaient des affaires quotidiennes.

Dans les plateformes d'orchestration de conteneurs, les choses sont beaucoup plus compliquées :

* Des centaines ou des milliers de conteneurs avec des adresses IP individuelles
* Les conteneurs sont déployés sur des variétés de différents hôtes, différents centres de données ou même des géolocalisations
* Les conteneurs ou les services ont besoin de DNS pour communiquer. L'utilisation d'adresses IP est presque impossible
* Les informations sur les conteneurs doivent être supprimées du système lorsqu'elles sont supprimées.

La solution au problème est encore une fois l'automatisation. Au lieu d'avoir une liste de serveurs maintenue manuellement (ou dans ce cas des conteneurs), toutes les informations sont placées dans un registre de service. Trouver d'autres services dans le réseau et demander des informations à leur sujet s'appelle Service Discovery.

### Approches de la découverte de services

**DNS**
Les serveurs DNS modernes dotés d'une API de service peuvent être utilisés pour enregistrer de nouveaux services au fur et à mesure de leur création. Cette approche est assez simple, car la plupart des organisations disposent déjà de serveurs DNS dotés des fonctionnalités appropriées.

**Key-Value-Store**
Utilisation d'un magasin de données fortement cohérent, en particulier pour stocker des informations sur les services. De nombreux systèmes sont capables de fonctionner en haute disponibilité avec de solides mécanismes de basculement. Les choix populaires, en particulier pour le clustering, sont [etcd](https://github.com/coreos/etcd/), [Consul](https://www.consul.io/) ou [Apache Zookeeper](https://zookeeper.apache.org/).

## Service Mesh

Étant donné que la mise en réseau est un élément crucial des microservices et des conteneurs, la mise en réseau peut devenir très complexe et opaque pour les développeurs et les administrateurs. En plus de cela, de nombreuses fonctionnalités telles que la surveillance, le contrôle d'accès ou le cryptage du trafic réseau sont souhaitées lorsque les conteneurs communiquent entre eux.

Au lieu d'implémenter toutes ces fonctionnalités dans votre application, vous pouvez simplement démarrer un deuxième conteneur dans lequel cette fonctionnalité est implémentée. Le logiciel que vous pouvez utiliser pour gérer le trafic réseau s'appelle un proxy. Il s'agit d'une application serveur qui se situe entre un client et un serveur et peut modifier ou filtrer le trafic réseau avant qu'il n'atteigne le serveur. Les représentants populaires sont [nginx](https://www.nginx.com/), [haproxy](http://www.haproxy.org/) ou [envoy](https://www.envoyproxy.io/).

Poussant cette idée un peu plus loin, un maillage de services ajoute un serveur proxy à chaque conteneur que vous avez dans votre architecture.

![Architecture Istio, extraite de istio.io](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/t231ilorswu8-Istioarchitecture.png)

Référence:[Istio](https://istio.io/v1.10/docs/ops/deployment/architecture/)

Vous pouvez maintenant utiliser les proxys pour gérer la communication réseau entre vos services.

Prenons l'exemple du cryptage. Si deux applications ou plus devaient crypter leur trafic lorsqu'elles se parlent, cela nécessiterait l'ajout de bibliothèques et la configuration et la gestion de certificats numériques qui prouvent l'identité des applications impliquées. Cela peut représenter beaucoup de travail et peut également être source d'erreurs s'il n'est pas fait avec un soin particulier.

Lorsqu'un maillage de services est utilisé, les applications ne se parlent pas directement, mais le trafic est acheminé via les proxies à la place. Les maillages de services les plus populaires en ce moment sont [istio](https://istio.io/) et [linkerd](https://linkerd.io/). Bien qu'ils aient des différences d'implémentation, l'architecture est la même.

Les proxys d'un maillage de services forment le plan de données. C'est là que les règles de mise en réseau sont mises en œuvre et façonnent le flux de trafic.

Ces règles sont gérées de manière centralisée dans le plan de contrôle du maillage de services. C'est ici que vous définissez comment le trafic circule du service A au service B et quelle configuration doit être appliquée aux proxys.

Ainsi, au lieu d'écrire du code et d'installer des bibliothèques, vous écrivez simplement un fichier de configuration dans lequel vous indiquez au maillage de services que le service A et le service B doivent toujours communiquer de manière cryptée. La configuration est ensuite téléchargée sur le plan de contrôle et distribuée sur le plan de données pour appliquer la nouvelle règle.

Pendant longtemps, le terme "service mesh" n'a décrit qu'une idée de base de la manière dont le trafic dans les plates-formes de conteneurs pouvait être géré avec des proxys. Le projet [Service Mesh Interface (SMI)](https://smi-spec.io/) vise à définir une spécification sur la façon dont un maillage de services de différents fournisseurs peut être mis en œuvre. En mettant l'accent sur Kubernetes, leur objectif est de normaliser l'expérience de l'utilisateur final pour les maillages de services, ainsi qu'une norme pour les fournisseurs qui souhaitent s'intégrer à Kubernetes. Vous pouvez trouver la [spécification](https://github.com/servicemeshinterface/smi-spec) actuelle sur GitHub.

## Stockage

Du point de vue du stockage, les conteneurs ont un défaut important : ils sont éphémères. Pour comprendre ce que cela signifie exactement, nous devons comprendre ce qui se passe lorsqu'un conteneur démarre à partir d'une image de conteneur.

De manière générale, les images de conteneur sont en lecture seule et se composent de différentes couches qui incluent tout ce que vous avez ajouté pendant la phase de construction. Cela garantit que chaque fois que vous démarrez un conteneur à partir d'une image, vous obtenez le même comportement et les mêmes fonctionnalités. Comme vous pouvez probablement l'imaginer, de nombreuses applications ont besoin d'écrire des fichiers. Pour permettre l'écriture de fichiers, une couche en lecture-écriture est placée au-dessus de l'image du conteneur lorsque vous démarrez un conteneur à partir d'une image.

![Couches de conteneur, extraites de la documentation Docker](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/mup11dl71nvf-ContainerLayers.png)
Référence:[Docker](https://docs.docker.com/storage/storagedriver/)

Le problème ici est que cette couche de lecture-écriture est perdue lorsque le conteneur est arrêté ou supprimé. Tout comme la mémoire de votre ordinateur est effacée lorsque vous l'éteignez. Pour conserver les données, vous devez les écrire sur votre disque.

Si un conteneur doit conserver des données sur un hôte, un volume peut être utilisé pour y parvenir. Le concept et la technologie pour cela sont assez simples : au lieu d'isoler l'ensemble du système de fichiers d'un processus, les répertoires qui résident sur l'hôte sont transmis au système de fichiers du conteneur. Si vous pensez que cela fragilise l'isolation du conteneur, vous avez raison. Lorsque des volumes de conteneur sont utilisés, vous donnez effectivement accès au système de fichiers hôte.

![Les données sont partagées entre deux conteneurs sur le même hôte](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/du5zcuxz6i1b-Dataissharedbetweentwocontainersonthesamehost.png)

Lorsque vous orchestrez un grand nombre de conteneurs, la persistance des données sur l'hôte sur lequel le conteneur a été démarré peut ne pas être le seul défi. Souvent, les données doivent être accessibles par plusieurs conteneurs qui sont démarrés sur différents systèmes hôtes ou lorsqu'un conteneur démarre sur un hôte différent, il doit toujours avoir accès à son volume.

Les systèmes d'orchestration de conteneurs tels que Kubernetes peuvent aider à atténuer ces problèmes, mais nécessitent toujours un système de stockage robuste connecté aux serveurs hôtes.

Le stockage est provisionné via un système de stockage central. Les conteneurs sur le serveur A et le serveur B peuvent partager un volume pour lire et écrire des données

Afin de suivre la croissance ininterrompue de diverses implémentations de stockage, encore une fois, la solution consistait à implémenter une norme. La [Container storage interface (CSI)](https://github.com/container-storage-interface/spec) a été conçue pour offrir une interface uniforme qui permet de connecter différents systèmes de stockage, qu'il s'agisse d'un stockage dans le cloud ou sur site.


## Ressources additionnelles

En apprendre plus...

### L'historique des conteneurs

* A Brief History of Containers: From the 1970s Till Now, by Rani Osnat (2020)
* It's Here: Docker 1.0, by Julien Barbier (2014)

### Chroot

* chroot

### Container Performance

* Container Performance Analysis at DockerCon 2017, by Brendan Gregg

### Best Practices on How to Build Container Images

* Top 20 Dockerfile Best Practices, by Álvaro Iradier (2021)
* 3 simple tricks for smaller Docker images, by Daniele Polencic (2019)

### Best practices for building containers

* Alternatives to Classic Dockerfile Container Building
* Buildpacks vs Jib vs Dockerfile: Comparing containerization methods, by James Ward (2020)

### Service Discovery

* Service Discovery in a Microservices Architecture, by Chris Richardson (2015)

### Container Networking

* Kubernetes Networking Part 1: Networking Essentials, By Simon Kurth (2021)
* Life of a Packet (I), by Michael Rubin (2017)
* Computer Networking Introduction - Ethernet and IP (Heavily Illustrated), by Ivan Velichko (2021)

### Container Storage

* Managing Persistence for Docker Containers, by Janakiram MSV (2016)
* Container and Kubernetes Security
* Secure containerized environments with updated thread matrix for Kubernetes, by Yossi Weizman (2021)

### Docker Container Playground

* Play with Docker

