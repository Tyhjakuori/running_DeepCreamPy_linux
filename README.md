## Table of contents
* [Introduction](#introduction)
* [Initial setup](#initial-setup)
	* [Arch](#arch)
	* [Gentoo](#gentoo)
* [hent-AI](#hent-ai)
	* [Arch](#arch-1)
	* [Gentoo](#gentoo-1)
* [DeepCreamPy](#deepcreampy)
	* [Arch](#arch-2)
	* [Gentoo](#gentoo-2)
* [Sources](#sources)
* [Summary](#summary)


## Introduction
I have noticed that DeepCreamPy for windows doesn't work (although i have only tried running it with wine), for this reason i'm writing this guide for those not that familiar with linux.
As from my experience DeepCreamPy works well with linux. You will need handfull of tools to make it run, but i will cover them all in this.
You don't have to follow this exactly how it's written and please do change the names if needed/wanted.
This will just serve as an example.
I'm using arch linux so please keep that in mind if using some other distribution.
I will add other distributions commands here as well if needed.


## Initial setup
You will need these tools before we can start doing anything.
You DON'T need to install all of them and before you install/run commands please check what they do before hand if you don't know.
Blindly installing/running commands will do you no good. Probably it will work, but there is no benefit if you don't understand what you just did.

### Arch
```
$ pacman -S git python python-virtualenv base-devel unp unzip ffmpeg
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
$ emerge --ask dev-vcs/git dev-python/pip dev-python/virtualenv dev-lang/tk app-arch/unp app-arch/unzip media-video/ffmpeg
``` 
(we will need dev-lang/tk in order for tkinter to work)

I will be using directory called "git" in my /home that will have all cloned files
and i will use directory called "Environments" to contain all python virtual environments
but you can use whatever you want just edit the commands correctly
```
$ mkdir ~/git
$ mkdir ~/Environments
```

## hent-AI
You can either use the win executable or compile it from source
The one i'm using is the source and that is the one i will showcase

```
$ cd git/
$ git clone https://github.com/natethegreate/hent-AI
```

### Arch
You will need python 3.5 for hent-AI to work properly, so
```
$ yay -S python35
```
OR
```
$ git clone https://aur.archlinux.org/python35.git
$ cd python35/
$ makepkg -si
```
If makepkg return with error: ==> ERROR: One or more PGP signatures could not be verified!
You can resolve it with this
```
$ gpg --recv-key 3A5CA953F73C700D
```
in my case the key was this, if it's different for you use that key

### Gentoo
Getting python 3.5 on gentoo requires a little more work
First download python 3.5 & 3.6 sources from [here](https://www.python.org/downloads/source/)
(We will need python 3.6 with deepcreampy so might as well donwload them both now)
Unpack them, run configure and install them with make
(You can also use ./configure --enable-optimizations if wanted. It will optimize your python builds, but the build time will be significantly longer.
If you are only using these python versions for hent-AI and DeepCreamPy benefit from using the optimizations isn't that great.)
(make altinstall is recommended way of installing these older versions as "make install can overwrite or masquerade the python3 binary." Find more about this from [here](https://docs.python.org/3/using/unix.html?highlight=altinstall#building-python))
```
$ cd ~/Downloads
$ unp -u Python-3.5.10.tar.xz
$ cd Python-3.5.10
$ ./configure
$ make altinstall
```
And do the same to the other version            


You will need the latest model for better accuracy
(More info on this can be found [here](https://github.com/natethegreate/hent-AI#the-model))
Download the lastest model and unpack it
```
$ unp -u weights268.zip
$ cd weights268/
$ cp weights.h5 ~/git/hent-AI/
```
Making virtual environment and installing requirements
(You can use either cpu or gpu for hent-ai, but in my case i'm using cpu so edit commands accordingly if different)
```
$ cd ~/Environments
$ python3.5 -m venv hent-ai
$ source hent-ai/bin/activate
(hent-ai) $ cd ~/git/hent-AI/
(hent-ai) $ pip install tensorflow==1.8.0
(hent-ai) $ pip install -r requirements-cpu.txt
(hent-ai) $ python setup.py install
```
If you get an error while installing opencv-python like this: 
Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-build-4sr09xm7/opencv-python/setup.py", line 9, in <module>
        import skbuild
    ImportError: No module named 'skbuild'
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-4sr09xm7/opencv-python/

You can solve it by running: 
```
(hent-ai) $ pip install --upgrade pip
```
While this isn't a perfect answer it works(for me at least)


Now you should have working hent-AI installed on your system, you can check by running:
```
(hent-ai) $ python main.py
```
If it runs and you get window saying "Welcome to hentAI" you are all set to continue with installing DeepCreamPy
First deactivate your hent-ai environment by running:
```
(hent-ai) $ deactivate
```
And we can continue


## DeepCreamPy
First let's clone DeepCreamPy
(mirror of it as the original has been deleted, you can find more info from [here](https://old.reddit.com/r/AnimeResearch/comments/mg2ro4/what_happened_to_deepcreampy/))
```
$ cd ~/git
$ git clone https://github.com/FoxwellsGarden/DeepCreamPy-git
```

Models
(You can find models link [here](https://github.com/FoxwellsGarden/DeepCreamPy-git))
Now unpack and move them into deepcreampy folder
```
$ cd ~/Downloads
$ unp -u 09-11-2019\ DCPv2\ model.zip
$ cd 09-11-2019\ DCPv2\ model
$ cd 09-11-2019\ DCPv2\ model
$ mv * ~/git/DeepCreamPy/models/
```

### Arch
Installing Python 3.6
```
$ yay -S python36
```
OR
```
$ cd ~/git/
$ git clone https://aur.archlinux.org/python36.git
$ cd python36
$ makepkg -si
```

### Gentoo
We already configured and installed python 3.6 in hent-AI section so continue below.


Making virtual environment and installing requirements
(You will need to install tensorflow==1.14 manually and edit requirements-cpu.txt to remove tensorboard==1.14.0, tensorflow-estimator==1.14.0 and tensorflow==1.15.2)
(I installed every package manually, but now installing from the file should work)
(edit: Tested on gentoo installation, installing from file works after manual removal of above mentioned lines)
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
$ source ~/Environments/hent-ai/bin/activate
(hent-ai) $ cd ~/git/hent-AI/
(hent-ai) $ python main.py
```
Run hent-AI and point it to the directory that has all your .png you want to decensor and DeepCreamPy-git directory.
(Keep in mind that pictures HAVE to be .png)
Personally i run this at two patches, first all bar censorship with Hent-Ai and then run DeepCreamPy
```
(hent-ai) $ deactivate
$ source ~/Environments/deepcream/bin/activate
(deepcream) $ cd ~/git/DeepCreamPy-git/
(deepcream) $ python main.py
```
Then do the same with mosaic.


## Sources
yay: https://github.com/Jguer/yay

hent-AI: https://github.com/natethegreate/hent-AI

DeepCreamPy: https://github.com/FoxwellsGarden/DeepCreamPy-git (mirror)

Python source codes: https://www.python.org/downloads/source/

Python altinstall information: https://docs.python.org/3/using/unix.html?highlight=altinstall#building-python

Info about DeepCreamPy: https://old.reddit.com/r/AnimeResearch/comments/mg2ro4/what_happened_to_deepcreampy/

## Summary
And there we go, if everything went correctly you should now have uncencored pictures.
I do hope this guide was useful to at least someone.
Please do inform me if something is missing or incorrect.
