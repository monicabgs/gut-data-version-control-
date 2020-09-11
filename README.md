# gut-data-version-control- <br></p>

The objective of this project is used dvc in ML and Data Science projects. For this we'll store large files in MinIO.<br></p>

## MinIO's Setup <br></p>

**1.Install Go**
sudo passwd root<br></p>
su root<br></p>

apt update<br></p>
wget -c https://dl.google.com/go/go1.14.2.linux-amd64.tar.gz<br></p>
tar xvf go1.14.2.linux-amd64.tar.gz<br></p>
sudo chown -R root:root ./go<br></p>
sudo mv go /usr/local<br></p>
sudo echo 'export PATH=$PATH:/usr/local/go/bin' >> /etc/profile<br></p>
source /etc/profile<br></p>
rm go1.14.2.linux-amd64.tar.gz<br></p>
go version<br></p>

**2. Install MinIO server** <br></p>
cd ~<br></p>
wget https://dl.min.io/server/minio/release/linux-amd64/minio<br></p>

sudo useradd --system minio --shell /sbin/nologin<br></p>
sudo usermod -L minio<br></p>
sudo chage -E0 minio<br></p>

sudo mv minio /usr/local/bin<br></p>
sudo chmod +x /usr/local/bin/minio<br></p>
sudo chown minio:minio /usr/local/bin/minio<br></p>

sudo touch /etc/default/minio<br></p>
sudo echo 'MINIO_ACCESS_KEY="minio"' >> /etc/default/minio<br></p>
sudo echo 'MINIO_VOLUMES="/usr/local/share/minio/"' >> /etc/default/minio<br></p>
sudo echo 'MINIO_OPTS="-C /etc/minio --address :9000"' >> /etc/default/minio<br></p>
sudo echo 'MINIO_SECRET_KEY="gutteste"' >> /etc/default/minio<br></p>

sudo mkdir /usr/local/share/minio<br></p>
sudo mkdir /etc/minio<br></p>

sudo chown minio:minio /usr/local/share/minio<br></p>
sudo chown minio:minio /etc/minio<br></p>

**3. Install initialize script Minio's systemd**<br></p>
cd ~<br></p>
wget https://raw.githubusercontent.com/minio/minio-service/master/linux-systemd/minio.service<br></p>
vim minio_service<br></p>
[delete all and paste the code available in **/setup/minio_service.py**]<br></p>

sudo mv minio.service /etc/systemd/system<br></p>

**4.Firewall's  configuration**<br></p>
cd ~<br></p>
sudo ufw default deny incoming<br></p>
sudo ufw default allow outgoing<br></p>
sudo ufw allow ssh<br></p>
sudo ufw allow 9000<br></p>
sudo ufw allow http<br></p>
sudo ufw allow https<br></p>
sudo ufw enable<br></p>
sudo ufw allow 80<br></p>
sudo ufw allow 443<br></p>
sudo ufw status verbose<br></p>


**5. Start Minio's service**<br></p>
cd ~ 
sudo systemctl daemon-reload<br></p>
sudo systemctl enable minio<br></p>
sudo systemctl start minio<br></p>
sudo systemctl status minio<br></p>
q (to quit)

**6. Install Minio Customer (MC)**<br></p>
cd ~<br></p>
wget https://dl.min.io/client/mc/release/linux-amd64/mc<br></p>
chmod +x mc<br></p>

**7. Generates TLS certificates to Minio service (http - https)**<br></p>
cd ~<br></p>
sudo apt instalar software -properties-common<br></p>
sudo add-apt-repository universe<br></p> 
sudo apt update<br></p>
sudo apt install certbot<br></p>
sudo certbot certonly --standalone -d minio.your_domain<br></p> 
sudo cp /etc/letsencrypt/live/minio.your_domain_name/privkey.pem /etc/minio/certs/<br></p>
private.key sudo cp /etc/letsencrypt/live/minio.your_domain_name/fullchain.pem /etc/minio/certs/public.crt<br></p>
sudo chown minio: minio /etc/minio/certs/private.key<br></p>
sudo chown minio: minio /etc/minio/certs/public.crt<br></p> 
sudo systemctl restart minio<br></p>
