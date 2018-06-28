---
title: 'Xamarin.Mac: Verwandte Dokumentationen'
description: Dieses Dokument enthält Links zu Dokumentationen, die für Xamarin.Mac-Entwickler relevant sind, z.B. zur Xamarin.iOS-Dokumentation, zum Mac Developer Center von Apple sowie zu verschiedenen Leitfäden, in denen das Erstellen von Benutzeroberflächen mit Xamarin.Mac beschrieben wird.
ms.prod: xamarin
ms.assetid: 0a282c58-1c37-4f73-8440-85de2daf454a
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
ms.openlocfilehash: 1bef756d89a92d082bd5ee29e18047bc4bed2498
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792527"
---
# <a name="xamarinmac--related-documentation"></a>Xamarin.Mac: Verwandte Dokumentationen

Zusätzlich zum Mac-Bereich auf [developer.xamarin.com](~/mac/get-started/index.md) gibt es drei Stellen, an denen Sie Dokumentationen finden, die Ihnen helfen können, wenn Sie Fragen zu Xamarin.Mac haben:

- [**Die Xamarin.iOS-Dokumentation**](~/ios/get-started/index.md): Für viele APIs (hauptsächlich außerhalb von AppKit und UIKit) bestehen nur geringfügige Unterschiede zwischen den iOS- und den macOS-Versionen. Wenn eine iOS-API z.B. den Namen `UIFoo` hat, können Sie häufig eine ähnliche API mit dem Namen `NSFoo` unter macOS finden. Diese Beispiele sind prinzipiell schon in C#.

- **Das [Mac Developer Center](https://developer.apple.com/devcenter/mac/) von Apple**: Häufig können Beispiele der aufzurufenden APIs in Objective-C unkompliziert in C# konvertiert werden. Weitere Informationen dazu finden Sie unter [Understanding Mac APIs (Grundlegendes zu Mac-APIs)](~/mac/app-fundamentals/mac-apis.md).

- [**Stack Overflow**](http://stackoverflow.com/): eine praktische Ressource für einfache, einmalige Fragen wie die folgende: [Wie kann ich alle Knoten in NSOutlineView automatisch erweitern?](http://stackoverflow.com/questions/519751/nsoutlineview-auto-expand-all-nodes). Diese Beispiele sind oft in Objective-C geschrieben und müssen in C# konvertiert werden. Es gibt jedoch auch einige Antworten in C#.

## <a name="user-interface"></a>Benutzeroberfläche

Ein Entwickler, der mit C# und .NET in einer Xamarin.Mac-Anwendung arbeitet, hat Zugang zu denselben Benutzeroberflächen-Steuerelementen wie ein Entwickler, der mit *Objective-C* und *Xcode* arbeitet. Da Xamarin.Mac direkt in Xcode integriert ist, kann der Entwickler _Interface Builder_ von Xcode verwenden, um die Benutzeroberfläche einer App zu erstellen und zu verwalten (oder um diese optional in C#-Code zu erstellen).

In folgenden Leitfäden erhalten Sie ausführliche Informationen zum Arbeiten mit macOS-Elementen in einer Xamarin.Mac-Anwendung:

- [Windows](~/mac/user-interface/window.md)
- [Dialogfelder](~/mac/user-interface/dialog.md)
- [Benachrichtigungen](~/mac/user-interface/alert.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Symbolleisten](~/mac/user-interface/toolbar.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
