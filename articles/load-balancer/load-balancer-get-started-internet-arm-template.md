---
title: Een internetgerichte load balancer maken in Resource Manager met behulp van een sjabloon | Microsoft Docs
description: Meer informatie over hoe u met een sjabloon een internetgerichte load balancer maakt in Resource Manager
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: b24f4729-4559-4458-8527-71009d242647
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 540706ec32e3a0fbdfc29edd7e3e7b1784ecc720

---

# <a name="creating-an-internet-facing-load-balancer-using-a-template"></a>Aan de slag met het maken van een internetgerichte load balancer met een sjabloon

[!INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. Hier vindt u [meer informatie over hoe u een internetgerichte load balancer maakt met het klassieke implementatiemodel](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploy-the-template-by-using-click-to-deploy"></a>De sjabloon implementeren met Klik om te implementeren

De voorbeeldsjabloon in de openbare opslagplaats maakt gebruik van een parameterbestand dat de standaardwaarden bevat voor het genereren van het hierboven beschreven scenario. Als u deze sjabloon wilt implementeren met behulp van Klik om te implementeren, volgt u [deze koppeling](http://go.microsoft.com/fwlink/?LinkId=544801). Klik op **Distribueren naar Azure**, vervang indien nodig de standaardparameterwaarden en volg de instructies in de portal.

## <a name="deploy-the-template-by-using-powershell"></a>De sjabloon implementeren met PowerShell

Volg onderstaande stappen als u de sjabloon die u hebt gedownload, wilt implementeren met PowerShell.

1. Als u Azure PowerShell nog niet eerder hebt gebruikt, kunt u [Azure PowerShell installeren en configureren](../powershell-install-configure.md) raadplegen en de instructies helemaal tot aan het einde volgen om u aan te melden bij Azure en uw abonnement te selecteren.
2. Voer de cmdlet **New-AzureRmResourceGroupDeployment** uit als u een resourcegroep wilt maken met de sjabloon.

    ```powershell
        New-AzureRmResourceGroupDeployment -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.parameters.json'
    ```

## <a name="deploy-the-template-by-using-the-azure-cli"></a>De sjabloon implementeren met de Azure CLI

Volg onderstaande stappen als u de sjabloon wilt implementeren met de Azure CLI.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../xplat-cli-install.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de opdracht **azure config mode** uit om over te schakelen naar de modus Resource Manager, zoals hieronder weergegeven.

    ```azurecli
        azure config mode arm
    ```

    Dit is de verwachte uitvoer voor de bovenstaande opdracht:

        info:    New mode is arm

3. Vanuit de browser navigeert u naar de [Quickstart-sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules). Kopieer de inhoud van het json-bestand en plak deze in een nieuw bestand op uw computer. In dit scenario kopieert u de onderstaande waarden naar een bestand met de naam **c:\lb\azuredeploy.parameters.json**.
4. Voer de cmdlet **azure group deployment create** uit om de nieuwe load balancer te implementeren met behulp van de sjabloon en de parameterbestanden die u hebt gedownload en hierboven hebt gewijzigd. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    ```azurecli
        azure group create --name TestRG --location westus --template-file 'https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json' --parameters-file 'c:\lb\azuredeploy.parameters.json'
    ```

## <a name="next-steps"></a>Volgende stappen

[Aan de slag met het configureren van een interne load balancer](load-balancer-get-started-ilb-arm-ps.md)

[Een distributiemodus voor de load balancer configureren](load-balancer-distribution-mode.md)

[TCP-time-outinstellingen voor inactiviteit voor de load balancer configureren](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO2-->


