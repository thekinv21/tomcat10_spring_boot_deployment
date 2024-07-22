
## Java Spring Boot Deployment ( VPS Ubuntu 20.04 )

 - [Gürkan Uçar Deploy Example](https://medium.com/@gurkanucar/tomcat10-spring-boot-3-java-17-deployment-on-ubuntu-server-a3420c4f431f)

 - [How to uninstall Tomcat from VPS Ubuntu](https://webhostinggeeks.com/howto/how-to-uninstall-apache-tomcat-on-ubuntu/)

 - [How to uninstall Java JDK from VPS Ubuntu](https://webhostinggeeks.com/howto/how-to-uninstall-java-jdk-on-ubuntu/)


 
## NOT
    
Bu işlemleri yapmadan önce elinizde bir `VPS Ubuntu 20.04` sunucusu ve `DNS adresiniz` olması lazım onun nasıl alınacağı ve kurulacağına dair bilgiler aşağıdaki repository README.md dosyalarında adım adım verilmiştir

 - [ReactJS Deployment](https://github.com/thekinv21/reactjs-deployment)

 - [NextJS Deployment](https://github.com/thekinv21/nextjs-portfolio-deployment)

 - [PostgreSql Deployment](https://github.com/thekinv21/postgresql_deployment)


 ## 1.ADIM

 - Vps ubuntu sunucusuna giriş yapalım:

```
ssh root@<IP_ADDRESS>
```

 -  Giriş yaptıktan sonra aşağıdaki 2 komutu çalıştıyoruz:

```
sudo ufw allow ssh
```

```
sudo apt update
```

## 2.ADIM ( Java 17 Kurulumu )

   - 1-2.komut

``` 
sudo apt-get install openjdk-17-jdk
```

``` 
sudo update-java-alternatives --list
```
![Ekran Resmi 2024-07-22 00 51 27](https://github.com/user-attachments/assets/7dbd5d49-59d7-4a71-9762-961246f72bf2)

##

   - 3-4.komut

   ``` 
sudo update-java-alternatives --set java-1.17.0-openjdk-amd64
   ```

   ``` 
java -version
   ```

![Ekran Resmi 2024-07-22 00 52 09](https://github.com/user-attachments/assets/c0f0c45e-836b-4d15-951d-fa6e2065a1f7)


##


   - 5-6.komut

   ``` 
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
   ```

   ``` 
echo $JAVA_HOME
   ```

![Ekran Resmi 2024-07-22 00 52 39](https://github.com/user-attachments/assets/24a551b8-ba06-45a4-b3eb-224e78133b86)




##


## 3.ADIM ( Tomcat10 Kurulumu ve Yetkilendirilmesi )



   - 1-2.komut

   ``` 
sudo groupadd tomcat
   ```

   ``` 
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

   ```


   ``` 
cd /tmp
   ```

   - Burada Tomcat Versiyonları çok hızlı değişebilmektedir onu kontrol ederek indirmelisiniz ve   onu kontrol etmek için ise tomcatin ana sayfasına gidip hangi versiyonu indirmek istiyorsanız o versiyona dair güncell paketi almalısınız

   - Ben bu işlemi yaparken güncel versiyonu `tomcat-10.1.26'dır`

   ``` 
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz
   ```
![Ekran Resmi 2024-07-22 00 55 21](https://github.com/user-attachments/assets/236d6a4e-ef8a-4fe2-b415-25f0af298417)




##


   - Opt dosyası içinde yeni `tomcat` dosyası oluştur
     
 ``` 
sudo mkdir /opt/tomcat
```

``` 
cd /opt/tomcat
```


 - Bu dosya içinde ise Yetkileri ekle

``` 
 sudo tar xzvf /tmp/apache-tomcat-10.*tar.gz -C /opt/tomcat --strip-components=1
```

```
sudo chown -R tomcat:tomcat /opt/tomcat
```

``` 
sudo chmod -R u+x /opt/tomcat/bin
```

``` 
sudo chown -R tomcat webapps/ work/ temp/ logs/
```


![Ekran Resmi 2024-07-22 00 56 20](https://github.com/user-attachments/assets/542ce1ca-cf1f-4cb0-8069-ad5372b34b7e)


##


## 4.ADIM ( JAVA 17 Set edilmesi )

 - Aşağıdaki komutu çalıştırdıktan sonra bir konfigurasiyon penceresi açılacaktır
   oraya da 2.komutu kopyalayıp eklemelisiniz

```
sudo nano /etc/systemd/system/tomcat.service
```

   - Konfigurasiyon dosyasına eklenecek

```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking
WorkingDirectory=/opt/tomcat/webapps

Environment=JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment=CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC
Environment=JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/v/urandom

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

```

![Ekran Resmi 2024-07-22 00 57 21](https://github.com/user-attachments/assets/a629ee20-98de-4d2e-90eb-f19d4a59c514)




## Tomcat server başlatılması:
 
 * Tomcat reload edilir:

```
sudo systemctl daemon-reload
```

* Tomcat start edilir:

```
sudo systemctl start tomcat
```

* Tomcat durumu kontrol edilir:

```
sudo systemctl status tomcat
```

 * Tomcat aktif mi değil kontrol edilir
```
sudo systemctl enable tomcat
```


 * Tomcat 8080 portuna set edilir

```
sudo ufw allow 8080
```

##

## Bu işlemleri tamamladıktan sonra :

   - Eğer bu işlemi başarıyla tamamladıysanız `IP_ADDRESS:8080` adresine  browserdan gittiğinizde tomcat ana ekranını görmeniz gerekmektedir



![Ekran Resmi 2024-07-22 00 56 20]( https://miro.medium.com/v2/resize:fit:4800/format:webp/0*1QbCFkp6t6dtNN06.png)


   - Eğer bu ekranı görüyorsanız tomcat çalışıyor demektir, değilse adımları dikkatlice takip ederek yapmalısınız ::)
  
##


## 5.ADIM ( Tomcat'i yönetmek için Kullanıcı ve Rollerin Eklenmesi )

 - Aşağıdaki 1.komutu çalıştırdığınızda karşınıza tomcatin `user` ve `role` konfigurasiyonu çıkacaktır o konfigurasiyon içine `2.komutu` kopyalayıp eklemelisiniz

 - 1.komut

```
sudo nano /opt/tomcat/conf/tomcat-users.xml
```


 - 2.komut

```
<role rolename="tomcat"/>
<role rolename="admin-gui"/>
<role rolename="manager"/>
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="admin" password="pass123" roles="tomcat,admin-gui,manager,manager-gui,manager-script,manager-jmx,manager-status"/>
```


![Ekran Resmi 2024-07-22 01 00 05](https://github.com/user-attachments/assets/a615cb84-01e2-4b0a-8888-386e7698eef2)




## Şimdi her biri için Tomcat'e uzaktan erişime izin verin

- Aşağıdaki komutları yazdığınızda karşınıza konfigurasiyon dosyası çıkacaktır o konfigurasiyon dosyası içinde bulunan <Value> tagını yorum satırı yapmalısınız ikisinde de bunu yapma sebebimiz de tomcate uzaktan erişimi sağlamak içindir..

```
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
```

```
sudo nano /opt/tomcat/webapps/manager/META-INF/context.xml
```

![Ekran Resmi 2024-07-22 01 01 10](https://github.com/user-attachments/assets/a7b54e10-2184-473c-9111-998956fca24c)





###  Tomcat Restart Edilmesi

```
sudo systemctl daemon-reload
```

```
sudo systemctl restart tomcat
```



## 6.ADIM ( Tomcat Manager'de Spring Boot Application deploy etme )


![Ekran Resmi 2024-07-22 01 01 10](https://miro.medium.com/v2/resize:fit:4800/format:webp/0*FY1XoMCrmP5WIUgZ.png)

 
##



# - DIKKAT EDİLMESİ GEREKEN ÖNEMLİ YERLER



### 1 ) pom.xml içinde packaging kısmı `WAR` olmalıdır

   
   ```
<packaging>war</packaging>
```
   


##


### 2 ) Spring Boot uygulamamızı dağıtmak için SpringBootServletInitializer'ı genişletmemiz gerekiyor. Spring Boot'ta genişletmek, uygulamanın harici servlet kapsayıcılarında bir WAR olarak konuşlandırılmasını sağlar


```
@SpringBootApplication
public class SpringBootDemoApplication extends SpringBootServletInitializer {

  public static void main(String[] args) {
    SpringApplication.run(SpringBootDemoApplication.class, args);
  }
}
```

- Eğer yukarıdaki class uygulamanızda yok ise oluşturulmalısınız

##


### 3 ) pom.xml içindeki build tagı altına finalName eklenmeli

   - Spring Boot uygulamanın Tomcat managerde hangi ad ile görüntüleneceğini belirlemek için

   
   ```
<finalName>deployment-be</finalName>
```



### 4 ) Son olarak

   - Aşağıdaki 1.komutu çalıştırıp build almalıyız eğer aşağıdaki 1.komut hata veriyor ise 2.komutu kullanmalısınız

   1.komut 

   ```
mvn clean package
```

   2.komut 
   
   ```
mvn clean package -DskipTests=true
```



## 


- Eğer komut başarılı bir şekilde çalıştı ise `target` klasörü altında sizin `pom.xml` içinde `finalName` tagı altında vermiş olduğunuz isim ile aynı bir isimde `.war` dosyası oluşmuş olacaktır





- {finalName}.war  dosyasını tomcat manager'e upload etmeniz gerekmektedir

## 


![Ekran Resmi 2024-07-22 01 01 10](https://miro.medium.com/v2/resize:fit:640/format:webp/0*6NZwWhtWwRz0cQK_.png )

## 


### Bu dosya boyutu çok büyük olabiliyor ve upload edemeyebilirsiniz bazen bu durumda ne yapmalıyız?

#### 1.Yöntem:

   - VPS Ubuntu kısmında Tomcat için verilen MB arttırma yapabiliriz, default olarak tomcat için 50MB verilmiştir bunu daha da artırabilirsiniz

#### 2.Yöntem

   - SCP kullanarak deploy etmeyi deneyebilirsiniz



#### Örnek:

   - Ben 2.Yöntemi kullanarak deploy ettim izlediğim adımlar sırasıyla aşağıda verilmiştir


##


## 2.yöntemin Uygulanması ( IntelliJ IDEA terminalinde )

 - ilk olarak projeyi build edelim:

 ```
 mvn clean package -DskipTests=true
```


 - target altındaki {{finalName}}.war dosyasını ana dizine taşıyalım:

 ```
mv ./target/{{finalName}}.war .  
```

![Ekran Resmi 2024-07-22 01 17 43](https://github.com/user-attachments/assets/8d422139-3d39-458d-8202-f76fe5f4c7c5)




 - SCP ile deploy etmeye çalışalım:

```
scp {{finalName}}.war VPS_SERVER_USERNAME@IP_ADDRESS:/opt/tomcat/webapps

```
![Ekran Resmi 2024-07-22 01 30 44](https://github.com/user-attachments/assets/863f40dd-5558-4d14-8179-7cbf58dbc0a8)


- Bu işlem başarılı tamamlandıktan sonra tomcat manager'de aşağıdaki gibi uygulamanızı görebilrisiniz

![Ekran Resmi 2024-07-22 01 35 27](https://github.com/user-attachments/assets/a6f5dd71-b66b-4f24-a930-278f64de7902)




##

## EKSTRA İŞLEMLER
- Eğer yukarıdaki komut access denied hatası verdi ise

 - Burada bazen yetki vermemiş olabilirisiniz bu durumda size IntelliJ IDEA terminali access denied hatası verebilir bu durumda siz `VPS Ubuntu` sunucunuza girip `/opt/*` altındaki bütün dosyalara Yetki vermeniz gerekebilir onu da aşağıdaki komutlar ile yapabiliriz



```
cd /opt/tomcat/

```


```
sudo chmod -R 777  webapps/ work/ temp/ logs/
```

### TOMCAT RELOAD EDİLİR VE YUKARIDAKİ KOMUT TEKRAR DENENİR

```
sudo systemctl daemon-reload
```

```
sudo systemctl restart tomcat
```












