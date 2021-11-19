
# De Slimme meter uitlezen met prometheus en grafana

### Deployed on

* lenovo T580 & fedora 35 - x86
* raspberry pi 4 & fedora 35 - aarch64

### Requierments

* image build 'de slimme meter exporter gotsmart': https://github.com/Nozzie/gotsmart
* ansible=2.11 
* ansible-galaxy collection install containers.podman

### deploy

```bash
# ansible-playbook -b -l dsm deploy-dsm.yaml 
```

### End points

* grafana: http://dsm:3000
* prometheus: http://dsm:9090
* node exporter: http://dsm:9100/metrics
* dsm exporter: http://dsm:8080/metrics
* alertmanager: http://dsm:9093

### debug USB to serial cable to connect to the smart meter

```bash
root@dsm ~]# ll /dev/serial/by-id/usb-FTDI_FT232R_USB_UART_AC2YPOKN-if00-port0
lrwxrwxrwx. 1 root root 13 Nov  4 01:00 /dev/serial/by-id/usb-FTDI_FT232R_USB_UART_AC2YPOKN-if00-port0 -> ../../ttyUSB0

root@dsm ~]# chmod 666 /dev/ttyUSB0
root@dsm ~]# stty -F /dev/ttyUSB0 115200

[root@dsm ~]# cat < /dev/ttyUSB0 
-1012

1-3:0.2.8(50)
0-0:1.0.0(211119173247W)
0-0:96.1.1(4530303433303037303435303838353138)
1-0:1.8.1(006462.887*kWh)
1-0:1.8.2(007905.585*kWh)
1-0:2.8.1(000000.000*kWh)
1-0:2.8.2(000000.000*kWh)
0-0:96.14.0(0002)
1-0:1.7.0(00.173*kW)
1-0:2.7.0(00.000*kW)
0-0:96.7.21(00009)
0-0:96.7.9(00003)
1-0:99.97.0(1)(0-0:96.7.19)(200420123608S)(0000000430*s)
1-0:32.32.0(00126)
1-0:32.36.0(00001)
0-0:96.13.0()
1-0:32.7.0(224.0*V)
1-0:31.7.0(001*A)
1-0:21.7.0(00.209*kW)
1-0:22.7.0(0/ISK5\2M550E-1012
```
