# INSTALLATION FrontEnd
sudo apt update
sudo apt upgrade
sudo apt install git
cd /home/aria
git clone https://github.com/LlamasScripters/TerraFlora.git
cd TerraFlora
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
nvm install --lts
cd client/
npm install
VITE_API_URL=api.terraflora.store VITE_STRIPE_PUBLISHABLE_KEY=pk_test_51PT1o1RvflFVG7kRTxwFUX1OYaloXE98AQYjAAi1Ij6u5u0nldTu18K8cYjd0o6Ca8Shk4xRh51Ei3ApltZxvE6P0060u2VL4H npm run build

# NGINX
sudo apt update
sudo apt upgrade
sudo apt install nginx
cd /
cd /etc/nginx/sites-available
sudo nano terraflora.store.conf 
sudo ln -s /etc/nginx/sites-available/terraflora.store.conf /etc/nginx/sites-enabled/terraflora.store.conf
sudo nginx -t
sudo systemctl reload nginx

# CERTBOT
sudo apt update
sudo apt upgrade
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx
sudo nginx -t
sudo systemctl reload nginx
crontab -e
0 1 1 * * /usr/bin/certbot renew --quiet

# SAMBA

sudo apt install samba
sudo mkdir -p /srv/samba/share
sudo chown aria:aria /srv/samba/share/
sudo nano /etc/samba/smb.conf

ajout config 
[share]
path = /srv/samba/share
browseable = yes
writable = yes
guest ok = no

sudo smbpasswd -a aria
sudo systemctl reload smbd
sudo systemctl reload nmbd

# Fail2Ban

sudo apt install fail2ban
cd /etc/fail2ban/
nano jail.local
sudo cp jail.conf jail.local
sudo nano jail.local

cd filter.d
sudo nano nginx-403.conf

# CONFIGURATION SSH

adduser aria
usermod -aG sudo aria
su aria
mkdir ~/.ssh
nano .ssh/authorized_keys
sudo nano /etc/ssh/sshd_config
Port 2002
PermitRootLogin no
PasswordAuthentication no


