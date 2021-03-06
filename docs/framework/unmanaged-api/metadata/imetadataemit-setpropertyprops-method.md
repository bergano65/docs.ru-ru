---
title: Метод IMetaDataEmit::SetPropertyProps
ms.date: 03/30/2017
api_name:
- IMetaDataEmit.SetPropertyProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataEmit::SetPropertyProps
helpviewer_keywords:
- SetPropertyProps method [.NET Framework metadata]
- IMetaDataEmit::SetPropertyProps method [.NET Framework metadata]
ms.assetid: e2501fc8-b2bc-4dcc-9205-e3acd5a53ffe
topic_type:
- apiref
ms.openlocfilehash: 0fdec87324d6efa0f911e37573093c19b93c0349
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2019
ms.locfileid: "74440549"
---
# <a name="imetadataemitsetpropertyprops-method"></a>Метод IMetaDataEmit::SetPropertyProps
Задает функции, хранимые в метаданных для свойства, определенного при предыдущем вызове [метода DefineProperty](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-defineproperty-method.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
HRESULT SetPropertyProps (   
    [in]  mdProperty      pr,   
    [in]  DWORD           dwPropFlags,   
    [in]  DWORD           dwCPlusTypeFlag,   
    [in]  void const      *pValue,   
    [in]  ULONG           cchValue,   
    [in]  mdMethodDef     mdSetter,   
    [in]  mdMethodDef     mdGetter,   
    [in]  mdMethodDef     rmdOtherMethods[]   
);  
```  
  
## <a name="parameters"></a>Параметры  
 `pr`  
 окне Токен изменяемого свойства  
  
 `dwPropFlags`  
 окне Флаги свойств.  
  
 `dwCPlusTypeFlag`  
 окне Тип значения свойства по умолчанию.  
  
 `pValue`  
 окне Значение по умолчанию для свойства.  
  
 `cchValue`  
 окне Число символов Юникода в `pValue`.  
  
 `mdSetter`  
 окне Метод, который задает значение свойства.  
  
 `mdGetter`  
 окне Метод, который получает значение свойства.  
  
 `rmdOtherMethods[]`  
 окне Массив других методов, связанных со свойством. Завершите этот массив маркером `mdTokenNil`.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** см. раздел [Требования к системе](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** COR. h  
  
 **Библиотека:** Используется в качестве ресурса в MSCorEE. dll  
  
 **Версии платформы .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также:

- [Интерфейс IMetaDataEmit](../../../../docs/framework/unmanaged-api/metadata/imetadataemit-interface.md)
- [Интерфейс IMetaDataEmit2](../../../../docs/framework/unmanaged-api/metadata/imetadataemit2-interface.md)
