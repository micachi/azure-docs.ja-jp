---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 9f1c30b303bd1fe02e0685c7d848be92073ca2f6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016873"
---
#### <a name="to-add-a-storage-account-credential-in-the-same-azure-subscription-as-the-storsimple-device-manager-service"></a>StorSimple デバイス マネージャー サービスと同じ Azure サブスクリプションにストレージ アカウントの資格情報を追加するには、次の手順を実行します。

1. StorSimple デバイス マネージャー サービスに移動します。 **[構成]** セクションで **[ストレージ アカウントの資格情報]** をクリックします。

    ![ストレージ アカウントの資格情報に移動](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct1.png)

2. **[ストレージ アカウントの資格情報]** ブレードで、 **[+ 追加]** をクリックします。

    ![ストレージ アカウントの資格情報を追加する](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct2.png)

3. **[ストレージ アカウント資格情報の追加]** ブレードで、次の手順を実行します。

    1. お使いのサービスと同じ Azure サブスクリプションにストレージ アカウントの資格情報を追加するので、 **[現在]** が選択されていることを確認します。

    2. **[ストレージ アカウント]** ボックスの一覧で、既存のストレージ アカウントを選択します。

    3. 選択したストレージ アカウントに基づいて、 **[場所]** が表示されます (灰色表示になっており、変更することはできません)。

    4. **[SSL モードを有効にする]** を選択して、デバイスとクラウド間のネットワーク通信用のセキュリティで保護されたチャネルを作成します。 プライベート クラウド内で動作している場合にのみ、 **[SSL を有効にする]** を無効にします。

        ![[ストレージ アカウントの資格情報] ブレードの追加](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct3.png)

    5. **[追加]** をクリックすると、ストレージ アカウントの資格情報のジョブの作成が開始されます。 ストレージ アカウントの資格情報が正常に作成されると、その旨が通知が表示されます。

        ![ストレージ アカウントの資格情報が正常に作成された旨の通知](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct5.png)

新しく作成されたストレージ アカウントの資格情報が、 **[ストレージ アカウントの資格情報]** の一覧に表示されます。

![ストレージ アカウントの資格情報の一覧](./media/storsimple-8000-configure-new-storage-account-u2/createnewstorageacct6.png)

