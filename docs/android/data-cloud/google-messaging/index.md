---
title: Google-Messaging
description: Dieser Abschnitt enthält Anleitungen, die beschreiben, wie Xamarin.Android-apps mithilfe von Google-messaging-Diensten implementiert.
ms.prod: xamarin
ms.assetid: 85E8DF92-D160-4763-A7D3-458B4C31635F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: cf1eaec3dfee7c3457a4614147c9b5564843b2a7
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2018
ms.locfileid: "31044664"
---
# <a name="google-messaging"></a>Google-Messaging

_Dieser Abschnitt enthält Anleitungen, die beschreiben, wie Xamarin.Android-apps mithilfe von Google-messaging-Diensten implementiert._

## <a name="firebase-cloud-messagingfirebase-cloud-messagingmd"></a>[Firebase Cloud Messaging](firebase-cloud-messaging.md)

Firebase Cloud Messaging (FCM) ist ein Dienst, der erleichtert das messaging zwischen mobilen apps und serveranwendungen. FCM ist Google-Nachfolger von Google Cloud Messaging. Dieser Artikel bietet einen Überblick über die Funktionsweise von FCM, und es bietet eine schrittweise Anleitung für den Erwerb von Anmeldeinformationen, damit Ihre app FCM-Dienste verwenden kann.

## <a name="remote-notifications-with-firebase-cloud-messagingremote-notifications-with-fcmmd"></a>[Remote-Benachrichtigungen mit Firebase Cloud-Messaging](remote-notifications-with-fcm.md)

Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remote Benachrichtigungen (so genannte Pushbenachrichtigungen) zu implementieren, in einer Anwendung Xamarin.Android. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die für die Kommunikation mit Firebase Cloud Messaging (FCM) erforderlich sind, bietet Beispiele für die Vorgehensweise zum Konfigurieren der Android-Manifest für den Zugriff auf FCM und zeigt downstream-messaging mithilfe der Firebase Konsole.

## <a name="google-cloud-messaginggoogle-cloud-messagingmd"></a>[Google Cloud Messaging](google-cloud-messaging.md)

Dieser Abschnitt enthält einen allgemeinen Überblick darüber, wie Google Cloud Messaging (GCM) Nachrichten zwischen Ihrer app und ein app-Server weiterleitet, und es bietet eine schrittweise Anleitung für den Erwerb von Anmeldeinformationen, sodass Ihre app GCM-Dienste verwenden kann. (Beachten Sie, dass GCM von FCM ersetzt wurde.)

> [!NOTE]
> GCM wurde ersetzt wurde, indem Sie [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM-Server und Client-APIs [sind veraltet](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) und wird nicht mehr verfügbar sein so bald wie 11. April 2019.

## <a name="remote-notifications-with-google-cloud-messagingremote-notifications-with-gcmmd"></a>[Remote-Benachrichtigungen mit Google Cloud Messaging](remote-notifications-with-gcm.md)

Dieser Abschnitt enthält eine schrittweise Erklärung der Implementierung von remote-Benachrichtigungen in Xamarin.Android mithilfe von Google Cloud Messaging.
Es wird erläutert, die verschiedenen Komponenten, die genutzt werden müssen, um Google Cloud Messaging in einer Android-Anwendung zu aktivieren.


