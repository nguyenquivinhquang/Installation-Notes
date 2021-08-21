# Update - upgrade - auto clean:

	sudo apt update -y && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt clean -y && sudo apt autoclean -y
	
# Install vietnamese keyboard
	sudo add-apt-repository ppa:bamboo-engine/ibus-bamboo && sudo apt-get update && sudo apt-get install ibus-bamboo && ibus restart
	
# Install Slimbook battery
	sudo add-apt-repository ppa:slimbook/slimbook
	sudo apt update
	sudo apt install slimbookbattery

# Install gnome-tweaks:
	sudo apt install gnome-tweaks

# Limit batterry charge: (Asus)
First, open terminal and run:

	sudo nano /etc/crontab
Second, add this line and save:
		
	@reboot root echo 80 > =/sys/class/power_supply/BAT0/charge_control_end_threshold


# Turn off auto conda base:
	conda config --set auto_activate_base false
