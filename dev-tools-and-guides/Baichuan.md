# <center>Baichuan</center>

## Download & Resources
[Official Website |](https://www.baichuan-ai.com/home#introduce)
[Documentation |](https://platform.baichuan-ai.com/docs/api#4)

## Introduction
Baichuan is a large language model platform providing APIs for conversational AI and other NLP tasks. Below is a Python example for using the Baichuan API.

## Python Usage
```py
import requests
from dotenv import dotenv_values
import json
import time
import hashlib
import sys

url = "https://api.baichuan-ai.com/v1/chat"
env_vars = dotenv_values('.env')
baichuan_api_key = env_vars['BAICHUAN_API_KEY']
baichuan_secret_key = env_vars['BAICHUAN_SECRET_KEY']

def calculate_md5(input_string):
    # Calculate MD5 hash of the input string
    md5 = hashlib.md5()
    md5.update(input_string.encode('utf-8'))
    encrypted = md5.hexdigest()
    return encrypted

def baichuanChat(query):
    # Send a chat request to the Baichuan API
    print("Response header:")
    data = {
        "model": "Baichuan2-53B",
        "messages": query
    }

    json_data = json.dumps(data)
    time_stamp = int(time.time())
    signature = calculate_md5(baichuan_secret_key + json_data + str(time_stamp))

    headers = {
        "Content-Type": "application/json",
        "Authorization": "Bearer " + baichuan_api_key,
        "X-BC-Request-Id": "your requestId",
        "X-BC-Timestamp": str(time_stamp),
        "X-BC-Signature": signature,
        "X-BC-Sign-Algo": "MD5",
    }

    response = requests.post(url, data=json_data, headers=headers)

    if response.status_code == 200:
        # Parse the response data
        result = json.loads(response.text)
        # Return the chat response
        return result["data"]["messages"][0]["content"]
    else:
        # Handle request failure
        return 'error', 500