# Telegram의 BotFather를 이용해 Chatbot 만들기

## 구조

User <--> Telegram Server <--> ngrok <--> Flask



## Telegram

1. Telegram Bot Token
   - 검색 > BotFather > /newbot > token 발급
   - [Telegram Bot API](https://core.telegram.org/bots/api) 
2. sendMessage
   - `https://api.telegram.org/bot<token>/sendMessage?chat_id=<chat_id>&text=<text>`  
3. setWebhook
   - `https://api.telegram.org/bot{token}/setWebhook?url=<ngrok_url>`



## ngrok

1. [ngrok download](https://ngrok.com/download)
2. working directory로 `ngrok.exe` 이동 
3. bash에서 `./ngrok.exe https 5000` 
   - 외부(telegram)에서 로컬(Flask)에 접근할 수 있는 url 제공
   - forwarding한 주소(`https://example.ngrok.io`)를 telegram bot에게 setWebhook의 url 파라미터로 제공

4. 생성된 url을 실행(url 클릭)하면 webhook이 설정된다.

## Flask

### app.py



```python
from flask import Flask, response, request
import requests

@app.route('/')
def home(): # / 로 들어오면 해당 메소드 실행
    return "Hello Flask" 

@app.route('/telegram', methods=['POST'])
def telegram():
    sender_id = request.get_json().get('message').get('from').get('id')
    sender_msg = request.get_json().get('message').get('text')

    token = 'my_token'
    url = f'https://api.telegram.org/bot{token}/sendMessage'
    
    text = 'text_by_bot'
    msg_url = f'{url}?chat_id={sender_id}&text={text}'
    
    requests.get(msg_url)
    return '', 200
    
    
if __name__ == '__main__':
    app.run(debug=True)

# debug on 실행하려면
# FLASK_ENV=development flask run
```



1. flask 패키지에서 가져온 request 모듈은 flask가 받은 내용을 담고있다. 이를 json 형태로 변환해 원하는 정보를 가져올 수 있다.

2. requests 모듈에 url을 담으면 해당 url이 실행된다. 위 코드에선 url 실행시 봇이 메세지를 보낸다. 

3. `@app.route`는 입력받는 url에 따라 어떤 행동을 하는지 control한다.

4. bash에서 flask를 실행시키면 (`flask run` 또는 `FLASK_ENV=development flask run`) 프로그램 준비 완료