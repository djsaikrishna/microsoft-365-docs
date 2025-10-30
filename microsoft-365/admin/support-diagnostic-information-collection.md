---
title: "Understanding Microsoft 365 case creation and data access"
ms.author: deniseb
author: denisebmsft
manager: dansimp
ms.date: 10/24/2025
audience: Admin
ms.topic: concept-article
ms.service: microsoft-365-business
ms.localizationpriority: medium
ms.collection:
- Tier1
- scotvorg
- Adm_O365
- Adm_TOC
description: "Learn about the diagnostic data Microsoft 365 Support engineers access to resolve support cases, including consent, data categories, retention periods, and logging details."
---

# Understanding Microsoft 365 case creation and data access

This article describes consent that's granted to Microsoft when a support case is opened, the types of data that can be accessed and for how long, and how support activities are logged.

## Consent for diagnostic information

When a user contacts [Microsoft Support](get-help-support.md), consent is implied that Microsoft will be granted access to limited tenant information that's needed to support your issue. When a user selects **Contact Me**, cross-tenant access is initiated in your organization's tenant. This access allows Microsoft Support to collect diagnostic information that helps with troubleshooting and resolving issues.

## What happens when cross-tenant access is granted to Microsoft Support?

When a user creates a support request, cross-tenant access is granted to Microsoft Support. That access is time bound and uses least-privileged access, in accordance with [Zero Trust principles](/security/zero-trust/zero-trust-overview). 

Here are some important points to keep in mind:

- Microsoft Support engineers can access only the specific resources needed for diagnostics and troubleshooting. 
- When a user creates a support request, that user's level of access doesn't change. For example, if the user has a nonprivileged role, their general restrictions don't change because of the support request.
- All support activity is logged in the Microsoft Entra audit and sign-in logs. (See the section, [Where is support activity on a customer tenant logged?](#where-is-support-activity-on-a-customer-tenant-logged) (in this article).)

## How long does Microsoft have this access?

Access is removed automatically when your support case is closed. If your case isn't closed 30 days after the request is created, access will be removed, and you'll be prompted to provide access again. If you have multiple cases open, access expires 30 days after the latest support request was created.

Depending on the nature of your support request, the data that Microsoft can access would belong under one or more of the following categories:

| Category | Examples |
|--|--|
| **Support data**: All data provided to Microsoft by the customer as part of a customer engagement to obtain support services | Support requests from customers and phone conversations, online chat sessions, or remote assistance sessions between support professionals and customers<br/><br/> Case notes and/or records related to support requests from customers<br/><br/>Data provided to Microsoft by the customer as part of support activities |
| **Account data**: Contact, billing, purchase, payment, and/or license information | Customer's provisioning information<br/><br/>Account configuration and billing data<br/><br/>Tenant administrator contact information (such as tenant administrator's name, address, e-mail address, phone number)<br/><br/>Licensing and purchase information |
| **System metadata**: Data generated in the course of running the service | Event logs<br/><br/>Access control logs<br/><br/>Account information belonging to Microsoft operations personnel<br/><br/>Microsoft server names/server IPs<br/><br/>Server patching and vulnerability data<br/><br/>Service configuration data<br/><br/>Telemetry (on-premises or cloud) |
| **Organization identifiable information (OII)**: Data that can be used to identify a particular tenant, deployment, or organization (generally config or usage data) | Tenant ID (non-GUID)<br/><br/>TenantID (GUID) due to the existence of many out of boundary `TenantID` to name mapping tables<br/><br/>Tenant usage data<br/><br/>Tenant IP addresses (IPv4), such as tenant's firewall IP address<br/><br/>Global prefix and subnet ID (first 64 bits of IPv6 address)<br/><br/>Tenant domain name in e-mail address (such as `joe@contoso.com`)<br/><br/>Mapping of organizational GUID to organization |
| **End-user identifiable information (EUII)**: Data that directly identifies or could be used to identify the authenticated user of a Microsoft service | User-specific IP address (IPv4)<br/><br/>User principal name (`joe@company.com`)<br/><br/>Address book data<br/><br/>User's machine name<br/><br/>SIP URI |
| **End-user pseudonymous identifiers (EUPI)**: An identifier created by Microsoft tied to the user of a Microsoft service | User GUIDs or PUIDs<br/><br/>Session IDs |

## How long is diagnostic data retained in Microsoft systems?

Microsoft retains diagnostic data for up to 28 days after it's collected. After this period, the data is deleted.

## Where is support activity on a customer tenant logged?

Activity performed on a customer tenant is available under Microsoft Entra audit logs. The following table describes log entries that are created.

| Scenario | Audit log details |
|--|--|
| A support case is created and cross-tenant access is granted | **Activity type**: Add a partner to cross-tenant access setting<br/>**Category**: `CrossTenantAccessSettings`<br/>**Initiated by (actor)**: <br/>- **Type**: `Application` <br/>- **Display Name**: `EntraGDAP`<br/><br/>**Activity Type**: Add allowed assignable roles<br/>**Category**: `DelegatedAdminServiceProviderConstraints`<br/>**Initiated by (actor)**: <br/>- **Type**: Application <br/>- **Display Name**: `EntraGDAP` |
| A support engineer signs in to investigate and troubleshoot an issue | An entry each time: <br/>- A support engineer signs in <br/>- Diagnostics that involve operations are run<br/><br/>**Initiated by (actor)**:<br/>- **Type**: `Application`<br/>- **Display Name**: `AssistAPI`  |
| Access is removed | **Activity type**: Delete allowed assignable roles<br/>**Category**: `DelegatedAdminServiceProviderConstraints`<br/>**Initiated by (actor)**: <br/>- **Type**: `Application` <br/>- **Display Name**: `EntraGDAP`<br/><br/>**Activity type**: Delete partner specific cross-tenant access setting<br/>**Category**: `CrossTenantAccessSettings`<br/>**Initiated by (actor)**: <br/>- **Type**: `Application` <br/>- **Display Name**: `EntraGDAP` |

## Learn more about audit logs

See the following resources for more information about the audit logs:

- [What are Microsoft Entra audit logs?](/entra/identity/monitoring-health/concept-audit-logs)
- [How to customize and filter identity activity logs](/entra/identity/monitoring-health/howto-customize-filter-logs)