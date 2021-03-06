---
author: v-demjoh
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/15/2020
ms.author: v-demjoh
ms.openlocfilehash: 87af99d5136a8bed93854e9396895cddeb22549a
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540479"
---
## <a name="download-and-install"></a>ダウンロードしてインストールする

#### <a name="windows-install"></a>[Windows のインストール](#tab/windowsinstall)

Windows に Speech CLI をインストールするには、次の手順に従います。

1. Windows では、お使いのプラットフォームに対応した [Microsoft Visual Studio 2019 の Visual C++ 再頒布可能パッケージ](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)が必要です。 これを初めてインストールする場合、再起動が必要になる場合があります。
2. Speech CLI [ZIP アーカイブ](https://aka.ms/speech/spx-zips.zip)をダウンロードして、抽出します。
3. `spx-zips` を抽出したディレクトリに移動します。 このフォルダーには、さまざまなプラットフォームの Speech CLI 用のプログラム ファイルが含まれています。 
4. ご利用のプラットフォーム用のファイルを抽出します (.NET Framework 4.7 の場合は `spx-net471`、x64 CPU での .NET Core 3.0 の場合は `spx-netcore-win-x64`)。 このディレクトリから `spx` を実行することにご注意ください。

### <a name="run-the-speech-cli"></a>Speech CLI を実行する

1. コマンド プロンプトまたは PowerShell を開き、Speech CLI を抽出したディレクトリに移動します。  
2. 「`spx`」と入力して、Speech CLI のヘルプ コマンドを表示します。

> [!NOTE]
> Powershell では、コマンドの検索時にローカル ディレクトリがチェックされません。 Powershell で、ディレクトリを `spx` の場所に変更し、`.\spx` と入力してツールを呼び出します。
> このディレクトリをパスに追加すると、Powershell と Windows コマンド プロンプトが `.\` プレフィックスを含めずにディレクトリから `spx` を検索します。

### <a name="font-limitations"></a>フォントの制限事項

Windows の Speech CLI では、ローカル コンピューター上のコマンド プロンプトで使用できるフォントのみを表示できます。
[Windows ターミナル](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701)では、Speech CLI によって対話的に生成されるすべてのフォントがサポートされます。

ファイルに出力すると、メモ帳などのテキスト エディターや、Microsoft Edge などの Web ブラウザーでも、すべてのフォントを表示できます。

#### <a name="linux-install"></a>[Linux のインストール](#tab/linuxinstall)

Linux の x64 CPU で Speech CLI をインストールするには、次の手順に従います。

1. [.NET Core 3.0](https://dotnet.microsoft.com/download/dotnet-core/3.0) をインストールします。
2. Speech CLI [ZIP アーカイブ](https://aka.ms/speech/spx-zips.zip)をダウンロードして、抽出します。
3. ダウンロードから抽出したルートディレクトリ `spx-zips` にアクセスし、`spx-netcore-30-linux-x64` を新しい `~/spx` ディレクトリに抽出します。
4. ターミナルで、次のコマンドを入力します。
   1. `cd ~/spx`
   2. `sudo chmod +r+x spx`
   3. `PATH=~/spx:$PATH`

「`spx`」と入力して、Speech CLI のヘルプを表示します。

#### <a name="docker-install-windows-linux-macos"></a>[Docker のインストール (Windows、Linux、macOS)](#tab/dockerinstall)

Docker コンテナー内に Speech CLI をインストールするには、次の手順に従います。

1. プラットフォームにインストールされていない場合、<a href="https://www.docker.com/get-started" target="_blank">Docker Desktop をインストール<span class="docon docon-navigate-external x-hidden-focus"></span></a>します。
2. 新しいコマンド プロンプトまたはターミナルで、次のコマンドを入力します: 
   ```console   
   docker pull msftspeech/spx
   ```
3. 次のコマンドを入力します。 Speech CLI のヘルプ情報が表示されます: 
   ```console 
   docker run -it --rm msftspeech/spx help
   ```

### <a name="mount-a-directory-in-the-container"></a>コンテナーにディレクトリをマウントする

Speech CLI ツールは、構成設定をファイルとして保存し、ヘルプ コマンドを除く任意のコマンドを実行するときに、これらのファイルを読み込みます。
Docker コンテナー内で Speech CLI を使用する際は、コンテナーからローカル ディレクトリをマウントする必要があります。これにより、ツールで構成設定を格納したり検索したりできるようになります。また、音声のオーディオ ファイルなど、コマンドに必要なファイルの読み取りまたは書き込みを行うことができるようになります。

Windows の場合は、次のコマンドを入力して、Speech CLI がコンテナー内から使用できるローカル ディレクトリを作成します。

`mkdir c:\spx-data`

Linux または macOS の場合は、ターミナルでこのコマンドを入力してディレクトリを作成し、その絶対パスを確認します。

```bash
mkdir ~/spx-data
cd ~/spx-data
pwd
```

Speech CLI を呼び出すときは、この絶対パスを使用します。

### <a name="run-speech-cli-in-the-container"></a>コンテナーで Speech CLI を実行する

このドキュメントでは、Docker 以外のインストールで使用される Speech CLI `spx` コマンドについて説明します。
Docker コンテナーで `spx` コマンドを呼び出す場合は、Speech CLI が構成値を格納および検索してファイルを読み書きできるように、コンテナー内のディレクトリをファイル システムにマウントする必要があります。

Windows の場合、コマンドは次のように始まります。

```console
docker run -it -v c:\spx-data:/data --rm msftspeech/spx
```

Linux または macOS の場合、コマンドは以下の例のようになります。 `ABSOLUTE_PATH` をマウントされたディレクトリの絶対パスに置き換えます。 このパスは、前のセクションの `pwd` コマンドによって返されました。 

キーとリージョンを設定する前にこのコマンドを実行すると、キーとリージョンを設定するよう求めるエラーが表示されます。
```console   
sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
```

コンテナーにインストールされている `spx` コマンドを使用するには、必ず上記の完全なコマンドを入力し、その後に要求のパラメーターを入力します。
たとえば、Windows では、次のコマンドによってキーが設定されます。

```console
docker run -it -v c:\spx-data:/data --rm msftspeech/spx config @key --set SUBSCRIPTION-KEY
```

コマンド ライン ツールとのより拡張された操作については、entrypoint パラメーターを追加することで、対話型の bash シェルでコンテナーを開始できます。
Windows では、このコマンドを入力して、対話型のコマンド ライン インターフェイスを公開するコンテナーを起動します。ここでは、複数の `spx` コマンドを入力できます。
```console
docker run -it --entrypoint=/bin/bash -v c:\spx-data:/data --rm msftspeech/spx
```

> [!WARNING]
> Docker コンテナー内で Speech CLI を実行するときは、コンピューターのマイクを使用することはできません。 ただし、ローカルにマウントされたディレクトリ内のオーディオ ファイルを読み取って保存することができます。 

<!-- Need to troubleshoot issues with docker pull image

### Optional: Create a command line shortcut

If you're running the the Speech CLI from a Docker container on Linux or macOS you can create a shortcut. 

Follow these instructions to create a shortcut:
1. Open `.bash_profile` with your favorite text editor. For example:
   ```shell
   nano ~/.bash_profile
   ```
2. Next, add this function to your `.bash_profile`. Make sure you update this function with the correct path to your mounted directory:
   ```shell   
   spx(){
       sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx
   }
   ```
3. Source your profile:
   ```shell
   source ~/.bash_profile
   ```
4. Now instead of running `sudo docker run -it -v ABSOLUTE_PATH:/data --rm msftspeech/spx`, you can just type `spx` followed by arguments. For example: 
   ```shell
   // Get some help
   spx help recognize

   // Recognize speech from an audio file 
   spx recognize --file /mounted/directory/file.wav
   ```

> [!WARNING]
> If you change the mounted directory that Docker is referencing, you need to update the function in `.bash_profile`.
--->
***

## <a name="create-subscription-config"></a>サブスクリプション構成の作成

Speech CLI の使用を開始するには、Speech サブスクリプション キーとリージョン識別子を入力する必要があります。 「[Speech Service を無料で試す](../overview.md#try-the-speech-service-for-free)」の手順に従って、これらの資格情報を取得します。
サブスクリプション キーとリージョン識別子 (たとえば、 `eastus`、`westus`) を入手したら、次のコマンドを実行します。

```console
spx config @key --set SUBSCRIPTION-KEY
spx config @region --set REGION
```

これで、今後の SPX 要求のためのサブスクリプション認証が格納されるようになりました。 これらの格納されている値のいずれかを削除する必要がある場合は、`spx config @region --clear` または `spx config @key --clear` を実行します。
