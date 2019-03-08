+++
author = "Alexander Sparkowsky"
categories = ["linux", "virtualbox", "howto", "ubuntu"]
date = 2019-03-08T12:00:00Z
description = "How to setup vboxautostart on Ubuntu 18.04."
draft = false
slug = "vboxautostart-service"
tags = ["linux", "virtualbox", "howto", "ubuntu"]
title = "vboxautostart Setup on Ubuntu 18.04"

+++
## Setup vboxautostart
```
# set environment variables
sudo tee -a /etc/default/virtualbox <<'EOF'
VBOXAUTOSTART_DB=/etc/vbox
VBOXAUTOSTART_CONFIG=/etc/vbox/vboxauto.conf
EOF

# set vboxautostart policy
sudo tee -a /etc/vbox/vboxauto.conf << "EOF"
default_policy = deny
$USER = {
    allow = true
}
EOF

sudo chgrp vboxusers /etc/vbox
sudo chmod 1775 /etc/vbox
sudo usermod -aG vboxusers $USER
VBoxManage setproperty autostartdbpath /etc/vbox

sudo service vboxautostart-service restart
sudo reboot
```

## Enable autostart for Your VM
```
VBoxManage -nologo list vms
VM="Name_Of_Your_VM"
VBoxManage modifyvm "${VM}" --autostart-enabled on --autostop-type acpishutdown
sudo systemctl stop vboxautostart-service
sudo systemctl start vboxautostart-service
```
