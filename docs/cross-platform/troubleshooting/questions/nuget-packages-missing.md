---
title: Fehlende Pakete Fehler nach dem Aktualisieren von NuGet-Pakete
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: asb3993
ms.author: amburns
ms.openlocfilehash: 4f1c4f51b690e35711efc0fc4fac56565b9cb3c7
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33917381"
---
# <a name="missing-packages-error-after-updating-nuget-packages"></a>Fehlende Pakete Fehler nach dem Aktualisieren von NuGet-Pakete

Dieses Problem wurde in erster Linie auf Xamarin.Forms app Beispielprojektmappen gemeldet, aber das Potenzial für dieses Problem kann auftreten, für jedes Projekt, das NuGet-Pakete verwendet. 

Wenn nach dem Aktualisieren von NuGet-Pakete in das Projekt oder die Projektmappe, sehen Sie einen Fehler, der die alte Paket-Versionsnummern, z. B. verweist:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)

```

In diesem Beispiel *Xamarin.Forms.1.3.1.6296* wird die alte Versionsnummer, die mit dem NuGet-Paket-Update entfernt wurde.

Dies kann geschehen, wenn die XML-Elemente in der CSPROJ-Datei, die Versionsnummer des alten Pakets verweisen manuell hinzugefügt wurde oder bearbeitet, Nuget wird weder entfernt noch aktualisiert werden, wenn sie manuell hinzugefügt/bearbeitet, gewesen wäre, damit das Projekt jetzt für Pakete angezeigt wird, die wurden gelöscht. 

Um dieses Problem zu beheben, bearbeiten Sie die CSPROJ-Dateien manuell, und löschen Sie alle Elemente, die die alten Versionsnummer verweisen an. 

Beispielelemente zu entfernen (Wenn sie die Versionsnummer des alten Pakets aufweisen):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />

```

