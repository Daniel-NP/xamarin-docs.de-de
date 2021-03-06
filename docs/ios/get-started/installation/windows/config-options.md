---
title: Konfigurieren von Visual Studio 2017
description: In diesem Artikel werden verschiedene Xamarin.iOS-Konfigurationsoptionen für Visual Studio 2017 beschrieben.
ms.prod: xamarin
ms.assetid: 22D82244-890D-4325-B3CC-C0AC49130BCA
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/16/2018
ms.openlocfilehash: ee08cf7d68bd9d10026f1c15d4438077349fe367
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2018
---
# <a name="configuring-visual-studio-2017"></a>Konfigurieren von Visual Studio 2017

_In diesem Artikel werden verschiedene Xamarin.iOS-Konfigurationsoptionen für Visual Studio beschrieben._

## <a name="using-matching-xamarinios-versions"></a>Verwenden übereinstimmender Xamarin.iOS Versionen

Visual Studio 2017 muss dieselbe Version von Xamarin.iOS verwenden, die auf dem Mac-Buildhost installiert ist. So stellen Sie sicher, dass dies der Fall ist:

 - Wählen Sie bei Verwendung von Visual Studio 2017 den Updatekanal **Stabil** in Visual Studio für Mac aus.

 - Wählen Sie bei Verwendung der Vorschauversion von Visual Studio 2017 den Updatekanal **Alpha** in Visual Studio für Mac aus.

> [!NOTE]
> Ab der [Version 15.6 von Visual Studio 2017](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning) wird automatisch erkannt, ob der Mac-Buildhost dieselbe Version von Xamarin.iOS wie Windows verwendet. Liegt ein Versionskonflikt vor, stellt Visual Studio 2017 die Option bereit, die richtige Version auf dem Mac-Buildhost per Remotezugriff zu installieren. Weitere Informationen finden Sie im Abschnitt [Automatische Bereitstellung eines Macs](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning) im Leitfaden [Durchführen einer Kopplung mit einem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

## <a name="ios-toolbar"></a>iOS-Symbolleiste

Wenn ein iOS-Projekt in Visual Studio 2017 geöffnet ist, sollte die iOS-Symbolleiste angezeigt werden.  Standardmäßig umfasst diese vier Schaltflächen, die für die Xamarin.iOS-Entwicklung nützlich sind:

![iOS-Symbolleiste in Visual Studio 2017 ](config-options-images/ios-toolbar.png "Visual Studio 2017's iOS toolbar")

- **Mit Mac koppeln**: öffnet das Dialogfeld „Mit Mac koppeln“. Diese Option ist aktiviert, wenn ein iOS-Projekt in Visual Studio 2017 geöffnet ist.
- **iOS-Simulator anzeigen**: öffnet den iOS-Simulator auf dem Mac-Buildhost im Vordergrund. Diese Option ist aktiviert, wenn ein iOS-Projekt in Visual Studio 2017 geöffnet ist.
- **Geräteprotokoll**: öffnet ein Fenster, in dem Sie Geräteprotokolle untersuchen können. Diese Option ist aktiviert, wenn ein iOS-Projekt in Visual Studio 2017 geöffnet ist.
- **IPA-Datei auf Buildserver anzeigen**: öffnet ein Fenster auf dem Mac-Buildhost, in dem der Speicherort der IPA-Datei für die App angezeigt wird. Diese Option wird nach Abschluss eines Builds aktiviert, für den eine IPA-Datei erstellt wurde.

Wenn diese Symbolleiste nicht angezeigt wird, öffnen Sie das Menü **Ansicht** in Visual Studio 2017, und klicken Sie auf **Symbolleisten > iOS**:

![Aktivieren der iOS-Symbolleiste](config-options-images/ios-toolbar-enable.png "Enabling the iOS toolbar")

## <a name="solution-platforms-drop-down-menu"></a>Dropdownmenü „Projektmappenplattformen“

Über das Dropdownmenü **Projektmappenplattformen** können Sie festlegen, ob für Ihren nächsten Build ein physisches Gerät oder ein Simulator als Zielplattform verwendet werden soll.

So stellen Sie sicher, dass das Dropdownmenü in der Standardsymbolleiste angezeigt wird:

- Klicken Sie in Visual Studio 2017 rechts in der Symbolleiste auf den Abwärtspfeil.
- Wählen Sie **Schaltflächen hinzufügen oder entfernen** aus. 
- Stellen Sie sicher, dass das **Projektmappenplattformen**-Element aktiviert ist:

![Aktivieren des Dropdownmenüs „Projektmappenplattformen“](config-options-images/solution-platforms-enable.png "Enabling the Solution Platforms drop-down menu")

Falls ein iOS-Projekt geöffnet ist, sollten die **Standardsymbolleiste** und die **iOS-Symbolleiste** nun wie auf dem folgenden Screenshot aussehen:

![Standardsymbolleiste und iOS-Symbolleiste](config-options-images/toolbars.png "Standard and iOS toolbars")


