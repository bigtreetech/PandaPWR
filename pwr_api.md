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