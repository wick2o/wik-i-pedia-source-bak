---
title: "Using NSIS to create an auto installer"
date: 2014-03-14
tags:
  - general
  - python
---
NSIS (Nullsoft Scriptable Install System) is a professional open source system to create Windows installers.  You can download NSIS from the main website [HERE](http://nsis.sourceforge.net/Main_Page).

<!--more-->

**The Code Explained**:

You will want to create a new text file with the nsi extension. The follwing is a breakdown of the script.

Here we want to include code to check if we are on x64 or x32
``` bash 

!include "x64.nsh"
!include "LogicLib.nsh"

```
 Here we want to name our script and exe file
``` bash

Name PythonInstaller
OutFile PythonInstaller

```
Lets setup the stype and permission level needed
``` bash

XPStyle on
RequestExecutionLevel admin
ShowInstDetails show

```
Lets define our Section
``` bash

Section "Install Python 2.7.6"
    InitPluginsDir
    IfFileExists C:\python27\python.exe WeAreGood WeAreBad
    WeAreGood:
        DetailPrint "Already Installed: Skipping Python..."
        Goto Finished
    WeAreBad:
      ${If} ${RunningX64}
          NSISdl::download "http://legacy.python.org/ftp/python/2.7.6/python-2.7.6.amd64.msi" $PLUGINSDIR\python.msi
      ${Else}
          NSISdl::download "http://legacy.python.org/ftp/python/2.7.6/python-2.7.6.msi" $PLUGINSDIR\python.msi
      ${EndIf}
      DetailPrint "Installing Python"
      nsExec::ExecToLog 'msiexec /i "$PLUGINSDIR\python.msi" /passive /quiet /norestart TARGETDIR=C:\Python27'
      Pop $0
      ${If} $0 != 0
          About "Failed to install Python: $0"
      ${EndIf}
    Finished:
SectionEnd

```

**Compiling the nsi**:

All that is left is to right click on the file and choose "Compile NSIS Script"

**The Results**:

You now have a script that will check if Python is install, if not it will download and then do a silent install.

Your Welcome!

