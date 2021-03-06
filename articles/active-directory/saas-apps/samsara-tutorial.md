---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Samsara 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Samsara 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/15/2020
ms.author: jeedes
ms.openlocfilehash: b864f4204fa546fa1f06e50550376a8a899d5b8c
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91337666"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-samsara"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Samsara 的集成

本教程介绍如何将 Samsara 与 Azure Active Directory (Azure AD) 集成。 将 Samsara 与 Azure AD 集成后，你可以：

* 在 Azure AD 中控制哪一用户有权访问 Samsara。
* 让用户能够使用其 Azure AD 帐户自动登录到 Samsara。
* 在一个中心位置（Azure 门户）管理帐户。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用 Samsara 单一登录 (SSO) 的订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Samsara 支持 SP 和 IDP 启动的 SSO 
* Samsara 支持实时用户预配

## <a name="adding-samsara-from-the-gallery"></a>从库中添加 Samsara

若要配置 Samsara 与 Azure AD 的集成，需要从库中将 Samsara 添加到托管 SaaS 应用的列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 Azure 门户。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。

    ![“Azure Active Directory”按钮](common/select-azuread.png)
    
1. 导航到“企业应用程序”，选择“所有应用程序”   。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

1. 若要添加新的应用程序，请选择“新建应用程序”  。

    ![“新增应用程序”按钮](common/add-new-app.png)

1. 在“从库中添加”部分的搜索框中，键入“Samsara” 。

     ![结果列表中的 OneTrust Privacy Management Software](common/search-new-app.png)

1. 从结果面板中选择“Samsara”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-sso-for-samsara"></a>配置并测试 Samsara 的 Azure AD SSO

配置 Samsara 的 Azure AD SSO，并使用名为“B.Simon”的测试用户来对其进行测试。 若要使 SSO 正常工作，需要在 Azure AD 用户与 Samsara 中的相关用户之间建立链接关系。

若要配置并测试 Samsara 的 Azure AD SSO，请执行以下步骤：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. [配置 Samsara SSO](#configure-samsara-sso) - 在应用程序端配置单一登录设置。
    1. [创建 Samsara 测试用户](#create-samsara-test-user) - 在 Samsara 中创建 B.Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 Azure 门户中的 Samsara 应用程序集成页上，找到“管理”部分并选择“单一登录”  。

    ![配置单一登录链接](common/select-sso.png)

1. 在“选择单一登录方法”页上选择“SAML” 。

    ![单一登录选择模式](common/select-saml-option.png)

1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 在“基本 SAML 配置”部分，输入以下字段的值：

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://cloud.samsara.com/signin/<ORGID>` 用于美国云客户，`https://cloud.eu.samsara.com/signin/<ORGID>` 用于欧洲云客户

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL：`urn:auth0:samsara-dev:samlp-orgid-<ORGID>`

    c. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://samsara-dev.auth0.com/login/callback?connection=samlp-orgid-<ORGID>`

    > [!NOTE]
    > 这些不是实际值。 请使用实际登录 URL、回复 URL 和标识符来更新这些值。 请联系 [Samsara 客户端支持团队](mailto:support@samsara.com)以获取这些值，或者请在 Samsara 中转到“设置” > “单一登录” > ”新建 SAML 连接”，以获取 \<ORGID\>  。 还可参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式****。

1. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中，找到“证书(Base64)”，选择“下载”以下载该证书并将其保存到计算机上     。

    ![证书下载链接](common/certificatebase64.png)

1. 在“设置 Samsara”部分，复制登录 URL 

    ![复制配置 URL](common/copy-configuration-urls.png)
    
### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，将授予 B.Simon 访问 Samsara 的权限，以使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。  
1. 在应用程序列表中，选择”Samsara“。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。
1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。
1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。
1. 如果你希望将某角色分配给用户，可以从“选择角色”下拉列表中选择该角色。 如果尚未为此应用设置任何角色，你将看到选择了“默认访问权限”角色。
1. 在“添加分配”对话框中，单击“分配”按钮。

## <a name="configure-samsara-sso"></a>配置 Samsara SSO

若要在 Samsara 端配置单一登录，需要将从 Azure 门户的下载的“证书(base64)”以及登录 URL 发送到 [Samsara 支持团队](mailto:support@samsara.com)  。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-samsara-test-user"></a>创建 Samsara 测试用户

在本部分中，将在 Samsara 中创建一个名为“B.Simon”的用户。 Samsara 支持默认启用的实时用户预配。 此部分不存在任何操作项。 在身份验证之后会新建一个用户（如果该用户尚不存在于 Samsara 中），其默认角色为组织的标准管理员（无 Dash Cam 访问权限）。 然后，可以在 Samsara 中根据需要增加或减少用户的访问权限。

## <a name="test-sso"></a>测试 SSO 

在本部分，你将使用以下选项测试 Azure AD 单一登录配置。 

1. 在 Azure 门户中单击“测试此应用程序”。 这样将会重定向到 Samsara 登录 URL，可以从那里启动登录流。 

2. 直接转到 Samsara 登录 URL，并从那里启动登录流。

3. 可以使用 Microsoft 访问面板。 在访问面板中单击 Samsara 磁贴时，将会重定向到 Samsara 登录 URL。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。


## <a name="next-steps"></a>后续步骤

配置 Samsara 后，可以强制实施会话控制，以实时防止组织的敏感数据发生外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。


