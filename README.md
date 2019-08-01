# nextcloud-docker

1. Install certbot and docker
    ```
    sudo apt-get update
    sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
    sudo add-apt-repository universe
    sudo add-apt-repository ppa:certbot/certbot
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo apt-key fingerprint 0EBFCD88
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io certbot
    sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
    sudo chmod +x /usr/bin/docker-compose
    sudo usermod -aG docker $USER
   ```

4. Setup nginx
  Generate dhparams
  ```
  sudo openssl dhparam -out ./ssl/dhparams.pem 2048
  ```

  Using following snippet for `conf.d/default.conf`
  ```
  server {
    listen 80 default_server;
    server_name _;
    root /var/www/html;
  }
  ```
  
  Start nginx `docker-compose up -d nginx`
  
  Execute create letsencrypt cert (change your domain name)
  ```
  sudo certbot certonly -w /usr/share/nginx/html -d doc.example.com
  ```

  Revert `conf.d/default.conf`, change your domain name and restart nginx with `docker-compose restart nginx`
