# Deploy Trade Journal Pro on AWS EC2

These commands host the app by pulling it from GitHub and running it with Node.js.

## 1. Create the EC2 server

- Launch an Ubuntu EC2 instance.
- In the security group, allow inbound:
  - SSH: port `22` from your IP
  - HTTP app: port `3000` from anywhere, or from your IP while testing

## 2. Connect to the server

```bash
ssh -i /path/to/your-key.pem ubuntu@YOUR_EC2_PUBLIC_IP
```

## 3. Install Node.js and Git

```bash
sudo apt update
sudo apt install -y git curl
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node -v
npm -v
```

## 4. Pull the app from GitHub

```bash
git clone -b codex/improve-trade-journal https://github.com/blackeyy22/traderaj.git
cd traderaj
npm start
```

Open:

```text
http://YOUR_EC2_PUBLIC_IP:3000
```

## 5. Keep it running with PM2

```bash
sudo npm install -g pm2
pm2 start server.js --name trade-journal-pro
pm2 save
pm2 startup
```

After `pm2 startup`, run the command it prints.

## 6. Update later from GitHub

```bash
cd ~/traderaj
git pull
pm2 restart trade-journal-pro
```

## Optional: Use port 80

For a cleaner URL without `:3000`, install Nginx and proxy port `80` to Node on `3000`.

```bash
sudo apt install -y nginx
sudo tee /etc/nginx/sites-available/trade-journal-pro >/dev/null <<'EOF'
server {
    listen 80;
    server_name _;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
EOF
sudo ln -sf /etc/nginx/sites-available/trade-journal-pro /etc/nginx/sites-enabled/trade-journal-pro
sudo nginx -t
sudo systemctl reload nginx
```

Then open:

```text
http://YOUR_EC2_PUBLIC_IP
```
