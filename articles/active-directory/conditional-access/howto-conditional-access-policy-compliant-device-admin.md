---
title: Require administrators use compliant or hybrid joined devices
description: Create a custom Conditional Access policy to require compliant or hybrid joined devices for admins

services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 09/30/2022

ms.author: joflore
author: MicrosoftGuyJFlo
manager: amycolannino
ms.reviewer: calebb, lhuangnorth

ms.collection: M365-identity-device-management
---
# Common Conditional Access policy: Require compliant or hybrid Azure AD joined device for administrators

Accounts that are assigned administrative rights are targeted by attackers. Requiring users with these highly privileged rights to perform actions from devices marked as compliant or hybrid Azure AD joined can help limit possible exposure.

More information about device compliance policies can be found in the article, [Set rules on devices to allow access to resources in your organization using Intune](/intune/protect/device-compliance-get-started)

Requiring a hybrid Azure AD joined device is dependent on your devices already being hybrid Azure AD joined. For more information, see the article [Configure hybrid Azure AD join](../devices/howto-hybrid-azure-ad-join.md).

Microsoft recommends you require enable this policy for the following roles at a minimum, based on [identity score recommendations](../fundamentals/identity-secure-score.md):

- Global administrator
- Application administrator
- Authentication Administrator
- Billing administrator
- Cloud application administrator
- Conditional Access administrator
- Exchange administrator
- Helpdesk administrator
- Password administrator
- Privileged authentication administrator
- Privileged Role Administrator
- Security administrator
- SharePoint administrator
- User administrator

Organizations can choose to include or exclude roles as they see fit.

## User exclusions
[!INCLUDE [active-directory-policy-exclusions](../../../includes/active-directory-policy-exclude-user.md)]

[!INCLUDE [active-directory-policy-deploy-template](../../../includes/active-directory-policy-deploy-template.md)]

## Create a Conditional Access policy

The following steps will help create a Conditional Access policy to require multifactor authentication, devices accessing resources be marked as compliant with your organization's Intune compliance policies, or be hybrid Azure AD joined.

1. Sign in to the **Azure portal** as a Conditional Access Administrator, Security Administrator, or Global Administrator.
1. Browse to **Azure Active Directory** > **Security** > **Conditional Access**.
1. Select **New policy**.
1. Give your policy a name. We recommend that organizations create a meaningful standard for the names of their policies.
1. Under **Assignments**, select **Users or workload identities**.
   1. Under **Include**, select **Directory roles** and choose built-in roles like:
      - Global Administrator
      - Application administrator
      - Authentication Administrator
      - Billing Administrator
      - Cloud application Administrator
      - Conditional Access Administrator
      - Exchange Administrator
      - Helpdesk Administrator
      - Password Administrator
      - Privileged authentication Administrator
      - Privileged Role Administrator
      - Security Administrator
      - SharePoint Administrator
      - User Administrator
   
      > [!WARNING]
      > Conditional Access policies support built-in roles. Conditional Access policies are not enforced for other role types including [administrative unit-scoped](../roles/admin-units-assign-roles.md) or [custom roles](../roles/custom-create.md).

   1. Under **Exclude**, select **Users and groups** and choose your organization's emergency access or break-glass accounts.
1. Under **Cloud apps or actions** > **Include**, select **All cloud apps**.
1. Under **Access controls** > **Grant**.
   1. Select **Require device to be marked as compliant**, and **Require hybrid Azure AD joined device**
   1. **For multiple controls** select **Require one of the selected controls**.
   1. Select **Select**.
1. Confirm your settings and set **Enable policy** to **Report-only**.
1. Select **Create** to create to enable your policy.

After confirming your settings using [report-only mode](howto-conditional-access-insights-reporting.md), an administrator can move the **Enable policy** toggle from **Report-only** to **On**.

> [!NOTE]
> You can enroll your new devices to Intune even if you select **Require device to be marked as compliant** for **All users** and **All cloud apps** using the steps above. **Require device to be marked as compliant** control does not block Intune enrollment. 

### Known behavior

On Windows 7, iOS, Android, macOS, and some third-party web browsers, Azure AD identifies the device using a client certificate that is provisioned when the device is registered with Azure AD. When a user first signs in through the browser the user is prompted to select the certificate. The end user must select this certificate before they can continue to use the browser.

#### Subscription activation

Organizations that use the [Subscription Activation](/windows/deployment/windows-10-subscription-activation) feature to enable users to “step-up” from one version of Windows to another, may want to exclude the Universal Store Service APIs and Web Application, AppID 45a330b1-b1ec-4cc1-9161-9f03992aa49f from their device compliance policy.

## Next steps

[Conditional Access common policies](concept-conditional-access-policy-common.md)

[Determine impact using Conditional Access report-only mode](howto-conditional-access-insights-reporting.md)

[Simulate sign in behavior using the Conditional Access What If tool](troubleshoot-conditional-access-what-if.md)

[Device compliance policies work with Azure AD](/intune/device-compliance-get-started#device-compliance-policies-work-with-azure-ad)
