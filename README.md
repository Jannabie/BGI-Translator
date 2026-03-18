# BGI Translator

Tools penerjemahan visual novel berbasis engine **Ethornell / BGI (Buriko General Interpreter)**.

---

## Persyaratan

- Windows 10 / 11 (64-bit)
- [.NET 10 Runtime](https://dotnet.microsoft.com/download)
- `EthornellEditor.dll` dari [arcusmaximus/EthornellEditor](https://github.com/arcusmaximus/EthornellEditor/releases)
- `CSystemArc.exe` — untuk ekstrak/pack file `.arc`

---

## Instalasi

Letakkan file berikut dalam satu folder:

```
📁 BGITranslator\
  ├── BGITranslator.exe
  ├── BGITranslator.dll
  ├── BGITranslator.runtimeconfig.json
  ├── EthornellEditor.dll   ← wajib ada
  └── CSystemArc.exe
```

> `EthornellEditor.dll` harus berada di folder yang sama dengan `.exe`.

---

## Build dari Source

```bash
git clone https://github.com/Jannabie/BGI-Translator.git
cd BGI-Translator
dotnet build -c Release
```

Output ada di `bin\Release\net10.0-windows\`.

---

## Cara Pakai

1. Ekstrak arsip game dengan `CSystemArc.exe`
2. Buka BGI Translator → drag & drop file script `.sc`
3. Terjemahkan teks di kolom kanan
4. Simpan → pack ulang dengan `CSystemArc.exe`

### Fitur

| Fitur | Keterangan |
|---|---|
| Editor dua kolom | JP di kiri, terjemahan di kanan |
| Pencarian realtime | `Ctrl+F`, navigasi dengan `F3` |
| Filter baris | Tampilkan hanya yang belum diterjemahkan |
| Kamus Nama | Deteksi otomatis nama karakter, ganti sekaligus |
| Glosarium batch | Terapkan daftar kosakata ke seluruh file |
| Export / Import TSV | Backup atau kolaborasi via spreadsheet |
| Progress bar | Pantau progres terjemahan |

### Pintasan Keyboard

| Tombol | Aksi |
|---|---|
| `Ctrl+O` | Buka file |
| `Ctrl+S` | Simpan |
| `Ctrl+F` | Pencarian |
| `Ctrl+N` | Kamus Nama Karakter |
| `F3 / Shift+F3` | Hasil pencarian berikutnya / sebelumnya |
| `Tab` | Konfirmasi & pindah ke baris berikutnya |

---

## Kredit

- **[arcusmaximus](https://github.com/arcusmaximus)** — `EthornellEditor.dll`, `CSystemArc.exe`, `BgiDisassembler.exe`
