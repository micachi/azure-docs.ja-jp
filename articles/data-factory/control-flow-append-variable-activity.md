---
title: Azure Data Factory の変数追加アクティビティ
description: 変数追加アクティビティを設定して、Data Factory パイプラインで定義されている既存の配列変数に値を追加する方法について説明します
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: maghan
ms.date: 10/09/2018
ms.openlocfilehash: 16bdd1d31440ed440faf67e939485da613e3886f
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2020
ms.locfileid: "96490943"
---
# <a name="append-variable-activity-in-azure-data-factory"></a>Azure Data Factory の変数追加アクティビティ
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]
変数追加アクティビティを使用して、Data Factory パイプラインで定義されている既存の配列変数に値を追加する方法について説明します。

## <a name="type-properties"></a>型のプロパティ

プロパティ | 説明 | 必須
-------- | ----------- | --------
name | パイプラインのアクティビティの名前 | はい
description | アクティビティの動作を説明するテキスト | いいえ
type | アクティビティの種類は AppendVariable です | はい
value | 指定した変数に追加するために使用する文字列リテラルまたは式のオブジェクト値 | はい
variableName | アクティビティによって変更される変数の名前。変数は "Array" 型でなければなりません | はい

## <a name="next-steps"></a>次のステップ
Data Factory でサポートされている関連制御フロー アクティビティについては、以下を参照してください。 

- [変数アクティビティの設定](control-flow-set-variable-activity.md)
