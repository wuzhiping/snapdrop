<pre>

1 snapdrop:
    image: "node:lts-alpine"
    user: "node"
    working_dir: /home/node/app
    volumes:
      - ./snapdrop/server/:/home/node/app
    command: ash -c "npm i && node index.js"

2 ngx:

    ... 

    links:
      - snapdrop
    volumes:
      - ./snapdrop/client:/home/airdrop


3 default.conf

  location /airdrop {
      alias   /home/airdrop;
      index  index.html index.htm;
  }

  location /airdrop/server {
     proxy_connect_timeout 300;
     proxy_pass http://snapdrop:3000;
     proxy_set_header Connection "upgrade";
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header X-Forwarded-for $remote_addr;
  }


</pre>
