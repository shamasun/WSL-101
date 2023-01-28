# Working with Windows Subsystem for Linux (WSL)

WSL is a Windows feature that enables users to run native Linux applications, containers and command-line tools directly on Windows 11 and later. WSL2  is the 2nd generation of WSL.

Why WSL?  
- Developers working across Linux and Windows have experience a disruptive workflow. They have to stop all work to switch to the other system.
- There was a need for seamless transition and better productivity.
- Further, it allows access to applications on Windows, hitherto only available on Linux.
- TensorFlow 2.10 was the last TensorFlow release that supported GPU on native-Windows. Starting with TensorFlow 2.11, one needs to have WSL2 to use TensorFlow. The other compromise is to use tensorflow-cpu.

Steps  
- I started with a clean slate, by doing a factory reset of my Windows laptop.
- Once done, I ensured I had the latest windows update.
- I then downloaded the right NVIDIA graphics driver for my NVIDIA RTX 2060 graphics card, from [here](https://www.nvidia.com/download/index.aspx). 
- Restart whenever prompted.
- Now, run Powershell as an administrator
- In powershell, run the command
    ```
    wsl.exe --install
    ```  
- Restart system again and run Powershell as an administrator
- In powershell, run the command 
    ```
    wsl.exe --update
    ```  
- Restart system and run Powershell as an administrator
- In powershell, run the command 
    ```
    wsl -l -o  
    ``` 
- This prints several distributions of Linux on screen. Pick Ubuntu-20.04 from the list and run the command below to install the chosen version of Ubuntu.
    ```
    wsl --install -d Ubuntu-20.04
    ```  
- From the start button on the windows taskbar, run the Ubuntu-20.04 application. If you get an error that the VM could not be started because the required feature could not be installed, this means that the hypervisor is not installed. Please enable Hyper-V from your BIOS.
- Launch Ubuntu-20.04 again from start menu. Configure a UNIX user name and password when asked.
- Test if you can navigate to the C drive on Windows using 
    ```
    cd /mnt/c
    ```  
- Now, to install docker, get back to your home directory in Linux by running
    ```
    cd ...
    ```  
- Download docker by running the command below. You may get a message recommending using Docker Desktop for Windows instead. Ignore this. If you want to use the NVIDIA GPU, you got to use WSL.
    ```
    curl https://get.docker.com | sh   
    ```  
- After the installation is done, test by running 
    ```
    docker
    ```  
- Now, to install the NVIDIA Container toolkit, run the following commands one after another - 
    ```
    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg
    curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    ```  
    - Then run 
        ```
        sudo apt-get update
        ```  
    - Now install NVIDIA docker by running 
        ```
        sudo apt-get install -y nvidia-docker2
        ```  
- Open a separate WSL2 window,
    - start the docker service by running 
        ```
        sudo service docker start
        ```  
- On the original WSL2 window, run the code below to notice that nothing is running as expected
    ```
    sudo docker ps
    ```  
- Now run a benchmark test by running the code below. You should get the benchmark result printed on screen
    ```
    sudo docker run --gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
    ```  
- Also test this by running
    ```
    sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
    ```  
- Also test Jupyter notebooks by running
    ```
    sudo docker run --gpus all -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3-jupyter
    ```  
    - wait for the download to finish
    - then pick up the URL to load jupyter on browser
    - create a new notebook and run the python code below to check if GPU is available.
        ```
        import sys
        import tensorflow.keras
        import tensorflow as tf

        check_gpu = len(tf.config.list_physical_devices('GPU'))>0

        print(f"Tensor Flow Version: {tf.__version__}")
        print(f"Keras Version: {tensorflow.keras.__version__}")
        print()
        print(f"Python {sys.version}")
        print("GPU is", "available" if check_gpu else "NOT AVAILABLE")
        ```  

Sources: 
https://www.youtube.com/watch?v=CO43b6XWHNI
https://www.tensorflow.org/install/pip#windows-wsl2
https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html
https://learn.microsoft.com/en-us/windows/wsl/install
https://docs.nvidia.com/cuda/wsl-user-guide/index.html#step-3-set-up-a-linux-development-environment
https://github.com/jeffheaton/t81_558_deep_learning/blob/master/t81_558_class_01_1_overview.ipynb