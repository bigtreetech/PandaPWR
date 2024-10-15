# Basics

## URL

Assuming the IP address of PWR is `192.168.4.1` then the base path is `http://192.168.4.1`
  
## Power switch control

### POST http://192.168.4.1/set

#### switch off
**Body raw(not json)**  
```
power=0
```

#### switch on
**Body raw**  
```
power=1
```
<img src=img/power_on.png width="600"/>

## USB1 switch control

### POST http://192.168.4.1/set

#### switch off
**Body raw**  
```
usb=0
```

#### switch on
**Body raw**  
```
usb=1
```
<img src=img/usb_on.png width="600"/>

## Other parameters format 
| parameter | detail|
| :-----| :----: |
| factory | 1:restore the setting |   
| reset_usage | 1:reset the power usage |
| countdown_ctl|0:stop countdown 1:start countdown 2:pause by Panda Touch 3:pause by web|      
| countdown_val|1-(86400)|   
| auto_poweroff|0:switch off 1:switch on|   

## multi parameters

### POST http://192.168.4.1/set

If we need to turn on both power and usb1,we can use multi parameters like this:
**Body raw**  
```
usb=1&power=1
```
<img src=img/multi_parameters.png width="600"/>

## update the power data
### GET http://192.168.4.1/update_ele_data
### PWR response a json:
~~~
{
  "countdown_state": 0,
  "auto_poweroff": "0",
  "countdown": "0",
  "voltage": 229,
  "current": 0,
  "power": 0,
  "power_state": 1,
  "usb_state": 0,
  "ele": 0
}
~~~

## communicate with PWR by a command
#### Frame Format
| Frame Head | Command | Data Length | Data         | Checksum | Frame Tail |
|------------|---------|-------------|--------------|----------|------------|
| AA         | XX      | N           | Byte0-Byten  | crc16    | 5AA5       |

### GET http://192.168.3.71/get_power
 
#### Example

| Byte Range      | Description              | Example Value                         | Notes                                   |
|-----------------|--------------------------|--------------------------------------|-----------------------------------------|
| `0x00`          | Start Identifier         | `aa`                                 | Fixed start identifier                  |
| `0x01`          | Command Type             | `01`                                 | Get status                              |
| `0x02`          | Data Length              | `3c`                                 |                                         |
| `0x03`          | Error Code               | `00`                                 |                                         |
| `0x04 - 0x0F`   | IP Address               | `31 39 32 2e 31 36 38 2e 33 2e 37 31` | ASCII values of the string `192.168.3.71` |
| `0x10 - 0x13`   | Reserved                 | `00 00 00 00`                        | Reserved field                          |
| `0x14 - 0x1F`   | Voltage                  | `fb e9 64 43`                        | 32-bit float (voltage value)          |
| `0x20 - 0x23`   | Current                  | `00 00 00 00`                        | 32-bit float (current value)          |
| `0x24 - 0x27`   | Power                    | `00 00 00 00`                        | 32-bit float (power value)            |
| `0x28 - 0x2B`   | Energy                   | `00 00 00 00`                        | 32-bit float (energy value)           |
| `0x2C - 0x2F`   | Frequency                | `46 55 44 42`                        | 32-bit float (frequency value)        |
| `0x30 - 0x33`   | Switch State             | `01 00 00 00`                        | `power`, `auto_power_off`, `countdown`, `usb`, `usb_follow` |
| `0x34 - 0x3B`   | auto power off                 | `00 00 00 00 00 00 00 00`            | auto power off config                           |
| `0x3C - 0x3D`   | Status End Identifier    | `f3 ad`                              | CRC16                                   |
| `0x3E - 0x3F`   | Status End Identifier    | `5a a5`                              | Fixed end identifier                    |

