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
- I first began with doing a factory reset of my Asus windows laptop  
- I then check my Windows laptop for the latest windows update
- I then downloaded the right Nvidia graphics driver for my Nvidia RTX 2060 graphics card. The download link is https://www.nvidia.com/download/index.aspx. Then complete the installation, restarting the system when prompted.
- Run Powershell a administrator
- In powershell, run the command wsl.exe --install
- Restart system again
- Run Powershell a administrator
- In powershell, run the command wsl.exe --update
- Restart system again
- Run Powershell a administrator
- In powershell, run the command wsl -l -o  This will print on screen several distributions of Linux. Pick Ubuntu-20.04 from the list and run the command wsl --install -d Ubuntu-20.04  This will install the chosen version of Ubuntu.
- From the start button on the taskbar, run the Ubuntu-20.04 application. If you get an error hat the VM could not be started because the required feature could not be installed. This means that the hypervisor is not installed. Please enable Hyper-V from your BIOS
- Launch Ubuntu-20.04 again from start menu. Configure a UNIX user name and password when asked.
- Test if you can navigate to the C drive on Windows using cd /mnt/c
- Now, to install docker. Get back to your home directory in Linux
- Download docker by running the command curl https://get.docker.com | sh   You may get a message that recommends using Docker Desktop for Windows instead. Ignore this. You want to be able to use the NVIDIA GPU. If you want to use NVIDIA GPU, you got to use WSL.
- After the installation is done, test by running "docker"
- Now, to install the NVIDIA Container toolkit.
    - distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    - curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
    - curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    - Then run sudo apt-get update
    - Now install NVIDIA docker by running sudo apt-get install -y nvidia-docker2
- Open a separate WSL2 window,
    - start the docker service by running sudo service docker start
- On the original WSL2 window
    - run sudo docker ps You will notice that nothing is running as expected
- Now run a benchmark test by running
    - sudo docker run --gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
    - You should get the benchmark result printed on screen
- Also test this by running
    - sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
- Also test Jupyter notebooks by running
    - sudo docker run --gpus all -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3-jupyter
    - wait for the download to finish
    - then pick up the link to load jupyter on browser
    - create a new notebook and test running this python code
    - check the result printed on screen if GPU is available.
    - The code below comes from https://github.com/jeffheaton/t81_558_deep_learning/blob/master/t81_558_class_01_1_overview.ipynb
import sys
import tensorflow.keras
import tensorflow as tf

check_gpu = len(tf.config.list_physical_devices('GPU'))>0

print(f"Tensor Flow Version: {tf.__version__}")
print(f"Keras Version: {tensorflow.keras.__version__}")
print()
print(f"Python {sys.version}")
print("GPU is", "available" if check_gpu else "NOT AVAILABLE")



Check for Windows update first  
Run C:\> wsl.exe to access Ubuntu Linux



Source: https://www.youtube.com/watch?v=CO43b6XWHNI
https://www.tensorflow.org/install/pip#windows-wsl2



Notes:

Don't have to do the Windows Insidewr Program. You had to do that before. Now stable.
D