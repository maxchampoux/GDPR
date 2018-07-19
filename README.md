# GDPR Delete Contacts

* [Overview](#gdpr-overview)
* [Identify usage of a specific contact in my account](#gdpr-retrieve-contact)
* [Proceed deletion](#gdpr-delete-contact)

## Overview

Under the European Union's General Data Protection Regulation (GDPR), recipients in your Mailjet contacts database have the right to request that you delete all their personal data stored on your end. In such cases, the GDPR requires the permanent removal of their contact record from your database, including contact properties, email tracking history and other engagement data. You’ll typically need to respond to these requests within 30 days. 

With the Mailjet API you are able to comply with the GDPR policy and delete a specific contact on a specific API Key.

*NOTE:* To perform a GDPR-compliant deletion, you must proceed the following calls *on all accounts / subaccounts you currently own*.

##
#
[ANNOTATION:

BY &#39;Atanas Damyanliev&#39;
ON &#39;2018-07-12T08:00:49&#39;
NOTE: &#39;atm it is &quot;GDPR Delete Contacts&quot;. Let me know if you have other suggestions&#39;]

#
[ANNOTATION:

BY &#39;Maxime Champoux&#39;
ON &#39;2018-07-11T12:25:18&#39;
NOTE: &#39;The title of this section in the guide will be what exactly?&#39;]
Contact Deletion - API Process



Overview

#
[ANNOTATION:

BY &#39;Atanas Damyanliev&#39;
ON &#39;2018-07-12T08:05:52&#39;
NOTE: &#39;&#39;]

#
[ANNOTATION:

BY &#39;Maxime Champoux&#39;
ON &#39;2018-07-11T12:27:30&#39;
NOTE: &#39;&#39;]
Retrieve a Contact

#
[ANNOTATION:

BY &#39;Atanas Damyanliev&#39;
ON &#39;2018-07-12T08:06:10&#39;
NOTE: &#39;&#39;]

#
[ANNOTATION:

BY &#39;Maxime Champoux&#39;
ON &#39;2018-07-11T12:28:02&#39;
NOTE: &#39;&#39;]
Delete a Contact



### Overview

Under the European Union&#39;s General Data Protection Regulation (GDPR), recipients in your Mailjet contacts database have the right to request that you delete all their personal data stored on your end. In such cases, the GDPR requires the permanent removal of their contact record from your database, including contact properties, email tracking history and other engagement data. You&#39;ll typically need to respond to these requests within 30 days.

With the Mailjet API you are able to comply with the GDPR policy and delete a specific contact on a specific API Key.

**NOTE** : _To perform a GDPR-compliant deletion, you must proceed the following calls_ **on all accounts / subaccounts you currently own** _._

You must complete the following steps to successfully delete a contact:

1. Identify the presence of this contact in your Mailjet account.

2. Save the Mailjet `{contact_ID}` related to this recipient.

3. Proceed with the deletion using the `{contact_ID}` you retrieved.

###

### Retrieve a Contact

To delete a contact, you must first identify its presence in the contact database of your account.

#
[ANNOTATION:

BY &#39;Maxime Champoux&#39;
ON &#39;2018-07-11T12:31:11&#39;
NOTE: &#39;if the contact is present in you account, the API will return contact details, otherwise you will have a null object.&#39;]
Use GET /v3/REST/contact/{contact\_email} to do it.

| curl -s -X GET \
--user &quot;$MJ\_APIKEY\_PUBLIC:$MJ\_APIKEY\_PRIVATE&quot; \
https://api.mailjet.com/v3/REST/contact/{contact\_email} |
| --- |

&gt;API Response:

| {
    &quot;Count&quot;: 1,
    &quot;Data&quot;: [
        {
            &quot;CreatedAt&quot;: &quot;2018-01-01T00:01:02Z&quot;,
            &quot;DeliveredCount&quot;: 3,
            &quot;Email&quot;: &quot;email@example.com&quot;,
            &quot;ExclusionFromCampaignsUpdatedAt&quot;: &quot;&quot;,
             **&quot;ID&quot;**** : **** 12345678 ****,**
            &quot;IsExcludedFromCampaigns&quot;: false,
            &quot;IsOptInPending&quot;: false,
            &quot;IsSpamComplaining&quot;: false,
            &quot;LastActivityAt&quot;: &quot;2018-01-01T01:02:03Z&quot;,
            &quot;LastUpdateAt&quot;: &quot;&quot;,
            &quot;Name&quot;: &quot;&quot;,
            &quot;UnsubscribedAt&quot;: &quot;&quot;,
            &quot;UnsubscribedBy&quot;: &quot;&quot;
        }
    ],
    &quot;Total&quot;: 1
} |
| --- |

Save the contact ID - you need it to complete the deletion process.

###
#
[ANNOTATION:

BY &#39;Maxime Champoux&#39;
ON &#39;2018-07-10T15:08:02&#39;
NOTE: &#39;So here the title should be more functional in my opinion. like: &quot;Proceed the deletion&quot;&#39;]
Delete the Contact

Use the {contact\_ID} you retrieved to DELETE the contact with the /v4/contacts/{contact\_ID} endpoint.  When the deletion is successful, the API will return a `200 OK` status. Any other response will indicate that the deletion was not successfully processed.

| curl -s -X DELETE \
--user &quot;$MJ\_APIKEY\_PUBLIC:$MJ\_APIKEY\_PRIVATE&quot; \
https://api.mailjet.com/v4/contacts/{contact\_ID} \ |
| --- |

**Note:** Contact details are immediately anonymized and all records will be deleted after 30 days. This process cannot be reversed. The anonymized contact will retain its contact ID and general configuration settings until it is removed when the 30-day period ends.

| {
    &quot;Count&quot;: 1,
    &quot;Data&quot;: [
        {
            &quot;CreatedAt&quot;: &quot;2017-09-26T14:12:32Z&quot;,
            &quot;DeliveredCount&quot;: 0,
            &quot;Email&quot;: &quot;\\xae872d762f67cc4fc5d4bfe04e607256579928ac@domain.invalid&quot;,
            &quot;ExclusionFromCampaignsUpdatedAt&quot;: &quot;&quot;,
            &quot;ID&quot;: 21339837,
            &quot;IsExcludedFromCampaigns&quot;: false,
            &quot;IsOptInPending&quot;: false,
            &quot;IsSpamComplaining&quot;: false,
            &quot;LastActivityAt&quot;: &quot;2017-09-26T14:12:32Z&quot;,
            &quot;LastUpdateAt&quot;: &quot;2018-07-12T09:04:23Z&quot;,
            &quot;Name&quot;: &quot;Anonymized&quot;,
            &quot;UnsubscribedAt&quot;: &quot;&quot;,
            &quot;UnsubscribedBy&quot;: &quot;&quot;
        }
    ],
    &quot;Total&quot;: 1
} |
| --- |



The deletion of a contact does not prevent you from re-uploading the same contact in the future. If you are using an external database to sync contacts with your Mailjet contact database, please make sure to simultaneously remove the contacts from it as well.

This way you will be completely GDPR-compliant and will ensure that the contacts won&#39;t be added by mistake later on.
