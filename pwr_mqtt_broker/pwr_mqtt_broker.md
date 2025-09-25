# new pwr with mqtt broker

## Network Configuration

* The device will create a hotspot named **"Panda_PWR_FCE8C02BF288"**, with the default password **"987654321"**.

* Connect to this hotspot using your mobile phone or computer.

    <img src=img/ap_select.png width="300"/>

* Open a browser and enter: **[http://192.168.254.1/](http://192.168.254.1/)**

* After scanning for Wi-Fi, choose the target network, enter its password, and click **Connect**.

    <img src=img/ap_connect.png width="300"/>

* You can check the ip now.

    <img src=img/check_sta_ip.png width="300"/> 

## Mqtt broker
| parameter | detail|
| :-----| :----: | 
| Topic | pandapwr/state |
| Client Id | A unique one |
| Host|mqtt://192.168.3.190(replace it with a actual one)|      
| port|1883|   
| Username|null|   
| Password|null|   

## mqtt broker command 
* pwr control
~~~
{
    "sequence_id": "0",
    "command": "set.panda_pwr"
    "data": {
    "power_mode": true, // boolean: true: turn on 220V 
                            //          false: turn off 220V 
    "usb_mode": true, // boolean: true: turn on usb1
                          //          false: turn off usb1
    "reset_usage": true,// boolean: true:  
    "countdown_ctl": 1,// int 0:turn off 1：turn on 2：reserve 3：pause
    "countdown_val": 60, // int
    }
}
~~~

* factory restore
~~~
{
    "sequence_id": "0",
    "command": "set.factory_reset" 
} 
~~~

* report all data
~~~
{
    "type": "data",
    "device": "panda_pwr",
    "data": {
        "type": "part", //"all","part" 
        "power_mode": true,
        "usb_mode": true,
        "voltage": "220V", // string
        "current": "0.13A", // string
        "power": "15W", // string
        "energy_usage": "0.039KWh", // string
        "countdown_ctl": 0, // int: current seconds
        "countdown_val": 60, // int: target seconds
        "countdown_current_val": 59, // int
    }
}
~~~