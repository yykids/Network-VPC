## Network > VPC > API 가이드

## 사전 준비

인스턴스 API를 사용하려면 토큰 발급과 같은 사전 준비가 필요합니다. [API 사용 준비 가이드](/Infrastructure%20Common/ko/api-guide/)를 참조합니다.


## 보안 그룹 API
보안 그룹 생성, 삭제, 조회 및 업데이트 기능을 제공합니다. 보안 그룹을 인스턴스에 등록/해제하는 기능은 [인스턴스 API](/Compute/Instance/ko/api-guide/)를 통해 제공됩니다.

### 보안 그룹 목록 조회
접근 가능한 보안 그룹의 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Query | String | O | 조회할 보안 그룹 ID. 기재하지 않을 경우 모든 보안 그룹의 정보를 조회합니다. |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroups": [
        {
            "description": "{Desctiption}",
            "id": "{Security Group ID}",
            "name": "{Name}",
            "securityGroupRules": [
                {
                    "direction": "egress",
                    "ethertype": "IPv4",
                    "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                    "portRangeMax": null,
                    "portRangeMin": null,
                    "protocol": null,
                    "remoteGroupId": null,
                    "remoteIpPrefix": null,
                    "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                    "description": ""
                }
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | 보안 그룹 설명 |
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| securityGroupRules | Body | List | 보안 그룹 규칙 목록, [보안 그룹 규칙 API](#security-group-rules-api) 참조 |

### 보안 그룹 생성
새로운 보안 그룹을 생성합니다.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| name | Body | String | - |보안 그룹 이름 |
| description | Body | String | O | 보안 그룹 설명 |

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "description": "{Description}",
        "id": "{Security Group ID}",
        "name": "{Name}",
        "securityGroupRules": [
            {
                "direction": "egress",
                "ethertype": "IPv4",
                "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                "portRangeMax": null,
                "portRangeMin": null,
                "protocol": null,
                "remoteGroupId": null,
                "remoteIpPrefix": null,
                "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                "description": ""
            }
        ]
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | 보안 그룹 설명 |
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| securityGroupRules | Body | List | 보안 그룹 규칙 목록, [보안 그룹 규칙 API](#security-group-rules-api) 참조 |

### 보안 그룹 업데이트
보안 그룹의 이름, 설명을 변경합니다.

#### Method, URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Path | String | - | 변경할 보안 그룹의 ID |

#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Name | Body | String | - | 보안 그룹 이름 |
| Description | Body | String | O | 보안 그룹 설명 |

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "id": "{Security Group ID}",
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Security Group ID | Body | String | 보안 그룹 ID |
| Name | Body | String | 보안 그룹 이름 |
| Description | Body | String | 보안 그룹 설명 |

### 보안 그룹 삭제
지정한 보안 그룹을 삭제합니다.

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupId | Query | String | - | 삭제할 보안 그룹 ID |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```


## 보안 그룹 규칙 API
보안 그룹 규칙 추가/삭제 및 조회 기능을 제공합니다.

### 보안 그룹 규칙 조회
접근 가능한 모든 보안 그룹 규칙의 정보를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupRuleId | Query | String | O | 조회할 보안 그룹 규칙 ID. 기재하지 않을 경우 모든 보안 그룹 규칙의 정보를 조회합니다. |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRules": [
        {
            "direction": "{Direction}",
            "ethertype": "{Ethernet Type}",
            "id": "{Rule ID}",
            "portRangeMax": "{Port Range MAX}",
            "portRangeMin": "{Port Range MIN}",
            "protocol": "{Protocol}",
            "remoteGroupId": "{Remote Group ID}",
            "remoteIpPrefix": "{Remote IP Prefix}",
            "securityGroupId": "{Security Group ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | "IPv4" 또는 "IPv6" |
| Rule ID | Body | String | 보안 그룹 규칙 ID |
| Port Range MAX | Body | Integer | 규칙이 적용되는 최대 포트 번호 |
| Port Range MIN | Body | Integer | 규칙이 적용되는 최소 포트 번호 |
| Protocol | Body | String | IP 프로토콜 "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 규칙이 적용되는 Remote Group의 ID |
| Remote IP Prefix | Body | String | 규칙이 적용되는 Remote IP의 Prefix |
| Security Group ID | Body | String | 규칙이 적용되는 보안 그룹의 ID |

### 보안 그룹 규칙 생성
새로운 보안 그룹 규칙을 생성합니다.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
```json
{
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "portRangeMin": "{Port Range MAX}",
        "portRangeMax": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Direction | Body | String | - | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | O | "IPv4" 또는 "IPv6" |
| Port Range MAX | Body | Integer | O | 규칙이 적용되는 최대 포트 번호 |
| Port Range MIN | Body | Integer | O | 규칙이 적용되는 최소 포트 번호 |
| Protocol | Body | String | O | IP 프로토콜. "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | O | 규칙이 적용되는 Remote 보안 그룹의 ID |
| Remote IP Prefix | Body | String | - | 규칙이 적용되는 Remote IP의 Prefix. |
| Security Group ID | Body | String | - | 규칙이 적용되는 보안 그룹의 ID |

##### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "id": "{Rule ID}",
        "portRangeMax": "{Port Range MAX}",
        "portRangeMin": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | 규칙이 적용되는 방향, "ingress" 또는 "egress" |
| Ethernet Type | Body | String | "IPv4" 또는 "IPv6" |
| Rule ID | Body | String | 보안 그룹 규칙 ID |
| Port Range MAX | Body | Integer | 규칙이 적용되는 최대 포트 번호 |
| Port Range MIN | Body | Integer | 규칙이 적용되는 최소 포트 번호 |
| Protocol | Body | String | IP 프로토콜. "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 규칙이 적용되는 Remote 보안 그룹의 ID |
| Remote IP Prefix | Body | String | 규칙이 적용되는 Remote IP의 Prefix |
| Security Group ID | Body | String | 규칙이 적용되는 보안 그룹의 ID |

### 보안 그룹 규칙 삭제
지정한 보안 그룹 규칙을 삭제합니다.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuelsId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 토큰 ID |
| securityGroupRuleId | Query | String | - | 삭제할 보안 그룹 규칙 ID |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```

## 네트워크 API
인스턴스에서 연결할 수 있는 네트워크 정보 조회 기능을 제공합니다.

### 네트워크 상태
네트워크는 다음 상태 값을 같습니다.

| Status | Description |
| -- | -- |
| BUILD | 네트워크 구축 중 |
| ACTIVE | 네트워크 활성화 상태 |
| DOWN | 네트워크 비활성화 상태 |
| ERROR | 에러 발생 |

### 네트워크 정보 조회
접근 가능한 네트워크의 정보를 조회합니다.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 토큰 ID |
| networkId | Query | String | O | 조회할 네트워크 ID. 기재하지 않을 경우 모든 네트워크의 정보를 조회합니다. |

#### Request Body
이 Request는 Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "networks": [
        {
            "adminStateUp": "{Administrative State}",
            "id": "{Network ID}",
            "name": "{Network Name}",
            "router:external": "{External Router Provided}",
            "status": "{Network Status}",
            "subnets": [
                "{Subnet ID}"
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Administrative State | Body | Boolean |네트워크 관리 상태. true: up, false: down |
| Network ID | Body | String | 네트워크 ID |
| Network Name | Body | String | 네트워크 이름 |
| External Router Provided | Body | Boolean | 라우터를 통한 플로팅 IP 제공 가능 여부 |
| Network Status | Body | String | 네트워크 상태. ACTIVE, DOWN, BUILD, ERROR |
| Subnet ID | Body | String | 서브넷 ID |

## 서브넷 API
### 서브넷 정보 조회
접근 가능한 서브넷의 정보를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "subnets": [
        {
            "allocationPools": [
                {
                    "start": "{Start IP}",
                    "end": "{End IP}"
                }
            ],
            "cidr": "{CIDR}",
            "enableDhcp": "{Enable DHCP}",
            "gatewayIp": "{Gateway IP}",
            "hostRoutes": [],
            "id": "{Subnet ID}",
            "ipVersion": "{IP version}",
            "name": "{Subnet Name}",
            "networkId": "{Network ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Start IP | Body | String | 할당 Pool의 시작 IP. 예) 10.161.244.13 |
| End IP | Body | String | 할당 Pool의 마지막 IP. 예) 10.161.244.121 |
| CIDR | Body | String | Classless Inter-Domain Routing. 예) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | DHCP 활성화 여부 |
| Subnet ID | Body | String | 서브넷 ID |
| IP Version | Body | Integer | 서브넷의 IP 버전 |
| Subnet Name | Body | Integer | 서브넷 이름 |
| Network ID | Body | Integer | 서브넷이 속한 네트워크 ID |

## 플로팅 IP API
플로팅 IP 생성, 삭제, 정보 조회 기능을 제공합니다.

### 플로팅 IP Status
플로팅 IP는 다음 상태값을 갖습니다.

| Status | Description |
| -- | -- |
| ACTIVE | 플로팅 IP가 인스턴스와 연결되어 사용중인 상태 |
| DOWN | 플로팅 IP가 연결되어 있지 않은 상태 |
| ERROR | 에러 발생 |

### 플로팅 IP 조회
사용 가능한, 또는 사용 중인 플로팅 IP 정보를 조회합니다.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |토큰 ID |
| floatingIpId | Query | String | O | 조회할 플로팅 IP의 ID. 기재하지 않을 경우 모든 플로팅 IP의 정보를 조회합니다. |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingips": [
        {
        	"id": "{Floating IP ID}",
            "floatingIpAddress": "{Floating IP Address}",
            "fixedIpAddress": "{Fixed IP Address}",
            "floatingNetworkId": "{Floating IP Network ID}",
            "portId": "{Port ID}",
            "routerId": "{Router ID}",
            "status": "{Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 플로팅 IP ID |
| Floating IP Address | Body | String | 플로팅 IP 주소 |
| Fixed IP Address | Body | String | 플로팅 IP가 연결된 인스턴스 NIC의 IP 주소. Status가 "ACTIVE" 인 경우에만 표시 |
| Floating Network ID | Body | String | 플로팅 IP가 연결된 네트워크 ID |
| Port ID | Body | String | 플로팅 IP가 연결된 포트 ID. 상태가 "ACTIVE" 인 경우에만 표시 |
| Router ID | Body | String | 플로팅 IP의 라우터 ID. 상태가 "ACTIVE" 인 경우에만 표시 |
| Status | Body | String | 플로팅 IP의 상태 |

### 플로팅 IP 생성
플로팅 IP를 생성합니다.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |

#### Request Body
이 API는 Request Body를 필요로 하지 않습니다.

#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingip": {
    	"id": "{Floating IP ID}",
        "floatingIpAddress": "{Floating IP Address",
        "floatingNetworkId": "{Floating IP Network ID}",
        "status": "{Status}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 플로팅 IP ID |
| Floating IP Address | Body | String | 플로팅 IP 주소 |
| Floating Network ID | Body | String | 플로팅 IP가 연결된 네트워크의 ID |
| Status | Body | String | 플로팅 IP의 상태 |

### 플로팅 IP 삭제
지정한 플로팅 IP를 삭제합니다.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 토큰 ID |
| floatingIpId | Path | String | - | 삭제할 플로팅 IP ID |

#### Request Body
이 API는 request body를 필요로 하지 않습니다.

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
