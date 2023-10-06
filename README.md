# *Tekton: De Toekomst van CI/CD voor Cloud-Native Apps op Kubernetes*

<img src="https://tekton.dev/images/tekton-horizontal-color.png" width="200>" align="right">

Dit blog is geschreven om inzicht te geven op een DevOps tool voor de DevOps minor. In dit geval gaat het over Tekton CI/CD. Het doel van deze blog is om ontwikkelaars kennis te laten maken met Tekton. Het is sterk aangeraden om al kennis te hebben van Kubernetes en Linux om commando's uit te voeren in de pipelines. Kennis van andere CI/CD systemen is een pré.

Tekton speelt een cruciale rol als een open-source framework ontworpen voor het bouwen, testen en implementeren van moderne applicaties. Specifiek gericht op Continuous Integration/Continuous Deployment (CI/CD) pipelines binnen een Kubernetes-omgeving, biedt Tekton een gestandaardiseerde en krachtige oplossing.

## Wat is Tekton?

Tekton is een open-source framework dat wordt gebruikt voor het bouwen, testen en implementeren van cloud-native applicaties. Het is specifiek ontworpen om CI/CD (Continuous Integration/Continuous Deployment) pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Hierdoor kunnen ontwikkelaars via cloud providers zoals Kubernetes hun applicatie bouwen, testen en deployen (Tekton, z.d.).

Er zijn aardig wat overeenkomsten tussen andere CI/CD Tools. Het grootste verschil is dat Tekton vooral de nadruk op containerisatie zet. Daarom is Tekton op Kubernetes gebouwd. Tekton maakt en voert alle instanties van taken en pipelines uit op verschillende containers. Deze pipelines zijn van tevoren te configureren met YAML bestanden. Bij Gitlab CI/CD is er een voorgeconfigureerde Gitlab runner nodig om pipelines te kunnen draaien.  

## Hoe werkt tekton?

Tekton werkt door gebruik te maken van een reeks van Kubernetes-resources die samenwerken om CI/CD-pipelines te definiëren en uit te voeren. Hierdoor is het makkelijker om Tekton met een bestaande Kubernetes infrastructuur te integreren (Amir, 2023). De belangrijkste kenmerken hierbij zijn:
![https://tekton.dev/docs/concepts/concept-model/](https://tekton.dev/docs/concepts/concept-tasks-pipelines.png)
**Figuur 2: Een Taskrun dat meerdere tasks bevat in een pipeline**

**Resources:** Tekton maakt gebruik van resources om input- en outputgegevens van een pipeline te beheren. Dit kunnen bijvoorbeeld broncode-repositories, container-images, configuratiebestanden of andere artefacten zijn.

**Steps**: Een step is een uitvoering van een commando in CI/CD, zoals het uitvoeren van unit tests, of het implementeren van een applicatie. Tekton voert elke stap uit met een Docker container image (Overview, 2023).

**Tasks:** Een taak bestaat uit een collectie van *steps*. Het is een geautomatiseerde stap die specifieke acties uitvoert, zoals het bouwen van een containerimage, het uitvoeren van tests of. In Gitlab is dit vergelijkbaar met de script annotatie.

**Pipeline:** Een pipeline in Tekton bestaat uit een collectie *tasks* die Tekton op een bepaalde volgorde uitvoert. Hiermee kunnen ontwikkelaars complexe workflows definiëren die bestaan uit meerdere taken.

![Voorbeelden toepassing pipeline](https://tekton.dev/docs/concepts/concept-runs.png)**Figuur 3: Voorbeeld van een Pipelinerun dat op een bepaalde trigger draait**

**TaskRuns**: Toont alle instanties met de type *task*

**PipelineRuns**: Toont alle instanties met de type *pipeline*.

**Trigger:** Tekton kan worden geïntegreerd met triggersystemen, waardoor pipelines kunnen worden geactiveerd op basis van gebeurtenissen zoals codecommits, pull-aanvragen of andere gebeurtenissen in een versiebeheersysteem.

**Workspaces:** Workspaces bieden een manier om gegevens tussen taken in een pipeline te delen. Ze stellen taken in staat om gegevens te schrijven naar en te lezen uit een gedeelde opslagplaats.

Over het algemeen biedt Tekton een gestandaardiseerde manier om CI/CD-pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Het is ontworpen om eenvoudig te integreren met andere tools en systemen, waardoor het een populaire keuze is geworden voor teams die Kubernetes gebruiken voor containerorkestratie.

## Kenmerken en voordelen

Tekton maakt gebruik van YAML-bestanden om CI/CD-pipelines te definiëren. Dit maakt het gemakkelijk te begrijpen en te beheren, en bevordert een infrastructuur-als-code aanpak. Dit is in principe hetzelfde als Gitlab en Github.

Ook brengt Tekton een aantal voordelen voor developers die CI/CD systemen bouwen:

- Platformonafhankelijkheid: Tekton draait op Kubernetes als een set van [Kubernetes Custom Resource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), waardoor Tekton niet afhankelijk is van een specifieke platform en los draait van andere Kubernetes resources (Cloud Native CI/CD with Tekton and ArgoCD on AWS | Amazon Web Services, 2022).
- Native Kubernetes-integratie: Tekton is ontworpen om naadloos samen te werken met Kubernetes. Het maakt gebruik van Kubernetes-podspecs om taken uit te voeren, waardoor het eenvoudig kan worden geïntegreerd in bestaande Kubernetes-omgevingen [^1](https://platform9.com/blog/argo-cd-vs-tekton-vs-jenkins-x-finding-the-right-gitops-tooling/).
- Declaratieve configuratie: Tekton maakt gebruik van YAML-bestanden voor het definiëren van pipelines, taken en resources. Dit zorgt voor een eenvoudige en begrijpelijke manier om CI/CD-pijplijnen te definiëren en te onderhouden. Het is hierdoor makkelijker om deze pipelines in verschillende projecten neer te zetten door de herbruikbaarheid als de configuratie op Git staat.
- Schaalbaarheid: Door gebruik te maken van Kubernetes voor het orkestreren van taken, profiteert Tekton van de schaalbaarheid en betrouwbaarheid van het Kubernetes-platform. Hierdoor kunnen teams pipelines uitvoeren op een schaal die geschikt is voor hun project, bijvoorbeeld meerdere microservices met een of meerdere pipelines. Als er een vraag naar capaciteit is, is het zo simpel als meer nodes aan een Kubernetes cluster toevoegen. Het is dan niet nodig om de pipeline aan te passen omdat Tekton automatisch schaalt (Overview, 2023).
- Uitbreidbaar. Via [Tekton Hub](https://hub.tekton.dev/) is het makkelijk om nieuwe pipelines te maken met voorgedefinieerde componenten of gebruik te maken van pipelines die anderen hebben gemaakt (Gravestijn, z.d.).
- Combinatie ArgoCD: Waar Tekton voor het hele development zorgt, zoals het bouwen van de applicatie tot het pushen naar een git repository, kan ArgoCD met de Git repo een nieuwe Kubernetes cluster uitrollen (Piotr.Minkowski, 2021).
![Werking Tekton & ArgoCD](https://i0.wp.com/piotrminkowski.com/wp-content/uploads/2021/08/tekton-argocd-kubernetes.png?resize=1024%2C362&ssl=1) **Figuur 3: Tekton en ArgoCD gecombineerd**

## Uitlezen README van Pitstop met Tekton

Voor een praktisch voorbeeld gaan we de [{itstop](https://github.com/EdwinVW/pitstop) clonen en de Readme bestand daarvan uitlezen met de [Cat](https://www.geeksforgeeks.org/cat-command-in-linux-with-examples/) commando.

Minimale Vooreisen:

```text
Minikube: lokaal draaien van Kubernetes. Als het goed is, zit dit standaard bij Docker Desktop inbegrepen
Kubectl: Command line voor Kubernetes. Zelfde als Minikube
tkn: Command line voor Tekton.
```

Tkn is op [meerdere manieren](https://tekton.dev/docs/cli/) te installeren. Zelf heb ik het met Chocolatey voor Windows gedaan, aangezien een package manager het meeste werk doet.

1.Start Kubernetes lokaal op met minikube:

```text
minikube start --kubernetes-version v1.27.4
```

Kubernetes is nu beschikbaar op de lokale machine

![Start van minikube](images/image-1.png)

2.Deploy Tekton op de Kubernetes cluster.

```text
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

Nu zijn er allerlei [Tekton componenten](https://tekton.dev/docs/concepts/overview/#what-are-the-components-of-tekton) geinstalleerd.

2.Maak een nieuwe bestand aan die de pipeline definieerd, genaamd `pipeline.yaml`. Het is handiger om dit in een aparte folder te doen dan in de root.

3.Voeg in `pipeline.yaml` het volgende toe:

- Een configuratie toe met een parameter die de pipeline run moet ontvangen.
- Een referentie naar een workspace zodat de pipelinerun de gedownloade bestande kan opslaan. Dit is vergelijkbaar met [Caching in Gitlab](https://docs.gitlab.com/ee/ci/caching/).
- De taak die de ingevoerde parameter en de workspace van hiervoor gebruikt.

```yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: clone-read
spec:
  description: | 
    This pipeline clones a git repo, then echoes the README file to the stout.
  params:
  - name: repo-url
    type: string
    description: The git repo URL to clone from.
  workspaces:
  - name: shared-data
    description: | 
      This workspace contains the cloned repo files, so they can be read by the
      next task.
  - name: git-credentials
    description: My ssh credentials
  tasks:
  - name: fetch-source
    taskRef:
      name: git-clone
    workspaces:
    - name: output
      workspace: shared-data
    - name: ssh-directory
      workspace: git-credentials
    params:
    - name: url
      value: $(params.repo-url)
```

4.Maak daarna een nieuwe `pipelinerun.yaml` aan. Deze pipelinerun instantieert de Pipeline die in de vorige stap is gedefinieerd. Gebruik de workspace die in `pipeline.yaml` gedefinieerd is. De grootte van de workspace is zelf in te stellen.

```yaml
apiVersion: tekton.dev/v1beta1
kind: PipelineRun
metadata:
  generateName: clone-read-run-
spec:
  pipelineRef:
    name: clone-read
  podTemplate:
    securityContext:
      fsGroup: 65532
  workspaces:
  - name: shared-data
    volumeClaimTemplate:
      spec:
        accessModes:
        - ReadWriteOnce
        resources:
          requests:
            storage: 1Gi
  - name: git-credentials
    secret:
      secretName: git-secret
```

5.Zet de URL van de git repository hierin als parameter binnen de spec. In dit geval is dit de Pitstop applicatie via HTTPS.

```yaml
  params:
  - name: repo-url
    value: https://github.com/EdwinVW/pitstop.git
```

5a.*Optioneel*: Maak gebruik van SSH om Pitstop te clonen. Hierbij is het gebruik van [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) nodig.

6.Definieer een tweede taak genaamd `show-readme.yaml` die de readme bestand van Pitstop leest.

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: show-readme
spec:
  description: Read and display README file.
  workspaces:
  - name: source
  steps:
  - name: read
    image: alpine:latest
    script: | 
      #!/usr/bin/env sh
      cat $(workspaces.source.path)/README.md
```

7.Klik terug op `pipeline.yaml` en voeg binnen de spec onder `tasks`de tweede taak toe.

```yaml
  tasks:
  - name: show-readme
    runAfter: ["fetch-source"]
    taskRef:
      name: show-readme
    workspaces:
    - name: source
      workspace: shared-data
```

8.Installeer met `kubectl` of `tkn` de git-clone task vanuit de [Tekton Catalog](https://hub.tekton.dev/tekton/task/git-clone).

```text
tkn hub install task git-clone
```

```text
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.6/git-clone.yaml
```

9.Navigeer naar de folder waarin de YAML bestanden staan. Pas nu alle Tekton YAML bestanden toe met `kubectl apply` en creeër daarna een pipeline run met `kubectl create`. Ik heb dit zelf allemaal één voor één gedaan.

```text
cd voorbeeld

kubectl apply -f show-readme.yaml

kubectl apply -f pipeline.yaml

kubectl create -f pipelinerun.yaml

```

Er komt een pipelinerun terug met een unieke naam.
![Alt text](images/image.png)

10.Het is nu mogelijk om de logs van de Pipeline run te bekijken. Als het goed is komt hier de Readme van Pitstop terug.

```text
tkn pipelinerun logs  clone-read-run-ttfln -f
```
![Alt text](images/readme-pipeline.png)

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

- **Leercurve**: Ontwikkelaars moeten Kubernetes goed onder de knie hebben om te kunnen beginnen aan Tekton. Het helpt als er al kennis van andere CI/CD systemen zijn, aangezien de concepten vergelijkbaar zijn.

- **Bestaande CI/CD-systemen**: Als er al bestaande CI/CD-tools en -processen zijn, kan het integreren van Tekton met deze systemen een uitdaging zijn. Dit kan aanpassingen aan bestaande workflows vereisen.

- **Resource Management**: Het beheren van resources zoals Workspaces, Secrets en ConfigMaps kan complex zijn, vooral als er een complexe applicatiestructuur is.

## Conclusie

Dit blog heeft een aantal theoretische aspecten van Tekton en een praktisch voorbeeld door een git repository te clonen en de readme van de git repository uit te lezen.

Tekton biedt een gestandaardiseerde en flexibele oplossing voor CI/CD binnen Kubernetes-omgevingen. Met zijn sterke focus op containerisatie, native integratie met Kubernetes en schaalbaarheid, staat Tekton als een toekomstgerichte keuze voor teams die werken met cloud-native applicaties.

## Bronnen

*ChatGPT.* (z.d.). OpenAI. <https://chat.openai.com/>

*Tekton.* (z.d.). Tekton. <https://tekton.dev/>

Amir, R. (2023, 21 juni). GitHub Actions vs Bitbucket Pipelines vs GitLab CI vs Tekton: The best CI/CD tool for you? Stakater. <https://www.stakater.com/post/github-actions-vs-bitbucket-pipelines-vs-gitlab-ci-vs-tekton-bestcicdtool>

Piotr.Minkowski. (2021, 5 augustus). Kubernetes CI/CD with Tekton and ArgoCD - Piotr's TechBlog. Piotr’s TechBlog. <https://piotrminkowski.com/2021/08/05/kubernetes-ci-cd-with-tekton-and-argocd/>

*Overview.* (2023, 9 maart). Tekton. <https://tekton.dev/docs/concepts/overview/>

Gravestijn, T. (z.d.). Tekton. <https://www.hcs-company.com/blog/tekton>

*Cloud Native CI/CD with Tekton and ArgoCD on AWS | Amazon Web Services.* (2022, 25 januari). Amazon Web Services. <https://aws.amazon.com/blogs/containers/cloud-native-ci-cd-with-tekton-and-argocd-on-aws/#:~:text=Tekton%20is%20an%20open%2Dsource,natively%20on%20top%20of%20Kubernetes>
