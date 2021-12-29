
# De Slimme meter uitlezen met prometheus en grafana

* de slimme meter exporter gotsmart
* prometheus
* node exporter
* grafana
* alertmanager (alerts to slack)
* influxdb (voor toevoegen buitentemperatuur, fijnstof en luchtvochtiheid)

### Deployed on

* lenovo T580 & fedora 35 - x86
* raspberry pi 4 & fedora 35 - aarch64

### Requierments

* pip install ansible 
* ansible-galaxy collection install containers.podman
* slimme meter
* P1 kabel

### Prepare deployment

Replace encrypted ansible-vault secrets with your own.


```bash
$ ansible-vault encrypt_string 'mypassword' -n _privatekey_passphrase
_privatekey_passphrase: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          61613566393936313337636339663961346435663330643034656632336265313938373436306634
          3339623864386531633637643935353433343766366433350a383365376235303434313233616632
          33303065393838346130383264386539656231303866653336353231373636386266633034633161
          3839613236366138310a356435613931636466613033306436336561393163636135386363383230
          3838
Encryption successful
```

 ansible all -m shell -a  'echo "{{_privatekey_passphrase}}"' -e@roles/dsm/defaults/main.yml


### deploy

```bash
# ansible-playbook -b -l dsm deploy-dsm.yaml 
```

### End points

* grafana: https://dsm:3000
* prometheus: https://dsm:9090
* node exporter: https://dsm:9100/metrics
* dsm exporter: http://dsm:8080/metrics
* alertmanager: https://dsm:9093
* karma: https://dsm:8081/metrics
* blackbox: https://dsm:9115

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

### resources

* de slimme meter exporter gotsmart: https://raw.githubusercontent.com/Nozzie/gotsmart

### grafana dashboard

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard1.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard1.png"
alt="dashboard1" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard2.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard2.png"
alt="dashboard2" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard3.png"
 target="_blank3"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard3.png"
alt="dashboard" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard4.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard4.png"
alt="dashboard4" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard5.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard5.png"
alt="dashboard5" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard6.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard6.png"
alt="dashboard6" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard7.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard7.png"
alt="dashboard7" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard8.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard8.png"
alt="dashboard8" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard9.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard9.png"
alt="dashboard9" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard10.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard10.png"
alt="dashboard10" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard11.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard11.png"
alt="dashboard11" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard12.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard12.png"
alt="dashboard12" width="800" height=auto border="10" /></a><br>

<a href="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard13.png"
 target="_blank"><img src="https://raw.githubusercontent.com/tedsluis/dsm-prometheus-grafana/main/media/smartdashboard13.png"
alt="dashboard13" width="800" height=auto border="10" /></a><br>