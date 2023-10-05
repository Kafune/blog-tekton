# *Tekton onderzoek*

<img src="https://tekton.dev/images/tekton-horizontal-color.png" width="200>" align="right">

Dit is het start van het onderzoek voor Tekton. Introductie op blogpost schrijven.
Bron met footnote[^1],'

## Gebruik van Tekton: Clonen repository
Pre reqs:
```
Minikube: lokaal draaien van Kubernetes
Tekton CLI: tkn
```


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
Tekton is een open-source framework dat wordt gebruikt voor het bouwen, testen en implementeren van cloud-native applicaties. Het is specifiek ontworpen om CI/CD (Continuous Integration/Continuous Deployment) pipelines te definiëren en uit te voeren in een Kubernetes-omgeving. Tekton maakt het mogelijk om de verschillende stappen van een ontwikkelingsproces te automatiseren en te orchestreren, waardoor teams efficiënter kunnen werken bij het ontwikkelen en implementeren van software in Kubernetes-clusters.



Tekton CI/CD is een open-source project dat een raamwerk biedt voor het creëren van continuous integration en continuous delivery (CI/CD) systemen. Het is ontworpen om cloud-native te zijn en werkt goed samen met gecontaineriseerde applicaties en Kubernetes.



## Waar bestaat Tekton uit?

It consists of Tekton Pipelines, which provides the building blocks, and of supporting components, such as Tekton CLI and Tekton Catalog, that make Tekton a complete ecosystem. Tekton is part of the CD Foundation, a Linux Foundation project.[https://tekton.dev/docs/concepts/overview/]

## Kenmerken en voordelen

Het brengt een aantal voordelen met zich mee:

Container-Native: Tekton was designed to work natively on Kubernetes, and it incorporates Kubernetes best practices by default[^1](https://platform9.com/blog/argo-cd-vs-tekton-vs-jenkins-x-finding-the-right-gitops-tooling/).

Tekton maakt gebruik van YAML-bestanden om CI/CD-pipelines te definiëren. Dit maakt het gemakkelijk te begrijpen en te beheren, en bevordert een infrastructuur-als-code aanpak. Dit is in principe hetzelfde als Gitlab en Github

- Maakt gebruik van YAML om pipelines te beschrijven
- Alleen bedoeld voor native Kubernetes
- Werkt goed met Jenkins, Skafoold en andere CI/CD Tools
- Tekton installs and runs as an extension on a Kubernetes cluster and comprises a set of Kubernetes Custom Resources that define the building blocks you can create and reuse for your pipelines.

Ook brengt Tekton een aantal voordelen voor developers die CI/CD systemen bouwen:

- Platformonafhankelijkheid: Tekton kan worden uitgevoerd op verschillende cloudplatforms, zoals Kubernetes, en is ook geschikt voor on-premises implementaties. Dit betekent dat je de vrijheid hebt om het te draaien waar je infrastructuur zich bevindt.
- Native Kubernetes-integratie: Tekton is ontworpen om naadloos samen te werken met Kubernetes. Het maakt gebruik van Kubernetes-podspecs om taken uit te voeren, waardoor het eenvoudig kan worden geïntegreerd in bestaande Kubernetes-omgevingen.
- Declaratieve configuratie: Tekton maakt gebruik van YAML-bestanden voor het definiëren van pipelines, taken en resources. Dit zorgt voor een eenvoudige en begrijpelijke manier om CI/CD-pijplijnen te definiëren en te onderhouden.
- Schaalbaarheid: Door gebruik te maken van Kubernetes voor het orkestreren van taken, profiteert Tekton van de schaalbaarheid en betrouwbaarheid van het Kubernetes-platform. Hierdoor kunnen teams pipelines uitvoeren op een schaal die geschikt is voor hun project.

- Customizable. Tekton entities are fully customizable, allowing for a high degree of flexibility. Platform engineers can define a highly detailed catalog of building blocks for developers to use in a wide variety of scenarios.
- Reusable. Tekton entities are fully portable, so once defined, anyone within the organization can use a given pipeline and reuse its building blocks. This allows developers to quickly build complex pipelines without “reinventing the wheel.”
- Expandable. Tekton Catalog is a community-driven repository of Tekton building blocks. You can quickly create new and expand existing pipelines using pre-made components from the Tekton Catalog.
- Standardized. Tekton installs and runs as an extension on your Kubernetes cluster and uses the well-established Kubernetes resource model. Tekton workloads execute inside Kubernetes containers.
- Scalable. To increase your workload capacity, you can simply add nodes to your cluster. Tekton scales with your cluster without the need to redefine your resource allocations or any other modifications to your pipelines.

## Hoe werkt tekton?

![Tekton triggerflow](https://tekton.dev/images/TriggerFlow.svg)

```
Kortere versie:
Tekton werkt binnen Kubernetes en maakt gebruik van taken en pipelines om CI/CD-processen te automatiseren.

Task: Een afzonderlijke eenheid van werk.
Pipeline: Een reeks van taken die in een bepaalde volgorde worden uitgevoerd.
TaskRun: Een instantie van een Task die wordt uitgevoerd.
PipelineResource: Biedt toegang tot externe bronnen.
Workspace: Gedeelde ruimte voor gegevensuitwisseling tussen stappen.
Trigger en EventListener: Starten pipelines op basis van gebeurtenissen.
Dit maakt het mogelijk om flexibele CI/CD-pipelines te creëren en uit te voeren in Kubernetes-omgevingen.
```

Tekton werkt door gebruik te maken van een reeks van Kubernetes-resources die samenwerken om CI/CD-pipelines te definiëren en uit te voeren. Hier zijn de belangrijkste concepten en stappen:

## steps, tasks, pipelines

Task (Taak):

Een Task definieert een enkele eenheid van werk, zoals het bouwen van een container, uitvoeren van tests, of pushen van een image naar een container registry. Een Task is een op zichzelf staande entiteit die herbruikbaar is.
Pipeline (Pipeline):

Een Pipeline is een reeks van taken die in een bepaalde volgorde worden uitgevoerd. Het definieert de workflow van het ontwikkelproces. Een Pipeline bestaat uit verschillende stappen, elk bestaande uit een specifieke taak.
TaskRun (TaakRun):

Een TaskRun is een instantie van een Task die wordt uitgevoerd. Het bevat informatie zoals de specifieke parameters en de uitvoeringsstatus van de Task.
PipelineResource (PipelineBron):

Een PipelineResource biedt toegang tot externe bronnen zoals Git-repositories, container images en andere artefacten die tijdens de pipeline worden gebruikt.
Workspace (Werkruimte):

Een Workspace is een gedeelde ruimte die wordt gebruikt om gegevens tussen verschillende stappen in een pipeline door te geven.
Trigger (Trigger):

Een Trigger is een gebeurtenis die de start van een pipeline triggert. Dit kan bijvoorbeeld een code push naar een Git-repository zijn.
EventListener (EvenementLuisteraar):

Een EventListener luistert naar triggers en start de bijbehorende pipeline op basis van het getriggerde evenement.
![https://tekton.dev/docs/concepts/concept-model/](https://tekton.dev/docs/concepts/concept-tasks-pipelines.png)

Samen zorgen deze concepten ervoor dat ontwikkelaars CI/CD-pipelines kunnen definiëren en uitvoeren binnen een Kubernetes-omgeving. Hier is een algemene workflow:

Definieer taken: Ontwikkelaars definiëren herbruikbare taken die specifieke stappen van het CI/CD-proces uitvoeren, zoals bouwen, testen, implementeren, enz.

Bouw de pipeline: Gebruikmakend van deze taken, definieer je een pipeline die de stappen en de volgorde van uitvoering bepaalt.

Configureren van resources: Stel de benodigde bronnen zoals Git-repositories en container images in met behulp van PipelineResources.

Start de pipeline: Gebruik triggers of start handmatig een pipeline om het CI/CD-proces te starten.

Monitor en beheer de uitvoering: Volg de status van de taken en pipelines, bekijk de uitvoeringslogboeken en grijp in indien nodig.

Dit is een vereenvoudigde uitleg, maar het geeft een idee van hoe Tekton werkt. Het biedt een flexibele en schaalbare manier om CI/CD-processen te automatiseren in Kubernetes-omgevingen.
## Gebruik met Git

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

## Taskruns en piperuns
![Alt text](https://tekton.dev/docs/concepts/concept-runs.png)

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
