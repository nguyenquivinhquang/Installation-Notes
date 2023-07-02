# Limit battery charging to 80 capacity Ubuntu

## 1. [Install TLP](https://linrunner.de/tlp/faq/installation.html):
```bash
sudo add-apt-repository ppa:linrunner/tlp
sudo apt update
sudo apt install tlp
# Check what package needed for battery:
sudo tlp-stat -b

# If acpi_call is recommended
sudo apt install acpi-call-dkms

# If smapi is recommended
sudo apt install tp-smapi-dkms
```

## 2. Open config file of TLP
```bash
sudo gedit /etc/tlp.conf
```

## 3. Find the lines regarding battery settings, remove the leading # for comment and maybe insert the value you want
```bash
START_CHARGE_THRESH_BAT0=75
STOP_CHARGE_THRESH_BAT0=80
```

## 4. Restart tlp
```
sudo tlp start
```

## 5. Error: 
if occur error: 
```
>>> Invoke 'systemctl mask power-profiles-daemon.service' to correct this!
```

Run:
```
sudo apt remove power-profiles-daemon
```


**Source**: https://askubuntu.com/questions/34452/how-can-i-limit-battery-charging-to-80-capacity
