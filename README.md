- 👋 Hi, I’m @kjyyoung
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
kjyyoung/kjyyoung is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->


upstream django {
    server unix:/home/kjyyoung/codespaces-blank/django/mysite/uwsgi.sock;
}

server {
        listen 80 default_server;
        server_name autoread.duckdns.org;
        return 301 https://$server_name$request_uri;
}

server {
        listen 443 ssl http2 default_server;
        server_name autoread.duckdns.org;
        root /var/www/html;

        ssl_certificate /etc/letsencrypt/live/autoread.duckdns.org/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/autoread.duckdns.org/privkey.pem;
        ssl_trusted_certificate /etc/letsencrypt/live/autoread.duckdns.org/chain.pem;
        ssl_dhparam /etc/ssl/dhparam.pem;
        ssl_session_timeout 10m;
        ssl_session_cache shared:SSL:10m;
        ssl_session_tickets off;

        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers on;
        ssl_ciphers TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256:TLS_AES_128_GCM_SHA256:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES128-GCM-SHA256;
        ssl_ecdh_curve secp384r1;

        add_header Strict-Transport-Security max-age=31536000;
        add_header X-Content-Type-Options "nosniff" always;
        add_header X-Frame-Options "SAMEORIGIN" always;
        add_header X-XSS-Protection "1; mode=block" always;

        ssl_stapling on;
        ssl_stapling_verify on;

        index index.html index.nginx-debian.html;


        location /media  {
                alias /home/kjyyoung/codespaces-blank/django/mysite/media;
        }

        #6 Django static 디렉토리 경로
        location /static {
                alias /home/kjyyoung/codespaces-blank/django/mysite/static;
        }

        #7 media와 static을 제외한 모든 요청을 upstream으로 보냅니다.
        location / {
                #8 uwsgi_pass [upstream name] (위에 upstream으로 설정한 block의 이름)
                uwsgi_pass  django;
                #9 uwsgi_params의 경로
                include /home/kjyyoung/codespaces-blank/uwsgi_params;
        }

        #       location / {
        #       try_files $uri $uri/ =404;
        #}
}
