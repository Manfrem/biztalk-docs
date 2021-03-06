﻿---
title: ISSONotification.SendNotification Method
TOCTitle: ISSONotification.SendNotification Method
ms:assetid: ab5c09e5-9017-4272-a53f-8db46a7a1971
ms:mtpsurl: https://msdn.microsoft.com/en-us/library/Aa705137(v=BTS.80)
ms:contentKeyID: 51530386
ms.date: 08/30/2017
mtps_version: v=BTS.80
dev_langs:
- c++
---

# ISSONotification.SendNotification Method

 

Sends a notification, such as a password change, from the adapter to the ENTSSO system.

## Syntax

``` c++
  
HRESULT SendNotification(  
SSendNotification SendNotification,  
GUID* pguidTrackingId  
);  
```

#### Parameters

`SendNotification`  
\[in\] The notification to send to ENTSSO from the adapter.

`pguidTrackingId`  
\[out\] When this method returns, contains the tracking ID generated by ENTSSO. You can use the tracking ID for auditing purposes or to correlate request responses. May be NULL.

## Return Value

This method returns an HRESULT indicating whether it completed correctly. For more information, see the Error Values section.

## Error Values

This method returns an HRESULT containing one of the values in the following table.

<table>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>S_OK</td>
<td>The method was successful.</td>
</tr>
<tr class="even">
<td>E_ACCESSDENIED</td>
<td>Access is denied.</td>
</tr>
<tr class="odd">
<td>ENTSSO_E_NO_SERVER</td>
<td>Could not contact the ENTSSO server. Check that the ENTSSO service is running.</td>
</tr>
<tr class="even">
<td>ENTSSO_E_WRONG_STATE</td>
<td>This method has been called in the wrong state.</td>
</tr>
<tr class="odd">
<td>ENTSSO_E_INVALID_NOTIFICATION</td>
<td>Invalid notification type.</td>
</tr>
</tbody>
</table>


## Remarks

You can use **SendNotification** to send password changes and other notifications to the ENTSSO system.

If **SendNotification** returns S\_OK, this does not mean that a password change was completed on the destination system. Instead, receiving an S\_OK means ENTSSO has accepted and eventually will complete your request.

A password change from an external system can have several consequences:

  - If a partial password sync is configured, then the SSO database might be updated, if a current mapping exists for the external account.

  - If a full password sync is configured, then the password change might also be made to a Windows account.

If the external account has no current mapping in the SSO database, the password change might have no effect.

The password change complete notification is issued when the password change is considered complete from the ENTSSO point-of-view, which as discussed above, could mean different things. In some cases, it could mean no change was done, that only the SSO database was updated, or that the Windows password was changed.

Note that password change complete notifications sent back to the adapter are not completely reliable. Under some error conditions, Single Sign-On may never actually receives the requested notifications.

In ENTSSO, the definition of credentials, such as those sent by **SendNotification** for password updates, is more flexible than a simple password. When you define an SSO application, you also define the credential fields. The fields identify the labels to use for the UI fields, and whether those fields are masked or not. In addition, there is also a special flage which specifies whether the field should be synchronized or not. Field 0 is a special case and defines the label for the user ID. For more information, see the [ISSOAdmin Interface](issoadmin-interface-com.md).

## Requirements

**Platforms:**  Windows

## See Also

[ISSONotification Interface (COM)](issonotification-interface-com.md)  
[ISSONotification Members](issonotification-members.md)  
[Programming with Enterprise Single Sign-On](https://msdn.microsoft.com/library/aa704508\(v=bts.80\))

