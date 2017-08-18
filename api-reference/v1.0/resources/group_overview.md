# Working with groups in Microsoft Graph

Groups are collections of [users](user.md) and other principals who share access to resources in Microsoft services or in your app. Microsoft Graph provides APIs that you can use to create and manage different types of groups and group-related functionality. All  operations on groups in Microsoft Graph require administrator consent.

> **Note**: You must use a work or school account to create groups. Groups are not supported for personal Microsoft accounts.

## Types of groups

The following table lists the types of groups that you can access via Microsoft Graph APIs.

| Type              | Used to | groupType | Mail-enabled | Security-enabled | Can be created via APIs? |
|-------------------|----------|-----------|--------------|------------------|--------------------------------|
| Office 365 groups | Facilitate user collaboration with shared Microsoft online resources. | ["Unified"] | True | False | Yes | [user](user.md) | 
| Security groups | Control user access to in-app resources. | [] | False | True | Yes |
| Mail-enabled security groups | Control user access to in-app resources, with a shared group mailbox. | [] | True | True | No |
| Distribution groups | Distribute mail to the members of the group. We recommend that you use Office 365 groups instead of distribution groups because they provide a richer set of resources. | [] | True | False | Yes |

## Office 365 groups in Outlook
Office 365 groups enable powerful collaboration, and are designed for people who work together on a project or a team. They are created with resources that members of the group share including:

- Outlook conversations
- Outlook calendar
- SharePoint files
- OneNote notebook
- SharePoint team site
- Planner
- Intune device management

Other resources that are created but can't be accessed via APIs include: 

- Dynamics CRM
- Microsoft Social Engagement
- Microsoft Stream
- Microsoft StaffHub

### Example of an Office 365 group in Outlook
```http

{
    "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#groups/$entity",
    "id": "4c5ee71b-e6a5-4343-9e2c-4244bc7e0938",
    "deletedDateTime": null,
    "classification": "MBI",
    "createdDateTime": "2016-08-23T14:46:56Z",
    "description": "This is an Outlook group",
    "displayName": "OutlookGroup101",
    "groupTypes": [
        "Unified"
    ],
    "mail": "outlookgroup101@service.microsoft.com",
    "mailEnabled": true,
    "mailNickname": "outlookgroup101",
    "preferredLanguage": null,
    "proxyAddresses": [
        "smtp:outlookgroup101@microsoft.onmicrosoft.com",
        "SMTP:outlookgroup101@service.microsoft.com"
    ],
    "securityEnabled": false,
    "theme": null,
    "visibility": "Public"
}
```
Learn more about Office 365 groups and the administrator experiences [here](https://support.office.com/en-us/article/Learn-about-Office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2).

## Security groups and mail-enabled security groups
Security groups are for controlling user access to resources. By checking whether or not a user is a member of a security group, your app can make authorization decisions when that user is trying to access some secure resources in your app. Security groups can have users and other security groups as members.

Mail-enabled security groups are used in the same way as security groups are, but with the added feature of a shared mailbox for the groups. Mail-enabled security groups can't be created through the API, but other group operations will still work here. Learn more in the [Manage mail-enabled security groups Exchange article](https://technet.microsoft.com/en-us/library/bb123521(v=exchg.160).aspx).

### Example of security group

```http
{
    "@odata.type": "#microsoft.graph.group",
    "id": "f87faa71-57a8-4c14-91f0-517f54645106",
    "deletedDateTime": null,
    "classification": null,
    "createdDateTime": "2016-07-20T09:21:23Z",
    "description": "This group is a Security Group",
    "displayName": "SecurityGroup101",
    "groupTypes": [],
    "mail": null,
    "mailEnabled": false,
    "mailNickname": "mysg",
    "preferredLanguage": null,
    "proxyAddresses": [],
    "securityEnabled": true
}
```
## Dynamic membership 
All types of groups can have dynamic membership rules which automatically add or remove members from the group based on user properties. For example, a "Marketing employees" group would include every user with the department property set to "Marketing", so that new marketing employees are automatically added to the group and employees who leave the department are automatically removed from the group. This rule can be specified in a "membershipRule" field during group creation as ```"membershipRule": 'user.department -eq "Marketing"'``` GroupType must also include ```"DynamicMembership"```. The following request creates a new Office 365 group for the marketing employees: 

```http
POST https://graph.microsoft.com/v1.0/groups
{
    "description": "Marketing department folks",
    "displayName": "Marketing department",
    "groupTypes": [
        "Unified",
        "DynamicMembership"
    ],
    "mailEnabled": true,
    "mailNickname": "marketing",
    "securityEnabled": false,
    "membershipRule": 'user.department -eq "Marketing"',
    "membershipRuleProcessingState": "on"
}
```

Learn more about formulating membershipRules in [advanced rules](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal).

> **Note**: Dynamic membership rules requires the tenant to have a license at tier [Azure Active Directory Premium P1](https://azure.microsoft.com/en-us/pricing/details/active-directory/) or greater.

## Other types of groups

Office 365 groups in Yammer are used to facilitate user collaboration through Yammer posts. This type of group can be returned through a read request, but their posts can't be accessed through the API. To learn more, see [Yammer developer API docs](https://developer.yammer.com/docs).

## Common use cases

Using Microsoft Graph, you can perform the these common operations and more:

| **Use cases**		   | **REST resources**	| **See also** |
|:---------------|:--------|:----------|
| **Group object and methods** | | |
| Create new groups, get existing groups, update the properties on groups, and delete groups. Currently, only security groups and Outlook groups can be created through the API. | [group](group.md) | [Create new groups](../api/group_post_groups.md) <br/> [List groups](../api/group_list.md) <br/> [Update groups](../api/group_update.md) <br/> [Delete groups](../api/group_delete.md) |
| **Group membership methods** | | |
| List the members of a group, and add or remove members. | [user](user.md) <br/> [group](group.md)| [List members](../api/group_list_members.md) <br/> [Add member](../api/group_post_members.md) <br/> [Remove member](../api/group_delete_members.md)|
| Check if a user is a member of a group, get all the groups the user is a member of. | [user](user.md) <br/> [group](group.md)| [Check member groups](../api/group_checkmembergroups.md) <br/> [Get member groups](../api/group_get_membergroups.md)|
| List the owners of a group, and add or remove owners. | [user](user.md) <br/> [group](group.md)| [List owners](../api/group_list_members.md) <br/> [Add member](../api/group_post_members.md) <br/> [Remove member](../api/group_delete_members.md)|

