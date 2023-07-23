# Jetson-Orin-Developer-kit-setup


Setup Tips 


1) JETSON ORIN DEVELOPER KIT SETUP (Physical and software setup)  
   - Remove SD card
   - Install NVME drive (e.g. Samsung 970 EVO Plus - 1TB)
   - Use Jetson SDK Manager to install software into NVME drive 
   - Attach USB webcam e.g. Logitech C902

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
 - Execute Docker Container (select one of the 7 demo python programs to execute)
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
