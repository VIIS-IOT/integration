# VIIS DEVICE API INTEGRATION

## Menu
1. [Webhook](#webhook)
    - 1.1 [Device Event](#device-event)
    - 1.2 [Timeseries Event](#timeseries-event)
2. [Device and Device Profile API](#device-and-device-profile-api)
    - 2.1 [List Device](#list-device)
    - 2.2 [List Device Profile](#list-device-profile)
    - 2.3 [Device timeseries data](#device-timeseries-data)
    - 2.4 [Control device](#control-rpc)


---

## Demo credentials

1. [smartfarm.viis.tech](http://smartfarm.viis.tech) account:

   ```json
   {
     "usr": "foodmap@gmail.com",
     "pwd": "123456"
   }
   ```

## <a id="webhook"></a>Webhook
### <a id="device-event"></a>Device Event

Bridge data qua endpoint của khách hàng qua phương thức `POST`

Example body:

`DEVICE-CREATE`:

```json
{
  "event": "DEVICE-CREATE",
  "data": {
    "name": "cf991cf0-425b-11ef-b623-15218337b1e8",
    "label": null,
    "modified": null,
    "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
    "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
    "is_gateway": 0,
    "serial_number": "DEMO_FOODMAP_05",
    "device_id_thingsboard": "cf991cf0-425b-11ef-b623-15218337b1e8",
    "access_token_thingsboard": "ffcc312e-81d2-4e88-bdb8-06ee8f5678df",
    "device_profile": null,
    "customer_name": null,
    "description": null
  },
  "entity": "device",
  "action": 1,
  "timestamp": "2024-07-15 03:39:16.533000"
}
```

`DEVICE-UPDATE`:

```json
{
  "event": "DEVICE-UPDATE",
  "data": {
    "name": "fee461f0-411a-11ef-b623-15218337b1e8",
    "label": "DEMO_FOODMAP_02",
    "modified": "2024-07-15 10:33:29.737195",
    "is_gateway": 0,
    "serial_number": "DEMO_FOODMAP_02",
    "device_id_thingsboard": "fee461f0-411a-11ef-b623-15218337b1e8",
    "access_token_thingsboard": "13715dd9-4f28-44b0-aed6-c242eb20753d",
    "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
    "customer_name": "Foodmap Integration",
    "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
    "device_profile": "Foodmap demo device"
  },
  "entity": "device",
  "action": 3,
  "timestamp": "2024-07-15 03:33:29.773000"
}
```

`DEVICE-DETACH`

```json
{
  "event": "DEVICE-DETACH",
  "data": {
    "name": "cf991cf0-425b-11ef-b623-15218337b1e8"
  },
  "entity": "device",
  "action": 5,
  "timestamp": "2024-07-15 03:40:23.758000"
}
```

_Note:_

```js
//entity
export enum WebhookEntity {
  DEVICE = 'device' //hien tai chi dang dung 1 loai la device
}

//action
export enum WebhookActionCode {
  CREATE = 1, // CREATE entity
  READ = 2, // READ entity
  UPDATE = 3, // UPDATE entity
  DELETE = 4, // DELETE entity
  DETACH = 5, // DETACH entity
}
```

</details>

### <a id="timeseries-event"></a>Timeseries Event

Khi VIIS đăng ký thiết bị cho khách hàng, data telemetry từ device sẽ được bridge qua endpoints mà khách hàng cung cấp.

Example body:

```json
[
  {
    "ts": "1721014991099",
    "key": "deviceId",
    "value": "fee461f0-411a-11ef-b623-15218337b1e8"
  },
  {
    "ts": "1721014991099",
    "key": "temp",
    "value": 99
  },
  {
    "ts": "1721014991099",
    "key": "humid",
    "value": 85
  },
  {
    "ts": "1721014991099",
    "key": "state",
    "value": true
  },
  {
    "ts": "1721014991099",
    "key": "online",
    "value": true
  }
]
```

## Integration APIs
## <a id="device-and-device-profile-api"></a>Device and Device Profile API

VIIS Endpoint: `https://iot.viis.tech/api/v2/developer`

Authorization: Basic Auth

### <a id="list-device"></a>List Device

### `/device`

### `params`

1. **`page`**
   - **Mặc định**: `1`
   - **Mô tả**: Số trang hiện tại để phân trang.
2. **`size`**
   - **Mặc định**: `1`
   - **Mô tả**: Số lượng kết quả trên mỗi trang.
3. **`order_by`**
   - **Mặc định**: `null`
   - **Mô tả**: Điều kiện để sắp xếp kết quả.
4. **`filters`**

   - **Mặc định**: `'[]'`
   - **Mô tả**: Các điều kiện lọc dữ liệu dưới dạng string array.
   - Ví dụ: `[["iot_device","name","like","fee461f0-411a-11ef-b623-15218337b1e8"]]`

   Example response:

   ```json
   {
     "result": {
       "data": [
         {
           "name": "fee461f0-411a-11ef-b623-15218337b1e8",
           "id": null,
           "create_time": null,
           "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
           "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
           "type": null,
           "label": "DEMO_FOODMAP_02",
           "title": null,
           "firmware_id": null,
           "software_id": null,
           "is_gateway": 0,
           "device_id_thingsboard": "fee461f0-411a-11ef-b623-15218337b1e8",
           "access_token_thingsboard": "13715dd9-4f28-44b0-aed6-c242eb20753d",
           "device_profile": "Foodmap demo device",
           "zone_id": "a69d1ccf68",
           "customer_name": "Foodmap Integration",
           "image": "",
           "description": null,
           "serial_number": "DEMO_FOODMAP_02",
           "index": "0",
           "sort_index": "0",
           "project_id": "7dd3b73494",
           "project_label": "Demo",
           "zone_label": "Demo zone 1",
           "function_list": [
             {
               "name": "c9124736f8",
               "label": "Nhiệt độ",
               "identifier": "temp",
               "data_type": "Value",
               "icon_url": null,
               "data_on_text": "On",
               "data_off_text": "Off",
               "enum_value": "",
               "unit": "Celcius",
               "data_permission": "r",
               "description": null,
               "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
               "data_measure_max": "100",
               "data_measure_min": "0",
               "data_eligible_max": "100",
               "data_eligible_min": "0",
               "checkbox_bit_label1": null,
               "checkbox_bit_label2": null,
               "checkbox_bit_label3": null,
               "checkbox_bit_label4": null,
               "checkbox_bit_label5": null,
               "checkbox_bit_label6": null,
               "checkbox_bit_label7": null,
               "checkbox_bit_label8": null,
               "chart_type": "Line",
               "round_type": "Raw",
               "is_hidden": null,
               "index_sort": 1
             },
             {
               "name": "4f031ad75b",
               "label": "state",
               "identifier": "state",
               "data_type": "Bool",
               "icon_url": null,
               "data_on_text": "On",
               "data_off_text": "Off",
               "enum_value": "",
               "unit": "",
               "data_permission": "rw",
               "description": null,
               "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
               "data_measure_max": "0",
               "data_measure_min": "0",
               "data_eligible_max": "0",
               "data_eligible_min": "0",
               "checkbox_bit_label1": null,
               "checkbox_bit_label2": null,
               "checkbox_bit_label3": null,
               "checkbox_bit_label4": null,
               "checkbox_bit_label5": null,
               "checkbox_bit_label6": null,
               "checkbox_bit_label7": null,
               "checkbox_bit_label8": null,
               "chart_type": "Text_Line",
               "round_type": "Raw",
               "is_hidden": null,
               "index_sort": 1
             },
             {
               "name": "c673e14609",
               "label": "Độ ẩm",
               "identifier": "humid",
               "data_type": "Value",
               "icon_url": null,
               "data_on_text": "On",
               "data_off_text": "Off",
               "enum_value": "",
               "unit": "%",
               "data_permission": "r",
               "description": null,
               "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
               "data_measure_max": "100",
               "data_measure_min": "0",
               "data_eligible_max": "100",
               "data_eligible_min": "0",
               "checkbox_bit_label1": null,
               "checkbox_bit_label2": null,
               "checkbox_bit_label3": null,
               "checkbox_bit_label4": null,
               "checkbox_bit_label5": null,
               "checkbox_bit_label6": null,
               "checkbox_bit_label7": null,
               "checkbox_bit_label8": null,
               "chart_type": "Line",
               "round_type": "Float_2",
               "is_hidden": null,
               "index_sort": 2
             }
           ],
           "device_profile_image": "/private/files/microchip.addc6a72-2a7d-4046-92d7-60520809e155.png",
           "latest_data": [
             {
               "key": "lastDisconnectTime",
               "ts": 1720881208224,
               "value": 1720881208213
             },
             {
               "key": "lastConnectTime",
               "ts": 1720881208224,
               "value": 1720881208224
             },
             {
               "key": "method",
               "ts": 1720881632453,
               "value": "set_state"
             },
             {
               "key": "params",
               "ts": 1720881632453,
               "value": {
                 "state": false
               }
             },
             {
               "key": "humid",
               "ts": 1721015984274,
               "value": 13
             },
             {
               "key": "state",
               "ts": 1721015984274,
               "value": true
             },
             {
               "key": "online",
               "ts": 1721015984274,
               "value": true
             },
             {
               "key": "temp",
               "ts": 1721015984274,
               "value": 99
             }
           ],
           "IP": "",
           "online": true,
           "lastDisconnectTime": 1720881208213,
           "lastConnectTime": 1720881208224
         }
       ],
       "pagination": {
         "totalElements": 1,
         "totalPages": 1,
         "currentPage": 2,
         "pageSize": 1
       }
     }
   }
   ```

### <a id="list-device-profile"></a>List Device Profile

### `/device/device-profile`

### `params`

1. **`page`**
   - **Mặc định**: `1`
   - **Mô tả**: Số trang hiện tại để phân trang.
2. **`size`**
   - **Mặc định**: `1`
   - **Mô tả**: Số lượng kết quả trên mỗi trang.
3. **`filters`**
   - **Mặc định**: `'[]'`
   - **Mô tả**: Các điều kiện lọc dữ liệu dưới dạng string array.
   - Ví dụ: `[["iot_device_profile","name","like","dce00150-401a-11ef-b623-15218337b1e8"]]`

Example response:

```json
{
  "result": {
    "data": [
      {
        "name": "dce00150-401a-11ef-b623-15218337b1e8",
        "tb_device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
        "create_time": null,
        "label": "Foodmap demo device",
        "profile_type": "DEFAULT",
        "profile_image": "/private/files/microchip.addc6a72-2a7d-4046-92d7-60520809e155.png",
        "transport_type": "DEFAULT",
        "provision_type": "DISABLED",
        "profile_data": null,
        "profile_description": "Foodmap demo device",
        "is_default": 0,
        "firmware_id": null,
        "software_id": null,
        "default_rule_chain_id": null,
        "default_dashboard_id": null,
        "default_queue": null,
        "provision_device_key": null,
        "functions": [
          {
            "name": "c9124736f8",
            "type": "Property",
            "label": "Nhiệt độ",
            "identifier": "temp",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "Celcius",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Raw",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "c9124736f8",
            "type": "Property",
            "label": "Nhiệt độ",
            "identifier": "temp",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "Celcius",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Raw",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "c9124736f8",
            "type": "Property",
            "label": "Nhiệt độ",
            "identifier": "temp",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "Celcius",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Raw",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "c9124736f8",
            "type": "Property",
            "label": "Nhiệt độ",
            "identifier": "temp",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "Celcius",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Raw",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "c673e14609",
            "type": "Property",
            "label": "Độ ẩm",
            "identifier": "humid",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "%",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Float_2",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 2
          },
          {
            "name": "c673e14609",
            "type": "Property",
            "label": "Độ ẩm",
            "identifier": "humid",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "%",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Float_2",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 2
          },
          {
            "name": "c673e14609",
            "type": "Property",
            "label": "Độ ẩm",
            "identifier": "humid",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "%",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Float_2",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 2
          },
          {
            "name": "c673e14609",
            "type": "Property",
            "label": "Độ ẩm",
            "identifier": "humid",
            "data_type": "Value",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "%",
            "data_permission": "r",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "100",
            "data_measure_min": "0",
            "data_eligible_max": "100",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Line",
            "round_type": "Float_2",
            "md_size": 1,
            "show_chart": 1,
            "index_sort": 2
          },
          {
            "name": "4f031ad75b",
            "type": "Property",
            "label": "state",
            "identifier": "state",
            "data_type": "Bool",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "",
            "data_permission": "rw",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "0",
            "data_measure_min": "0",
            "data_eligible_max": "0",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Text_Line",
            "round_type": "Raw",
            "md_size": 2,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "4f031ad75b",
            "type": "Property",
            "label": "state",
            "identifier": "state",
            "data_type": "Bool",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "",
            "data_permission": "rw",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "0",
            "data_measure_min": "0",
            "data_eligible_max": "0",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Text_Line",
            "round_type": "Raw",
            "md_size": 2,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "4f031ad75b",
            "type": "Property",
            "label": "state",
            "identifier": "state",
            "data_type": "Bool",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "",
            "data_permission": "rw",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "0",
            "data_measure_min": "0",
            "data_eligible_max": "0",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Text_Line",
            "round_type": "Raw",
            "md_size": 2,
            "show_chart": 1,
            "index_sort": 1
          },
          {
            "name": "4f031ad75b",
            "type": "Property",
            "label": "state",
            "identifier": "state",
            "data_type": "Bool",
            "icon_url": null,
            "data_on_text": "On",
            "data_off_text": "Off",
            "enum_value": "",
            "unit": "",
            "data_permission": "rw",
            "description": null,
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "data_measure_max": "0",
            "data_measure_min": "0",
            "data_eligible_max": "0",
            "data_eligible_min": "0",
            "checkbox_bit_label1": null,
            "checkbox_bit_label2": null,
            "checkbox_bit_label3": null,
            "checkbox_bit_label4": null,
            "checkbox_bit_label5": null,
            "checkbox_bit_label6": null,
            "checkbox_bit_label7": null,
            "checkbox_bit_label8": null,
            "chart_type": "Text_Line",
            "round_type": "Raw",
            "md_size": 2,
            "show_chart": 1,
            "index_sort": 1
          }
        ],
        "devices": [
          {
            "name": "d3083e70-411b-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_03",
            "device_id_thingsboard": "d3083e70-411b-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "c3e2a9ec-ca49-4748-aa6c-20b9fe806f09",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_03",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "04a3d430-411c-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_04",
            "device_id_thingsboard": "04a3d430-411c-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "5ad1f07f-e6de-486c-85f0-b099023a39f9",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": null,
            "zone_name": null,
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_04",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_01",
            "device_id_thingsboard": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "7d9e3809-dedb-4bd2-bdc2-741d586fac5d",
            "create_time": null,
            "description": "DEMO_FOODMAP_01",
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_01_active",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "fee461f0-411a-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_02",
            "device_id_thingsboard": "fee461f0-411a-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "13715dd9-4f28-44b0-aed6-c242eb20753d",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_02",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "d3083e70-411b-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_03",
            "device_id_thingsboard": "d3083e70-411b-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "c3e2a9ec-ca49-4748-aa6c-20b9fe806f09",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_03",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "04a3d430-411c-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_04",
            "device_id_thingsboard": "04a3d430-411c-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "5ad1f07f-e6de-486c-85f0-b099023a39f9",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": null,
            "zone_name": null,
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_04",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_01",
            "device_id_thingsboard": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "7d9e3809-dedb-4bd2-bdc2-741d586fac5d",
            "create_time": null,
            "description": "DEMO_FOODMAP_01",
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_01_active",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "fee461f0-411a-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_02",
            "device_id_thingsboard": "fee461f0-411a-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "13715dd9-4f28-44b0-aed6-c242eb20753d",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_02",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "d3083e70-411b-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_03",
            "device_id_thingsboard": "d3083e70-411b-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "c3e2a9ec-ca49-4748-aa6c-20b9fe806f09",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_03",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "04a3d430-411c-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_04",
            "device_id_thingsboard": "04a3d430-411c-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "5ad1f07f-e6de-486c-85f0-b099023a39f9",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": null,
            "zone_name": null,
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_04",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_01",
            "device_id_thingsboard": "bce4ea10-401e-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "7d9e3809-dedb-4bd2-bdc2-741d586fac5d",
            "create_time": null,
            "description": "DEMO_FOODMAP_01",
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_01_active",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          },
          {
            "name": "fee461f0-411a-11ef-b623-15218337b1e8",
            "id": null,
            "is_gateway": 0,
            "serial_number": "DEMO_FOODMAP_02",
            "device_id_thingsboard": "fee461f0-411a-11ef-b623-15218337b1e8",
            "access_token_thingsboard": "13715dd9-4f28-44b0-aed6-c242eb20753d",
            "create_time": null,
            "description": null,
            "additional_info": null,
            "customer_id": "d4aa2cbc-425d-4843-a979-59a445779e76",
            "device_profile_id": "dce00150-401a-11ef-b623-15218337b1e8",
            "zone_id": "a69d1ccf68",
            "zone_name": "Demo zone 1",
            "device_data": null,
            "type": null,
            "label": "DEMO_FOODMAP_02",
            "firmware_id": null,
            "software_id": null,
            "customer_name": "Foodmap Integration",
            "image": "",
            "sort_index": 0
          }
        ]
      }
    ],
    "pagination": {
      "totalElements": 1,
      "totalPages": 1,
      "currentPage": 1,
      "pageSize": 1
    }
  }
}
```

### <a id="device-timeseries-data"></a>Device timeseries data


### `/device/get-device-timeseries-history/:id?keys=key1,key2&startTs=1720683261837&endTs=1720769661837&agg=AVG&limit=150&interval=576000`

### **Params:**

- `:id` (Path Parameter): Đây là ID của thiết bị mà bạn muốn truy xuất dữ liệu lịch sử. ID này được cung cấp trong URL của yêu cầu.
  Ví dụ: `/api/v2/device/developer/get-device-timeseries-history/12345` sẽ truy xuất dữ liệu từ thiết bị có ID là `12345`.
- `keys` (Query Parameter): Danh sách các khóa (keys) của dữ liệu thời gian mà bạn muốn truy xuất, cách nhau bằng dấu phẩy.
  Ví dụ: `keys=temperature,humidity` sẽ truy xuất các dữ liệu thời gian liên quan đến nhiệt độ và độ ẩm.
- `startTs` (Query Parameter): Thời gian bắt đầu của khoảng thời gian cần truy xuất dữ liệu, tính bằng mili giây kể từ Unix epoch (1970-01-01 00:00:00 UTC).
  Ví dụ: `startTs=1720683261837` đại diện cho một thời điểm cụ thể trong tương lai.
- `endTs` (Query Parameter): Thời gian kết thúc của khoảng thời gian cần truy xuất dữ liệu, tính bằng mili giây kể từ Unix epoch.
  Ví dụ: `endTs=1720769661837` đại diện cho một thời điểm cụ thể trong tương lai.
- `agg` (Query Parameter): Loại phép tính tổng hợp (aggregation) được sử dụng để tính toán dữ liệu trong các khoảng thời gian nhất định. Các giá trị phổ biến có thể bao gồm `AVG` (trung bình), `MIN` (giá trị nhỏ nhất), `MAX` (giá trị lớn nhất), `SUM` (tổng).
  Ví dụ: `agg=AVG` sẽ trả về giá trị trung bình của các dữ liệu trong khoảng thời gian được chỉ định.
- `limit` (Query Parameter): Giới hạn số lượng bản ghi được trả về.
  Ví dụ: `limit=150` sẽ trả về tối đa 150 bản ghi.
- `interval` (Query Parameter): Khoảng thời gian giữa các điểm dữ liệu trong kết quả, tính bằng mili giây.
  Ví dụ: `interval=576000` (khoảng 6 phút) sẽ nhóm và tổng hợp các dữ liệu trong các khoảng thời gian 6 phút.

### <a id="control-rpc"></a>Control device

### `/device/rpc/oneway/:id`

### **Body:**

```json
{
  "method": "set_state",
  "params": {
    "key_name": "value" //ví dụ: "button_1": false
  },
  "timeout": 10000
}
```

Expected response:

```json
{
  "result": ""
}
```
