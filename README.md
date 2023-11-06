<div align="center">

<h1>Retrieval-based-Voice-Conversion-WebUI</h1>
An easy-to-use voice conversion (voice changer) framework based on VITS<br><br>

[![madewithlove](https://img.shields.io/badge/made_with-%E2%9D%A4-red?style=for-the-badge&labelColor=orange
)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)

<img src="https://counter.seku.su/cmoe?name=rvc&theme=r34" /><br>

[![Open In Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)](https://colab.research.google.com/github/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/Retrieval_based_Voice_Conversion_WebUI.ipynb)
[![Licence](https://img.shields.io/badge/LICENSE-MIT-green.svg?style=for-the-badge)](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/LICENSE)
[![Huggingface](https://img.shields.io/badge/🤗%20-Spaces-yellow.svg?style=for-the-badge)](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/)

[![Discord](https://img.shields.io/badge/RVC%20Developers-Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white)](https://discord.gg/HcsmBBGyVk)

[**更新日志**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/blob/main/docs/Changelog_CN.md) | [**Frequently Asked Questions**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E7%AD%94) | [**AutoDL - 50 cents to train AI singers**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8B) | [**Record of control experiments**](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/Autodl%E8%AE%AD%E7%BB%83RVC%C2%B7AI%E6%AD%8C%E6%89%8B%E6%95%99%E7%A8%8B](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/wiki/%E5%AF%B9%E7%85%A7%E5%AE%9E%E9%AA%8C%C2%B7%E5%AE%9E%E9%AA%8C%E8%AE%B0%E5%BD%95)) | [**Online Demo**](https://huggingface.co/spaces/Ricecake123/RVC-demo)

</div>

------

[**English**](./docs/README.en.md) | [**中文简体**](./README.md) | [**日本語**](./docs/README.ja.md) | [**한국어**](./docs/README.ko.md) ([**韓國語**](./docs/README.ko.han.md))

Click here to view our [demo video](https://www.bilibili.com/video/BV1pm4y1z7Gm/) !

> Real-time voice conversion using RVC: [w-okada/voice-changer](https://github.com/w-okada/voice-changer)

> Online demo of a vocal-to-acoustic guitar model trained with the RVC vocoder ：https://huggingface.co/spaces/lj1995/vocal2guitar

> RVC Vocal to Guitar Effect Demonstration Video ：https://www.bilibili.com/video/BV19W4y1D7tT/

> The bottom model is trained using close to 50 hours of open-source, high-quality VCTK training set, with no copyright concerns.

> High-quality licensed vocal training sets will be added to train the base model.

## Introduction
This repository has the following features
+ Eliminate tone leakage by replacing input source features with training set features using top1 searches.
+ Fast training even on relatively poor graphics cards.
+ Good results with small amounts of data (at least 10 minutes of low-floor-noise speech data is recommended).
+ Possibility to change the timbre by model fusion (ckpt-merge in the ckpt-processing tab).
+ Easy-to-use web interface
+ UVR5 modeling for fast separation of vocals and accompaniment
+ Rooting out mute problems with the state-of-the-art [vocal pitch extraction algorithm InterSpeech2023-RMVPE] (#Reference Item). Best results (significantly) but faster and less resource intensive than crepe_full!

## Environment Configuration
The following commands need to be executed in an environment with Python version greater than 3.8.  

(Windows/Linux)  
Start by installing the main dependencies via pip:
```bash
# Install Pytorch and its core dependencies, skip if already installed
# Reference from: https://pytorch.org/get-started/locally/
pip install torch torchvision torchaudio

# For Win + Nvidia Ampere architecture (RTX30xx), according to #21 experience, you need to specify the cuda version of pytorch.
#pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu117
```

Dependencies can be installed using poetry:
```bash
# Install Poetry dependency management tool, skip if already installed.
# Reference from: https://python-poetry.org/docs/#installation
curl -sSL https://install.python-poetry.org | python3 -

# Install the dependencies via poetry
poetry install
```

You can also install the dependencies via pip:
```bash
pip install -r requirements.txt
```

------
Mac users can install dependencies via `run.sh`:
```bash
sh ./run.sh
```

## Other pre-model preparation
RVC needs some other pre-models for inference and training.

You can download these models from our [Hugging Face space](https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/)

Here is a list with the names of all the pre-models and other files needed for RVC:
```bash
hubert_base.pt

./pretrained 

./uvr5_weights

Additional downloads are required if you want to test the v2 version of the model.

./pretrained_v2 

If you are using Windows, you may need this file, but skip it if ffmpeg and ffprobe are already installed; ubuntu/debian users can install these two libraries via apt install ffmpeg, and Mac users can install ffmpeg via brew install ffmpeg (requires the pre-installed brew)

./ffmpeg

https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffmpeg.exe

./ffprobe

https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/ffprobe.exe

If you want to use the latest RMVPE vocal pitch extraction algorithm, then you need to download the pitch extraction model parameters and place them in the RVC root directory

https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.pt

```
After that, use the following command to start WebUI.
```bash
python infer-web.py
```

If you are using Windows or macOS, you can just download and unzip `RVC-beta.7z` and run `go-web.bat` to start WebUI for the former, or run the command `sh . /run.sh` to start WebUI.

There is also a `White Easy Tutorial.doc` in the repository for reference.

## Reference projects
+ [ContentVec](https://github.com/auspicious3000/contentvec/)
+ [VITS](https://github.com/jaywalnut310/vits)
+ [HIFIGAN](https://github.com/jik876/hifi-gan)
+ [Gradio](https://github.com/gradio-app/gradio)
+ [FFmpeg](https://github.com/FFmpeg/FFmpeg)
+ [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui)
+ [audio-slicer](https://github.com/openvpi/audio-slicer)
+ [Vocal pitch extraction:RMVPE](https://github.com/Dream-High/RMVPE)
  + The pretrained model is trained and tested by [yxlllc](https://github.com/yxlllc/RMVPE) and [RVC-Boss](https://github.com/RVC-Boss).

## Thanks to all contributors for their efforts
<a href="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=RVC-Project/Retrieval-based-Voice-Conversion-WebUI" />
</a>
