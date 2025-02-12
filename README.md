# Jetson-Orin-Developer-kit-setup

** THIS IS OLD - I'VE ONLY LEFT IT HERE FOR ARCHIVE REASONS - THE JETSON ORIN NANO SUPER IS SO MUCH BETTER - I WILL UPDATE THIS ONE DAY **


Setup Tips 

Be aware that when a Jetson Orin developer kit is powered down, it seems to forget what USB devices are attached to it. Consider that after a powerup, you always have to remove and re-insert the USB cables for the webcam and speakers to make these devices re-recognised as being attached.   

## 1) ORIN - JETSON ORIN DEVELOPER KIT SETUP - PART1 - (Physical and software setup) 
   - In your Ubuntu desktop, execute the Jetson SDK Manager software to flash the Jetson Orin nano as per here. N.B. Be aware, the steps below refer to the use of a NVME drive with no SD card installed.
     - https://www.youtube.com/watch?v=Ucg5Zqm9ZMk&t
   - Here are some notes of the actions that worked for me, based on the video above with the actions listed in sequence, but grouped by the device you need to progress the actions on
     - Ubuntu Desktop
       - Install Ubuntu 20.04 onto desktop with at least 256 G hard drive memory 
       - Download and install SDK manager software from here https://developer.nvidia.com/embedded/downloads#?search=sdk
     - Jetson Orin Developer kit (start with all cables unplugged and no power cable connected)
       - Physically remove SD card (Just push it in and it will spring out slightly)
       - Physically install NVME drive (e.g. Samsung 970 EVO Plus - 1TB) https://www.youtube.com/watch?v=q4fGac-nrTI - Don't do anything else in the video - just install NVME drive
       - Use a female to female dupont lead to short pins 9 and 10 together - N.B. To locate these pins, look at the row of 12 pins seen on the side of the Jetson developer kit.
       - Attach USB "C" cable to Jetson Orin developer kit and connect to Ubuntu desktop
       - Attach network cable to Jetson Orin Developer kit and connect to router/network 
       - Attach HDMI cable to Jetson Orin Developer kit and connect to a display
       - Attach USB keyboard and mouse 
       - Attach power cable and power up Jetson Orin (it will boot in recovery mode because pins 9 and 10 are shorted)
     - Ubuntu Desktop
       - Execute SDK manager and start and progress each step of the 4 step process using the manual method - Click here to see how this looks https://www.youtube.com/watch?v=Ucg5Zqm9ZMk&t=417s N.B. The Jetson Orin developer kit should be recognised and ticked "green", with a summary of the actions seen in the video, listed as follows:
         - Choose a "USB" connection
         - Choose "pre-config"  &nbsp; &nbsp; &nbsp; N.B. Whilst this selection is different from what the video says, using "pre-config" worked for me
         - Choose "Manual method"
         - Select "NVME" drive for installing software.
         - Note the name you are giving the Jetson Orin developer kit - I used "orin"
         - Note the user and password you are using.
         - Start the download and flashing process 
       - If flashing seems to be working OK, wait a few seconds and then remove the dupont lead from pins 9 and 10. If this dupont lead was left connected, it would cause a re-boot problem later on because the Jetson Orin developer kit would always boot in recovery mode. Later on in this process, we really want the Jetson Orin developer kit to boot to a logon screen instead of booting in recovery mode, so remove the dupont lead now!
       - When the first part of initial flashing has completed, another window is displayed - Before pressing continue, do a few things on the Jetson Orin developer kit to find out the IP address (as below in the next few steps). Therafter continue as per the next section titled "Ubuntu Desktop" 
     - Jetson Orin Developer kit 
       - Remove the USB "C" cable
       - Log into the Jetson Orin developer kit using the user and password you noted before - and find out its IP address through the network cable
       - Log out of Jetson Orin Developer kit
     - Ubuntu Desktop
       - In the new window
         - Choose an "ETHERNET" connection (not "USB")
         - Enter the IP address just found
         - Press continue
         - Wait until process is complete.
       - Once the flashing is complete, most people no longer need to keep the Ubuntu desktop really.   
     - Jetson Orin Developer kit 
       - Log into the Jetson Orin developer kit using the user and password you noted before
       - Attach USB webcam e.g. Logitech C902  
       - Attach USB speakers
       - OPTIONALLY - Set up Wifi as approrpriate - via a wifi card or dongle and remove the network cable.

## 2) ORIN - JETSON ORIN DEVELOPER KIT SETUP - PART2 - (Setup, options and useful commands that you use in a terminal window or SSH session) 
 - Set up SWAP and make it permanent - By executing the following in a terminal window
   - sudo systemctl disable nvargus-daemon.service
   - sudo systemctl stop nvzramconfig.service
   - sudo systemctl disable nvzramconfig
   - sudo fallocate -l 16G /16GB.swap
   - sudo chmod 600 /16GB.swap
   - sudo mkswap /16GB.swap
   - sudo swapon /16GB.swap
   - sudo vim ~/.etc/fstab
     - Notes on how to use VIM
       - To change/insert, press "Esc" and enter ":i"
       - To exit, press "ESC" and enter ":x!" to save and exit or ":q" to quit without saving
     - Add the following line to the end of /etc/fstab to make the change persistent:
	    - /ssd/16GB.swap  none  swap  sw 0  0
 - Set up the $PATH variable so that it is activate during SSH access - By executing the following in a terminal window
   - sudo vim ~/.bash_profile
     - Notes on how to use VIM
       - To change/insert, press "Esc" and enter ":i"
       - To exit, press "ESC" and enter ":x!" to save and exit or ":q" to quit without saving
     - Add the following line to the end of .bash_profile to make the change persistent:
	    - export PATH=$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
- How to access Jetson via SSH
  - On your WIN10 PC, open a terminal window and execute the following command
    - ssh 192.168.1.*AAA* -l *BBB*		(where *AAA* points to your Orin Nano in the network, and *BBB* is the name you have given it during the setup above e.g. "orin")    
- Temporaily stop the desktop - From terminal window or SSH session
  - sudo init 3
- How to restart the desktop - from a SSH session
  - sudo init 5 
- Change boot up behaviour - If you wish to make this persistent across reboots, you can use the following commands to change the boot-up behavior:
  - To disable desktop on boot
	 - sudo systemctl set-default multi-user.target
  - To enable desktop on boot
	 - sudo systemctl set-default graphical.target
- To improve SSH access memory
  - sudo nvpmodel -m 0
  - sudo jetson_clocks
- To shutdown immediately  
  - sudo shutdown -h now  
- To shutdown and restart  
  - sudo shutdown -r now
- To get stats on memory use
  - free -m
  - free -h
  - sudo tegrastats
      
## 3) ORIN - Install Jetson Inference Demo software (Dusty-nv)
  (N.B. Make sure you attach a Webcam before executing the DEMO programs)
 - Install Docker Container - Open a terminal and execute the following:
   - cd Desktop
   - mkdir zdemo
   - cd zdemo
   - git clone --recursive --depth=1 https://github.com/dusty-nv/jetson-inference
 - Execute Docker Container (select one of the 7 demo python programs to execute) N.B. The first time each python program is executed, they take a long time to complete their "one-time only" build - Just wait - The first execution can take up to 10 mins, but subsequent executions fire up after a few seconds.
   - cd ~/Desktop/zdemo/jetson-inference
   - docker/run.sh
   - cd jetson-inference/build/aarch64/bin 
   - ./imagenet.py /dev/video0   &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 1 - Image Classification)
   - ./detectnet.py /dev/video0  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 2 - Object Detection)
   - ./segnet.py /dev/video0     &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 3 - Semantic Segmentation)
   - ./posenet.py /dev/video0    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 4 - Pose Estimation)
   - ./actionnet.py /dev/video0  &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 5 - Action Recognition)
   - ./backgroundnet.py /dev/video0   &nbsp; &nbsp; (DEMO 6 - Background removal)
   - ./depthnet.py /dev/video0        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (DEMO 7 - Monocular Depth)     

## 4) ORIN - Install LLAMA text-generation-webui
Tutorial is here  [http://www.jetson-ai-lab.com/tutorial-intro.html](https://www.jetson-ai-lab.com/tutorial_text-generation.html)

N.B. If you have followed enough of the steps below to get the webpage to be displayed (http://192.168.1.*aaa*:7860), it makes sense to re-boot the Jetson Orin nano in "headless" mode (i.e. without the desktop being displayed) and use SSH to login, and then execute the following commands to get the webpage back up. By doing this, you maximise the memory that can be used by the LLM model.  
 - cd jetson-containers
 - ./run.sh $(./autotag text-generation-webui)

However, these are the steps to help you get things setup while you are still in "desktop" mode
 - Open a terminal window  
   - git clone --depth=1 https://github.com/dusty-nv/jetson-containers
   - cd jetson-containers
   - sudo apt update
   - sudo apt install -y python3-pip
   - pip3 install -r requirements.txt

     - Open a browser on your WIN10 machines and go to http://192.168.1.XXX:7860  where XXX points to your networked Jetson Orin Nano. If you have not networked your Orin nano, then execute a browser on your Orin Nano and use http://localhost:7860  
       - Set the model - Near the top, click on menu tab **"model"** 
         - Action 1 - Download **"TheBloke/Mistral-7B-OpenOrca-GPTQ"**
           - In field **"Download model or loRA"** (right window), populate field with **"TheBloke/Mistral-7B-OpenOrca-GPTQ"**.
           - Press button **"Get File List"**
           - Press button **"Download"** and wait until you see a message saying "Done"
         - Action 2 - Load Model  **"TheBloke_Mistral-7B-OpenOrca-GPTQ"** *(Note that the "/" has turned into a "-")*
           - In field **"Model"** (left window near the top), select  **"TheBloke_Mistral-7B-OpenOrca-GPTQ"**
           - In field **"Model Loader"** (left window near the top), select  **"ExLlamav2_HF"**
           - In field **"max_seq_len"** (left window near the top), enter a value of **"15360"** (if you get an error, start with a value of **"4098"** and increase until there is no error)
           - Press button **"Load"** (left window near the top) - Wait until a message is seen in the right window saying "Successfully loaded"
       - Set parameters - Near the top, click on menu tab **"Parameters"**, and then on sub-menu tab **"Character"**
         - Action 1 - In the Character submenu, you can load a set of names and pictures of both you and the AI - but be aware, they are only stored for your browser session
         - Action 2 - Via the dropdown, make sure you select the character you want the AI to use via the dropdown
       - Have a chat with your AI - Near the top, click on menu tab **"Chat"**
         - Action 1 - Start your conversation !      

       - *OPTIONAL*
         - *Only bother downloading and uploading these models if you want to practise getting models and you want to experiment with them. The model listed above (**TheBloke/Mistral-7B-OpenOrca-GPTQ**) is the best model to use anyway - as of January 2024* 
           - **OPTIONAL MODEL 1** 
             - Download -  **"TheBloke/Llama-2-7b-Chat-GGUF"**
               - In field **"Download model or loRA"** (right window), populate field with **"TheBloke/Llama-2-7b-Chat-GGUF"**.
               - Press button **"Get File List"**
               - Press button **"Download"** and wait until you see a message saying "Done"
             - Upload model 1 - **"llama-2-7b-chat.Q4_K_M.gguf"**
               - In field **"Model"** (left window near the top), select  **"llama-2-7b-chat.Q4_K_M.gguf"**
               - In field **"Model Loader"** (left window near the top), select  **llama.cpp**
               - Populate other fields as below:
                 - n-gpu-layers = 128
                 - n-ctx = 2048
                 - threads = 4
                 - threads_batch = 4
                 - n_batch = 512
               - Press button **"Load"** (left window near the top) - Wait until a message is seen in the right window saying "Successfully loaded"
             - Alternative model 2 - **"llama-2-7b-chat.Q5_K_M.gguf"**
               - In field **"Model"** (left window near the top), select  **"llama-2-7b-chat.Q5_K_M.gguf"**
               - In field **"Model Loader"** (left window near the top), select  **"llama.cpp"**
               - Populate other fields
                 - n-gpu-layers = 128
                 - n-ctx = 1024
                 - threads = 4
                 - threads_batch = 4
                 - n_batch = 512
               - Press button **"Load"** (left window near the top) - Wait until a message is seen in the right window saying "Successfully loaded" 
           - **OPTIONAL MODEL 2** 
             - Download -  **"TheBloke/Mistral-7B-v0.1-GGUF"**
               - In field **"Download model or loRA"** (right window), populate field with **"TheBloke/Mistral-7B-v0.1-GGUF"**.
               - Press button **"Get File List"**
               - Press button **"Download"** and wait until you see a message saying "Done"
             - Load Model  **"mistral-7b-v0.1.Q5_K_M.gguf"**
               - In field **"Model"** (left window near the top), select  **"mistral-7b-v0.1.Q5_K_M.gguf"**
               - In field **"Model Loader"** (left window near the top), select  **"llama.cpp"**
               - Populate other fields as below:
                 - n-gpu-layers = 128
                 - n-ctx = 4096
                 - threads = 4
                 - threads_batch = 4
                 - n_batch = 512
	       - Press button **"Load"** (left window near the top) - Wait until a message is seen in the right window saying "Successfully loaded" 
           - **OPTIONAL MODEL 3** 
             - Download -  **"TheBloke/Mistral-7B-OpenOrca-GGUF"**
               - In field **"Download model or loRA"** (right window), populate field with **"TheBloke/Mistral-7B-OpenOrca-GGUF"**.
               - Press button **"Get File List"**
               - Press button **"Download"** and wait until you see a message saying "Done"
             - Load Model  **"mistral-7b-openorca.Q5_K_M.gguf"**
               - In field **"Model"** (left window near the top), select  **"mistral-7b-openorca.Q5_K_M.gguf"**
               - In field **"Model Loader"** (left window near the top), select  **"llama.cpp"**
               - Populate other fields as below:
                 - n-gpu-layers = 128
                 - n-ctx = 4096
                 - threads = 4
                 - threads_batch = 4
                 - n_batch = 512
	       - Press button **"Load"** (left window near the top) - Wait until a message is seen in the right window saying "Successfully loaded" 

## 5) ORIN - INSTALL DEEPSTREAM - Follow info from these 2 references (I hope to write a condensed walkthrough soon - on how to do this) 
   - https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_Quickstart.html
   - https://www.youtube.com/watch?v=vDxL2-YJcSY&t=637s
   - Once setup, use the commands listed against either the "DEEPSTREAM APP" option or the "PYTHON PROGRAMS" option
     - DEEPSTREAM APP option
       - Run the C program code by executing the fllowing command
         - sudo deepstream-app -c /opt/nvidia/deepstream/deepstream-6.4/samples/configs/deepstream-app/source1_usb_dec_infer_resnet_int8.txt 
     - PYTHON PROGRAMS option
       - Consideration to think about - When executing the python program, should you use the prefix sudo or not?
         - You only need to use "sudo" the first time you execute the python program. This gives permission to the program to store the created model for subsequent re-use. If sudo is not used at least once, then the python program will not find the model and so the program will decide to create the model every time the program is executed - it takes AGES to do this!
       - Change the directory permissions using the command in the following line (This allows you to store custom code in the directory):
         - sudo chmod a+rwx /opt/nvidia/deepstream/deepstream/sources/deepstream_python_apps/apps/deepstream-test1-usbcam/
       - Make changes to content of directory
         - Clone "config.txt" file and change the clone using vim (or other text editor
           - Copy "config.txt" file to "zconfig.txt"
           - Edit "zconfig.txt" to change the line that denotes the model engine ref to use **B30** instead of **B1**
           - Save "zconfig.txt"
         - Clone python program "deepstream_test_1_usb.py" and change the clone using vim (or other text editor)
           - Copy "python" program "deepstream_test_1_usb.py" to "zdeepstream_test_1_usb.py"
           - Edit "zdeepstream_test_1_usb.py" and change the line that refers to "config.txt" to refer instead to "zconfig.txt" (as created in the previous step)
           - Save "zdeepstream_test_1_usb.py" 
       - Run the cloned python program code by executing the following code)
         - cd /opt/nvidia/deepstream/deepstream/sources/deepstream_python_apps/apps/deepstream-test1-usbcam/
         - sudo -E python3 zdeepstream_test_1_usb.py /dev/video0 

## 6) ORIN - Install vscode
   - Using a browser, download the Ubuntu ".deb" file for "ARM64" from https://code.visualstudio.com/download
   - Open a terminal and execute the following: 
     - cd Downloads
     - sudo dpkg -i XXXXX.deb    (Where XXXXX.deb is the downloaded ".deb" file)
   - After it has installed, the VsCode icon can be accessed when you press the bottom left icon (like START in Windows!) and look in the "programming" section.   
   - Right click the VsCode icon and Select "add to desktop" for easy access

## 7) ORIN - Face and speech recognition. 
   - Open a terminal and execute:
     - sudo apt-get update
     - sudo apt-get upgrade
     - sudo apt install libcanberra-gtk-module libcanberra-gtk3-module -y
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
   - Look here for programs. To install, use "git clone" and execute within VsCode - make sure you change the python version from 3.9 to 3.8 (VsCode-Bottom Right)
     - Face Recognition
       - https://github.com/CurlyWurly-1/Face_Recognition_With_Moving_Eyeball
     - Speech Recognition with a spoken response taken from GPT-3 
       - https://github.com/CurlyWurly-1/Chatbot
         
## 8) WIN10 - Install the FileZilla "client" on your Win10 laptop
   - On your Windows machine, download the filezilla client via https://filezilla-project.org/  It makes your life a lot easier when you are able to transfer files to and from the Jetson device with ease. It means you can use a win10 desktop to backup important Jetson stuff (e.g. Jetson python programs and tip sheets) and easily re-store it back whenever you re-flash your Jetson device with SDK manager.
     

       
## Spare info Stuff (backup - not tested - ignore)

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
