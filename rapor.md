### 1. Alan Adı Kaydı
- METUnic üzerinden kişisel formatta (neslihanbukte.name.tr) bir alan adı kaydı yapıldı.

### 2. AWS Free Tier Sunucu Kurulumu
- AWS Console üzerinden Free Tier kapsamında **t2.micro** tipinde bir sunucu oluşturuldu.

**Makine Tipi :** t2.micro

**İşletim Sistemi :** Amazon Linux

**Region :** eu-central-1 

- Key Pair oluşturuldu.
- Security Group ayarları yapıldı. : 
**SSH, HTTP, HTTPS**

### 3. DNS ve Web Sunucusu Yapılandırması
- **METUnic DNS** yönetim paneli üzerinden aşağıdaki kayıtlar oluşturuldu:  
  - **A Kaydı**: `neslihanbukte.name.tr` → `<EC2-PUBLIC-IP>`  
  - **CNAME Kaydı**: `www.neslihanbukte.name.tr` → `neslihanbukte.name.tr`  
- Sunucuda web sunucusu kurulumu gerçekleştirildi.
 ```bash
     sudo yum install -y nginx  
     sudo systemctl enable nginx
     sudo systemctl start nginx
```
- Site için yeni bir **server block** dosyası oluşturuldu:
     `/etc/nginx/conf.d/site.conf`  

     İçeriği aşağıdaki şekilde ayarlandı (Certbot tarafından yönetilen SSL ayarları dahil):  
     ```nginx
     server {
         server_name neslihanbukte.name.tr www.neslihanbukte.name.tr;

         root /usr/share/nginx/html;
         index index.html;

         listen 443 ssl; # managed by Certbot
         ssl_certificate /etc/letsencrypt/live/neslihanbukte.name.tr/fullchain.pem; # managed by Certbot
         ssl_certificate_key /etc/letsencrypt/live/neslihanbukte.name.tr/privkey.pem; # managed by Certbot
         include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
         ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
     }

     server {
         if ($host = www.neslihanbukte.name.tr) {
             return 301 https://$host$request_uri;
         }
         if ($host = neslihanbukte.name.tr) {
             return 301 https://$host$request_uri;
         }

         listen 80;
         server_name neslihanbukte.name.tr www.neslihanbukte.name.tr;
         return 404; # managed by Certbot
     }
     ```

- Web içeriği `/usr/share/nginx/html/index.html` dizinine yerleştirildi.
```bash
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Neslihan Bükte</title>
</head>
<body>
  <h1>Neslihan Bükte</h1>
</body>
</html>
```
- Yapılandırma test edildi ve nginx teniden başlatıldı:
```bash
sudo systemctl restart nginx
```

- HTTPS için ssl yapılandırması yapıldı.
**certbot** aracı ile Let's Encrypt üzerinden ücretsiz SSL sertifikası alındı.
```bash
sudo yum install -y certbot python2-certbot-nginx
sudo certbot --nginx -d neslihanbukte.name.tr -d www.neslihanbukte name.tr
```
- Alan adı neslihanbukte.name.tr üzerinden yayına alındı. 

### 4. SSH Erişim Yetkisi 
- Sunucuya erişim için verilen public SSH anahtarı yetkilendirildi.
- Bu anahtar ilgili kullanıcının `~/.ssh/authorized_keys` dosyasına eklendi.



