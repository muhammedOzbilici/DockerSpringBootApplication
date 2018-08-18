# Docker üzerinde basit bir Spring Boot örneği

Öncelikle Docker uygulamızı kuruyoruz

https://www.docker.com/products/docker-desktop

Linux versiyonu için ;

https://docs.docker.com/install/linux/docker-ce/ubuntu/

Spring Boot uygulamamızı oluşturuyoruz.
/rest/hello adresine yönlendirip ekrana "Hello Docker!" yazdıracağız.

Maven clean-install yapmadan önce , target klasörü altında oluşturulacak jar'ımıza isim veriyoruz
Bunun için pom.xml içerisinde, < build > tag'i içerisine şu satırı yazıyoruz.

`< finalName > docker-spring-boot </ finalName >`

Dockerfile dosyamızı oluşturuyoruz

![alt text](http://i.hizliresim.com/DDWZ0z.png)

Daha sonra, oluşturduğumuz Dockerfile dosyasına şunları yazacağız;
İlk olarak repository seçeceğiz ; 

https://hub.docker.com/ 

adresinden repository'leri görüntüleyebilirsiniz.
java repository şu anda deprecate olmuş durumda, yani artık kullanılmıyor, bu yüzden openjdk repository kullanacağız.

`FROM openjdk:8` 

target klasörü altındaki jar dosyamızı gösteriyoruz

`ADD target/docker-spring-boot.jar docker-spring-boot.jar`

port adresimizi veriyoruz

`EXPOSE 8086`

entrypoint veriyoruz, bunu main metodumuz gibi düşünebiliriz.

`ENTRYPOINT ["java","-jar","docker-spring-boot.jar"]`

Dockerfile hazır durumdadır. Terminal ekranımızı açıyoruz.(Intellij içerisindeki terminal ekranı üzerinden de yapabilirsiniz.)

`docker -v` 

yazarak, bilgisayarımızda docker uygulaması yüklü ise versiyonunu görebiliriz. Benim sistemde kurulu olan versiyon ;

>Docker version 18.06.0-ce, build 0ffa825

Docker image'imizi oluşturuyoruz.

`docker build -f Dockerfile -t docker-spring-boot .`

openjdk repository indirme işlemini yapacaktır. Oluşturulan image dosyalarını görüntülemek için >

`docker images`

yazıp entere bastığımızda, şu image'ler görüntülenecektir.

![alt text](http://i.hizliresim.com/oVqvom.png)


docker image dosyamızı çalıştırıyoruz

`docker run -p 8086:8086 docker-spring-boot`

web browser üzerinden şu adrese gittiğimizde, Hello Docker yazısını göreceğiz.

http://localhost:8086/rest/hello

yeni bir terminal açıp , aynı image'i , farklı bir port üzerinden de çalıştırabiliriz

`docker run -p 8087:8086 docker-spring-boot`

hem 8086 portu üzerinde hem de 8087 portu üzerinde uygulamamızın çalıştığını göreceksiniz.

https://resmim.net/f/ayPsRh.png
