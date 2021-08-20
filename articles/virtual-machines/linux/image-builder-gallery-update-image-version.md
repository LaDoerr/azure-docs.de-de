---
title: Erstellen einer neuen VM-Imageversion aus einer vorhandenen Imageversion mit Azure Image Builder
description: Erstellen einer neuen VM-Imageversion aus einer vorhandenen Imageversion mit Azure Image Builder unter Linux.
author: kof-f
ms.author: kofiforson
ms.reviewer: cynthn
ms.date: 03/02/2020
ms.topic: how-to
ms.service: virtual-machines
ms.subservice: image-builder
ms.openlocfilehash: 85f41327476ae96af8dcc28eaaa3e49ef3ce80e5
ms.sourcegitcommit: 2cff2a795ff39f7f0f427b5412869c65ca3d8515
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2021
ms.locfileid: "113596531"
---
# <a name="create-a-new-vm-image-version-from-an-existing-image-version-using-azure-image-builder-in-linux"></a>Erstellen einer neuen VM-Imageversion aus einer vorhandenen Imageversion mit Azure Image Builder unter Linux

In diesem Artikel erfahren Sie, wie Sie eine vorhandene Imageversion in einem [Katalog mit freigegebenen Images](../shared-image-galleries.md) aktualisieren und als neue Imageversion im Katalog veröffentlichen.

Wir verwenden zum Konfigurieren des Images eine JSON-Beispielvorlage. Wir verwenden die JSON-Datei [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/azure/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json). 


## <a name="register-the-features"></a>Registrieren des Features
Um Azure Image Builder zu verwenden, müssen Sie das Feature registrieren.

Überprüfen Sie die Registrierung.

```azurecli-interactive
az provider show -n Microsoft.VirtualMachineImages | grep registrationState
az provider show -n Microsoft.KeyVault | grep registrationState
az provider show -n Microsoft.Compute | grep registrationState
az provider show -n Microsoft.Storage | grep registrationState
az provider show -n Microsoft.Network | grep registrationState
```

Wenn nicht „registered“ ausgegeben wird, führen Sie den folgenden Befehl aus:

```azurecli-interactive
az provider register -n Microsoft.VirtualMachineImages
az provider register -n Microsoft.Compute
az provider register -n Microsoft.KeyVault
az provider register -n Microsoft.Storage
az provider register -n Microsoft.Network
```


## <a name="set-variables-and-permissions"></a>Festlegen von Variablen und Berechtigungen

Wenn Sie Ihren Katalog mit freigegebenen Images anhand von [Erstellen eines ein Images und Verteilen des Images in einem Katalog mit freigegebenen Images](image-builder-gallery.md) erstellt haben, haben Sie bereits einige der benötigten Variablen erstellt. Richten Sie andernfalls nun einige Variablen ein, die in diesem Beispiel verwendet werden.


```console
# Resource group name 
sigResourceGroup=ibLinuxGalleryRG
# Gallery location 
location=westus2
# Additional region to replicate the image version to 
additionalregion=eastus
# Name of the shared image gallery 
sigName=myIbGallery
# Name of the image definition to use
imageDefName=myIbImageDef
# image distribution metadata reference name
runOutputName=aibSIGLinuxUpdate
```

Erstellen Sie eine Variable für Ihre Abonnement-ID. Diese können Sie mit `az account show | grep id` abrufen.

```console
subscriptionID=<Subscription ID>
```

Rufen Sie die Imageversion ab, die Sie aktualisieren möchten.

```azurecli
sigDefImgVersionId=$(az sig image-version list \
   -g $sigResourceGroup \
   --gallery-name $sigName \
   --gallery-image-definition $imageDefName \
   --subscription $subscriptionID --query [].'id' -o json | grep 0. | tr -d '"' | tr -d '[:space:]')
```

## <a name="create-a-user-assigned-identity-and-set-permissions-on-the-resource-group"></a>Erstellen einer vom Benutzer zugewiesene Identität und Festlegen von Berechtigungen für die Ressourcengruppe
Da Sie im vorigen Beispiel die Benutzeridentität eingerichtet haben, müssen Sie nur deren Ressourcen-ID ermitteln, die dann an die Vorlage angefügt wird.

```azurecli-interactive
#get identity used previously
imgBuilderId=$(az identity list -g $sigResourceGroup --query "[?contains(name, 'aibBuiUserId')].id" -o tsv)
```

Wenn Sie bereits über einen eigenen Katalog mit freigegebenen Images verfügen und nicht nach dem vorherigen Beispiel vorgegangen sind, müssen Sie Image Builder Berechtigungen zum Zugreifen auf die Ressourcengruppe zuweisen, damit er auf den Katalog zugreifen kann. Überprüfen Sie die Schritte im Beispiel [Erstellen und Verteilen eines Images an eine Shared Image Gallery](image-builder-gallery.md).


## <a name="modify-helloimage-example"></a>Ändern des Beispiels helloImage
Sie können das verwendete Beispiel überprüfen, indem Sie die JSON-Datei [helloImageTemplateforSIGfromSIG.json](https://raw.githubusercontent.com/azure/azvmimagebuilder/master/quickquickstarts/2_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json) zusammen mit der [Image Builder-Vorlagenreferenz](image-builder-json.md) öffnen. 


Laden Sie das JSON-Beispiel herunter, und konfigurieren Sie es mit Ihren Variablen. 

```console
curl https://raw.githubusercontent.com/azure/azvmimagebuilder/master/quickquickstarts/8_Creating_a_Custom_Linux_Shared_Image_Gallery_Image_from_SIG/helloImageTemplateforSIGfromSIG.json -o helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<subscriptionID>/$subscriptionID/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<rgName>/$sigResourceGroup/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<imageDefName>/$imageDefName/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<sharedImageGalName>/$sigName/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s%<sigDefImgVersionId>%$sigDefImgVersionId%g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<region1>/$location/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<region2>/$additionalregion/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s/<runOutputName>/$runOutputName/g" helloImageTemplateforSIGfromSIG.json
sed -i -e "s%<imgBuilderId>%$imgBuilderId%g" helloImageTemplateforSIGfromSIG.json
```

## <a name="create-the-image"></a>Erstellen des Images

Senden Sie die Imagekonfiguration an den VM-Image Builder-Dienst.

```azurecli-interactive
az resource create \
    --resource-group $sigResourceGroup \
    --properties @helloImageTemplateforSIGfromSIG.json \
    --is-full-object \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n helloImageTemplateforSIGfromSIG01
```

Starten Sie den Imagebuild.

```azurecli-interactive
az resource invoke-action \
     --resource-group $sigResourceGroup \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n helloImageTemplateforSIGfromSIG01 \
     --action Run 
```

Warten Sie, bis das Image erstellt und repliziert wurde, bevor Sie mit dem nächsten Schritt fortfahren.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

```azurecli-interactive
az vm create \
  --resource-group $sigResourceGroup \
  --name aibImgVm001 \
  --admin-username azureuser \
  --location $location \
  --image "/subscriptions/$subscriptionID/resourceGroups/$sigResourceGroup/providers/Microsoft.Compute/galleries/$sigName/images/$imageDefName/versions/latest" \
  --generate-ssh-keys
```

Erstellen Sie eine SSH-Verbindung mit dem virtuellen Computer unter Verwendung seiner öffentlichen IP-Adresse.

```console
ssh azureuser@<pubIp>
```

Sie sollten sehen, dass das Image angepasst wurde. Es wird eine „Nachricht des Tages“ ausgegeben, sobald die SSH-Verbindung hergestellt wurde.

```output
*******************************************************
**            This VM was built from the:            **
**      !! AZURE VM IMAGE BUILDER Custom Image !!    **
**         You have just been Customized :-)         **
*******************************************************
```

Geben Sie `exit` ein, um die SSH-Verbindung zu schließen.

Sie können auch die Imageversionen auflisten, die nun in Ihrem Katalog verfügbar sind.

```azurecli-interactive
az sig image-version list -g $sigResourceGroup -r $sigName -i $imageDefName -o table
```


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Komponenten der in diesem Artikel verwendeten JSON-Datei finden Sie unter [Image Builder-Vorlagenreferenz](../linux/image-builder-json.md).
