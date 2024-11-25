# Digging Up History
## TISC Level 3
### Description
Ah, who exactly is behind the attacks? If only our enemies left more images on their image transformation server. We are one step closer, but there is still so much to uncover...

A disc image file was recovered from them! We have heard that they have a history of hiding sensitive data through file hosting sites... Can you help us determine what they might be hiding this time?

https://assets-hgsv2z3wsyxzjayx.sgp1.digitaloceanspaces.com/disk.zip

#### Reading the disk.zip
1. Install Autopsy
2. Download https://github.com/markmckinnon/Autopsy-Plugins/tree/master/AD1_Extractor, this is needed as Autopsy needs this plugin in order to read the AD1 file that is found in the disk.zip
3. Add the module to Autopsy

#### Flag.lnk

Under `Document and Settings/csitfan1/Recent`, a file called `flag.lnk` can be found.

However, unable to view the data in the file as a it is .lnk file, thus will need a program to do that.


#### Reading the .lnk file:
Download LECmd from https://github.com/EricZimmerman/LECmd
````
PS D:\CTF\TISC 2024> .\LECmd.exe -f .\flag.lnk
LECmd version 1.5.0.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/LECmd

Command line: -f .\flag.lnk

Warning: Administrator privileges not found!


Processing D:\CTF\TISC 2024\flag.lnk

Source file: D:\CTF\TISC 2024\flag.lnk
  Source created:  2024-09-05 07:42:42
  Source modified: 2024-09-05 07:46:04
  Source accessed: 2024-09-15 09:01:10

--- Header ---
  Target created:  2024-09-05 07:42:04
  Target modified: 2024-09-05 07:43:28
  Target accessed: 2024-09-05 07:46:00

  File size (bytes): 39
  Flags: HasTargetIdList, HasLinkInfo, HasRelativePath, HasWorkingDir, IsUnicode
  File attributes: FileAttributeArchive
  Icon index: 0
  Show window: SwNormal (Activates and displays the window. The window is restored to its original size and position if the window is minimized or maximized.)

Relative Path: ..\Desktop\flag.sus
Working Directory: C:\Documents and Settings\csitfan1\Desktop

--- Link information ---
Flags: VolumeIdAndLocalBasePath

>> Volume information
  Drive type: Fixed storage media (Hard drive)
  Serial number: 903376EF
  Label: (No label)
  Local path: C:\Documents and Settings\csitfan1\Desktop\flag.sus

--- Target ID information (Format: Type ==> Value) ---

  Absolute path: My Computer\C:\Documents and Settings\csitfan1\Desktop\flag.sus

  -Root folder: GUID ==> My Computer

  -Drive letter ==> C:

  -Directory ==> Documents and Settings
    Short name: DOCUME~1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Documents and Settings
    Created:     2024-09-05 14:46:22
    Last access: 2024-09-05 07:35:34

  -Directory ==> csitfan1
    Short name: csitfan1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: csitfan1
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:35:22

  -Directory ==> Desktop
    Short name: Desktop
    Modified:    2024-09-05 07:42:12
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Desktop
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:42:12

  -File ==> flag.sus
    Short name: flag.sus
    Modified:    2024-09-05 07:43:30
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: flag.sus
    Created:     2024-09-05 07:42:06
    Last access: 2024-09-05 07:43:30

--- End Target ID information ---

--- Extra blocks information ---

>> Special folder data block
   Special Folder ID: 16

>> Tracker database block
   Machine ID:  csitfan
   MAC Address: 00:0c:29:b9:5a:c1
   MAC Vendor:  VMWARE
   Creation:    2024-09-05 07:42:41

   Volume Droid:       59619484-1590-495d-a487-6443e10f0e14
   Volume Droid Birth: 59619484-1590-495d-a487-6443e10f0e14
   File Droid:         6e84fc54-6b5a-11ef-a0d5-000c29b95ac1
   File Droid birth:   6e84fc54-6b5a-11ef-a0d5-000c29b95ac1


---------- Processed D:\CTF\TISC 2024\flag.lnk in 0.13285330 seconds ----------



PS D:\CTF\TISC 2024> .\LECmd.exe -f '.\flag.txt (2).lnk'
LECmd version 1.5.0.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/LECmd

Command line: -f .\flag.txt (2).lnk

Warning: Administrator privileges not found!


Processing D:\CTF\TISC 2024\flag.txt (2).lnk

Source file: D:\CTF\TISC 2024\flag.txt (2).lnk
  Source created:  2024-09-05 07:47:23
  Source modified: 2024-09-05 07:47:23
  Source accessed: 2024-09-15 09:03:30

--- Header ---
  Target created:  2024-09-05 07:42:04
  Target modified: 2024-09-05 07:46:40
  Target accessed: 2024-09-05 07:47:22

  File size (bytes): 39
  Flags: HasTargetIdList, HasLinkInfo, HasRelativePath, HasWorkingDir, IsUnicode
  File attributes: FileAttributeArchive
  Icon index: 0
  Show window: SwNormal (Activates and displays the window. The window is restored to its original size and position if the window is minimized or maximized.)

Relative Path: ..\Desktop\flag.txt
Working Directory: C:\Documents and Settings\csitfan1\Desktop

--- Link information ---
Flags: VolumeIdAndLocalBasePath

>> Volume information
  Drive type: Fixed storage media (Hard drive)
  Serial number: 903376EF
  Label: (No label)
  Local path: C:\Documents and Settings\csitfan1\Desktop\flag.txt

--- Target ID information (Format: Type ==> Value) ---

  Absolute path: My Computer\C:\Documents and Settings\csitfan1\Desktop\flag.txt

  -Root folder: GUID ==> My Computer

  -Drive letter ==> C:

  -Directory ==> Documents and Settings
    Short name: DOCUME~1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Documents and Settings
    Created:     2024-09-05 14:46:22
    Last access: 2024-09-05 07:35:34

  -Directory ==> csitfan1
    Short name: csitfan1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: csitfan1
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:35:22

  -Directory ==> Desktop
    Short name: Desktop
    Modified:    2024-09-05 07:42:12
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Desktop
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:42:12

  -File ==> flag.txt
    Short name: flag.txt
    Modified:    2024-09-05 07:46:42
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: flag.txt
    Created:     2024-09-05 07:42:06
    Last access: 2024-09-05 07:47:16

--- End Target ID information ---

--- Extra blocks information ---

>> Special folder data block
   Special Folder ID: 16

>> Tracker database block
   Machine ID:  csitfan
   MAC Address: 00:0c:29:b9:5a:c1
   MAC Vendor:  VMWARE
   Creation:    2024-09-05 07:42:41

   Volume Droid:       59619484-1590-495d-a487-6443e10f0e14
   Volume Droid Birth: 59619484-1590-495d-a487-6443e10f0e14
   File Droid:         6e84fc54-6b5a-11ef-a0d5-000c29b95ac1
   File Droid birth:   6e84fc54-6b5a-11ef-a0d5-000c29b95ac1


---------- Processed D:\CTF\TISC 2024\flag.txt (2).lnk in 0.05253550 seconds ----------



PS D:\CTF\TISC 2024> .\LECmd.exe -f '.\flag.txt.lnk'
LECmd version 1.5.0.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/LECmd

Command line: -f .\flag.txt.lnk

Warning: Administrator privileges not found!


Processing D:\CTF\TISC 2024\flag.txt.lnk

Source file: D:\CTF\TISC 2024\flag.txt.lnk
  Source created:  2024-09-05 07:46:20
  Source modified: 2024-09-05 07:46:20
  Source accessed: 2024-09-15 09:05:20

--- Header ---
  Target created:  2024-09-05 07:42:04
  Target modified: 2024-09-05 07:43:28
  Target accessed: 2024-09-05 07:46:14

  File size (bytes): 39
  Flags: HasTargetIdList, HasLinkInfo, HasRelativePath, HasWorkingDir, IsUnicode
  File attributes: FileAttributeArchive
  Icon index: 0
  Show window: SwNormal (Activates and displays the window. The window is restored to its original size and position if the window is minimized or maximized.)

Relative Path: ..\Desktop\flag.txt.sus
Working Directory: C:\Documents and Settings\csitfan1\Desktop

--- Link information ---
Flags: VolumeIdAndLocalBasePath

>> Volume information
  Drive type: Fixed storage media (Hard drive)
  Serial number: 903376EF
  Label: (No label)
  Local path: C:\Documents and Settings\csitfan1\Desktop\flag.txt.sus

--- Target ID information (Format: Type ==> Value) ---

  Absolute path: My Computer\C:\Documents and Settings\csitfan1\Desktop\flag.txt.sus

  -Root folder: GUID ==> My Computer

  -Drive letter ==> C:

  -Directory ==> Documents and Settings
    Short name: DOCUME~1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Documents and Settings
    Created:     2024-09-05 14:46:22
    Last access: 2024-09-05 07:35:34

  -Directory ==> csitfan1
    Short name: csitfan1
    Modified:    2014-12-02 08:56:18
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: csitfan1
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:35:22

  -Directory ==> Desktop
    Short name: Desktop
    Modified:    2024-09-05 07:42:12
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: Desktop
    Created:     2014-12-02 08:56:18
    Last access: 2024-09-05 07:42:12

  -File ==> flag.txt.sus
    Short name: FLAGTX~1.SUS
    Modified:    2024-09-05 07:43:30
    Extension block count: 1

    --------- Block 0 (Beef0004) ---------
    Long name: flag.txt.sus
    Created:     2024-09-05 07:42:06
    Last access: 2024-09-05 07:46:02

--- End Target ID information ---

--- Extra blocks information ---

>> Special folder data block
   Special Folder ID: 16

>> Tracker database block
   Machine ID:  csitfan
   MAC Address: 00:0c:29:b9:5a:c1
   MAC Vendor:  VMWARE
   Creation:    2024-09-05 07:42:41

   Volume Droid:       59619484-1590-495d-a487-6443e10f0e14
   Volume Droid Birth: 59619484-1590-495d-a487-6443e10f0e14
   File Droid:         6e84fc54-6b5a-11ef-a0d5-000c29b95ac1
   File Droid birth:   6e84fc54-6b5a-11ef-a0d5-000c29b95ac1


---------- Processed D:\CTF\TISC 2024\flag.txt.lnk in 0.05402600 seconds ----------
```` 
Given the output from LECmd, flag.lnk is actually a shortcut to `C:\Documents and Settings\csitfan1\Desktop\flag.sus`. 

However, unable to find the flag.sus file after navigating to the directory. 

Thus, need to dig around more to find the actual location of the flag.sus file
# Solution
The actual location of the `flag.sus` file can be found in `Local Settings/Application Data/Mypal68/Profiles a80xxxxxxxxxx/entries`.

First sort by `Date Modified`, then look at the last few files

There is a file with the link https://csitfan-chall.s3.amazonaws.com/flag.sus

Base64 decode the text in the file

Flag: TISC{tru3_1nt3rn3t_h1st0r13_8445632pq78dfn3s}
