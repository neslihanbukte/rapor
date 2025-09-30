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

- Nginx yapılandırıldı. 
- Alan adı neslihanbukte.name.tr üzerinden yayına alındı. 


