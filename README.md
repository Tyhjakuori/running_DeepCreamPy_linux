## Table of contents
* [Introduction](#introduction)
* [Initial setup](#initial-setup)
	* [Arch](#arch)
	* [Gentoo](#gentoo)
	* [Ubuntu](#ubuntu)
* [hent-AI](#hent-ai)
	* [Arch](#arch-1)
	* [Gentoo](#gentoo-1)
	* [Ubuntu](#ubuntu-1)
* [DeepCreamPy](#deepcreampy)
	* [Arch](#arch-2)
	* [Gentoo](#gentoo-2)
	* [Ubuntu](#ubuntu-2)
* [Summary](#summary)
* [Troubleshooting](#troubleshooting)
	* [makepkg return with error](#makepkg-return-with-error)
	* [Error while installing opencv-python](#error-while-installing-opencv-python)
	* [Import error no module named tkinter](#import-error-no-module-named-tkinter)
* [Sources](#sources)


## Introduction
   
I have noticed that DeepCreamPy for windows doesn't work (although i have only tried running it with wine), for this reason i'm writing this guide for those not that familiar with linux.
As from my experience DeepCreamPy works well with linux. You will need handfull of tools to make it run, but i will cover them all in this.
You don't have to follow this exactly how it's written and please do change the names if needed/wanted.
This will just serve as an example.
I'm using arch linux so please keep that in mind if using some other distribution.
I will add other distributions commands here as well if needed.


## Initial setup
   
I have included my deepcream virtual environments pip freeze output as "my_deepcream_requirements_cpu.txt", use it if you want.   
You will need these tools before we can start doing anything.
You DON'T need to install all of them and before you install/run commands please check what they do before hand if you don't know.
Blindly installing/running commands will do you no good. Probably it will work, but there is no benefit if you don't understand what you just did.

I will be using directory called "git" in my /home that will have all cloned files
and i will use directory called "Environments" to contain all python virtual environments
but you can use whatever you want just edit the commands correctly
```
$ mkdir ~/git
$ mkdir ~/Environments
```

### Arch
```
$ pacman -S git python python-virtualenv base-devel unp unzip ffmpeg wget
```
(You will probably have git and python already but for the sake of throughoutness i will include everything needed)
(Some of these aren't nescessary needed, but i will showcase them here as they will make your life easier)

Installing yay (not needed, i will also include way to install without yay)
```
$ cd ~
$ git clone https://aur.archlinux.org/yay-bin.git 
$ cd yay
$ makepkg -si
```

### Gentoo

On gentoo python will be installed with the base so you won't need to download that
```
$ emerge --ask dev-vcs/git dev-python/pip dev-python/virtualenv dev-lang/tk app-arch/unp app-arch/unzip media-video/ffmpeg net-misc/wget
``` 
   
### Ubuntu

I used minimal virtualbox installation for this and not actual hardware unlike in Arch & Gentoo sections so if there is any problems that occur, 
because of this let me know. First we will need following packages   
(Note: if you are using virtualbox remember to give enough base memory to it as i spend 2 days troubleshooting DeepCreamPy crashing and it just didn't have enough memory.)
```
$ apt-get install build-essential git python3-pip python3-virtualenv python3-openssl libssl-dev tk-dev ffmpeg unp unzip wget
```
You can either acquire necessary python versions from source (see [Gentoo](#gentoo-1)) or from deadsnakes repository.
We will need to add deadsnakes repository to our system's software recources.   
Keep in mind this repository isn't availible to every Ubuntu version, check [here](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) to see if your version is supported. 
```
$ add-apt-repository ppa:deadsnakes
$ apt-get update
```


## hent-AI
   
### Arch
You will need python 3.5 for hent-AI to work properly, so install with yay
```
$ yay -S python35
```
Or clone from AUR
```
$ git clone https://aur.archlinux.org/python35.git
$ cd python35/
$ makepkg -si
```

### Gentoo
Getting python 3.5 on gentoo requires a little more work
First download python 3.5 & 3.6 sources from [here](https://www.python.org/downloads/source/)
(We will need python 3.6 with deepcreampy so might as well download them both now)
Unpack them, run configure and install them with make
(You can also use ./configure --enable-optimizations if wanted. It will optimize your python builds, but the build time will be significantly longer.
If you are only using these python versions for hent-AI and DeepCreamPy benefit from using the optimizations isn't that great.)
(make altinstall is recommended way of installing these older versions as "make install can overwrite or masquerade the python3 binary." Find more about this from [here](https://docs.python.org/3/using/unix.html?highlight=altinstall#building-python))
```
$ cd ~/Downloads
$ wget https://www.python.org/ftp/python/3.5.10/Python-3.5.10.tar.xz
$ unp -u Python-3.5.10.tar.xz
$ cd Python-3.5.10
$ ./configure
$ make altinstall
```
And do the same to the other version            

### Ubuntu
   
(If you installed from source skip this part and continue below)   
We will install required python version from deadsnakes repository   
```
$ apt-get install python3.5
```
You will need to install pip manually 
```
$ cd ~/Environments
$ python3.5 -m venv --without-pip hent-AI
```
Now download python 3.5 get-pip.py file which you can get [here](https://bootstrap.pypa.io/pip/get-pip.py)   
After that file downloads:
```
$ cd ~/Downloads
$ source ~/Environments/hent-AI/bin/activate
(hent-AI) $ python3.5 get-pip.py
(hent-AI) $ pip install --upgrade pip
```
   
   
   
Now we can continue with installing hen-AI as all system specific installations are done.      
You can either use the win executable or compile it from source   
The one i'm using is the source and that is the one i will showcase   
```
$ cd git/
$ git clone https://github.com/natethegreate/hent-AI
```

You will need the latest model for better accuracy   
(More info on this can be found [here](https://github.com/natethegreate/hent-AI#the-model))   
Download the lastest model and unpack it   
```
$ cd ~/Downloads/
$ wget https://www.dropbox.com/s/zvf6vbx3hnm9r31/weights268.zip
$ unp -u weights268.zip
$ cd weights268/
$ cp weights.h5 ~/git/hent-AI/
```
   
Making virtual environment and installing requirements
(You can use either cpu or gpu for hent-ai, but in my case i'm using cpu so edit commands accordingly if different)
```
$ cd ~/Environments
$ python3.5 -m venv hent-AI
$ source hent-AI/bin/activate
(hent-AI) $ cd ~/git/hent-AI/
(hent-AI) $ pip install tensorflow==1.8.0
(hent-AI) $ pip install -r requirements-cpu.txt
(hent-AI) $ python setup.py install
```

Now you should have working hent-AI installed on your system, you can check by running:
```
(hent-AI) $ python main.py
```
If it runs and you get window saying "Welcome to hentAI" you are all set to continue with installing DeepCreamPy
First deactivate your hent-ai environment by running:
```
(hent-AI) $ deactivate
```
And we can continue
   
   
   
## DeepCreamPy
   
### Arch
   
Installing Python 3.6 with yay
```
$ yay -S python36
```
OR clone from AUR
```
$ cd ~/git/
$ git clone https://aur.archlinux.org/python36.git
$ cd python36
$ makepkg -si
```

### Gentoo

We already configured and installed python 3.6 in hent-AI section so continue below.

### Ubuntu
   
(If you installed from source skip this part and continue below)   
Install from deadsnakes  
```
$ apt-get install python3.6
```
You will need to install pip manually 
```
$ cd ~/Environments
$ python3.6 -m venv --without-pip deepcream
```
Download python get-pip.py file which you can get [here](https://bootstrap.pypa.io/get-pip.py)
```
$ cd ~/Downloads
$ source ~/Environments/deepcream/bin/activate
(deepcream) $ python3.6 get-pip.py
(deepcream) $ pip install --upgrade pip
```


      
First let's clone DeepCreamPy
(mirror of it as the original has been deleted, you can find more info from [here](https://old.reddit.com/r/AnimeResearch/comments/mg2ro4/what_happened_to_deepcreampy/))
```
$ cd ~/git
$ git clone https://github.com/FoxwellsGarden/DeepCreamPy-git
```

Models
(You can find models link [here](https://github.com/FoxwellsGarden/DeepCreamPy-git/blob/master/docs/INSTALLATION.md))
Now unpack and move them into deepcreampy folder
```
$ cd ~/Downloads
$ unp -u 09-11-2019\ DCPv2\ model.zip
$ cd 09-11-2019\ DCPv2\ model
$ cd 09-11-2019\ DCPv2\ model
$ mv * ~/git/DeepCreamPy/models/
```
OR you can use wget
```
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=1IMwzqZUuRnTv5jcuKdvZx-RZweknww5x' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=1IMwzqZUuRnTv5jcuKdvZx-RZweknww5x" -O models.zip && rm -rf /tmp/cookies.txt
```
   
Making virtual environment and installing requirements   
You will need to install tensorflow==1.14.0 manually and edit requirements-cpu.txt to remove tensorboard==1.14.0, tensorflow-estimator==1.14.0 and tensorflow==1.15.2   
I installed every package manually, but now installing from the file should work   
edit: Tested on gentoo installation, installing from file works after manual removal of above mentioned lines   
```
$ cd ~/Environments/
$ python3.6 -m venv deepcream
$ source deepcream/bin/activate
(deepcream) $ cd ~/git/DeepCreamPy-git/
(deepcream) $ pip install tensorflow==1.14
(deepcream) $ pip install -r requirements-cpu.txt
```
   
   
Now you have everything ready to run hent-AI.
```
(deepcream) $ deactivate
$ source ~/Environments/hent-AI/bin/activate
(hent-AI) $ cd ~/git/hent-AI/
(hent-AI) $ python main.py
```
Run hent-AI and point it to the directory that has all your .png you want to decensor and DeepCreamPy-git directory.
(Keep in mind that pictures HAVE to be .png)
Personally i run this at two patches, first all bar censorship with Hent-Ai and then run DeepCreamPy
```
(hent-AI) $ deactivate
$ source ~/Environments/deepcream/bin/activate
(deepcream) $ cd ~/git/DeepCreamPy-git/
(deepcream) $ python main.py
```
Then do the same with mosaic.



## Summary
   
And there we go, if everything went correctly you should now have uncencored pictures.
I do hope this guide was useful to at least someone.
Please do inform me if something is missing or incorrect.
   
   
   
## Troubleshooting

### makepkg return with error
If makepkg return with error during python3.5 installation
Error was: "==> ERROR: One or more PGP signatures could not be verified!""
You can resolve it with this
```
$ gpg --recv-key 3A5CA953F73C700D
```
in my case the key was this, if it's different for you use that key

### Error while installing opencv-python
If you get an error while installing opencv-python like this:   
Complete output from command python setup.py egg_info:   
&#10137;Traceback (most recent call last):   
&#10137;&#10137;File "<string>", line 1, in <module>   
&#10137;&#10137;&#10137;File "/tmp/pip-build-4sr09xm7/opencv-python/setup.py", line 9, in <module>   
&#10137;&#10137;import skbuild   
&#10137;ImportError: No module named 'skbuild'   
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-4sr09xm7/opencv-python/   

You can solve it by running:
```
(hent-AI) $ pip install --upgrade pip
```
While this isn't a perfect answer it works(for me at least)

### Import error no module named tkinter
If you have following error:   
Traceback (mostrecent call last):   
&#10137;File"/usr/lib/python3.5/tkinter/__init.py__", line 36, in <module>   
&#10137;&#10137;import _tkinter   
ImportError: No module named '_tkinter'   

You can solve it by installing python3.5-tk



## Sources
    
hent-AI: https://github.com/natethegreate/hent-AI

DeepCreamPy: https://github.com/FoxwellsGarden/DeepCreamPy-git (mirror)
   
yay: https://github.com/Jguer/yay
   
Python source codes: https://www.python.org/downloads/source/

Python altinstall information: https://docs.python.org/3/using/unix.html?highlight=altinstall#building-python

deadsnakes: https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa
   
Pip documentation: https://pip.pypa.io/en/latest/

get-pip.py file (for python 3.5): https://bootstrap.pypa.io/pip/get-pip.py
   
Info about DeepCreamPy: https://old.reddit.com/r/AnimeResearch/comments/mg2ro4/what_happened_to_deepcreampy/
