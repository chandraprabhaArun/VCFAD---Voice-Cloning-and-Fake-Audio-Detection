# Voice-Cloning-and-Fake-Audio-Detection
### Introduction
A technology company working in the Cyber Security industry wants to build data driven systems that help individuals and organizations to understand whether audio and video media is authentic or fake.

### Data Description:
Two datasets will be used in this project:

[**TIMIT Dataset:**](https://github.com/philipperemy/timit) The TIMIT corpus of read speech is designed to provide speech data for acoustic-phonetic studies and for the development and evaluation of automatic speech recognition systems. TIMIT contains a total of 6300 sentences, 10 sentences spoken by each of 630 speakers from 8 major dialect regions of the United States.

[**CommonVoice Dataset:**](https://commonvoice.mozilla.org/en/datasets) Common Voice is part of Mozilla's initiative to help teach machines how real people speak. Common Voice is a corpus of speech data read by users on the [Common Voice website](https://commonvoice.mozilla.org/), and based upon text from a number of public domain sources like user submitted blog posts, old books, movies, and other public speech corpora. Its primary purpose is to enable the training and testing of automatic speech recognition (ASR) systems.

### Goals(s):
The goal of this project is to build algorithms that can synthesize spoken audio by converting a speaker’s voice to another speaker’s voice with the end goal to detect if any spoken audio is pristine or fake.

### Success Metrics:
* **Voice cloning (VC):** We will use Word Error Rate (WER) for automatic evaluation of the voice cloning (VC) system for the speech generation part of the project
* **Fake audio detection (FAD):** We will evaluate the performance of the classifier model using F-score 

### Approach:
#### Voice Cloning system (VC)
We will use the [implementation](https://github.com/CorentinJ/Real-Time-Voice-Cloning) of [Transfer Learning from Speaker Verification to Multispeaker Text-To-Speech Synthesis (SV2TTS)](https://arxiv.org/abs/1806.04558) to develop the voice cloning system for this project. SV2TTS is a deep learning framework that utilizes the TIMIT dataset for training and synthesizing spoken audio. It consists of the following components:
* **Encoder**: Pretrained encoder that creates a digital representation (embeddings) of a voice from a few seconds of audio.
* **Synthesizer**: Model for synthesizing spectrograms from text and speaker embeddings.
* **Vocoder**: Pretrained vocoder model for generating the waveform from spectrograms.

To evaluate the performace of the voice cloning system we will experiment with the following voice cloning techniques:
* Short Audio with a short text: The original files have been cloned with one short sentence. WER: 0.25
* Short Audio with a long text: The original files have been cloned with longer sentences. WER: 0.58
* Long Audio with a short text: Each speaker's audio files have been merged into one file and cloned with one short sentence. WER: 0.16
* Long Audio with a long text: Each speaker's audio files have been merged into one file and cloned with longer sentence. WER: 0.44

The third technique has been chosen to clone audio based on how accurate the model is to convert the text to speech. This was done by converting the cloned audio to the text again and calculating the similarity between the original text and the one from the cloned audio. The model performs well while generating audio files that are shorter and uses long audio files as refernece.

#### Fake audio detection system (FAD) 
We will build a classification model that detects if a given audio is synthetically generated or not. We will utilize the CommonVoice dataset as it consists of thousands of naturally spoken audio which could be used as positive examples and creating negative examples using the voice cloning system as automatic data/label generator. The FAD system involves the following steps:

* Collect real and fake audio samples. Additionally, the model is also evaluated using synthetic audio generated as part of the [blizzard challenge](https://www.cstr.ed.ac.uk/projects/blizzard/data.html)
* Audio features such as MFCCs, chroma, mel, contrast, and tonnetz are extracted from both real and synthetic datasets.
* Scale the features using StandardScaler.
* Train a binary classification model using the scaled features.

#### Results:
The model achieved a perfect F-score on the test data, signifying its capability to distinguish fake audio with the highest precision and recall.
![Screenshot 2023-11-03 224938](https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection/assets/47222625/76c6604b-8f87-462f-adc1-94d03e88a201)

| Metrics | Score |
| --- | --- |
| Training Accuracy | 100%  |
| Test Accuracy | 99.87% |
| F1 Score | 0.99 |

### WebApp

#### Description:

This repository contains a small web application developed using Flask, Python, and TensorFlow. The web app allows users to interact with a machine learning model that predicts whether a page is being flipped or not.

#### Usage

1. Clone this repository: `git clone https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection.git`
2. Navigate to the project directory: `cd WebApp`
3. Download the pretrained models manually from [here](https://github.com/CorentinJ/Real-Time-Voice-Cloning/wiki/Pretrained-models)
4. Install the required packages: `pip install -r requirements.txt`
5. Run the Flask app: `python app.py`
6. Open your web browser and visit `http://localhost:5000`
![Screenshot 2023-11-03 225959](https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection/assets/47222625/a59e4cde-c7d6-4324-a4db-a2392344cfd2)
7. Browse the audio recording that you would like to create a clone of and type the text you want to create the audio
![Screenshot 2023-11-03 230244](https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection/assets/47222625/7e7815e7-7bcc-49d2-84f5-1853fcb8317e)
![Screenshot 2023-11-03 230413](https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection/assets/47222625/262e9826-fdd5-4fdf-b848-5dd739e22e46)
8. For the fake audio detection, upload a recording to detect whether the audio file is synthetic or not.
![Screenshot 2023-11-03 230609](https://github.com/sudarshanng7/Voice-Cloning-and-Fake-Audio-Detection/assets/47222625/34cd0364-4acd-4394-845a-a28a683a635a)

9. Additionally, a Dockerfile has been added to create a containerized version of the webapp. Build the image using `docker build -f Dockerfile -t imagename`
10. Deploy the application using `docker run -p 5000:5000 imagename`
11. Open your web browser and visit `http://localhost:5000`


#### Technologies Used

- Python
- Flask
- TensorFlow
