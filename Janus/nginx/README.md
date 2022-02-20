# Sample Nginx Conf

 under `sites-enabled` you can find sample nginx conf for your secure domain to expose websocket and rest enpoints. you can use certbot to fetch ssl certificate for your domain and use nginx plugin to automate ssl certificate deployment and therefore using nginx proxy-pass you can deploy janus endpoints like `https://mydomain.com/rest` and `wss://mydomain.com/websocket`
