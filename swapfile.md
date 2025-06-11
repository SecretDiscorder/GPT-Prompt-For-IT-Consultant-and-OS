Berikut adalah panduan **membuat swap file di Ubuntu 16.04 dan juga swapfile di Windows (semacamnya, yaitu virtual memory / pagefile)**. Penjelasan dibagi per sistem operasi:

---

## 🐧 **A. Membuat Swapfile di Ubuntu 16.04**

Langkah-langkah (aman untuk semua sistem file seperti ext4):

### 1. **Cek apakah sudah ada swap**

```bash
swapon --show
```

Jika tidak ada output, berarti belum ada swap aktif.

---

### 2. **Buat swapfile (misalnya 2GB)**

```bash
sudo fallocate -l 2G /swapfile
```

Atau jika `fallocate` tidak berfungsi (jarang, tapi mungkin di sistem lama):

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=2048
```

---

### 3. **Set izin aman**

```bash
sudo chmod 600 /swapfile
```

---

### 4. **Jadikan file tersebut swap**

```bash
sudo mkswap /swapfile
```

---

### 5. **Aktifkan swap**

```bash
sudo swapon /swapfile
```

---

### 6. **Agar aktif saat boot, edit `/etc/fstab`**

Tambahkan baris berikut ke `/etc/fstab`:

```
/swapfile none swap sw 0 0
```

---

### 7. (Opsional) **Atur swappiness (seberapa agresif swap digunakan)**

Edit `/etc/sysctl.conf`, tambahkan:

```
vm.swappiness=10
```

Lalu jalankan:

```bash
sudo sysctl -p
```

---

## 🪟 **B. Swapfile di Windows (Pagefile / Virtual Memory)**

Windows menggunakan **pagefile.sys** sebagai pengganti swap. Sudah aktif secara default, tapi kamu bisa mengatur ukurannya:

### Langkah:

1. **Buka System Properties**:

   * Klik kanan **This PC → Properties → Advanced system settings**.

2. Klik tab **Advanced → Settings (di bawah Performance)**.

3. Tab **Advanced → Virtual Memory → Change**.

4. Hilangkan centang **"Automatically manage paging file size for all drives"**.

5. Pilih drive C: → Pilih **Custom size**:

   * Initial size: misalnya 2048 MB
   * Maximum size: 4096 MB (atau sesuai kebutuhan)

6. Klik **Set**, lalu **OK**, dan **restart komputer** jika diminta.

---

## 🔚 Kesimpulan

| OS      | Tipe Swap      | Bisa Diatur Manual | Nama File                                 |
| ------- | -------------- | ------------------ | ----------------------------------------- |
| Ubuntu  | Swapfile       | Ya                 | `/swapfile`                               |
| Windows | Virtual Memory | Ya                 | `pagefile.sys` (otomatis dikelola sistem) |

Kamu mau swap diaktifkan untuk membantu RAM kecil, atau karena keperluan hibernasi, atau alasan lain? Bisa kubantu sesuaikan jumlah dan fungsinya.
