---
title: Azure Blob Storage のアクセス層 - ホット、クール、およびアーカイブ
description: Azure Blob Storage のホット、クール、およびアーカイブ アクセス層について説明します。 階層制御がサポートされるストレージ アカウントを確認します。
author: mhopkins-msft
ms.author: mhopkins
ms.date: 01/11/2021
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: klaasl
ms.openlocfilehash: 570fd7f356969bbd37ef6b661334501a062c36df
ms.sourcegitcommit: 16887168729120399e6ffb6f53a92fde17889451
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/13/2021
ms.locfileid: "98165690"
---
# <a name="access-tiers-for-azure-blob-storage---hot-cool-and-archive"></a>Azure Blob Storage のアクセス層 - ホット、クール、およびアーカイブ

Azure Storage にはさまざまなアクセス層が用意されており、最もコスト効果の高い方法で BLOB オブジェクト データを格納できます。 利用できるアクセス層:

- **ホット** - 頻繁にアクセスされるデータの格納に最適化されています。
- **クール** - アクセスされる頻度は低いものの、少なくとも 30 日以上保管されるデータの格納に最適化されています。
- **アーカイブ** - ほとんどアクセスされず、少なくとも 180 日以上保管され、待ち時間の要件が柔軟 (数時間単位) であるデータの格納に最適化されています。

次の考慮事項は、さまざまなアクセス層に当てはまります。

- アクセス層は、アップロード中またはアップロード後に、BLOB に設定できます。
- アカウント レベルで設定できるのはホット アクセス層とクール アクセス層だけです。 アーカイブ アクセス層は、BLOB レベルでのみ設定できます。
- クール アクセス層のデータは、可用性が若干低くなりますが、ホット データと同程度の高い持続性、取得待ち時間、およびスループット特性があります。 クール データの場合、ホット データと比較して可用性が若干低くアクセス コストが高めであっても、全体的なストレージ コストが低ければ許容できます。 詳細については、「[Storage の SLA](https://azure.microsoft.com/support/legal/sla/storage/v1_5/)」を参照してください。
- アーカイブ アクセス層のデータはオフラインで格納されます。 アーカイブ層は、ストレージ コストが最も低くなりますが、アクセス コストと待ち時間が最も大きくなります。
- ホットとクール層では、すべての冗長性オプションがサポートされています。 アーカイブ層では、LRS、GRS、RA-GRS のみがサポートされます。
- データ ストレージの上限は、アクセス層ごとに設定されるのではなく、アカウント レベルで設定されます。 すべての制限を 1 つの層で使用するか、3 つすべての層で使用するかは、ユーザーが選択できます。

クラウド内の格納データが急激に増加しています。 ストレージのニーズが拡大する中でコストを管理するには、アクセスの頻度や予定保有期間などの属性に基づいてデータを整理し、コストを最適化する方法が効果的です。 クラウドに格納されるデータは、有効期間を通じてどのように生成、処理、アクセスされるかに基づいて異なる場合があります。 有効期間を通じて活発にアクセスおよび変更されるデータもあれば、 有効期間の初期に頻繁にアクセスされ、古くなるにつれて大幅にアクセスが減るデータもあります。 また、クラウド内でアイドル状態のままとなり、格納されてからはほとんどアクセスされないデータもあります。

各データ アクセス シナリオは、特定のアクセス パターン用に最適化された異なるアクセス層の利点を利用しています。 Azure Blob Storage では、ホット、クール、アーカイブの各アクセス層によって、分化されたアクセス層のニーズに異なる価格モデルで対応しています。

次のツールとクライアント ライブラリはすべて、BLOB レベルの階層制御とアーカイブ ストレージをサポートしています。

- Azure portal
- PowerShell
- Azure CLI ツール
- .NET クライアント ライブラリ
- Java クライアント ライブラリ
- Python クライアント ライブラリ
- Node.js クライアント ライブラリ

[!INCLUDE [storage-multi-protocol-access-preview](../../../includes/storage-multi-protocol-access-preview.md)]

## <a name="storage-accounts-that-support-tiering"></a>階層制御をサポートするストレージ アカウント

ホット、クール、アーカイブの間でのオブジェクト ストレージ データの階層制御は、Blob Storage および General Purpose v2 (GPv2) アカウントでサポートされています。 General Purpose v1 (GPv1) アカウントでは階層制御はサポートされていません。 既存の GPv1 または Blob Storage アカウントを Azure portal を使用して GPv2 アカウントに簡単に変換できます。 GPv2 では、BLOB、ファイル、キューに対して新しい価格と機能が提供されます。 一部の機能と値引きは、GPv2 アカウントでのみ提供されています。 ワークロードによっては、GPv2 は GPv1 より高額になることがあります。 詳細については、「[Azure ストレージ アカウントの概要](../common/storage-account-overview.md)」を参照してください。

Blob Storage および GPv2 アカウントでは、アカウント レベルで **アクセス層** 属性が公開されます。 この属性では、オブジェクト レベルでアクセス層が明示的に設定されていない BLOB に対して、既定のアクセス層を指定できます。 層が明示的に設定されているオブジェクトについては、アカウント層は適用されません。 アーカイブ層は、オブジェクト レベルでのみ適用することができます。 アクセス層はいつでも切り替えることができます。

階層制御には、Blob Storage アカウントではなく GPv2 を使用します。 GPv2 は、Blob Storage アカウントで利用できるすべての機能をサポートしており、それ以外にもさらに多くのものを利用できます。 Blob Storage と GPv2 の料金体系はほぼ同じですが、いくつかの新機能と値下げの対象は GPv2 アカウントに限られています。

GPv1 アカウントと GPv2 アカウントとでは料金体系が異なります。GPv2 アカウントをご使用になる前に、両者を慎重に比較して評価してください。 既存の Blob Storage アカウントまたは GPv1 アカウントは、簡単なワンクリックのプロセスで GPv2 に変換することができます。 詳細については、「[Azure ストレージ アカウントの概要](../common/storage-account-overview.md)」を参照してください。

## <a name="hot-access-tier"></a>ホット アクセス層

ホット アクセス層は、クール層とアーカイブ層に比べてストレージ コストが高くなるものの、アクセス コストが最も低くなります。 ホット アクセス層の使用シナリオの例には、次のようなものがあります。

- 活発に使用されたり、頻繁に読み取りおよび書き込みがされると予想されるデータ
- 処理段階にあり、最終的にはクール アクセス層に移行されるデータ

## <a name="cool-access-tier"></a>クール アクセス層

クール アクセス層は、ホット ストレージに比べてストレージ コストが低くなり、アクセス コストが高くなります。 この層は、少なくとも 30 日以上クール層で保持されるデータを対象としています。 クール アクセス層の使用シナリオの例には、次のようなものがあります。

- 短期的なバックアップおよびディザスター リカバリー
- 頻繁には使用されないが、アクセスされたときにはすぐに利用できることが期待されている古いデータ
- コスト効率よく格納する必要があり、さらなるデータが今後処理するため収集されている、大規模なデータ セット

## <a name="archive-access-tier"></a>アーカイブ アクセス層

アーカイブ アクセス層はストレージ コストが最も低くなりますが、ホットとクールの各層に比べてデータ取得コストが高くなります。 データは、少なくとも 180 日間、アーカイブ層に保持される必要があります。そうでない場合、早期削除料金の対象になります。 アーカイブ アクセス層のデータの取得は、指定されたリハイドレート優先度によっては、数時間かかることがあります。 小さいオブジェクトの場合、優先度の高いリハイドレートは 1 時間未満でアーカイブからオブジェクトを取得できます。 「[アーカイブ層から BLOB データをリハイドレートする](storage-blob-rehydration.md)」で詳細を確認してください。

BLOB がアーカイブ ストレージ内にある間、BLOB データはオフラインであり、読み取りも変更もできません。 アーカイブ内の BLOB を読み取るかダウンロードするには、最初にそれをオンライン層にリハイドレートする必要があります。 アーカイブ ストレージ内の BLOB のスナップショットを作成することはできません。 ただし、BLOB メタデータはオンラインのままで使用でき、BLOB、そのプロパティ、メタデータ、および BLOB インデックス タグを一覧表示できます。 アーカイブ中の BLOB メタデータの設定または変更は許可されていません。 ただし、BLOB インデックス タグを設定したり変更したりすることはできます。 アーカイブ中の BLOB の場合、有効な操作は [Get Blob Properties](/rest/api/storageservices/get-blob-properties)、[Get Blob Metadata](/rest/api/storageservices/get-blob-metadata)、[Set Blob Tags](/rest/api/storageservices/set-blob-tags)、[Get Blob Tags](/rest/api/storageservices/get-blob-tags)、[Find Blobs by Tags](/rest/api/storageservices/find-blobs-by-tags)、[List Blobs](/rest/api/storageservices/list-blobs)、[Set Blob Tier](/rest/api/storageservices/set-blob-tier)、[Copy Blob](/rest/api/storageservices/copy-blob)、[Delete Blob](/rest/api/storageservices/delete-blob) のみです。

アーカイブ アクセス層の使用シナリオの例には、次のようなものがあります。

- 長期バックアップ、セカンダリ バックアップ、アーカイブ データセット
- 最終的に使用可能な形式に処理した後も保持する必要がある、元の (未加工の) データ
- 長期間格納しておく必要があり、ほとんどアクセスされることがない、コンプライアンスおよびアーカイブ データ

> [!NOTE]
> アーカイブ層は、ZRS、GZRS、RA-GZRS アカウントではサポートされていません。

## <a name="account-level-tiering"></a>アカウント レベルの階層制御

この 3 つのアクセス階層に保存されているすべての BLOB は、同一アカウントに共存させることができます。 層が明示的に割り当てられていない BLOB では、アカウントのアクセス層の設定から層が推定されます。 アクセス層がアカウントから取得されている場合、BLOB の **推定されたアクセス層** プロパティは "true" に設定され、**アクセス層** プロパティはアカウントの階層と一致します。 Azure portal では、_アクセス層の推定_ プロパティが、**ホット (推定)** や **クール (推定)** として BLOB アクセス層と共に表示されます。

アカウント アクセス層の変更は、アカウントに格納されている、層が明示的に設定されていないすべての "_推定されたアクセス層_" オブジェクトに適用されます。 アカウントの階層をホットからクールに切り替えた場合、層が設定されていない全 BLOB に関して書き込み操作 (10,000 件単位) の料金が適用されます。この料金が適用されるのは GPv2 アカウントのみです。 Blob Storage アカウントでは、この変更に関しての料金は発生しません。 Blob Storage アカウントまたは GPv2 アカウントでクールからホットに切り替えた場合は、読み取り操作 (10,000 件単位) とデータ取得 (GB 単位) の両方の料金が発生します。

既定のアカウント アクセス層として設定できるのは、ホット アクセス層とクール アクセス層だけです。 アーカイブは、オブジェクト レベルでのみ設定することができます。 BLOB のアップロードで、選択したアクセス層を、既定のアカウント層に関係なく、ホット、クール、またはアーカイブにするよう指定できます。 この機能により、データを直接アーカイブ層に書き込み、BLOB ストレージでデータを作成した瞬間からコストを削減することができます。

## <a name="blob-level-tiering"></a>BLOB レベルの階層制御

BLOB レベルの階層制御では、[Put Blob](/rest/api/storageservices/put-blob) または [Put Block List](/rest/api/storageservices/put-block-list) 操作を使用して、選択したアクセス層にデータをアップロードし、[Set Blob Tier](/rest/api/storageservices/set-blob-tier) 操作または[ライフサイクル管理](#blob-lifecycle-management)機能を使用して、オブジェクト レベルでデータの層を変更できます。 必要なアクセス層にデータをアップロードし、その後、使用パターンの変化に応じて、アカウント間でデータを移動することなく、BLOB のアクセス層をホット、クール、アーカイブに簡単に変更することができます。 すべての階層変更要求はすぐに発生し、ホットとクール間の層の変更は瞬時に行われます。 アーカイブ層からの BLOB のリハイドレートには、数時間かかる場合があります。

BLOB 層が最後に変更された時間は、BLOB の **アクセス層変更時間** プロパティを介して公開されます。 ホット層またはクール層の BLOB を上書きするとき、作成時に新しい BLOB アクセス層を明示的に設定しない限り、新しく作成された BLOB では上書きされた BLOB の階層を引き継ぎます。 BLOB がアーカイブ層にあると、上書きできないため、このシナリオでは、同じ BLOB をアップロードすることは許可されません。

> [!NOTE]
> アーカイブ ストレージと BLOB レベルの階層制御では、ブロック BLOB のみがサポートされます。

### <a name="blob-lifecycle-management"></a>BLOB のライフサイクル管理

Blob Storage のライフサイクル管理には優れたルールベースのポリシーが用意されており、これを使用して、最適なアクセス層にデータを移行し、ライフサイクルの最後にデータを期限切れにすることができます。 詳細については、「[Azure Blob Storage アクセス層の自動化によるコストの最適化](storage-lifecycle-management-concepts.md)」を参照してください。

> [!NOTE]
> ブロック BLOB ストレージ アカウント (Premium パフォーマンス) に格納されているデータは、現在、[BLOB 層の設定](/rest/api/storageservices/set-blob-tier)または Azure Blob Storage のライフサイクル管理を使用して、ホット、クールまたはアーカイブに階層化することはできません。
> データを移動するには、[Put Block From URL API](/rest/api/storageservices/put-block-from-url) を使用するか、この API をサポートするバージョンの AzCopy を使用して BLOB をブロック BLOB ストレージ アカウントから別のアカウントのホット アクセス層に同期的にコピーする必要があります。
> **Put Block From URL** API は、サーバー上でデータを同期的にコピーします。つまり、呼び出しは、すべてのデータが元のサーバーの場所から移動先の場所に移動された場合にのみ完了します。

### <a name="blob-level-tiering-billing"></a>BLOB レベルの階層制御の課金

BLOB がアップロードされたり階層間で移動されたりすると、アップロードまたは階層変更の直後に、対応する料金が請求されます。

BLOB をよりクールな層 (ホットからクール、ホットからアーカイブ、またはクールからアーカイブ) に移動するとき、この操作は移動先の層への書き込み操作として課金され、移動先の層の書き込み操作 (10,000 件単位) およびデータ書き込み (GB 単位) の料金が適用されます。

BLOB をよりホットな層 (アーカイブからクール、アーカイブからホット、またはクールからホット) に移動するとき、この操作は移動元の層からの読み取りとして課金され、移動元の層の読み取り操作 (10,000 件単位) およびデータ取得 (GB 単位) の料金が適用されます。 クール層またはアーカイブ層から移動された BLOB については、[早期削除](#cool-and-archive-early-deletion)料金も適用される場合があります。 [アーカイブ層からのデータのリハイドレート](storage-blob-rehydration.md)には時間がかかり、データがオンラインに復元され、BLOB 層がホットまたはクールに変更されるまで、データにはアーカイブ料金が課金されます。

次の表には、各層の変更料金をまとめてあります。

| | **書き込み料金 (操作 + アクセス)** | **読み取り料金 (操作 + アクセス)** |
| ---- | ----- | ----- |
| **Set Blob Tier** | ホット -> クール<br> ホット -> アーカイブ<br> クール -> アーカイブ | アーカイブ -> クール<br> アーカイブ -> ホット<br> クール -> ホット

### <a name="cool-and-archive-early-deletion"></a>クールおよびアーカイブの早期削除

クール層 (GPv2 アカウントのみ) に移動された BLOB には、30 日のクール早期削除期間が適用されます。 アーカイブ層に移動された BLOB には、180 日のアーカイブ早期削除期間が適用されます。 この料金は日割り計算されます。 たとえば、BLOB をアーカイブに移動し、45 日後に削除するかホット層に移動した場合、その BLOB のアーカイブへの保存について、135 (180 - 45) 日分に相当する早期削除料金が適用されます。

クールとアーカイブの層の間を移動する場合、いくつかの詳細があります。

1. BLOB がストレージ アカウントの既定のアクセス層に基づいてクールと推定され、BLOB がアーカイブに移動された場合、早期削除料金は発生しません。
1. BLOB が明示的にクール層に移動された後にアーカイブに移動された場合は、早期削除料金が適用されます。

アクセス層の変更がなかった場合は、**Last-Modified** BLOB プロパティを使用して、早期削除時間を計算します。 それ以外の場合は、BLOB プロパティ **access-tier-change-time** を表示してアクセス層が最後にクールまたはアーカイブに変更されたときを確認します。 BLOB プロパティの詳細については、「[Get Blob Properties](/rest/api/storageservices/get-blob-properties)」を参照してください。

## <a name="comparing-block-blob-storage-options"></a>ブロック BLOB ストレージ オプションの比較

次の表は、Premium パフォーマンス ブロック BLOB ストレージ、ホット アクセス層、クール アクセス層、アーカイブ アクセス層を比較したものです。

|                                           | **Premium パフォーマンス**   | **ホット層** | **クール層**       | **アーカイブ層**  |
| ----------------------------------------- | ------------------------- | ------------ | ------------------- | ----------------- |
| **可用性**                          | 99.9%                     | 99.9%        | 99%                 | オフライン           |
| **可用性** <br> **(RA-GRS 読み取り)**  | 該当なし                       | 99.99%       | 99.9%               | オフライン           |
| **利用料金**                         | より高いストレージ コスト、より低いアクセスおよびトランザクション コスト | より高いストレージ コスト、より低いアクセスおよびトランザクション コスト | より低いストレージ コスト、より高いアクセスおよびトランザクション コスト | 最も低いストレージ コスト、最も高いアクセスおよびトランザクション コスト |
| **最小ストレージ存続期間**              | 該当なし                       | 該当なし          | 30 日<sup>1</sup> | 180 日
| **待機時間** <br> **(1 バイト目にかかる時間)** | 1 桁ミリ秒 | ミリ秒 | ミリ秒        | 数時間<sup>2</sup> |

<sup>1</sup> GPv2 アカウントのクール層のオブジェクトには、30 日の最小リテンション期間があります。 Blob Storage アカウントには、クール層の最小リテンション期間はありません。

<sup>2</sup> Archive Storage では、取得の待ち時間とコストが異なる High と Standard の 2 つのリハイドレート優先度が現在サポートされています。 詳細については、「[アーカイブ層から BLOB データをリハイドレートする](storage-blob-rehydration.md)」を参照してください。

> [!NOTE]
> Blob Storage アカウントでは、汎用 v2 ストレージ アカウントと同じパフォーマンスとスケーラビリティ ターゲットがサポートされます。 詳細については、「[Blob Storage のスケーラビリティとパフォーマンスのターゲット](scalability-targets.md)」を参照してください。

## <a name="pricing-and-billing"></a>価格と課金

すべてのストレージ アカウントでは、各 BLOB の層に基づいたブロック BLOB ストレージの価格モデルを採用しています。 次の課金に関する考慮事項に留意してください。

- **ストレージ コスト**:データの格納のコストは、格納されているデータの量だけでなく、アクセス層にも左右されます。 よりクールな層になるほど、ギガバイトあたりのコストが下がります。
- **データ アクセス コスト**:データ アクセス料金は、よりクールな層になるほど高くなります。 クール アクセス層およびアーカイブ アクセス層のデータの場合、読み取りに対して、ギガバイト単位のデータ アクセス料金が課金されます。
- **トランザクション コスト**:すべての層に対してトランザクションごとに課金されます。よりクールな層になるほどコストが高くなります。
- **geo レプリケーション データ転送コスト**:GRS と RA-GRS を含む geo レプリケーションが構成されているアカウントだけに適用されます。 geo レプリケーション データ転送には、ギガバイトあたりの料金がかかります。
- **送信データ転送コスト**:送信データ転送 (Azure リージョン外に転送されるデータ) では、帯域幅使用量に対する課金が 1 ギガバイトごとに発生します。これは、汎用ストレージ アカウントと同じです。
- **アクセス層の変更**:アカウント アクセス層を変更すると、層が明示的に設定されていないすべての BLOB に階層変更料金が発生します。 1 つの BLOB のアクセス層の変更については、「[BLOB レベルの階層制御の課金](#blob-level-tiering-billing)」を参照してください。

    バージョン管理が有効になっている場合、または BLOB にスナップショットがある場合に BLOB のアクセス層を変更すると、追加料金が発生することがあります。 バージョン管理が有効になっている BLOB については、BLOB のバージョン管理に関するドキュメントの「[価格と課金](versioning-overview.md#pricing-and-billing)」を参照してください。 スナップショットを含む BLOB については、BLOB のスナップショットに関するドキュメントの「[価格と課金](snapshots-overview.md#pricing-and-billing)」を参照してください。

> [!NOTE]
> ブロック BLOB の価格について詳しくは、「[ブロック BLOBの価格](https://azure.microsoft.com/pricing/details/storage/blobs/)」を参照してください。 送信データ転送料金について詳しくは、「[帯域幅の料金詳細](https://azure.microsoft.com/pricing/details/bandwidth/)」ページを参照してください。

## <a name="availability"></a>可用性

さまざまなアクセス層を、BLOB レベルの階層制御と共に、任意のリージョンでご利用いただけます。 詳細なリストについては、[Azure のリージョン別の利用可能な製品](https://azure.microsoft.com/global-infrastructure/services/?products=storage)に関するページを参照してください。

## <a name="next-steps"></a>次の手順

BLOB とアカウントをアクセス層間で管理する方法について説明します。

- [Azure Storage アカウントの BLOB の層を管理する方法](manage-access-tier.md)
- [Azure Storage アカウントの既定のアカウント アクセス層を管理する方法](../common/manage-account-default-access-tier.md)
- [Azure Blob Storage アクセス層の自動化によるコストの最適化](storage-lifecycle-management-concepts.md)
