---
title: 教程：将 Azure Active Directory 与 Amazon Web Services (AWS) 集成以连接多个帐户 | Microsoft Docs
description: 了解如何在 Azure AD 和 Amazon Web Services (AWS)  (旧式教程) 之间配置单一登录。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 08/07/2020
ms.author: jeedes
ms.openlocfilehash: 24814ede954980e3a9fc3c3ba60546cedad4e8fd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91713441"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws-legacy-tutorial"></a>教程： Azure Active Directory 与 Amazon Web Services (AWS)  (旧式教程) 

本教程介绍如何将 Azure Active Directory (Azure AD) 与 Amazon Web Services ()  () 。

将 Amazon Web Services (AWS) 与 Azure AD 集成提供以下优势：

- 可以在 Azure AD 中控制谁有权访问 Amazon Web Services (AWS)。
- 可以让用户使用其 Azure AD 帐户自动登录到 Amazon Web Services (AWS)（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 的集成的详细信息，请参阅 [什么是使用 Azure Active Directory 的应用程序访问和单一登录](../manage-apps/what-is-single-sign-on.md)。

![关系图显示 Azure A D，其中的 W S 应用通过 I D P 启动 S O 连接到三个 W S 帐户。](./media/aws-multi-accounts-tutorial/amazonwebservice.png)

> [!NOTE]
> 请注意，不建议将 AWS 应用连接到所有 AWS 账户。 相反，建议使用[此](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial)方法将 AWS 帐户的多个实例配置为 Azure AD 中 AWS 应用的多个实例。 只应使用此方法，因为在其中有少量的 AWS 帐户和角色时，此模型将无法缩放，因为这些帐户中的 AWS 帐户和角色会增长。 此方法不使用 Azure AD 用户预配的 AWS 角色导入功能，因此你必须手动添加/更新/删除角色。 有关此方法的其他限制，请参阅下面的详细信息。

请注意，不建议使用此方法的原因如下****：

* 必须使用 Microsoft Graph 资源管理器方法将所有角色修补到应用中。 不建议使用清单文件方法。

* 我们了解到，客户反映在为单个 AWS 应用添加约 1200 个应用角色后，在应用上执行任何操作都会开始引发与大小相关的错误。 应用程序对象的大小有硬性限制。

* 在任一帐户中添加角色时，必须手动更新角色，遗憾的是，这是一种“替换”方法而非“附加”方法。 另外，如果帐户不断增长，这就变成了帐户和角色的 n x n 的关系。

* 所有 AWS 帐户将都使用相同的联合元数据 XML 文件，并且在证书滚动更新时，必须同时在所有 AWS 帐户上进行大量的操作来更新证书

## <a name="prerequisites"></a>必备条件

若要配置 Azure AD 与 Amazon Web Services (AWS) 的集成，需要具有以下项：

* 一个 Azure AD 订阅。 如果你没有 Azure AD 环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。
* 启用了 Amazon Web Services (AWS) 单一登录的订阅

> [!NOTE]
> 不建议使用生产环境测试本教程中的步骤。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以 [获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* Amazon Web Services (AWS) 支持 **SP 和 IDP** 发起的 SSO
* 配置 Amazon Web Services (AWS) 可强制实施会话控制，这将实时保护组织的敏感数据的渗透和渗透。 会话控制扩展自条件访问。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>从库中添加 Amazon Web Services (AWS)

要配置 Amazon Web Services (AWS) 与 Azure AD 的集成，需要从库将 Amazon Web Services (AWS) 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务  。
1. 导航到“企业应用程序”，选择“所有应用程序”   。
1. 若要添加新的应用程序，请选择“新建应用程序”  。
1. 在“从库中添加”部分的搜索框中，键入 **Amazon Web Services (AWS)** 。
1. 在结果窗格中，选择“Amazon Web Services (AWS)”，然后添加该应用。 在该应用添加到租户时等待几秒钟。

1. 添加应用程序后，请切换到 " **属性** " 页并复制 " **对象 ID**"。

    ![结果列表中的 Amazon Web Services (AWS)](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-properties.png)

## <a name="configure-and-test-azure-ad-sso"></a>配置和测试 Azure AD SSO

在本部分中，将基于名为“Britta Simon”的测试用户配置并测试 Amazon Web Services (AWS) 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Amazon Web Services (AWS) 用户。 换句话说，需在 Azure AD 用户与 Amazon Web Services (AWS) 中的相关用户间建立链接关系。

可通过将 Azure AD 中“用户名”**** 的值指定为 Amazon Web Services (AWS) 中“用户名”**** 的值来建立此链接关系。

若要配置并测试 Amazon Web Services (AWS) 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 Amazon Web Services (AWS) 单一登录](#configure-amazon-web-services-aws-single-sign-on)** - 在应用程序端配置单一登录设置。
3. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，会在 Azure 门户中启用 Azure AD 单一登录并在 Amazon Web Services (AWS) 应用程序中配置单一登录。

**若要配置 Amazon Web Services (AWS) 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 [Azure 门户](https://portal.azure.com/)的 Amazon Web Services (AWS)**** 应用程序集成页上，选择“单一登录”****。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框    。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 在 " **基本 SAML 配置** " 部分中，用户不必执行任何步骤，因为该应用已经与 Azure 预先集成，并单击 " **保存**"。

5. Amazon Web Services (AWS) 应用程序需要采用特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可以在应用程序集成页的“用户属性和声明”部分管理这些属性的值。 在 " **设置具有 SAML 的单一 Sign-On** " 页上，单击 " **编辑** " 按钮以打开 " **用户属性" & 声明** "对话框。

    ![屏幕截图显示了 "编辑" 控件所调用的用户属性。](common/edit-attribute.png)

6. 在“用户属性”对话框的“用户声明”部分中，按上图所示配置 SAML 令牌属性，并执行以下步骤：

    | 名称  | 源属性  | 命名空间 |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | `https://aws.amazon.com/SAML/Attributes` |
    | 角色            | user.assignedroles |  `https://aws.amazon.com/SAML/Attributes`|
    | SessionDuration             | “提供介于 900 秒（15 分钟）到 43200 秒（12 小时）之间的值” |  `https://aws.amazon.com/SAML/Attributes` |

    a. 单击“添加新声明”以打开“管理用户声明”对话框。

    ![屏幕截图显示具有添加新声明的用户声明，并将其保存。](common/new-save-attribute.png)

    ![屏幕截图显示 "管理用户声明"，您可以在其中输入此步骤中描述的值。](common/new-attribute-details.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 在“命名空间”**** 文本框中，键入为该行显示的命名空间值。

    d. 选择“源”作为“属性”。

    e. 在“源属性”  列表中，键入为该行显示的属性值。

    f. 单击“确定”

    g. 单击“ **保存**”。

7. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分中，单击“下载”以下载“联合元数据 XML”并将其保存在计算机上****************。

    ![证书下载链接](common/metadataxml.png)

### <a name="configure-amazon-web-services-aws-single-sign-on"></a>配置 Amazon Web Services (AWS) 单一登录

1. 在其他浏览器窗口中，以管理员身份登录 Amazon Web Services (AWS) 公司站点。

1. 单击“AWS 主页”****。

    ![配置单一登录主页][11]

1. 单击 " **标识和访问管理**"。

    ![配置单一登录标识][12]

1. 单击“标识提供者”****，并单击“创建提供者”****。

    ![配置单一登录提供者][13]

1. 在“配置提供者”**** 对话框页上，执行以下步骤：

    ![配置单一登录对话框][14]

    a. 对于“提供者类型”****，请选择“SAML”****。

    b. 在“提供者名称”**** 文本框中，键入提供者名称（例如：*WAAD*）。

    c. 若要上传从 Azure 门户下载的**元数据文件**，请单击“选择文件”****。

    d. 单击“下一步”****。

1. 在“验证提供者信息”**** 对话框页上，单击“创建”****。

    ![配置单一登录验证][15]

1. 单击“角色”****，然后单击“创建角色”****。

    ![配置单一登录角色][16]

1. 在“创建角色”页中，执行以下步骤：  

    ![配置单一登录信任][19]

    a. 在“选择可信实体的类型”下选择“SAML 2.0 联合身份验证”。********

    b. 在“选择 SAML 2.0 提供程序”**** 部分中，选择之前创建的 SAML 提供程序****（例如：*WAAD*）

    c. 选择“允许以编程方式和通过 AWS 管理控制台进行访问”。
  
    d. 单击“下一步: 权限”****。

1. 在搜索栏中搜索 " **管理员访问权限** " 并选择 " **AdministratorAccess** " 复选框，然后单击 " **下一步：标记**"。

    ![屏幕截图显示选定为策略名称的 AdministratorAccess。](./media/aws-multi-accounts-tutorial/administrator-access.png)

1. 在 " **添加标记 (可选) ** " 部分中，执行以下步骤：

    ![屏幕截图显示 "添加标记" 窗格，可在其中添加键值对。](./media/aws-multi-accounts-tutorial/config2.png)

    a. 在 " **密钥** " 文本框中，输入 Ex： Azureadtest 的密钥名称。

    b. 在 " ** (可选) ** " 文本框中，使用以下格式输入密钥值 `accountname-aws-admin` 。 帐户名称应为全部小写。

    c. 单击 " **下一步：查看**"。

1. 在“审阅”**** 对话框中，执行以下步骤：

    ![配置单一登录审阅][34]

    a. 在 " **角色名称** " 文本框中，输入以下模式的值 `accountname-aws-admin` 。

    b. 在 " **角色说明** " 文本框中，输入与用于角色名称相同的值。

    c. 单击“创建角色”****。

    d. 创建所需数量的角色，并将其映射到标识提供者。

    > [!NOTE]
    > 同样，创建其他其他角色，如 accountname-财务-管理、帐户名-只读用户、devops-user、accountname-需要附加不同策略的用户。 稍后还可以根据每个 AWS 帐户的要求更改这些角色策略，但始终可以在 AWS 帐户中保留每个角色的相同策略。

1. 请记下以下突出显示的 AWS 帐户的帐户 ID： EC2 属性或 IAM 仪表板：

    ![屏幕截图显示帐户 I D 在 W S 窗口中的显示位置。](./media/aws-multi-accounts-tutorial/aws-accountid.png)

1. 现在登录 [Azure 门户](https://portal.azure.com/) ，导航到 **组**。

1. 创建与前面创建的 IAM 角色名称相同的新组，并记下这些新组的 **对象 id** 。

    ![屏幕截图显示在 "概述" 窗格中输入帐户 I 的位置。 ](./media/aws-multi-accounts-tutorial/copy-objectids.png)

1. 从当前 AWS 帐户注销，使用要在其中配置 Azure AD 单一登录的另一个帐户登录。

1. 在帐户中创建所有角色后，这些角色将显示在这些帐户的“角色”列表中。****

    ![屏幕截图显示角色名称、说明和受信任实体的角色列表。](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-listofroles.png)

1. 我们需要捕获所有帐户中所有角色的所有“角色 ARN”和“受信任实体”，并需要将其手动映射到 Azure AD 应用程序。

1. 单击角色复制“角色 ARN”和“受信任实体”值。******** 对于要在 Azure AD 中创建的所有角色，需要使用这些值。

    ![屏幕截图显示选中 "信任关系" 选项卡的 "摘要" 窗格。](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-role-summary.png)

1. 针对所有帐户中的所有角色执行上述步骤，并以 **<角色 ARN>,<受信任实体>** 的格式将其存储在记事本中。

1. 在另一个窗口中打开 [Microsoft Graph 资源管理器](https://developer.microsoft.com/graph/graph-explorer) 。

    a. 使用租户的全局管理员/共同管理员凭据登录到 Microsoft Graph 资源管理器站点。

    b. 需要拥有足够的权限才能创建角色。 单击“修改权限”以获取所需的权限。****

    ![屏幕截图使用 "修改权限" 链接显示图形资源管理器身份验证窗口。](./media/aws-multi-accounts-tutorial/graph-explorer-new9.png)

    c. 从列表中选择以下权限（如果尚未这样做），然后单击“修改权限” 

    ![屏幕截图显示了三个选定的权限： AccessAsUser、目录. 全部和目录。](./media/aws-multi-accounts-tutorial/graph-explorer-new10.png)

    d. 系统会要求重新登录并接受许可。 接受许可后，会再次登录到 Microsoft Graph 资源管理器。

    e. 在下拉列表中将版本更改为 **beta**。 若要从租户提取所有服务主体，请使用以下查询：

    `https://graph.microsoft.com/beta/servicePrincipals`

    如果使用多个目录，则可以使用包含主域的以下模式：`https://graph.microsoft.com/beta/contoso.com/servicePrincipals`

    ![屏幕截图显示 "获取"、"beta" 和 "运行" 查询已选中。](./media/aws-multi-accounts-tutorial/graph-explorer-new1.png)

    f. 从提取的服务主体的列表中，获取需要修改的那一个。 还可使用 Ctrl+F 从列出的所有服务主体中搜索应用程序。 您可以使用以下查询，方法是使用从 Azure AD "属性" 页复制的 " **服务主体" 对象 ID** 转到相应的服务主体。

    `https://graph.microsoft.com/beta/servicePrincipals/<objectID>`.

    ![屏幕截图显示了使用查询获取服务主体对象。](./media/aws-multi-accounts-tutorial/graph-explorer-new2.png)

    g. 从服务主体对象中提取 appRoles 属性。

    ![屏幕截图显示服务主体对象的详细信息。](./media/aws-multi-accounts-tutorial/graph-explorer-new3.png)

    h. 现在需要为应用程序生成新角色。 

    i. 下面 JSON 是 appRoles 对象的示例。 创建类似的对象，以添加应用程序所需的角色。

    ```
    {
    "appRoles": [
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "msiam_access",
            "displayName": "msiam_access",
            "id": "7dfd756e-8c27-4472-b2b7-38c17fc5de5e",
            "isEnabled": true,
            "origin": "Application",
            "value": null
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Admin,WAAD",
            "displayName": "Admin,WAAD",
            "id": "4aacf5a4-f38b-4861-b909-bae023e88dde",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Admin,arn:aws:iam::12345:saml-provider/WAAD"
        },
        {
            "allowedMemberTypes": [
                "User"
            ],
            "description": "Auditors,WAAD",
            "displayName": "Auditors,WAAD",
            "id": "bcad6926-67ec-445a-80f8-578032504c09",
            "isEnabled": true,
            "origin": "ServicePrincipal",
            "value": "arn:aws:iam::12345:role/Auditors,arn:aws:iam::12345:saml-provider/WAAD"
        }    ]
    }
    ```

    > [!Note]
    > 对于修补操作，只能在 **msiam_access** 之后添加新角色。 此外，可以根据组织的需要添加任意数量的角色。 Azure AD 会将这些角色的 **值** 作为 SAML 响应中的声明值发送。

    j. 返回 Microsoft Graph 资源管理器，并将方法从 " **GET** " 更改为 " **PATCH**"。 通过更新 appRoles 属性（类似于上面示例中所示的属性）来修补服务主体对象以获取所需的角色。 单击“运行查询”执行此修补操作。**** 随后会显示一条成功消息，确认已创建 Amazon Web Services 应用程序的角色。

    ![屏幕截图显示已选择 Patch 方法的图形资源管理器。](./media/aws-multi-accounts-tutorial/graph-explorer-new11.png)

1. 使用更多角色修补服务主体后，可将用户/组分配到相应的角色。 可通过转到门户并导航到 Amazon Web Services 应用程序来完成此操作。 在顶部单击“用户和组”选项卡****。

1. 我们建议为每个 AWS 角色创建新组，以便可以在该组中分配该特定角色。 请注意，这是一个组到一个角色的一对一映射。 然后，可以添加属于该组的成员。

1. 创建组后，请选择该组并分配到应用程序。

    ![屏幕截图显示 "添加分配"，其中选择了 "用户和组" 以打开 "用户和组" 窗格。](./media/aws-multi-accounts-tutorial/graph-explorer-new5.png)

    > [!Note]
    > 分配组时不支持嵌套组。

1. 若要将角色分配到组，请选择该角色，然后单击页面底部的“分配”按钮。****

    ![屏幕截图显示已选择一个组的 "添加分配"。](./media/aws-multi-accounts-tutorial/graph-explorer-new6.png)

    > [!Note]
    > 请注意，需要在 Azure 门户中刷新会话才能看到新角色。

### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击“Amazon Web Services (AWS)”磁贴时，应会看到 Amazon Web Services (AWS) 应用程序页，其中包含用于选择角色的选项。

![屏幕截图显示了 "W S 应用程序" 页，您可以在其中选择角色。](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-screen.png)

此外，还可以验证 SAML 响应，以查看角色是否作为声明传递。

![屏幕截图显示了具有属性值的 SAML 响应的一部分。](./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-test-saml.png)

有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../active-directory-saas-access-panel-introduction.md)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

* [如何使用 MS Graph Api 配置预配](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-configure-api)
* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)
* [Microsoft Cloud App Security 中的会话控制是什么？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
* [如何通过高级可见性和控制保护 Amazon Web Services (AWS)](https://docs.microsoft.com/cloud-app-security/protect-aws)

<!--Image references-->

[11]: ./media/aws-multi-accounts-tutorial/ic795031.png
[12]: ./media/aws-multi-accounts-tutorial/ic795032.png
[13]: ./media/aws-multi-accounts-tutorial/ic795033.png
[14]: ./media/aws-multi-accounts-tutorial/ic795034.png
[15]: ./media/aws-multi-accounts-tutorial/ic795035.png
[16]: ./media/aws-multi-accounts-tutorial/ic795022.png
[17]: ./media/aws-multi-accounts-tutorial/ic795023.png
[18]: ./media/aws-multi-accounts-tutorial/ic795024.png
[19]: ./media/aws-multi-accounts-tutorial/ic795025.png
[32]: ./media/aws-multi-accounts-tutorial/ic7950251.png
[33]: ./media/aws-multi-accounts-tutorial/ic7950252.png
[35]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning.png
[34]: ./media/aws-multi-accounts-tutorial/config3.png
[36]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials.png
[37]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-securitycredentials-continue.png
[38]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-createnewaccesskey.png
[39]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-automatic.png
[40]: ./media/aws-multi-accounts-tutorial/tutorial-amazonwebservices-provisioning-testconnection.png
[41]: ./media/aws-multi-accounts-tutorial/