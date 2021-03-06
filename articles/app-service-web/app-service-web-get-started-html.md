---
title: In vijf minuten uw eerste web-app implementeren in Azure | Microsoft Docs
description: Hier ontdekt u door een voorbeeld-app te implementeren hoe eenvoudig het is om web-apps in App Service uit te voeren. U kunt snel een app gaan ontwikkelen en onmiddellijk de resultaten bekijken.
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 10/13/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9c10763c0e57295c6fc75ffc22f8127e5e943844


---
# <a name="deploy-your-first-web-app-to-azure-in-five-minutes"></a>In vijf minuten uw eerste web-app implementeren in Azure
Deze zelfstudie helpt u om een eenvoudige HTML-CSS-web-app te implementeren in [Azure App Service](../app-service/app-service-value-prop-what-is.md).
Met App Service kunt u web-apps, [back-ends voor mobiele apps](/documentation/learning-paths/appservice-mobileapps/) en [API-apps](../app-service-api/app-service-api-apps-why-best-platform.md) maken.

U gaat het volgende doen: 

* Een web-app maken in Azure App Service.
* HTML en CSS erop implementeren.
* Zien hoe de pagina’s live in productie wordt uitgevoerd.
* De inhoud op dezelfde manier bijwerken als waarop u [Git-doorvoeracties pusht](https://git-scm.com/docs/git-push).

## <a name="prerequisites"></a>Vereisten
* [Git](http://www.git-scm.com/downloads).
* [Azure CLI](../xplat-cli-install.md).
* Een Microsoft Azure-account. Als u geen account hebt, kunt u zich [aanmelden voor een gratis proefversie](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> U kunt [App Service proberen](http://go.microsoft.com/fwlink/?LinkId=523751) zonder een Azure-account. U kunt een beginnerstoepassing maken en hier een uur mee spelen. U hebt geen creditcard nodig en u doet geen toezeggingen.
> 
> 

## <a name="deploy-a-simple-html-site"></a>Een eenvoudige HTML-site implementeren
1. Open een nieuw(e) Windows-opdrachtprompt, PowerShell-venster, Linux-shell of OS X-terminal. Voer `git --version` en `azure --version` uit om te controleren of Git en Azure CLI op uw computer zijn geïnstalleerd.
   
    ![De installatie van CLI-hulpprogramma's testen voor uw eerste web-app in Azure](./media/app-service-web-get-started/1-test-tools.png)
   
    Als u de hulpprogramma's niet hebt geïnstalleerd, raadpleeg dan [Vereisten](#Prerequisites) voor downloadkoppelingen.
2. Meld u als volgt aan bij Azure:
   
        azure login
   
    Volg de aanwijzingen in het Help-bericht om door te gaan met de aanmelding.
   
    ![Aanmelden bij Azure om uw eerste web-app te maken](./media/app-service-web-get-started/3-azure-login.png)
3. Zet Azure CLI in de ASM-modus en stel vervolgens de implementatiegebruiker voor App Service in. U gaat later code implementeren met behulp van de referenties.
   
        azure config mode asm
        azure site deployment user set --username <username> --pass <password>
4. Schakel naar een werkmap (`CD`) en ga als volgt te werk om de voorbeeld-app te kopiëren:
   
        git clone https://github.com/Azure-Samples/app-service-web-html-get-started.git
5. Schakel naar de opslagplaats van uw voorbeeld-app. 
   
        cd app-service-web-html-get-started
6. Maak de App Service-toepassingsbron in Azure met een unieke naam en de implementatiegebruiker die u eerder hebt geconfigureerd. Geef het nummer van de gewenste regio op wanneer dit wordt gevraagd.
   
        azure site create <app_name> --git --gitusername <username>
   
    ![De Azure-resource maken voor uw eerste web-app in Azure](./media/app-service-web-get-started/4-create-site.png)
   
    Uw app is nu gemaakt in Azure. Bovendien is de huidige map voor Git geïnitialiseerd en als Git remote verbonden met de nieuwe App Service-app.
    U kunt naar de URL van de app bladeren (http://&lt;app_naam>.azurewebsites.net), om de mooi vormgegeven standaard-HTML-pagina te bekijken, maar in deze zelfstudie gaan we uw code hier neerzetten.
7. Implementeer de voorbeeldcode in de Azure-app op dezelfde manier als waarop u code zou pushen met Git. Gebruik, wanneer u daarnaar wordt gevraagd, het wachtwoord dat u eerder hebt geconfigureerd.
   
        git push azure master
   
    ![Code pushen naar uw eerste web-app in Azure](./media/app-service-web-get-started/5-push-code.png)
   
    Als u een van de taalframeworks hebt gebruikt, ziet u andere uitvoer. Dat komt omdat `git push` niet alleen code in Azure plaatst, maar ook implementatietaken in de implementatie-engine activeert. Als u package.json- (Node.js) of requirements.txt-bestanden (Python) in de hoofdmap van het project (opslagplaats) hebt, of als er een packages.config-bestand in uw ASP.NET-project staat, worden de vereiste pakketten met de implementatiescripts voor u hersteld. U kunt ook [de Composer-extensie inschakelen](web-sites-php-mysql-deploy-use-git.md#composer) als u composer.json-bestanden in uw PHP-app automatisch wilt verwerken.

Gefeliciteerd, u hebt uw app geïmplementeerd in Azure App Service.

## <a name="see-your-app-running-live"></a>Uw app live in werking zien
Als u uw app in Azure in werking wilt zien, voert u deze opdracht uit vanuit een willekeurige map in de opslagplaats:

    azure site browse

## <a name="make-updates-to-your-app"></a>Updates aanbrengen in uw app
Nu kunt u met Git op elk moment push-acties uitvoeren vanuit het project (opslagplaats) om een actieve site bij te werken. Dit werkt op dezelfde manier als toen u de code voor het eerst implementeerde. Zo hoeft u telkens wanneer u een nieuwe wijziging wilt pushen die u lokaal hebt getest, alleen de volgende opdrachten uit te voeren vanuit de hoofdmap van het project (opslagplaats):

    git add .
    git commit -m "<your_message>"
    git push azure master

## <a name="next-steps"></a>Volgende stappen
Zoek als volgt de meest geschikte ontwikkel- en implementatiestappen voor uw taalframework:

> [!div class="op_single_selector"]
> * [.NET](web-sites-dotnet-get-started.md)
> * [PHP](app-service-web-php-get-started.md)
> * [Node.js](app-service-web-nodejs-get-started.md)
> * [Python](web-sites-python-ptvs-django-mysql.md)
> * [Java](web-sites-java-get-started.md)
> 
> 

Of doe meer met uw eerste web-app. Bijvoorbeeld:

* Probeer [andere manieren om uw code in Azure te implementeren](web-sites-deploy.md). Als u bijvoorbeeld wilt implementeren vanuit een van uw GitHub-opslagplaatsen, selecteert u in **Implementatieopties** **GitHub** in plaats van **Lokale Git-opslagplaats**.
* Breng uw Azure-app naar een hoger niveau. Verifieer uw gebruikers. Schaal de app op basis van vraag. Stel prestatiewaarschuwingen in. Dit alles met slechts enkele klikken. Zie [Functionaliteit toevoegen aan uw eerste web-app](app-service-web-get-started-2.md).




<!--HONumber=Nov16_HO2-->


