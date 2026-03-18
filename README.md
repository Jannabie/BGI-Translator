# BGI Translator

> Tools penerjemahan visual novel berbasis engine **Ethornell / BGI (Buriko General Interpreter)**.  
> Diuji pada: **Sakura no Uta -Haru no Yuki-** (さくらのうた -桜の詩-)

---

## Tentang

BGI Translator adalah pengganti [EEGUI](https://github.com/arcusmaximus/EthornellEditor) yang lebih lengkap dan ramah pemula. Dibangun di atas library `EthornellEditor.dll` milik **arcusmaximus**, tools ini menambahkan fitur yang tidak ada di EEGUI: pencarian teks, filter baris, deteksi tag otomatis, glosarium batch, progress bar, dan export/import TSV.

| Fitur | EEGUI | BGI Translator |
|---|---|---|
| Editor dua kolom JP ↔ ID | ✗ | ✓ |
| Pencarian teks realtime | ✗ | ✓ |
| Filter baris | ✗ | ✓ |
| Deteksi & sisip tag | ✗ | ✓ |
| Progress bar | ✗ | ✓ |
| Export / Import TSV | ✗ | ✓ |
| Glosarium otomatis batch | ✗ | ✓ |
| Drag & drop file | ✗ | ✓ |
| Dark theme | ✗ | ✓ |

---

## Persyaratan

- **Windows 10 / 11** (64-bit)
- **.NET 10 SDK** — untuk build: [download di sini](https://dotnet.microsoft.com/download)
- **.NET 10 Runtime** — untuk menjalankan EXE hasil build (biasanya sudah terinstall bersama SDK)
- `EthornellEditor.dll` — dari [EthornellEditor oleh arcusmaximus](https://github.com/arcusmaximus/EthornellEditor/releases)
- `CSystemArc.exe` — untuk ekstrak / pack arsip `.arc`
- `BgiDisassembler.exe` — untuk disassemble file script `.sc`

---

## Cara Build

**1. Clone atau download repository ini**
```
git clone https://github.com/username/bgi-translator.git
cd bgi-translator
```

**2. Pastikan `EthornellEditor.dll` ada di folder yang sama dengan file `.csproj`**
```
bgi-translator/
  ├── BGITranslator.csproj
  ├── Program.cs
  ├── MainForm.cs
  ├── BurikoWrapper.cs
  ├── Theme.cs
  └── EthornellEditor.dll   ← letakkan di sini
```

**3. Build**
```
dotnet build -c Release
```

**4. EXE hasil build ada di:**
```
bin\Release\net10.0-windows\BGITranslator.exe
```

---

## Cara Deploy

Buat folder baru dan isi dengan file-file berikut:

```
📁 BGITranslator\
  ├── BGITranslator.exe                 ← hasil build
  ├── BGITranslator.dll                 ← hasil build
  ├── BGITranslator.runtimeconfig.json  ← hasil build
  ├── EthornellEditor.dll               ← WAJIB — dimuat saat runtime
  ├── CSystemArc.exe                    ← untuk pack/unpack .arc
  ├── BgiDisassembler.exe               ← untuk disassemble .sc
  └── BgiImageEncoder.exe               ← untuk konversi gambar (opsional)
```

> ⚠️ `EthornellEditor.dll` **harus** berada di folder yang sama dengan `BGITranslator.exe`. Tanpanya tools tidak bisa membuka file script sama sekali.

---

## Alur Kerja Terjemahan

Berikut alur lengkap dari file game hingga patch siap pakai.

### Langkah 1 — Ekstrak arsip `.arc`

File script game tersimpan di dalam arsip `.arc`. Ekstrak menggunakan `CSystemArc.exe`:

```
CSystemArc.exe extract <nama_game>.arc output\
```

Hasilnya adalah folder berisi file-file `.sc` (script), gambar, dan aset lainnya.

---

### Langkah 2 — Disassemble script `.sc` (opsional)

Jika ingin melihat struktur script sebelum mengedit:

```
BgiDisassembler.exe namafile.sc
```

Output berupa file teks `.sc.txt` yang bisa dibaca manual.

> Untuk BGI Translator, langkah ini **tidak wajib** — tools langsung membuka file `.sc` biner.

---

### Langkah 3 — Terjemahkan dengan BGI Translator

1. Jalankan `BGITranslator.exe`
2. **File → Buka Script** (atau drag & drop file `.sc` ke jendela)
3. Terjemahkan teks di kolom kanan
4. Gunakan fitur-fitur berikut untuk mempercepat pekerjaan:

| Fitur | Cara pakai |
|---|---|
| **Pencarian** | Ketik di kotak 🔍, navigasi dengan `F3` / `Shift+F3` |
| **Filter** | Pilih "Belum diterjemahkan" untuk fokus ke baris kosong |
| **Tag otomatis** | Tag seperti `\n`, `@name` terdeteksi otomatis — klik untuk sisipkan |
| **Glosarium** | Tools → Glosarium Otomatis → isi term JP → ID → Terapkan |
| **Export TSV** | File → Export TSV — untuk backup atau kolaborasi |
| **Import TSV** | File → Import TSV — untuk load terjemahan dari file eksternal |

5. **File → Simpan** untuk menyimpan script yang sudah diterjemahkan

---

### Langkah 4 — Pack ulang ke `.arc`

Setelah semua script diterjemahkan, pack kembali ke arsip `.arc`:

```
CSystemArc.exe pack output\ <nama_game>_patched.arc
```

Ganti file `.arc` asli di folder game dengan file yang sudah dipatch.

---

## Pintasan Keyboard

| Tombol | Aksi |
|---|---|
| `Ctrl+O` | Buka file script |
| `Ctrl+S` | Simpan |
| `Ctrl+F` | Fokus ke pencarian |
| `F3` | Hasil pencarian berikutnya |
| `Shift+F3` | Hasil pencarian sebelumnya |
| `Ctrl+G` | Pergi ke nomor baris |
| `Enter` | Edit sel terjemahan |
| `Tab` | Konfirmasi dan pindah ke baris berikutnya |
| `Esc` | Bersihkan pencarian |

---

## Format TSV

File TSV yang diekspor memiliki format berikut (UTF-8, tab-separated):

```
# BGI Translator Export | 2025-01-01 12:00
# Index	Original	Terjemahan
0	桜の森で	Di hutan sakura
1	世界が鳴った。	Dunia pun berdentang.
```

Format ini kompatibel dengan spreadsheet editor seperti LibreOffice Calc atau Google Sheets.

---

## Catatan Teknis

### Kenapa perlu `Encoding.RegisterProvider`?

`EthornellEditor.dll` menggunakan Shift-JIS (code page 932) secara internal via class `BGIEncoding`. Di .NET Framework (tempat DLL ini dikompilasi), Shift-JIS selalu tersedia. Di .NET 5 ke atas termasuk .NET 10, encoding non-Unicode tidak diinclude secara default. `BGI Translator` mendaftarkan `CodePagesEncodingProvider` sebelum DLL dimuat sehingga masalah ini teratasi.

### API `EthornellEditor.dll`

Berdasarkan analisis metadata .NET (`MethodDef` table) dari DLL:

```csharp
// Membaca file script, mengembalikan array string asli
string[] Import(byte[] rawFileData)

// Menyimpan dengan terjemahan baru, mengembalikan bytes file
byte[] Export(string[] translations)

// Versi tanpa parameter (menggunakan internal state)
byte[] Export()
```

---

## Kredit

- **[arcusmaximus](https://github.com/arcusmaximus)** — `EthornellEditor.dll`, `BgiDisassembler.exe`, `CSystemArc.exe`, `BgiImageEncoder.exe`

