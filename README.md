# Working-with-WSL
Working with WSL

WSL = Windows feature that enables users to run native Linux applications, containers and command-line tools directly on Windows 11 and later.  
WSL2  
- second generation of WSL
- has a VM with a Linux WSL Kernel
- tightly integrated with the Microsoft Windows OS
- interoperable with other Windows desktop

Why WSL?  
- developers working across both Linux and Windows environments have a very disruptive workflow (stop all the work and then switch the system or reboot)
- need for seamless transition and better productivity
- applications hitherto only available on Linux to be available on Windows

ML Platforms and using them on Windows with GPU support  
- TensorFlow 2.10 was the last TensorFlow release that supported GPU on native-Windows. Starting with TensorFlow 2.11, you will need to install TensorFlow in WSL2, or install tensorflow-cpu
- TensorFlow with GPU access is supported for WSL2 on Windows 10 19044 or higher. This corresponds to Windows 10 version 21H2, the November 2021 update.


Steps  
Check for Windows update first  
Run C:\> wsl.exe to access Ubuntu Linux



Source: https://www.youtube.com/watch?v=CO43b6XWHNI
https://www.tensorflow.org/install/pip#windows-wsl2



Notes:

Don't have to do the Windows Insidewr Program. You had to do that before. Now stable.
D