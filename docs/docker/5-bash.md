# Bash

Bu başlıkta docker'ın en yaygın kullanım alanlarından biri olan geçici çalışma ortamlarını inceleyeceğiz.

Bir container'ın terminaline girmek için iki yol vardır. Eğer container çalışıyorsa `docker exec` komutu kullanılır.

Aşağıda buna örnek olarak bir `busybox` container'ı çalıştırıp `exec` ile terminaline giriyoruz:

``` sh
# busybox imajı ile bir container başlat.
# sleep komutu container'ın bir süre açık kalmasını sağlar.
# --rm ile işimiz bitince container'ın silinmesini sağlıyoruz
# --name ile kolay erişim için bir isim veriyoruz.
# -d ile container'ın arka planda açılmasını sağlıyoruz. Böylece arkasından başka bir komut yazabiliriz.
docker run --rm -d --name debug busybox sleep 60000

# exec ile container'ın terminaline gir.
docker exec -it debug sh

/ # cat /etc/hostname
9c47c8133eed
```

Bu yöntem ile çalışmakta olan container'ların terminaline giriş yapıp sorunlarını çözebilir, içinde değişiklik yapabilir ya da dosyalarına bakabiliriz. Container'ları debug etmek için genelde bu yöntem kullanılır

İkinci yöntem ise container'ı açarken terminal başlatmaktır. Bu şekilde genelde yukarıda bahsettiğim çalışma ortamlarını kullanabiliriz. Örnek olarak yine aynı imajı kullanalım:

``` sh
# -it argümanları terminal komutu (sh) ile birlikte kullanılır.
# Bunlar olmazsa docker birşeyler yazmamıza müsade etmeden container'ı kapatır.
❯ docker run -it --rm busybox sh
/ # cat /etc/hostname 
f8798ad38f50
```

Daha gerçekçi bir örnek ele alalım. Bilgisayarımızda JDK olmadığını ya da en azından şimdi kullanacağımız JDK dağıtım ve sürümünün olmadığı farzedelim. Proje dizininde aşağıdaki komutu çalıştırarak proje dosyalarımızı ve ihtiyacımız olan araçları içeren bir çalışma ortamı hazırlayabilriz.

``` sh
cd dev
docker run -it --rm -v $(pwd)/demo:/work -w /work eclipse-temurin:17-jdk bash
```