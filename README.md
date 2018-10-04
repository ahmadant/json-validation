Validasi JSON
===============

JSON Validasi adalah jenis media untuk menggambarkan dan memvalidasi struktur
Data JSON. Selama bertahun-tahun, saya telah menulis banyak JSON Schemas dan membantu orang lain
di semua jenis domain menulis JSON Schemas mereka. Validasi JSON adalah a
pencitraan ulang JSON Skema berdasarkan pengalaman dan pengetahuan saya. Dalam bagian,
ini merupakan implementasi tentang bagaimana saya mendefinisikan JSON Skema jika diberi batu tulis kosong,
tetapi kebanyakan saya menggunakan implementasi ini sebagai kotak pasir untuk mencoba ide-ide baru dan
pendekatan ke gaya Skema JSON.

Dokumen Validasi JSON adalah a [Referensi JSON](https://github.com/jdesrosiers/json-reference)
dokumen dan memiliki tipe konten `application/validation+json`. Untuk mengerti
JSON Validasi, Anda harus terlebih dahulu memahami Referensi JSON. Berhati-hatilah bahwa ini
tidak cukup Referensi JSON yang sama yang digunakan dalam JSON Schema.

Sebuah dokumen Validasi JSON adalah sekumpulan batasan delaratif yang dibuat oleh seorang JSON
dokumen harus sesuai agar dianggap valid. Validasi JSON
dokumen dapat diuraikan menjadi fungsi murni yang dapat digunakan untuk memvalidasi JSON
dokumen dalam bahasa apa pun.

Instalasi
------------

Pemakaian
-----

```http
GET http://validation.hyperjump.io/example1 HTTP/1.1
Accept: application/reference+json
```

```http
HTTP/1.1 200 OK
Content-Type: application/validation+json

{
  "type": "object",
  "properties": {
    "foo": { "type": "string" }
  },
  "required": ["foo"]
}
```

```javascript
(async () => {
  // Dapatkan dokumen validasi
  const doc = await JsonValidation.get("http://validation.hyperjump.io/example1");

  // Dapatkan fungsi validator dari dokumen validasi
  const validate = await JsonValidation.validate(doc);

  const result1 = validate({ "foo": "bar" });
  ValidationResult.isValid(result1); // => true

  const result2 = validate({ "abc": 123 });
  ValidationResult.isValid(result2); // => false
}());
```

Berkontribusi
------------

### Tes

Jalankan tes

```bash
npm test
```

Jalankan tes dengan pelari uji kontinyu
```bash
npm test -- --watch
```

Filosofi dan Ketertinggalan Arsitektur
---------------------------------------

### JSON

JSON Validasi hanya melakukan apa yang dikatakannya; Ini memvalidasi JSON. Tidak
ditujukan untuk memvalidasi jenis bahasa pemrograman tertentu atau struktur data.

### Client-Server

JSON Validasi dirancang untuk digunakan sebagai bagian dari arsitektur client-server.
Oleh karena itu dokumen Validasi JSON, harus diidentifikasi oleh URL dan harus
dapat diambil.

### Layered System

Validasi JSON harus dapat disusun pada berbagai level sebanyak mungkin. Ada sebuah
set kata kunci yang telah ditetapkan. Kata kunci baru dapat didefinisikan sebagai komposisi
kata kunci lainnya. (segera akan datang)

Dokumen JSON Validasi adalah kumpulan kata kunci. Setiap kata kunci menambahkan
paksaan. Dokumen Validasi JSON yang kosong (`{}`) tidak memiliki hambatan. Semua JSON
dokumen valid. Setiap kata kunci menambah kendala semakin mempersempit apa
merupakan dokumen yang valid.

### Stateless

Semua kata kunci tidak bernegara. Hasil memvalidasi kata kunci tergantung
hanya pada nilai yang divalidasi dan nilai kata kunci. Kata kunci tidak bisa
tergantung pada kata kunci lain atau data eksternal apa pun.

### Cache

Dokumen Validasi JSON tidak dapat diubah dan harus dapat disimpan dalam cache selamanya. Sekali
diterbitkan, mereka seharusnya tidak pernah berubah. Jika mereka perlu berubah, dokumen baru
harus dibuat yang diidentifikasi oleh URL unik.

### Uniform Interface

Dokumen Validasi JSON harus memvalidasi cara yang sama tidak peduli bahasa apa
validator diimplementasikan. Setiap dokumen mengidentifikasi kata kunci apa
implementasi harus mendukung. Membangkitkan fungsi validasi akan menghasilkan
kesalahan jika implementasi tidak mendukung satu atau lebih dari kata kunci
dalam dokumen Validasi JSON. (segera akan datang)

Hasil validasi dokumen JSON harus mengikuti struktur standar.
(segera akan datang)

### Code on Demand

Untuk hal-hal yang tidak dapat digambarkan sebagai kata kunci, itu harus mungkin
menggambarkan implementasi kata kunci menggunakan JavaScript (segera hadir).

TODOs
-----

* Kata kunci sebagai URL
* Kata kunci harus dinyatakan
* Validasi meta untuk kata kunci yang nilainya adalah skema
* Validator JON serializable
* Hasil validasi lebih rinci
* Kata kunci pesan galat
* Nilai sebagai dokumen Referensi JSON
* Komposisi kata kunci
* `$data` kata kunci
