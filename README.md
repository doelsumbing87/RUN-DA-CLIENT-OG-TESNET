
# 🖥️ Setup 0g-da-client di VPS Ubuntu

## 1️⃣ Persiapan VPS
Pastikan VPS Anda telah siap dengan spesifikasi berikut:
- **Sistem Operasi:** Ubuntu 20.04 / 22.04
- **RAM Minimal:** 4GB (disarankan 8GB)
- **Paket Dasar Terinstal:** `curl`, `wget`, `git`, `tmux`, `screen`

### 🔹 Update dan Install Paket Dasar
Pertama, perbarui sistem dan install paket yang diperlukan:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git tmux screen unzip -y
```

Kemudian, download dan install Go:
```bash
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
```

Ekstrak arsip Go:
```bash
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
```

Tambahkan Go ke variabel PATH:
```bash
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile
source ~/.profile
```

---

## 2️⃣ Download dan Install 0g-da-client
Clone repository 0g-da-client dan masuk ke direktori proyek:
```bash
cd /root
git clone https://github.com/0glabs/0g-da-client.git
cd 0g-da-client
```

### 🔹 Install Dependencies
Pastikan Go dan Make telah terinstal:
```bash
sudo apt install -y golang make
```

Periksa apakah Go telah terinstal dengan:
```bash
go version
```

### 🔹 Build atau Download Binary
Untuk membangun dari source code:
```bash
make build
```

Atau, Anda dapat menggunakan binary yang sudah tersedia:
```bash
wget -O bin/combined https://your-binary-link
chmod +x bin/combined
```

---

## 3️⃣ Konfigurasi RPC & Private Key
Ubah nilai berikut sesuai kebutuhan Anda di file konfigurasi:
```bash
RPC_URL="https://16600.rpc.thirdweb.com/"
PRIVATE_KEY=""
KV_DB_PATH="/root/0g-da-client/run/"
```

---

## 4️⃣ Menjalankan 0g-da-client
Untuk menjalankan aplikasi, disarankan menggunakan **tmux** atau **screen** agar tetap berjalan setelah logout.

Jalankan command berikut:
```bash
/root/0g-da-client/disperser/bin/combined \
    --chain.rpc https://16600.rpc.thirdweb.com/ \
    --chain.private-key $gantipk \
    --chain.receipt-wait-rounds 180 \
    --chain.receipt-wait-interval 1s \
    --chain.gas-limit 2000000 \
    --combined-server.use-memory-db \
    --combined-server.storage.kv-db-path ./../run/ \
    --combined-server.storage.time-to-expire 2592000 \
    --disperser-server.grpc-port 51001 \
    --batcher.da-entrance-contract 0x857C0A28A8634614BB2C96039Cf4a20AFF709Aa9 \
    --batcher.da-signers-contract 0x0000000000000000000000000000000000001000 \
    --batcher.finalizer-interval 20s \
    --batcher.confirmer-num 3 \
    --batcher.max-num-retries-for-sign 3 \
    --batcher.finalized-block-count 50 \
    --batcher.batch-size-limit 500 \
    --batcher.encoding-interval 3s \
    --batcher.encoding-request-queue-size 1 \
    --batcher.pull-interval 10s \
    --batcher.signing-interval 3s \
    --batcher.signed-pull-interval 20s \
    --encoder-socket https://encoder.testnet.0g.ai \
    --encoding-timeout 300s \
    --signing-timeout 60s \
    --chain-read-timeout 12s \
    --chain-write-timeout 13s \
    --combined-server.log.level-file trace \
    --combined-server.log.level-std trace \
    --combined-server.log.path ./../run/run.log
```

Untuk keluar dari **tmux** tanpa menghentikan proses, tekan:
```bash
CTRL + B, lalu tekan D
```

---

## 5️⃣ Cek Status
Untuk mengecek apakah proses berjalan, gunakan:
```bash
ps aux | grep 0g-da-client
```

Jika Anda ingin melihat log, jalankan:
```bash
tail -f /root/0g-da-client/run/run.log
```

---

## 6️⃣ Menjalankan di Background (Opsional)
Jika Anda tidak ingin menggunakan **tmux**, Anda bisa menjalankan aplikasi dengan `nohup` agar tetap berjalan setelah logout:
```bash
nohup /root/0g-da-client/bin/combined --chain.rpc $RPC_URL ... &> /root/0g-da-client/run.log &
```

Periksa apakah aplikasi berjalan:
```bash
ps aux | grep 0g-da-client
```

---

🎯 **Sekarang 0g-da-client seharusnya berjalan di VPS Anda dengan aman!** 🚀
