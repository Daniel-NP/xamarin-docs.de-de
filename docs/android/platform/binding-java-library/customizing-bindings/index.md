---
title: Anpassen von Bindungen
description: Sie können eine Xamarin.Android-Bindung anpassen, indem Sie bearbeiten die Metadaten, die den Bindungsprozess steuert. Diese manuellen Änderungen sind oft notwendig, zum Auflösen von Buildfehler und die resultierende-API können strukturiert werden, damit sie besser mit c# konsistent sind / .NET. Diese Handbücher erläutern die Struktur dieser Metadaten, wie die Metadaten zu ändern sowie deren JavaDoc zu können, um die Namen der Parameter der Methode wiederherzustellen.
ms.prod: xamarin
ms.assetid: 63C5078D-9E42-4F70-AF8C-8CEEA84FB6AF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 09/25/2017
ms.openlocfilehash: bb4f3b24be2072cb8b33893899a23951ace63607
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30763136"
---
# <a name="customizing-bindings"></a>Anpassen von Bindungen

_Sie können eine Xamarin.Android-Bindung anpassen, indem Sie bearbeiten die Metadaten, die den Bindungsprozess steuert. Diese manuellen Änderungen sind oft notwendig, zum Auflösen von Buildfehler und die resultierende-API können strukturiert werden, damit sie besser mit c# konsistent sind / .NET. Diese Handbücher erläutern die Struktur dieser Metadaten, wie die Metadaten zu ändern sowie deren JavaDoc zu können, um die Namen der Parameter der Methode wiederherzustellen._


## <a name="overview"></a>Übersicht
 
Xamarin.Android automatisiert einen Großteil des des Bindungsvorgangs; Allerdings ist in einigen Fällen manuelle Änderung erforderlich, um die folgenden Aspekte berücksichtigen:

-   Beheben von Fehler durch fehlende Typen, abgeblendete Typen, doppelte Namen, Klasse Sichtbarkeit Probleme und andere Situationen, in denen nicht aufgelöst werden können erstellen durch die Xamarin.Android-Tools. 

-   Ändern die Zuordnung, die Xamarin.Android verwendet, um die Android-API für verschiedene Typen in c# zu binden (z. B. viele Entwickler bevorzugen Java zuordnen `int` Konstanten in c# `enum` Konstanten).

-   Entfernen von nicht verwendeten Typen, die nicht gebunden werden müssen. 

-   Hinzufügen von Typen, die über keine Entsprechung in der zugrunde liegenden Java-API verfügen. 

Sie können einige oder alle diese Änderungen vornehmen, durch Ändern der Metadaten, die den Bindungsprozess steuert.


## <a name="guides"></a>Führungslinien

Die folgenden Handbüchern beschrieben, die Metadaten, die den Bindungsprozess steuert und erläutert, wie diese Metadaten, um diese Probleme zu ändern:

-   [Java-Bindungen Metadaten](~/android/platform/binding-java-library/customizing-bindings/java-bindings-metadata.md) bietet eine Übersicht über die Metadaten, die in einer Java-Bindung aufgenommen wird.
    Es beschreibt die verschiedenen manuellen Schritte, die in einigen Fällen erforderlich sind, eine Bindung-Bibliothek für Java abgeschlossen, und es wird erläutert, wie Sie eine API verfügbar gemacht werden, von einer Bindung genauer .NET Entwurfsrichtlinien befolgen strukturieren.

-   [Benennen von Parametern mit Javadoc](~/android/platform/binding-java-library/customizing-bindings/naming-parameters-with-javadoc.md) wird erläutert, wie die Parameternamen in einem Projekt für die Java-Bindung mit Javadoc nutzen, die aus den gebundenen Java-Projekt wiederherstellen.


 

