---
title: Вставка изображения из файла
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 35900aa2-5615-4174-8212-ba184c6b82fb
ms.openlocfilehash: d47f5b7eaf6b5f6a3174982e6b4cf43859c031a5
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/07/2019
ms.locfileid: "70794148"
---
# <a name="inserting-an-image-from-a-file"></a>Вставка изображения из файла
Большой двоичный объект (BLOB) можно записывать в базу данных в виде двоичных или символьных данных в зависимости от типа поля источника данных. BLOB - это общий термин для типов данных `text`, `ntext` и `image`, используемых, как правило, для хранения документов и изображений.  
  
 Чтобы записать значение большого двоичного объекта в базу данных, выполните соответствующую инструкцию INSERT или UPDATE и передайте значение большого двоичного объекта в качестве входного параметра (см. раздел [Настройка параметров и типов данных параметров](../configuring-parameters-and-parameter-data-types.md)). Если объект BLOB хранится как текст, например поле SQL Server `text`, можно передать значение BLOB в виде строкового параметра. Если объект BLOB хранится в двоичном формате, например поле SQL Server `image`, в качестве двоичного параметра можно передать массив с типом данных `byte`.  
  
## <a name="example"></a>Пример  
 Следующий пример кода добавляет сведения о сотрудниках в таблицу Employees базы данных Northwind. Фотографии сотрудников считываются из файла и добавляются в поле Photo в таблице, имеющей тип данных image.  
  
```vb  
Public Shared Sub AddEmployee( _  
  lastName As String, _  
  firstName As String, _  
  title As String, _  
  hireDate As DateTime, _  
  reportsTo As Integer, _  
  photoFilePath As String, _  
  connectionString As String)  
  
  Dim photo() as Byte = GetPhoto(photoFilePath)  
  
  Using connection As SqlConnection = New SqlConnection( _  
    connectionString)  
  
  Dim command As SqlCommand = New SqlCommand( _  
    "INSERT INTO Employees (LastName, FirstName, Title, " & _  
    "HireDate, ReportsTo, Photo) " & _  
    "Values(@LastName, @FirstName, @Title, " & _  
    "@HireDate, @ReportsTo, @Photo)", connection)   
  
  command.Parameters.Add("@LastName",  _  
    SqlDbType.NVarChar, 20).Value = lastName  
  command.Parameters.Add("@FirstName", _  
    SqlDbType.NVarChar, 10).Value = firstName  
  command.Parameters.Add("@Title", _  
    SqlDbType.NVarChar, 30).Value = title  
  command.Parameters.Add("@HireDate", _  
    SqlDbType.DateTime).Value = hireDate  
  command.Parameters.Add("@ReportsTo", _  
    SqlDbType.Int).Value = reportsTo  
  
  command.Parameters.Add("@Photo", _  
    SqlDbType.Image, photo.Length).Value = photo  
  
  connection.Open()  
  command.ExecuteNonQuery()  
  
  End Using  
End Sub  
  
Public Shared Function GetPhoto(filePath As String) As Byte()  
  Dim stream As FileStream = new FileStream( _  
     filePath, FileMode.Open, FileAccess.Read)  
  Dim reader As BinaryReader = new BinaryReader(stream)  
  
  Dim photo() As Byte = reader.ReadBytes(stream.Length)  
  
  reader.Close()  
  stream.Close()  
  
  Return photo  
End Function  
```  
  
```csharp  
public static void AddEmployee(  
  string lastName,   
  string firstName,   
  string title,   
  DateTime hireDate,   
  int reportsTo,   
  string photoFilePath,   
  string connectionString)  
{  
  byte[] photo = GetPhoto(photoFilePath);  
  
  using (SqlConnection connection = new SqlConnection(  
    connectionString))  
  
  SqlCommand command = new SqlCommand(  
    "INSERT INTO Employees (LastName, FirstName, " +  
    "Title, HireDate, ReportsTo, Photo) " +  
    "Values(@LastName, @FirstName, @Title, " +  
    "@HireDate, @ReportsTo, @Photo)", connection);   
  
  command.Parameters.Add("@LastName",    
     SqlDbType.NVarChar, 20).Value = lastName;  
  command.Parameters.Add("@FirstName",   
      SqlDbType.NVarChar, 10).Value = firstName;  
  command.Parameters.Add("@Title",       
      SqlDbType.NVarChar, 30).Value = title;  
  command.Parameters.Add("@HireDate",   
       SqlDbType.DateTime).Value = hireDate;  
  command.Parameters.Add("@ReportsTo",   
      SqlDbType.Int).Value = reportsTo;  
  
  command.Parameters.Add("@Photo",  
      SqlDbType.Image, photo.Length).Value = photo;  
  
  connection.Open();  
  command.ExecuteNonQuery();  
  }  
}  
  
public static byte[] GetPhoto(string filePath)  
{  
  FileStream stream = new FileStream(  
      filePath, FileMode.Open, FileAccess.Read);  
  BinaryReader reader = new BinaryReader(stream);  
  
  byte[] photo = reader.ReadBytes((int)stream.Length);  
  
  reader.Close();  
  stream.Close();  
  
  return photo;  
}  
```  
  
## <a name="see-also"></a>См. также

- [Использование команд для изменения данных](../using-commands-to-modify-data.md)
- [Извлечение двоичных данных](../retrieving-binary-data.md)
- [Двоичные данные и данные большого объема SQL Server](sql-server-binary-and-large-value-data.md)
- [Сопоставления типов данных SQL Server](../sql-server-data-type-mappings.md)
- [Общие сведения об ADO.NET](../ado-net-overview.md)
