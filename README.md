# Snap & Ask: Multi-Modal AI Assistant

A web app that lets you talk to an image. Upload a JPG or PNG, get a caption, ask questions by typing or speaking, read the text inside the image with OCR, and see the sentiment of that text. Built for the NLP course (202047809) at G H Patel College of Engineering and Technology.

## Features

- **Image captioning:** a sentence describing the uploaded image
- **Object keywords:** objects are found by matching a keyword list against the caption (there is no object detector)
- **Question answering:** ask questions about the image in a chat; follow-ups keep the chat history
- **OCR:** read printed text from the image, with a Copy text button
- **Sentiment:** Positive, Negative or Neutral with a score from -1 to +1 for the extracted text
- **Voice:** speak your question and optionally hear the answer read aloud
- **Accounts and history:** sign up and log in, reopen or delete past images and their chats

## How it works

1. The user logs in and uploads an image (max 10 MB), which is resized.
2. BLIP writes a caption, and objects are matched from it.
3. A question goes to `reasoning.py`, which answers from the caption or asks the BLIP VQA model about the image.
4. "Read text" runs EasyOCR, then VADER scores the extracted text.
5. Images, chats and OCR text are saved in SQLite.

## Models (all pretrained, no training done)

| Task | Model |
|---|---|
| Captioning | `Salesforce/blip-image-captioning-base` (Hugging Face) |
| Visual question answering | `Salesforce/blip-vqa-base` (Hugging Face) |
| OCR | EasyOCR |
| Sentiment | NLTK VADER (rule-based) |
| Voice | Browser Web Speech API |

## Tech stack

Python, FastAPI, Uvicorn, PyTorch, Hugging Face Transformers, EasyOCR, NLTK, SQLite, HTML, CSS, JavaScript

## Project structure

```
server.py          FastAPI backend and routes
image_analyzer.py  BLIP captioning, object keywords and VQA
reasoning.py       question routing
ocr.py             EasyOCR text extraction
sentiment.py       VADER sentiment
db.py              SQLite users, images and chats
static/index.html  web interface
```

## Setup

```bash
pip install -r requirements-web.txt
python server.py
```

Open http://127.0.0.1:8000

The first run downloads the BLIP models, and the first "Read text" click downloads the OCR model, so the first start needs internet. After that, the models run locally and images are not sent anywhere.

## Usage

1. Create an account and log in.
2. Upload an image.
3. Ask a question in the chat, or click the mic.
4. Click **Read text** to extract the text and see its sentiment.

## Limitations

- OCR is slow without a GPU
- VQA answers can be short
- Voice recognition needs Chrome or Edge with internet
- Sentiment works on English text only
- One image per session, no bounding boxes

## Future scope

Bounding boxes with a real object detector, streaming answers, a stronger vision model, multiple images per session, and online deployment with HTTPS.

## Team

- Hiva Shah (12302130501025)
- Khushi Parekh (12302130501029)

Guide: Prof. Adit Vyas

## References

- Li et al., "BLIP: Bootstrapping Language-Image Pre-training", ICML 2022
- Antol et al., "VQA: Visual Question Answering", ICCV 2015
- Hutto and Gilbert, "VADER", ICWSM 2014
