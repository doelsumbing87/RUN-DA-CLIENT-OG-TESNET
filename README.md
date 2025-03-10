# 🖥️ Setup 0g-da-client di VPS Ubuntu

## 1️⃣ Persiapan VPS
Pastikan VPS sudah siap dengan:
- **Sistem operasi:** Ubuntu 20.04 / 22.04
- **RAM minimal:** 4GB (disarankan 8GB)
- **Paket dasar terinstal:** `curl`, `wget`, `git`, `tmux`, `screen`

### 🔹 Update dan Install Paket Dasar
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl wget git tmux screen unzip -y
```

---

## 2️⃣ Download dan Install 0g-da-client
```bash
cd /root
git clone https://github.com/0glabs/0g-da-client.git
cd 0g-da-client
```
Install Dependencies**
Pastikan Go dan Make sudah terinstal:
```sh
sudo apt install -y golang make
```
Cek apakah Go sudah terinstal dengan:
```sh
go version
```

### 🔹 Build atau Download Binary
Jika ingin membangun dari source:
```bash
make build
```
Atau gunakan binary yang sudah tersedia:
```bash
wget -O bin/combined https://your-binary-link
chmod +x bin/combined
```

---

## 3️⃣ Konfigurasi RPC & Private Key
**Ubah nilai berikut sesuai kebutuhan:**
```bash
RPC_URL="https://16600.rpc.thirdweb.com/"
PRIVATE_KEY=""
KV_DB_PATH="/root/0g-da-client/run/"
```

---

## 4️⃣ Menjalankan 0g-da-client
Gunakan **tmux** atau **screen** agar tetap berjalan setelah logout:
Lalu jalankan command berikut:
```bash
tmux new -s 0g-client "/root/0g-da-client/disperser/bin/combined \
    --chain.rpc https://16600.rpc.thirdweb.com/ \
    --chain.private-key $GANTI_PK \
    --chain.receipt-wait-rounds 180 \
    --chain.receipt-wait-interval 1s \
    --chain.gas-limit 2000000 \
    --combined-server.use-memory-db \
    --combined-server.storage.kv-db-path /root/0g-da-client/run/ \
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
    --combined-server.log.path /root/0g-da-client/run/run.log

```

Untuk keluar dari **tmux** tanpa menghentikan proses:
```bash
CTRL + B, lalu tekan D
```

---

## 5️⃣ Cek Status
Cek apakah proses berjalan:
```bash
ps aux | grep 0g-da-client
```
Jika ingin melihat log:
```bash
tail -f /root/0g-da-client/run/run.log
```

---

## 6️⃣ Menjalankan di Background (Opsional)
Jika tidak ingin pakai **tmux**, jalankan dengan `nohup` agar tetap berjalan setelah logout:
```bash
nohup /root/0g-da-client/bin/combined --chain.rpc $RPC_URL ... &> /root/0g-da-client/run.log &
```

Cek apakah berjalan:
```bash
ps aux | grep 0g-da-client
```

---

🎯 **Sekarang 0g-da-client seharusnya berjalan di VPS baru dengan aman!** 🚀
