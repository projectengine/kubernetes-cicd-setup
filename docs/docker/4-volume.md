# Volume

Volume'ler, container'ların host makinedeki dosya ve dizinleri kullanabilmesi ya da container içinde değişiklik yapılan dosya ve dizinlerin korunması için kullanılır.

## Host makinedeki dizini kullanma

Önce içinde 'Hello Docker' yazılı `hello.txt` dosyasını oluşturalım. Ardından aşağıdaki komutla container'ımızı çalıştırıp dosyayı da içindeki bir dizine almasını söyleyelim:

`docker run -v $(pwd)/hello.txt:/tmp/hello.txt busybox cat /tmp/hello.txt`

Bu komut çalıştığında container ekrana 'Hello Docker' yazıp kapanacak. Container bu metni, oluşturduğumuz dosyadan okuyor ve dosyayı da volume ile kendi sistemine alıyor.

Bu tip volume'lerin kullanım alanı genelde config dosyalarını container'a dışarıdan vermektir. Bu tarz bir kullanım örneğini Dockerfile yazmaya başladığımızda göreceğiz.

## Container içindeki değişiklikleri koruma

Bu kullanım şekli için bir veritabanı container'ı ayağa kaldıralım.

`docker run -d --name postgres -e POSTGRES_PASSWORD=123456 postgres:15-alpine`

Veritabanının loglarını `docker logs -f postgres` komutu ile görebiliriz. Önceki komuttaki `-d` argümanı container'ın arkaplanda çalışmasını sağlıyor. Böylece `ctrl + c` yaptığımızda container kapanmıyor. `-e` ile de container'a ortam değişkeni verebiliyoruz. Bunu da genelde container'ın ayarlarını değiştirmek için kullanıyoruz.

Veritabanımız ayağa kalkınca `docker exec -it postgres sh` komutu ile container'ın terminaline girebiliriz. `exec` aslen container içinde bir komut çalıştırmaya yarıyor. Biz komut olarak `sh` yani terminal komutu kullanıyoruz ve `-it` ile interaktif yani sadece sonucunu basmak yerine yazılabilir olmasını söylüyoruz.

Container'nı terminali açılınca aşağıdaki komutlarla veritabanına bağlanıp birkaç değişiklik yapalım.

```sh
psql -U postgres # Veritabanına bağlan
\dt # Did not find any relations.
create table yeni (id serial, name text); # tablo yarat
\dt
#         List of relations
# Schema | Name | Type  |  Owner   
#--------+------+-------+----------
# public | yeni | table | postgres
#(1 row)
exit
```

Şimdi `exit` yazarak terminalden de çıkalım ve `docker rm -f postgres` ile container'ı silelim. Yaptığımız değişiklikler artık yok.

Bu değişikliklerin kalıcı olmasını istiyorsak (örneğin container imajını güncellerken, veriyi farklı bir container için kullanırken vs.) volume özelliğini kullanabilriiz. Bunun için önce bir volume oluşturup sonra ilk komutumuza bir argüman daha ekliyoruz.

1. `docker volume create db`
2. `docker run -d --name postgres -e POSTGRES_PASSWORD=123456 -v db:/var/lib/postgresql/data postgres:15-alpine`

Yukarıdaki işlemleri bu container için de uygulayalım (psql ile bağlan, tablo yarat, tabloları listele, çıkış yap). Sonra container'ı silelim ve 2 nolu komutu kullanalarak yeniden oluşturalım. Bu kez veritabanına bağlanıp `\dt` ile listelediğimizde yeni tablosunun olduğunu görebiliriz.