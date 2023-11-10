# Kurulum

Lokalde kubernetes kurulumu için aşağıdaki linklerden faydalanabilirsiniz.

- [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
- [minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/)
- [Rancher Desktop](https://rancherdesktop.io/)

Sonrasında ortamımıza erişebilmek için `kubectl` aracını kullanmamız gerekecek. Onu da aşağdaıki linkten faydalanarak kurabilirsiniz.

https://kubernetes.io/docs/tasks/tools/

Yukarıdaki kurulumları yaptıktan sonra ev dizininde (linux'da `/home/$user` altında, windows'da `C:\Kullanıcılar\$user` altında) `.kube` dizini altında `config` adında bir dosya bulunması gerekiyor. Bu dosyada ortama erişmek için gerekli bilgiler bulunuyor (IP, sertifika, token vs).

Kurulumların başarılı olduğunu test etmek için aşağıdaki komutu çalıştırın:

`kubectl get nodes`

Bu komutun sonucunda aşağıdakine benzer bir çıktı alacaksınız:

``` sh
❯ kubectl get nodes
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   41s   v1.27.3
```