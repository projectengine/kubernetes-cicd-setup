# Portlar

`docker run -p 8000:8000 jwilder/whoami` komutu ile ekrana hostname'ini yazan bir web uygulaması çalıştıralım. Uygulama 8000 portundan yayın yapıyor.

Tarayıcımzıdan [http://localhost:8000](http://localhost:8000) sayfasını açtığımızda `I'm 7b5c6dc7a66b` gibi bir metin görmekteyiz.

Bu şekilde `run` komutuna verdiğimiz `-p host-port:container-port` argümanı ile container içinde çalışan uygulamaya container'ın dışından erişebilmekteyiz.

!!! not

    `docker run` komutunda genelde bağlantılar `host:container` şeklinde verilir (ör. portlar, volume'ler)