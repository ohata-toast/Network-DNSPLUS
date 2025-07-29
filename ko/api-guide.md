## Network > DNS Plus > API 가이드

DNS Plus 서비스의 API를 설명합니다.


## API 공통 정보

### 사전 준비

- API를 사용하려면 앱키가 필요합니다.
- 앱키는 콘솔 상단 **URL & Appkey** 메뉴에서 확인이 가능합니다.

### 응답 공통 정보

- 모든 API 요청에 '200 OK'로 응답합니다. 자세한 응답 결과는 응답 본문의 헤더를 참고하세요.

[성공 응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```

[실패 응답 본문]

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

### DNS Zone 조회

- DNS Zone 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최대 3,000개 | 선택 |  | DNS Zone ID 목록 |
| zoneStatusList | List | CREATING, <br>DELETING, <br>DELETING_FAIL, <br> USE | 선택 | | DNS Zone 상태 목록 <br>(CREATING: 생성 중, <br>DELETING: 삭제 중, <br>DELETING_FAIL: 삭제 실패, <br>USE: 사용) |
| searchZoneName | String |  | 선택 |  | 검색할 DNS Zone 이름 |
| engineId | String | | 선택 |  | DNS 서버 ID |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>ZONE_NAME, <br>ZONE_STATUS, <br>RECORDSET_COUNT | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>ZONE_NAME: DNS Zone 이름, <br>ZONE_STATUS: DNS Zone 상태, <br>RECORDSET_COUNT: 레코드 세트 개수) |

#### 응답

[응답 본문]

```
{
    "header": {
        // 생략
    },
    "totalCount": 1,
    "zoneList": [
        {
            "engineId": "e13a1bcf0aa8e07f6a4fae94ed869c39",
            "zoneId": "bff20a9a-24cf-4670-8b34-007622ec010e",
            "zoneName": "test.dnsplus.com.",
            "zoneStatus": "USE",
            "description": "테스트",
            "createdAt": "2019-06-04T12:32:50.000+09:00",
            "updatedAt": "2019-06-04T12:32:50.000+09:00",
            "recordsetCount": 2
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 DNS Zone 개수 |
| zoneList | List | DNS Zone 목록 |
| zoneList[0].engineId | boolean | DNS 서버 ID |
| zoneList[0].zoneId | String | DNS Zone ID |
| zoneList[0].zoneName | String | DNS Zone 이름 |
| zoneList[0].zoneStatus | String | DNS Zone 상태 |
| zoneList[0].description | String | 설명 |
| zoneList[0].createdAt | DateTime | 생성일 |
| zoneList[0].updatedAt | DateTime | 수정일 |
| zoneList[0].recordsetCount | long | 레코드 세트 개수 |


### DNS Zone 생성

- DNS Zone을 생성합니다.
- **DNS Zone 이름**은 DNS 서버에서 유일해야 합니다.
- 동일한 **DNS Zone 이름**은 DNS 서버 수만큼 생성 가능합니다. DNS 서버는 3대입니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "zoneName": "test.dnsplus.com.", "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zone | Object |  | 필수 |  | DNS Zone |
| zone.zoneName | String | 최대 254자<br>영소문자와 숫자, '.', '-', '_'<br>마지막 문자 '.' | 필수 |  | 생성할 DNS Zone 이름, <br>도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |
| zone.description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

#### 응답

[응답 본문]

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


### DNS Zone 수정

- DNS Zone을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}' \
-H 'Content-Type: application/json' \
--data '{ "zone": { "description": "test" }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zone | Object |  | 필수 |  | DNS Zone |
| zone.description | String | 최대 255자 | 선택 |  | DNS Zone 설명 |

#### 응답

[응답 본문]

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


### DNS Zone 삭제 (비동기)

- 여러 개의 DNS Zone을 삭제하며, DNS Zone의 레코드 세트도 함께 삭제합니다.
- 실제 데이터 삭제는 비동기로 처리됩니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- DNS Zone ID는 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/async?
zoneIdList=bff20a9a-24cf-4670-8b34-007622ec010e,52bc0031-37eb-4b82-b4d7-eaab24188dc4'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| zoneIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | DNS Zone ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## 레코드 세트 API

### 레코드 세트 조회

- 레코드 세트 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordsetIdList | List | 최대 3,000개 | 선택 |  | 레코드 세트 목록 |
| recordsetTypeList | List | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS, SOA | 선택 | | 레코드 세트 타입 목록 |
| searchRecordsetName | String |  | 선택 |  | 검색할 레코드 세트 이름 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>RECORDSET_NAME, <br>RECORDSET_TYPE, <br>RECORDSET_TTL | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>RECORDSET_NAME: 레코드 세트 이름, <br>RECORDSET_TYPE: 레코드 세트 타입, <br>RECORDSET_TTL: TTL(초)) |

#### 응답

[응답 본문]

```
{
    "header": {
        // 생략
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
                    // 생략: 레코드 세트 타입에 따라 다름
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
                    // 생략: 레코드 세트 타입에 따라 다름
                },
                {
                    "recordDisabled": false,
                    "recordContent": "ns.toastdns-jin.net.",
                    // 생략: 레코드 세트 타입에 따라 다름
                }
            ]
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 레코드 세트 개수 |
| recordsetList | List | 레코드 세트 목록 |
| recordsetList[0].recordsetId | String | 레코드 세트 ID |
| recordsetList[0].recordsetName | String | 레코드 세트 이름 |
| recordsetList[0].recordsetType | String | 레코드 세트 타입 |
| recordsetList[0].recordsetTtl | int | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordsetList[0].recordsetStatus | String | 레코드 세트 상태 |
| recordsetList[0].createdAt | DateTime | 생성일 |
| recordsetList[0].updatedAt | DateTime | 수정일 |
| recordsetList[0].recordList | List | 레코드 목록 |
| recordsetList[0].recordList[0].recordDisabled | boolean | 레코드 비활성화 여부 |
| recordsetList[0].recordList[0].recordContent | String | 레코드값이며 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |


### 레코드 세트 생성

- 레코드 세트를 생성합니다.
- **레코드 세트 타입**으로 A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, SOA를 지원합니다.
- SOA 레코드 세트는 생성, 수정, 삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성, 수정, 삭제할 수 없습니다.
- 레코드 세트 내의 레코드 목록의 길이는 최대 512바이트입니다.
- DNS Zone당 레코드 세트는 최대 5,000개까지 생성할 수 있습니다.
- 레코드 세트 생성 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- 레코드값은 필수이며 입력 방법으로 recordset.recordList[0].recordContent 필드 또는 상세 필드를 선택할 수 있습니다.
- recordContent 필드는 공백을 구분 문자로 하여 상세 필드를 한 줄로 표시한 내용입니다. 상세 필드는 [레코드 세트 타입에 따른 상세 필드]에서 확인할 수 있습니다.
- 상세 필드와 recordContent 필드를 동시에 입력하면 recordContent 필드를 기준으로 생성됩니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset | Object |  | 필수 |  | 레코드 세트 |
| recordset.recordsetName | String | 최대 254자<br>영소문자와 숫자, '.', '-', '_'<br>(DNS Zone 이름 포함) | 필수 |  | 생성할 레코드 세트 이름, <br>도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | 필수 |  | 레코드 세트 타입 |
| recordset.recordsetTtl | int | 최소 10, 최대 2147483647 | 필수 |  | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordset.recordList | List |  | 필수 |  | 레코드 목록 |
| recordset.recordList[0].recordDisabled | boolean |  | 선택 | false | 레코드 비활성화 여부 |
| recordset.recordList[0].recordContent | String |  | 필수 |  | 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |

[레코드 세트 타입에 따른 상세 필드]

- A 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 하나의 도메인명에 여러 개의 IPv4 주소를 등록할 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV4 | String |  | 필수 |  | IPv4 형식의 주소 |


- AAAA 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 하나의 도메인명에 여러 개의 IPv6 주소를 등록할 수 있습니다

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].ipV6 | String |  | 필수 |  | IPv6 형식의 주소 |


- CAA 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 도메인에 발급이 허용된 인증 기관(CA)을 지정하면 허용되지 않은 인증 기관(CA)이 인증서를 발급하는 것을 방지할 수 있습니다.
    - issue 태그는 도메인 또는 하위 도메인에 대한 인증서 발행 권한입니다.
    - issuewild 태그는 도메인 또는 하위 도메인에 대한 와일드카드 인증서 발행 권한입니다.
        - issue 태그와 issuewild 태그의 설정 방법은 동일합니다.
        - 인증서 발행 허용: 인증 기관 주소 입력, 부가 설정이 필요한 경우 세미콜론(;)으로 분리하고 '이름=값' 쌍으로 지정
        - 인증서 발행 금지: 세미콜론(;) 입력
    - iodef 태그는 인증 기관(CA)이 잘 못 된 요청을 받을 경우 설정된 이메일 또는 URL 주소로 알립니다.
        - 메일 입력 형식: "mailto:*email-address*"
        - URL 주소 입력 형식: "http://*URL*" 또는 "https://*URL*"
    - 사용자 지정 태그는 인증 기관(CA)에서 RFC 표준 외의 부가 기능을 지원하는 경우의 설정입니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].flags | int | 0 또는 128 | 필수 |  | 정의된 태그인 경우 0, <br>사용자 지정 태그인 경우 128 |
| recordset.recordList[0].tag | String | TAG_ISSUE, <br>TAG_ISSUEWILD, <br>TAG_IODEF, <br>사용자 지정 태그 최대 15 | 필수 |  | TAG_ISSUE: issue 태그, <br>TAG_ISSUEWILD: issuewild 태그, <br>TAG_IODEF: iodef 태그, <br>사용자 지정 태그 |
| recordset.recordList[0].stringValue | String | 최대 512자(인용부호 포함) | 필수 |  | 태그에 따른 내용 |


- CNAME 레코드 세트
    - 하나의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름을 정규 이름의 별칭(canonical)으로 정의합니다.
    - 동일한 레코드 세트 이름에 대하여 다른 레코드 세트 타입이 없는 경우 CNAME 레코드 세트를 생성할 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- MX 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 도메인에 대한 메일 서버를 지정합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 최소 0, 최대 65535 | 필수 |  | 우선순위 |
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- NAPTR 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - DDDS(Dynamic Delegation Discovery System) 애플리케이션에서 하나의 값을 다른 값으로 변환하거나 대체하기 위해 사용합니다.
    - 순서 항목은 DDDS 애플리케이션이 레코드를 평가하는 순서입니다.
    - 선호 순서 항목은 두 개 이상의 레코드가 순서 항목이 동일한 경우 우선하여 평가하는 순서입니다.
    - 구분 항목은 DDDS 애플리케이션 설정으로 공백, 'S', 'A', 'U', 'P'를 사용할 수 있으며 그 외의 문자는 예약되어 있습니다.
    - 서비스 항목은 DDDS 애플리케이션 설정으로 상세 정의는 RFC 문서에서 확인할 수 있습니다.
        - URI DDDS 애플리케이션 [RFC 3404#section-4.4](https://tools.ietf.org/html/rfc3404#section-4.4)
        - S-NAPTR DDDS 애플리케이션 [RFC 3958#section-6.5](https://tools.ietf.org/html/rfc3958#section-6.5)
        - U-NAPTR DDDS 애플리케이션 [RFC 4848#section-4.5](https://tools.ietf.org/html/rfc4848#section-4.5)
    - 정규식 항목은 DDDS 애플리케이션에서 입력값을 출력값으로 변환하는 데 사용합니다. 상세 정의는 [RFC 3402#section-3.2](https://tools.ietf.org/html/rfc3402#section-3.2)에서 확인할 수 있습니다.
    - 대체값 항목은 DDDS 애플리케이션이 DNS 쿼리를 제출할 도메인 이름으로 입력값을 대체합니다. 정규식 항목을 설정하는 경우 '.'로 설정합니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].order | int | 최소 0, 최대 65535 | 필수 |  | 순서 |
| recordset.recordList[0].preference | int | 최소 0, 최대 65535 | 필수 |  | 선호 순서 |
| recordset.recordList[0].flags | String | 최대 3자(인용부호 포함) | 필수 |  | 구분 |
| recordset.recordList[0].service | String | 최대 257자(인용부호 포함) | 필수 |  | 서비스 |
| recordset.recordList[0].regexp | String | 최대 257자(인용부호 포함) | 필수 |  | 정규식 |
| recordset.recordList[0].replacement | String | 최대 255자 | 필수 |  | 대체값으로 '.' 또는 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- PTR 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - IP 주소를 이용해서 도메인 정보를 조회하는 역방향 질의 기능입니다. ISP 업체에 요청하여 설정해야 합니다.
    - IP 주소는 역순으로 레코드 세트 이름에 입력해야 합니다. (예제) 127.0.0.1, 1.0.0.127.in-addr.arpa

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- TXT 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름에 대한 텍스트 내용을 입력합니다.
    - TXT 레코드 세트 타입으로 SPF 레코드를 생성할 수 있습니다.
        - 이메일 발신자 도메인 인증 방식으로 수신 메일 서버가 발송 메일 서버와 메일 주소가 일치하는지 확인하는 기능입니다.
        - 아래와 같은 형태로 입력 가능하며 상세 정의는 [RFC4408](https://tools.ietf.org/html/rfc4408)에서 확인할 수 있습니다.
        - 수식자의 기본값은 '+'이며 메커니즘에 따라 IP, 도메인 이름 등을 추가로 입력합니다.
            - 형태: "v=spf1 {수식자}{메커니즘}{내용} {변경자}={내용}"
            - 수식자: '+'(Pass), '-'(Fail), '~'(Soft Fail), '?'(Neutral)
            - 메커니즘: all, include, a, mx, ptr, ip4, ip6, exists
            - 변경자: redirect, exp, 사용자 지정
            - (예제)
                - "v=spf1 mx -all"
                - "v=spf1 ip4:192.168.0.1/16 -all"
                - "v=spf1 a:toast.com -all"
                - "v=spf1 redirect=toast.com"

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].stringValue | String | 최대 255바이트(인용부호 포함) | 필수 |  | 텍스트 내용 |


- SRV 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 유사한 TCP/IP 기반 서비스를 제공하는 여러 서버를 단일 DNS 쿼리 동작을 사용하여 찾을 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].priority | int | 최소 0, 최대 65535 | 필수 |  | 우선순위 |
| recordset.recordList[0].weight | int | 최소 0, 최대 65535 | 필수 |  | 가중치 |
| recordset.recordList[0].port | int | 최소 0, 최대 65535 | 필수 |  | 포트 |
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


- NS 레코드 세트
    - 여러 개의 레코드를 입력할 수 있습니다.
    - 레코드 세트 이름에 대한 네임 서버를 지정합니다.
    - 레코드 세트 이름은 DNS Zone 이름의 하위 도메인으로만 생성이나 수정할 수 있습니다.

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset.recordList[0].domainName | String | 최대 255자 | 필수 |  | 도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |


#### 응답

[응답 본문]

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


### 레코드 세트 대량 생성

- 레코드 세트를 여러 개 생성합니다. 요청당 최대 2,000개까지 생성할 수 있습니다.
- **레코드 세트 타입**으로 A, AAAA, CAA, CNAME, MX, NAPTR, PTR, TXT, SRV, NS, SOA를 지원합니다.
- SOA 레코드 세트는 생성, 수정, 삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성, 수정, 삭제할 수 없습니다.
- 레코드 세트 내의 레코드 목록의 길이는 최대 512바이트입니다.
- DNS Zone당 레코드 세트는 최대 5,000개까지 생성할 수 있습니다.
- 레코드 세트 생성 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- 레코드값은 필수이며 입력 방법으로 recordset.recordList[0].recordContent 필드 또는 상세 필드를 선택할 수 있습니다.
- recordContent 필드는 공백을 구분 문자로 하여 상세 필드를 한 줄로 표시한 내용입니다. 상세 필드는 [레코드 세트 생성](./api-guide/#_14)에 [레코드 세트 타입에 따른 상세 필드]에서 확인할 수 있습니다.
- 상세 필드와 recordContent 필드를 동시에 입력하면 recordContent 필드를 기준으로 생성됩니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/list' \
-H 'Content-Type: application/json' \
--data '{ "recordsetList": [{ "recordsetName": "sub.test.dnsplus.com.", "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }]}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordsetList | List |  | 필수 |  | 레코드 세트 목록 |
| recordsetList[0].recordsetName | String | 최대 254자<br>영소문자와 숫자, '.', '-', '_'<br>(DNS Zone 이름 포함) | 필수 |  | 생성할 레코드 세트 이름, <br>도메인을 [FQDN](https://en.wikipedia.org/wiki/Fully_qualified_domain_name)으로 입력 |
| recordsetList[0].recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | 필수 |  | 레코드 세트 타입 |
| recordsetList[0].recordsetTtl | int | 최소 10, 최대 2147483647 | 필수 |  | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordsetList[0].recordList | List |  | 필수 |  | 레코드 목록 |
| recordsetList[0].recordList[0].recordDisabled | boolean |  | 선택 | false | 레코드 비활성화 여부 |
| recordsetList[0].recordList[0].recordContent | String |  | 필수 |  | 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### 레코드 세트 수정

- 레코드 세트를 수정합니다.
- **레코드 세트 이름**은 수정할 수 없으며, **레코드 세트 타입**과 **TTL(초)**, **레코드값**은 수정할 수 있습니다.
- SOA 레코드 세트는 생성, 수정, 삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성, 수정, 삭제할 수 없습니다.
- 레코드 세트 내의 레코드 목록의 길이는 최대 512바이트입니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- {recordsetId}는 레코드 세트 ID이며 [레코드 세트 조회](./api-guide/#_11)를 통해서 알 수 있습니다.
- 레코드값은 필수이며 입력 방법으로 recordset.recordList[0].recordContent 필드 또는 상세 필드를 선택할 수 있습니다.
- recordContent 필드는 공백을 구분 문자로 하여 상세 필드를 한 줄로 표시한 내용입니다. 상세 필드는 [레코드 세트 생성](./api-guide/#_14)에 [레코드 세트 타입에 따른 상세 필드]에서 확인할 수 있습니다.
- 상세 필드와 recordContent 필드를 동시에 입력하면 recordContent 필드를 기준으로 수정됩니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets/{recordsetId}' \
-H 'Content-Type: application/json' \
--data '{ "recordset": { "recordsetType": "A", "recordsetTtl": 86400, "recordList": [{ "recordDisabled": false, "recordContent": "1.1.1.1" }] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordset | Object |  | 필수 |  | 레코드 세트 |
| recordset.recordsetType | String | A, AAAA, CAA, CNAME, MX, <br>NAPTR, PTR, TXT, SRV, NS | 필수 |  | 레코드 세트 타입 |
| recordset.recordsetTtl | int | 최소 10, 최대 2147483647 | 필수 |  | 네임 서버에서 레코드 세트 정보의 갱신 주기 |
| recordset.recordList | List |  | 필수 |  | 레코드 목록 |
| recordset.recordList[0].recordDisabled | boolean |  | 필수 |  | 레코드 비활성화 여부 |
| recordset.recordList[0].recordContent | String |  | 필수 |  | 레코드 세트 타입에 따른 상세 필드를 한 줄로 표시한 내용 |


#### 응답

[응답 본문]

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


### 레코드 세트 삭제

- 여러 개의 레코드 세트를 삭제하며, 레코드 세트의 레코드도 함께 삭제합니다.
- SOA 레코드 세트는 생성, 수정, 삭제할 수 없으며, NS 레코드 세트는 **DNS Zone 이름**으로 생성, 수정, 삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {zoneId}는 DNS Zone ID이며 [DNS Zone 조회](./api-guide/#dns-zone)를 통해서 알 수 있습니다.
- 레코드 세트 ID는 [레코드 세트 조회](./api-guide/#_11)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/zones/{zoneId}/recordsets?
recordsetIdList=edb9512b-6e62-409c-99ee-092d340e0adf,edb9512b-6e62-409c-99ee-092d340e0adf'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| recordsetIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | 레코드 세트 ID 목록 |

#### 응답

[응답 본문]

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

### GSLB 조회

- GSLB 목록을 조회합니다.
- Pool에 헬스 체크가 연결되어 있는 경우 GSLB 정상 상태와 Pool 정상 상태, 엔드포인트 정상 상태를 알 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?showHealthy=true'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslbIdList | List | 최대 3,000개 | 선택 |  | GSLB ID 목록 |
| searchGslbName | String |  | 선택 |  | 검색할 GSLB 이름 |
| gslbDomain | String |  | 선택 |  | GSLB 도메인 |
| showHealthy | boolean |  | 선택 |  | 헬스 체크 결과 보기 여부 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>GSLB_NAME, <br>GSLB_DOMAIN, <br>GSLB_TTL, <br>GSLB_ROUTING_RULE, <br>GSLB_DISABLED | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>GSLB_NAME: GSLB 이름, <br>GSLB_DOMAIN: GSLB 도메인, <br>GSLB_TTL: GSLB 도메인 갱신 주기, <br>GSLB_ROUTING_RULE: 라우팅 규칙, <br>GSLB_DISABLED: GSLB 비활성화 여부) |

#### 응답

[응답 본문]

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
                        // Pool 정보 생략
                    }
                },
                {
                    "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                    "connectedPoolOrder": 2,
                    "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                    "pool": {
                        // Pool 정보 생략
                    }
                }
            ],
            "createdAt": "2019-12-18T20:44:02.000+09:00",
            "updatedAt": "2019-12-18T21:01:05.000+09:00"
        }
    ]
}
```

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 GSLB 개수 |
| gslbList | List | Pool 목록 |
| gslbList[0].gslbId | String | GSLB ID |
| gslbList[0].gslbName | String | GSLB 이름 |
| gslbList[0].gslbDomain | String | GSLB 도메인 |
| gslbList[0].gslbTtl | String | GLSB 도메인 갱신 주기 |
| gslbList[0].gslbRoutingRule | String | 라우팅 규칙 |
| gslbList[0].gslbDisabled | boolean | GSLB 비활성화 여부 |
| gslbList[0].healthy | boolean | GSLB 정상 여부 |
| gslbList[0].connectedPoolList | List | 연결된 Pool 목록 |
| gslbList[0].connectedPoolList[0].poolId | String | 연결된 Pool ID |
| gslbList[0].connectedPoolList[0].pool | Object | 연결된 Pool 정보 |
| gslbList[0].connectedPoolList[0].connectedPoolOrder | int | 연결된 Pool 우선순위 |
| gslbList[0].connectedPoolList[0].connectedPoolRegionContent | String | 연결된 Pool의 지역을 한 줄로 표시한 내용 |
| gslbList[0].createdAt | DateTime | 생성일 |
| gslbList[0].updatedAt | DateTime | 수정일 |


### GSLB 생성

- GSLB와 Pool 연결 설정을 생성합니다.
- **라우팅 규칙**은 GSLB 도메인에 대한 로드밸런싱 방법으로 FAILOVER, RANDOM, GEOLOCATION을 선택할 수 있습니다.
    - FAILOVER: 연결된 Pool의 우선순위로 라우팅합니다.
    - RANDOM: 연결된 Pool 중 사용 가능한 Pool을 무작위로 선택하여 라우팅합니다.
    - GEOLOCATION: 설정된 지역의 트래픽을 해당 연결된 Pool로 라우팅합니다. 지역 설정이 없는 경우 우선순위로 라우팅합니다.
- **연결된 Pool**의 **우선순위**는 작을수록 라우팅 순서가 높으며, 중복될 수 없습니다.
- GSLB 생성 개수와 Pool 연결 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbRoutingRule": "FAILOVER", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2 } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslb | Object |  | 필수 |  | GSLB |
| gslb.gslbName | String | 최대 100자,<br>영어 대소문자와 숫자, '-', '_' | 필수 |  | GSLB 이름 |
| gslb.gslbTtl | int |  | 필수 | false | GSLB 도메인 갱신 주기 |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | 필수 |  | 라우팅 규칙 |
| gslb.gslbDisabled | boolean |  | 선택 | false | GSLB 비활성화 여부 |
| gslb.connectedPoolList | List |  | 선택 |  | 연결된 Pool 목록 |
| gslb.connectedPoolList[0].poolId | String |  | 필수 |  | 연결된 Pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

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
                    // Pool 정보 생략
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "pool": {
                    // Pool 정보 생략
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:44:03.000+09:00"
    }
}
```


### GSLB 수정

- GSLB와 Pool 연결 설정을 수정합니다.
- [GSLB 생성](./api-guide/#gslb_1)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}' \
-H 'Content-Type: application/json' \
--data '{ "gslb": { "gslbName": "GSLB-test", "gslbTtl": 300, "gslbDisabled": true, "gslbRoutingRule": "GEOLOCATION", "connectedPoolList": [ { "poolId": "8e4326d4-3862-4b46-819e-83a786add570", "connectedPoolOrder": 1 }, { "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818", "connectedPoolOrder": 2, "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA" } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslb | Object |  | 필수 |  | GSLB |
| gslb.gslbName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | GSLB 이름 |
| gslb.gslbTtl | int |  | 필수 | false | GSLB 도메인 갱신 주기 |
| gslb.gslbRoutingRule | String | FAILOVER, RANDOM, GEOLOCATION  | 필수 |  | 라우팅 규칙 |
| gslb.gslbDisabled | boolean |  | 선택 | false | GSLB 비활성화 여부 |
| gslb.connectedPoolList | List |  | 선택 |  | 연결된 Pool 목록 |
| gslb.connectedPoolList[0].poolId | String |  | 필수 |  | 연결된 Pool ID |
| gslb.connectedPoolList[0].connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| gslb.connectedPoolList[0].connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

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
                    // Pool 정보 생략
                }
            },
            {
                "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
                "connectedPoolOrder": 2,
                "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
                "pool": {
                    // Pool 정보 생략
                }
            }
        ],
        "createdAt": "2019-12-18T20:44:02.000+09:00",
        "updatedAt": "2019-12-18T20:59:49.000+09:00"
    }
}
```


### GSLB 삭제

- 여러 개의 GSLB를 삭제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs?
gslbIdList=91de0c6f-aeaa-44ec-b361-822acfcd5921,269eff10-f3c0-4b11-b072-ec53e7c604bf'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| gslbIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | GSLB ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


### Pool 연결

- GSLB에 Pool을 연결합니다.
- **연결된 Pool**의 **우선순위**는 작을수록 라우팅 순서가 높으며, 기존에 연결된 Pool과 동일한 우선순위를 입력한 경우 기존 Pool의 라우팅 순서가 낮아집니다.
- Pool 연결 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1 } }'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 필수 |  | 연결된 Pool |
| connectedPool.connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

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
                // Pool 정보 생략
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```

### Pool 연결 수정

- GSLB에 연결된 Pool 설정을 수정합니다.
- [GSLB 생성](./api-guide/#gslb_1)의 Pool 설정 또는 [Pool 연결](./api-guide/#pool)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.
- connectedPoolRegionContent 필드는 쉼표(,)를 구분 문자로 하여 **지역**을 한 줄로 작성합니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "connectedPool": { "connectedPoolOrder": 1, "connectedPoolRegionContent": "WESTERN_NORTH_AMERICA" } }'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| connectedPool | Object |  | 필수 |  | 연결된 Pool |
| connectedPool.connectedPoolOrder | int | 최소 1, 최대 2,147,483,647 | 필수 |  | 연결된 Pool 우선순위 |
| connectedPool.connectedPoolRegionContent | String | WESTERN_NORTH_AMERICA,<br>EASTERN_NORTH_AMERICA,<br>WESTERN_EUROPE,<br>EASTERN_EUROPE,<br>NORTHERN_SOUTH_AMERICA,<br>SOUTHERN_SOUTH_AMERICA,<br>OCEANIA,<br>MIDDLE_EAST,<br>NORTHERN_AFRICA,<br>SOUTHERN_AFRICA,<br>INDIA,<br>SOUTHEAST_ASIA,<br>NORTHEAST_ASIA | 선택 |  | 연결된 Pool 지역 설정 |

#### 응답

[응답 본문]

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
                // Pool 정보 생략
            }
        },
        {
            "poolId": "8e4326d4-3862-4b46-819e-83a786add570",
            "connectedPoolOrder": 2,
            "pool": {
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```

### Pool 연결 해제

- GSLB에 연결된 여러 개의 Pool을 해제합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {gslbId}는 GSLB ID이며 [GSLB 조회](./api-guide/#gslb)를 통해서 알 수 있습니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/gslbs/{gslbId}/connected-pools?
poolIdList=52da0e48-9062-43f7-bef8-8aec4b795bfe,12bc396a-eb97-4a6b-ab4c-73d1a1dfb093'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | Pool ID 목록 |

#### 응답

[응답 본문]

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
                // Pool 정보 생략
            }
        },
        {
            "poolId": "2f89d3fe-03bc-4711-826e-db2c89c12818",
            "connectedPoolOrder": 3,
            "connectedPoolRegionContent": "NORTHEAST_ASIA,SOUTHEAST_ASIA",
            "pool": {
                // Pool 정보 생략
            }
        }
    ]
}
```


## Pool API

### Pool 조회

- Pool 목록을 조회합니다.
- 헬스 체크가 연결되어 있는 경우 Pool 정상 상태와 엔드포인트 정상 상태를 알 수 있습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?showHealthy=true'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최대 3,000개 | 선택 |  | Pool ID 목록 |
| searchPoolName | String |  | 선택 |  | 검색할 Pool 이름 |
| healthCheckId | String |  | 선택 |  | 연결 된 헬스 체크 ID |
| showHealthy | boolean |  | 선택 |  | 헬스 체크 결과 보기 여부 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>POOL_NAME, <br>POOL_DISABLED, <br>HEALTH_CHECK_ID | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>POOL_NAME: Pool 이름, <br>POOL_DISABLED: Pool 비활성화 여부, <br>HEALTH_CHECK_ID: 연결된 헬스 체크 ID) |

#### 응답

[응답 본문]

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
                // 헬스 체크 정보 생략
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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 Pool 개수 |
| poolList | List | Pool 목록 |
| poolList[0].poolId | String | Pool ID |
| poolList[0].poolName | String | Pool 이름 |
| poolList[0].poolDisabled | boolean | Pool 비활성화 여부 |
| poolList[0].healthy | boolean | Pool 정상 여부 |
| poolList[0].healthCheckId | String | 연결된 헬스 체크 ID |
| poolList[0].healthCheck | Object | 연결된 헬스 체크 정보 |
| poolList[0].endpointList | List | 엔드포인트 목록 |
| poolList[0].endpointList[0].endpointAddress | String | 엔드포인트 주소 |
| poolList[0].endpointList[0].endpointWeight | double | 엔드포인트 가중치 |
| poolList[0].endpointList[0].healthy | boolean | 엔드포인트 정상 여부 |
| poolList[0].endpointList[0].failureReason | String | 엔드포인트 비정상 이유 |
| poolList[0].createdAt | DateTime | 생성일 |
| poolList[0].updatedAt | DateTime | 수정일 |


### Pool 생성

- Pool과 Pool 내에 엔드포인트를 생성합니다.
- Pool 내의 엔드포인트의 접근성을 확인할 **헬스 체크**를 설정할 수 있습니다.
- **엔드포인트 주소**는 도메인 주소 또는 IPv4로 입력할 수 있으며 제한된 입력이 있습니다.
    - 하이픈과 마침표로 시작할 수 없으며 하이픈으로 끝날 수 없습니다. 마침표와 하이픈을 연이어서 입력할 수 없습니다.
    - [예약된 IP 주소](https://en.wikipedia.org/wiki/Reserved_IP_addresses)는 입력할 수 없습니다.
    - Pool 내에서 중복될 수 없습니다.
- 엔드포인트의 **가중치**는 Pool 내의 다른 엔드포인트 가중치와 상대적으로 동작합니다. 동일한 가중치는 Pool 내에서 동일한 비중을 가집니다.
- Pool의 생성 개수, Pool 내의 엔드포인트 개수, 전체 엔드포인트의 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "endpointList": [ { "endpointAddress": "test.dnsplus.com" }, { "endpointAddress": "123.123.123.123" } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| pool | Object |  | 필수 |  | Pool |
| pool.poolName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | Pool 이름 |
| pool.poolDisabled | boolean |  | 선택 | false | Pool 비활성화 여부 |
| pool.healthCheckId | String |  | 선택 |  | 헬스 체크 ID |
| pool.endpointList | List |  | 필수 |  | 엔드포인트 목록 |
| pool.endpointList[0].endpointAddress | String | 최대 254자,<br>소문자와 숫자, '.', '-', '_' | 필수 |  | 엔드포인트 주소 |
| pool.endpointList[0].endpointWeight | double | 최소 0, 최대 1.00 | 선택 | 1.00 | 엔드포인트 가중치 |
| pool.endpointList[0].endpointDisabled | boolean |  | 선택 | false | 엔드포인트 비활성화 여부 |

#### 응답

[응답 본문]

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


### Pool 수정

- Pool과 Pool 내에 엔드포인트를 수정합니다.
- [Pool 생성](./api-guide/#pool_4)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {poolId}는 Pool ID이며 [Pool 조회](./api-guide/#pool_3)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools/{poolId}' \
-H 'Content-Type: application/json' \
--data '{ "pool": { "poolName": "POOL-test", "poolDisabled": true, "healthCheckId": "b9165853-7859-4309-8059-48f12ebdbc17", "endpointList": [ { "endpointAddress": "test.dnsplus.com", "endpointWeight": 1.00, "endpointDisabled": true }, { "endpointAddress": "123.123.123.123", "endpointWeight": 0.5, "endpointDisabled": true } ] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| pool | Object |  | 필수 |  | Pool |
| pool.poolName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | Pool 이름 |
| pool.poolDisabled | boolean |  | 선택 | false | Pool 비활성화 여부 |
| pool.healthCheckId | String |  | 선택 |  | 헬스 체크 ID |
| pool.endpointList | List |  | 필수 |  | 엔드포인트 목록 |
| pool.endpointList[0].endpointAddress | String | 최대 254자,<br>소문자와 숫자, '.', '-', '_' | 필수 |  | 엔드포인트 주소 |
| pool.endpointList[0].endpointWeight | double | 최소 0, 최대 1.00 | 선택 | 1.00 | 엔드포인트 가중치 |
| pool.endpointList[0].endpointDisabled | boolean |  | 선택 | false | 엔드포인트 비활성화 여부 |

#### 응답

[응답 본문]

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
            // 헬스 체크 정보 생략
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


### Pool 삭제

- 여러 개의 Pool을 삭제하며, Pool의 엔드포인트도 함께 삭제합니다.
- GSLB에 연결되어 있는 Pool은 삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/pools?
poolIdList=8e4326d4-3862-4b46-819e-83a786add570,2f89d3fe-03bc-4711-826e-db2c89c12818'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| poolIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | Pool ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```


## 헬스 체크 API

### 헬스 체크 조회

- 헬스 체크 목록을 조회합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| GET | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X GET 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks'
```

[옵션]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 최대 3,000개 | 선택 |  | 헬스 체크 ID 목록 |
| searchHealthCheckName | String |  | 선택 |  | 검색할 헬스 체크 이름 |
| page | int | 최소 1 | 선택 | 1 | 페이지 번호 |
| limit | int | 최소 1, 최대 3,000 | 선택 | 50 | 조회 개수 |
| sortDirection | String | DESC, ASC | 선택 | DESC | 정렬 방향(DESC: 내림차순, ASC: 오름차순) |
| sortKey | String | CREATED_AT, <br>UPDATED_AT, <br>HEALTH_CHECK_NAME, <br>PROTOCOL, <br>PORT | 선택 | CREATED_AT | 정렬 대상 <br>(CREATED_AT: 생성일, <br>UPDATED_AT: 수정일, <br>HEALTH_CHECK_NAME: 헬스 체크 이름, <br>PROTOCOL: 프로토콜, <br>PORT: 포트) |

#### 응답

[응답 본문]

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

[필드]

| 이름 | 타입 | 설명 |
|---|---|---|
| totalCount | long | 전체 헬스 체크 개수 |
| healthCheckList | List | 헬스 체크 목록 |
| healthCheckList[0].healthCheckId | String | 헬스 체크 ID |
| healthCheckList[0].healthCheckName | String | 헬스 체크 이름 |
| healthCheckList[0].protocol | String | 프로토콜 |
| healthCheckList[0].port | int | 포트 |
| healthCheckList[0].interval | int | 헬스 체크 주기 |
| healthCheckList[0].timeout | int | 최대 응답 대기 시간 |
| healthCheckList[0].retries | int | 최대 재시도 횟수 |
| healthCheckList[0].path | String | 경로 |
| healthCheckList[0].expectedCodes | String | 예상 상태 코드 |
| healthCheckList[0].expectedBody | String | 예상 응답 본문 |
| healthCheckList[0].allowInsecure | boolean | 인증서 검증 안 함 |
| healthCheckList[0].requestHeaderList | List | 요청 헤더 목록 |
| healthCheckList[0].requestHeaderList[0] | Object | 요청 헤더 이름, 값 객체 |
| healthCheckList[0].createdAt | DateTime | 생성일 |
| healthCheckList[0].updatedAt | DateTime | 수정일 |


### 헬스 체크 생성

- 헬스 체크를 생성합니다.
- 헬스 체크 **프로토콜**은 HTTPS, HTTP, TCP를 지원하며 선택한 프로토콜에 따라 입력 할 수 있는 정보가 다릅니다.
    - HTTPS 입력 가능 항목: 인증서 검증 안 함, 포트, 헬스 체크 주기, 최대 응답 대기 시간, 최대 재시도 횟수, 경로, 예상 상태 코드, 예상 응답 본문, 요청 헤더
    - HTTP 입력 가능 항목: 포트, 헬스 체크 주기, 최대 응답 대기 시간, 최대 재시도 횟수, 경로, 예상 상태 코드, 예상 응답 본문, 요청 헤더
    - TCP 입력 가능 항목: 포트, 헬스 체크 주기, 최대 응답 대기 시간, 최대 재시도 횟수
- **인증서 검증 안 함**을 사용하면 헬스 체크가 수행될 때 엔드포인트의 TLS/SSL 인증서가 유효하지 않아도 무시할 수 있습니다.
- **예상 상태 코드**와 **예상 응답 본문**을 판단할 때 엔드포인트에서 리다이렉션된 페이지에 대해서는 지원하지 않습니다.
- 헬스 체크 생성 개수는 제한되어 있으며 연장이 필요한 경우 별도로 문의해 주시기 바랍니다. [1:1 문의](https://www.toast.com/kr/support/inquiry?alias=tab3_02)

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| POST | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X POST 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "2xx", "allowInsecure": false, "requestHeaderList": [{ "Host": "nhncloud.com" }] }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 필수 |  | 헬스 체크 |
| healthCheck.healthCheckName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | 헬스 체크 이름 |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | 필수 |  | 헬스 체크 수행 프로토콜 |
| healthCheck.port | int | 최소 1, 최대 65535 | 필수 |  | 헬스 체크 수행 포트 |
| healthCheck.interval | int | 최소 10 또는 (retries+1)*timeout, 최대 3600 | 선택 | 60 | 헬스 체크 주기 |
| healthCheck.timeout | int | 최소 1, 최대 10 | 선택 | 5 | 최대 응답 대기 시간 |
| healthCheck.retries | int | 최소 0, 최대 5 | 선택 | 2 | 최대 재시도 횟수 |
| healthCheck.path | String | 최대 254자,<br>시작 문자 '/' | 선택 |  | 헬스 체크 수행 경로,<br>HTTPS, HTTP일 때 사용 |
| healthCheck.expectedCodes | String | 숫자와 와일드카드 'x' | 선택 |  | 헬스 체크 예상 상태 코드,<br>HTTPS, HTTP일 때 사용<br>(예제) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 최대 10KB | 선택 |  | 헬스 체크 예상 응답 본문,<br>HTTPS, HTTP일 때 사용 |
| healthCheck.allowInsecure | boolean |  | 선택 |  | 헬스 체크 인증서 검증 안 함,<br>HTTPS 일 때 사용 |
| healthCheck.requestHeaderList | List |  | 선택 |  | 요청 헤더 목록,<br>HTTPS, HTTP일 때 사용,<br> 목록 내 항목은 `{ "헤더이름": "헤더 값" }`형태로 요청 |

#### 응답

[응답 본문]

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


### 헬스 체크 수정

- 헬스 체크를 수정합니다.
- [헬스 체크 생성](./api-guide/#_48)에서 입력한 항목을 수정합니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| PUT | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId} |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.
- {healthCheckId}는 헬스 체크 ID이며 [헬스 체크 조회](./api-guide/#_45)를 통해서 알 수 있습니다.

```
curl -X PUT 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks/{healthCheckId}' \
-H 'Content-Type: application/json' \
--data '{ "healthCheck": { "healthCheckName": "HTTPS-443", "protocol": "HTTPS", "port": 443, "interval": 60, "timeout": 5, "retries": 2, "path": "/", "expectedCodes": "3xx", "allowInsecure": false }}'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheck | Object |  | 필수 |  | 헬스 체크 |
| healthCheck.healthCheckName | String | 최대 100자,<br>영대소문자와 숫자, '-', '_' | 필수 |  | 헬스 체크 이름 |
| healthCheck.protocol | String | HTTPS, HTTP, TCP | 필수 |  | 헬스 체크 수행 프로토콜 |
| healthCheck.port | int | 최소 1, 최대 65535 | 필수 |  | 헬스 체크 수행 포트 |
| healthCheck.interval | int | 최소 10 또는 (retries+1)*timeout, 최대 3600 | 선택 | | 헬스 체크 주기 |
| healthCheck.timeout | int | 최소 1, 최대 10 | 선택 | | 최대 응답 대기 시간 |
| healthCheck.retries | int | 최소 0, 최대 5 | 선택 | | 최대 재시도 횟수 |
| healthCheck.path | String | 최대 254자,<br>시작 문자 '/' | 선택 |  | 헬스 체크 수행 경로,<br>HTTPS, HTTP일 때 사용 |
| healthCheck.expectedCodes | String | 숫자와 와일드카드 'x' | 선택 |  | 헬스 체크 예상 상태 코드,<br>HTTPS, HTTP일 때 사용<br>(예제) 2xx, 20x, 200 |
| healthCheck.expectedBody | String | 최대 10KB | 선택 |  | 헬스 체크 예상 응답 본문,<br>HTTPS, HTTP일 때 사용 |
| healthCheck.allowInsecure | boolean |  | 선택 |  | 헬스 체크 인증서 검증 안 함,<br>HTTPS 일 때 사용 |
| healthCheck.requestHeaderList | List |  | 선택 |  | 요청 헤더 목록,<br>HTTPS, HTTP일 때 사용,<br> 목록 내 항목은 `{ "헤더이름": "헤더 값" }`형태로 요청 |

#### 응답

[응답 본문]

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


### 헬스 체크 삭제

- 여러 개의 헬스 체크를 삭제합니다.
- Pool에 연결되어 있는 헬스 체크는 삭제할 수 없습니다.

#### 요청

[URI]

| 메서드 | URI |
|---|---|
| DELETE | https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks |

[요청 본문]

- {appkey}는 콘솔에서 확인한 값으로 변경합니다.

```
curl -X DELETE 'https://dnsplus.api.nhncloudservice.com/dnsplus/v1.0/appkeys/{appkey}/health-checks?
healthCheckIdList=b9165853-7859-4309-8059-48f12ebdbc17,d2629d6b-9381-4645-9cf3-43d7ad491e2b'
```

[필드]

| 이름 | 타입 | 유효 범위 | 필수 여부 | 기본값 | 설명 |
|---|---|---|---|---|---|
| healthCheckIdList | List | 최소 1개, 최대 3,000개 | 필수 |  | 헬스 체크 ID 목록 |

#### 응답

[응답 본문]

```
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```