---
title: アプリケーションのシングル サインオンを構成する
description: Azure AD で開発中または登録中のカスタム アプリケーションのシングル サインオンを構成する方法。
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: ryanwi
ROBOTS: NOINDEX
ms.openlocfilehash: 4665e4240d7563a585b7c224365172d69d7efddf
ms.sourcegitcommit: 5cdd0b378d6377b98af71ec8e886098a504f7c33
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/25/2021
ms.locfileid: "98753875"
---
# <a name="how-to-configure-single-sign-on-for-an-application"></a>アプリケーションのシングル サインオンを構成する方法

アプリのフェデレーション シングル サインオン (SSO) は、OpenID Connect、SAML 2.0、または WS-Fed を使用して Azure AD 経由でフェデレーションしている場合、自動的に有効になります。 Azure AD とのセッションが既に存在しているのにエンド ユーザーのサインインが必要な場合、アプリが正しく構成されていない可能性があります。

* ADAL/MSAL を使用している場合、**PromptBehavior** を、**Always** ではなく **Auto** にしてください。

* モバイル アプリを作成している場合、仲介型または仲介型でない SSO を有効にするために、追加の構成が必要になることがあります。

Android については、[Android でクロス アプリ SSO を有効にする方法](../azuread-dev/howto-v1-enable-sso-android.md)に関するページを参照してください。<br>

iOS については、[iOS でクロス アプリ SSO を有効にする方法](../azuread-dev/howto-v1-enable-sso-ios.md)に関するページを参照してください。

## <a name="next-steps"></a>次のステップ

[Azure AD SSO](../manage-apps/what-is-single-sign-on.md)<br>

[Android でクロス アプリ SSO を有効にする方法](../azuread-dev/howto-v1-enable-sso-android.md)<br>

[iOS でクロス アプリ SSO を有効にする方法](../azuread-dev/howto-v1-enable-sso-ios.md)<br>

[アプリと Azure AD の統合](./quickstart-register-app.md)<br>

[Microsoft ID プラットフォームでのアクセス許可と同意](./v2-permissions-and-consent.md)<br>

[Azure AD StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)