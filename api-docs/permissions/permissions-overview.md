# Permissions Overview

Permissions enable you to control access to the API on a granular basis (following the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege)). As an example, if you have an employee who needs to initiate payments, but shouldn't be able to manipulate policies, you can do that.

Start by [Creating a Permission](permissions/createpermission.md) with some allowed operations in it, and [Assign it](permissions/createassignment.md) to a User.

## Terminology

* **Permission -** a Permission contains a set of Operations, and can be assigned to users (or Service-Accounts, or Applications). When assigned to a user, a Permission allows him to perform these operations in our API. Each created Permission has a unique name, and unique ID. A Permission can be assigned to one user, or to multiple users, depending on what you need.
* **Operation -** an operation can be added to a Permission, and grants access to one action in the API. There is a fixed list of operations ([see below](permissions-overview.md#available-operations)) that you can include in Permissions. Every API endpoint requires one (or several) operations to use it. Eg. the endpoint [Create Wallet](../wallets/create-wallet/#required-permissions) is the operation `Wallets:Create`.
* **Assignment** - or "Permission Assignment", is the assignment of a given Permission to a given User (or Service-Account, or Application). A permission can be assigned to a User (aka "granted"), or un-assigned from him (aka "revoked").



## Dfns-managed Permissions

When your Dfns organisation is created, some Permissions already exist in it. They are special, some of them are automatically assigned, and some of them are immutable (cannot be updated or archived).

### **`DfnsFullAdminAccess`**

This Permission is automatically assigned to the first User of the Organisation. It includes all existing (and future) operations available in Dfns API. It's immutable, so you cannot update it or archive it. You can only assign it or revoke it.

### **`DfnsDefaultEndUserAccess`**&#x20;

This  unique Permission **is assigned by default to any new EndUser** in your organisation, and comes with an initial set of operations (which you can update at any time) allowing any `EndUser` to "do stuff with the wallet he owns" by default. Here are the initial set of operations in this permission:

```
Keys:Read
Keys:Signatures:Create
Keys:Signatures:Read
Wallets:Read
Wallets:Transactions:Create
Wallets:Transactions:Read
Wallets:Transfers:Create
Wallets:Transfers:Read
```

{% hint style="info" %}
Note: regardless of Permissions, by design, an `EndUser` can never access any wallet that he doesn't own. So this permission does not allow your end-users to access any of your org wallets (aka "custodial" wallets)
{% endhint %}

This permission is meant to facilitate end-user permission management. Since all your end users have this permission assigned by default, you don't necessarily need to explicitly grant them other permissions to allow them to use their wallets, and you only need to modify this one permission to affect all your end users at once.

This permission is not immutable, and you still have full-control over it (update operations in it, un-assign it, deactivate it, though these last two are probably edge-cases)



## List of Operations

Here's a list of all operations available in Dfns API:

```typescript
[
  'Auth:Action:Sign', // only needed for Apps, not for users
  'Auth:Apps:Create',
  'Auth:Apps:Read',
  'Auth:Apps:Update',
  'Auth:Creds:Create',  // only needed for Apps, not for users
  'Auth:Creds:Read',  // only needed for Apps, not for users
  'Auth:Creds:Update',
  'Auth:Creds:Code:Create',  // only needed for Apps, not for users
  'Auth:Types:Application',
  'Auth:Types:Employee',
  'Auth:Types:EndUser',
  'Auth:Types:Pat',
  'Auth:Types:ServiceAccount',
  'Auth:Users:Create',
  'Auth:Users:Delegate',
  'Auth:Users:Read',
  'Auth:Users:Update',
  'Exchanges:Create',
  'Exchanges:Read',
  'Exchanges:Delete',
  'Exchanges:Deposits:Create',
  'Exchanges:Withdrawals:Create',
  'FeeSponsors:Create',
  'FeeSponsors:Read',
  'FeeSponsors:Update',
  'FeeSponsors:Delete',
  'Orgs:Read',
  'Orgs:Update',
  'Orgs:Settings:Read',
  'Orgs:Settings:Update',
  'PermissionAssignments:Create',
  'PermissionAssignments:Read',
  'PermissionAssignments:Revoke',
  'Permissions:Archive',
  'Permissions:Create',
  'Permissions:Read',
  'Permissions:Update',
  'Policies:Archive',
  'Policies:Create',
  'Policies:Read',
  'Policies:Update',
  'Policies:Approvals:Read',
  'Policies:Approvals:Approve',
  'Signers:ListSigners',
  'Stakes:Create',
  'Stakes:Read',
  'Stakes:Update',
  'Keys:Create',
  'Keys:Read',
  'Keys:Update',
  'Keys:Reuse',
  'Keys:Delegate',
  'Keys:Import',
  'Keys:Export',
  'Keys:Signatures:Create',
  'Keys:Signatures:Read',
  'Wallets:Create',
  'Wallets:Read',
  'Wallets:Update',
  'Wallets:Tags:Add',
  'Wallets:Tags:Delete',
  'Wallets:Transactions:Create',
  'Wallets:Transactions:Read',
  'Wallets:Transfers:Create',
  'Wallets:Transfers:Read',
  'Webhooks:Create',
  'Webhooks:Read',
  'Webhooks:Update',
  'Webhooks:Delete',
  'Webhooks:Ping',
  'Webhooks:Events:Read',
  'Billing:Read',
  'Billing:Write',
]
```

