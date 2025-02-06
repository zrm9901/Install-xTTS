<h1>install Ubuntu 22.04 on WSL2</h1>
wsl --install -d Ubuntu-22.04

<h1>install cuda:</h1>
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin <br>
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600 <br>
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb <br>
sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb <br>
sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/ <br>
sudo apt-get update <br>
sudo apt-get -y install cuda <br>

<h1>install cuDNN:</h1> 
<a href="https://developer.nvidia.com/downloads/compute/cudnn/secure/8.9.7/local_installers/11.x/cudnn-local-repo-ubuntu2204-8.9.7.29_1.0-1_amd64.deb/" target="_blank">Download this and move it into your WSL home directory ("\\wsl.localhost\Ubuntu-22.04\home\{"whatever you named your user when installing wsl"}</a> <br>
sudo dpkg -i cudnn-local-repo-ubuntu2204-8.9.7.29_1.0-1_amd64.deb <br>
sudo cp /var/cudnn-local-repo-ubuntu2204-8.9.7.29/cudnn-local-8AE81B24-keyring.gpg /usr/share/keyrings/ <br>

<h1>Install PyEnv</h1>
curl -fsSL https://pyenv.run | bash <br>
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc <br>
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc <br>
echo 'eval "$(pyenv init - bash)"' >> ~/.bashrc <br>
touch ~/.profile <br>
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.profile <br>
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.profile <br>
echo 'eval "$(pyenv init - bash)"' >> ~/.profile <br>
exec "$SHELL" <br>
sudo apt update; sudo apt install build-essential libssl-dev zlib1g-dev \ <br>
libbz2-dev libreadline-dev libsqlite3-dev curl git \ <br>
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev <br>

<h1>Install Python 10.0.16</h1>
pyenv install 3.10.16 <br>
pyenv global 3.10.16 <br>
python3 -m pip install --upgrade pip <br>
python -m pip install --upgrade setuptools wheel <br>

<h1>Clone coqui-ai-TTS</h1>
git clone https://github.com/idiap/coqui-ai-TTS <br>
cd coqui-ai-TTS <br>

<h1>Install the Training Notebook Dependencies</h1>
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118 <br>
pip install -e .[server,notebooks] <br>
python3 -m pip install -r TTS/demos/xtts_ft_demo/requirements.txt <br>
pip install --force-reinstall fastapi[all]==0.110.0 starlette==0.36.3 pydantic==2.6.4 <br>

<h1>Configure The Whisper Model</h1>
Your going to want to go into "coqui-ai-TTS/TTS/demos/xtts_ft_demo/utils/formatter.py" and change large-v2 in "asr_model = WhisperModel("large-v2", device=device, compute_type="float16")" on line 60 to either tiny, small, or medium, depending on your amount of vram. please note that the smaller the model the less accurate. In my case im on a laptop 4050 so I will be using small.

<h1>Fine Tune Your Model</h1>
python3 TTS/demos/xtts_ft_demo/xtts_demo.py <br>
<a href="https://www.youtube.com/watch?v=8tpDiiouGxc&amp;feature=youtu.be" target="_blank">Follow this tutorial video after clicking the link the terminal gives you to fine tune the model</a> <br>
Best of luck! <br>
