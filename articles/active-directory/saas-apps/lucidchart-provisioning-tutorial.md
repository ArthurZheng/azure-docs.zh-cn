---
title: 教程：为 Lucidchart 配置自动用户预配 Azure Active Directory |Microsoft Docs
description: 了解如何自动将用户 Azure AD 帐户预配到 Lucidchart 以及取消其预配。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: article
ms.date: 01/13/2020
ms.author: Zhchia
ms.openlocfilehash: 43d6ac8354e992a87091bc66417f7ca88e784199
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/09/2020
ms.locfileid: "91850653"
---
# <a name="tutorial-configure-lucidchart-for-automatic-user-provisioning"></a>教程：为 Lucidchart 配置自动用户预配

本教程介绍了需要在 Lucidchart 和 Azure Active Directory (Azure AD) 中执行的步骤，以配置自动用户预配。 配置后，Azure AD 使用 Azure AD 预配服务自动设置用户和组并取消其预配到 [Lucidchart](https://www.lucidchart.com/user/117598685#/subscriptionLevel) 。 有关此服务的功能、工作原理以及常见问题的重要详细信息，请参阅[使用 Azure Active Directory 自动将用户预配到 SaaS 应用程序和取消预配](../manage-apps/user-provisioning.md)。 


## <a name="capabilities-supported"></a>支持的功能
> [!div class="checklist"]
> * 在 Lucidchart 中创建用户
> * 当用户不再需要访问权限时，删除 Lucidchart 中的用户
> * 使用户属性在 Azure AD 和 Lucidchart 之间保持同步
> * 在 Lucidchart 中预配组和组成员身份
> * [单一登录](https://docs.microsoft.com/azure/active-directory/saas-apps/lucidchart-tutorial) 到 Lucidchart (建议) 

## <a name="prerequisites"></a>先决条件

本教程中概述的方案假定你已具有以下先决条件：

* [Azure AD 租户](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* 具有配置预配[权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的 Azure AD 用户帐户（例如应用程序管理员、云应用程序管理员、应用程序所有者或全局管理员）。 
* 启用了 [企业计划](https://www.lucidchart.com/user/117598685#/subscriptionLevel) 或更佳计划的 LucidChart 租户。
* LucidChart 中具有管理员权限的用户帐户。

## <a name="step-1-plan-your-provisioning-deployment"></a>步骤 1。 规划预配部署
1. 了解[预配服务的工作原理](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。
2. 确定谁在[预配范围](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)中。
3. 确定要 [在 Azure AD 与 Lucidchart 之间映射](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)的数据。 

## <a name="step-2-configure-lucidchart-to-support-provisioning-with-azure-ad"></a>步骤 2. 配置 Lucidchart 以支持 Azure AD 的预配

1. 登录到 [Lucidchart 管理控制台](https://www.lucidchart.com)。 导航到 **Team > 应用集成**。

      :::image type="content" source="./media/lucidchart-provisioning-tutorial/team1.png" alt-text="Lucidchart 管理控制台的屏幕截图。&quot;团队&quot; 菜单将突出显示并打开。在 &quot;管理员&quot; 下，突出显示应用集成。" border="false":::

2. 导航到 **SCIM**。

      :::image type="content" source="./media/lucidchart-provisioning-tutorial/scim.png" alt-text="Lucidchart 管理控制台的屏幕截图。&quot;团队&quot; 菜单将突出显示并打开。在 &quot;管理员&quot; 下，突出显示应用集成。" border="false":::

3. 向下滚动以查看 **持有者令牌** 和 **Lucidchart 基 URL**。 复制并保存 **持有者令牌**。 此值将在 Azure 门户的 LucidChart 应用程序的 "预配" 选项卡的 " **机密令牌** " 字段中输入。 

      ![Lucidchart 标记](./media/lucidchart-provisioning-tutorial/token.png)

## <a name="step-3-add-lucidchart-from-the-azure-ad-application-gallery"></a>步骤 3. 从 Azure AD 应用程序库添加 Lucidchart

从 Azure AD 应用程序库中添加 Lucidchart，开始管理预配到 Lucidchart。 如果以前为 SSO 设置了 Lucidchart，则可以使用相同的应用程序。 但建议你在最初测试集成时创建一个单独的应用。 可在[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app)详细了解如何从库中添加应用程序。 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>步骤 4. 定义谁在预配范围中 

使用 Azure AD 预配服务，可以根据对应用程序的分配和/或用户/组的属性来限定谁在预配范围内。 如果选择根据分配来查看要将谁预配到应用，则可以使用以下[步骤](../manage-apps/assign-user-or-group-access-portal.md)将用户和组分配给应用程序。 如果选择仅根据用户或组的属性来限定要对谁进行预配，可以使用[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)所述的范围筛选器。 

* 将用户和组分配到 Lucidchart 时，必须选择 " **默认" 访问权限**以外的其他角色。 具有“默认访问”角色的用户将从预配中排除，并在预配日志中被标记为未有效授权。 如果应用程序上唯一可用的角色是默认访问角色，则可以[更新应用程序清单](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)以添加其他角色。 

* 先小部分测试。 在向全员推出之前，请先使用少量的用户和组进行测试。 如果预配范围设置为分配的用户和组，则可以先尝试将一两个用户或组分配到应用。 当预配范围设置为所有用户和组时，可以指定[基于属性的范围筛选器](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts)。 


## <a name="step-5-configure-automatic-user-provisioning-to-lucidchart"></a>步骤 5。 配置 Lucidchart 的自动用户预配 

本部分介绍了如何配置 Azure AD 预配服务以基于 Azure AD 中的用户和/或组分配在 TestApp 中创建、更新和禁用用户和/或组。

### <a name="to-configure-automatic-user-provisioning-for-lucidchart-in-azure-ad"></a>若要在 Azure AD 中配置 Lucidchart 的自动用户预配：

1. 登录 [Azure 门户](https://portal.azure.com)。 依次选择“企业应用程序”、“所有应用程序” 。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，选择“Lucidchart”****。

    ![应用程序列表中的 Lucidchart 链接](common/all-applications.png)

3. 选择“预配”选项卡。

    ![带有称为 "预配" 选项的 "管理" 选项的屏幕截图。](common/provisioning.png)

4. 将“预配模式”设置为“自动”。

    ![具有 "自动" 选项的 "预配模式" 下拉列表屏幕截图。](common/provisioning-automatic.png)

5. 在 "**管理员凭据**" 部分下，输入之前在 "**机密令牌**" 字段中检索到的**持有者令牌**值。 单击 " **测试连接** " 以确保 Azure AD 可以连接到 Lucidchart。 如果连接失败，请确保 Lucidchart 帐户具有管理员权限，然后重试。

      ![预配](./media/Lucidchart-provisioning-tutorial/lucidchart1.png)

6. 在“通知电子邮件”字段中，输入应接收预配错误通知的个人或组的电子邮件地址，并选中“发生故障时发送电子邮件通知”复选框 。

    ![通知电子邮件](common/provisioning-notification-email.png)

7. 选择“保存”。

8. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 用户同步到 Lucidchart**"。

9. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 Lucidchart 的用户属性。 选为 " **匹配** " 属性的特性用于匹配 Lucidchart 中的用户帐户以执行更新操作。 如果选择更改 [匹配的目标属性](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)，将需要确保 Lucidchart API 支持基于该属性筛选用户。 选择“保存”按钮以提交任何更改。

   |Attribute|类型|
   |---|---|
   |userName|字符串|
   |emails[type eq "work"].value|字符串|
   |活动|Boolean|
   |name.givenName|字符串|
   |name.familyName|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:department|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:division|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:costCenter|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:organization|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:employeeNumber|字符串|
   |urn:ietf:params:scim:schemas:extension:enterprise:2.0:User:manager|参考|
   |urn： ietf： params： scim：架构：扩展： lucidchart：1.0： User： canEdit|布尔值|

10. 在 " **映射** " 部分下，选择 " **将 Azure Active Directory 组同步到 Lucidchart**"。

11. 在 " **属性映射** " 部分中，查看从 Azure AD 同步到 Lucidchart 的组属性。 选为 " **匹配** " 属性的特性用于匹配 Lucidchart 中的组以执行更新操作。 选择“保存”按钮以提交任何更改。

      |Attribute|类型|
      |---|---|
      |displayName|字符串|
      |members|参考|

12. 若要配置范围筛选器，请参阅[范围筛选器教程](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md)中提供的以下说明。

13. 若要为 Lucidchart 启用 Azure AD 预配服务，请在 "**设置**" 部分中将 "**预配状态**" 更改为 **"打开**"。

    ![预配状态已打开](common/provisioning-toggle-on.png)

14. 通过在 "**设置**" 部分的 "**范围**" 中选择所需的值，定义要预配到 Lucidchart 的用户和/或组。

    ![预配范围](common/provisioning-scope.png)

15. 已准备好预配时，单击“保存”。

    ![保存预配配置](common/provisioning-configuration-save.png)

此操作会对“设置”部分的“范围”中定义的所有用户和组启动初始同步周期 。 初始周期执行的时间比后续周期长，只要 Azure AD 预配服务正在运行，后续周期大约每隔 40 分钟就会进行一次。 

## <a name="step-6-monitor-your-deployment"></a>步骤 6. 监视部署
配置预配后，请使用以下资源来监视部署：

1. 通过[预配日志](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)来确定哪些用户已预配成功或失败
2. 检查[进度栏](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user)来查看预配周期的状态以及完成进度
3. 如果怀疑预配配置处于非正常状态，则应用程序将进入隔离状态。 有关隔离状态的详细信息，请访问[此处](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status)。  

## <a name="change-log"></a>更改日志

* 04/30/2020-为用户添加了对企业扩展属性和自定义属性 "CanEdit" 的支持。
* 06/15/2020-已启用用户的软删除 (支持 [活动](https://tools.ietf.org/html/rfc7643) 属性) 。

## <a name="additional-resources"></a>其他资源

* [管理企业应用的用户帐户预配](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>后续步骤

* [了解如何查看日志并获取有关预配活动的报告](../manage-apps/check-status-user-account-provisioning.md)