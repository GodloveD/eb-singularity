# eb-singularity                                                                
Stuff related to integrating [EasyBuild](https://github.com/easybuilders/easybuild) &amp; [Singularity](https://github.com/singularityware/singularity)
                                                                                
So far, we have an experimental approach to creating Singularity containers using [EasyBuild easyconfig files](https://github.com/easybuilders/easybuild-easyconfigs).
                                                                                
## Approach                                                                     
The basic idea is to give Singularity an easyconfig file that it will use to build a container. The approach taken here is to `create` and `bootstrap` a Singularity image in a rec   tory that has one or more easyconfig files in it.  The [build.def](/build.def) file will look for any easyconfig file(s) that exist in the same directory and will copy them tothe     container during the `%setup` portion of the `bootstrap`.  
                                                                                
## Instructions for use                                                         
Assuming you have Singularity installed on your system, you could use these steps to install `[ack]`(https://beyondgrep.com/) into a container via the [ack easyconfig](https://github.com/easybuilders/easybuild-easyconfigs/blob/master/easybuild/easyconfigs/a/ack/ack-2.14.eb) 
                                                                                
First download the [easyconfig repo](https://github.com/easybuilders/easybuild-easyconfigs) and copy the `zlib` easyconfig to a new directory.
```                                                                             
$ cd ~                                                                          
                                                                                
$ git clone https://github.com/easybuilders/easybuild-easyconfigs.git           
                                                                                
$ mkdir easybuild-test                                                          
                                                                                
$ cp easybuild-easyconfigs/easybuild/easyconfigs/a/ack/ack-2.14.eb easybuild-test/
```                                                                             
Then download this repo and copy the [build.def](/build.def) file into the directory you just created.
```                                                                             
$ git clone https://github.com/easybuilders/eb-singularity.git

$ cp eb-singularity/build.def easybuild-test/                                                                                                               
```
Now just build a container in that directory using the [build.def](/build.def) file.
```
$ cd easybuild-test

$ singularity create -s 2000 ack-2.14.img

$ sudo singularity bootstrap ack-2.14.img build.def
```
And your all set!
```
$ singularity shell ack-2.14.img

> source /etc/bashrc # hope to fix this bug soon

> module load which ack

> ack --bar
```
