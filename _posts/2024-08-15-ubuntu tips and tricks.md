---
layout: post
title: Ubuntu Tips and Tricks
date: 2024-08-15 21:01:00
# description: this is what included images could look like
# tags: formatting images
# categories: sample-posts
# thumbnail: assets/img/9.jpg
---



**Setting up integrated Clipboard manager similar to the functionality of Windows + v (contains all the ctrl+c)**\
Install using the following commands:
```shell
sudo add-apt-repository ppa:diodon-team/stable
sudo apt-get update
sudo apt-get install -y diodon
```
After installtion, we need to set the shortcut. For GNOME/Unity this can be done by opening Settings->keyboard. There you go to View and Customize Shortcuts -> Custom Shortcuts and add a new one with the name “Diodon” and command “/usr/bin/diodon”. Afterwards assign your preferred hotkey and you are done.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/blogs/diodon.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


**Setting up power management**\
Install using the following commands:
```shell
sudo add-apt-repository ppa:linrunner/tlp
sudo apt update
sudo apt install tlp tlp-rdw
sudo systemctl enable tlp.service
```

**Setting up your favourite video player**\
Install using the following command:
```shell
sudo snap install vlc
```

**Setting up your favourite code editor**\
Download the latest .deb file from the followiing link: <a href="https://go.microsoft.com/fwlink/?LinkID=760868">https://go.microsoft.com/fwlink/?LinkID=760868</a>\
Install using the following command:
```shell
sudo apt install ./<file>.deb
```

**Setting up your favouite browser**\
Download the latest .deb file from the followiing link: <a href="https://www.google.com/chrome/?platform=linux">https://www.google.com/chrome/?platform=linux</a>\
Install using the following command:
```shell
sudo apt install ./<file>.deb
```

**Setting up your facourite image editing software**
Install using the following command:
```shell
sudo apt install gimp
```

**Setting up your favouirte Terminal**\
Install using the following commands:
```shell
sudo apt-get update
sudo apt-get install terminator
```

Next, we need to add the *Open in Terminator* to filemanager menu as shown in the following figure.

<div class="row mt-1">
    <div class="col-sm mt-1">
        {% include figure.liquid loading="eager" path="assets/img/blogs/open_in_terminator.png" class="img-fluid rounded z-depth-1" zoomable=true width="300" %}
    </div>
</div>

Install the following packages:
```shell
sudo apt install python3-nautilus python3-gi procps libjs-jquery
```

Download Actions For Nautilus from the following github:
```shell
git clone https://github.com/bassmanitram/actions-for-nautilus.git
cd actions-for-nautilus
# to install for only your use
make install 
# to install for all users
sudo make install_global 
nautilus -q
```
Next, open the Actions For Nautilus configuration from the menu and set the options as shown in the below image.

<div class="row mt-1">
    <div class="col-sm mt-1">
        {% include figure.liquid loading="eager" path="assets/img/blogs/actions_nautilus.png" class="img-fluid rounded z-depth-1" zoomable=true%}
    </div>
</div>
