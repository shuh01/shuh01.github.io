---
layout: single
title: Django - Setup/Install
categories: Django
use_math: true
toc: true
---
# Set up virtual environment and install Django

## Check for the python3 Version

```powershell
$python3 -V
```

## Make or move to directory where you want to set up your virtual environment for Django

```powershell
$mkdir Dev
$cd Dev
$mkdir tryDjango
$cd tryDjango
$virtualenv -p python3 .  #This will create a virtual environment
$source bin/activate    #This command will get you into a virtual environment
$deactivate    #This command will get you out of a virtual environment
```

## 3 different ways to create virtual environments

```powershell
#After move in to a directory you want to create virtual environment

$virtualenv venv  #Create virtual environment named 'venv'

$virtualenv venv2 -p python3  #Create virtual environment with specific python version you want

$virtualenv ven3 -p  #Create virtual environment with python version installed in pc

```

## Install Django on the virtual environment

```powershell
$pip install django==2.0.7
```

## Create a black Django project

```powershell
$django-admin
$django-admin startproject trydjango .   #This command creates a new project named trydjango
$python manage.py runserver
>>> Starting development server at http://~~   #This is your URL address you can debug on
```
