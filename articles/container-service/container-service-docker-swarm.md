---
title: Containerbeheer in Azure Container Service met Docker Swarm | Microsoft Docs
description: Containers implementeren in een Docker Swarm in Azure Container Service
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: Docker, Containers, Micro-services, Mesos, Azure
ms.assetid: af8f6fb2-13dc-429c-b82a-24a741168d42
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 99953be1a9d99cc1fdd5cd46522ccd262c09e25b


---
# <a name="container-management-with-docker-swarm"></a>Containerbeheer met Docker Swarm
Docker Swarm biedt een omgeving voor het implementeren van beperkte workloads in een gegroepeerde set Docker-hosts. Docker Swarm maakt gebruik van de systeemeigen Docker API. De werkstroom voor het beheer van containers in een Docker Swarm is bijna identiek aan de werkstroom op een enkele containerhost. Dit document bevat enkele eenvoudige voorbeelden van de implementatie van beperkte workloads in een Azure Container Service-exemplaar van Docker Swarm. Zie [Docker Swarm op Docker.com](https://docs.docker.com/swarm/) voor uitgebreidere documentatie bij Docker Swarm.

Vereisten voor de oefeningen in dit document:

[Een Swarm-cluster in Azure Container Service maken](container-service-deployment.md)

[Verbinding maken met het Swarm-cluster in Azure Container Service](container-service-connect.md)

## <a name="deploy-a-new-container"></a>Een nieuwe container implementeren
Als u in de Docker Swarm een nieuwe container wilt maken, gebruikt u de opdracht `docker run` (waarbij u ervoor zorgt dat u een SSH-tunnel naar de masters hebt geopend overeenkomstig de bovenstaande vereisten). In dit voorbeeld wordt een container van gemaakt op basis van de `yeasy/simple-web`-installatiekopie:

```bash
user@ubuntu:~$ docker run -d -p 80:80 yeasy/simple-web

4298d397b9ab6f37e2d1978ef3c8c1537c938e98a8bf096ff00def2eab04bf72
```

Nadat de container is gemaakt, gebruikt u `docker ps` om informatie over de container te retourneren. U ziet hier dat de Swarm-agent wordt vermeld die als host fungeert voor de container:

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   31 seconds ago      Up 9 seconds        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

U hebt nu toegang tot de toepassing die wordt uitgevoerd in deze container via de openbare DNS-naam van de load balancer van de Swarm-agent. U kunt deze informatie vinden in Azure Portal:  

![Echte bezoekresultaten](media/real-visit.jpg)  

De Load Balancer heeft standaard de poorten 80, 8080 en 443 open. Als u via een andere poort verbinding wilt maken, moet u die poort openen in de Azure Load Balancer voor de Agent-pool.

## <a name="deploy-multiple-containers"></a>Meerdere containers implementeren
Als meerdere containers worden gestart, door 'docker run' meermaals uit te voeren, kunt u de opdracht `docker ps` gebruiken om te zien op welke hosts de containers worden uitgevoerd. In het voorbeeld hieronder zijn drie containers gelijkmatig verdeeld over de drie Swarm-agents:  

```bash
user@ubuntu:~$ docker ps

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
11be062ff602        yeasy/simple-web    "/bin/sh -c 'python i"   11 seconds ago      Up 10 seconds       10.0.0.6:83->80/tcp   swarm-agent-34A73819-2/clever_banach
1ff421554c50        yeasy/simple-web    "/bin/sh -c 'python i"   49 seconds ago      Up 48 seconds       10.0.0.4:82->80/tcp   swarm-agent-34A73819-0/stupefied_ride
4298d397b9ab        yeasy/simple-web    "/bin/sh -c 'python i"   2 minutes ago       Up 2 minutes        10.0.0.5:80->80/tcp   swarm-agent-34A73819-1/happy_allen
```  

## <a name="deploy-containers-by-using-docker-compose"></a>Containers implementeren met behulp van Docker Compose
U kunt Docker Compose gebruiken om de implementatie en configuratie van meerdere containers te automatiseren. Hiervoor moet een SSH-tunnel (Secure Shell-tunnel) zijn gemaakt en moet de variabele DOCKER_HOST zijn ingesteld (zie de bovenstaande vereisten).

Maak een docker-compose.yml-bestand op het lokale systeem. Gebruik dit [voorbeeld](https://raw.githubusercontent.com/rgardler/AzureDevTestDeploy/master/docker-compose.yml) om dit te doen.

```bash
web:
  image: adtd/web:0.1
  ports:
    - "80:80"
  links:
    - rest:rest-demo-azure.marathon.mesos
rest:
  image: adtd/rest:0.1
  ports:
    - "8080:8080"

```

Voer `docker-compose up -d` uit om de containerimplementaties te starten:

```bash
user@ubuntu:~/compose$ docker-compose up -d
Pulling rest (adtd/rest:0.1)...
swarm-agent-3B7093B8-0: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/rest:0.1... : downloaded
swarm-agent-3B7093B8-3: Pulling adtd/rest:0.1... : downloaded
Creating compose_rest_1
Pulling web (adtd/web:0.1)...
swarm-agent-3B7093B8-3: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-0: Pulling adtd/web:0.1... : downloaded
swarm-agent-3B7093B8-2: Pulling adtd/web:0.1... : downloaded
Creating compose_web_1
```

Tot slot wordt de lijst met actieve containers geretourneerd. Deze lijst bevat de containers die zijn geïmplementeerd met behulp van Docker Compose:

```bash
user@ubuntu:~/compose$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                     NAMES
caf185d221b7        adtd/web:0.1        "apache2-foreground"   2 minutes ago       Up About a minute   10.0.0.4:80->80/tcp       swarm-agent-3B7093B8-0/compose_web_1
040efc0ea937        adtd/rest:0.1       "catalina.sh run"      3 minutes ago       Up 2 minutes        10.0.0.4:8080->8080/tcp   swarm-agent-3B7093B8-0/compose_rest_1
```

Uiteraard kunt u `docker-compose ps` gebruiken om alleen de containers te onderzoeken die in uw bestand `compose.yml` zijn gedefinieerd.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Docker Swarm](https://docs.docker.com/swarm/)




<!--HONumber=Nov16_HO2-->


