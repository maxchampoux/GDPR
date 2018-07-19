# GDPR Delete Contacts

* [Overview](#gdpr-overview)
* [Identify usage of a specific contact in my account](#gdpr-retrieve-contact)
* [Proceed deletion](#gdpr-delete-contact)

## Overview <a id="gdpr-overview"></a>

Under the European Union's General Data Protection Regulation (GDPR), recipients in your Mailjet contacts database have the right to request that you delete all their personal data stored on your end. In such cases, the GDPR requires the permanent removal of their contact record from your database, including contact properties, email tracking history and other engagement data. You’ll typically need to respond to these requests within 30 days. 

With the Mailjet API you are able to comply with the GDPR policy and delete a specific contact on a specific API Key.

**NOTE** : To perform a GDPR-compliant deletion, you must proceed the following calls **on all accounts / subaccounts you currently own**.

You must complete the following steps to successfully delete a contact:
1. Identify the presence of this contact in your Mailjet account.
2. Save the Mailjet `{contact_ID}` related to this recipient.
3. Proceed with the deletion using the `{contact_ID}` you retrieved.

## Identify usage of a specific contact in my account <a id="gdpr-retrieve-contact"></a>

To delete a contact, you must first identify its presence in the contact database of your account.

Use `GET /v3/REST/contact/{contact_email}` to proceed it.

> API request:

```json 
curl -s -X GET \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v3/REST/contact/{contact_email}
```

> API response:

```json 
{
    "Count": 1,
    "Data": [
        {
            "CreatedAt": "2018-01-01T00:01:02Z",
            "DeliveredCount": 3,
            "Email": "email@example.com",
            "ExclusionFromCampaignsUpdatedAt": "",
            "ID": 12345678,
            "IsExcludedFromCampaigns": false,
            "IsOptInPending": false,
            "IsSpamComplaining": false,
            "LastActivityAt": "2018-01-01T01:02:03Z",
            "LastUpdateAt": "",
            "Name": "",
            "UnsubscribedAt": "",
            "UnsubscribedBy": ""
        }
    ],
    "Total": 1
} 
```

> If the contact is present in your account, the API will return the contact resource, otherwise you will retrieve a null object.
> Save the contact ID - you will need it to complete the deletion.


## Proceed deletion <a id="gdpr-delete-contact"></a>

Use the {contact_ID} to DELETE a given contact with /v4/contacts/{contact_ID} endpoint. 
When deletion is successful, the API will return a `200 OK` status. Any other response will indicate that the deletion was not successfully processed.

> API request:
```json 
curl -s -X DELETE \
--user "$MJ_APIKEY_PUBLIC:$MJ_APIKEY_PRIVATE" \
https://api.mailjet.com/v4/contacts/{contact_ID} \
```

**Note:** Contact details are immediately anonymized and all records will be deleted after 30 days. This process cannot be reversed. The anonymized contact will retain its contact ID and general configuration settings until it is removed when the 30-day period ends.

> API response:

```json 
{
    "Count": 1,
    "Data": [
        {
            "CreatedAt": "2017-09-26T14:12:32Z",
            "DeliveredCount": 0,
            "Email": "\\xae872d762f67cc4fc5d4bfe04e607256579928ac@domain.invalid",
            "ExclusionFromCampaignsUpdatedAt": "",
            "ID": 21339837,
            "IsExcludedFromCampaigns": false,
            "IsOptInPending": false,
            "IsSpamComplaining": false,
            "LastActivityAt": "2017-09-26T14:12:32Z",
            "LastUpdateAt": "2018-07-12T09:04:23Z",
            "Name": "Anonymized",
            "UnsubscribedAt": "",
            "UnsubscribedBy": ""
        }
    ],
    "Total": 1
}
```

The deletion of a contact does not prevent you from re-uploading the same contact in the future. If you are using an external database to sync contacts with your Mailjet contact database, please make sure to simultaneously remove the contacts from it as well.

This way you will be completely GDPR-compliant and will ensure that the contacts won&#39;t be added by mistake later on.
