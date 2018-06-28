---
title: Plattformfunktionen watchOS
description: Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, in denen WatchOS Plattformfunktionen, z. B. Apple Pay, Benachrichtigungen, Komplikationen, proaktive Vorschläge, Trainings-apps und vieles mehr beschrieben.
ms.prod: xamarin
ms.assetid: 13F23E01-BAED-43EB-A70E-3B30EF53D379
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/25/2018
ms.openlocfilehash: 16d10dd69223f404aac7c933302992a1544461e9
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066613"
---
# <a name="watchos-platform-features"></a>Plattformfunktionen watchOS

Dieses enthält Dokumentenlinks zu verschiedenen Handbüchern, in denen WatchOS Plattformfunktionen, z. B. Apple Pay, Benachrichtigungen, Komplikationen, proaktive Vorschläge, Trainings-apps und vieles mehr beschrieben.

## <a name="introduction-to-watchos-4introduction-to-watchos4md"></a>[Einführung in watchOS 4](introduction-to-watchos4.md)

Dieses Dokument enthält eine allgemeine Übersicht über Features hinzugefügt und im WatchOS 4 aktualisiert.

## <a name="introduction-to-watchos-3introduction-to-watchos3indexmd"></a>[Einführung in watchOS 3](introduction-to-watchos3/index.md)

Dieser Artikel beschreibt die neuen und aktualisierten-APIs in WatchOS 3.

## <a name="apple-pay-enhancementsioswatchosplatformapple-paymd"></a>[Apple Pay-Erweiterungen](~/ios/watchos/platform/apple-pay.md)

In WatchOS 3 wurde das Framework PassKit erweitert, um Unterstützung für sicheren, in der app Zahlungen (von sowohl physischen waren und Dienstleistungen) für die auf der Apple Watch-apps zu ermöglichen.

## <a name="background-tasksioswatchosplatformbackground-tasksmd"></a>[Hintergrundaufgaben](~/ios/watchos/platform/background-tasks.md)

WatchOS 3 führt mehrere Hintergrundaufgaben, die app nicht verwenden kann, die Informationen sicherstellen, dass er den Inhalt verfügt, die der Benutzer muss, bevor sie es öffnen aktualisieren.

## <a name="notificationsnotificationsmd"></a>[Benachrichtigungen](notifications.md)

Erfahren Sie, wie eine benutzerdefinierte Benachrichtigung behandeln in Watch-app bereitzustellen.

## <a name="complicationscomplicationsmd"></a>[Komplikationen](complications.md)

Fügen Sie Komplikation unterstützt das Anzeigen von aktuellen Daten auf dem Zifferblatt der Uhr hinzu.

## <a name="proactive-suggestionsioswatchosplatformproactive-suggestionsmd"></a>[Proaktive Vorschläge](~/ios/watchos/platform/proactive-suggestions.md)

WatchOS 3 ermöglicht der app, Informationen zum Benutzer in proaktiv präsentieren Kontexten angegeben. Zur Unterstützung dieser Funktion die [NSUserActivity](https://developer.apple.com/reference/foundation/nsuseractivity) enthält jetzt die `MapItem` -Eigenschaft, die die app, Informationen zum Speicherort für die spätere Verwendung durch andere apps bereitstellen kann.

## <a name="quick-interaction-techniquesioswatchosplatformquick-interaction-techniquesmd"></a>[Schnelle Interaktion Techniken](~/ios/watchos/platform/quick-interaction-techniques.md)

Eine schnelle Benutzerinteraktionen sind für das Erstellen von überzeugenden Apple Watch-apps und Komplikationen unverzichtbar. Neue WatchOS 3, ist Apple Unterstützung für den Zugriff auf die digitalen Crown und eine neue Benachrichtigung für Benutzer und Navigation Techniken Geste Merkmale hinzugefügt. Auf diese, zusammen mit Unterstützung für die SceneKit SpriteKit, können Entwickler auf einfache Weise umfangreiche, anzeigbare Schnittstellen zu erstellen, die schnelle und Reaktionsfähigkeit sind.

## <a name="workout-app-enhancementsioswatchosplatformworkout-appsmd"></a>[Trainings-app-Erweiterungen](~/ios/watchos/platform/workout-apps.md)

Neue WatchOS 3, Trainings bezüglich apps haben die Möglichkeit, auf der Apple Watch im Hintergrund ausgeführt. Um dieses Feature aktivieren (und erhalten Zugriff auf Daten HealthKit), muss die app enthalten die `WKBackgroundModes` -Schlüssel in der `Info.plist` Datei mit dem Wert `workout-processing`.
