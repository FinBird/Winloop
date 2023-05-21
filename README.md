![Alt text](https://raw.githubusercontent.com/Vizonex/Winloop/main/winloop.png)

# Winloop
An Alternative library for uvloop compatability with windows Because let's face it. Window's python asyncio can be garabage at times. 
I never really liked the fact that I couldn't make anything run faster escpecially when you have fiber internet connections in place. 
It always felt dissapointing when libuv is avalible on windows but doesn't have uvloop compatability. 
So I went ahead and downloaded the uvloop source code and I'm currently modifying alot of parts to make it windows compatable. 

This library is still being worked on but I wanted to make sure for right now that the name of this library has been claimed by me. 
I will not be posting any code yet until I have resolved licensing with the uvloop devs. 

The main differences will be some name changes incase there's any problems and the disabling of non-windows compatable apis...

## Current Progress

This has not been finished yet but as of right now I have done all the followings to try and get this to work 
- disabled epoll because Windows cannot truely fork proccesses. 
- disabled pthreads due to the same problem and infact the C function called pthread_atfork() is not avalible on Windows so __install_atfork() is not there anymore...

Luckily managed to get the library to compile by invoking the following libraries 
- uv_a.lib (dlls are slow, so I'm going static)
- Ws2_32.lib
- Advapi32.lib
- iphlpapi.lib
- WSock32.lib
- Userenv.lib
- User32.lib


## Updates

Made my own socketpair function in C inspired by libcurl's version for winloop to use so that the current ways that the library does polling wouldn't break.
I also replaced `uv_poll_init` with `uv_pool_init_socket` as a temporary monkey_patch/solution. 


- As of May 18th 2023 I have gotten winloop finally working but it will require some more tests , I plan to do those within a couple of days...

- As of May 20th 2023 We are having problems with fixing some of the implementations on `Winloop/handles/process.pyx` `UVProccess._init` has to be reworked because windows can only spawn a proccess not fork one... In fact all Unix parts may need to be redone. But luckily we have a headstart and shouldn't be to difficult to implement because we have the base-work layed out for us so that we don't have to reinvent the wheel.

- I will be trying to get premission from the magicstack Team if I can just go ahead and upload my modfied cython files soon to this repository. But for now I will be soon uploading my `socketpair.h` function based on where I have my `setup.py` file stored which I will also be uploading today. You have my permission to use the code for socketpair because it's the only file where I had to write in a bunch of newer things... It's likely my code will be `cygwin` compatable soon so that's a start in the right direction going forward...
- I just now pointed out that we no longer will have licensing problems and I'll just need to povide the original Apache License alongside my own and all the parts that I modify will be MIT Licensed... [https://github.com/MagicStack/uvloop/issues/536](url)
