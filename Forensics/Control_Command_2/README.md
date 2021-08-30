# Control and Command - level 2

## Rootme Challenge

At the beginning of this challenge, we have to download what seems to be a memory dump, named "ch2.dmp".

To see if there is something interesting, we can execute the `strings`command line but it gives nothing.

On Rootme, in the hints part, we can observe that `volatility`tool is needed to succeed in this challenge.

So, after some documentation of this tool, we can first execute:
```shell
vol.py -f ch2.dmp imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x86_23418, Win7SP0x86, Win7SP1x86_24000, Win7SP1x86
                     AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                     AS Layer2 : FileAddressSpace (<YOUR_PATH>/ch2.dmp)
                      PAE type : PAE
                           DTB : 0x185000L
                          KDBG : 0x82929be8L
          Number of Processors : 1
     Image Type (Service Pack) : 0
                KPCR for CPU 0 : 0x8292ac00L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2013-01-12 16:59:18 UTC+0000
     Image local date and time : 2013-01-12 17:59:18 +0100
```

Thanks to that, we have the different profiles of the machine. We are going to use the first suggested to continue the challenge.

The second step is to show the different processes contained in this dump. For that, we execute:

```shell
vol.py -f ch2.dmp --profile=Win7SP1x86_23418 pslist
Volatility Foundation Volatility Framework 2.6.1
Offset(V)  Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                          
---------- -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0x87978b78 System                    4      0    103     3257 ------      0 2013-01-12 16:38:09 UTC+0000                                 
0x88c3ed40 smss.exe                308      4      2       29 ------      0 2013-01-12 16:38:09 UTC+0000                                 
0x8929fd40 csrss.exe               404    396      9      469      0      0 2013-01-12 16:38:14 UTC+0000                                 
0x892ac2b8 wininit.exe             456    396      3       77      0      0 2013-01-12 16:38:14 UTC+0000                                 
0x88d03a00 csrss.exe               468    448     10      471      1      0 2013-01-12 16:38:14 UTC+0000                                 
0x892ced40 winlogon.exe            500    448      3      111      1      0 2013-01-12 16:38:14 UTC+0000                                 
0x896294c0 services.exe            560    456      6      205      0      0 2013-01-12 16:38:16 UTC+0000                                 
0x896427b8 lsass.exe               576    456      6      566      0      0 2013-01-12 16:38:16 UTC+0000                                 
0x8962f7e8 lsm.exe                 584    456     10      142      0      0 2013-01-12 16:38:16 UTC+0000                                 
0x8962f030 svchost.exe             692    560     10      353      0      0 2013-01-12 16:38:21 UTC+0000                                 
0x897b5c20 svchost.exe             764    560      7      263      0      0 2013-01-12 16:38:23 UTC+0000                                 
0x89805420 svchost.exe             832    560     19      435      0      0 2013-01-12 16:38:23 UTC+0000                                 
0x89852918 svchost.exe             904    560     17      409      0      0 2013-01-12 16:38:24 UTC+0000                                 
0x8986b030 svchost.exe             928    560     26      869      0      0 2013-01-12 16:38:24 UTC+0000                                 
0x898911a8 svchost.exe            1084    560     10      257      0      0 2013-01-12 16:38:26 UTC+0000                                 
0x898b2790 svchost.exe            1172    560     15      475      0      0 2013-01-12 16:38:27 UTC+0000                                 
0x898a7868 AvastSvc.exe           1220    560     66     1180      0      0 2013-01-12 16:38:28 UTC+0000                                 
0x8a0f9c40 spoolsv.exe            1712    560     14      338      0      0 2013-01-12 16:38:58 UTC+0000                                 
0x8a102748 svchost.exe            1748    560     18      310      0      0 2013-01-12 16:38:58 UTC+0000                                 
0x88cded40 sppsvc.exe             1872    560      4      143      0      0 2013-01-12 16:39:02 UTC+0000                                 
0x8a1d84e0 vmtoolsd.exe           1968    560      6      220      0      0 2013-01-12 16:39:14 UTC+0000                                 
0x9541c7e0 wlms.exe                336    560      4       45      0      0 2013-01-12 16:39:21 UTC+0000                                 
0x8a1f5030 VMUpgradeHelpe          448    560      4       89      0      0 2013-01-12 16:39:21 UTC+0000                                 
0x9542a030 TPAutoConnSvc.         1612    560      9      135      0      0 2013-01-12 16:39:23 UTC+0000                                 
0x87ac0620 taskhost.exe           2352    560      8      149      1      0 2013-01-12 16:40:24 UTC+0000                                 
0x87ad44d0 dwm.exe                2496    904      5       77      1      0 2013-01-12 16:40:25 UTC+0000                                 
0x87ac6030 explorer.exe           2548   2484     24      766      1      0 2013-01-12 16:40:27 UTC+0000                                 
0x87ae2880 TPAutoConnect.         2568   1612      5      146      1      0 2013-01-12 16:40:28 UTC+0000                                 
0x87a9c288 conhost.exe            2600    468      1       35      1      0 2013-01-12 16:40:28 UTC+0000                                 
0x87b82438 VMwareTray.exe         2660   2548      5       80      1      0 2013-01-12 16:40:29 UTC+0000                                 
0x87aa9220 VMwareUser.exe         2676   2548      8      190      1      0 2013-01-12 16:40:30 UTC+0000                                 
0x87b784b0 AvastUI.exe            2720   2548     14      220      1      0 2013-01-12 16:40:31 UTC+0000                                 
0x898fe8c0 StikyNot.exe           2744   2548      8      135      1      0 2013-01-12 16:40:32 UTC+0000                                 
0x87b6b030 iexplore.exe           2772   2548      2       74      1      0 2013-01-12 16:40:34 UTC+0000                                 
0x898fbb18 SearchIndexer.         2900    560     13      636      0      0 2013-01-12 16:40:38 UTC+0000                                 
0x87bd35b8 wmpnetwk.exe           3176    560      9      240      0      0 2013-01-12 16:40:48 UTC+0000                                 
0x89f3d2c0 svchost.exe            3352    560      9      141      0      0 2013-01-12 16:40:58 UTC+0000                                 
0x87c6a2a0 swriter.exe            3452   2548      1       19      1      0 2013-01-12 16:41:01 UTC+0000                                 
0x87ba4030 soffice.exe            3512   3452      1       28      1      0 2013-01-12 16:41:03 UTC+0000                                 
0x95483d18 soffice.bin            3556   3544      0 --------      1      0 2013-01-12 16:41:05 UTC+0000   2013-01-12 16:41:39 UTC+0000  
0x87b8ca58 soffice.bin            3564   3512     12      400      1      0 2013-01-12 16:41:05 UTC+0000                                 
0x89f1d3e8 svchost.exe            3624    560     14      348      0      0 2013-01-12 16:41:22 UTC+0000                                 
0x95495c18 taskmgr.exe            1232   2548      6      116      1      0 2013-01-12 16:42:29 UTC+0000                                 
0x87bf7030 cmd.exe                3152   2548      1       23      1      0 2013-01-12 16:44:50 UTC+0000                                 
0x87c595b0 conhost.exe            3228    468      2       54      1      0 2013-01-12 16:44:50 UTC+0000                                 
0x89898030 cmd.exe                1616   2772      2      101      1      0 2013-01-12 16:55:49 UTC+0000                                 
0x954826b0 conhost.exe            2168    468      2       49      1      0 2013-01-12 16:55:50 UTC+0000                                 
0x9549f678 iexplore.exe           1136   2548     18      454      1      0 2013-01-12 16:57:44 UTC+0000                                 
0x87d4d338 iexplore.exe           3044   1136     37      937      1      0 2013-01-12 16:57:46 UTC+0000                                 
0x87c90d40 audiodg.exe            1720    832      5      117      0      0 2013-01-12 16:58:11 UTC+0000                                 
0x87cbfd40 winpmem-1.3.1.         3144   3152      1       23      1      0 2013-01-12 16:59:17 UTC+0000
```

All of these seem to be normal instead of one: `winpmem-1.3.1`.

After that, we are going to remember the process's PID and use the `dlllist`option to list different information of this process. For that:

```shell
vol.py -f ch2.dmp --profile=Win7SP1x86_23418 dlllist -p 3144
Volatility Foundation Volatility Framework 2.6.1
************************************************************************
winpmem-1.3.1. pid:   3144
Command line : winpmem-1.3.1.exe  ram.dmp


Base             Size  LoadCount LoadTime                       Path
---------- ---------- ---------- ------------------------------ ----
0x00210000    0x22000     0xffff 1970-01-01 00:00:00 UTC+0000   C:\Users\JOHNDO~1\AppData\Local\Temp\imagedump\winpmem-1.3.1.exe
0x77660000   0x13c000     0xffff 1970-01-01 00:00:00 UTC+0000   C:\Windows\SYSTEM32\ntdll.dll
0x70e70000    0x3c000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Program Files\AVAST Software\Avast\snxhk.dll
0x77480000    0xd4000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\system32\KERNEL32.dll
0x75920000    0x4a000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\system32\KERNELBASE.dll
0x76ee0000    0xa0000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\system32\ADVAPI32.dll
0x76a60000    0xac000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\system32\msvcrt.dll
0x76ec0000    0x19000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\SYSTEM32\sechost.dll
0x76c10000    0xa1000     0xffff 2013-01-12 16:59:17 UTC+0000   C:\Windows\system32\RPCRT4.dll
```

Here, we can see the user's name but we still haven't the machine's name. So, after reading the documetation and some information on Windows registry, there is an option in `volatility` which list addresses of registries in memory and other information.

For that, we execute:

```shell
vol.py -f ch2.dmp --profile=Win7SP1x86_23418 hivelist
Volatility Foundation Volatility Framework 2.6.1
Virtual    Physical   Name
---------- ---------- ----
0x8ee66740 0x141c0740 \SystemRoot\System32\Config\SOFTWARE
0x90cab9d0 0x172ab9d0 \SystemRoot\System32\Config\DEFAULT
0x9670e9d0 0x1ae709d0 \??\C:\Users\John Doe\ntuser.dat
0x9670f9d0 0x04a719d0 \??\C:\Users\John Doe\AppData\Local\Microsoft\Windows\UsrClass.dat
0x9aad6148 0x131af148 \SystemRoot\System32\Config\SAM
0x9ab25008 0x14a61008 \SystemRoot\System32\Config\SECURITY
0x9aba79d0 0x11a259d0 \??\C:\Windows\ServiceProfiles\LocalService\NTUSER.DAT
0x9abb1720 0x0a7d4720 \??\C:\Windows\ServiceProfiles\NetworkService\NTUSER.DAT
0x8b20c008 0x039e1008 [no name]
0x8b21c008 0x039ef008 \REGISTRY\MACHINE\SYSTEM
0x8b23c008 0x02ccf008 \REGISTRY\MACHINE\HARDWARE
0x8ee66008 0x141c0008 \Device\HarddiskVolume1\Boot\BCD
```

Here we are ! We obtain the registry's address of the machine's system ! So we are going to copy the virtual address and keep it. We are going to search on the Internet where the machine's name is stored on a Windows's machine. We found that it is stored at this path: `ControlSet001\Control\ComputerName\ComputerName`. So, now, we are going to use the `printkey`option to display data on the registry.

Execute this command:
```shell
vol.py -f ch2.dmp --profile=Win7SP1x86_23418 printkey -o 0x8b21c008 -K 'ControlSet001\Control\ComputerName\ComputerName'
Volatility Foundation Volatility Framework 2.6.1
Legend: (S) = Stable   (V) = Volatile

----------------------------
Registry: \REGISTRY\MACHINE\SYSTEM
Key name: ComputerName (S)
Last updated: 2013-01-12 00:58:30 UTC+0000

Subkeys:

Values:
REG_SZ                        : (S) mnmsrvc
REG_SZ        ComputerName    : (S) WIN-ETSA91RKCFP
```

That's it ! We found the machine's name.