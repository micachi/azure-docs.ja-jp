---
title: Azure Functions のトリガーとバインド
description: トリガーとバインドを使用して、Azure 関数をオンライン イベントおよびクラウドベース サービスに接続する方法を学習します。
author: craigshoemaker
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: aa0d78d52ec13c91b82e6a8d10720269076f59a1
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353546"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure Functions でのトリガーとバインドの概念

この記事では、関数のトリガーとバインドに関する概念の概要について学習します。

トリガーは、関数が実行される原因です。 トリガーで関数の呼び出し方法が定義されます。1 つの関数には正確に 1 つのトリガーを含める必要があります。 トリガーにはデータが関連付けられていて、多くの場合、関数のペイロードとして提供されます。 

関数へのバインドは、関数に別のリソースを宣言的に接続する方法です。バインドは *入力バインド* または *出力バインド*、あるいは両方として接続される場合があります。 バインドからのデータは、パラメーターとして関数に提供されます。

ニーズに合わせて、さまざまなバインドを組み合わせることができます。 バインドは省略可能であり、関数には 1 つまたは複数の入力または出力バインドがある場合があります。

トリガーとバインドによって、他のサービスへのアクセスのハードコーディングを回避することができます。 関数は、関数パラメーターでデータ (キュー メッセージの内容など) を受信します。 関数の戻り値を使用して、(たとえば、キュー メッセージを作成するために) データを送信します。 

さまざまな関数の実装方法の以下の例について考えてみます。

| サンプル シナリオ | トリガー | 入力バインド | 出力バインド |
|-------------|---------|---------------|----------------|
| 新しいキュー メッセージが到着し、別のキューに書き込むための関数が実行されます。 | キュー<sup>*</sup> | *なし* | キュー<sup>*</sup> |
|スケジュールされたジョブで Blob Storage のコンテンツが読み取られ、新しい Cosmos DB ドキュメントが作成されます。 | Timer | Blob Storage | Cosmos DB |
|Event Grid は、Blob Storage からイメージを読み取り、Cosmos DB からドキュメントを読み取って、メールを送信するために使用されます。 | Event Grid | Blob Storage と Cosmos DB | SendGrid |
| Microsoft Graph を使用して Excel シートを更新する webhook。 | HTTP | *なし* | Microsoft Graph |

<sup>\*</sup> さまざまなキューを表します

これらの例には、すべて網羅されているわけではなく、トリガーとバインドを一緒に使用できる方法を示すために提供されています。

###  <a name="trigger-and-binding-definitions"></a>トリガーとバインドの定義

トリガーとバインドは、開発方法に応じて異なる方法で定義されます。

| プラットフォーム | トリガーとバインドは、以下を行うことで構成されます |
|-------------|--------------------------------------------|
| C# クラス ライブラリ | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C# 属性でのメソッドとパラメーターの修飾 |
| 他のすべて (Azure portal を含む) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[function.json](./functions-reference.md) ([スキーマ](http://json.schemastore.org/function)) の更新 |

ポータルではこの構成のために UI が提供されますが、関数の **[統合]** タブで使用できる **詳細エディター** を開いて直接ファイルを編集できます。

.NET では、パラメーター型で入力データのデータ型が定義されます。 たとえば、キュー トリガーのテキストにバインドするには `string` を、バイナリとして読み取るにはバイト配列を、オブジェクトを逆シリアル化するにはカスタム型を使用します。

JavaScript などの動的に型指定される言語の場合は、*function.json* ファイルの `dataType` プロパティを使用します。 たとえば、バイナリ形式で HTTP 要求のコンテンツを読み取るには、`dataType` を `binary` に設定します。

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

`dataType` のその他のオプションは、`stream` と `string` です。

## <a name="binding-direction"></a>バインドの方向

すべてのトリガーとバインドには、[function.json](./functions-reference.md) ファイルに `direction` プロパティがあります。

- トリガーの場合、方向は常に `in` です
- 入出力バインドは `in` と `out` を使用します
- 一部のバインドは、特殊な方向の `inout` をサポートしてします。 `inout` を使用する場合、ポータルの **[統合]** タブで使用できるのは **詳細エディター** のみです。

[クラス ライブラリの属性](functions-dotnet-class-library.md)を使用してトリガーとバインドを構成した場合、その方向は属性コンストラクターで提供されるか、またはパラメーター型から推論されます。

## <a name="add-bindings-to-a-function"></a>関数にバインドを追加する

入力バインドまたは出力バインドを使用して、関数を他のサービスに接続できます。 関数に特定の定義を追加することで、バインドを追加します。 詳細については、「[Azure Functions の既存の関数にバインドを追加する](add-bindings-existing-function.md)」を参照してください。  

## <a name="supported-bindings"></a>サポートされるバインディング

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

どのバインディングがプレビューでどのバインディングが実稼働環境で承認されているかについては、[サポートされている言語](supported-languages.md)に関する記事をご覧ください。

## <a name="bindings-code-examples"></a>バインドのコードの例

次の表を使用して、関数でバインドを使用する方法を示す特定のバインドの種類の例を参照してください。 まず、実際のプロジェクトに対応する言語のタブを選択します。 

[!INCLUDE [functions-bindings-code-example-chooser](../../includes/functions-bindings-code-example-chooser.md)]

## <a name="custom-bindings"></a>カスタム バインド

カスタムの入力および出力バインドを作成できます。 バインドは .NET で作成する必要がありますが、サポートされている任意の言語から使用できます。 カスタム バインドの作成の詳細については、「[Creating custom input and output bindings (カスタムの入力および出力バインドの作成)](https://github.com/Azure/azure-webjobs-sdk/wiki/Creating-custom-input-and-output-bindings)」を参照してください。

## <a name="resources"></a>リソース
- [バインド式とパターン](./functions-bindings-expressions-patterns.md)
- [Azure 関数の戻り値の使用](./functions-bindings-return-value.md)
- [バインド式を登録する方法](./functions-bindings-register.md)
- テスト:
  - [Azure Functions のコードをテストするための戦略](functions-test-a-function.md)
  - [HTTP によってトリガーされない関数を手動で実行する](functions-manually-run-non-http.md)
- [バインド エラーの処理](./functions-bindings-errors.md)

## <a name="next-steps"></a>次のステップ
> [!div class="nextstepaction"]
> [Azure Functions バインド拡張機能を登録する](./functions-bindings-register.md)
