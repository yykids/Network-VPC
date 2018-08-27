## Network > VPC > API 指南

## 事前准备

要想使用网络 VPC API，则需要App key和令牌（token）。利用[API Endpoint URL](/Compute/Instance/zh/api-guide/#api-endpoint-url)和[令牌 API](/Compute/Instance/zh/api-guide/#api)来准备App key和令牌。 API Endpoint URL中包含App key， Request Body中包含令牌。

例如，想查询安全组的列表，应通过以下URL来进行查询。

	GET https://api-compute.cloud.toast.com/compute/v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}


## 安全组API
提供安全组创建、删除、查询及更新功能。通过[实例API](/Compute/Instance/zh/api-guide/)，提供将实例添加到安全组和将实例移除安全组的功能。

### 安全组列表查询
查询具有权限的安全组信息。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |
| securityGroupId | Query | String | O | 要查询的安全组ID. 如果没有则将查询所有安全组信息。|

#### Request Body
该 API无需 Request Body。

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
| Description | Body | String | 安全组描述 |
| Security Group ID | Body | String | 安全组 ID |
| Name | Body | String | 安全组名称 |
| securityGroupRules | Body | List | 安全组规则条目.参考[安全组规则 API](#api_1)  |

### 创建安全组
创建一个新的安全组。

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |

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
| Name | Body | String | - |安全组名称 |
| Description | Body | String | O | 安全组描述 |

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
| Description | Body | String | 安全组描述 |
| Security Group ID | Body | String | 安全组 ID |
| Name | Body | String | 安全组名称 |
| securityGroupRules | Body | List | 安全组规则条目. 参考[安全组规则 API](#api_1) |

### 修改安全组
修改安全组名称、描述。

#### Method, URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |
| securityGroupId | Path | String | - | 要修改的安全组ID |

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
| Name | Body | String | - | 安全组名称 |
| Description | Body | String | O | 安全组描述 |

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
| Security Group ID | Body | String | 安全组 ID |
| Name | Body | String | 安全组名称 |
| Description | Body | String | 安全组描述 |

### 删除安全组
删除指定的安全组。

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |
| securityGroupId | Query | String | - | 要删除的安全组 ID |

#### Request Body
该 API无需 Request Body。

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


## 安全组规则 API
提供安全组中规则的添加/删除及查询功能。

### 安全组规则查询
查询具有权限的所有安全组规则信息。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |
| securityGroupRuleId | Query | String | O | 要查询的安全组规则ID. 如果没有，则查询所有安全组规则信息。|

#### Request Body
该 API无需 Request Body。

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
| Direction | Body | String | 规则适用的方向, "ingress" 或者 "egress" |
| Ethernet Type | Body | String | "IPv4" 或者 "IPv6" |
| Rule ID | Body | String | 安全组规则 ID |
| Port Range MAX | Body | Integer | 规则适用的最大端口号 |
| Port Range MIN | Body | Integer | 规则适用的最小端口号 |
| Protocol | Body | String | IP协议 "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 规则适用的其它安全组的 ID |
| Remote IP Prefix | Body | String | 规则适用的授权IP地址段 |
| Security Group ID | Body | String | 规则适用的安全组 ID |

### 创建安全组规则
创建新的安全组规则。
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |

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
| Direction | Body | String | - | 规则适用的方向, "ingress" 或者 "egress" |
| Ethernet Type | Body | String | O | "IPv4" 或者 "IPv6" |
| Port Range MAX | Body | Integer | O | 设定规则适用的最大端口号时（范围1~65535）."protocol" 项目不可省略 |
| Port Range MIN | Body | Integer | O | 设定规则适用的最小端口号时（范围1~65535）. "protocol"项目不可省略 |
| Protocol | Body | String | O | IP 协议. "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | O | 设定规则适用的其它安全组的ID时. <br />"remoteIpPrefix" 值省略<Paste> |
| Remote IP Prefix | Body | String | - | 设定规则适用的IP地址段时. <br />"remoteGroupId" 值省略. |
| Security Group ID | Body | String | - | 规则适用的安全组 ID |

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
| Direction | Body | String | 规则适用的方向, "ingress" 或者 "egress" |
| Ethernet Type | Body | String | "IPv4" 或者 "IPv6" |
| Rule ID | Body | String | 安全组规则 ID |
| Port Range MAX | Body | Integer | 规则适用的最大端口号 |
| Port Range MIN | Body | Integer | 规则适用的最小端口号 |
| Protocol | Body | String | IP 协议. "icmp", "tcp", "udp", "null" |
| Remote Group ID | Body | String | 规则适用的其它安全组的 ID |
| Remote IP Prefix | Body | String | 规则适用的IP地址段 |
| Security Group ID | Body | String | 规则适用的安全组 ID |

### 删除安全组规则
删除指定的安全组规则。
#### Method, URL

```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | 令牌 ID |
| securityGroupRuleId | Query | String | - | 要删除的安全组规则 ID |

#### Request Body
该 API无需 Request Body。

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

## 网络 API
在实例（instance）中，提供可连接的网络信息查询功能。

### 网络状态
网络具备如下状态值。

| Status | Description |
| -- | -- |
| BUILD | 网络创建中状态 |
| ACTIVE | 网络已连接状态 |
| DOWN | 网络已停用状态 |
| ERROR | 网络产生错误状态 |

### 查询网络信息
查询具有权限的网络信息。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 令牌 ID |
| networkId | Query | String | O | 要查询的网络 ID. 如果没有，则查询所有网络信息。|

#### Request Body
该 API无需 Request Body。

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
| Administrative State | Body | Boolean |网络管理状态. true: up, false: down |
| Network ID | Body | String | 网络 ID |
| Network Name | Body | String | 网络名称 |
| External Router Provided | Body | Boolean | 是否提供，通过路由器的弹性IP |
| Network Status | Body | String | 网络状态. ACTIVE, DOWN, BUILD, ERROR |
| Subnet ID | Body | String | 子网 ID |

## 子网 API
### 子网信息查询
查询具有权限的子网信息。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | 令牌 ID |

#### Request Body
该 API无需 Request Body。

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
| Start IP | Body | String | 分配 Pool的开始 IP. 例) 10.161.244.13 |
| End IP | Body | String | 分配 Pool的最后 IP. 例) 10.161.244.121 |
| CIDR | Body | String | 无类别域间路由（Classless Inter-Domain Routing），是IP地址分配的方法之一. 例) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | DHCP 激活与否 |
| Subnet ID | Body | String | 子网 ID |
| IP Version | Body | Integer | 子网的 IP 版本 |
| Subnet Name | Body | Integer | 子网名称 |
| Network ID | Body | Integer | 子网所属的网络 ID |

## 弹性IP API
提供弹性 IP 的创建、删除、信息查询功能。

### 弹性IP Status
弹性IP具备如下状态值。

| Status | Description |
| -- | -- |
| ACTIVE | 弹性IP与实例（instance）绑定并使用中的状态 |
| DOWN | 弹性IP空闲的状态 |
| ERROR | 发生错误 |


### 查询弹性IP Pool
查询弹性IP Pool列表。

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ip-pools
X-Auth-Token: {tokenId}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌 ID |

#### Request Body
该 API无需 Request Body。

#### Response Body
```json
{
    "header" : {
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true,
        "resultCode" :  0
    },
    "pools" : [
         {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
         }
    ]
}
```
|  Name | In | Type | Description |
|--|--|--|--|
| Pool ID | Body | String | 弹性IP Pool的ID |
| Pool Name | Body | String | 弹性IP Pool的名称 |


### 查询弹性IP 
查询可以使用，或者使用中的弹性IP信息。
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |令牌 ID |
| floatingIpId | Query | String | O | 要查询的弹性IP的ID. 如果没有，则查询所有弹性IP信息。|

#### Request Body
该 API无需 Request Body。

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
            "portId": "{Port ID}",
            "routerId": "{Router ID}",
            "pool" : {
                "id" :  "{Pool ID}",
                "name" :  "{Pool Name}"
            },
            "status": "{Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 弹性IP ID |
| Floating IP Address | Body | String | 弹性IP地址 |
| Fixed IP Address | Body | String | 绑定弹性IP实例的NIC IP地址. 只有在Status为 "ACTIVE" 的情况下显示 |
| Port ID | Body | String | 绑定弹性IP的端口ID. 只有在状态为 "ACTIVE" 的情况下显示 |
| Router ID | Body | String | 绑定弹性IP的路由器ID. 只有状态为 "ACTIVE" 的情况下显示 |
| Pool ID | Body | String | 弹性IP所属Pool的ID |
| Pool Name | Body | String | 弹性IP所属Pool的名称 |
| Status | Body | String | 弹性IP的状态 |

### 创建弹性IP 
创建弹性IP。
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌 ID |

#### Request Body
```json
{
    "pool" : {
        "id" :  "{Pool ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
|  Pool ID | Body | String | - | 弹性IP Pool的ID |

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
        "floatingIpAddress": "{Floating IP Address}",
        "pool": {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
        },
        "status": "{Status}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | 弹性IP ID |
| Floating IP Address | Body | String | 弹性IP 地址 |
| Pool ID | Body | String | 弹性IP Pool的ID |
| Pool Name | Body | String | 弹性IP Pool的名称 |
| Status | Body | String | 弹性IP的状态 |

### 删除弹性IP 
删除指定的弹性IP。解除绑定之后，可删除使用中的(ACTIVE)弹性IP。
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | 令牌 ID |
| floatingIpId | Path | String | - | 要删除的弹性IP ID |

#### Request Body
该 API无需 Request Body。

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


