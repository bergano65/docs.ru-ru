---
title: WCF и международные доменные имена
ms.date: 03/30/2017
ms.assetid: c8a3e10a-8bc2-4a78-8d86-a562ba6e65fa
ms.openlocfilehash: 1db62f3e7d073fd1bf9bf9d4d0e17703310f2e69
ms.sourcegitcommit: 37616676fde89153f563a485fc6159fc57326fc2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2019
ms.locfileid: "69988590"
---
# <a name="wcf-and-internationalized-domain-names"></a>WCF и международные доменные имена
Добавлена поддержка служб WCF с интернационализированными именами домена (IDN). Интернационализированное имя домена представляет собой имя домена, содержащее символы, не входящие в набор символов ASCII. Данная поддержка включает в себя как возможность размещения службы WCF с именем IDN, так и возможность диалога клиента WCF с веб-службой с именем IDN.  
  
## <a name="systemuri-and-idn"></a>System.Uri и IDN  
 У объекта класса <xref:System.Uri> есть два свойства: <xref:System.Uri.Host%2A> и <xref:System.Uri.DnsSafeHost%2A>. Эти свойства содержат значения Unicode или Punycode в зависимости от параметров конфигурации IDN.  
  
 IDN активируется в файле конфигурации приложения с помощью следующего кода XML  
  
```xml  
<configuration>  
  <uri>  
    <idn enabled="All/AllExceptIntranet/None" />  
  </uri>  
</configuration>  
```  
  
 Элемент \<> IDN содержит атрибут Enabled, для которого можно задать одно из следующих значений:  
  
1. None  
  
2. "Аллексцептинтранет"  
  
3. Каждого  
  
 Если для параметра IDN задано значение "нет", никакие преобразования с помощью URI. Host или URI. Днссафехост не выполняются. Если для параметра IDN задано значение "ALL", URI. Узел остается в Юникоде и URI. Днссафехост преобразуется в Punycode. Если для параметра IDN задано значение "Аллексцептинтранет", URI. Днссафехост преобразуется в Punycode для адресов Интернета и остается в Юникоде для адресов интрасети. Этот параметр важен для верного разрешения имен DNS. Обратите внимание, что он не требует настройки в Windows 8 и более поздних версиях.  
  
> [!WARNING]
> Никогда не следует вводить адрес вручную с использованием Punycode. WCF преобразует адрес в соответствии с примененными параметрами конфигурации.  
  
> [!WARNING]
> При добавлении в applicationHost.exe.config символов Юникода сохраните файл в кодировке UTF-8.  
  
## <a name="see-also"></a>См. также

- <xref:System.Uri?displayProperty=nameWithType>
