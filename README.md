from http.server import BaseHTTPRequestHandler
import requests
import json

TOKEN = "8260559145:AAEnOQUcjl3vh_HvH1yIJwSffSWAZIASSsY"

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b'Bot is running!')
    
    def do_POST(self):
        try:
            content_length = int(self.headers['Content-Length'])
            post_data = self.rfile.read(content_length)
            update = json.loads(post_data)
            
            chat_id = update['message']['chat']['id']
            text = update['message'].get('text', '')
            
            if text == '/start':
                requests.post(
                    f'https://api.telegram.org/bot{TOKEN}/sendMessage',
                    json={
                        'chat_id': chat_id, 
                        'text': '🎉 آفرین! ربات finalmente کار می‌کنه! 🎉\n\nدستور /start رو فرستادی!'
                    }
                )
            
            self.send_response(200)
            self.end_headers()
        except Exception as e:
            self.send_response(200)
            self.end_headers()
