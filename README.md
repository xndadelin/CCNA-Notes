---
icon: flask-vial
---

# Miscellaneous

## Password recovery

1. Turn off the router, then turn it on back, while its booting (< 60 sec) press the "BREAK" button on keyboard or just CTRL+C.&#x20;
2. Now you are in ROMmon 1, type this command in the prompt to enter ROMmon 2 and follow the next commands.

```
rommon 1 > confreg 0x2142
rommon 2 > reset
Router> enable
Router#copy startup-config running-config !to load up the NVRAM
....
Router(config)#config-register 0x2102
Router(config)#exit
Router#copy running-config startup-config
```
