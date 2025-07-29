## Network > DNS Plus > API Guide

The document describes API of the DNS Plus service.


## Common Information of API

### Preparation

- An appkey is required to use the API.
- Your appkey can be found in the **URL & Appkey** menu on the top of the console.

### Common Response Information

- '200 OK' is returned for all API requests. For details on response results, refer to the header of each response.

[Success response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[Failure response body]

```
{
    "header": {
        "isSuccessful": false,
        "resultCode": 4010001,
        "resultMessage": "Invalid appKey. "
    }
}
```


## DNS Zone API

### Query DNS Zone

- Retrieves the list of DNS Zones.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[Options]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Max. 3,000 | Optional |  | DNS Zone ID List |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | Optional | | DNS Zone status list <br>(CREATING: Creating, <br>DELETING: Deleting, <br>DELETING_FAIL: Failed to delete, <br>USE: Use) |
| searchZoneName | String |  | Optional |  | DNS Zone name to search for |
| engineId | String | | Optional |  | DNS server ID |
| page | int | Min. 1 | Optional | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Optional | 50 | Query count |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort order (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | Optional | CREATED_AT | Sort criteria <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>ZONE_NAME: DNS Zone name, <br>ZONE_STATUS: DNS Zone status, <br>RECORDSET_COUNT: Record set count) |

#### Response

[Response body]

```
{
    "header": {
        // Omitted
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            "description": "Test",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total DNS Zone count |
| zoneList | List | DNS Zone list |
| zoneList[0].engineId | boolean | DNS server ID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone name |
| zoneList[0].zoneStatus | String | DNS Zone status |
| zoneList[0].description | String | Description |
| zoneList[0].createdAt | DateTime | Created date |
| zoneList[0].updatedAt | DateTime | Modified date |
| zoneList[0].recordsetCount | long | Record set count |


### Create DNS Zone

- Creates a DNS Zone.
- The **DNS Zone name** must be unique within the DNS server.
- The same **DNS Zone name** can be created as many as the number of DNS servers. There are three DNS servers.

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| zone | Object |  | Required |  | DNS Zone |
| zone.zoneName | String | Max. 254 characters<br>Lowercase characters and numbers, '.', '-', '_'<br>Last character '.' | Required |  | DNS Zone name to create,  <br>Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| zone.description | String | Max. 255 characters | Optional |  | DNS Zone description |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "zone": {
        "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
        "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
        "zoneName": "test.dnsplus.com.",
        "zoneStatus": "USE",
        "description": "test",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:32:50.000+09:00",
        "recordsetCount": 2
    }
}
```


### Update DNS Zone

- Updates the DNS Zone.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| zone | Object |  | Required |  | DNS Zone |
| zone.description | String | Max. 255 characters | Optional |  | DNS Zone description |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "zone": {
        "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
        "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
        "zoneName": "test.dnsplus.com.",
        "zoneStatus": "USE",
        "description": "test",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:42:00.000+09:00",
        "recordsetCount": 2
    }
}
```


### Delete DNS Zone (async)

- Deletes multiple DNS Zones along with their record sets.
- Actual deletion of data is processed asynchronously.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[Request body]

- Change {appkey} to the value found in the console.
- DNS Zone ID can be found by [Query DNS Zone](./api-guide/#query-dns-zone).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| zoneIdList | List | Min. 1, Max. 3,000 | Required |  | DNS Zone ID List |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## Record Set API

### Query Record Set

- Retrieves the list of record sets.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[Options]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Max. 3,000 | Optional |  | Record set list |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS, SOA | Optional | | Record set type list |
| searchRecordsetName | String |  | Optional |  | Record set name to search for |
| page | int | Min. 1 | Optional | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Optional | 50 | Query count |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort order (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | Optional | CREATED_AT | Sort criteria <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>RECORDSET_NAME: Record set name, <br>RECORDSET_TYPE: Record set type, <br>RECORDSET_TTL: TTL (sec)) |

#### Response

[Response body]

```
{
    "header": {
        // Omitted
    },
    "totalCount": 2,
    "recordsetList": [
        {
            "recordsetId": "9e92b547-e2c1-4398-8904-552e0ca465e2",
            "recordsetName": "test.dnsplus.com.",
            "recordsetType": "SOA",
            "recordsetTtl": 1500,
            "recordsetStatus": "USE",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordList": [
                {
                    "recordDisabled": false,
                    "recordContent": "ns1.dnsplus.com. hostmaster.dnsplus.com. 2019060401 10800 3600 604800 1200",
                    // Omitted: Varies by record set type
                }
            ]
        },
        {
            "recordsetId": "edb9512b-6e62-409c-99ee-092d340e0adf",
            "recordsetName": "test.dnsplus.com.",
            "recordsetType": "NS",
            "recordsetTtl": 1500,
            "recordsetStatus": "USE",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordList": [
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.com.",
                    // Omitted: Varies by record set type
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // Omitted: Varies by record set type
                }
            ]
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total record set count |
| recordsetList | List | Record set list |
| recordsetList[0].recordsetId | String | Record set ID |
| recordsetList[0].recordsetName | String | Record set name |
| recordsetList[0].recordsetType | String | Record set type |
| recordsetList[0].recordsetTtl | int | Update cycle of the record set data in the name server |
| recordsetList[0].recordsetStatus | String | Record set status |
| recordsetList[0].createdAt | DateTime | Created date |
| recordsetList[0].updatedAt | DateTime | Modified date |
| recordsetList[0].recordList | List | Record list |
| recordsetList[0].recordList[0].recordDisabled | boolean | Whether record is disabled or not |
| recordsetList[0].recordList[0].recordContent | String | A record value. It displays the detailed field by record set type in a line. |


### Create Record Set

- Creates a record set.
- The supported **record set types** are A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, and SOA.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.
- The maximum length of the record list within the record set is 512 bytes.
- Up to 5,000 record sets can be created per DNS Zone.
- There is a limit to the maximum number of record sets that can be created. If you want to raise the limit, please contact us. [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed field in one line separated by space. You can check the detailed field in [Detailed field by record set type].
- If you enter values in both the detailed field and the recordContent field at the same time, the value in the recordContent field will take priority.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset | Object |  | Required |  | Record set |
| recordset.recordsetName | String | Max. 254 characters<br>Lowercase characters and numbers, '.', '-', '_'<br>(including name of DNS Zone) | Required |  | Name of the record set to create, <br>Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required |  | Record set type |
| recordset.recordsetTtl | int | Min. 10, Max. 2147483647 | Required |  | Update cycle of the record set data in the name server |
| recordset.recordList | List |  | Required |  | Record list |
| recordset.recordList[0].recordDisabled | boolean |  | Optional | false | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String |  | Required |  | It displays the detailed field by record set type in a line. |

[Detailed field by record set type]

- A record set
    - Multiple records can be entered.
    - One domain name can be used for multiple IPv4 addresses.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | Required |  | IPv4 type address |


- AAAA record set
    - Multiple records can be entered.
    - One domain name can be used for multiple IPv6 addresses.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | Required |  | IPv6 type address |


- CAA record set
    - Multiple records can be entered.
    - Setting an authorized certificate authority (CA) to the domain allows you to prevent any unauthorized CA from issuing the certificate.
    - The issue tag is a certificate issuance permission for domain or sub-domain.
    - The issuewild tag is a wildcard certificate issuance permission for domain or sub-domain.
        - You can use the same setting method for the issue tag and the issuewild tag.
        - Allow the issuance of a certificate: Enter the address of the certificate authority. If additional settings are required, use semicolon (;) as a separator and set them in the 'name=value' pairs.
        - Do not allow the issuance of a certificate: Enter a semicolon (;)
    - The iodef tag is used for notifying via a specified email or URL address when an invalid request has been received by the CA.
        - Email address input format: "mailto:*email-address*"
        - URL address input format: "http://*URL*" or "https://*URL*"
    - A custom tag can be used when the CA supports additional features other than the RFC standard.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0 or 128 | Required |  | 0 for a defined tag, <br>128 for a custom tag |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>Up to 15 custom tags | Required |  | TAG_ISSUE: issue tag, <br>TAG_ISSUEWILD: issuewild tag, <br>TAG_IODEF: iodef tag, <br>custom tag |
| recordset.recordList[0].stringValue | String | Max. 512 characters (including quotation marks) | Required |  | Tag-dependent content |


- CNAME record set
    - A single record can be entered.
    - Define the record set name as an alias of a canonical name (canonical).
    - CNAME record set can be created if there is no other record set type for the same record set name.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- MX record set
    - Multiple records can be entered.
    - Specify the mail server for the domain.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Min. 0, Max. 65535 | Required |  | Priority |
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- NAPTR record set
    - Multiple records can be entered.
    - In the Dynamic Delegation Discovery System (DDDS) application, it is used to convert or replace one value with another.
    - Order is the order of records evaluation by the DDDS application.
    - Preferred order is the order of records evaluation when there are more than two records with exactly the same order.
    - Separator, which is set in the DDDS application, uses space, 'S', 'A', 'U', and 'P'; the other characters are reserved for something else.
    - Service is set in the DDDS application. For more detailed definition, see the RFC document.
        - URI DDDS Application [RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS Application [RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS Application [RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - Regexp is used to convert an input value to an output value in the DDDS application. For detailed definition, see [RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2).
    - Replacement value replaces an input value with the name of a domain where the DDDS application will submit DNS queries. Set this as '.' if regexp is set.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | Min. 0, Max. 65535 | Required |  | Order |
| recordset.recordList[0].preference | int | Min. 0, Max. 65535 | Required |  | Preference |
| recordset.recordList[0].flags | String | Max. 3 characters (including quotation marks) | Required |  | Classification |
| recordset.recordList[0].service | String | Max. 257 characters (including quotation marks) | Required |  | Service |
| recordset.recordList[0].regexp | String | Max. 257 characters (including quotation marks) | Required |  | Regular expression |
| recordset.recordList[0].replacement | String | Max. 255 characters | Required |  | Enter '.' as a replacement value or enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)  |


- PTR record set
    - Multiple records can be entered.
    - This is a reverse query to perform lookup of the domain information with its IP address. You should set this by requesting to your ISP company.
    - The IP address must be entered in the record set name in the reverse order. (Example) 127.0.0.1, 1.0.0.127.in-addr.arpa

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- TXT record set
    - Multiple records can be entered.
    - Enter the text for the record set name.
    - SPF records can be created with the TXT record set type.
        - This feature verifies whether the incoming email server has the same email address as the outgoing mail server by the email sender domain authentication method.
        - Enter as follows. For detailed definition, see [RFC4408](https://tools.ietf.org/html/rfc4408).
        - The default value of the qualifier is '+', and you can add IP or domain name depending on the mechanism.
            - Format: "v=spf1 {qualifier}{mechanism}{content} {modifier}={content}"
            - qualifier: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
            - mechanism: all, include, a, mx, ptr, ip4, ip6, exists
            - modifier: redirect, exp, custom
            - (Example)
                - "v=spf1 mx -all"
                - "v=spf1 ip4:192.168.0.1/16 -all"
                - "v=spf1 a:toast.com -all"
                - "v=spf1 redirect=toast.com"

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | Max. 255 bytes (including quotation marks) | Required |  | Text |


- SRV record set
    - Multiple records can be entered.
    - Find multiple servers which provide similar TCP/IP-based services with a single DNS query operation.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | Min. 0, Max. 65535 | Required |  | Priority |
| recordset.recordList[0].weight | int | Min. 0, Max. 65535 | Required |  | Weight |
| recordset.recordList[0].port | int | Min. 0, Max. 65535 | Required |  | Port |
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


- NS record set
    - Multiple records can be entered.
    - Enter the name server for the record set name.
    - The record set name can be created or modified only with the sub-domain of the DNS Zone name.

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | Max. 255 characters | Required |  | Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |


#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "recordset": {
        "recordsetId": "d0b7ee57-8e41-438f-ad04-d4b316793d42",
        "recordsetName": "sub.test.dnsplus.com.",
        "recordsetType": "A",
        "recordsetTtl": 86400,
        "recordsetStatus": "USE",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:32:50.000+09:00",
        "recordList": [
            {
                "recordDisabled": false,
                "recordContent": "1.1.1.1",
                "ipV4": "1.1.1.1"
            }
        ]
    }
}
```


### Bulk Create Record Sets

- You can create multiple record sets, up to 2,000 sets per request.
- The supported **record set types** are A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, and SOA.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.
- The maximum length of the record list within the record set is 512 bytes.
- Up to 5,000 record sets can be created per DNS Zone.
- There is a limit to the maximum number of record sets that can be created. If you want to raise the limit, please contact us. [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed field in one line separated by space. You can check the detailed field in the [Detailed field by record set type] section of [Create Record Set](./api-guide/#create-record-set).
- If you enter values in both the detailed field and the recordContent field at the same time, the value in the recordContent field will take priority.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list' \
-H 'Content-Type: application/json' \
--data '{ "recordsetList": [{ "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }]}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetList | List |  | Required |  | Record set list |
| recordsetList[0].recordsetName | String | Max. 254 characters<br>Lowercase characters and numbers, '.', '-', '_'<br>(including name of DNS Zone) | Required |  | Name of the record set to create, <br>Enter the domain as [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) |
| recordsetList[0].recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required |  | Record set type |
| recordsetList[0].recordsetTtl | int | Min. 10, Max. 2147483647 | Required |  | Update cycle of the record set data in the name server |
| recordsetList[0].recordList | List |  | Required |  | Record list |
| recordsetList[0].recordList[0].recordDisabled | boolean |  | Optional | false | Whether record is disabled or not |
| recordsetList[0].recordList[0].recordContent | String |  | Required |  | It displays the detailed field by record set type in a line. |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### Modify Record Set

- Modifies a record set.
- **Record set name** cannot be modified. However, **record set type**, **TTL (sec)**, and **record value** can be modified.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.
- The maximum length of the record list within the record set is 512 bytes.

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).
- {recordsetId} is the record set ID and you can check this by performing [Query Record Set](./api-guide/#query-record-set).
- Record value is required. You can enter the value by selecting either recordset.recordList[0].recordContent field or the detailed field.
- The recordContent field displays the detailed field in one line separated by space. You can check the detailed field in the [Detailed field by record set type] section of [Create Record Set](./api-guide/#create-record-set).
- If you enter values in both the detailed field and the recordContent field at the same time, the recordContent field will take priority.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordset | Object |  | Required |  | Record set |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | Required |  | Record set type |
| recordset.recordsetTtl | int | Min. 10, Max. 2147483647 | Required |  | Update cycle of the record set data in the name server |
| recordset.recordList | List |  | Required |  | Record list |
| recordset.recordList[0].recordDisabled | boolean |  | Required |  | Whether record is disabled or not |
| recordset.recordList[0].recordContent | String |  | Required |  | It displays the detailed field by record set type in a line. |


#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "recordset": {
        "recordsetId": "d0b7ee57-8e41-438f-ad04-d4b316793d42",
        "recordsetName": "sub.test.dnsplus.com.",
        "recordsetType": "A",
        "recordsetTtl": 86400,
        "recordsetStatus": "USE",
        "createdAt": "2019-06-04T12:32:50.000+09:00",
        "updatedAt": "2019-06-04T12:42:00.000+09:00",
        "recordList": [
            {
                "recordDisabled": false,
                "recordContent": "1.1.1.1",
                "ipV4": "1.1.1.1"
            }
        ]
    }
}
```


### Delete Record Set

- Deletes multiple record sets along with the records in the record sets.
- SOA record set cannot be created, modified, or deleted. NS record set cannot be created, modified, or deleted using the **DNS Zone name**.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[Request body]

- Change {appkey} to the value found in the console.
- {zoneId} is the DNS Zone ID, which can be found by [Query DNS Zone](./api-guide/#query-dns-zone).
- You can check the record set ID by performing [Query Record Set](./api-guide/#query-record-set).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| recordsetIdList | List | Min. 1, Max. 3,000 | Required |  | Record set ID list |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

## GSLB API

### Query GSLB

- Retrieves the list of GSLBs.
- When a health check is connected to pools, you can check the health status of GSLB, pools, and endpoints.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?showHealthy=true'
```

[Options]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| gslbIdList | List | Max. 3,000 | Optional |  | GSLB ID list |
| searchGslbName | String |  | Optional |  | Name of GSLB to search for |
| gslbDomain | String |  | Optional |  | GSLB domain |
| showHealthy | boolean |  | Optional |  | Whether to view the health check results |
| page | int | Min. 1 | Optional | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Optional | 50 | Query count |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort order (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>GSLB_NAME, <br>GSLB_DOMAIN, <br>GSLB_TTL, <br>GSLB_ROUTING_RULE, <br>GSLB_DISABLED | Optional | CREATED_AT | Sort criteria <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>GSLB_NAME: GSLB name, <br>GSLB_DOMAIN: GSLB domain, <br>GSLB_TTL: GSLB domain update cycle, <br>GSLB_ROUTING_RULE: Routing rule, <br>GSLB_DISABLED: Whether GSLB is disabled or not) |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "gslbList": [
        {
            "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
            "gslbName": "GSLB-test",
            "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
            "gslbTtl": 300,
            "gslbRoutingRule": "GEOLOCATION",
            "gslbDisabled": false,
            "healthy": true,
            "connectedPoolList": [
                {
                    "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                    "connectedPoolOrder": 1,
                    "pool": {
                        // Pool information omitted
                    }
                },
                {
                    "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                    "connectedPoolOrder": 2,
                    "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                    "pool": {
                        // Pool information omitted
                    }
                }
            ],
            "createdAt": "2019-12-18T20:44:02.000+09:00",
            "updatedAt": "2019-12-18T21:01:05.000+09:00"
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of GSLBs |
| gslbList | List | Pool list |
| gslbList[0].gslbId | String | GSLB ID |
| gslbList[0].gslbName | String | GSLB name |
| gslbList[0].gslbDomain | String | GSLB domain |
| gslbList[0].gslbTtl | String | GSLB domain update cycle |
| gslbList[0].gslbRoutingRule | String | Routing rule |
| gslbList[0].gslbDisabled | boolean | Whether GSLB is disabled or not |
| gslbList[0].healthy | boolean | Whether GSLB is healthy or not |
| gslbList[0].connectedPoolList | List | Connected pool list |
| gslbList[0].connectedPoolList[0].poolId | String | Connected pool ID |
| gslbList[0].connectedPoolList[0].pool | Object | Connected pool information |
| gslbList[0].connectedPoolList[0].connectedPoolOrder | int | Connected pool priority |
| gslbList[0].connectedPoolList[0].connectedPoolRegionContent | String | It displays the connected pool's regions in a line |
| gslbList[0].createdAt | DateTime | Created date |
| gslbList[0].updatedAt | DateTime | Modified date |


### Create GSLB

- Creates GSLB and pool connection settings.
- For **routing rule**, you can select FAILOVER, RANDOM, or GEOLOCATION as a load balancing method for GSLB domain.
    - FAILOVER: Performs routing based on the priorities of connected pools.
    - RANDOM: Performs routing by randomly selecting an available pool among connected pools.
    - GEOLOCATION: Routes the traffic of the configured region to the connected pool. When there is no configured region, performs routing based on the priorities of connected pools.
- The smaller the **priority** of the **connected pool**, the higher the routing order, and it cannot be duplicated.
- There are limits to the maximum number of GSLBs that can be created and to the maximum number of pools that can be connected. If you want to raise the limits, please contact us. [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[Request body]

- Change {appkey} to the value found in the console.
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbRoutingRule": "FAILOVER", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2 } ] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| gslb | Object |  | Required |  | GSLB |
| gslb.gslbName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | GSLB name |
| gslb.gslbTtl | int |  | Required | false | GSLB domain update cycle |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | Required |  | Routing rule |
| gslb.gslbDisabled | boolean |  | Optional | false | Whether GSLB is disabled or not |
| gslb.connectedPoolList | List |  | Optional |  | Connected pool list |
| gslb.connectedPoolList[0].poolId | String |  | Required |  | Connected pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | Min. 1, Max. 2,147,483,647 | Required |  | Connected pool priority |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional |  | Connected pool region settings |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "gslb": {
        "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
        "gslbName": "GSLB-test",
        "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
        "gslbTtl": 300,
        "gslbRoutingRule": "FAILOVER",
        "gslbDisabled": false,
        "connectedPoolList": [
            {
                "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                "connectedPoolOrder": 1,
                "pool": {
                    // Pool information omitted
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "pool": {
                    // Pool information omitted
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:44:03.000+09:00"
    }
}
```


### Update GSLB

- Updates GSLB and pool connection settings.
- Updates the items entered in [Create GSLB](./api-guide/#create-gslb).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId} |

[Request body]

- Change {appkey} to the value found in the console.
- {gslbId} is a GSLB ID and can be found by [Query GSLB](./api-guide/#query-gslb).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbDisabled": true, "gslbRoutingRule": "GEOLOCATION", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2, "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA" } ] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| gslb | Object |  | Required |  | GSLB |
| gslb.gslbName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | GSLB name |
| gslb.gslbTtl | int |  | Required | false | GSLB domain update cycle |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | Required |  | Routing rule |
| gslb.gslbDisabled | boolean |  | Optional | false | Whether GSLB is disabled or not |
| gslb.connectedPoolList | List |  | Optional |  | Connected pool list |
| gslb.connectedPoolList[0].poolId | String |  | Required |  | Connected pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | Min. 1, Max. 2,147,483,647 | Required |  | Connected pool priority |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional |  | Connected pool region settings |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "gslb": {
        "gslbId": "91de0c6f-aeaa-44ec-b361-822acfcd5921",
        "gslbName": "GSLB-test",
        "gslbDomain": "rgpac3e7q9onlipdfg.toastgslb.com",
        "gslbTtl": 300,
        "gslbRoutingRule": "GEOLOCATION",
        "gslbDisabled": true,
        "connectedPoolList": [
            {
                "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
                "connectedPoolOrder": 1,
                "pool": {
                    // Pool information omitted
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                "pool": {
                    // Pool information omitted
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:59:49.000+09:00"
    }
}
```


### Delete GSLB

- Deletes multiple GSLBs.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?
gslbIdList=91de0c6f-aeaa-44ec-b361-822acfcd5921,269eff10-f3c0-4b11-b072-ec53e7c604bf'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| gslbIdList | List | Min. 1, Max. 3,000 | Required |  | GSLB ID list |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### Connect Pool

- Connects a pool to GSLB.
- The smaller the **priority** of the **connected pool**, the higher the routing order. If you enter the same priority as the previously connected pool, the routing order for the existing pool gets lower.
- There is a limit to the maximum number of pools that can be connected. If you want to raise the limit, please contact us. [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[Request body]

- Change {appkey} to the value found in the console.
- {gslbId} is a GSLB ID and can be found by [Query GSLB](./api-guide/#query-gslb).
- {poolId} is a pool ID, which can be found by [Query Pool](./api-guide/#query-pool).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1 } }'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| connectedPool | Object |  | Required |  | Connected pool |
| connectedPool.connectedPoolOrder | int | Min. 1, Max. 2,147,483,647 | Required |  | Connected pool priority |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional |  | Connected pool region settings |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "52da0e48-9062-43f7-bef8-8aec4b795bfe",
            "connectedPoolOrder": 1,
            "pool": {
                // Pool information omitted
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool information omitted
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool information omitted
            }
        }
    ]
}
```

### Update Pool Connection

- Updates the settings of a pool connected to GSLB.
- Updates the items entered in pool settings of [Create GSLB](./api-guide/#create-gslb) or [Connect Pool](./api-guide/#connect-pool).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[Request body]

- Change {appkey} to the value found in the console.
- {gslbId} is a GSLB ID and can be found by [Query GSLB](./api-guide/#query-gslb).
- {poolId} is a pool ID, which can be found by [Query Pool](./api-guide/#query-pool).
- For the connectedPoolRegionContent field, enter **regions** in one line with commas (,) as delimiters.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1, "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA" } }'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| connectedPool | Object |  | Required |  | Connected pool |
| connectedPool.connectedPoolOrder | int | Min. 1, Max. 2,147,483,647 | Required |  | Connected pool priority |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | Optional |  | Connected pool region settings |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "52da0e48-9062-43f7-bef8-8aec4b795bfe",
            "connectedPoolOrder": 1,
            "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA",
            "pool": {
                // Pool information omitted
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool information omitted
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool information omitted
            }
        }
    ]
}
```

### Detach Pool

- Detaches multiple pools connected to GSLB.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools |

[Request body]

- Change {appkey} to the value found in the console.
- {gslbId} is a GSLB ID and can be found by [Query GSLB](./api-guide/#query-gslb).

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools?
poolIdList=52da0e48-9062-43f7-bef8-8aec4b795bfe,12bc396a-eb97-4a6b-ab4c-73d1a1dfb093'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Min. 1, Max. 3,000 | Required |  | Pool ID list |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "connectedPoolList": [
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool information omitted
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool information omitted
            }
        }
    ]
}
```


## Pool API

### Query Pool

- Retrieves the list of pools.
- When a health check is connected, you can check the health status of pools and endpoints.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?showHealthy=true'
```

[Options]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Max. 3,000 | Optional |  | Pool ID list |
| searchPoolName | String |  | Optional |  | Pool name to search for |
| healthCheckId | String |  | Optional |  | Connected health check ID |
| showHealthy | boolean |  | Optional |  | Whether to view the health check results |
| page | int | Min. 1 | Optional | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Optional | 50 | Query count |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort order (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>POOL_NAME, <br>POOL_DISABLED, <br>HEALTH_CHECK_ID | Optional | CREATED_AT | Sort criteria <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>POOL_NAME: Pool name, <br>POOL_DISABLED: Whether a pool is disabled or not, <br>HEALTH_CHECK_ID: Connected health check ID) |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "poolList": [
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "poolName": "POOL-test",
            "poolDisabled": false,
            "healthy": true,
            "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
            "healthCheck": {
                // Health check information omitted
            },
            "endpointList": [
                {
                    "endpointAddress": "test.dnsplus.com",
                    "endpointWeight": 1.0,
                    "endpointDisabled": false,
                    "healthy": true,
                    "failureReason": "No failures"
                },
                {
                    "endpointAddress": "123.123.123.123",
                    "endpointWeight": 1.0,
                    "endpointDisabled": false,
                    "healthy": false,
                    "failureReason": "HTTP timeout occurred"
                },
                {
                    "endpointAddress": "test2.dnsplus.com",
                    "endpointWeight": 1.0,
                    "endpointDisabled": true
                }
            ],
            "createdAt": "2019-12-18T18:36:02.000+09:00",
            "updatedAt": "2019-12-18T18:43:31.000+09:00"
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of pools |
| poolList | List | Pool list |
| poolList[0].poolId | String | Pool ID |
| poolList[0].poolName | String | Pool name |
| poolList[0].poolDisabled | boolean | Whether a pool is disabled or not |
| poolList[0].healthy | boolean | Whether a pool is healthy or not |
| poolList[0].healthCheckId | String | Connected health check ID |
| poolList[0].healthCheck | Object | Connected health check information |
| poolList[0].endpointList | List | Endpoint list |
| poolList[0].endpointList[0].endpointAddress | String | Endpoint address |
| poolList[0].endpointList[0].endpointWeight | double | Endpoint weight |
| poolList[0].endpointList[0].healthy | boolean | Whether an endpoint is healthy or not |
| poolList[0].endpointList[0].failureReason | String | Reason for an unhealthy endpoint |
| poolList[0].createdAt | DateTime | Created date |
| poolList[0].updatedAt | DateTime | Modified date |


### Create Pool

- Creates a pool and an endpoint in the pool.
- You can set **a health check** to check the accessibility of endpoints in the pool.
- The **endpoint address** can be entered as a domain address or IPv4 and has a limit to the input field.
    - The endpoint address cannot start with a hyphen and a period, and cannot end with a hyphen. You cannot enter periods and hyphens consecutively.
    - You cannot enter a [reserved IP address](https://en.wikipedia.org/wiki/Reserved_IP_addresses)
    - The endpoint addresses cannot be duplicated in the pool.
- The **weight** for an endpoint is applied relative to the weights for other endpoints. Equal weights have the same priority in the pool.
- There are limits to the maximum number of pools that can be created, the maximum number of endpoints in a pool, and the maximum total number of endpoints. If you want to raise the limits, please contact us. [1:1 inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "endpointList": [ { "endpointAddress": "test.dnsplus.com" }, { "endpointAddress": "123.123.123.123" } ] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| pool | Object |  | Required |  | Pool |
| pool.poolName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | Pool name |
| pool.poolDisabled | boolean |  | Optional | false | Whether a pool is disabled or not |
| pool.healthCheckId | String |  | Optional |  | Health check ID |
| pool.endpointList | List |  | Required |  | Endpoint list |
| pool.endpointList[0].endpointAddress | String | Max. 254 characters,<br>Lowercase characters and numbers, '.', '-', '_' | Required |  | Endpoint address |
| pool.endpointList[0].endpointWeight | double | Min. 0, Max. 1.00 | Optional | 1.00 | Endpoint weight |
| pool.endpointList[0].endpointDisabled | boolean |  | Optional | false | Whether an endpoint is disabled or not |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "pool": {
        "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
        "poolName": "POOL-test",
        "poolDisabled": false,
        "healthCheckId": "",
        "endpointList": [
            {
                "endpointAddress": "test.dnsplus.com",
                "endpointWeight": 1.0,
                "endpointDisabled": false
            },
            {
                "endpointAddress": "123.123.123.123",
                "endpointWeight": 1.0,
                "endpointDisabled": false
            }
        ],
        "createdAt": "2019-12-18T18:36:02.000+09:00",
        "updatedAt": "2019-12-18T18:36:02.000+09:00"
    }
}
```


### Update Pool

- Updates pools and endpoints in the pools.
- Updates the items entered in [Create Pool](./api-guide/#create-pool).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId} |

[Request body]

- Change {appkey} to the value found in the console.
- {poolId} is a pool ID, which can be found by [Query Pool](./api-guide/#query-pool).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "poolDisabled": true, "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17", "endpointList": [ { "endpointAddress": "test.dnsplus.com", "endpointWeight": 1.00, "endpointDisabled": true }, { "endpointAddress": "123.123.123.123", "endpointWeight": 0.5, "endpointDisabled": true } ] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| pool | Object |  | Required |  | Pool |
| pool.poolName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | Pool name |
| pool.poolDisabled | boolean |  | Optional | false | Whether a pool is disabled or not |
| pool.healthCheckId | String |  | Optional |  | Health check ID |
| pool.endpointList | List |  | Required |  | Endpoint list |
| pool.endpointList[0].endpointAddress | String | Max. 254 characters,<br>Lowercase characters and numbers, '.', '-', '_' | Required |  | Endpoint address |
| pool.endpointList[0].endpointWeight | double | Min. 0, Max. 1.00 | Optional | 1.00 | Endpoint weight |
| pool.endpointList[0].endpointDisabled | boolean |  | Optional | false | Whether an endpoint is disabled or not |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "pool": {
        "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
        "poolName": "POOL-test",
        "poolDisabled": true,
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheck": {
            // Health check information omitted
        },
        "endpointList": [
            {
                "endpointAddress": "test.dnsplus.com",
                "endpointWeight": 1.0,
                "endpointDisabled": true
            },
            {
                "endpointAddress": "123.123.123.123",
                "endpointWeight": 0.5,
                "endpointDisabled": true
            }
        ],
        "createdAt": "2019-12-18T18:36:02.000+09:00",
        "updatedAt": "2019-12-18T18:37:45.000+09:00"
    }
}
```


### Delete Pool

- Deletes multiple pools along with the endpoints in the pools.
- You cannot delete the pools connected to GSLB.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?
poolIdList=8e4326d4-3862-4b46-819e-83a786add570,2f89d3fe-03bc-4711-826e-db2c89c12818'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| poolIdList | List | Min. 1, Max. 3,000 | Required |  | Pool ID list |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## Health Check API

### Query Health Check

- Retrieves the list of health checks.

#### Request

[URI]

| Method | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks'
```

[Options]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheckIdList | List | Max. 3,000 | Optional |  | Health check ID list |
| searchHealthCheckName | String |  | Optional |  | Health check name to search for |
| page | int | Min. 1 | Optional | 1 | Page No. |
| limit | int | Min. 1, Max. 3,000 | Optional | 50 | Query count |
| sortDirection | String | DESC, ASC | Optional | DESC | Sort order (DESC: Descending, ASC: Ascending) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>HEALTH_CHECK_NAME, <br>PROTOCOL, <br>PORT | Optional | CREATED_AT | Sort criteria <br>(CREATED_AT: Created date, <br>UPDATED_AT: Modified date, <br>HEALTH_CHECK_NAME: Health check name, <br>PROTOCOL: Protocol, <br>PORT: Port) |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "totalCount": 1,
    "healthCheckList": [
        {
            "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
            "healthCheckName": "HTTPS-443",
            "protocol": "HTTPS",
            "interval": 60,
            "timeout": 5,
            "retries": 2,
            "path": "/",
            "expectedCodes": "2xx",
            "expectedBody": "OK",
            "allowInsecure": false,
            "requestHeaderList": [
                { "Host": "nhncloud.com" }
            ],
            "createdAt": "2019-12-18T12:31:34.000+09:00",
            "updatedAt": "2019-12-18T14:19:20.000+09:00"
        }
    ]
}
```

[Fields]

| Name | Type | Description |
|---|---|---|
| totalCount | long | Total number of health checks |
| healthCheckList | List | Health check list |
| healthCheckList[0].healthCheckId | String | Health check ID |
| healthCheckList[0].healthCheckName | String | Health check name |
| healthCheckList[0].protocol | String | Protocol |
| healthCheckList[0].port | int | Port |
| healthCheckList[0].interval | int | Health check interval |
| healthCheckList[0].timeout | int | Maximum response latency (timeout) |
| healthCheckList[0].retries | int | Maximum number of retries |
| healthCheckList[0].path | String | Path |
| healthCheckList[0].expectedCodes | String | Expected status code |
| healthCheckList[0].expectedBody | String | Expected response body |
| healthCheckList[0].allowInsecure | boolean | Disable certificate validation |
| healthCheckList[0].requestHeaderList | List | List of request headers |
| healthCheckList[0].requestHeaderList[0] | Object | Request header name, value object |
| healthCheckList[0].createdAt | DateTime | Created date |
| healthCheckList[0].updatedAt | DateTime | Modified date |


### Create Health Check

- Creates a health check.
- For the health check **protocol**, HTTPS, HTTP, and TPC are supported, and the information that can be entered differs depending on the selected protocol.
    - HTTPS input items: No certificate verification, port, health check interval, Maximum response latency (timeout), maximum retries, path, expected status code, expected response body, and request header
    - HTTP input items: Port, health check interval, Maximum response latency (timeout), maximum retries, path, expected status code, expected response body, and request header
    - TCP input items: Port, health check interval, Maximum response latency (timeout), maximum retry count
- Using **Disable certificate validation** allows you to ignore the TLS/SSL certificate of an endpoint being invalid when a health check is performed.
- Does not support a page redirected from an endpoint when determining **Expected status code** and **Expected response body**.
- There is a limit to the maximum number of health checks that can be created. If you want to raise the limit, please contact us. [1:1 Inquiry](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### Request

[URI]

| Method | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "2xx", "allowInsecure": false, "requestHeaderList": [{ "Host": "nhncloud.com" }] }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheck | Object |  | Required |  | Health check |
| healthCheck.healthCheckName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | Health check name |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | Required |  | Protocol to use when performing health checks |
| healthCheck.port | int | Min. 1, Max. 65535 | Required |  | Port to use when performing health checks |
| healthCheck.interval | int | Min. 10 or (retries+1)*timeout, Max. 3600 | Optional | 60 | Health check interval|
| healthCheck.timeout | int | Min. 1, Max. 10 | Optional | 5 | Maximum response latency (timeout) |
| healthCheck.retries | int | Min. 0, Max. 5 | Optional | 2 | Maximum number of retries |
| healthCheck.path | String | Max. 254 characters,<br>Start character '/' | Optional |  | Path to use when performing health checks,<br>used for HTTPS and HTTP |
| healthCheck.expectedCodes | String | Numbers and a wildcard 'x' | Optional |  | Expected status code for health checks,<br>used for HTTPS and HTTP<br>(Example) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | Max. 10KB | Optional |  | Expected response body for health checks,<br>used for HTTPS and HTTP |
| healthCheck.allowInsecure | boolean |  | Optional |  | Disable certificate validation for health checks,<br>used for HTTPS |
| healthCheck.requestHeaderList | List |  | Optional |  | A list of request headers,<br>used for HTTPS and HTTP<br> Items in the list are requested in the ` form { "Header Name": "Header Value" }` |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "healthCheck": {
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheckName": "HTTPS-443",
        "protocol": "HTTPS",
        "port": 443,
        "interval": 60,
        "timeout": 5,
        "retries": 2,
        "path": "/",
        "expectedCodes": "2xx",
        "allowInsecure": false,
        "requestHeaderList": [
            { "Host": "nhncloud.com" }
        ],
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:31:34.000+09:00"
    }
}
```


### Update Health Check

- Updates a health check.
- Updates the items entered in [Create Health Check](./api-guide/#create-health-check).

#### Request

[URI]

| Method | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId} |

[Request body]

- Change {appkey} to the value found in the console.
- {healthCheckId} is a health check ID and can be found by [Query Health Check](./api-guide/#query-health-check).

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId}' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "3xx", "allowInsecure": false }}'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheck | Object |  | Required |  | Health check |
| healthCheck.healthCheckName | String | Max. 100 characters,<br>Uppercase and lowercase characters and numbers, '-', '_' | Required |  | Health check name |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | Required |  | Protocol to use when performing health checks |
| healthCheck.port | int | Min. 1, Max. 65535 | Required |  | Port to use when performing health checks |
| healthCheck.interval | int | Min. 10 or (retries+1)*timeout, Max. 3600 | Optional | | Health check interval|
| healthCheck.timeout | int | Min. 1, Max. 10 | Optional | | Maximum response latency (timeout) |
| healthCheck.retries | int | Min. 0, Max. 5 | Optional | | Maximum number of retries |
| healthCheck.path | String | Max. 254 characters,<br>Start character '/' | Optional |  | Path to use when performing health checks,<br>used for HTTPS and HTTP |
| healthCheck.expectedCodes | String | Numbers and a wildcard 'x' | Optional |  | Expected status code for health checks,<br>used for HTTPS and HTTP<br>(Example) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | Max. 10KB | Optional |  | Expected response body for health checks,<br>used for HTTPS and HTTP |
| healthCheck.allowInsecure | boolean |  | Optional |  | Disable certificate validation for health checks,<br>used for HTTPS |
| healthCheck.requestHeaderList | List |  | Optional |  | A list of request headers,<br>used for HTTPS and HTTP<br> Items in the list are requested in the ` form { "Header Name": "Header Value" }` |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "healthCheck": {
        "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17",
        "healthCheckName": "HTTPS-443",
        "protocol": "HTTPS",
        "port": 443,
        "interval": 60,
        "timeout": 5,
        "retries": 2,
        "path": "/",
        "expectedCodes": "3xx",
        "allowInsecure": false,
        "requestHeaderList": [
            { "Host": "nhncloud.com" }
        ],
        "createdAt": "2019-12-18T12:31:34.000+09:00",
        "updatedAt": "2019-12-18T12:36:20.000+09:00"
    }
}
```


### Delete Health Check

- Deletes multiple health checks.
- You cannot delete the health checks connected to the pool.

#### Request

[URI]

| Method | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[Request body]

- Change {appkey} to the value found in the console.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks?
healthCheckIdList=b9165853-7859-4309-8059-48f12ebdbc17,d2629d6b-9381-4645-9cf3-43d7ad491e2b'
```

[Fields]

| Name | Type | Valid range | Required | Default | Description |
|---|---|---|---|---|---|
| healthCheckIdList | List | Min. 1, Max. 3,000 | Required |  | Health check ID list |

#### Response

[Response body]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```