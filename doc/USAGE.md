# Usage:
```
ruri [OPTIONS]...
ruri [ARGS]... [CONTAINER_DIRECTORY]... [COMMAND [ARGS]...]
```
## Options:
```
-v, --version ...............................: Show version info
-V, --version-code ..........................: Show version code
-h, --help ..................................: Show helps
-H, --show-examples .........................: Show commandline examples
-P, --ps [container_dir] ....................: Show process status of the container
```
These four options will show the info.
********************************************
```
-U, --umount [container_dir] ................: Umount a container
```
When running a container, ruri needs to mount some directories on it.         
And after running the container, you can use `-U` option to umount a container.          
This option needs to be run with root(sudo).      
WARNING: Always do `sudo ruri -U /path/to/container` before you removing the container.      
## Arguments:
Common ruri container should be run with sudo, but you can also use `-r` option to run rootless container if you want.      
*****************************************
```
-D, --dump-config ...........................: Dump the config
```
ruri supports using config file, you can use `-D` option to dump current config of container.      
For example:      
```
ruri -D -k cap_sys_admin -d cap_sys_chroot ./t
```
This will dump the container config with `-k cap_sys_admin -d cap_sys_chroot`, so next time  you can just use the config instead of `-k cap_sys_admin -d cap_sys_chroot` argument.      
********************************************
```
-o, --output [config file] ..................: Set output file of `-D` option
```
This option is used for `-D` option, to save the config to a file.      
For example:
```
ruri -D -o test.conf -k cap_sys_admin -d cap_sys_chroot ./t
```
This will save config to test.conf.      
*****************************************
```
-c, --config [config file] ..................: Use config file
```
You can use `ruri -c config_file` to run a container with config file.      
For example:      
```
ruri -c test.conf
```
This will run container using test.conf.
***********************************************
```      
-a, --arch [arch] ...........................: Simulate architecture via binfmt_misc/QEMU
-q, --qemu-path [path] ......................: Specify the path of QEMU
```
These two arguments should be set at the same time.      
ruri supports to use qemu-user-static with the binfmt_misc feature of kernel to run cross-arch containers.      
`-q` option can use qemu path in host, it will be copied to /qemu-ruri in container.      
For example:      
```
ruri -q /usr/bin/qemu-x86_64-static -a x86_64 ./test-x86_64
```
But remember that do not use this feature to simulate host architecture.      
*******************************************************************
```
-u, --unshare ...............................: Enable unshare feature
```
ruri supports unshare container, but NET and USER namespace is not supported.        
*****************************************
```
-n, --no-new-privs ..........................: Set NO_NEW_PRIVS flag
```
This argument will set NO_NEW_PRIVS, commands like `sudo` will be unavailable for common user.      
****************************************
```
-N, --no-rurienv ............................: Do not use .rurienv file
```
ruri will create /.rurienv in container to save container config by default, you can use this option to disable it.      
*********************************************
```
-s, --enable-seccomp ........................: Enable built-in Seccomp profile
```
ruri provides a built-in seccomp profile, but if you really need to use seccomp, you might need to edit src/seccomp.c with your own rules and recompile it.      
****************************************
```
-p, --privileged ............................: Run privileged container
```
This argument will give all capabilities to container, but you can also use `-d` option to filter out capabilities you don't want to keep.      
*******************************************
```
-r, --rootless ..............................: Run rootless container
```
This option should be run with common user, so you can run rootless container with user ns.      
This option require `uidmap` package and user namespace support.      
*********************************************
```
-k, --keep [cap] ............................: Keep the specified capability
-d, --drop [cap] ............................: Drop the specified capability
```
This two option can control the capability in container, cap can both be value or name.
For example, `-k cap_chown` have the same effect with `-k 0`.      
**************************************************
```
-e, --env [env] [value] .....................: Set environment variables to its value
```
A very useless function, I hope it works.      
*********************************************
```
-m, --mount [dir/dev/img/file] [target] .....: Mount dir/block-device/image/file to target
-M, --ro-mount [dir/dev/img/file] [target] ..: Mount dir/block-device/image/file as read-only
```
ruri provides a powerful mount function, here are some examples:
```
ruri -m /dev/sda1 / ./test
```
This command will mount /dev/sda1 to ./test and run container.      
```
ruri -m ./test.img / ./test
```
This command will mount ./test.img to ./test and run container.       
```
ruri -m /sdcard /sdcard ./test
```
This command will mount /sdcard to ./test/sdcard and run container.       
It can also bind-mount file/FIFO/socket.      
******************************************
```
-S, --host-runtime ..........................: Bind-mount /dev/, /sys/ and /proc/ from host
```
ruri will create /dev/, /sys/ and /proc/ after chroot(2) into container for better security. You can use `-S` option to force it to bind-mount system runtime dirs.    
*************************************
```
-R, --read-only .............................: Mount / as read-only
```
This will make the whole container rootfs read-only.
***********************************************
```
-l, --limit [cpuset=cpu/memory=mem] .........: Set cpuset/memory limit
```
ruri currently supports cpuset and memory cgroup.            
Each `-l` option can only set one of the cpuset/memory limits      
for example:       
```
ruri -l memory=1M -l cpuset=1 /test
```
**************************************************
```
-w, --no-warnings ...........................: Disable warnings
```
There might be some warnings when running ruri, if you don't like, use `-w` option to disable them.       
****************************************************
```
-f, --fork ..................................: fork() before exec the command
```
ruri will set its process name to `ruri`,     
but after exec(), this name will be changed.     
unshare and rootless container will always fork() before running commands in container,      
you can use this option to make common chroot container have the same behaiver,      
so you can find all running container by finding the process name `ruri` in `ps` command.      