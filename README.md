# Arthera Devnet node kurulum rehberi

# sistem gereksinimleri
````
CPU: 4 cores / 8 threads, 2.6GHz, or faster
RAM: 8GB or more
Disk: 500GB of SSD storage or more
````
# Docker kurulumu
[Docker Kurulum Kılavuzu](https://docs.docker.com/engine/install/)

# En son Arthera düğüm görüntüsünü çekin
````
docker pull arthera/arthera-node:latest
````
# Arthera Node verilerinizi tutmak için bir klasör oluşturun.
````
mkdir $HOME/arthera
````
# Doğrulayıcınız için bir cüzdan oluşturun.
````
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest account new
````
# Yeni hesap için bir şifre girin ve aşağıdakine benzer bir çıktı alacaksınız.
````
Your new key was generated

Public address of the key:   0x0bEdA7aAB9cFAa963443e27e30a8F04A79f099C3
Path of the secret key file: /data/keystore/UTC--2023-07-18T21-48-46.572871703Z--088dc563b8184107010c947c4b94bbc856992985

- You can share your public address with anyone. Others need it to interact with you.
- You must NEVER share the secret key with anyone! The key controls access to your funds!
- You must BACKUP your key file! Without the key, it's impossible to access account funds!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
````
- Oluşturulan key dosyanızı $HOME/arthera/keystore/UTC-**** konumundan kopyalayıp yedeklemelisiniz. Şifrenizi unutmayın.

# cüzdanınıza test tokeni almalısınız.
- Doğrulayıcı cüzdanınıza en az 100.001 AA test tokeni almalısınız.

# Doğrulayıcı kimliği oluşturuyoruz.
````
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest validator new
````
# Doğrulayıcı hesabı için bir şifre girin ve aşağıdakine benzer bir çıktı alacaksınız
````
Your new key was generated

Public key:         0xc004c1edab0ed2b77a316c81114177bb071bbc94ec2674a3ccb42c59fe539dd301c1868b919a7cd208d1e8798af141cf2fc41994c35cf4dfc3dd7df74f431f086022
   Address:         0xcF95c3A3DBe170caF1CCf9c8E02C70D4a1E72a2a
Path of the secret key file: /data/keystore/validator/c004c1edab0ed2b77a316c81114177bb071bbc94ec2674a3ccb42c59fe539dd301c1868b919a7cd208d1e8798af141cf2fc41994c35cf4dfc3dd7df74f431f086022

- You can share your public key with anyone. Others need it to validate messages from you.
- You must NEVER share the secret key with anyone! The key controls access to your validator!
- You must BACKUP your key file! Without the key, it's impossible to operate the validator!
- You must REMEMBER your password! Without the password, it's impossible to decrypt the key!
````
- oluşan dosyamızın yedeğini yine alıyoruz. Oluşturduğumuz şifreyi unutmuyoruz.

# Doğrulayıcımızı kayıt ediyoruz.
- Doğrulayıcınızı kaydetmek için [Arthera cüzdanına](https://wallet-test.arthera.net/) ihtiyacımız var.
1. **Arthera Devnet'e MetaMask'i Yapılandırma (Ayrıntıları görün)**
   MetaMask'i Arthera Devnet'e bağlamak için:
   - [Ağ ayrıntılarına bakarak](https://docs.arthera.net/network-details) MetaMask'i yapılandırın.
   
2. **Cüzdan Özel Anahtarınızı İçe Aktarma (5. adımda oluşturuldu)**
   MetaMask'e validator cüzdanınızın özel anahtarını içe aktarmak için:
   - [Bu adrese](https://wallet-dev.arthera.net) gidin.
   - MetaMask ile giriş yapın ve içe aktarılacak hesabı seçin.
   
3. **Validator İçin Cüzdan Oluşturma (Metamask'ta)**
   MetaMask'ta bir validator cüzdanı oluşturmak için:
   - [Arthera Wallet'a git](https://wallet-dev.arthera.net).
   - MetaMask ile giriş yapın ve içe aktardığınız hesabı seçin.
   - Arthera Cüzdan'ın Hesap menüsüne gidin.
   
4. **Validator Olmak İçin Bilgileri Girme (Validator Kimliği Oluşturma)**
   Arthera Wallet'ta bir validator kimliği oluşturmak için:
   - Hesap menüsünde "Bir Validator Ol" bölümüne gidin.
   - Adımlarınızı takip ederek validator genel anahtarınızı girin (7. adımda oluşturuldu).
   - Kayıt oluştur düğmesine tıklayın.
   
5. **Kayıt Başarılı Olursa**
   Başarılı bir kayıt sonrasında, validator kimliğiniz görüntülenecektir.

# Bir şifre dosyası oluşturun. 
````
echo "YOUR_PASSWORD" > $HOME/arthera/password
````
# Düğüm anahtarınızı oluşturun.
````
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest p2p genkey /data/node.key
````
# Aşağıdakileri çalıştırarak anahtarın başarıyla oluşturulup oluşturulmadığını kontrol edin.
````
docker run -it -v $HOME/arthera:/data arthera/arthera-node:latest p2p enodeurl /data/node.key
````
# Aşağıdakine benzer bir çıktı görmelisiniz.
````
enode://f2927b8b1bc1b05997acf60f713cda3c776300cdac52b80d8dbfb2b434ef0e7283f8d3ca891d832bab3927b03c52593e575d48d9916804af5bf66a75b5b1288a@127.0.0.1:6534
````
- Yukarıdaki çıktı, düğümünüzün kodlama URL'sidir. Metrik panosunda düğümünüzü tanımlamak için daha sonra ihtiyacınız olacak.

# En son Devnet genesis dosyasını indirin.
````
curl -o $HOME/arthera/devnet.g http://release.arthera.net/devnet.g
````
# Node muzu test edelim.
Validatorinizi başlatmak için aşağıdaki değişkenleri değiştirin ve aşağıdaki komutu çalıştırın:

- `YOUR_PUBLIC_IP`: Gerçek Genel IP adresinizle değiştirin.
- `ID_FROM_STEP_8`: Adım 8'den elde edilen Validator Kimliği ile değiştirin.
- `VALIDATOR_PUBLIC_KEY_FROM_STEP_7`: Adım 7'den alınan Validator Genel Anahtarınızla değiştirin.
````
docker pull arthera/arthera-node:latest
docker run -v $HOME/arthera:/data arthera/arthera-node:latest -p 6535:6060 -p 18545:18545 -p 18546:18546 \
  --nodekey=/data/node.key \
  --metrics --verbosity=4 \
  --nat=extip:YOUR_PUBLIC_IP \
  --validator.id ID_FROM_STEP_8 \
  --validator.pubkey="VALIDATOR_PUBLIC_KEY_FROM_STEP_7" \
  --validator.password="/data/password"
````
# Aşağıdaki bilgilere sahip olmak için çıktıyı kontrol edin.
````
WARN [08-12|05:21:14.977] Genesis file doesn't refer to any trusted preset
INFO [08-12|05:21:14.995] Applying genesis state
INFO [08-12|05:21:14.995] - Reading epochs
INFO [08-12|05:21:14.995] - Reading blocks
INFO [08-12|05:21:14.996] - Reading EVM data
INFO [08-12|05:21:15.090] Applied genesis state                    name=dev id=10245 genesis=0x53cb19b6c63e7ea459f532daf844f658d411d19c5de834c02397f9e8307e072c
....
INFO [08-12|05:21:15.090] Bootnode                                 url=enode://2195b2c0ca9695cff2fe84851e6213f2a231d21d056572f6e9bc52b800299beea846ac47ab37256200638aa574fb0387df006638eb0026eaac8ef8a5a3f7b604@49.13.19.99:6535
INFO [08-12|05:21:15.090] Bootnode                                 url=enode://a50aca72719cec5e2a1f8cd2361adb8eb363c99968be9b633277e20fd86b702a7b3cc7fecaefaf99200023808a01aba98301b16a931081f49b877d48563db510@168.119.169.170:6535
INFO [08-12|05:21:15.090] Bootnode                                 url=enode://03c117cb3bca6902c31923d99e1cc6afa8aa16cfbd9d1daa262da8b38b96ba2e716e100d1f58b2f6bc26d1b332dd750ea813dd83291038b41659a97adaf88e16@65.108.216.225:6535
....
DEBUG[08-12|05:23:22.804] New event                                ....
DEBUG[08-12|05:23:22.804] New event                                ....
DEBUG[08-12|05:23:22.804] New event                                ....
DEBUG[08-12|05:23:22.805] Persisted trie from memory database      ....
INFO [08-12|05:23:22.805] New block                                ....
````
- If you receive messages like "New event" and "New block," it indicates that your node is syncing and operating correctly.

# Node çalıştıralım.
- Node başlatmak için aşağıdaki değişkenleri değiştirin ve aşağıdaki komutu çalıştırın.

- `YOUR_PUBLIC_IP`: Replace with your actual Public IP address.
- `ID_FROM_STEP_8`: Replace with the Validator ID obtained from step 8 ("Register your validator").
- `VALIDATOR_PUBLIC_KEY_FROM_STEP_7`: Replace with the Public key of your validator obtained from step 7 ("Create the validator identity").
````
docker pull arthera/arthera-node:latest
docker run -d --restart unless-stopped -p 6535:6060 -p 18545:18545 -p 18546:18546 \
  -v $HOME/arthera:/data arthera/arthera-node:latest \
  --nodekey /data/node.key \
  --metrics \
  --nat extip:YOUR_PUBLIC_IP \
  --validator.id ID_FROM_STEP_8 \
  --validator.pubkey "VALIDATOR_PUBLIC_KEY_FROM_STEP_7" \
  --validator.password "/data/password"
````
- Düğümün `docker ps` ile başlatıldığını kontrol edin.
````
CONTAINER ID   IMAGE                         COMMAND                  CREATED         STATUS        PORTS                                                                                                              NAMES
d6ac0f5fdf14   arthera/arthera-node:latest   "arthera-node --data…"   2 seconds ago   Up 1 second   0.0.0.0:18545-18546->18545-18546/tcp, :::18545-18546->18545-18546/tcp, 0.0.0.0:6535->6060/tcp, :::6535->6060/tcp   heuristic_northcutt
````
- Günlükleri kontrol edin `docker logs <CONTAINER_ID>` Aşağıdaki çıktıyı görmelisiniz.
````
INFO [08-12|05:57:39.972] New block                                ....
INFO [08-12|05:57:39.972] New block                                ....
INFO [08-12|05:57:39.972] New block                                ....
INFO [08-12|05:57:39.973] New block                                ....
INFO [08-12|05:57:39.973] New block                                ....
INFO [08-12|05:57:39.973] New block                                ....
````
# Sorun giderme.
- Node senkronize değil.
  -- En yaygın sorunlar, güvenlik duvarları ve yanlış sağlanan harici IP adresidir
- Bozuk veritabanı hatası.
`rm -rf $HOME/arthera/arthera-node $HOME/arthera/chaindata` komut ile veritabanını silip senkronize olması için yeniden başlatalım.

# Lütfen beğenip forklamayı unutmayalım.
