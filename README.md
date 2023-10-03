# *Tekton onderzoek*

<img src="https://tekton.dev/images/tekton-horizontal-color.png" width="200>" align="right">

Dit is het start van het onderzoek voor Tekton. Introductie op blogpost schrijven.
Bron met footnote[^1],

## Wat is Tekton?

Tekton is een open-source framework voor het bouwen, testen en implementeren van code. Het is speciaal ontworpen voor het uitvoeren van Continuous Integration (CI) en Continuous Deployment (CD) pipelines binnen een Kubernetes-omgeving. Tekton biedt een set van Kubernetes-custom resources die kunnen worden gebruikt om CI/CD-workflows te definiëren.

## Kenmerken en voordelen

Het brengt een aantal voordelen met zich mee:

Container-Native: Tekton was designed to work natively on Kubernetes, and it incorporates Kubernetes best practices by default[^1](https://platform9.com/blog/argo-cd-vs-tekton-vs-jenkins-x-finding-the-right-gitops-tooling/).

Tekton maakt gebruik van YAML-bestanden om CI/CD-pipelines te definiëren. Dit maakt het gemakkelijk te begrijpen en te beheren, en bevordert een infrastructuur-als-code aanpak. Dit is in principe hetzelfde als Gitlab en Github

- Maakt gebruik van YAML om pipelines te beschrijven
- Alleen bedoeld voor native Kubernetes
- Werkt goed met Jenkins, Skafoold en andere CI/CD Tools
- Tekton installs and runs as an extension on a Kubernetes cluster and comprises a set of Kubernetes Custom Resources that define the building blocks you can create and reuse for your pipelines.

## Voor wie bedoeld?

- Platform engineers who build CI/CD systems for the developers in their organization.
- Developers who use those CI/CD systems to do their work.

## Hoe werkt tekton?

![Tekton triggerflow](https://tekton.dev/images/TriggerFlow.svg)

Tekton draait op Kubernetes door gebruik te maken van een CRD(custom resource definitions) om CI/CD als Kubernates resource te registreren.

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
