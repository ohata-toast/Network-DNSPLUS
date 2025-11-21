## Network > DNS Plus > APIガイド

DNS PlusサービスのAPIを説明します。


## API共通情報

### 事前準備

- APIを使用するにはアプリケーションキーが必要です。
- アプリケーションキーは、コンソールの下にある**URL & Appkey**メニューで確認できます。

### レスポンス共通情報

- すべてのAPIリクエストに'200 OK'でレスポンスします。詳細なレスポンス結果は、レスポンス本文のヘッダを参照してください。

[成功レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[失敗レスポンス本文]

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

### DNS Zone照会

- DNS Zoneリストを照会します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[リクエスト本文]

- {appkey}は、コンソールで確認した値に変更します。

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最大3,000個 | 任意 |  | DNS Zone IDリスト |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | 任意 | | DNS Zone状態リスト <br>(CREATING：作成中、 <br>DELETING：削除中、<br>DELETING_FAIL：削除失敗、 <br>USE：使用) |
| searchZoneName | String |  | 任意 |  | 検索するDNS Zone名 |
| engineId | String | | 任意 |  | DNSサーバーID |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | 任意 | CREATED_AT | ソート対象 <br>(CREATED_AT：作成日、 <br>UPDATED_AT：修正日、 <br>ZONE_NAME： DNS Zone名、 <br>ZONE_STATUS： DNS Zone状態、 <br>RECORDSET_COUNT：レコードセット数) |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        // 省略
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            "description": "テスト",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | 全DNS Zone数 |
| zoneList | List | DNS Zoneリスト |
| zoneList[0].engineId | boolean | DNSサーバーID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone名 |
| zoneList[0].zoneStatus | String | DNS Zone状態 |
| zoneList[0].description | String | 説明 |
| zoneList[0].createdAt | DateTime | 作成日 |
| zoneList[0].updatedAt | DateTime | 修正日 |
| zoneList[0].recordsetCount | long | レコードセット数 |


### DNS Zone作成

- DNS Zoneを作成します。
- **DNS Zone名**は、DNSサーバーで唯一のものにする必要があります。
- 同じ**DNS Zone名**は、DNSサーバーの数だけ作成可能です。DNSサーバーは3台です。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zone | Object |  | 必須 |  | DNS Zone |
| zone.zoneName | String | 最大254文字<br>英数字、(.)(-)(_)<br>最後の文字'.' | 必須 |  | 作成するDNS Zone名、<br>ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |
| zone.description | String | 最大255文字 | 任意 |  | DNS Zoneの説明 |

#### レスポンス

[レスポンス本文]

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


### DNS Zone修正

- DNS Zoneを修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zone | Object |  | 必須 |  | DNS Zone |
| zone.description | String | 最大255文字 | 任意 |  | DNS Zoneの説明 |

#### レスポンス

[レスポンス本文]

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


### DNS Zone削除(非同期)

- 複数のDNS Zoneを削除し、DNS Zoneのレコードセットも一緒に削除します。
- 実際のデータ削除は、非同期で処理されます。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- DNS Zone IDは、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| zoneIdList | List | 最小1個、最大3,000個 | 必須 |  | DNS Zone IDリスト |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## レコードセットAPI

### レコードセット照会

- レコードセットリストを照会します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最大3,000個 | 任意 |  | レコードセットリスト |
| recordsetTypeList | List | A、AAAA、CAA、CNAME、MX、<br>NAPTR、PTR、TXT、SRV、NS、SOA | 任意 | | レコードセットタイプリスト |
| searchRecordsetName | String |  | 任意 |  | 検索するレコードセット名 |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | 任意 | CREATED_AT | ソート対象<br>(CREATED_AT：作成日、 <br>UPDATED_AT：修正日、 <br>RECORDSET_NAME：レコードセット名、 <br>RECORDSET_TYPE：レコードセットタイプ、 <br>RECORDSET_TTL： TTL(秒)) |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        // 省略
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
                    // 省略：レコードセットタイプによって異なる
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
                    // 省略：レコードセットタイプによって異なる
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // 省略：レコードセットタイプによって異なる
                }
            ]
        }
    ]
}
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | 全レコードセット数 |
| recordsetList | List | レコードセットリスト |
| recordsetList[0].recordsetId | String | レコードセットID |
| recordsetList[0].recordsetName | String | レコードセット名 |
| recordsetList[0].recordsetType | String | レコードセットタイプ |
| recordsetList[0].recordsetTtl | int | ネームサーバーでレコードセット情報の更新周期 |
| recordsetList[0].recordsetStatus | String | レコードセット状態 |
| recordsetList[0].createdAt | DateTime | 作成日 |
| recordsetList[0].updatedAt | DateTime | 修正日 |
| recordsetList[0].recordList | List | レコードリスト |
| recordsetList[0].recordList[0].recordDisabled | boolean | レコードを無効にするかどうか |
| recordsetList[0].recordList[0].recordContent | String | レコード値。レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |


### レコードセット作成

- レコードセットを作成します。
- **レコードセットタイプ**としてA、AAAA、CAA、CNAME、MX、NAPTR、PTR、TXT、SRV、NS、SOAをサポートします。
- SOAレコードセットは、作成、修正、削除できず、NSレコードセットは、**DNS Zone名**で作成、修正、削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。
  - TXTレコードセットは最大4096バイトです。
- DNS Zoneつ当たり、レコードセットは最大5,000個まで作成できます。
- レコードセットの作成数は制限されています。拡張が必要な場合は別途お問い合わせください。[1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- レコード値は必須で、入力方法にrecordset.recordList[0].recordContentフィールドまたは詳細フィールドを選択できます。
- recordContentフィールドは、空白を区切り文字にして詳細フィールドを1行で表示した内容です。詳細フィールドは、[レコードセットタイプ別の詳細フィールド]で確認できます。
- 詳細フィールドとrecordContentフィールドを同時に入力すると、recordContentフィールドを基準に作成されます。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必須 |  | レコードセット |
| recordset.recordsetName | String | 最大254文字<br>英数字、(.)(-)(_)<br>(DNS Zone名含む) | 必須 |  | 作成するレコードセット名、<br>ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |
| recordset.recordsetType | String | A、AAAA、CAA、CNAME、MX、<br>NAPTR、PTR、TXT、SRV、NS | 必須 |  | レコードセットタイプ |
| recordset.recordsetTtl | int | 最小10、最大2147483647 | 必須 |  | ネームサーバーでレコードセット情報の更新周期 |
| recordset.recordList | List |  | 必須 |  | レコードリスト |
| recordset.recordList[0].recordDisabled | boolean |  | 任意 | false | レコードを無効にするかどうか |
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |

[レコードセットタイプ別の詳細フィールド]

- Aレコードセット
    - 複数のレコードを入力できます。
    - 1つのドメイン名に複数のIPv4アドレスを登録できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | 必須 |  | IPv4形式のアドレス |


- AAAAレコードセット
    - 複数のレコードを入力できます。
    - 1つのドメイン名に複数のIPv6アドレスを登録できます

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | 必須 |  | IPv6形式のアドレス |


- CAAレコードセット
    - 複数のレコードを入力できます。
    - ドメインに発行が許可されている認証機関(CA)を指定すると、許可されていない認証機関(CA)が証明書を発行することを防止できます。
    - issueタグは、ドメインまたはサブドメインに対する証明書発行権限です。
    - issuewildタグは、ドメインまたはサブドメインに対するワイルドカード証明書発行権限です。
        - issueタグとissuewildタグの設定方法は同じです。
        - 証明書発行許可：認証機関アドレスの入力、付加設定が必要な場合は、セミコロン(;)で区切って'名前=値'のペアで指定
        - 証明書発行禁止：セミコロン(;)入力
    - iodefタグは、認証機関(CA)が無効なリクエストを受け取った場合、設定されたメールまたはURLに通知します。
        - メール入力形式："mailto:*email-address*"
        - URL入力形式："http://*URL*"または"https://*URL*"
    - ユーザー指定タグは、認証機関(CA)でRFC標準外の付加機能をサポートする場合の設定です。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0または128 | 必須 |  | 定義されたタグの場合0、<br>ユーザー指定タグの場合128 |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>ユーザー指定タグ最大15 | 必須 |  | TAG_ISSUE：issueタグ、 <br>TAG_ISSUEWILD：issuewildタグ、 <br>TAG_IODEF：iodefタグ、 <br>ユーザー指定タグ |
| recordset.recordList[0].stringValue | String | 最大512文字(引用符号含む) | 必須 |  | タグに応じた内容 |


- CNAMEレコードセット
    - 1つのレコードを入力できます。
    - レコードセット名を正規名のカノニカル(canonical)で定義します。
    - 同じレコードセット名の他のレコードセットタイプがない場合は、CNAMEレコードセットを作成できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- MXレコードセット
    - 複数のレコードを入力できます。
    - ドメインに対するメールサーバーを指定します。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0、最大65535 | 必須 |  | 優先順位 |
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- NAPTRレコードセット
    - 複数のレコードを入力できます。
    - DDDS(Dynamic Delegation Discovery System)アプリケーションで、1つの値を別の値に変換または代替するために使用します。
    - 順序項目は、DDDSアプリケーションがレコードを評価する順序です。
    - 優先順序項目は、2個以上のレコードの順序項目が同じ場合、優先して評価する順序です。
    - 区分項目は、DDDSアプリケーション設定で空白、'S', 'A', 'U', 'P'を使用でき、それ以外の文字は予約されています。
    - サービス項目はDDDSアプリケーション設定で、詳細定義はRFC文書で確認できます。
        - URI DDDSアプリケーション[RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDSアプリケーション[RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDSアプリケーション[RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - 正規表現項目は、DDDSアプリケーションで入力値を出力値に変換するのに使用します。詳細定義は、[RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2)で確認できます。
    - 代替値項目は、DDDSアプリケーションがDNSクエリーを提出するドメイン名で入力値を代替します。正規表現項目を設定する場合は、'.'に設定します。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | 最小0、最大65535 | 必須 |  | 順序 |
| recordset.recordList[0].preference | int | 最小0、最大65535 | 必須 |  | 優先順序 |
| recordset.recordList[0].flags | String | 最大3文字(引用符号含む) | 必須 |  | 区分 |
| recordset.recordList[0].service | String | 最大257文字(引用符号含む) | 必須 |  | サービス |
| recordset.recordList[0].regexp | String | 最大257文字(引用符号含む) | 必須 |  | 正規表現 |
| recordset.recordList[0].replacement | String | 最大255文字 | 必須 |  | 代替値に'.'またはドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- PTRレコードセット
    - 複数のレコードを入力できます。
    - IPアドレスを利用してドメイン情報を照会する逆方向クエリ機能です。ISP業者にリクエストして設定する必要があります。
    - IPアドレスは、逆順でレコードセット名に入力する必要があります。(例) 127.0.0.1, 1.0.0.127.in-addr.arpa

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- TXTレコードセット
    - 複数のレコードを入力できます。
    - レコードセット名に対するテキストの内容を入力します。
    - TXTレコードセットタイプでSPFレコードを作成できます。
        - メール送信者のドメイン認証方式です。受信メールサーバーが送信メールサーバーとメールアドレスが一致しているかを確認する機能です。
        - 下記のような形式で入力でき、詳細定義は[RFC4408](https://tools.ietf.org/html/rfc4408)で確認できます。
        - 修飾子のデフォルト値は'+'で、メカニズムによってIP、ドメイン名などを追加で入力します。
            - 形式："v=spf1 {修飾子}{メカニズム}{内容} {変更者}={内容}"
            - 修飾子: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
            - メカニズム：all、include、a、mx、ptr、ip4、ip6、exists
            - 変更者：redirect、exp、ユーザー指定
            - (例)
                - "v=spf1 mx -all"
                - "v=spf1 ip4:192.168.0.1/16 -all"
                - "v=spf1 a:toast.com -all"
                - "v=spf1 redirect=toast.com"

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---------------------|---|---|---|
| recordset.recordList[0].stringValue | String | 最大4096バイト(引用符号含む) | 必須 |  | テキスト内容 |


- SRVレコードセット
    - 複数のレコードを入力できます。
    - 類似したTCP/IP基盤サービスを提供する複数のサーバーを、単一DNSクエリー動作を使用して検索できます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 最小0、最大65535 | 必須 |  | 優先順位 |
| recordset.recordList[0].weight | int | 最小0、最大65535 | 必須 |  | 重み |
| recordset.recordList[0].port | int | 最小0、最大65535 | 必須 |  | ポート |
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


- NSレコードセット
    - 複数のレコードを入力できます。
    - レコードセット名に対するネームサーバーを指定します。
    - レコードセット名はDNS Zone名のサブドメインでのみ、作成や修正ができます。

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 最大255文字 | 必須 |  | ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |


#### レスポンス

[レスポンス本文]

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


### レコードセット大量作成

- レコードセットを複数作成します。1回のリクエストで最大2,000個まで作成できます。
- **レコードセットタイプ**は、A、AAAA、CAA、CNAME、MX、NAPTR、PTR、TXT、SRV、NS、SOAをサポートします。
- SOAレコードセットは、作成、修正、削除できず、NSレコードセットは、**DNS Zone名**で作成、修正、削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。
  - TXTレコードセットは最大4096バイトです。
- DNS Zoneごとにレコードセットは、最大5,000個まで作成できます。
- レコードセット作成数は、制限されており、数を増やしたい場合は、別途お問い合わせください。 [1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDです。[DNS Zone照会](./api-guide/#dns-zone)から確認できます。
- レコード値は必須です。入力方法としてrecordset.recordList[0].recordContentフィールドまたは詳細フィールドを選択できます。
- recordContentフィールドは、スペースを区切り文字とします。詳細フィールドを1行で表示した内容です。詳細フィールドは、[レコードセット作成](./api-guide/#_14)の[レコードセットタイプに基づいた詳細フィールド]で確認できます。
- 詳細フィールドとrecordContentフィールドを同時に入力した場合、recordContentフィールドを基準に作成されます。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list' \
-H 'Content-Type: application/json' \
--data '{ "recordsetList": [{ "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }]}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordsetList | List |  | 必須 |  | レコードセットリスト |
| recordsetList[0].recordsetName | String | 最大254文字<br>英数字、(.)(-)(_)<br>(DNS Zone名を含む) | 必須 |  | 作成するレコードセット名、 <br>ドメインを[FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)で入力 |
| recordsetList[0].recordsetType | String | A、AAAA、CAA、CNAME、MX、<br>NAPTR、PTR、TXT、SRV、NS | 必須 |  | レコードセットタイプ |
| recordsetList[0].recordsetTtl | int | 10～2147483647 | 必須 |  | ネームサーバーでレコードセット情報の更新周期 |
| recordsetList[0].recordList | List |  | 必須 |  | レコードリスト |
| recordsetList[0].recordList[0].recordDisabled | boolean |  | 任意 | false | レコードが無効になっているかどうか |
| recordsetList[0].recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプに基づいた詳細フィールドを1行で表示した内容 |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### レコードセット修正

- レコードセットを修正します。
- **レコードセット名**は修正できず、**レコードセットタイプ**と**TTL(秒)**、**レコード値**は修正できます。
- SOAレコードセットは作成、修正、削除できず、NSレコードセットは**DNS Zone名**で作成、修正、削除できません。
- レコードセット内のレコードリストの長さは、最大512バイトです。
  - TXTレコードセットは最大4096バイトです。
  
#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- {recordsetId}はレコードセットIDで、[レコードセット照会](./api-guide/#_11)を通して確認できます。
- レコード値は必須で、入力方法にrecordset.recordList[0].recordContentフィールドまたは詳細フィールドを選択できます。
- recordContentフィールドは、空白を区切り文字にして詳細フィールドを1行で表示した内容です。詳細フィールドは、[レコードセット作成](./api-guide/#_14)に[レコードセットタイプ別の詳細フィールド]で確認できます。
- 詳細フィールドとrecordContentフィールドを同時に入力すると、recordContentフィールドを基準に修正されます。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordset | Object |  | 必須 |  | レコードセット |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | 必須 |  | レコードセットタイプ |
| recordset.recordsetTtl | int | 最小10、最大2147483647 | 必須 |  | ネームサーバーでレコードセット情報の更新周期 |
| recordset.recordList | List |  | 必須 |  | レコードリスト |
| recordset.recordList[0].recordDisabled | boolean |  | 必須 |  | レコードを無効にするかどうか |
| recordset.recordList[0].recordContent | String |  | 必須 |  | レコードセットタイプに応じて詳細フィールドを1行で表示した内容 |


#### レスポンス

[レスポンス本文]

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


### レコードセット削除

- 複数のレコードセットを削除し、レコードセットのレコードも一緒に削除します。
- SOAレコードセットは作成、修正、削除できず、NSレコードセットは**DNS Zone名**で作成、修正、削除できません。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {zoneId}はDNS Zone IDで、[DNS Zone照会](./api-guide/#dns-zone)を通して確認できます。
- レコードセットIDは、[レコードセット照会](./api-guide/#_11)を通して確認できます。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| recordsetIdList | List | 最小1個、最大3,000個 | 必須 |  | レコードセットIDリスト |

#### レスポンス

[レスポンス本文]

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

### GSLBの照会

- GSLBリストを照会します。
- Poolにヘルスチェックが接続されている場合は、GSLB正常状態、Pool正常状態、エンドポイント正常状態を確認できます。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?showHealthy=true'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| gslbIdList | List | 最大3,000個 | 任意 |  | GSLB IDリスト |
| searchGslbName | String |  | 任意 |  | 検索するGSLBの名前 |
| gslbDomain | String |  | 任意 |  | GSLBドメイン |
| showHealthy | boolean |  | 任意 |  | ヘルスチェック結果の表示 |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT、 <br>UPDATED_AT、 <br>GSLB_NAME、 <br>GSLB_DOMAIN、 <br>GSLB_TTL、 <br>GSLB_ROUTING_RULE、 <br>GSLB_DISABLED | 任意 | CREATED_AT | ソート対象 <br>(CREATED_AT：作成日、<br>UPDATED_AT：修正日、<br>GSLB_NAME：GSLBの名前、<br>GSLB_DOMAIN：GSLBドメイン、<br>GSLB_TTL：GSLBドメイン更新周期、<br>GSLB_ROUTING_RULE：ルーティングルール、<br>GSLB_DISABLED：GSLBが無効かどうか) |

#### レスポンス

[レスポンス本文]

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
                        // Pool情報省略
                    }
                },
                {
                    "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                    "connectedPoolOrder": 2,
                    "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                    "pool": {
                        // Pool情報省略
                    }
                }
            ],
            "createdAt": "2019-12-18T20:44:02.000+09:00",
            "updatedAt": "2019-12-18T21:01:05.000+09:00"
        }
    ]
}
```

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | GSLBの総数 |
| gslbList | List | Poolリスト |
| gslbList[0].gslbId | String | GSLB ID |
| gslbList[0].gslbName | String | GSLBの名前 |
| gslbList[0].gslbDomain | String | GSLBドメイン |
| gslbList[0].gslbTtl | String | GLSBドメイン更新周期 |
| gslbList[0].gslbRoutingRule | String | ルーティングルール |
| gslbList[0].gslbDisabled | boolean | GSLBが無効かどうか |
| gslbList[0].healthy | boolean | GSLBが正常かどうか |
| gslbList[0].connectedPoolList | List | 接続されたPoolリスト |
| gslbList[0].connectedPoolList[0].poolId | String | 接続されたPool Id |
| gslbList[0].connectedPoolList[0].pool | Object | 接続されたPool情報 |
| gslbList[0].connectedPoolList[0].connectedPoolOrder | int | 接続されたPoolの優先順位 |
| gslbList[0].connectedPoolList[0].connectedPoolRegionContent | String | 接続されたPoolの地域を1行で表示した内容 |
| gslbList[0].createdAt | DateTime | 作成日 |
| gslbList[0].updatedAt | DateTime | 修正日 |


### GSLBの作成

- GSLBとPoolの接続設定を作成します。
- **ルーティングルール**は、GSLBドメインのロードバランシング方法にFAILOVER、RANDOM、GEOLOCATIONを選択できます。
    - FAILOVER：接続されたPoolの優先順位でルーティングします。
    - RANDOM：接続されたPoolのうち、使用可能なPoolをランダムに選択してルーティングします。
    - GEOLOCATION：設定した地域のトラフィックを該当の接続されたPoolにルーティングします。地域設定がない場合は優先順位でルーティングします。
- **接続されたPool**の**優先順位**は、小さいほどルーティング順序が高く、重複した値は使用できません。
- GSLBの作成数とPoolの接続数は制限されています。拡張が必要な場合は別途お問い合わせください。[1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- connectedPoolRegionContentフィールドはカンマ(,)を区切り文字にして**地域**を1行で作成します。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbRoutingRule": "FAILOVER", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2 } ] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| gslb | Object |  | 必須 |  | GSLB |
| gslb.gslbName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | GSLBの名前 |
| gslb.gslbTtl | int |  | 必須 | false | GSLBドメイン更新周期 |
| gslb.gslbRoutingRule | String | FAILOVER、RANDOM、GEOLOCATION  | 必須 |  | ルーティングルール |
| gslb.gslbDisabled | boolean |  | 任意 | false | GSLBが無効かどうか |
| gslb.connectedPoolList | List |  | 任意 |  | 接続されたPoolリスト |
| gslb.connectedPoolList[0].poolId | String |  | 必須 |  | 接続されたPool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 最小1、最大2,147,483,647 | 必須 |  | 接続されたPoolの優先順位 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 任意 |  | 接続されたPoolの地域設定 |

#### レスポンス

[レスポンス本文]

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
                    // Pool情報省略
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "pool": {
                    // Pool情報省略
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:44:03.000+09:00"
    }
}
```


### GSLBの修正

- GSLBとPool接続設定を修正します。
- [GSLB作成](./api-guide/#gslb_1)で入力した項目を修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {gslbId}はGSLB IDで、[GSLB照会](./api-guide/#gslb)を通して確認できます。
- connectedPoolRegionContentフィールドはカンマ(,)を区切り文字にして**地域**を1行で作成します。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbDisabled": true, "gslbRoutingRule": "GEOLOCATION", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2, "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA" } ] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| gslb | Object |  | 必須 |  | GSLB |
| gslb.gslbName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | GSLBの名前 |
| gslb.gslbTtl | int |  | 必須 | false | GSLBドメイン更新周期 |
| gslb.gslbRoutingRule | String | FAILOVER、RANDOM、GEOLOCATION  | 必須 |  | ルーティングルール |
| gslb.gslbDisabled | boolean |  | 任意 | false | GSLBが無効かどうか |
| gslb.connectedPoolList | List |  | 任意 |  | 接続されたPoolリスト |
| gslb.connectedPoolList[0].poolId | String |  | 必須 |  | 接続されたPool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 最小1、最大2,147,483,647 | 必須 |  | 接続されたPoolの優先順位 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 任意 |  | 接続されたPoolの地域設定 |

#### レスポンス

[レスポンス本文]

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
                    // Pool情報省略
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                "pool": {
                    // Pool情報省略
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:59:49.000+09:00"
    }
}
```


### GSLBの削除

- 複数のGSLBを削除します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?
gslbIdList=91de0c6f-aeaa-44ec-b361-822acfcd5921,269eff10-f3c0-4b11-b072-ec53e7c604bf'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| gslbIdList | List | 最小1個、最大3,000個 | 必須 |  | GSLB IDリスト |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### Pool接続

- GSLBにPoolを接続します。
- **接続されたPool**の**優先順位**は、小さいほどルーティング順序が高く、既に接続されたPoolと同じ優先順位を入力した場合、既存Poolのルーティング順序が低くなります。
- Pool接続数は制限されています。拡張が必要な場合は別途お問い合わせください。[1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {gslbId}はGSLB IDです。[GSLB照会](./api-guide/#gslb)で確認できます。
- {poolId}はPool IDです。[Pool照会](./api-guide/#pool_3)で確認できます。
- connectedPoolRegionContentフィールドはカンマ(,)を区切り文字にして**地域**を1行で作成します。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1 } }'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 必須 |  | 接続されたPool |
| connectedPool.connectedPoolOrder | int | 最小1、最大2,147,483,647 | 必須 |  | 接続されたPoolの優先順位 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 任意 |  | 接続されたPoolの地域設定 |

#### レスポンス

[レスポンス本文]

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
                // Pool情報省略
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool情報省略
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool情報省略
            }
        }
    ]
}
```

### Pool接続の修正

- GSLBに接続されたPoolの設定を修正します。
- [GSLB作成](./api-guide/#gslb_1)のPool設定または[Pool接続](./api-guide/#pool)で入力した項目を修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {gslbId}はGSLB IDでです。[GSLB照会](./api-guide/#gslb)で確認できます。
- {poolId}はPool IDです。[Pool照会](./api-guide/#pool_3)で確認できます。
- connectedPoolRegionContentフィールドはカンマ(,)を区切り文字にして**地域**を1行で作成します。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1, "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA" } }'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 必須 |  | 接続されたPool |
| connectedPool.connectedPoolOrder | int | 最小1、最大2,147,483,647 | 必須 |  | 接続されたPoolの優先順位 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 任意 |  | 接続されたPoolの地域設定 |

#### レスポンス

[レスポンス本文]

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
                // Pool情報省略
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool情報省略
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool情報省略
            }
        }
    ]
}
```

### Poolの接続解除

- GSLBに接続された複数のPoolを解除します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {gslbId}はGSLB IDです。[GSLB照会](./api-guide/#gslb)で確認できます。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools?
poolIdList=52da0e48-9062-43f7-bef8-8aec4b795bfe,12bc396a-eb97-4a6b-ab4c-73d1a1dfb093'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| poolIdList | List | 最小1個、最大3,000個 | 必須 |  | Pool IDリスト |

#### レスポンス

[レスポンス本文]

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
                // Pool情報省略
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool情報省略
            }
        }
    ]
}
```


## Pool API

### Poolの照会

- Poolリストを照会します。
- ヘルスチェックが接続されている場合は、Pool正常状態とエンドポイント正常状態を確認できます。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?showHealthy=true'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| poolIdList | List | 最大3,000個 | 任意 |  | Pool IDリスト |
| searchPoolName | String |  | 任意 |  | 検索するPoolの名前 |
| healthCheckId | String |  | 任意 |  | 接続されたヘルスチェックID |
| showHealthy | boolean |  | 任意 |  | ヘルスチェック結果を表示するかどうか |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC、ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT、 <br>UPDATED_AT、 <br>POOL_NAME、 <br>POOL_DISABLED、 <br>HEALTH_CHECK_ID | 任意 | CREATED_AT | ソート対象 <br>(CREATED_AT：作成日、<br>UPDATED_AT：修正日、<br>POOL_NAME：Poolの名前、<br>POOL_DISABLED：Poolが無効かどうか、<br>HEALTH_CHECK_ID：接続されたヘルスチェックID) |

#### レスポンス

[レスポンス本文]

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
                // ヘルスチェック情報省略
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

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | Pool総数 |
| poolList | List | Poolリスト |
| poolList[0].poolId | String | Pool ID |
| poolList[0].poolName | String | Poolの名前 |
| poolList[0].poolDisabled | boolean | Poolが無効かどうか |
| poolList[0].healthy | boolean | Poolが正常かどうか |
| poolList[0].healthCheckId | String | 接続されたヘルスチェックID |
| poolList[0].healthCheck | Object | 接続されたヘルスチェック情報 |
| poolList[0].endpointList | List | エンドポイントリスト |
| poolList[0].endpointList[0].endpointAddress | String | エンドポイントアドレス |
| poolList[0].endpointList[0].endpointWeight | double | エンドポイントの重み |
| poolList[0].endpointList[0].healthy | boolean | エンドポイントが正常かどうか |
| poolList[0].endpointList[0].failureReason | String | エンドポイントが異常な理由 |
| poolList[0].createdAt | DateTime | 作成日 |
| poolList[0].updatedAt | DateTime | 修正日 |


### Poolの作成

- PoolとPool内にエンドポイントを作成します。
- Pool内のエンドポイントのアクセシビリティを確認する**ヘルスチェック**を設定できます。
- **エンドポイントアドレス**はドメインアドレスまたはIPv4で入力することができ、制限された入力があります。
    - 最初の文字にハイフン、ドットは使用できず、最後の文字にハイフンは使用できません。またドットとハイフンは続けて入力できません。
    - [予約済のIPアドレス](https://en.wikipedia.org/wiki/Reserved_IP_addresses)は入力できません。
    - Pool内で重複してはいけません。
- エンドポイントの**重み**はPool内の他のエンドポイントの重みと相対的に動作します。同じ重みはPool内で同じ比重を持ちます。
- Poolの作成数、Pool内のエンドポイント数、エンドポイントの総数は制限されています。拡張が必要な場合は別途お問い合わせください。[1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "endpointList": [ { "endpointAddress": "test.dnsplus.com" }, { "endpointAddress": "123.123.123.123" } ] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| pool | Object |  | 必須 |  | Pool |
| pool.poolName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | Poolの名前 |
| pool.poolDisabled | boolean |  | 任意 | false | Poolが無効かどうか |
| pool.healthCheckId | String |  | 任意 |  | ヘルスチェックID |
| pool.endpointList | List |  | 必須 |  | エンドポイントリスト |
| pool.endpointList[0].endpointAddress | String | 最大254文字、<br>小文字と数字、(.-_) | 必須 |  | エンドポイントアドレス |
| pool.endpointList[0].endpointWeight | double | 最小0、最大1.00 | 任意 | 1.00 | エンドポイントの重み |
| pool.endpointList[0].endpointDisabled | boolean |  | 任意 | false | エンドポイントが無効かどうか |

#### レスポンス

[レスポンス本文]

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


### Poolの修正

- PoolとPool内のエンドポイントを修正します。
- [Pool作成](./api-guide/#pool_4)で入力した項目を修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {poolId}はPool IDです。[Pool照会](./api-guide/#pool_3)で確認できます。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "poolDisabled": true, "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17", "endpointList": [ { "endpointAddress": "test.dnsplus.com", "endpointWeight": 1.00, "endpointDisabled": true }, { "endpointAddress": "123.123.123.123", "endpointWeight": 0.5, "endpointDisabled": true } ] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| pool | Object |  | 必須 |  | Pool |
| pool.poolName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | Poolの名前 |
| pool.poolDisabled | boolean |  | 任意 | false | Poolが無効かどうか |
| pool.healthCheckId | String |  | 任意 |  | ヘルスチェックID |
| pool.endpointList | List |  | 必須 |  | エンドポイントリスト |
| pool.endpointList[0].endpointAddress | String | 最大254文字、<br>小文字と数字、(.-_) | 必須 |  | エンドポイントアドレス |
| pool.endpointList[0].endpointWeight | double | 最小0、最大1.00 | 任意 | 1.00 | エンドポイントの重み |
| pool.endpointList[0].endpointDisabled | boolean |  | 任意 | false | エンドポイントが無効かどうか |

#### レスポンス

[レスポンス本文]

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
            // ヘルスチェック情報省略
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


### Poolの削除

- 複数のPoolを削除し、Poolのエンドポイントも一緒に削除します。
- GSLBに接続されているPoolは削除できません。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?
poolIdList=8e4326d4-3862-4b46-819e-83a786add570,2f89d3fe-03bc-4711-826e-db2c89c12818'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| poolIdList | List | 最小1個、最大3,000個 | 必須 |  | Pool IDリスト |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## ヘルスチェックAPI

### ヘルスチェックの照会

- ヘルスチェックリストを照会します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks'
```

[オプション]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 最大3,000個 | 任意 |  | ヘルスチェックIDリスト |
| searchHealthCheckName | String |  | 任意 |  | 検索するヘルスチェックの名前 |
| page | int | 最小1 | 任意 | 1 | ページ番号 |
| limit | int | 最小1、最大3,000 | 任意 | 50 | 照会数 |
| sortDirection | String | DESC, ASC | 任意 | DESC | ソート方向(DESC：降順、ASC：昇順) |
| sortKey | String | CREATED_AT、 <br>UPDATED_AT、 <br>HEALTH_CHECK_NAME、 <br>PROTOCOL、 <br>PORT | 任意 | CREATED_AT | ソート対象 <br>(CREATED_AT：作成日、<br>UPDATED_AT：修正日、<br>HEALTH_CHECK_NAME：ヘルスチェック名前、<br>PROTOCOL：プロトコル、<br>PORT：ポート) |

#### レスポンス

[レスポンス本文]

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
            "port": 443,
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

[フィールド]

| 名前 | タイプ | 説明 |
|---|---|---|
| totalCount | long | ヘルスチェック総数 |
| healthCheckList | List | ヘルスチェックリスト |
| healthCheckList[0].healthCheckId | String | ヘルスチェックID |
| healthCheckList[0].healthCheckName | String | ヘルスチェックの名前 |
| healthCheckList[0].protocol | String | プロトコル |
| healthCheckList[0].port | int | ポート |
| healthCheckList[0].interval | int | ヘルスチェック周期 |
| healthCheckList[0].timeout | int | 最大レスポンス待機時間 |
| healthCheckList[0].retries | int | 最大再試行回数 |
| healthCheckList[0].path | String | パス |
| healthCheckList[0].expectedCodes | String | 予想ステータスコード |
| healthCheckList[0].expectedBody | String | 予想レスポンス本文 |
| healthCheckList[0].allowInsecure | boolean | 証明書検証しない |
| healthCheckList[0].requestHeaderList | List | リクエストヘッダリスト |
| healthCheckList[0].requestHeaderList[0] | Object | リクエストヘッダ名、値オブジェクト |
| healthCheckList[0].createdAt | DateTime | 作成日 |
| healthCheckList[0].updatedAt | DateTime | 修正日 |


### ヘルスチェックの作成

- ヘルスチェックを作成します。
- ヘルスチェック**プロトコル**はHTTPS、HTTP、TCPをサポートし、選択したプロトコルによって入力できる情報が異なります。
    - HTTPS入力可能項目：証明書検証しない、ポート、ヘルスチェック周期、最大レスポンス待機時間、最大再試行回数、パス、予想ステータスコード、予想レスポンス本文、リクエストヘッダ
    - HTTP入力可能項目：ポート、ヘルスチェック周期、最大レスポンス待機時間、最大再試行回数、パス、予想ステータスコード、予想レスポンス本文、リクエストヘッダ
    - TCP入力可能項目:ポート、ヘルスチェック周期、最大レスポンス待機時間、最大再試行回数
- **証明書検証しない**を使用すると、ヘルスチェックが実行される時、エンドポイントのTLS/SSL証明書が無効でも無視できます。
- **予想ステータスコード**と**予想レスポンス本文**を判断する時、エンドポイントからリダイレクトされたページについてはサポートしません。
- ヘルスチェックの作成数は制限されています。拡張が必要な場合は別途お問い合わせください。[1:1お問い合わせ](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks' \
-H 'Content-Type: application/json' \
'--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "2xx", "allowInsecure": false, "requestHeaderList": [{ "Host": "nhncloud.com" }] }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 必須 |  | ヘルスチェック |
| healthCheck.healthCheckName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | ヘルスチェックの名前 |
| healthCheck.protocol | String | HTTPS、HTTP、TCP | 必須 |  | ヘルスチェック実行プロトコル |
| healthCheck.port | int | 最小1、最大65535 | 必須 |  | ヘルスチェック実行ポート |
| healthCheck.interval | int | 最小10または(retries+1)*timeout、最大3600 | 任意 | 60 | ヘルスチェック周期 |
| healthCheck.timeout | int | 最小1、最大10 | 任意 | 5 | 最大レスポンス待機時間 |
| healthCheck.retries | int | 最小0、最大5 | 任意 | 2 | 最大再試行回数 |
| healthCheck.path | String | 最大254文字、<br>最初の文字'/' | 任意 |  | ヘルスチェック実行パス、<br>HTTPS/HTTPの時に使用 |
| healthCheck.expectedCodes | String | 数字とワイルドカード'x' | 任意 |  | ヘルスチェック予想ステータスコード、<br>HTTPS/HTTPの時に使用<br>(例) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 最大10KB | 任意 |  | ヘルスチェック予想レスポンス本文、<br>HTTPS/HTTPの時に使用 |
| healthCheck.allowInsecure | boolean |  | 任意 |  | ヘルスチェック証明書検証しない、<br>HTTPSの時に使用 |
| healthCheck.requestHeaderList | List |  | 任意 |  | リクエストヘッダリスト、<br>HTTPS, HTTPの場合に使用、<br> リスト内の項目は`{ "ヘッダ名": "ヘッダ値" }`の形でリクエスト |

#### レスポンス

[レスポンス本文]

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


### ヘルスチェックの修正

- ヘルスチェックを修正します。
- [ヘルスチェック作成](./api-guide/#_48)で入力した項目を修正します。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId} |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。
- {healthCheckId}はヘルスチェックIDです。[ヘルスチェック照会](./api-guide/#_45)で確認できます。

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId}' \
-H 'Content-Type: application/json' \
'--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "3xx", "allowInsecure": false }}'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 必須 |  | ヘルスチェック |
| healthCheck.healthCheckName | String | 最大100文字、<br>英字(大文字/小文字)、数字、(-)、(_) | 必須 |  | ヘルスチェックの名前 |
| healthCheck.protocol | String | HTTPS、HTTP、TCP | 必須 |  | ヘルスチェック実行プロトコル |
| healthCheck.port | int | 最小1、最大65535 | 必須 |  | ヘルスチェック実行ポート |
| healthCheck.interval | int | 最小10または(retries+1)*timeout、最大3600 | 任意 | | ヘルスチェック周期 |
| healthCheck.timeout | int | 最小1、最大10 | 任意 | | 最大レスポンス待機時間 |
| healthCheck.retries | int | 最小0、最大5 | 任意 | | 最大再試行回数 |
| healthCheck.path | String | 最大254文字、<br>最初の文字'/' | 任意 |  | ヘルスチェック実行パス、<br>HTTPS/HTTPの時に使用 |
| healthCheck.expectedCodes | String | 数字とワイルドカード'x' | 任意 |  | ヘルスチェック予想ステータスコード、<br>HTTPS/HTTPの時に使用<br>(例) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 最大10KB | 任意 |  | ヘルスチェック予想レスポンス本文、<br>HTTPS/HTTPの時に使用 |
| healthCheck.allowInsecure | boolean |  | 任意 |  | ヘルスチェック証明書を検証しない、<br>HTTPSの時に使用 |
| healthCheck.requestHeaderList | List |  | 任意 |  | リクエストヘッダリスト、<br>HTTPS、HTTPの場合に使用、<br> リスト内の項目は`{ "ヘッダ名": "ヘッダ値" }`の形式でリクエスト |

#### レスポンス

[レスポンス本文]

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


### ヘルスチェックの削除

- 複数のヘルスチェックを削除します。
- Poolに接続されているヘルスチェックは削除できません。

#### リクエスト

[URI]

| メソッド | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[リクエスト本文]

- {appkey}はコンソールで確認した値に変更します。

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks?
healthCheckIdList=b9165853-7859-4309-8059-48f12ebdbc17,d2629d6b-9381-4645-9cf3-43d7ad491e2b'
```

[フィールド]

| 名前 | タイプ | 有効範囲 | 必須かどうか | デフォルト値 | 説明 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 最小1個、最大3,000個 | 必須 |  | ヘルスチェックIDリスト |

#### レスポンス

[レスポンス本文]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
