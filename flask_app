from flask import Flask, render_template
import requests
import json

app = Flask(__name__)


@app.route('/')
def index():
    weather_data = get_weather_data()
    nasa_apod_data = get_nasa_apod_data()

    latitude = '28.393480'
    longitude = '-80.598790'
    wave_data = get_wave_data(latitude, longitude)

    return render_template('index.html', weather_data=weather_data, apod_data=nasa_apod_data, wave_data=wave_data)

def get_weather_data():
    api_key = ''
    latitude = '28.393480'
    longitude = '-80.598790'
    url = f'http://api.weatherapi.com/v1/current.json?key={api_key}&q={latitude},{longitude}&aqi=no&units=imperial'
    response = requests.get(url)
    return response.json()


def get_nasa_apod_data():
    api_key = ''
    url = f'https://api.nasa.gov/planetary/apod?api_key={api_key}'
    response = requests.get(url)
    return response.json()

def get_wave_data(latitude, longitude):
    api_key = ''
    url = f'https://api.stormglass.io/v2/weather/point?lat={latitude}&lng={longitude}&params=waveHeight,waveDirection,wavePeriod&source=sg'
    headers = {'Authorization': api_key}
    response = requests.get(url, headers=headers)
    data = response.json()

    if 'hours' not in data or len(data['hours']) == 0:
        return None

    hour_data = data['hours'][0]

    for key in ['waveHeight', 'waveDirection', 'wavePeriod']:
        if key not in hour_data or 'sg' not in hour_data[key]:
            return None

    return hour_data


def get_live_video_id():
    api_key = ''
    video_id = ''

    url = f'https://www.googleapis.com/youtube/v3/search?part=snippet&channelId={video_id}&type=video&eventType=live&key={api_key}'
    response = requests.get(url)


    try:
        return response.json()['items'][0]['id']['videoId']
    except IndexError:
        return None



if __name__ == '__main__':
    app.run(debug=True)
