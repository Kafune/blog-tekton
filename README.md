# *Tekton: De Toekomst van CI/CD voor Cloud-Native Apps op Kubernetes*
<img src="https://tekton.dev/images/tekton-horizontal-color.png" width="200>" align="right">

In een snel evoluerend landschap van cloud-native applicaties, speelt Tekton een cruciale rol als een open-source framework ontworpen voor het bouwen, testen en implementeren van deze moderne applicaties. Specifiek gericht op Continuous Integration/Continuous Deployment (CI/CD) pipelines binnen een Kubernetes-omgeving, biedt Tekton een gestandaardiseerde en krachtige oplossing.

## Wat is Tekton?

Tekton is een open-source framework dat wordt gebruikt voor het bouwen, testen en implementeren van cloud-native applicaties. Het is specifiek ontworpen om CI/CD (Continuous Integration/Continuous Deployment) pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Hierdoor kunnen ontwikkelaars via cloud providers zoals Kubernetes hun applicatie bouwen, testen en deployen [(Tekton, z.d)](https://tekton.dev/).

Er zijn aardig wat overeenkomsten tussen andere CI/CD Tools. Het grootste verschil is dat Tekton vooral de nadruk op containerisatie zet. Daarom is Tekton op Kubernetes gebouwd. Tekton maakt en voert alle instanties van taken en pipelines uit op verschillende containers. Deze pipelines zijn van tevoren te configureren met YAML bestanden. Bij Gitlab CI/CD is er een voorgeconfigureerde Gitlab runner nodig om pipelines te kunnen draaien.  

## Hoe werkt tekton?

Tekton werkt door gebruik te maken van een reeks van Kubernetes-resources die samenwerken om CI/CD-pipelines te definiëren en uit te voeren. Hierdoor is het makkelijker om Tekton met een bestaande Kubernetes infrastructuur te integreren[^bron](https://www.stakater.com/post/github-actions-vs-bitbucket-pipelines-vs-gitlab-ci-vs-tekton-bestcicdtool). De belangrijkste kenmerken hierbij zijn:
![https://tekton.dev/docs/concepts/concept-model/](https://tekton.dev/docs/concepts/concept-tasks-pipelines.png)
**Figuur 2**

**Resources:** Tekton maakt gebruik van resources om input- en outputgegevens van een pipeline te beheren. Dit kunnen bijvoorbeeld broncode-repositories, container-images, configuratiebestanden of andere artefacten zijn.

**Steps**: Een step is een uitvoering van een commando in CI/CD, zoals het uitvoeren van unit tests, of het implementeren van een applicatie. Tekton voert elke stap uit met een Docker container image[Tekton overview](https://tekton.dev/docs/concepts/overview/).

**Tasks:** Een taak bestaat uit een collectie van **steps**. Het is een geautomatiseerde stap die specifieke acties uitvoert, zoals het bouwen van een containerimage, het uitvoeren van tests of. In Gitlab is dit vergelijkbaar met de script annotatie.

**Pipeline:** Een pipeline in Tekton bestaat uit een collectie **tasks** die Tekton op een bepaalde volgorde uitvoert. Hiermee kunnen ontwikkelaars complexe workflows definiëren die bestaan uit meerdere taken.

![Voorbeelden toepassing pipeline](https://tekton.dev/docs/concepts/concept-runs.png)**Figuur 3**

**TaskRuns**: Toont alle instanties met de type **tasks**

**PipelineRuns**: Toont alle instanties met de type **pipeline**.

**Trigger:** Tekton kan worden geïntegreerd met triggersystemen, waardoor pipelines kunnen worden geactiveerd op basis van gebeurtenissen zoals codecommits, pull-aanvragen of andere gebeurtenissen in een versiebeheersysteem.

**Workspaces:** Workspaces bieden een manier om gegevens tussen taken in een pipeline te delen. Ze stellen taken in staat om gegevens te schrijven naar en te lezen uit een gedeelde opslagplaats.

Over het algemeen biedt Tekton een gestandaardiseerde manier om CI/CD-pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Het is ontworpen om eenvoudig te integreren met andere tools en systemen, waardoor het een populaire keuze is geworden voor teams die Kubernetes gebruiken voor containerorkestratie.

## Kenmerken en voordelen

Tekton maakt gebruik van YAML-bestanden om CI/CD-pipelines te definiëren. Dit maakt het gemakkelijk te begrijpen en te beheren, en bevordert een infrastructuur-als-code aanpak. Dit is in principe hetzelfde als Gitlab en Github.

Ook brengt Tekton een aantal voordelen voor developers die CI/CD systemen bouwen:

- Platformonafhankelijkheid: Tekton draait op Kubernetes als een set van [Kubernetes Custom Resource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), waardoor Tekton niet afhankelijk is van een specifieke platform en los draait van andere Kubernetes resources.
- Native Kubernetes-integratie: Tekton is ontworpen om naadloos samen te werken met Kubernetes. Het maakt gebruik van Kubernetes-podspecs om taken uit te voeren, waardoor het eenvoudig kan worden geïntegreerd in bestaande Kubernetes-omgevingen [^1](https://platform9.com/blog/argo-cd-vs-tekton-vs-jenkins-x-finding-the-right-gitops-tooling/).
- Declaratieve configuratie: Tekton maakt gebruik van YAML-bestanden voor het definiëren van pipelines, taken en resources. Dit zorgt voor een eenvoudige en begrijpelijke manier om CI/CD-pijplijnen te definiëren en te onderhouden. Het is hierdoor makkelijker om deze pipelines in verschillende projecten neer te zetten door de herbruikbaarheid als de configuratie op Git staat.
- Schaalbaarheid: Door gebruik te maken van Kubernetes voor het orkestreren van taken, profiteert Tekton van de schaalbaarheid en betrouwbaarheid van het Kubernetes-platform. Hierdoor kunnen teams pipelines uitvoeren op een schaal die geschikt is voor hun project, bijvoorbeeld meerdere microservices met een of meerdere pipelines. Als er een vraag naar capaciteit is, is het zo simpel als meer nodes aan een Kubernetes cluster toevoegen. Het is dan niet nodig om de pipeline aan te passen omdate Tekton automatisch schaalt [voordelen](https://tekton.dev/docs/concepts/overview/).
- Uitbreidbaar. Via [Tekton Hub](https://hub.tekton.dev/) is het makkelijk om nieuwe pipelines te maken met voorgedefinieerde componenten of gebruik te maken van pipelines die anderen hebben gemaakt.
- Combinatie ArgoCD: Waar Tekton voor het hele development zorgt, zoals het bouwen van de applicatie tot het pushen naar een git repository, kan ArgoCD met de Git repo een nieuwe Kubernetes cluster uitrollen [(piotr)](https://piotrminkowski.com/2021/08/05/kubernetes-ci-cd-with-tekton-and-argocd/)

![Werking Tekton & ArgoCD](https://i0.wp.com/piotrminkowski.com/wp-content/uploads/2021/08/tekton-argocd-kubernetes.png?resize=1024%2C362&ssl=1) 

## Gebruik van Tekton: Clonen Pitstop

Minimale Vooreisen:

```text
Minikube: lokaal draaien van Kubernetes
Kubectl: Command line voor Kubernetes
tkn: Command line voor Tekton
```

Tkn is op [meerdere manieren](https://tekton.dev/docs/cli/) te installeren. Zelf heb ik het met Chocolatey voor Windows gedaan, aangezien een package manager het meeste werk voor je doet.

[community hub](https://hub.tekton.dev/)
Stappen:

```text
0. Installeer Git clone volgens tutorial
- tkn hub install task git-clone

1. start kubernetes cluster
- minikube start --kubernetes-version v1.27.4
2. installeer Tekton pipelines
- kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
3. Apply bestanden
- kubectl apply -f show-readme.yaml
- kubectl apply -f pipeline.yaml
- kubectl create -f pipelinerun.yaml
- tkn pipelinerun logs  clone-read-run-4kgjr -f

- Ergens ertussen nog secret genereren
kubectl create secret generic ssh-secret --from-file=ssh-privatekey=C:\Users\Kachu\.ssh\id_rsa
```

1. Start Kubernetes lokaal op met minikube:

```text
minikube start --kubernetes-version v1.27.4
```

2. Installeer Tekton

```text
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

2. Maak nieuwe Pipeline aan genaamd `pipeline.yaml`:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: clone-read
```

## Best practices

Aantal best practices bij het schrijven van Tasks en pipelines:

- **Modulaire Pipelines**: Houd pipelines modulair door ze op te delen in kleinere taken en resources. Dit maakt het gemakkelijker om ze te begrijpen, onderhouden en hergebruiken.

- **Gebruik van GitOps**: Beheer de declaratieve beschrijving van pipelines en resources in een Git-repository. Hierdoor kunnen wijzigingen gevolgd worden, kunnen ze gereviewd worden en kan versiebeheer worden toegepast.

- **Logging en Tracing**: Implementeer logging en tracing in je pipelines. Dit helpt bij het oplossen van problemen en het identificeren van bottlenecks.

- **Gebruik van Workspaces**: Workspaces bieden een manier om gegevens tussen stappen in een pipeline te delen. Zorg ervoor dat workspaces goed worden gebruikt om gegevens efficiënt door te geven.
- **Beveiliging**: Zorg ervoor dat de nodige beveiligingsmaatregelen worden toegepast, zoals het beperken van de toegang tot resources, het gebruik van beveiligde verbindingen en het toepassen van de principes van de minste privileges.
- **Monitoring en Waarschuwingen**: Implementeer monitoring en waarschuwingen om de status van je pipelines te bewaken en op de hoogte te blijven van eventuele problemen.

## Uitdagingen

Het implementeren van Tekton CI/CD in een ontwikkelteam kan veel voordelen bieden, zoals gestroomlijnde en geautomatiseerde deployment pipelines. Echter, net zoals bij elke nieuwe technologie, zijn er ook uitdagingen die kunnen optreden. Hier zijn enkele mogelijke uitdagingen en oplossingen bij het implementeren van Tekton CI/CD:

Uitdagingen:

- **Leercurve**: Ontwikkelaars kunnen tijd nodig hebben om vertrouwd te raken met de Tekton-pijplijnconfiguratie en -implementatie. (En kubernetes)

- **Bestaande CI/CD-systemen**: Als je al bestaande CI/CD-tools en -processen hebt, kan het integreren van Tekton met deze systemen een uitdaging zijn. Dit kan aanpassingen aan bestaande workflows vereisen.

- **Complexiteit van de pipeline-definities**: Het kan complex lijken om Tekton-pijplijnen goed te configureren, vooral voor complexe applicaties met veel afhankelijkheden. Dit is deels verholpen als er kennis is van andere CI/CD systemen.

- **Resource Management**: Het beheren van resources zoals Workspaces, Secrets en ConfigMaps kan complex zijn, vooral als er een complexe applicatiestructuur is.

- **Beveiliging en Toegangsbeheer**: Het correct configureren van beveiliging en toegangsbeheer is cruciaal, maar kan complex zijn, vooral in grotere organisaties met meerdere teams en stakeholders.

## Conclusie

Tekton biedt een gestandaardiseerde en flexibele oplossing voor CI/CD binnen Kubernetes-omgevingen. Met zijn sterke focus op containerisatie, native integratie met Kubernetes en schaalbaarheid, staat Tekton als een toekomstgerichte keuze voor teams die werken met cloud-native applicaties.

## Bronnen

[https://www.hcs-company.com/blog/tekton]
Gitops en Tekton
[https://production-gitops.dev/guides/cp4i/apic/cluster-config/gitops-tekton-argocd/]

AWS Tekton
[https://aws.amazon.com/blogs/containers/cloud-native-ci-cd-with-tekton-and-argocd-on-aws/#:~:text=Tekton%20is%20an%20open%2Dsource,natively%20on%20top%20of%20Kubernetes.]

[https://cloud.google.com/tekton]
