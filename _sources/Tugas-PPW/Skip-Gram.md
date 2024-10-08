---
title: 210411100093_adz dzikry pradana putra_word embedding

---

## Word Embedding
Word Embedding adalah teknik untuk merepresentasikan kata-kata dalam bentuk vektor numerik, yang memungkinkan komputer untuk memahami hubungan antara kata-kata dalam suatu konteks. Teknik ini mengubah kata menjadi representasi yang memiliki makna semantik, sehingga kata-kata dengan makna serupa memiliki representasi yang lebih dekat satu sama lain dalam ruang vektor.

Dalam word embeddings, kata-kata yang memiliki sifat atau makna yang mirip, contohnya kata-kata yang sering muncul dalam konteks yang sama atau memiliki arti semantik yang mirip, akan ditempatkan berdekatan satu sama lain dalam ruang vektor tersebut. Jadi, kata-kata yang memiliki hubungan makna akan memiliki posisi yang tidak terlalu jauh satu sama lain dalam ruang tersebut, memungkinkan komputer untuk memahami hubungan dan kesamaan antar kata secara lebih baik.

Berikut merupakan contoh model untuk membangun word embedding seperti :
* Word2Vec
* Skip-gram
* GloVe 


### Skip-Gram
Skip-Gram adalah salah satu metode dalam Word2Vec yang digunakan untuk membangun word embeddings. Inti dari metode ini adalah mempelajari representasi vektor kata dengan cara memprediksi kata-kata yang kemungkinan besar muncul di sekitar kata tertentu dalam sebuah kalimat.

Skip-Gram bertugas untuk memprediksi kata-kata tetangga (kata-kata di sekitar) dari kata target. Misalnya, jika diberikan kata "Makan", model akan mencoba memprediksi kata-kata yang mungkin berada di sekeliling kata tersebut, seperti "saya", "nasi", "putih".

Skip-gram dapat disebut juga dengan rekayasa fitur

### Arsiterktur Skip-Gram
![![arsitektur skip gram](https://hackmd.io/_uploads/BylqbI40R.jpg)[Skip-Gram model structure. Current center word is "passes"]

Arsitektur Skip-Gram adalah salah satu model yang digunakan dalam pemodelan kata, terutama dalam konteks pembelajaran representasi kata (word embedding). Model ini dirancang untuk mempelajari representasi vektor dari kata-kata dalam suatu korpus berdasarkan konteks kata tersebut.

Model Skip-Gram bertujuan untuk memprediksi kata-kata konteks berdasarkan kata pusat

## Perhitungan Manual Model Skip-Gram
### Contoh Perhitungan model Skip-Gram
Contoh Kalimat = "yanto makan sate di madura dengan teman"

### Representasi One-Hot Encoding
mempresentasikan ke dalam bentuk vektor. membuat representasi one-hot encoding untuk setiap kata dalam kalimat tersebut.
|         | Yanto  | makan  | sate  | di | madura | dengan | teman |
|---------|------|------|------|---------|-----------|--------|-------|
| Yanto   | 1 | 0 | 0 | 0    | 0      | 0   | 0  |
| makan   | 0 | 1 | 0 | 0    | 0      | 0   | 0  | 
| sate    | 0 | 0 | 1 | 0    | 0      | 0   | 0  | 
| di      | 0 | 0 | 0 | 1    | 0      | 0   | 0  | 
| madura  | 0 | 0 | 0 | 0    | 1      | 0   | 0  | 
| dengan  | 0 | 0 | 0 | 0    | 0      | 1   | 0  | 
| teman   | 0 | 0 | 0 | 0    | 0      | 0   | 1  | 
 


### Pasangan Kata
membuat pasangan kata dengan kata tengah , kiri , dan kanan
Berikut adalah pasangan kata dan tetangganya
|  target  |  tetangga       |
|----------|-----------------|
|  yanto   |   makan         |
|  makan   |   yanto,sate    |
|  sate    |   makan,di      |
|  di      |   sate,madura   |
|  madura  |   di,dengan     |
|  dengan  |   madura,teman  |
|  teman   |   dengan        |

### Membuat Vektor Embedding

selanjutnya yaitu merubah kata menjadi bentuk vektor. lalu menentukan ukuran dimensi = 3

buat nilai embedding acak untuk setiap kata sebagai berikut :
|  kata    |  vector embedding|
|----------|------------------|
|  yanto   |  [0.1,0.2,0.3]   |
|  makan   |  [0.4,0.5,0.6]   |
|  sate    |  [0.7,0.8,0.9]   |
|  di      |  [0.2,0.3,0.1]   |
|  madura  |  [0.5,0.7,0.6]   |
|  dengan  |  [0.3,0.6,0.4]   |
|  teman   |  [0.8,0.2,0.1]   |


#### Iterasi ke 1
sekarang akan menghitung representasi untuk kata "makan" menggunakan tetangga kiri dan kanan yaitu 'yanto' dan 'sate'
1. untuk "makan" dengan tetangga kiri 'yanto':
        $$Vector \, makan = [0.4,0.5,0.6]$$
        $$Vector \, yanto = [0.1,0.2,0.3]$$
        
2. Untuk "makan" dengan tetangga "sate":
        $$Vector \, sate = [0.7,0.8,0.9]$$
     
     $$vector \, makan = \frac{[0.1,0.2,0.3]+[0.7,0.8,0.9]}{2}=\frac{[0.8,1.0,1.2]}{2}=[0.4,0.5,0.6]$$
     
**note:**
di atas merupakan bagian dari iterasi pertama, di mana kita menggunakan pasangan kata-tetangga untuk membentuk representasi awal dari kata. Setelah beberapa iterasi, representasi vektor akan diperbarui dan menjadi lebih akurat dalam merepresentasikan hubungan semantik antar kata.

#### Iterasi ke 2
Menggunakan contoh kata sama dengan kata target "makan" dan tetangganya "yanto" serta "sate". setelah iterasi pertama, kita menghitung error dan melakukan pembaharuan vektor pada kata "makan".

1. Vektor Awal dari Iterasi Pertama: Pada iterasi pertama, kita telah menghitung representasi sementara sebagai berikut:
**yanto : [0.1,0.2,0.3]
makan: [0.4,0.5,0.6]
sate : [0.7,0.8,0.9]**

Perbarui vektor embedding untuk kata target "makan" dengan memperhitungkan error

pada pasangan "makan" dengan tetangga "yanto" dan "sate", dilakukan pembaharuan embedding pada kata "makan" menggunakan rumus:

Rumus: $$vektor(makan)=vektor \, makan − learning \, rate × gradien \, error$$

1. Gradien error mengacu pada perbedaan antara prediksi model dan hasil yang diharapkan. Gradien yang dihasilkan adalah 0.05 untuk setiap dimensi vektor.
2. Pembaruan dilakukan berdasarkan gradient descent, di mana bobot embedding diubah sedikit untuk memperbaiki error. Menggunakan learning rate sebesar 0.1

Perhitungan :

Dimensi pertama vector makan:
- $$vektor \, makan^{new}=0.4−(0.1×0.05)=0.395$$

Dimensi kedua vector makan:
- $$vektor \, makan^{baru}=0.5−(0.1×0.05)=0.495$$

Dimensi Ketiga vector makan:
- $$vektor \, makan^{baru}=0.6−(0.1×0.05)=0.595$$

Setelah pembaruan untuk pasangan pertama, **vektor embedding untuk "makan" adalah:[0.395,0.495,0.595]**


Pembaruan Vektor "yanto"
selanjutnya akan memperbarui vektor embedding untuk kata "yanto" berdasarkan error yang dihitung setelah iterasi pertama.
Dimensi pertama vector yanto
- $$vektor \, yanto^{baru}=0.1−(0.1×0.05)=0.095$$

Dimensi kedua vector yanto
- $$vektor \, yanto^{baru}=0.2−(0.1×0.05)=0.195$$

Dimensi ketiga vector yanto
- $$vektor \, yanto^{baru}=0.3−(0.1×0.05)=0.295$$

**Hasil Vektor "yanto" setelah Iterasi Kedua =[0.095,0.195,0.295]**


selanjutnya yaitu memperbarui Vektor Embedding pada kata Tetangga "sate", Sama dengan sebelumnya, dilakukan pembaruan untuk tetangga "sate" menggunakan vektor embedding "makan".

Dimensi pertama vector sate
- $$vektor \, sate^{baru}=0.7−(0.1×0.05)=0.695$$

Dimensi kedua vector sate
- $$vektor \, sate^{baru}=0.8−(0.1×0.05)=0.795$$

Dimensi ketiga vector sate
- $$vektor \, sate^{baru}=0.9−(0.1×0.05)=0.895$$

**Setelah dilakukan proses pembaruan untuk tetangga "sate" didapatkan hasil =[0.695,0.795,0.895]**

### Rekapitulasi Vektor Embedding 
Setelah iterasi kedua, berikut adalah vektor embedding yang diperbarui untuk kata "makan", "yanto", dan "sate":
* **Vektor "makan": [0.395,0.495,0.595]**
* **Vektor "sate": [0.695,0.795,0.895]**
* **Vektor "yanto": [0.095,0.195,0.295]**

**note:**
Pada iterasi kedua, telah dilakukan pembaruan vektor embedding pada kata "makan", "yanto", dan "sate" berdasarkan hasil iterasi pertama dengan menggunakan backpropagation dan pembaruan bobot (embedding).

##### Iterasi ke 3
Pada iterasi ketiga, kita akan melanjutkan pembaruan vektor embedding untuk kata "makan", "yanto", dan "sate" berdasarkan hasil dari iterasi kedua.

Vektor embedding setelah iterasi kedua:

* **Vektor "makan": [0.395,0.495,0.595]**
* **Vektor "yanto": [0.095,0.195,0.295]**
* **Vektor "sate": [0.695,0.795,0.895]**

Pembaruan pada Iterasi Ketiga
Untuk iterasi ketiga, diasumsikan nilai gradien error tetap 0.05 per dimensi, dan learning rate tetap 0.1 .

1. Pembaruan Vektor **makan**

Dimensi pertama vektor "makan":
- $$vektor \, makan^{baru}=0.395−(0.1×0.05)=0.39$$

Dimensi kedua vektor "makan":
- $$vektor \, makan^{baru}=0.495−(0.1×0.05)=0.49$$

Dimensi ketiga vektor "makan":
- $$vektor \, makan^{baru}=0.595−(0.1×0.05)=0.59$$

Jadi, setelah memperbarui vektor embedding untuk "makan", vektor "makan" **didapatkan hasil berikut :[0.39,0.49,0.59]**

2. Pembaruan vector **yanto**

Dimensi pertama vektor "yanto":
- $$vektor \, yanto^{baru}=0.095−(0.1×0.05)=0.09$$

Dimensi kedua vektor "yanto":
- $$vektor \, yanto^{baru}=0.195−(0.1×0.05)=0.19$$

Dimensi ketiga vektor "yanto":
- $$vektor \, yanto^{baru}=0.295−(0.1×0.05)=0.29$$

Setelah perhitungan untuk pembaruan selesai, didapatkan **hasil vector embedding Yanto: [0.09,0.19,0.29]**

3. Pembaruan Vektor **sate**

Dimensi pertama vektor "sate":
- $$vektor \, sate^{baru}=0.695−(0.1×0.05)=0.69$$

Dimensi kedua vektor "sate":
- $$vektor \, sate^{baru}=0.795−(0.1×0.05)=0.79$$

Dimensi ketiga vektor "sate":
- $$vektor \, sate^{baru}=0.895−(0.1×0.05)=0.89$$

Setelah perhitungan untuk pembaruan selesai, didapatkan **hasil vector embedding "sate" menjadi: [0.69,0.79,0.89]**

### Rekapitulasi Vektor
Embedding Setelah Iterasi Ketiga
Setelah iterasi ketiga, berikut adalah vektor embedding yang diperbarui untuk kata "makan", "yanto", dan "sate":

* **Vektor "makan": [0.39,0.49,0.59]**
* **Vektor "yanto": [0.09,0.19,0.29]**
* **Vektor "sate": [0.69,0.79,0.89]**

Kesimpulan
Pada iterasi ketiga, telah dilakukan pembaruan vektor embedding untuk kata **"makan", "yanto", dan "sate"** dengan menggunakan proses backpropagation yang didasarkan pada gradien error dari iterasi sebelumnya