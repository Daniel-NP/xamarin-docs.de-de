---
title: Xamarin.Forms lokale Datenbanken
description: Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code. In diesem Artikel wird beschrieben, wie Xamarin.Forms lesen und Schreiben von Daten in einer lokalen SQLite.Net mit SQLite-Datenbank.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
ms.openlocfilehash: 123e65f1efe31935167ca8684e89e7c0b4505443
ms.sourcegitcommit: 7a89735aed9ddf89c855fd33928915d72da40c2d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2018
ms.locfileid: "36209218"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms lokale Datenbanken

_Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code. In diesem Artikel wird beschrieben, wie Xamarin.Forms lesen und Schreiben von Daten in einer lokalen SQLite.Net mit SQLite-Datenbank._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Anwendungen können die [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) Paket integrieren Sie Datenbankvorgängen in freigegebenen Code durch Verweisen auf die `SQLite` Klassen, die in der NuGet geliefert. Datenbankvorgänge können definiert werden, in der standardmäßigen .NET Bibliotheksprojekt Xamarin.Forms-Projektmappe mit plattformspezifischen Projekte, die einen Pfad zum Speicherort der Datenbank zurückgegeben.

Der zugehörige [beispielanwendung](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) ist eine einfache Aufgabenlisten Anwendung. Die folgenden Screenshots zeigen, wie das Beispiel auf jeder Plattform wird angezeigt:

[![Xamarin.Forms Datenbank Beispiel Screenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots") [ ![ Xamarin.Forms Datenbank Beispiel Screenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Verwenden von SQLite

In diesem Abschnitt wird gezeigt, wie die SQLite.Net NuGet-Pakete auf einer Xamarin.Forms-Projektmappe hinzufügen, Schreiben von Methoden zum Ausführen von Datenbankvorgängen und verwenden Sie die [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) um einen Speicherort zum Speichern der Datenbank auf jeder Plattform zu bestimmen.

<a name="XamarinForms_PCL_Project" />

### <a name="xamarinsforms-net-standard-or-pcl-project"></a>Xamarins.Forms .NET Standard- oder PCL-Projekt

Um SQLite Unterstützung einer Xamarin.Forms-Projekt hinzugefügt haben, verwenden Sie NuGet Search-Funktion gefunden **Sqlite-Net-Pcl** und das aktuellste Paket installieren:

![Hinzufügen von NuGet SQLite.NET PCL Paket](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL Paket hinzufügen")

Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen, das richtige Paket wurde diese Attribute:

- **Erstellt von:** Frank A. Krueger
- **ID:** Sqlite-Net-Pcl
- **NuGet-Link:** [Sqlite-Net-Pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!TIP]
> Verwenden der **Sqlite-Net-Pcl** NuGet-Paket auch in .NET Standard-Projekten.

Sobald der Verweis hinzugefügt wurde, Schreiben Sie eine Schnittstelle, die Clientplattform-spezifische Funktionalität abstrahiert, um den Speicherort der Datenbankdatei zu ermitteln. Die im Beispiel verwendete Schnittstelle definiert eine einzelne Methode:

```csharp
public interface IFileHelper
{
  string GetLocalFilePath(string filename);
}
```

Nachdem die Schnittstelle definiert wurde, verwenden Sie die [ `DependencyService` ](~/xamarin-forms/app-fundamentals/dependency-service/index.md) erhalten eine Implementierung und erhalten einen lokalen Pfad (Beachten Sie, dass diese Schnittstelle wurde noch nicht implementiert). Der folgende Code Ruft eine Implementierung ab, der `App.Database` Eigenschaft:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(DependencyService.Get<IFileHelper>().GetLocalFilePath("TodoSQLite.db3"));
    }
    return database;
  }
}
```

Die `TodoItemDatabase` Konstruktor wird unten gezeigt:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Dieser Ansatz wird eine einzelne datenbankverbindung, die geöffnet gehalten wird, während die Anwendung ausgeführt wird, daher und vermeidet die Kosten öffnen und Schließen der Datei jedes Mal, wenn ein Datenbankvorgang ausgeführt wird, erstellt.

Im weiteren Verlauf der `TodoItemDatabase` Klasse enthält SQLite-Abfragen, die über Plattformen hinweg ausführen. Beispielcode für die Abfrage wird unten angezeigt (Weitere Informationen zur Syntax finden Sie der [SQLite.NET verwenden](~/cross-platform/app-fundamentals/index.md) Artikel):

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Der Vorteil der Verwendung der asynchronen SQLite.Net-API wird diese Datenbank, die Vorgänge in Hintergrundthreads verschoben werden. Darüber hinaus besteht keine Notwendigkeit, zusätzliche Verarbeitung von Code, da die API es übernimmt Parallelität zu schreiben.

Alle Datenzugriffscode ist geschrieben, in der .NET Standard-Bibliotheksprojekt für alle Plattformen gemeinsam genutzt werden. Nur einen lokalen Pfad für die Datenbank bereit ist plattformspezifischen Code, wie in den folgenden Abschnitten beschrieben.

<a name="PCL_iOS" />

### <a name="ios-project"></a>iOS-Projekt

Der einzige Code, der erforderlich ist der `IFileHelper` Implementierung, die den Dateipfad für die Daten bestimmt. Der folgende Code platziert die SQLite-Datenbank-Datei in die **Library-Datenbanken** Ordner innerhalb der Anwendung Sandkasten. Finden Sie unter der [iOS arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md) Dokumentation weitere Informationen zu den verschiedenen Verzeichnissen, die für den Speicher verfügbar sind.

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.iOS
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      string docFolder = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
      string libFolder = Path.Combine(docFolder, "..", "Library", "Databases");

      if (!Directory.Exists(libFolder))
      {
        Directory.CreateDirectory(libFolder);
      }

      return Path.Combine(libFolder, filename);
    }
  }
}
```

Beachten Sie, die den Code enthält die `assembly:Dependency` Attribut, damit diese Implementierung auffindbar ist die `DependencyService`.

<a name="PCL_Android" />

### <a name="android-project"></a>Android-Projekt

Der einzige Code, der erforderlich ist der `IFileHelper` Implementierung, die den Dateipfad für die Daten bestimmt:

```csharp
[assembly: Dependency(typeof(FileHelper))]
namespace Todo.Droid
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
        string path = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
        return Path.Combine(path, filename);
    }
  }
}
```

<a name="PCL_UWP" />

### <a name="windows-10-universal-windows-platform-uwp"></a>Windows 10 Universelle Windows-Plattform (UWP)

Implementieren der `IFileHelper` -Schnittstelle mit der plattformspezifischen `Windows.Storage` -API, um den Pfad der Datendatei zu bestimmen:

```csharp
using Windows.Storage;
...

[assembly: Dependency(typeof(FileHelper))]
namespace Todo.UWP
{
  public class FileHelper : IFileHelper
  {
    public string GetLocalFilePath(string filename)
    {
      return Path.Combine(ApplicationData.Current.LocalFolder.Path, filename);
    }
  }
}
```

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code.

Dieser Artikel konzentriert sich auf **Zugriff auf** einer SQLite-Datenbank, indem Sie xamarin.Forms verwenden. Weitere Informationen zum Arbeiten mit SQLite.Net selbst finden Sie in der [SQLite.NET unter Android](~/android/data-cloud/data-access/using-sqlite-orm.md) oder [SQLite.NET auf iOS](~/ios/data-cloud/data/using-sqlite-orm.md) Dokumentation. Die meisten SQLite.Net-Code ist über alle Plattformen hinweg freigegeben werden. Konfigurieren nur den Speicherort der Datenbankdatei SQLite erfordert Clientplattform-spezifische Funktionen.

## <a name="related-links"></a>Verwandte Links

- [TODO-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Datenbank-Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
