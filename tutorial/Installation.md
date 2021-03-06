[:arrow_backward:](README.md) | [:arrow_forward:](Lesson01)
<!------------- https://github.com/exxeleron/enterprise-components/tree/master/tutorial/Installation.md --------------->

#                                           **`DemoSystem` Installation**

<!--------------------------------------------------------------------------------------------------------------------->
## `DemoSystem` prerequisites

<!--------------------------------------------------------------------------------------------------------------------->
### Operating system
`DemoSystem` can be run on Linux, Windows or MacOS operating systems.

<!--------------------------------------------------------------------------------------------------------------------->
### Compatibility libraries
In case of 64 bit Linux operating system, 32 bit compatibility libraries might need to be installed. For example:
- `lib32z1` on Ubuntu
- `zlib.i686` on Fedora
  
> :white_check_mark: Note:
  
> Our test runs on 'fresh' installation of openSuse 12.3 worked without need for any additional packages.

<!--------------------------------------------------------------------------------------------------------------------->
### `Kdb+` studio
Instructions in Lessons relay on checking data or status of `kdb+` processes, therefore, IDE for `kdb+` is required 
(e.g. http://code.kx.com/wiki/StudioForKdb+). Connection might need to be established from `localhost` 
(e.g. when both `kdb+` studio and `enterprise-components` are deployed locally) or over `TCP` 
(e.g. `enterprise-components` on workstation, `kdb+` studio on user’s desktop).

<!--------------------------------------------------------------------------------------------------------------------->
### `Kdb+`
`DemoSystem` package does not contain `q` binaries. There are following two options for the deployment:

1. Install `q` binaries in the `bin/q/` subdirectory [Recommended]

    It is recommended to install `q` binary into `bin/q/` subdirectory. In this case bin directory will contain entire software set required 
    to run the system.
    It will be independent from other installations on the same server.
    Each `ec`-based system running on the same machine will have own, independent version of `q`.

1. Use `q` that is already installed on the system

    Second option is to use `q` that is already installed on the system and available on the `PATH`.
    `QHOME` and `PATH` environmental variables have to be adjusted accordingly in `env.sh` (`env.bat` on Windows) file for each Lesson.


> :heavy_exclamation_mark: Note:

> `Kdb+` binaries can be downloaded from:
> - current version of FREE 32bit of `KDB+` binaries can be downloaded from http://kx.com/software-download.php
> - users with 64 bit `KDB+` license can download binaries from http://downloads.kx.com/
 

<!--------------------------------------------------------------------------------------------------------------------->
## `DemoSystem` installation

1. [Download](https://github.com/exxeleron/enterprise-components/releases) and unpack 
   package `ec_vX.X.X_DemoSystem_Linux32bit_Lessons_X-X.tgz`

    ```bash
    > tar zxvf ec_vX.X.X_DemoSystem_Linux32bit_Lessons_X-X.tgz
    ```

    On Windows download and unpack the package `ec_vX.X.X_DemoSystem_Win32_Lessons_X-X.zip`
    
    ```Batchfile
    > unzip ec_vX.X.X_DemoSystem_Windows32bit_Lessons_X-X.zip
    ```


1. [Optional step] Download [FREE 32bit `KDB+`](http://kx.com/software-download.php) or [LICENSED 64bit `KDB+`](http://downloads.kx.com) 
   matching the operating system and unpack the package.

    On Linux download and unpack the package `linux.zip` into `DemoSystem/bin/q/` subdirectory.

    ```bash
    > cd DemoSystem
    DemoSystem> unzip linux.zip -d bin
    ```

    On Windows download and unpack the package `windows.zip` into `DemoSystem/bin/q/` subdirectory.
    
    ```Batchfile
    > cd DemoSystem
    DemoSystem> unzip windows.zip -d bin
    ```

    > :heavy_exclamation_mark: Note:

    > Downloaded `q` package normally contains `q/` subdirectory. The result of unpacking should be as follows:


    ```bash
    DemoSystem> ls bin/q/
      l32/
      q.q
      q.k
      README.md
      README.txt
      [...]
    ```

    On Windows, use the `dir` command.

    > :heavy_exclamation_mark: Note:

    > Alternatively use own, pre-installed `q` executable that is available on the `PATH` with `QHOME` pointing to `q`     installation location.


1. Create link to configuration for LessonXX
  
    ```bash
    > cd DemoSystem
    DemoSystem> ln -s bin/ec/tutorial/LessonXX/etc
    ```

   The corresponding commands for Windows are
   
   ```Batchfile
   > cd DemoSystem
   DemoSystem> mklink /J etc bin\ec\tutorial\Lesson01\etc
   ```
  
1. Check if `etc` folder is linked to correct Lesson

    ```bash
    DemoSystem> ls -l etc
      etc -> bin/ec/tutorial/LessonXX/etc
    ```
    Use the `dir` command on Windows.
  
1. After installation `DemoSystem` directory should contain `bin` directory and `etc` symbolic link

    ```bash
    DemoSystem> ls -l
      bin
      etc -> bin/ec/tutorial/LessonXX/etc/
      Installation.md
      README.md
      Troubleshooting_linux.md
      Troubleshooting_windows.md
    ```

<!--------------------------------------------------------------------------------------------------------------------->
### Troubleshooting
Common installation problems with solutions for [linux](Troubleshooting_linux.md) and for [windows](Troubleshooting_windows.md).


<!--------------------------------------------------------------------------------------------------------------------->
## `DemoSystem` startup

1. Source environment

    ```bash
    DemoSystem> source etc/env.sh
    ```
    On Windows one needs to run
    
    ```Batchfile
    DemoSystem> etc\env.bat
    ```
    
1. Start all components in the system (restart command is used just in case the system was already running)

    ```bash
    DemoSystem> yak restart \*
      Stopping components...
        core.gen                      	Skipped
        core.tick                     	Skipped
        core.rdb                      	Skipped
      Starting components...
        core.gen                      	OK
        core.tick                     	OK
        core.rdb                      	OK
    ```
    
1. Check if all components are running

    ```bash
    DemoSystem> yak info \*
      uid                pid   port  status      started             stopped            
      ----------------------------------------------------------------------------------
      core.gen           11235 17009 RUNNING     2014.05.08 07:36:18                    
      core.rdb           11247 17011 RUNNING     2014.05.08 07:36:19                    
      core.tick          11241 17010 RUNNING     2014.05.08 07:36:18                    
    ```
    
1. Check if `data` and `log` directories were created

    ```bash
    DemoSystem> ls -l
      bin
      data
      etc -> bin/ec/tutorial/LessonXX/etc/
      log
      readme.txt
      troubleshooting.txt
    ```
    
    On Windows, use the `dir` command.

<!--------------------------------------------------------------------------------------------------------------------->

## Changing `DemoSystem` Lesson

Folder structure for `data` and `log` directories remains the same for all Lessons, therefore it is enough to change 
configuration pointer (symbolic link) to change the Lesson.

If you would like to start with fresh set of `data` and `log` directories - these can be removed while the system is 
stopped.

> :heavy_exclamation_mark: Note:

> Make sure that system is really stopped before deleting these directories, otherwise yak will lose its connection details resulting in 
error described in [Issue 4](../tutorial/Troubleshooting_linux.md#issue-4---startup-failed-address-already-in-use).

1. Stop the system

    ```bash
    DemoSystem> yak stop \*
      Stopping components...
        core.gen                        OK
        core.tick                       OK
        core.rdb                        OK
    ```
        
1. Remove link to configuration for LessonXX

    ```bash
    DemoSystem> ls -l etc
      etc -> bin/ec/tutorial/LessonXX/etc/
    DemoSystem> rm etc
    ```
    
    On Windows, do
    
    ```Batchfile
    DemoSystem> rmdir etc
    ```
    
1. Create link to configuration for LessonYY

    ```bash
    DemoSystem> ln -s bin/ec/tutorial/LessonYY/etc
    DemoSystem> ls -l etc
      etc -> bin/ec/tutorial/LessonYY/etc/
    ```
    
    On Windows:
    
    ```Batchfile
    DemoSystem> mklink /J etc bin\ec\tutorial\LessonYY\etc
    ```
    
1. Start the system again

    ```bash
    DemoSystem> yak start \*
      Starting components...
        core.gen                        OK
        core.tick                       OK
        core.rdb                        OK
    ```

<!--------------------------------------------------------------------------------------------------------------------->
[:arrow_backward:](README.md) | [:arrow_forward:](Lesson01)
