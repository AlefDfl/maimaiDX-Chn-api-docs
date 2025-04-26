# maimaiDX Chn titleserver docs

## **地址https://maimai-gm.wahlap.com:42081/Maimai2Servlet/**

## 地区id对照

| 编号 | 地区名称 |
| :--- | :------- |
| 1    | 北京     |
| 2    | 重庆     |
| 3    | 上海     |
| 4    | 天津     |
| 5    | 安徽     |
| 6    | 福建     |
| 7    | 甘肃     |
| 8    | 广东     |
| 9    | 贵州     |
| 10   | 海南     |
| 11   | 河北     |
| 12   | 黑龙江   |
| 13   | 河南     |
| 14   | 湖北     |
| 15   | 湖南     |
| 16   | 江苏     |
| 17   | 江西     |
| 18   | 吉林     |
| 19   | 辽宁     |
| 20   | 青海     |
| 21   | 陕西     |
| 22   | 山东     |
| 23   | 山西     |
| 24   | 四川     |
| 25   | 台湾     |
| 26   | 云南     |
| 27   | 浙江     |
| 28   | 广西     |
| 29   | 内蒙古   |
| 30   | 宁夏     |
| 31   | 新疆     |
| 32   | 西藏     |
| _    | Unknown  |

### GetGameSettingApiMaimaiChn 设置数据获取

#### 发送内容

| 字段名     | 类型   | 必填 | 示例值          | 规则                                     |
| :--------- | :----- | :--- | :-------------- | :--------------------------------------- |
| `placeId`  | 整数   | 是   | `1906`          | 整数类型，机厅id                         |
| `clientId` | 字符串 | 是   | `"A63E01E1191"` | 字符串类型，需用英文双引号包裹，机台狗号 |

- ##### 示例内容

  ```json
   {"placeId":地区id,"clientId":"狗号"}
  ```

  

#### 返回内容

看不懂啊，不做解析了直接上示例吧

```json
{
    "isAouAccession": false,
    "gameSetting": {
        "isMaintenance": false,
        "requestInterval": 1200,
        "rebootStartTime": "2016-01-01 04:00:00",
        "rebootEndTime": "2016-01-01 07:00:00",
        "rebootInterval": 0,
        "movieUploadLimit": 0,
        "movieStatus": 0,
        "movieServerUri": "",
        "deliverServerUri": "",
        "oldServerUri": "",
        "usbDlServerUri": "",
        "maxCountRivalMusic": 100,
        "replicationDelayLimit": 10,
        "exclusionStartTime": "04:30:00",
        "exclusionEndTime": "05:00:00",
        "pingDisable": false,
        "packetTimeout": 20000,
        "packetTimeoutLong": 60000,
        "packetRetryCount": 3,
        "userDataDlErrTimeout": 300000,
        "userDataDlErrRetryCount": 1000,
        "userDataDlErrSamePacketRetryCount": 1000,
        "userDataUpSkipTimeout": 0,
        "userDataUpSkipRetryCount": 0,
        "iconPhotoDisable": true,
        "uploadPhotoDisable": true,
        "maxCountMusic": 0,
        "maxCountItem": 0,
        "nameEntryDisable": true,
        "packetRecreateCount": 0
    }
}
```



### GetGameRankingApiMaimaiChn 推荐乐曲排行榜

#### 发送内容

`type`：整数类型，具体值分页获取，每页五十首，数字几就是第几页

##### 示例

```json
{"type":1}
```

#### 返回内容

- **根字段**

  - `type`：整数，为排行榜分页
  - `gameRankingList`：**数组类型**，一般有50个
  - `gameRankingInstantList`：一般返回为null，可忽略

- **数组内对象字段**

  - `id`：排行榜的歌曲id

  - `point`：也许是被游玩的次数，整数，并且排名越靠前数值越大

  - `userName`：一般留空（`""`）

  - **示例参考**

    ```json
    {
      "type": 1,
      "gameRankingList": [
        {"id": 11658, "point": 13865, "userName": ""},
        {"id": 11660, "point": 4973, "userName": ""},
        //省略
      ],
      "gameRankingInstantList": null
    }
    ```

### GetGameEventApiMaimaiChn获取活动

#### 发送内容

##### 1. **字段解释**

| 字段名       | 类型   | 必填 | 示例值 | 说明                                                         |
| :----------- | :----- | :--- | :----- | :----------------------------------------------------------- |
| `type`       | 整数   | 是   | `1`    | 表示页数，一般为1                                            |
| `isAllEvent` | 布尔值 | 是   | `true` | 是否返回全部事件，`true`则返回全部时间，false则返回目前进行中的活动 |

##### 2. **示例**

```json
{
  "type": 1,
  "isAllEvent": true
}
```

#### 返回内容

##### 1. **根字段规则**

| 字段名          | 类型 | 必填 | 示例    | 备注                                  |
| :-------------- | :--- | :--- | :------ | :------------------------------------ |
| `type`          | 整数 | 是   | `1`     | 与发送参数一致                        |
| `length`        | 整数 | 是   | `580`   | 与`gameEventList`数组包含对象数量相同 |
| `gameEventList` | 数组 | 是   | `[...]` | 事件列表                              |

##### 2. **事件对象字段规则** (`gameEventList`内)

| 字段名      | 类型   | 示例                    | 备注                                   |
| :---------- | :----- | :---------------------- | :------------------------------------- |
| `type`      | 整数   | `1`                     | 与发送内容一致                         |
| `id`        | 整数   | `191201`                | 活动id（年份后两位+月份+具体几日）     |
| `startDate` | 字符串 | `"2019-12-01 07:00:00"` | 活动开始时间                           |
| `endDate`   | 字符串 | `"2021-03-04 06:59:00"` | 活动结束时间，若年份为2099则是常驻活动 |

------

##### 示例

```json
{
  "type": 1,
  "length": 580,
  "gameEventList": [
    {
      "type": 1,
      "id": 191201,
      "startDate": "2019-12-01 07:00:00",
      "endDate": "2021-03-04 06:59:00"
    },
    {
      "type": 1,
      "id": 191202,
      "startDate": "2019-12-01 07:00:00",
      "endDate": "2021-03-04 06:59:00"
    },
    // 省略
  ]
}
```

### GetGameNgMusicIdApiMaimaiChn 获取屏蔽歌曲列表

#### 发送内容

发送为空（{}）

#### 返回内容

##### 1. **字段解释**

| 字段名        | 类型     | 必填 | 示例             | 说明                        |
| :------------ | :------- | :--- | :--------------- | :-------------------------- |
| `length`      | 整数     | 是   | `11`             | 与`musicIdList`数组的id一致 |
| `musicIdList` | 整数数组 | 是   | `[11321, 11322]` | 被屏蔽歌曲id                |

##### 2. **示例**

```
{
  "length": 11,
  "musicIdList": [11321, 11322, 11323, 11324, 11347, 11348, 11367, 11370, 11396, 11407, 11408]
}
```

### GetGameNgWordListApiMaimaiChn名称屏蔽词

#### 发送内容

发送内容为空

#### 返回内容

##### 1. **字段解释**

| 字段名                     | 类型       | 示例值           | 说明                                     |
| :------------------------- | :--------- | :--------------- | :--------------------------------------- |
| `ngWordExactMatchLength`   | 整数       | `18`             | 精确匹配敏感词数量，必须等于下方数组数量 |
| `ngWordExactMatchList`     | 字符串数组 | `["***", "***"]` | 需完全匹配的敏感词列表                   |
| `ngWordPartialMatchLength` | 整数       | `29`             | 部分匹配敏感词数量，必须等于下方数组数量 |
| `ngWordPartialMatchList`   | 字符串数组 | `["***", "***"]` | 内容包含即触发的敏感词列表（模糊匹配）   |

##### 2. **返回示例**

```json
{
  "ngWordExactMatchLength": 18,
  "ngWordExactMatchList": ["***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***"],
  "ngWordPartialMatchLength": 29,
  "ngWordPartialMatchList": ["***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***", "***"]
}
```

注意，因部分屏蔽词涉及政治，将用***代替

### UpsertClientSettingApiMaimaiChn 上传机台信息

#### 发送内容

##### 1. **根字段规则**

| 字段名          | 类型 | 说明     |
| :-------------- | :--- | :------- |
| `clientSetting` | 对象 | 机台设置 |

##### 2. **嵌套对象字段规则** (`clientSetting`内)

| 字段名       | 类型   | 示例值          | 说明                           |
| :----------- | :----- | :-------------- | :----------------------------- |
| `placeId`    | 整数   | `1003`          | 机厅id                         |
| `clientId`   | 字符串 | `"A63E01Exxxx"` | keychip狗号                    |
| `placeName`  | 字符串 | `""`            | 一般为空字符串，但字段必须存在 |
| `regionId`   | 整数   | `8`             | 地区id（如 `8=广东`）          |
| `regionName` | 字符串 | `"广东"`        | 与`regionId`对应的地区名称     |
| `bordId`     | 字符串 | `"ACAE01A9999"` | 主板ID                         |
| `romVersion` | 整数   | `100`           | 固件版本号（？）               |
| `isDevelop`  | 布尔值 | `true`          | 是否开发（？）不明意义         |
| `isAou`      | 布尔值 | `true`          | 不明意义                       |

##### 发送示例

```json
{
    "clientSetting": {
        "placeId": 1003,
        "clientId": "A63E01E1191",
        "placeName": "",
        "regionId": 8,
        "regionName": "广东",
        "bordId": "ACAE01A9999",
        "romVersion": 100,
        "isDevelop": true,
        "isAou": true
    }
}
```



#### 返回内容

| 字段名       | 类型   | 示例值                                         | 说明                             |
| :----------- | :----- | :--------------------------------------------- | :------------------------------- |
| `returnCode` | 整数   | `1`                                            | 状态码： - `1`=成功 - `非0`=失败 |
| `apiName`    | 字符串 | `"com.sega.maimai.api.UpsertClientSettingApi"` | 调用的api名称                    |

返回示例

```json
{
  "returnCode": 1,
  "apiName": "com.sega.maimai.api.UpsertClientSettingApi"
}
```

### UpsertClientTestmodeApiMaimaiChn 也许是机台进入test时发送的数据

#### 发送内容

##### 1. **根字段规则**

| 字段名           | 类型 | 必填 | 说明                   |
| :--------------- | :--- | :--- | :--------------------- |
| `clientTestmode` | 对象 | 是   | 测试模式配置的容器对象 |

##### 2. **嵌套对象字段规则** (`clientTestmode`内)

| 字段名           | 类型   | 示例值          | 说明                               |
| :--------------- | :----- | :-------------- | :--------------------------------- |
| `placeId`        | 整数   | `1003`          | 机厅id                             |
| `clientId`       | 字符串 | `"A63E01Exxxx"` | keychip狗号                        |
| `trackSingle`    | 整数   | `1`             | 也许与店内联机有关                 |
| `trackMulti`     | 整数   | `1`             | 也许与店内联机有关                 |
| `trackEvent`     | 整数   | `3`             | 事件追踪级别                       |
| `totalMachine`   | 整数   | `10`            | 机厅机台总数                       |
| `satelliteId`    | 整数   | `5`             | 卫星节点编号（用于分布式测试场景） |
| `cameraPosition` | 整数   | `1`             | 摄像头状态                         |

------

##### **完整示例**

```json
{
  "clientTestmode": {
    "placeId": 1003,
    "clientId": "A63E01Exxxx",
    "trackSingle": 1,
    "trackMulti": 1,
    "trackEvent": 3,
    "totalMachine": 10,
    "satelliteId": 5,
    "cameraPosition": 1
  }
}
```
