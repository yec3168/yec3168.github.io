---
title: Tacotron2 작성법
author:
name: Eungchan
link: https://github.com/yec3168
date: 2023-06-29
categories: [Project, tacotron2]
tags: [tacotron2, Project]
---


# 예시
Tacotron2 및 Waveglow 모델은 torch.hub에서 가져온다. Tacotron2는 ("hello my world, i miss you")와 같은 텍스트 입력을 주어지면 mel-spectrogram을 생성한다.

Waveglow는 mel-spectrogram을 입력으로 음성을 생성한다.

해당 정보는 `audio.wav` 파일로 저장된다.


# Package install

```bash
pip install numpy scipy librosa unidecode inflect librosa
```
```bash
apt-get update
```
```bash
apt-get install -y libsndfile1
```

위 명령어를 차례대로 터미널에 입력한다.


# Tacotron2 load

Tacotron2 모델을 불러오기 위해선 `torch`가 필요하다.

```python
import torch
tacotron2 = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_tacotron2', model_math='fp16')
tacotron2 = tacotron2.to('cuda')
tacotron2.eval()
```

위 명령어를 입력하게 되면 [LJ Speech dataset ](https://keithito.com/LJ-Speech-Dataset/)에 데이터셋에서 사전 훈련된 Tacotron2를 불러올 수 있다.

# WaveGlow load

```python
waveglow = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_waveglow', model_math='fp16')
waveglow = waveglow.remove_weightnorm(waveglow)
waveglow = waveglow.to('cuda')
waveglow.eval()
```
위 명령어도 입력하면 사전 훈련된 WaveGlow모델을 불러올 수 있다.

# Utils load

```python
utils = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_tts_utils')
sequences, lengths = utils.prepare_input_sequence([text])
```


# Input Text 

```python
text = "hello my world, i miss you"
```


# 연결된 모델 실행

```python
with torch.no_grad():
    mel_outputs_postnet, mel_lengths, alignments = tacotron2.infer(sequences, lengths)
    audio = waveglow.infer(mel)
audio_numpy = audio[0].data.cpu().numpy()
rate = 22050
```


# Audio 파일 저장

```python
from scipy.io.wavfile import write
write("audio.wav", rate, audio_numpy)
```


# 최종 코드

```python
import torch
from scipy.io.wavfile import write


tacotron2 = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_tacotron2', model_math='fp16')
waveglow = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_waveglow', model_math='fp16')
utils = torch.hub.load('NVIDIA/DeepLearningExamples:torchhub', 'nvidia_tts_utils')


tacotron2 = tacotron2.to('cuda')
waveglow = waveglow.to('cuda')
waveglow = waveglow.remove_weightnorm(waveglow)


tacotron2.eval()
waveglow.eval()


text = "hello my world, i miss you"

sequences, lengths = utils.prepare_input_sequence([text])


with torch.no_grad():
    mel_outputs_postnet, mel_lengths, alignments = tacotron2.infer(sequences, lengths)
    audio = waveglow.infer(mel)

audio_numpy = audio[0].data.cpu().numpy()


rate = 22050
write("audio.wav", rate, audio_numpy)

```

# Information
[Pytorch](https://pytorch.kr/hub/nvidia_deeplearningexamples_tacotron2/)와[Github](https://github.com/NVIDIA/DeepLearningExamples/tree/master/PyTorch/SpeechSynthesis/Tacotron2)를 참고해서 작성했다.