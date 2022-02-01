# KUBERNETES AND CLOUD NATIVE ESSENTIALS (LFS250)

## Aperçu des chapitres

Avec l'essor du cloud computing, les exigences et les possibilités de développement, de déploiement et de conception d'applications ont considérablement changé.

Les fournisseurs de cloud offrent une variété de services à la demande, à commencer par de simples serveurs (virtuels), la mise en réseau, le stockage, les bases de données et bien plus encore. Le déploiement et la gestion de ces services sont très pratiques, soit de manière interactive, soit à l'aide d'interfaces de programmation d'applications (API).

Dans ce chapitre, vous découvrirez les principes de l'architecture d'application moderne, souvent appelée architecture cloud native. Nous découvrirons ce qui rend ces applications natives des systèmes cloud et en quoi elles diffèrent des approches traditionnelles.

## Objectifs d'apprentissage

À la fin de ce chapitre, vous devriez être en mesure de :

* Discutez des caractéristiques de l'architecture cloud native.
* Expliquer les avantages de l'autoscaling et des modèles sans serveur.
* Comprendre l'importance des normes ouvertes et des projets communautaires dans les environnements cloud natifs.

## Fondamentaux de l'architecture cloud native

À la base, l'idée de l'architecture cloud native est d'optimiser votre logiciel pour la rentabilité, la fiabilité et une mise sur le marché plus rapide en utilisant une combinaison de modèles de conception culturels, technologiques et architecturaux.

Le terme natif du cloud peut être trouvé dans diverses définitions, dont certaines se concentrent sur les technologies, tandis que d'autres peuvent se concentrer sur l'aspect culturel des choses.

La Cloud Native Computing Foundation le définit comme suit :

« Les technologies cloud natives permettent aux organisations de créer et d'exécuter des applications évolutives dans des environnements modernes et dynamiques tels que les clouds publics, privés et hybrides. Les conteneurs, les maillages de services, les microservices, l'infrastructure immuable et les API déclaratives illustrent cette approche.

Ces techniques permettent des systèmes faiblement couplés qui sont résilients, gérables et observables. Combinés à une automatisation robuste, ils permettent aux ingénieurs d'apporter des modifications à fort impact fréquemment et de manière prévisible avec un minimum de canevas. [...] »

Les applications traditionnelles ou héritées sont généralement conçues avec une approche monolithique à l'esprit, ce qui signifie qu'elles sont autonomes et incluent toutes les fonctionnalités et tous les composants nécessaires pour accomplir une tâche. Une application monolithique a généralement une base de code unique et est fournie sous la forme d'un fichier binaire unique pouvant s'exécuter sur un serveur.

Si vous pensez à un logiciel de commerce électronique pour une boutique en ligne, une application monolithique inclurait toutes les fonctionnalités de l'interface utilisateur graphique, la liste des produits, le panier, le paiement, le traitement des commandes, et bien plus encore.

Bien qu'il puisse être très facile de développer et de déployer une application dans ce format, il peut être tout aussi difficile de gérer la complexité, de faire évoluer le développement entre plusieurs équipes, de mettre en œuvre des changements rapidement et de pouvoir faire évoluer l'application efficacement lorsqu'elle est soumise à de nombreuses contraintes. de charge.

L'architecture cloud native peut apporter des solutions à la complexité croissante des applications et à la demande croissante des utilisateurs. L'idée de base est de décomposer votre application en plus petits éléments, ce qui les rend plus faciles à gérer. Au lieu de fournir toutes les fonctionnalités dans une seule application, vous avez plusieurs applications découplées qui communiquent entre elles dans un réseau. Si nous nous en tenons à l'exemple précédent, vous pourriez avoir une application pour votre interface utilisateur, votre paiement et tout le reste. Ces petites applications indépendantes avec une portée de fonctions clairement définie sont souvent appelées microservices.

Cela permet d'avoir plusieurs équipes, chacune détenant la propriété de différentes fonctions de votre application, mais aussi de les exploiter et de les faire évoluer individuellement. Par exemple, si de nombreuses personnes essaient d'acheter des produits, vous pouvez mettre à l'échelle des services très sollicités, tels que le panier d'achat et le paiement.

![](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/ch9ud860v954-Monolithicvsmicroservices.png)

L'architecture cloud native peut présenter de nombreux avantages, mais peut également être très complexe à intégrer et doit donc répondre à certaines exigences pour fonctionner efficacement.

## Caractéristiques de l'architecture cloud native

### Caractéristiques de l'architecture cloud native

**Haut niveau d'automatisation**
Pour gérer toutes les parties mobiles de votre application cloud native, l'automatisation est recommandée à chaque étape, du développement au déploiement. Ceci peut être réalisé en utilisant des outils d'automatisation modernes et des pipelines d'intégration continue/livraison continue (CI/CD) (nous parlerons des pipelines plus tard dans le cours, mais pour l'instant, sachez qu'un pipeline CI/CD est un concept qui peut être utilisé pour les multiples étapes nécessaires lorsque vous livrez une nouvelle version de votre logiciel) qui sont soutenus par un système de contrôle de version comme git. La création, le test et le déploiement d'applications ainsi que d'une infrastructure avec une implication humaine minimale permettent des modifications rapides, fréquentes et incrémentielles de la production. Un système automatisé fiable permet également une reprise après sinistre beaucoup plus facile si vous devez reconstruire l'ensemble de votre système.

**Auto-guérison**
Les applications et l'infrastructure échouent de temps en temps. C'est normal, c'est pourquoi les frameworks d'application et les composants d'infrastructure natifs du cloud incluent des vérifications de l'état qui permettent de surveiller votre application de l'intérieur et de la redémarrer automatiquement si nécessaire. De plus, étant donné que l'application a été compartimentée, il est possible que seules certaines parties de votre application cessent de fonctionner ou ralentissent, contrairement à d'autres parties.

**Évolutif**
La mise à l'échelle de votre application décrit le processus de gestion de plus de charge tout en offrant une expérience utilisateur agréable. Une façon de mettre à l'échelle peut consister à démarrer plusieurs copies de la même application et à répartir la charge entre elles. L'automatisation de ce comportement en fonction de métriques d'application telles que le processeur ou la mémoire peut également garantir la disponibilité et les performances de vos services.

**(coût-) efficace**
Tout comme la mise à l'échelle de votre application pour les situations de trafic élevé, la réduction de votre application et les modèles de tarification basés sur l'utilisation des fournisseurs de cloud peuvent réduire les coûts si le trafic est faible. Pour optimiser l'utilisation de votre infrastructure, les systèmes d'orchestration tels que Kubernetes peuvent vous aider à placer les applications de manière plus efficace et plus dense.

**Facile à entretenir**
L'utilisation de microservices permet de décomposer les applications en plus petits morceaux et de les rendre plus portables, plus faciles à tester et à distribuer entre plusieurs équipes.

**Sécurisé par défaut**
Les environnements cloud sont souvent partagés entre plusieurs clients ou équipes, ce qui nécessite différents modèles de sécurité. Dans le passé, de nombreux systèmes étaient divisés en différentes zones de sécurité qui refusaient l'accès à différents réseaux ou personnes. Une fois que vous êtes à l'intérieur d'une zone, vous pouvez accéder à tous les systèmes à l'intérieur. Des modèles tels que l'informatique de confiance zéro atténuent cela en exigeant l'authentification de chaque utilisateur et processus.

Une bonne référence et un bon point de départ pour votre parcours cloud natif est l'application à douze facteurs. L'application à douze facteurs est une ligne directrice pour le développement d'applications cloud natives, qui commence par des choses simples comme le contrôle de version de votre base de code, une configuration sensible à l'environnement et des modèles plus sophistiqués comme l'apatridie et la simultanéité de votre application.

Bien que ces modèles et technologies offrent tous les avantages s'ils s'exécutent dans le cloud, ils peuvent également offrir de nombreux avantages lorsqu'ils sont appliqués à des systèmes sur site. Enfin, ils permettent une transition plus fluide si vous migrez vos applications et votre infrastructure vers le cloud.

## Mise à l'échelle automatique (Scalling)

Le modèle de mise à l'échelle automatique décrit l'ajustement dynamique des ressources en fonction de la demande actuelle. Le CPU et la mémoire sont les métriques évidentes pour décider quand faire évoluer les applications lorsque la charge augmente ou diminue, mais d'autres méthodes basées sur le temps ou les métriques commerciales peuvent également être envisagées pour faire évoluer vos services vers le haut ou vers le bas.

Généralement, lorsque nous parlons de mise à l'échelle automatique, nous parlons de mise à l'échelle horizontale, qui décrit le processus de génération de nouvelles ressources de calcul qui peuvent être de nouvelles copies de votre processus d'application, des machines virtuelles ou, de manière moins immédiate, même de nouveaux racks de serveurs. et autre matériel.

La mise à l'échelle verticale, quant à elle, décrit le changement de taille du matériel sous-jacent, qui ne fonctionne que dans certaines limites matérielles pour le bare metal, mais aussi pour les machines virtuelles. Les machines virtuelles et les processus peuvent être facilement mis à l'échelle en leur permettant de consommer plus de CPU et de mémoire, la limite supérieure étant définie par la capacité de calcul et de mémoire du matériel sous-jacent. Le matériel lui-même peut être mis à l'échelle, par exemple en ajoutant plus de RAM, mais uniquement jusqu'à ce que tous les emplacements de RAM soient occupés.

Pour illustrer la différence entre la mise à l'échelle verticale et horizontale, imaginez que vous devez porter un objet lourd que vous ne pouvez pas ramasser. Vous pouvez développer des muscles pour le porter vous-même, mais votre corps a une limite supérieure de force. C'est la mise à l'échelle verticale. Vous pouvez également appeler vos amis et leur demander de vous aider et de partager le travail. C'est la mise à l'échelle horizontale.

![Mise à l'échelle horizontale ou verticale](https://d36ai2hkxl16us.cloudfront.net/course-uploads/e0df7fbf-a057-42af-8a1f-590912be5460/q2cr3c5d6279-Horizontalvsverticalscaling.png)
Mise à l'échelle horizontale ou verticale

La configuration de l'autoscaling dans divers environnements nécessite de configurer une limite minimale et maximale d'instances (machines virtuelles ou conteneurs) et une métrique qui déclenche la mise à l'échelle. Afin de configurer la mise à l'échelle correcte, vous devez souvent exécuter de nombreux tests de charge (quasi-production) et analyser le comportement et l'équilibrage de charge lorsque votre application est mise à l'échelle.

Les environnements cloud qui s'appuient sur des modèles de tarification à la demande basés sur l'utilisation fournissent des plates-formes très efficaces pour la mise à l'échelle automatique, avec la possibilité de provisionner une grande quantité de ressources en quelques secondes ou même de passer à zéro, si les ressources ne sont temporairement pas nécessaires.

Même si la mise à l'échelle de vos applications et de l'infrastructure sous-jacente n'est pas automatisée au départ, la possibilité de faire évoluer votre application peut augmenter la disponibilité et la résilience de vos services dans des environnements plus traditionnels.

## Sans serveur (Serverless)

Contrairement à ce que laisse entendre le terme "sans serveur", les serveurs sont bien sûr toujours nécessaires comme base pour vos applications. Les fournisseurs de cloud suggèrent qu'il est très facile de déployer des applications, mais exigent que vous prépariez et configuriez plusieurs ressources comme un réseau, des machines virtuelles, des systèmes d'exploitation et des équilibreurs de charge pour exécuter une application Web simple. L'idée de l'informatique sans serveur est de soulager les développeurs de ces tâches compliquées. En un mot, vous pouvez simplement fournir le code de l'application, tandis que le fournisseur de cloud choisit le bon environnement pour exécuter votre application.

Tous les fournisseurs de cloud populaires proposent une ou plusieurs offres commerciales d'environnements d'exécution sans serveur propriétaires et un sous-ensemble sans serveur, également connu sous le nom de Function as a Service (FaaS). Le fournisseur de cloud abstrait l'infrastructure sous-jacente, afin que les développeurs puissent déployer des logiciels en téléchargeant leur code, par exemple sous forme de fichiers .zip, ou en fournissant une image de conteneur.

Contrairement à d'autres modèles de cloud computing, l'informatique sans serveur met davantage l'accent sur le provisionnement à la demande et la mise à l'échelle des applications. La mise à l'échelle automatique est un concept de base important du sans serveur et peut inclure la mise à l'échelle et le provisionnement en fonction d'événements tels que les demandes entrantes. Cela permet une facturation encore plus précise, qui peut être basée sur les événements mentionnés au lieu de la facturation basée sur le temps largement répandue.

Au lieu de remplacer entièrement les plates-formes d'orchestration de conteneurs ou les machines virtuelles plus traditionnelles, les systèmes FaaS sont souvent utilisés en combinaison ou en tant qu'extension de plates-formes existantes, car ils permettent un déploiement très rapide et constituent d'excellents environnements de test et de bac à sable.

Alors que les images de conteneurs sont un excellent moyen standardisé d'emballer des logiciels pour les systèmes FaaS ou sans serveur, des systèmes comme Knative qui sont construits sur Kubernetes permettent d'étendre les plates-formes existantes avec des capacités informatiques sans serveur. Cela peut être une solution viable pour un fonctionnement sans serveur dans des clouds privés et des environnements sur site. Gardez à l'esprit que cela peut faciliter le travail d'un développeur, mais augmenter la complexité de l'exploitation d'une plate-forme cloud.

Bien que la technologie sans serveur présente de nombreux avantages, elle a d'abord eu du mal à se normaliser. De nombreux fournisseurs de cloud proposent des offres propriétaires qui rendent difficile le passage d'une plate-forme à l'autre. Pour résoudre ces problèmes, le projet CloudEvents a été fondé et fournit une spécification de la manière dont les données d'événement doivent être structurées. Les événements constituent la base de la mise à l'échelle des charges de travail sans serveur ou du déclenchement des fonctions correspondantes. Plus les fournisseurs et les outils adoptent une telle norme, plus il devient facile d'utiliser des architectures sans serveur et pilotées par les événements sur plusieurs plates-formes.

Les applications écrites pour les plates-formes sans serveur ont des exigences encore plus strictes pour l'architecture cloud native, mais en même temps, elles peuvent en tirer le meilleur parti. L'écriture de petites applications sans état en fait une solution idéale pour les flux d'événements ou de données, les tâches planifiées, la logique métier ou le traitement par lots.

## Normes ouvertes (Open standards)

De nombreuses technologies natives du cloud s'appuient fortement sur des logiciels open source, ce qui peut faciliter le démarrage et empêcher le blocage des fournisseurs. Tout le monde peut collaborer à un projet open source, ce qui facilite la mise en œuvre de normes à l'échelle de l'industrie qui se retrouvent même dans les produits commerciaux et les offres des fournisseurs de cloud.

Un problème courant est de savoir comment créer et distribuer des packages logiciels, car les applications ont de nombreuses exigences et dépendances pour le système d'exploitation sous-jacent et l'exécution de l'application. Pour surmonter ce problème, les conteneurs ont évolué pour devenir un moyen standardisé de conditionner et d'expédier des logiciels modernes. Alors que Docker est souvent utilisé comme synonyme de technologies de conteneurs, la communauté s'est engagée à respecter la norme industrielle ouverte de l'Open Container Initiative (OCI).

Sous l'égide de la Linux Foundation, l'Open Container Initiative fournit deux normes qui définissent la manière de créer et d'exécuter des conteneurs. La spécification d'image définit comment créer et empaqueter des images de conteneur. Alors que la spécification d'exécution spécifie la configuration, l'environnement d'exécution et le cycle de vie des conteneurs. Un ajout plus récent au projet OCI est Distribution-Spec, qui fournit une norme pour la distribution de contenu en général et d'images de conteneurs en particulier.

Des standards ouverts comme celui-ci aident et complètent d'autres systèmes comme Kubernetes, qui est la plate-forme standard de facto pour orchestrer les conteneurs. Quelques normes que vous découvrirez dans les chapitres suivants sont :

* OCI Spec : spécification d'image, d'exécution et de distribution sur la façon d'exécuter, de créer et de distribuer des conteneurs
* Container Network Interface (CNI) : une spécification sur la façon de mettre en œuvre la mise en réseau pour les conteneurs.
* Container Runtime Interface (CRI) : spécification sur la manière d'implémenter les runtimes de conteneurs dans les systèmes d'orchestration de conteneurs.
* Interface de stockage de conteneurs (CSI) : spécification sur la manière d'implémenter le stockage dans les systèmes d'orchestration de conteneurs.
* Service Mesh Interface (SMI) : spécification Ap sur la façon de mettre en œuvre des maillages de service dans les systèmes d'orchestration de conteneurs en mettant l'accent sur Kubernetes.
* Suivant cette approche, d'autres systèmes comme Prometheus ou OpenTelemetry ont évolué et prospéré dans cet écosystème et fournissent des normes supplémentaires pour la surveillance et l'observabilité.

##Rôles natifs du cloud et ingénierie de la fiabilité des sites

Bien sûr, le changement technologique et culturel que nous avons connu au cours des deux dernières décennies a entraîné un ajustement des tâches et des descriptions de poste. Les offres d'emploi précédentes comprenaient des postes d'administrateur système, de réseau ou de base de données, de développeur de logiciels ou de responsable de test.

Les métiers du cloud computing sont plus difficiles à décrire et les transitions sont plus fluides, car les responsabilités sont souvent partagées entre plusieurs personnes venant de différents domaines et avec des compétences différentes.

### Rôles cloud natifs

**Architecte Cloud**
Responsable de l'adoption des technologies cloud, de la conception du paysage et de l'infrastructure des applications, en mettant l'accent sur la sécurité, l'évolutivité et les mécanismes de déploiement.

**Ingénieur DevOps**
Souvent décrit comme une simple combinaison de développeur et d'administrateur, mais cela ne rend pas justice au rôle. Les ingénieurs DevOps utilisent des outils et des processus qui équilibrent le développement logiciel et les opérations. En commençant par des approches d'écriture, de création et de test de logiciels tout au long du cycle de vie du déploiement.

**Ingénieur Sécurité**
Peut-être le rôle le plus facile à saisir. Néanmoins, le rôle des ingénieurs en sécurité a considérablement changé. Les technologies cloud ont créé de nouveaux vecteurs d'attaque et aujourd'hui, le rôle doit être vécu de manière beaucoup plus inclusive et en tant que partie intégrante d'une équipe.

**Ingénieur DevSecOps**
Dans un effort pour faire de la sécurité une partie intégrante des environnements informatiques modernes, l'ingénieur DevSecOps combine les rôles des deux précédents. Ce rôle est souvent utilisé pour établir des ponts entre les équipes de développement et de sécurité plus traditionnelles.

**Fermer Data Engineer**
Les ingénieurs de données sont confrontés au défi de collecter, stocker et analyser les grandes quantités de données qui sont ou peuvent être collectées dans de grands systèmes. Cela peut inclure le provisionnement et la gestion d'une infrastructure spécialisée, ainsi que l'utilisation de ces données.

**Développeur Full-Stack**
Un polyvalent qui est à l'aise dans le développement frontend et backend, ainsi que dans les éléments essentiels de l'infrastructure.

**Site Reliability Engineer (SRE)**
Un rôle avec une définition plus forte est l'ingénieur de fiabilité du site (SRE). SRE a été fondée vers 2003 chez Google et est devenue un travail important pour de nombreuses organisations. L'objectif principal de SRE est de créer et de maintenir un logiciel fiable et évolutif. Pour y parvenir, des approches d'ingénierie logicielle sont utilisées pour résoudre les problèmes opérationnels et automatiser les tâches d'exploitation.

Pour mesurer les performances et la fiabilité, les SRE utilisent trois métriques principales :

Objectifs de niveau de service (SLO) : "Spécifiez un niveau cible pour la fiabilité de votre service." - Un objectif qui est fixé, par exemple atteindre une latence de service inférieure à 100ms.
Indicateurs de niveau de service (SLI) : "Une mesure quantitative soigneusement définie d'un aspect du niveau de service fourni" - Par exemple, combien de temps une demande doit réellement recevoir une réponse.
Accords de niveau de service (SLA) : "Un contrat explicite ou implicite avec vos utilisateurs qui inclut les conséquences du respect (ou de l'absence) des SLO qu'ils contiennent. Les conséquences sont plus facilement reconnaissables lorsqu'elles sont financières - une remise ou une pénalité - mais elles peuvent prendre d'autres formes. - Répond à la question que se passe-t-il si les SLO ne sont pas atteints.
Autour de ces métriques, les SRE peuvent définir un budget d'erreur. Un budget d'erreur définit la quantité (ou le temps) d'erreurs que votre application peut avoir, avant que des actions ne soient prises, comme l'arrêt des déploiements en production.

## Communauté et gouvernance

De nombreux projets open source considérés par beaucoup comme la norme de l'industrie sont hébergés et pris en charge par la Cloud Native Computing Foundation (CNCF). Les projets sont classés en fonction de leur maturité et passent par une étape de bac à sable et d'incubation avant d'obtenir leur diplôme. La forte communauté de la CNCF peut soutenir les projets tout au long de leur cycle de vie, en commençant par des choses comme la visibilité publique et la classification dans le paysage CNCF jusqu'à ce que les projets arrivent à maturité pour une utilisation en production.

La CNCF dispose d'un comité de suivi technique (TOC) qui est chargé de définir et de maintenir la vision technique, d'approuver les nouveaux projets, d'accepter les commentaires du comité des utilisateurs finaux et de définir les pratiques communes qui doivent être mises en œuvre dans les projets de la CNCF.

Dans le même temps, le TOC ne contrôle pas les projets, mais les encourage à être autonomes et appartenant à la communauté et applique le principe de « gouvernance minimale viable ». Les directives pour ces projets incluent la manière dont les projets sont maintenus, révisés et publiés, quels sont ses utilisateurs et ses groupes de travail, et bien plus encore. Étant donné que les projets open source dépendent de leurs communautés respectives, la gouvernance est très différente des approches traditionnelles. La grande liberté des technologies cloud natives oblige les gens à s'imposer des règles et à les faire respecter.

## Additional Resources
Learn more about...

Cloud Native Architecture

* [Adoption of Cloud-Native Architecture, Part 1: Architecture Evolution and Maturity, by Srini Penchikala, Marcio Esteves, and Richard Seroter (2019)]()
* [5 principles for cloud-native architecture-what it is and how to master it, by Tom Grey (2019)]()
* [What is cloud native and what are cloud native applications?]()
* [CNCF Cloud Native Interactive Landscape]()

Well-Architected Framework

* [Google Cloud Architecture Framework]()
* [AWS Well-Architected Framework]()
* [Microsoft Azure Well-Architected Framework]()

Microservices

* [What are microservices?]()
* [Microservices, by James Lewis and Martin Fowler]()
* [Adopting Microservices at Netflix: Lessons for Architectural Design]()

Serverless

* [The CNCF takes steps toward serverless computing, by Kristen Evans (2018)]()
* [CNCF Serverless Whitepaper v1.0 (2019)]()

Serverless Architecture

* [Site Reliability Engineering]()
* [SRE Book, by Benjamin Treynor Sloss (2017)]()
* [DevOps, SRE, and Platform Engineering, by Ivan Velicho (2021)]()

[Référence](https://trainingportal.linuxfoundation.org/learn/course/kubernetes-and-cloud-native-essentials-lfs250/cloud-native-architecture/cloud-native-architecture)
