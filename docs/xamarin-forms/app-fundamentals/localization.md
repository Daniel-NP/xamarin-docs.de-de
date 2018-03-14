---
title: Lokalisierung
description: "Xamarin.Forms-apps können mithilfe von .NET Ressourcendateien lokalisiert werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: 852B4ED3-2D2D-48A5-A759-A6591F6A1509
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/06/2016
ms.openlocfilehash: ffde89558495c4b9ccb9ec41761b5fc7ca53db38
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="localization"></a>Lokalisierung

_Xamarin.Forms-apps können mithilfe von .NET Ressourcendateien lokalisiert werden._

## <a name="overview"></a>Übersicht

Die integrierten Mechanismen zum Lokalisieren von .NET Applications verwendet [RESX-Dateien](http://msdn.microsoft.com/library/ekyft91f(v=vs.90).aspx) und die Klassen in der `System.Resources` und `System.Globalization` Namespaces. Die RESX-Dateien, die mit übersetzten Zeichenfolgen werden in die Xamarin.Forms-Assembly zusammen mit einer vom Compiler generierte Klasse eingebettet, die stark typisierten Zugriff auf die Übersetzungen bereitstellt. Der Übersetzungstext kann dann im Code abgerufen werden.

Dieses Dokument enthält folgende Abschnitte:

**Globalisieren von Xamarin.Forms-Code**

* Hinzufügen und Verwenden von Zeichenfolgenressourcen in einer Xamarin.Forms-PCL-app.
* Aktivieren die Spracherkennung in jedem der systemeigenen apps.

**Lokalisieren von XAML**

* Lokalisieren von XAML mit einer `IMarkupExtension`.
* Aktivieren die Markuperweiterung in den systemeigenen apps.

**Lokalisieren von Clientplattform-spezifische Elemente**

* Lokalisieren von Images und den Anwendungsnamen in den systemeigenen apps.

### <a name="sample-code"></a>Beispielcode

Es gibt zwei Beispiele, die mit diesem Dokument verbundene:

* [UsingResxLocalization](https://github.com/xamarin/xamarin-forms-samples/tree/master/UsingResxLocalization) ist eine sehr einfache Demonstration der Konzepte erläutert. Die unten gezeigten Codeausschnitte sind alle von diesem Beispiel.
* [TodoLocalized](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized) ist eine grundlegende funktionierende-app, die dieser Techniken für die Lokalisierung verwendet.

#### <a name="shared-projects-are-not-recommended"></a>Gemeinsam genutzte Projekte werden nicht empfohlen.

Das TodoLocalized-Beispiel enthält eine [freigegebenes Projekt Demo](https://github.com/xamarin/xamarin-forms-samples/tree/master/TodoLocalized/SharedProject/) jedoch aufgrund der Einschränkungen des Buildsystems die Ressourcendateien nicht erhalten eine **. designer.cs** generierte Datei, den Zugriff auf unterbricht, übersetzte Zeichenfolgen [im Code stark typisierte](~/xamarin-forms/app-fundamentals/localization.md).

Im weiteren Verlauf dieses Dokuments bezieht sich auf Projekte mithilfe der Xamarin.Forms PCL-Vorlage.

## <a name="globalizing-xamarinforms-code"></a>Globalisieren von Xamarin.Forms-Code

**Globalisieren von** eine Anwendung versteht man somit "World-Ready". Dies bedeutet, dass beim Schreiben von Code, die zum Anzeigen von verschiedenen Sprachen kann.

Einer der wichtigsten Bestandteile Globalisieren von einer Anwendung wird die Benutzeroberfläche erstellen, so, dass keine *hartcodierte* Text. Alle Elemente angezeigt, die dem Benutzer sollten stattdessen aus einem Satz von Zeichenfolgen abgerufen werden, die in die ausgewählte Sprache übersetzt wurden.

In diesem Dokument untersuchen wir so verwenden Sie RESX-Dateien zum Speichern dieser Zeichenfolgen und für die Anzeige je nach Einstellung des Benutzers abrufen.

Die Beispiele sind als Ziel für Englisch, Französisch, Spanisch, Deutsch, Chinesisch, Japanisch, Russisch und Portugiesisch (Brasilien) Sprachen. Anwendungen können so wenig oder beliebig viele Sprachen nach Bedarf übersetzt werden.

### <a name="adding-resources"></a>Hinzufügen von Ressourcen

Der erste Schritt beim Globalisieren einer Xamarin.Forms-PCL-Anwendung besteht die RESX-Ressourcendateien hinzuzufügen, die zum Speichern von des verwendeten Texts in der app verwendet werden. Wir müssen eine RESX-Datei, die den Standardtext enthält, hinzufügen und dann zusätzliche RESX-Dateien für jede Sprache, die wir unterstützen möchten.

#### <a name="base-language-resource"></a>Basissprache Ressource

Die Basis-Ressourcendatei (RESX) enthält die standardmäßige sprachenzeichenfolgen (in den Beispielen wird davon ausgegangen, dass Englisch die Standardsprache ist). Die Datei auf das Codeprojekt der Xamarin.Forms gemeinsame hinzufügen, indem Sie mit der rechten Maustaste auf das Projekt und **hinzufügen > neuen Datei...** .

Wählen Sie einen aussagekräftigen Namen wie **AppResources** , und drücken Sie **OK**.

[![Fügen Sie die Ressourcendatei](localization-images/resx-new-file-sml.png "Dialogfeld "neue Datei"")](localization-images/resx-new-file.png#lightbox "Dialogfeld "neue Datei"")

Zwei Dateien werden dem Projekt hinzugefügt werden:

* **AppResources.resx** Datei, wobei übersetzbare Zeichenfolgen in einem XML-Format gespeichert werden.
* **AppResources.designer.cs** -Datei, die enthalten Verweise auf alle Elemente, die in der RESX XML-Datei erstellt eine partielle Klasse deklariert.

Die Struktur der Lösung werden die Dateien nach verwandten angezeigt. Die RESX-Datei *sollten* bearbeitet werden, um das Hinzufügen von neuer übersetzbare-Zeichenfolgen; die **. designer.cs** Datei sollte *nicht* bearbeitet werden.

![](localization-images/appresources-tree.png "AppResources.resx-Datei")

##### <a name="string-visibility"></a>Zeichenfolge-Sichtbarkeit

Standardmäßig bei der Generierung von Verweisen auf Zeichenfolgen stark typisierte werden `internal` auf die Assembly. Dies ist, da das Standard-Buildtool für RESX-Dateien generiert die **. designer.cs** -Datei mit `internal` Eigenschaften.

Wählen Sie die **AppResources.resx** Datei, und zeigen die **Eigenschaften** mit Leerstellen auffüllen, um zu ermitteln, in denen diese Buildtool konfigurieren. Der Screenshot unten zeigt die **benutzerdefiniertes Tool: ResXFileCodeGenerator**.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-internal-sml.png "Eigenschaftenfenster für AppResources.Resx")](localization-images/vs-resx-internal.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Eigenschaften mit Leerstellen auffüllen für AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)

-----

Die stark typisierte Zeichenfolgeneigenschaften vornehmen `public`, müssen Sie die Konfiguration manuell ändern **benutzerdefiniertes Tool: PublicResXFileCodeGenerator**, wie im folgenden Screenshot gezeigt:


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](localization-images/vs-resx-public-sml.png "Eigenschaftenfenster für AppResources.Resx")](localization-images/vs-resx-public.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](localization-images/xs-resx-internal-sml.png "Eigenschaften mit Leerstellen auffüllen für AppResources.Resx")](localization-images/xs-resx-internal.png#lightbox)


[![](localization-images/xs-resx-public-sml.png "Eigenschaften mit Leerstellen auffüllen für AppResources.Resx")](localization-images/xs-resx-public.png#lightbox)

-----

Diese Änderung ist optional, und ist nur erforderlich, wenn Sie lokalisierte Zeichenfolgen in anderen Assemblys zu verweisen (z. B., wenn Sie die RESX-Dateien in einer anderen Assembly in den Code einfügen) möchten. Das Beispiel in diesem Thema bewirkt, dass die Zeichenfolgen `internal` , da sie in der gleichen Xamarin.Forms PCL-Assembly definiert sind, in denen sie verwendet werden.

Sie müssen nur die benutzerdefinierten Tools auf der Basis RESX-Datei festlegen, wie oben gezeigt; Sie müssen nicht festlegen *alle* Buildtool auf den sprachspezifischen RESX-Dateien in den folgenden Abschnitten erläutert.

##### <a name="editing-the-resx-file"></a>Die RESX-Datei bearbeiten

Leider ist gibt es kein integrierter RESX-Editor in Visual Studio für Mac Das Hinzufügen von neuen übersetzbare Zeichenfolgen erfordert das Hinzufügen einer neuen XML- `data` -Element für jede Zeichenfolge. Jede `data` -Element kann Folgendes enthalten:

* `name` Attribut (erforderlich) ist der Schlüssel für diese Zeichenfolge übersetzt. Es muss ein gültiger c#-Eigenschaftsname - sein, damit keine Leerzeichen oder Sonderzeichen enthalten dürfen.
* `value` Element (erforderlich), das die tatsächliche Zeichenfolge, die in der Anwendung angezeigt wird.
* `comment` (optional)-Element kann Anweisungen für das Konvertierungsprogramm enthalten, aus der hervorgeht, wie diese Zeichenfolge verwendet wird.
* `xml:space` Attribut (optional), um zu steuern, wie der Abstand in der Zeichenfolge beibehalten wird.

Beispiele für eine `data` Elemente sind hier dargestellt:

```xml
<data name="NotesLabel" xml:space="preserve">
    <value>Notes:</value>
    <comment>label for input field</comment>
</data>
<data name="NotesPlaceholder" xml:space="preserve">
    <value>eg. buy milk</value>
    <comment>example input for notes field</comment>
</data>
<data name="AddButton" xml:space="preserve">
    <value>Add new item</value>
</data>
```

Wenn die Anwendung geschrieben wird, jedes Datenelement dem Benutzer angezeigten Text hinzugefügt werden sollen die Basis RESX-Ressourcen-Datei in einem neuen `data` Element. Es wird empfohlen, dass Sie enthalten `comment`s so weit wie möglich ist, um sicherzustellen, dass eine qualitativ hochwertige Übersetzung.

> [!NOTE]
> Visual Studio (einschließlich der kostenlosen Community Edition) enthält einen einfachen RESX-Editor. Wenn auf einem Windows-Computer zugreifen kann, die eine praktische Möglichkeit zum Hinzufügen und Bearbeiten von Zeichenfolgen in RESX-Dateien sein.

#### <a name="language-specific-resources"></a>Sprachspezifische Ressourcen

In der Regel nicht die eigentliche Übersetzung der Textzeichenfolgen standardmäßig ausgeführt, bis große Stücke aus der Anwendung (in diesem Fall die standardmäßige RESX-Datei viele Zeichenfolgen enthält) geschrieben wurden. Es ist immer noch eine gute Idee, frühzeitig im Entwicklungszyklus, die sprachspezifische Ressourcen hinzufügen optional Auffüllen mit maschinell übersetzten Text zum unterstützen der Lokalisierungscode zu testen.

Eine zusätzliche RESX-Datei wird für jede Sprache hinzugefügt, die wir unterstützen möchten.
Sprachspezifischen Ressourcendateien müssen eine bestimmte Benennungskonvention folgen: Verwenden Sie denselben Dateinamen wie die grundlegenden Ressourcen (z. b. Datei **AppResources**) gefolgt von einem Punkt (.) und der Sprachcode. Einfache Beispiele:

* **AppResources.fr.resx** -sprachübersetzungen Französisch.
* **AppResources.es.resx** -Spanisch Übersetzungen.
* **AppResources.de.resx** -deutschsprachige Übersetzungen.
* **AppResources.ja.resx** -japanischsprachigen Übersetzungen.
* **AppResources.zh Hans.resx** - Chinesisch (vereinfacht) sprachübersetzungen.
* **AppResources.zh Hant.resx** - Chinesisch (traditionell) sprachübersetzungen.
* **AppResources.pt.resx** -portugiesische Übersetzungen.
* **AppResources.pt BR.resx** -Brasilianisches Portugiesisch Übersetzungen.

Das allgemeine Muster ist die Verwendung von zwei Buchstaben bestehende Sprachcode, aber es gibt einige Beispiele für (z. B. Chinesisch), in dem ein anderes Format verwendet wird, und andere Beispiele (z. B. Portugiesisch (Brasilien)), ein vierstelligen Gebietsschemabezeichner erforderlich ist.

Diese sprachspezifischen Ressourcendateien *nicht* erfordern eine **. designer.cs** partielle Klasse, damit sie als reguläre XML-Dateien mit hinzugefügt werden können die **Buildvorgang: EmbeddedResource**festgelegt. Diese bildschirmabbildung zeigt eine Projektmappe, die sprachspezifischen Ressourcendateien enthält:

![](localization-images/appresources-langs.png "Sprachspezifischen Ressourcendateien")

Eine Anwendung entwickelt wird, und die base RESX-Datei Text hinzugefügt wurde, Sie sollten senden Übersetzer, die jede übersetzt wird `data` Element und der Rückgabewert eine sprachspezifische Ressourcendatei (mit der Benennungskonvention gezeigt), in der app eingeschlossen werden sollen. Beispiele für "Computer übersetzt" werden unten gezeigt:

**AppResources.es.resx (Spanisch)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>Escribir un artículo</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.ja.resx (Japanese)**

```xml
<data name="AddButton" xml:space="preserve">
    <value>新しい項目を追加</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

**AppResources.pt-BR.resx (Portugiesisch (Brasilien))**

```xml
<data name="AddButton" xml:space="preserve">
    <value>adicionar novo item</value>
    <comment>this string appears on a button to add a new item to the list</comment>
</data>
```

Nur die `value` Element muss vom Konvertierungsprogramm - aktualisiert werden die `comment` nicht übersetzt werden soll. Denken Sie daran: beim Bearbeiten von XML-Dateien als Escapesequenz für reservierte Zeichen wie `<`, `>`, `&` mit `&lt;`, `&gt;`, und `&amp;` erscheint in der `value` oder `comment`.

<a name="incode" />

### <a name="using-resources-in-code"></a>Verwenden von Ressourcen in Code

Zeichenfolgen in die RESX-Ressourcendateien stehen zur Verwendung in dem Benutzer Schnittstelle Code mit der `AppResources` Klasse. Die `name` zugewiesene jede Zeichenfolge in die RESX-Datei wird eine Eigenschaft für diese Klasse das Xamarin.Forms-Code verwiesen werden kann, wie unten dargestellt:

```csharp
var myLabel = new Label ();
var myEntry = new Entry ();
var myButton = new Button ();
// populate UI with translated text values from resources
myLabel.Text = AppResources.NotesLabel;
myEntry.Placeholder = AppResources.NotesPlaceholder;
myButton.Text = AppResources.AddButton;
```

Die Benutzeroberfläche unter iOS, Android und die Plattformen Windows gerendert wird, als Sie erwarten, außer jetzt es ist möglich, die app in mehrere Sprachen übersetzt werden, weil der Text aus einer Ressource geladen wird anstatt hartcodiert. Hier ist ein Screenshot, der auf jeder Plattform vor der Übersetzung die Benutzeroberfläche anzeigt:

![](localization-images/simple-example-english.png "Plattformübergreifende Benutzeroberflächen vor der Übersetzung")

### <a name="troubleshooting"></a>Problembehandlung

#### <a name="testing-a-specific-language"></a>Testen einer bestimmten Sprache

Es kann schwierig, wechseln Sie im Simulator oder ein Gerät in anderen Sprachen, besonders während der Entwicklung, wenn Sie unterschiedliche Kulturen schnell testen möchten.

Sie können erzwingen, dass eine bestimmte Sprache zum Laden durch Festlegen der `Culture` wie in diesem Codeausschnitt gezeigt:

```csharp
// force a specific culture, useful for quick testing
AppResources.Culture =  new CultureInfo("fr-FR");
```

Dieser Ansatz – Festlegen der Kultur direkt auf die `AppResources` -Klasse – kann auch zum Implementieren der Sprachauswahl in Ihrer app (statt mit dem Gerät Gebietsschema) verwendet werden.

#### <a name="loading-embedded-resources"></a>Laden von eingebettete Ressourcen

Der folgende Codeausschnitt ist hilfreich beim Debuggen von Problemen mit eingebetteten Ressourcen (z. B. RESX-Dateien). Fügen Sie diesen Code zu Ihrer app (einem frühen Zeitpunkt im Lebenszyklus Anwendung), und es werden alle in der Assembly, die mit der vollständigen Ressourcenbezeichner eingebetteten Ressourcen aufgeführt:

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly; // "EmbeddedImages" should be a class in your app
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

In der **AppResources.Designer.cs** Datei (um *Zeile 33*), die vollständige *Manager Ressourcenname* angegeben ist (z. b. `"UsingResxLocalization.Resx.AppResources"`) den folgenden Code ähnelt:

```csharp
System.Resources.ResourceManager temp =
        new System.Resources.ResourceManager(
                "UsingResxLocalization.Resx.AppResources",
                typeof(AppResources).GetTypeInfo().Assembly);
```

Überprüfen Sie die **Anwendungsausgabe** für die Ergebnisse von den oben aufgeführten Code Debuggen, um zu bestätigen die richtigen Ressourcen aufgelisteten (d. h. `"UsingResxLocalization.Resx.AppResources"`).

Wenn dies nicht der Fall, wird die `AppResources` Klasse wird in der Lage, ihre Ressourcen geladen werden.
Überprüfen Sie Folgendes ein, um Probleme zu beheben, in dem die Ressourcen können nicht gefunden werden:

* Der Standardnamespace für das Projekt entspricht den Stamm-Namespace in der **AppResources.Designer.cs** Datei.
* Wenn die **AppResources.resx** Datei befindet sich in einem Unterverzeichnis, das den Namen des Unterverzeichnisses muss Teil des Namespace *und* den Ressourcenbezeichner Teil.
* Die **AppResources.resx** Datei **Buildvorgang: EmbeddedResource**.
* Die **Projektoptionen > Quellcode > .NET Naming Policies > Verwenden von Visual Studio-Stil Ressourcen Namen** Konfigurationsdaten. Sie können dies untick, falls gewünscht, müssen jedoch die Namespaces verwendet, wenn die RESX-Ressourcen zu verweisen, die in der app aktualisiert werden.

#### <a name="doesnt-work-in-debug-mode-android-only"></a>Funktioniert nicht im Debugmodus (nur Android)

Wenn die übersetzten Zeichenfolgen in Ihre Version von Android-Builds jedoch nicht während des Debuggens verwenden, rechtsklicken Sie auf die **Android-Projekts** , und wählen Sie **Optionen > Erstellen > Android erstellen** und sicherstellen, dass die **Für die schnelle Assemblybereitstellung** nicht Konfigurationsdaten. Diese Option führt zu Problemen beim Laden von Ressourcen und sollte nicht verwendet werden, wenn Sie lokalisierte apps testen.

### <a name="displaying-the-correct-language"></a>Die richtige Sprache anzeigen

Bisher haben wir untersucht, wie Sie Code schreiben, sodass Übersetzungen bereitgestellt werden können, aber nicht, um tatsächlich sie anzuzeigen. Xamarin.Forms-Code kann von nutzen. NET Ressourcen, laden Sie die richtige sprachübersetzungen, aber wir müssen Abfragen des Betriebssystems auf jeder Plattform aus, um zu bestimmen, welche Sprache der Benutzer ausgewählt hat.

Da einige plattformspezifischen Code zum Abrufen der Spracheinstellung des Benutzers erforderlich ist, verwenden Sie eine [Abhängigkeitsdienst](~/xamarin-forms/app-fundamentals/dependency-service/index.md) für jede Plattform zu implementieren und machen diese Informationen in der app Xamarin.Forms.

Zuerst definieren Sie eine Schnittstelle, um die bevorzugte Kultur des Benutzers, ähnlich dem folgenden Code verfügbar zu machen:

```csharp
public interface ILocalize
{
    CultureInfo GetCurrentCultureInfo ();
    void SetLocale (CultureInfo ci);
}
```

Verwenden Sie die [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) in der Xamarin.Forms `App` Klasse aufrufen, die Schnittstelle und die Kultur unsere RESX-Ressourcen auf den richtigen Wert festgelegt. Beachten Sie, dass wir nicht zum manuell festlegen dieses Werts für Windows Phone und universellen Windows-Plattform, seit das Framework Ressourcen automatisch brauchen erkennt die ausgewählte Sprache auf diesen Plattformen.

```csharp
if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
{
    var ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
    Resx.AppResources.Culture = ci; // set the RESX for resource localization
    DependencyService.Get<ILocalize>().SetLocale(ci); // set the Thread for locale-aware methods
}
```

Die Ressource `Culture` muss festgelegt werden, wenn die Anwendung zuerst lädt, sodass die richtige sprachenzeichenfolgen verwendet werden. Dieser Wert entsprechend plattformspezifischen-Ereignissen, die möglicherweise auf iOS oder Android ausgelöst werden, wenn der Benutzer ihrer bevorzugten Sprache aktualisiert, während die app ausgeführt wird, kann optional aktualisiert werden.

Die Implementierungen für die `ILocalize` Schnittstelle werden angezeigt, der [plattformspezifischen Code](#Platform-specific_Code) Abschnitt weiter unten. Diese Implementierungen nutzen dieses `PlatformCulture` Hilfsklasse:

```csharp
public class PlatformCulture
{
    public PlatformCulture (string platformCultureString)
    {
        if (String.IsNullOrEmpty(platformCultureString))
        {
            throw new ArgumentException("Expected culture identifier", "platformCultureString"); // in C# 6 use nameof(platformCultureString)
        }
        PlatformString = platformCultureString.Replace("_", "-"); // .NET expects dash, not underscore
        var dashIndex = PlatformString.IndexOf("-", StringComparison.Ordinal);
        if (dashIndex > 0)
        {
            var parts = PlatformString.Split('-');
            LanguageCode = parts[0];
            LocaleCode = parts[1];
        }
        else
        {
            LanguageCode = PlatformString;
            LocaleCode = "";
        }
    }
    public string PlatformString { get; private set; }
    public string LanguageCode { get; private set; }
    public string LocaleCode { get; private set; }
    public override string ToString()
    {
        return PlatformString;
    }
}
```

<a name="Platform-specific_Code" />

### <a name="platform-specific-code"></a>Plattformspezifischen Code

Der Code zum erkennen, welche Sprache angezeigt plattformspezifischen sein muss, da diese Informationen iOS-, Android- und Windows-Plattformen etwas unterschiedlich verfügbar machen. Der Code für die `ILocalize` Abhängigkeitsdienst erfolgt unten für jede Plattform zusammen mit zusätzlichen Clientplattform-spezifische Anforderungen, um sicherzustellen, dass lokalisierte Text ordnungsgemäß gerendert wird.

Die plattformspezifischen Code muss auch Fälle, in dem das Betriebssystem ermöglicht den Benutzer, einen Gebietsschemabezeichner konfigurieren, der von nicht unterstützt wird. NET `CultureInfo` Klasse. In diesen Fällen muss benutzerdefinierter Code geschrieben werden, Erkennen von nicht unterstützten Gebietsschemas und Ersetzen am besten geeignet. NET-kompatiblen Gebietsschema.

#### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS-Benutzer wählen Sie ihre bevorzugte Sprache separat von Datums- und Uhrzeitangabe Kultur zu formatieren. Laden Sie die erforderlichen Ressourcen zum Lokalisieren einer Xamarin.Forms-app, es lediglich zum Abfragen muss, der `NSLocale.PreferredLanguages` Array für das erste Element.

Die folgende Implementierung des der `ILocalize` Abhängigkeitsdienst platziert werden soll, in der iOS-Anwendungsprojekt. Da iOS unterstrichen anstelle von Gedankenstrichen verwendet (also die standarddarstellung .NET) ersetzt der Code den Unterstrich vor der Instanziierung der `CultureInfo` Klasse:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.iOS.Localize))]

namespace UsingResxLocalization.iOS
{
public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo ()
        {
            var netLanguage = "en";
            if (NSLocale.PreferredLanguages.Length > 0)
            {
                var pref = NSLocale.PreferredLanguages [0];
                netLanguage = iOSToDotnetLanguage(pref);
            }
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string iOSToDotnetLanguage(string iOSLanguage)
        {
            var netLanguage = iOSLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (iOSLanguage)
            {
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage (PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "pt":
                    netLanguage = "pt-PT"; // fallback to Portuguese (Portugal)
                    break;
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                // add more application-specific cases here (if required)
                // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Die `try/catch` Blöcke in der `GetCurrentCultureInfo` Methode imitieren, die mit Gebietsschema-Spezifizierer – in der Regel verwendete, wenn die genaue Übereinstimmung nicht suchen für eine enge Übereinstimmung nur anhand der Sprache (erste Block von Zeichen in das Gebietsschema) gefunden wird, dieses Verhalten.
>
> Bei Xamarin.Forms mit, einigen Gebietsschemas kann in iOS gültig sind, sondern entsprechen keinem gültigen `CultureInfo` in .NET und der Code oben versucht, dieses Problem behoben.
>
> Beispielsweise iOS **Einstellungen > Allgemein Sprache &amp; Region** Bildschirm können Sie Ihr Telefon **Sprache** auf **Englisch** aber die  **Region** auf **Spanien**, was dazu führt, in einer gebietsschemazeichenfolge `"en-ES"`. Wenn die `CultureInfo` erstellen erzeugt einen Fehler, der Code auf zurückgegriffen nur die ersten beiden Buchstaben die Anzeigesprache auswählen.
>
> Entwickler sollten ändern, die `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` Methoden, um bestimmte Fälle für ihre unterstützten Sprachen erforderlich sind.


Es gibt einige systemdefinierte Benutzeroberflächenelemente, die automatisch von iOS, z. B. übersetzt werden die **Fertig** auf die Schaltfläche der `Picker` Steuerelement. So erzwingen iOS verwenden, um diese Elemente zu übersetzen, wir benötigen, um anzugeben, welche Sprachen in unterstützt, der **"Info.plist"** Datei. Sie können diese Werte über hinzufügen **"Info.plist" > Quelle** wie hier gezeigt:

![Lokalisierung Schlüssel in der Datei "Info.plist"](localization-images/info-plist.png "Lokalisierung Schlüssel in der Datei "Info.plist"")

Öffnen Sie alternativ die **"Info.plist"** Datei in einem XML-Editor, und bearbeiten Sie die Werte direkt:

```xml
<key>CFBundleLocalizations</key>
<array>
    <string>de</string>
    <string>es</string>
    <string>fr</string>
    <string>ja</string>
    <string>pt</string> <!-- Brazil -->
    <string>pt-PT</string> <!-- Portugal -->
    <string>ru</string>
    <string>zh-Hans</string>
    <string>zh-Hant</string>
</array>
<key>CFBundleDevelopmentRegion</key>
<string>en</string>
```

Nachdem Sie den Abhängigkeitsdienst implementiert und aktualisiert haben **"Info.plist"**, die iOS-app wird in der Lage, lokalisierten Text anzeigen.

> [!NOTE]
> Beachten Sie, dass Apple behandelt Portugiesisch etwas anders als Sie mussten erwarten.
> Von [ihre Dokumente](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/LocalizingYourApp/LocalizingYourApp.html#//apple_ref/doc/uid/10000171i-CH5-SW2): _"pt als verwenden die Sprach-ID für Portugiesisch es in Brasilien und pt-PT als die Sprach-ID für Portugiesisch verwendet wird, wie er in Portugal verwendet wird"_.
> Dies bedeutet, dass beim ausgewählt portugiesische in einem nicht standardmäßigen Gebietsschema fallback Sprache werden Portugiesisch (Brasilien) unter iOS, ab, es sei denn, der Code geschrieben wird, um dieses Verhalten zu ändern (z. B. die `ToDotnetFallbackLanguage` oben).

#### <a name="android-application-project"></a>Anwendungsprojekt für Android

Android verfügbar macht, das aktuell ausgewählte Gebietsschema über `Java.Util.Locale.Default`, und auch ein Unterstrich als Trennzeichen verwendet, statt einen Bindestrich (die durch den folgenden Code ersetzt wird). Fügen Sie dieser Abhängigkeit dienstimplementierung die Android-Anwendungsprojekt hinzu:

```csharp
[assembly:Dependency(typeof(UsingResxLocalization.Android.Localize))]

namespace UsingResxLocalization.Android
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale(CultureInfo ci)
        {
            Thread.CurrentThread.CurrentCulture = ci;
            Thread.CurrentThread.CurrentUICulture = ci;
        }
        public CultureInfo GetCurrentCultureInfo()
        {
            var netLanguage = "en";
            var androidLocale = Java.Util.Locale.Default;
            netLanguage = AndroidToDotnetLanguage(androidLocale.ToString().Replace("_", "-"));
            // this gets called a lot - try/catch can be expensive so consider caching or something
            System.Globalization.CultureInfo ci = null;
            try
            {
                ci = new System.Globalization.CultureInfo(netLanguage);
            }
            catch (CultureNotFoundException e1)
            {
                // iOS locale not valid .NET culture (eg. "en-ES" : English in Spain)
                // fallback to first characters, in this case "en"
                try
                {
                    var fallback = ToDotnetFallbackLanguage(new PlatformCulture(netLanguage));
                    ci = new System.Globalization.CultureInfo(fallback);
                }
                catch (CultureNotFoundException e2)
                {
                    // iOS language not valid .NET culture, falling back to English
                    ci = new System.Globalization.CultureInfo("en");
                }
            }
            return ci;
        }
        string AndroidToDotnetLanguage(string androidLanguage)
        {
            var netLanguage = androidLanguage;
            //certain languages need to be converted to CultureInfo equivalent
            switch (androidLanguage)
            {
                case "ms-BN":   // "Malaysian (Brunei)" not supported .NET culture
                case "ms-MY":   // "Malaysian (Malaysia)" not supported .NET culture
                case "ms-SG":   // "Malaysian (Singapore)" not supported .NET culture
                    netLanguage = "ms"; // closest supported
                    break;
                case "in-ID":  // "Indonesian (Indonesia)" has different code in  .NET
                    netLanguage = "id-ID"; // correct code for .NET
                    break;
                case "gsw-CH":  // "Schwiizertüütsch (Swiss German)" not supported .NET culture
                    netLanguage = "de-CH"; // closest supported
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
        string ToDotnetFallbackLanguage(PlatformCulture platCulture)
        {
            var netLanguage = platCulture.LanguageCode; // use the first part of the identifier (two chars, usually);
            switch (platCulture.LanguageCode)
            {
                case "gsw":
                    netLanguage = "de-CH"; // equivalent to German (Switzerland) for this app
                    break;
                    // add more application-specific cases here (if required)
                    // ONLY use cultures that have been tested and known to work
            }
            return netLanguage;
        }
    }
}
```

> [!NOTE]
> Die `try/catch` Blöcke in der `GetCurrentCultureInfo` Methode imitieren, die mit Gebietsschema-Spezifizierer – in der Regel verwendete, wenn die genaue Übereinstimmung nicht suchen für eine enge Übereinstimmung nur anhand der Sprache (erste Block von Zeichen in das Gebietsschema) gefunden wird, dieses Verhalten.
>
> Bei Xamarin.Forms mit, einigen Gebietsschemas kann in Android gültig sind, sondern entsprechen keinem gültigen `CultureInfo` in .NET und der Code oben versucht, dieses Problem behoben.
>
> Entwickler sollten ändern, die `iOSToDotnetLanguage` und `ToDotnetFallbackLanguage` Methoden, um bestimmte Fälle für ihre unterstützten Sprachen erforderlich sind.


Sobald das Android-Anwendungsprojekt mit diesem Code hinzugefügt wurde, wird es übersetzte Zeichenfolgen automatisch angezeigt werden.

> [!NOTE]
>️ **Warnung:** , wenn die übersetzten Zeichenfolgen in Ihre Version von Android-Builds jedoch nicht während des Debuggens verwenden, rechtsklicken Sie auf die **Android-Projekts** , und wählen Sie **Optionen > Erstellen > Android Erstellen Sie** und sicherstellen, dass die **für die schnelle Assemblybereitstellung** nicht Konfigurationsdaten. Diese Option führt zu Problemen beim Laden von Ressourcen und sollte nicht verwendet werden, wenn Sie lokalisierte apps testen.

#### <a name="windows-application-projects"></a>Windows-Anwendungsprojekten

Windows 8.1 und universelle Windows-Plattform (UWP) Projekte erfordern keine Abhängigkeitsdienst – diese Plattformen automatisch Kultur für die Ressource ordnungsgemäß festgelegt.

Implementieren die Verwendung von XAML-Markuperweiterung, die weiter unten in diesem Dokument beschriebenen erfordern möglicherweise die `ILocalize` unten für Windows Phone-Implementierung.

##### <a name="windows-phone-80"></a>Windows Phone 8.0

Obwohl in nicht verwendet die `App` Klasse, die hier ist die Windows Phone-Implementierung für die `ILocalize` Abhängigkeitsdienst. Fügen Sie dieser Klasse mit der Windows Phone-app-Projekt; Es wird erforderlich sein, wenn die Verwendung von XAML-Markuperweiterung, die weiter unten beschriebenen implementieren:

```csharp
[assembly: Dependency(typeof(UsingResxLocalization.WinPhone.Localize))]

namespace UsingResxLocalization.WinPhone
{
    public class Localize : UsingResxLocalization.ILocalize
    {
        public void SetLocale (CultureInfo ci) { }
        public System.Globalization.CultureInfo GetCurrentCultureInfo ()
        {
            return System.Threading.Thread.CurrentThread.CurrentUICulture;
        }
    }
}

```

Windows Phone 8.0-Projekte müssen ordnungsgemäß konfiguriert werden, damit lokalisierten Text, der angezeigt werden.
Unterstützte Sprachen müssen ausgewählt sein, in der Projektoptionen *und* der **WMAppManifest.xml** Dateien.
Wenn diese Einstellungen nicht aktualisiert werden, die lokalisierte RESX-Ressourcen werden nicht geladen werden.

##### <a name="project-options"></a>Projektoptionen

Mit der rechten Maustaste auf das Windows Phone-Projekt, und wählen Sie **Eigenschaften**. In der **Anwendung** Registerkarte Tick der **unterstützten Kulturen** , die von der Anwendung unterstützt:

[![](localization-images/winphone-projectproperties-sml.png "Projekteigenschaften - unterstützten Kulturen")](localization-images/winphone-projectproperties.png#lightbox "Projekteigenschaften - unterstützten Kulturen")

##### <a name="wmappmanifestxml"></a>WMAppManifest.xml

Erweitern Sie den Knoten "Eigenschaften" im Windows Phone-Projekt, und doppelklicken Sie auf die **WMAppManifest.xml** Datei. Klicken Sie auf die **Verpackung** Registerkarte, und aktivieren Sie die Sprachen, die von der Anwendung unterstützt.

[![](localization-images/winphone-wmappmanifest-sml.png "WMAppManifest.xml - unterstützte Sprachen")](localization-images/winphone-wmappmanifest.png#lightbox "WMAppManifest.xml - unterstützte Sprachen")

##### <a name="assemblyinfocs"></a>AssemblyInfo.cs

Erweitern Sie den Knoten "Eigenschaften" im Projekt Portable Klassenbibliothek (PCL), und doppelklicken Sie auf die **AssemblyInfo.cs** Datei. Fügen Sie die folgende Zeile zu der Datei, die kulturneutralen Ressourcen Assemblysprache auf Englisch festlegen:

```csharp
[assembly: NeutralResourcesLanguage("en")]
```

Damit wird den Ressourcen-Manager der Standardkultur für die app, daher sicherstellen, dass die Zeichenfolgen, die in der Sprache neutrale RESX-Datei definierten mitgeteilt (**AppResources.resx**) wird angezeigt, wenn die app in einem englischen Gebietsschemas ausgeführt wird.

### <a name="example"></a>Beispiel

Nach dem Aktualisieren der plattformspezifischen Projekte wie oben, und kompilieren Sie die app mit übersetzten RESX-Dateien erneut, werden die aktualisierte Übersetzungen in jeder app verfügbar. Hier ist ein Screenshot aus dem Beispielcode in Chinesisch (vereinfacht) übersetzt:

![](localization-images/simple-example-hans.png "Plattformübergreifende Benutzeroberflächen übersetzt, Chinesisch (vereinfacht)")

## <a name="localizing-xaml"></a>Lokalisieren von XAML

Beim Erstellen einer Xamarin.Forms-Benutzeroberfläche in XAML-Markup, mit Zeichenfolgen ähneln würde eingebettet direkt in der XML-Code:

```xaml
<Label Text="Notes:" />
<Entry Placeholder="eg. buy milk" />
<Button Text="Add to list" />
```

Im Idealfall konnte wir Benutzeroberflächen-Steuerelemente direkt in die XAML, die wir erstellen sonst können übersetzt eine *Markuperweiterung*. Der Code für eine Markuperweiterung, die die RESX-Ressourcen in XAML verfügbar macht, ist unten dargestellt. Diese Klasse sollte der allgemeine Xamarin.Forms-Code (zusammen mit der Verwendung von XAML-Seiten) hinzugefügt werden:

```csharp
using System;
using System.Globalization;
using System.Reflection;
using System.Resources;
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

namespace UsingResxLocalization
{
    // You exclude the 'Extension' suffix when using in Xaml markup
    [ContentProperty ("Text")]
    public class TranslateExtension : IMarkupExtension
    {
        readonly CultureInfo ci;
        const string ResourceId = "UsingResxLocalization.Resx.AppResources";

        private static readonly Lazy<ResourceManager> ResMgr = new Lazy<ResourceManager>(()=> new ResourceManager(ResourceId
                                                                                                                  , typeof(TranslateExtension).GetTypeInfo().Assembly));

        public TranslateExtension()
        {
            if (Device.RuntimePlatform == Device.iOS || Device.RuntimePlatform == Device.Android)
            {
                ci = DependencyService.Get<ILocalize>().GetCurrentCultureInfo();
            }
        }

        public string Text { get; set; }

        public object ProvideValue (IServiceProvider serviceProvider)
        {
            if (Text == null)
                return "";

            var translation = ResMgr.Value.GetString(Text, ci);

            if (translation == null)
            {
                #if DEBUG
                throw new ArgumentException(
                    String.Format("Key '{0}' was not found in resources '{1}' for culture '{2}'.", Text, ResourceId, ci.Name),
                    "Text");
                #else
                translation = Text; // returns the key, which GETS DISPLAYED TO THE USER
                #endif
            }
            return translation;
        }
    }
}
```

Die folgenden Abschnitten wird erläutert, die wichtigen Elemente im obigen Code:

* Die Klasse heißt `TranslateExtension`, entspricht jedoch gemäß der Konvention, verweisen wir auf **übersetzen** in unserer Markup.
* Die Klasse implementiert `IMarkupExtension`, das von Xamarin.Forms dafür Arbeit benötigt wird.
* `"UsingResxLocalization.Resx.AppResources"` ist der Ressourcenbezeichner für unsere RESX-Ressourcen. Es besteht unsere Standardnamespace, den Ordner, in dem die Ressourcendateien gespeichert sind, und der Standarddateiname RESX.
* Die `ResourceManager` Klasse wird erstellt, die mit `typeof(TranslateExtension)` zum Laden von Ressourcen aus die aktuelle Assembly zu bestimmen.
* `ci` verwendet den Abhängigkeitsdienst so das systemeigene Betriebssystem ausgewählte Sprache des Benutzers ab.
* `GetString` ist die Methode, die die tatsächliche übersetzte Zeichenfolge aus den Ressourcendateien abruft. Unter Windows Phone 8.1 und universellen Windows-Plattform `ci` wird null sein, da die `ILocalize` Schnittstelle ist nicht auf diesen Plattformen implementiert. Dies entspricht dem Aufrufen der `GetString` -Methode mit nur den ersten Parameter. Stattdessen wird die Ressourcen-Framework erkennt automatisch das Gebietsschema und wird die übersetzte Zeichenfolge aus der entsprechenden RESX-Datei abgerufen.
* Fehlerbehandlung eingefügt, um fehlende Ressourcen zu debuggen, indem eine Ausnahme ausgelöst wurde (in `DEBUG` nur im Modus).

Der folgende XAML-Ausschnitt zeigt, wie die Markuperweiterung verwendet wird. Es gibt zwei Schritte funktionieren:

1. Deklarieren von benutzerdefinierten `xmlns:i18n` Namespace im Stammknoten. Die `namespace` und `assembly` muss die projekteinstellungen genau – in diesem Beispiel diese identisch sind, aber möglicherweise unterschiedlich in Ihrem Projekt übereinstimmen.
2. Verwendung `{Binding}` Syntax auf Attribute, die normalerweise Text aufrufen, enthält die `Translate` Markuperweiterung. Der Ressourcenschlüssel ist der einzige erforderliche Parameter.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
        xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
        x:Class="UsingResxLocalization.FirstPageXaml"
        xmlns:i18n="clr-namespace:UsingResxLocalization;assembly=UsingResxLocalization">
    <StackLayout Padding="0, 20, 0, 0">
        <Label Text="{i18n:Translate NotesLabel}" />
        <Entry Placeholder="{i18n:Translate NotesPlaceholder}" />
        <Button Text="{i18n:Translate AddButton}" />
    </StackLayout>
</ContentPage>
```

Im folgenden ausführlicher Syntax ist auch für die Markuperweiterung gültig:

```xaml
<Button Text="{i18n:TranslateExtension Text=AddButton}" />
```

## <a name="localizing-platform-specific-elements"></a>Lokalisieren von Clientplattform-spezifische Elemente

Obwohl wir die Übersetzung der Benutzeroberfläche in Xamarin.Forms Code behandeln können, stehen einige Elemente, die in jedem Projekt plattformspezifischen ausgeführt werden müssen. Dieser Abschnitt befasst sich mit Informationen zum Lokalisieren von:

* Application Name
* Bilder

Das Beispielprojekt enthält eine lokalisierte Bildtyp namens **flag.png**, die verwiesen wird in c# wie folgt:

```csharp
var flag = new Image();
switch (Device.RuntimePlatform)
{
    case Device.iOS:
    case Device.Android:
        flag.Source = ImageSource.FromFile("flag.png");
        break;
    case Device.UWP:
        flag.Source = ImageSource.FromFile("Assets/Images/flag.png");
        break;
}
```

Das Flag-Bild wird auch in der XAML-Code wie folgt verwiesen:

```xaml
<Image>
  <Image.Source>
    <OnPlatform x:TypeArguments="ImageSource">
        <On Platform="iOS, Android" Value="flag.png" />
        <On Platform="UWP" Value="Assets/Images/flag.png" />
    </OnPlatform>
  </Image.Source>
</Image>
```

Alle Plattformen werden bildreferenzen wie diese auf lokalisierte Versionen der Bilder, automatisch aufgelöst werden, solange die Projektstrukturen, die nachfolgend beschrieben implementiert werden.

### <a name="ios-application-project"></a>Anwendungsprojekt für iOS

iOS verwendet einen Benennungsstandard Lokalisierungsprojekte genannte oder **.lproj** Verzeichnisse Image und Zeichenfolgenressourcen enthalten. Diese Verzeichnisse können enthalten lokalisierte Versionen von Bildern in der app, und auch die **InfoPlist.strings** -Datei, die zum Lokalisieren der app-Name verwendet werden kann.

#### <a name="images"></a>Bilder

Diese bildschirmabbildung zeigt die iOS-Beispiel-app mit sprachspezifischen **.lproj** Verzeichnisse. Das Verzeichnis spanische bezeichnet **es.lproj**, enthält die lokalisierte Versionen des Standardbilds, als auch **flag.png**:

![](localization-images/ios-resources.png "iOS Projektverzeichnisse Lokalisierung")

Jede Sprachenverzeichnis enthält eine Kopie des **flag.png**, für diese Sprache lokalisiert. Wenn kein Bild bereitgestellt wird, standardmäßig das Bild in das Standardverzeichnis für die Sprache des Betriebssystems. Geben Sie für vollständige Retina-Unterstützung  **@2x**  und  **@3x**  Kopien jedes Bild.

#### <a name="app-name"></a>App-Name

Der Inhalt der **InfoPlist.strings** ist nur ein einzelner Schlüssel-Wert zum Konfigurieren der app-Name:

```csharp
"CFBundleDisplayName" = "ResxEspañol";
```

Wenn die Anwendung ausgeführt wird, sind die app-Name und das Bild sowohl lokalisiert:

![](localization-images/ios-imageicon.png "iOS-Beispiel-App-Text und Image-Lokalisierung")

### <a name="android-application-project"></a>Anwendungsprojekt für Android

Android folgt ein anderes Schema für das Speichern von lokalisierten Images mit verschiedenen **zeichenbaren** und **Zeichenfolgen** Verzeichnisse mit einem Suffix der Language-Code. Wenn ein Gebietsschemacodes mit vier Buchstaben (z. B. Zh-TW oder pt-BR) erforderlich ist, beachten Sie, dass Android ein zusätzliches erfordert **r** nach Dash/vorangehenden code das Gebietsschema (z. b. Zh-rTW oder pt rBR).

#### <a name="images"></a>Bilder

Diese bildschirmabbildung zeigt die Android Beispiel mit einer einigen lokalisierten Drawables und Zeichenfolgen:

![](localization-images/android-resources.png "Android lokalisierte Zeichenfolge Verzeichnisse und Drawables")

Beachten Sie, dass Android Zh-Hans nicht verwendet wird und Zh-Hant-Codes für vereinfachtes und traditionelles Chinesisch; Stattdessen unterstützt nur landesspezifisch Codes Zh-CN und Zh-TW.

Zur Unterstützung der Bilder mit einer anderen Auflösung für HD-Bildschirme erstellen Sie zusätzliche sprachordnern mit `-*dpi` Suffixen, z. B. **Drawables-es-Mdpi**, **Drawables-es-Xdpi**, **Drawables-es-Xxdpi**usw. Finden Sie unter [Alternative Android Ressourcen bereitstellen](http://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources) für Weitere Informationen.

#### <a name="app-name"></a>App-Name

Der Inhalt der **strings.xml** ist nur ein einzelner Schlüssel-Wert zum Konfigurieren der app-Name:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">ResxEspañol</string>
</resources>
```

Update der **MainActivity.cs** im Android-app-Projekt, damit die `Label` verweist auf die XML-Zeichenfolgen.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

Die app wird zur Lokalisierung jetzt die app-Name und das Bild. Hier ist ein Screenshot, der das Ergebnis (in Spanisch):

![](localization-images/android-imageicon.png "Beispiel zu Android-App Text- und Image-Lokalisierung")

### <a name="windows-phone-80-application-project"></a>Windows Phone 8.0-Anwendungsprojekt

Windows Phone verfügt nicht über eine einfache, integrierte Möglichkeit, einen bestimmten lokalisierten Bild auswählen, noch zum Lokalisieren von der app-Name.

#### <a name="images"></a>Bilder

Zum Umgehen dieser Einschränkung liefert das Beispiel einen Vorschlag für lokalisierte Image Laden verwenden wie Sie implementiert möglicherweise eine [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) für die `Image` Steuerelement.

Der benutzerdefinierten Renderers Code wird unten - Wenn die Quelle ist ein `FileImageSource` und extrahiert den Dateinamen und erstellt einen Pfad mit einer lokalisierten Bild mithilfe der `CurrentUICulture`. Für einige Sprachen erfordern besondere Behandlung, sodass Zugriffe funktionieren wie erwartet; Im Beispiel wird standardmäßig nur die zwei Buchstaben bestehende Sprachcode außer in einigen besonderen Fällen zu verwenden:

```csharp
using System.IO;
using Xamarin.Forms;
using Xamarin.Forms.Platform.WinPhone;

[assembly: ExportRenderer(typeof(Image), typeof(UsingResxLocalization.WinPhone.LocalizedImageRenderer))]
namespace UsingResxLocalization.WinPhone
{
    public class LocalizedImageRenderer : ImageRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Image> e)
        {
            base.OnElementChanged(e);

            if (e.NewElement != null)
            {
                var s = e.NewElement.Source as FileImageSource;
                if (s != null)
                {
                    var fileName = s.File;
                    string ci = System.Threading.Thread.CurrentThread.CurrentUICulture.ToString();
                    // you might need some custom logic here to support particular cultures and fallbacks
                    if (ci == "pt-BR") {
                        // use the complete string 'as is'
                    } else if (ci == "zh-CN") {
                         // we could have named the image directories differently,
                         // but this keeps them consisent with RESX file naming
                        ci = "zh-Hans";
                    } else if (ci == "zh-TW" || ci == "zh-HK") {
                        ci = "zh-Hant";
                    } else {
                        // for all others, just use the two-character language code
                        ci = System.Threading.Thread.CurrentThread.CurrentUICulture.TwoLetterISOLanguageName;
                    }
                    e.NewElement.Source = Path.Combine("Assets/" + ci + "/" + fileName);
                }
            }
        }
    }
}
```

Dieser Code funktioniert mit den lokalisierten Bildern in der Verzeichnisstruktur unten angezeigt. Sie werden aufgefordert, den Code zum Erfüllen der Anforderungen der bestimmten Lokalisierung (z. B. die Behandlung von spezifischere Gebietsschemas und fallen zurück, wenn Images nicht verfügbar sind) zu ändern:

![](localization-images/winphone-resources.png "WinPhone lokalisiert Bilder-Verzeichnisstruktur")

Windows Phone zur Lokalisierung jetzt auf des Abbilds aus. Hier ist ein Screenshot, der das Ergebnis (in Spanisch und Chinesisch (vereinfacht)):

![](localization-images/winphone-image-sml.png "WinPhone-Beispiel-App-Text und Image-Lokalisierung")

#### <a name="app-name"></a>App-Name

Microsoft Dokumentation für [Lokalisieren von Windows Phone 8.0-app-Titel](http://msdn.microsoft.com/library/windows/apps/ff967550(v=vs.105).aspx).

### <a name="windows-phone-81-and-universal-windows-platform-application-projects"></a>Windows Phone 8.1 und Universal Windows Platform-Anwendungsprojekten

Windows Phone 8.1 und der universellen Windows-Plattform besitzt sowohl eine Ressource-Infrastruktur, die die Lokalisierung von Images und den Anwendungsnamen vereinfacht.

#### <a name="images"></a>Bilder

Bilder können lokalisiert werden, indem sie in einem Ordner ressourcenspezifischen platziert werden, wie im folgenden Screenshot gezeigt:

![](localization-images/uwp-image-folder-structure.png "WinPhone 8.1 und die Ordnerstruktur für uwp-Image Lokalisierung")

Zur Laufzeit wird die Windows-Ressource-Infrastruktur das passende Image basierend auf dem Gebietsschema des Benutzers ausgewählt.

#### <a name="app-name"></a>App-Name

Microsoft Dokumentation für [Windows 8.1 Store-apps: Lokalisieren von Informationen, die die app Benutzern beschrieben wird](https://msdn.microsoft.com/library/windows/apps/hh454044.aspx) und [Laden von Zeichenfolgen aus dem app-Manifest](https://msdn.microsoft.com/library/windows/apps/xaml/hh965323.aspx#loading_strings_from_the_app_manifest.).

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms-Anwendungen können mithilfe von RESX-Dateien und .NET-Klassen für die Globalisierung lokalisiert werden. Großteil der Lokalisierung ist nicht nur eine kleine Menge von plattformspezifischen Code zum Erkennen der Benutzer den bevorzugten Sprache, in der allgemeine Code zentralisiert.

Bilder werden in der Regel auf eine Weise plattformspezifischen zur Unterstützung der Multi-Lösung in iOS und Android nutzen behandelt. Windows Phone erfordert benutzerdefinierten Code zum Lokalisieren von Images auf Cross-Platform-freundliche Weise. Beispielcode wurde bereitgestellt, um diese Funktion hinzuzufügen.


## <a name="related-links"></a>Verwandte Links

- [RESX Localization Sample](https://developer.xamarin.com/samples/xamarin-forms/UsingResxLocalization/)
- [TodoLocalized-Beispiel-App](https://developer.xamarin.com/samples/xamarin-forms/TodoLocalized/)
- [Plattformübergreifende Lokalisierung](~/cross-platform/app-fundamentals/localization.md)
- [iOS Lokalisierung](~/ios/app-fundamentals/localization/index.md)
- [Android Lokalisierung](~/android/app-fundamentals/localization.md)
- [Verwenden der CultureInfo-Klasse (MSDN)](http://msdn.microsoft.com/library/87k6sx8t%28v=vs.90%29.aspx)
- [Suchen und Verwenden von Ressourcen für eine bestimmte Kultur (MSDN)](http://msdn.microsoft.com/library/s9ckwb4b%28v=vs.90%29.aspx)