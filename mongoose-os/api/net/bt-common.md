# BLE
| Github Repo | C Header | C source  | JS source |
| ----------- | -------- | --------  | ----------------- |
| [mongoose-os-libs/bt-common](https://github.com/mongoose-os-libs/bt-common) | [mgos_bt.h](https://github.com/mongoose-os-libs/bt-common/blob/master/include/mgos_bt.h) | &nbsp;  | [api_bt_gap.js](https://github.com/mongoose-os-libs/bt-common/blob/master/mjs_fs/api_bt_gap.js)         |



Currently contains only GATT server implmenetation for ESP32.

## Configuration section

`bt-common` library adds a `bt` configuration section with the following
settings:

```
"bt": {
  "enable": true,             // Enabled by default. Disabled on first reboot with WiFi on
  "dev_name": "",             // Device name. If empty, value equals to device.id
  "adv_enable": true,         // Advertise our Bluetooth services
  "keep_enabled": true,       // Keep enabled after successful boot with WiFi on
  "scan_rsp_data_hex": "",    // Custom scan response data, as hex string (e.g. `48656c6c6f` for `Hello`)
  "allow_pairing": true,      // Allow pairingbonding with other devices
  "max_paired_devices": 10,   // Allow pairing with up to this many devices; -1 - no limit
  "gatts": {
    "min_sec_level": 0,       // Minimum security level for all attributes of all services.
                              // 0 - no auth required, 1 - encryption reqd, 2 - encryption + MITM reqd
    "require_pairing": false  // Require taht device is paired before accessing services
  }
}
```

## Security

Default settings allow for unrestricted access: anyone can pair with a device and access the services.
A better idea is to set `bt.gatts.require_pairing` to true, `bt.allow_pairing` to false and only enable it for a limited time via `mgos_bt_gap_set_pairing_enable` when user performs some action, e.g. presses a button.
Raising `bt.gatts.min_sec_level` to at least 1 is also advisable.
_Note_: At present, level 2 (MITM protection) is not usable as it requires device to have at least output capability during pairing, and there's no API for displaying the pairing code yet.


 ----- 
#### (sizeof

```c
#define MGOS_BT_ADDR_STR_LEN (sizeof(struct mgos_bt_addr) * 3 + 2 /* type */)
#define MGOS_BT_UUID_STR_LEN (sizeof(struct mgos_bt_uuid) * 3)
#define MGOS_BT_DEV_NAME_LEN 32
```
>  Each byte is transformed into 3 bytes: "XX:", and last byte into "XX\0" 

### JS API

 --- 
#### GAP.parseName

```javascript
GAP.parseName(advData)
```
Parse name from adv data. Tries to get long, falls back to short.
