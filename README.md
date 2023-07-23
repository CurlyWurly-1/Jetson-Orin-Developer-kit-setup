# Jetson-Orin-Developer-kit-setup


Setup Tips 


1) JETSON ORIN DEVELOPER KIT SETUP (Physical and software setup) - Be aware that you may have to remove and re-insert the USB cables for the webcam and speakers to work OK after a power up. This seems to be necessary because after a power up, the Jetson Developer kit can "forget" what USB devices are already attached to it. Removing and re-inserting the USB cable for the affected devices seems to fix this.  
   - Use the Jetson SDK Manager software to flash the NVME drive with Jetson software as per here https://developer.nvidia.com/embedded/learn/jetson-agx-orin-devkit-user-guide/two_ways_to_set_up_software.html
     - Ubuntu Desktop
       - Install Ubuntu 20.04 onto desktop with at least 256 G hard drive memory 
       - Download and install SDK manager software from here https://developer.nvidia.com/embedded/downloads#?search=sdk
     - Jetson Orin Developer kit (start with all cables unplugged and no power cable connected)
       - Physically remove SD card (Just push it in and it will spring out slightly)
       - Physically install NVME drive (e.g. Samsung 970 EVO Plus - 1TB) https://www.youtube.com/watch?v=q4fGac-nrTI - Don't do anything else in the video - just install NVME drive
       - Use a female to female dupont lead to short pins 9 and 10 together. Pins 9 amd 10 are part of a row of 12 pins that can be seen on the side of the Jetson developer kit.
       - Attach USB "C" cable to Jetson Orin developer kit and connect to Ubuntu desktop
       - Attach network cable to Jetson Orin Developer kit and connect to router/network 
       - Attach HDMI cable to Jetson Orin Developer kit and connect to a display
       - Attach USB keyboard and mouse 
       - Attach power cable and power up Jetson Orin (it will boot in recovery mode because pins 9 and 10 are shorted)
     - Ubuntu Desktop
       - Execute SDK manager and the Jetson Orin developer kit should be seen - Start the download and flashing process and when prompted,
         - Choose a "USB" connection
         - Choose "pre-config"
         - Choose "Manual method"
         - Select "NVME" drive for installing software.
         - Note the name you are giving the Jetson Orin developer kit - I used "orin"
         - Note the user and password you are using.
       - When the flashing actually starts, wait a few seconds to check that all is progressing OK. If it is, remove the dupont lead from pins 9 and 10 because it is now no longer needed. If this dupont lead was left connected, it would cause a re-boot problem later on because the Jetson Orin developer kit would always boot in recovery mode instead of booting to a normal logon screen. Later on in this process, we really want the Jetson Orin developer kit to boot to a logon screen, so remove the dupont lead now!
       - When the first part of initial flashing has completed, another window is displayed - before continuing, do a few things on the Jetson Orin developer kit as below - To find out the IP address
     - Jetson Orin Developer kit 
       - Remove the USB "C" cable
       - Log into the Jetson Orin developer kit using the user and password you noted before - and find out its IP address through the network cable
       - Log out of Jetson Orin Developer kit
     - Ubuntu Desktop
       - In the new window
         - Ensure you are using the "ethernet" option
         - Enter the IP address just found
         - Press continue
         - Wait until process is complete.
       - Once the flashing is complete, most people no longer need to keep the Ubuntu desktop really.   
     - Jetson Orin Developer kit 
       - Log into the Jetson Orin developer kit using the user and password you noted before
       - Attach USB webcam e.g. Logitech C902  
       - Attach USB speakers
       - Set up Wifi as approrpriate - via a wifi card or dongle and optionally remove the network cable.

2) INSTALL VSCODE
   - Using a browser, download the Ubuntu ".deb" file for "ARM64" from https://code.visualstudio.com/download
   - Open a terminal and execute the following: 
     - cd Downloads
     - sudo dpkg -i XXXXX.deb    (Where XXXXX.deb is the downloaded ".deb" file)
   - After it has installed, the VsCode icon can be accessed when you press the bottom left icon (like START in Windows!) and look in the "programming" section.   
   - Right click the VsCode icon and Select "add to desktop" for easy access

3) INSTALL JETSON INFERENCE Demo software and test
 - Install Docker Container - Open a terminal and execute the following:
   - mkdir zdemo
   - cd zdemo
   - git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
 - Execute Docker Container (select one of the 7 demo python programs to execute) N.B. The first time each python program is executed, they take a long time to complete their "one-time only" build - Just wait - It can take up to 10 mins.
   - cd ~/Desktop/zdemo/jetson-inference
   - docker/run.sh
   - cd build/aarch64/bin 
   - ./imagenet.py /dev/video0   &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 1 - Image Classification)
   - ./detectnet.py /dev/video0  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 2 - Object Detection)
   - ./segnet.py /dev/video0     &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 3 - Semantic Segmentation)
   - ./posenet.py /dev/video0    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 4 - Pose Estimation)
   - ./actionnet.py /dev/video0  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 5 - Action Recognition)
   - ./backgroundnet.py /dev/video0   &nbsp; &nbsp; (DEMO 6 - Background removal)
   - ./depthnet.py /dev/video0        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 7 - Monocular Depth)     

4) INSTALL MORE LIBRARIES (for GITHUB programs in step 5 - Execute individually and respond "Y" when necessary)
   - Open a terminal and execute:
     - sudo apt-get update
     - sudo apt-get upgrade
     - sudo apt-get install nano
     - sudo apt-get install python3-pip 
     - export PATH="/home/XXX/.local/bin:$PATH"  (replace XXX with your system name e.g. ORIN)
     - pip3 install cython
     - pip3 install gtts
     - pip3 install playsound
     - sudo apt-get install sox
     - sudo apt-get install libsox-fmt-all
     - pip3 install pyttsx3
     - sudo apt install espeak
     - pip3 install pyserial
     - sudo apt-get update
     - sudo apt-get upgrade
     - sudo apt-get install cmake 
     - sudo pip3 install face_recognition
     - sudo apt-get install portaudio19-dev  
     - sudo apt-get install python-all-dev 
     - sudo apt-get install python3-all-dev 
     - pip3 install pyaudio
     - pip3 install SpeechRecognition
     - sudo pip3 install aiohttp
     - git clone https://github.com/openai/openai-python.git
     - cd openai-python
     - sudo python3 setup.py install
     - sudo apt-get install flac

5) Now your Jetson Orin Developer Kit is ready to do face recognition with full CUDA GPU acceleration and speech recognition. Look here for programs. To install, use "git clone" and execute within VsCode - make sure you change the python version from 3.9 to 3.8 (VsCode-Bottom Right)
 - Face Recognition
   - https://github.com/CurlyWurly-1/Face_Recognition_With_Moving_Eyeball
 - Speech Recognition with a spoken response taken from GPT-3 
   - https://github.com/CurlyWurly-1/Chatbot

       
## Extra Stuff (backup - not tested - ignore)

-) INSTALL THE COURSE JUPYTER NOTEBOOK. 
   - In a terminal window, execute the following commands (it takes quite a bit of time to install). When it finishes, check what the IP address of the nano is <Nano_ip> and you can access the jupyter notebook with a web browser using a url of <NANO_IP):8888  (use "dlinano" as the password) 
     - mkdir -p ~/nvdli-data
     - echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/nvdli-data:/nvdli-nano/data --device /dev/video0  nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh
     - chmod +x docker_dli_run.sh
     - ./docker_dli_run.sh    
   - If you reboot, you can bring back the Jupyter notebook by executing the last command of the above as follows (There is no need to re-install from scratch):
     - ./docker_dli_run.sh


-) ENROL IN COURSE HERE !!!
   - https://courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/courseware/b2e02e999d9247eb8e33e893ca052206/63a4dee75f2e4624afbc33bce7811a9b/


-) NOT NEEDED - JUST HERE FOR INFO - e.g. HOW TO INSTALL PYTHON VIRTUAL ENVIRONMENT.
   - In a terminal window, execute the following commands:
     - sudo apt-get install -y python3-venv
     - python3 -m venv .py3venv
     - ls -a
     - ls .py3venv/
     - source ~/.py3venv/bin/activate
     - deactivate
     - sudo apt-get update
     - sudo apt-get install build-essential
     - sudo apt-get install libncurses5-dev
     - sudo apt-get install libgdbm-dev
     - sudo apt-get install libnss3-dev
     - sudo apt-get install libssl-dev
     - sudo apt-get install libreadline-dev
     - sudo apt-get install libffi-dev
     - git clone https://github.com/openai/openai-python.git
     - cd openai-python
     - sudo python3 setup.py install
     - sudo apt-get install flac
     - sudo pip3 uninstall numpy
     - pip3 install numpy
