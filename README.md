# *Tekton onderzoek*

<img src="https://tekton.dev/images/tekton-horizontal-color.png" width="200>" align="right">

INTRODUCTIE

Dit is het start van het onderzoek voor Tekton. Introductie op blogpost schrijven.
Bron met footnote[^1],'

## Gebruik van Tekton: Clonen repository
Minimale Vooreisen:
```
Minikube: lokaal draaien van Kubernetes
Kubectl: Command line voor Kubernetes
tkn: Command line voor Tekton
```

[community hub](https://hub.tekton.dev/)
Stappen:
```
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

## Wat is Tekton?
Tekton is een open-source framework dat wordt gebruikt voor het bouwen, testen en implementeren van cloud-native applicaties. Het is specifiek ontworpen om CI/CD (Continuous Integration/Continuous Deployment) pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Hierdoor kunnen ontwikkelaars via cloud providers zoals Kubernetes hun applicatie bouwen, testen en deployen [(Tekton, z.d)](https://tekton.dev/).

Er zijn aardig wat overeenkomsten tussen andere CI/CD Tools. Het grootste verschil is dat Tekton vooral de nadruk op containerisatie zet. Daarom is Tekton op Kubernetes gebouwd. Tekton maakt en voert alle instanties van taken en pipelines uit op verschillende containers. Deze pipelines zijn van tevoren te configureren met YAML bestanden. Bij Gitlab CI/CD is er een voorgeconfigureerde Gitlab runner nodig om pipelines te kunnen draaien.  

## Hoe werkt tekton?

Tekton werkt door gebruik te maken van een reeks van Kubernetes-resources die samenwerken om CI/CD-pipelines te definiëren en uit te voeren. Hierdoor is het makkelijker om Tekton met een bestaande Kubernetes infrastructuur te integreren[^bron](https://www.stakater.com/post/github-actions-vs-bitbucket-pipelines-vs-gitlab-ci-vs-tekton-bestcicdtool) De belangrijkste kenmerken hierbij zijn:
![https://tekton.dev/docs/concepts/concept-model/](https://tekton.dev/docs/concepts/concept-tasks-pipelines.png)
**Figuur 2**

**Resources:** Tekton maakt gebruik van resources om input- en outputgegevens van een pipeline te beheren. Dit kunnen bijvoorbeeld broncode-repositories, container-images, configuratiebestanden of andere artefacten zijn.

**Steps**: Een step is een uitvoering van een commando in CI/CD, zoals het uitvoeren van unit tests, of het implementeren van een applicatie. Tekton voert elke stap uit met een Docker container image[Tekton overview]().

**Tasks:** Een taak bestaat uit een collectie van **steps**. Het is een geautomatiseerde stap die specifieke acties uitvoert, zoals het bouwen van een containerimage, het uitvoeren van tests of. In Gitlab is dit vergelijkbaar met de script annotatie.

**Pipeline:** Een pipeline in Tekton bestaat uit een collectie **tasks** die Tekton op een bepaalde volgorde uitvoert. Hiermee kunnen ontwikkelaars complexe workflows definiëren die bestaan uit meerdere taken.

![Voorbeelden toepassing pipeline](https://tekton.dev/docs/concepts/concept-runs.png)**Figuur 3**


**TaskRuns**: Toont alle instanties met de type **tasks**

**PipelineRuns**: Toont alle instanties met de type **pipeline**.

**Trigger:** Tekton kan worden geïntegreerd met triggersystemen, waardoor pipelines kunnen worden geactiveerd op basis van gebeurtenissen zoals codecommits, pull-aanvragen of andere gebeurtenissen in een versiebeheersysteem.

**Workspaces:** Workspaces bieden een manier om gegevens tussen taken in een pipeline te delen. Ze stellen taken in staat om gegevens te schrijven naar en te lezen uit een gedeelde opslagplaats.

Over het algemeen biedt Tekton een gestandaardiseerde manier om CI/CD-pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Het is ontworpen om eenvoudig te integreren met andere tools en systemen, waardoor het een populaire keuze is geworden voor teams die Kubernetes gebruiken voor containerorkestratie.

## Kenmerken en voordelen
### v1
Het brengt een aantal voordelen met zich mee:

Container-Native: Tekton was designed to work natively on Kubernetes, and it incorporates Kubernetes best practices by default[^1](https://platform9.com/blog/argo-cd-vs-tekton-vs-jenkins-x-finding-the-right-gitops-tooling/).

Tekton maakt gebruik van YAML-bestanden om CI/CD-pipelines te definiëren. Dit maakt het gemakkelijk te begrijpen en te beheren, en bevordert een infrastructuur-als-code aanpak. Dit is in principe hetzelfde als Gitlab en Github

- Maakt gebruik van YAML om pipelines te beschrijven
- Werkt goed met Jenkins, Skafoold en andere CI/CD Tools

Ook brengt Tekton een aantal voordelen voor developers die CI/CD systemen bouwen:

- Platformonafhankelijkheid: Tekton draait op Kubernetes als een set van [Kubernetes Custom Resource](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/), waardoor Tekton niet afhankelijk is van een specifieke platform en los draait van andere Kubernetes resources.
- Native Kubernetes-integratie: Tekton is ontworpen om naadloos samen te werken met Kubernetes. Het maakt gebruik van Kubernetes-podspecs om taken uit te voeren, waardoor het eenvoudig kan worden geïntegreerd in bestaande Kubernetes-omgevingen.
- Declaratieve configuratie: Tekton maakt gebruik van YAML-bestanden voor het definiëren van pipelines, taken en resources. Dit zorgt voor een eenvoudige en begrijpelijke manier om CI/CD-pijplijnen te definiëren en te onderhouden. Het is hierdoor makkelijker om deze pipelines in verschillende projecten neer te zetten.
- Schaalbaarheid: Door gebruik te maken van Kubernetes voor het orkestreren van taken, profiteert Tekton van de schaalbaarheid en betrouwbaarheid van het Kubernetes-platform. Hierdoor kunnen teams pipelines uitvoeren op een schaal die geschikt is voor hun project, bijvoorbeeld meerdere microservices met een of meerdere pipelines.


### v2 met bron

- Customizable. Tekton entities are fully customizable, allowing for a high degree of flexibility. Platform engineers can define a highly detailed catalog of building blocks for developers to use in a wide variety of scenarios.
- Reusable. Tekton entities are fully portable, so once defined, anyone within the organization can use a given pipeline and reuse its building blocks. This allows developers to quickly build complex pipelines without “reinventing the wheel.”
- Expandable. Tekton Catalog is a community-driven repository of Tekton building blocks. You can quickly create new and expand existing pipelines using pre-made components from the Tekton Catalog.
- Standardized. Tekton installs and runs as an extension on your Kubernetes cluster and uses the well-established Kubernetes resource model. Tekton workloads execute inside Kubernetes containers.
- Scalable. To increase your workload capacity, you can simply add nodes to your cluster. Tekton scales with your cluster without the need to redefine your resource allocations or any other modifications to your pipelines.
[Bron](https://tekton.dev/docs/concepts/overview/)



## Gebruik met Git
### Workshop git

Webhooks en Events:

Tekton kan worden geconfigureerd om te reageren op Git-events via webhooks. Wanneer er een push, pull request of een andere relevante gebeurtenis plaatsvindt in het Git-repository, kan Tekton automatisch een pipeline-start triggeren.
Git Resource en Task in Tekton:

Tekton bevat een concept genaamd "Resource". Een Git Resource stelt een pipeline in staat om te communiceren met een Git-repository. Je kunt een Git Resource gebruiken om toegang te krijgen tot je Git-repository en wijzigingen te detecteren.

Daarna kun je een "Task" definiëren om specifieke acties uit te voeren, zoals het klonen van de repository, het uitvoeren van tests, het bouwen van je applicatie, enzovoort.

Gebruik van Git ServiceAccounts:

ServiceAccounts zijn Kubernetes-objecten die worden gebruikt om identiteiten aan pods en containers toe te wijzen. Door ServiceAccounts te gebruiken, kun je de juiste rechten instellen om toegang te krijgen tot je Git-repository binnen je pipelines.
Gebruik van Git Secrets:

Je moet mogelijk Git-credentials, zoals gebruikersnaam en wachtwoord of een token, veilig opslaan en gebruiken binnen je pipeline. Dit kan worden bereikt met behulp van Kubernetes Secrets.
Gebruik van Git-trigger in Tekton Triggers:

Tekton Triggers is een aanvullende component van Tekton die het mogelijk maakt om pipelines te starten op basis van verschillende triggers, waaronder Git-webhooks. Met Tekton Triggers kun je complexe trigger-logica instellen die reageert op specifieke Git-events.
Git-integratie in CI/CD-stappen:

Binnen de stappen van je pipeline kun je Git-commando's gebruiken om handelingen uit te voeren, zoals het ophalen van de nieuwste code, het maken van nieuwe branches, het uitvoeren van commits, enzovoort.
Gebruik van GitOps en Tekton:

GitOps is een methode voor het beheren van infrastructuur en applicaties, waarbij de hele configuratie van de applicatie in een Git-repository wordt opgeslagen. Tekton kan worden gebruikt om CI/CD te automatiseren voor GitOps-workflows.
Gebruik van Git-repositories voor pipeline-configuratie:

Je kunt Git-repositories gebruiken om je pipeline-configuratie op te slaan. Dit maakt het gemakkelijk om wijzigingen in je pipelines te beheren en te versioneren.

## Best practices


Het schrijven van effectieve Tekton-pijplijnen en taken vereist een goed begrip van de Tekton-resources en best practices voor het ontwerpen van CI/CD-pijplijnen. Hier zijn enkele richtlijnen die je kunt volgen:

1. Modulariteit en Herbruikbaarheid:

    - Splits je pijplijn op in afzonderlijke taken (tasks) en gebruik deze als bouwstenen om complexere pijplijnen te maken. Herbruikbare taken kunnen in verschillende pijplijnen worden gebruikt.

2. Gebruik Parameters:

    - Maak gebruik van parameters om je taken en pijplijnen flexibeler te maken. Hierdoor kun je waarden dynamisch doorgeven aan taken.

3. Logging en Output:

    - Maak gebruik van Tekton's loggingfaciliteiten om relevante informatie vast te leggen. Gebruik tekton.dev/taskRun.logging om de logging-instellingen voor een taakrun te configureren.

4. Gebruik Resources Wijselijk:

    - Maak gebruik van Tekton-resources zoals PipelineResources om artefacten (zoals broncode, afbeeldingen, enz.) tussen taken te delen.

5. Parametriseer je Pijplijnstappen:

    - Maak gebruik van resource-parameters om flexibel om te gaan met de input van je taken. Dit maakt je pijplijn dynamischer en minder rigide.

6. Maak Effectief Gebruik van Conditionals:

    - Gebruik condities om bepaalde stappen alleen uit te voeren onder specifieke voorwaarden. Dit helpt om je pijplijnen aan te passen aan verschillende scenario's.
Handel Fouten en Uitzonderingen Af:

Implementeer adequate foutafhandeling in je taken en pijplijnen. Gebruik bijvoorbeeld try-catch-blokken om specifieke fouten te identificeren en erop te reageren.
Veiligheid en Beveiliging:

Zorg ervoor dat je de juiste beveiligingsmaatregelen implementeert, zoals het beperken van toegang tot gevoelige gegevens, het gebruik van beveiligde omgevingsvariabelen, enz.
Test je Pijplijn:

Test je taken en pijplijnen grondig om ervoor te zorgen dat ze de verwachte resultaten opleveren. Maak eventueel gebruik van een staging-omgeving om dit te doen.
Documentatie:

Zorg voor uitgebreide documentatie van je taken en pijplijnen. Dit maakt het voor andere teamleden gemakkelijker om ze te begrijpen en te gebruiken.
Versiebeheer:

Houd je Tekton-pijplijnen en taken onder versiebeheer, bijvoorbeeld met behulp van een tool als Git. Dit vergemakkelijkt het bijhouden van wijzigingen en het samenwerken met anderen.
Optimaliseer voor Snelheid en Efficiëntie:

Voer regelmatig prestatie-analyses uit om te controleren of je pijplijnen en taken efficiënt werken. Identificeer mogelijke knelpunten en optimaliseer waar nodig.
Door deze best practices te volgen, kun je effectieve en goed onderhouden Tekton-pijplijnen en taken ontwikkelen. Het helpt ook bij het creëren van een gestructureerde en goed gedocumenteerde CI/CD-infrastructuur.

## Mogelijkheden

Tekton is een open-source framework voor het bouwen, uitvoeren en onderhouden van CI/CD-pipelines. Het is speciaal ontworpen voor gebruik in cloudomgevingen en biedt verschillende mogelijkheden voor het implementeren van continue integratie (CI) en continue implementatie (CD). Hier zijn enkele van de belangrijkste mogelijkheden van Tekton:

Container-Native Workloads: Tekton is ontworpen om te werken met containers, waardoor het goed past in cloudomgevingen die vaak draaien op containerorkestratieplatforms zoals Kubernetes.

Declaratieve Pipelines: Tekton maakt gebruik van YAML-bestanden om pipelines te definiëren, waardoor je je CI/CD-processen kunt declareren en versiebeheer kunt toepassen.

Herbruikbare Taken: In Tekton kunnen taken worden gedefinieerd die herbruikbaar zijn in verschillende pipelines. Dit bevordert modulaire en gestandaardiseerde CI/CD-processen.

Ingebouwde Kubernetes Resources: Tekton maakt gebruik van native Kubernetes-resources zoals Pods en Deployments om taken en pipelines uit te voeren, waardoor het naadloos integreert met Kubernetes-clusters.

Event-Driven Pipelines: Tekton ondersteunt het triggeren van pipelines op basis van gebeurtenissen, bijvoorbeeld wanneer er code wordt gepusht naar een repository.

Parallellisme en Serialisatie: Tekton maakt het mogelijk om taken parallel uit te voeren, waardoor je de snelheid van je CI/CD-processen kunt optimaliseren. Tegelijkertijd kunnen taken ook in een bepaalde volgorde worden uitgevoerd.

Geïntegreerde Credentials en Secrets: Tekton biedt ingebouwde ondersteuning voor het beheren van credentials en secrets, waardoor het veilig werken met gevoelige gegevens mogelijk is.

Uitgebreide Gemeenschap en Ecosysteem: Tekton heeft een actieve gemeenschap en wordt ondersteund door verschillende CI/CD-tools en cloudproviders, waaronder Google Cloud, IBM Cloud en Red Hat OpenShift.

Ondersteuning voor Multi-Cloud: Tekton kan worden gebruikt in verschillende cloudomgevingen en is niet beperkt tot een specifiek cloudplatform.

Schaalbaarheid en Betrouwbaarheid: Tekton is ontworpen voor schaalbaarheid en betrouwbaarheid, wat belangrijk is voor bedrijven die in productieomgevingen werken.

## Werking
![](https://res.cloudinary.com/practicaldev/image/fetch/s--ro7n7uV8--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://jaruiz.io/images/tekton-argocd/pipeline-stages.png)

## Uitdagingen

Het implementeren van Tekton CI/CD in een ontwikkelteam kan veel voordelen bieden, zoals gestroomlijnde en geautomatiseerde deployment pipelines. Echter, net zoals bij elke nieuwe technologie, zijn er ook uitdagingen die kunnen optreden. Hier zijn enkele mogelijke uitdagingen en oplossingen bij het implementeren van Tekton CI/CD:

Uitdagingen:

Leercurve: Ontwikkelaars kunnen tijd nodig hebben om vertrouwd te raken met de Tekton-pijplijnconfiguratie en -implementatie.

Bestaande CI/CD-systemen: Als er al een bestaande CI/CD-infrastructuur is, kan de migratie naar Tekton uitdagend zijn.

Complexiteit van de pipeline-definities: Het kan complex lijken om Tekton-pijplijnen goed te configureren, vooral voor complexe applicaties met veel afhankelijkheden.

Integratie met andere tools: Tekton moet mogelijk worden geïntegreerd met bestaande tools voor versiebeheer, containerregistratie, enzovoort.

Schaalbaarheid: Als de pipeline te complex wordt, kan het moeilijk zijn om deze schaalbaar en beheersbaar te houden.

Oplossingen:

Opleiding en documentatie: Zorg voor voldoende training en documentatie voor het ontwikkelteam, inclusief voorbeelden en best practices.

Geleidelijke implementatie: Begin met het migreren van kleinere, minder kritieke projecten naar Tekton om vertrouwen op te bouwen en ervaring op te doen.

Herbruikbare componenten: Definieer gemeenschappelijke stappen en resources die kunnen worden hergebruikt over verschillende pijplijnen.

Integratie met bestaande tools: Zorg voor goed gedocumenteerde integraties met bestaande tools en platforms die het team gebruikt.

Pipeline-optimalisatie: Houd pijplijnen eenvoudig en vermijd overmatige complexiteit. Gebruik Tekton Resources (Tasks, Pipelines, etc.) om de structuur van pijplijnen georganiseerd te houden.

Monitoring en logging: Implementeer effectieve monitoring en logging om snel problemen te identificeren en op te lossen.

Automatische tests: Voeg geautomatiseerde tests toe aan de pipeline om ervoor te zorgen dat de implementatie betrouwbaar is.

Community en ondersteuning: Maak gebruik van de Tekton-gemeenschap en andere bronnen voor ondersteuning en advies.

Het is belangrijk om te onthouden dat de overgang naar Tekton tijd kan kosten, maar met de juiste aanpak en investering in opleiding en documentatie kan het team de voordelen van geautomatiseerde CI/CD-pipelines maximaliseren.

## Bronnen

[https://www.hcs-company.com/blog/tekton]
Gitops en Tekton
[https://production-gitops.dev/guides/cp4i/apic/cluster-config/gitops-tekton-argocd/]

AWS Tekton
[https://aws.amazon.com/blogs/containers/cloud-native-ci-cd-with-tekton-and-argocd-on-aws/#:~:text=Tekton%20is%20an%20open%2Dsource,natively%20on%20top%20of%20Kubernetes.]

[https://cloud.google.com/tekton]
