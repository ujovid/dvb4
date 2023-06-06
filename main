import json
import os

import pyaudio
import pyttsx3
import requests
import vosk
import webbrowser

tts = pyttsx3.init()

voices = tts.getProperty('voices')
tts.setProperty('voices', 'en')

for voice in voices:
    print(voice.name)
    if voice.name == 'Microsoft Zira Desktop - English (United States)':
        tts.setProperty('voice', voice.id)
model = vosk.Model('vosk-model-small-en-us-0.15')
record = vosk.KaldiRecognizer(model, 16000)
pa = pyaudio.PyAudio()
stream = pa.open(format=pyaudio.paInt16,
                 channels=1,
                 rate=16000,
                 input=True,
                 frames_per_buffer=8000)
stream.start_stream()


def listen():
    while True:
        data = stream.read(4000, exception_on_overflow=False)
        if record.AcceptWaveform(data) and len(data) > 0:
            answer = json.loads(record.Result())
            if answer['text']:
                yield answer['text']


def speak(say: str):
    tts.say(say)
    tts.runAndWait()


def weather():
    url = "https://api.openweathermap.org/data/2.5/weather?lang=en&units=metric&q=Санкт-Петербург&appid=aa710e11052f210bd165425d720d4a4f"
    response = requests.get(url)
    weather_data = response.json()
    cityname = weather_data['name']
    temp = str(weather_data['main']['temp'])
    feels = str(weather_data['main']['feels_like'])
    speed = str(weather_data['wind']['speed'])
    press = str(weather_data['main']['pressure'])
    desc = weather_data['weather'][0]['description']
    print(
        'City: ' + cityname + '\nTemperature: ' + temp + '\nFeels like: ' + feels + '\nWind velocity: ' + speed + '\nHeat: ' + press + '\nWeather: ' + desc)
    tts.say(
        'City: ' + cityname + '\nTemperature: ' + temp + '\nFeels like: ' + feels + '\nWind velocity: ' + speed + '\nHeat: ' + press + '\nWeather: ' + desc)
    tts.runAndWait()


print('start')
pwd = ''
for text in listen():
    if text in 'weather whether':
        weather()
    elif "calculator" in text:
        os.system('calc')
        # speak('which program to open?')
        # for text in listen():
        #     os.system(text)

    elif 'music' in text:
        webbrowser.open('https://www.youtube.com/watch?v=dQw4w9WgXcQ&ab_channel=RickAstley')
    else:
        print(text)
        speak(text)
