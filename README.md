# Membuat Drawing Board dengan ILI9341 LCD Touchscreen

## Deskripsi Project
Membuat Drawing Board Interaktif dengan LCD Touchscreen ILI9341

## Tujuan 
Proyek ini bertujuan untuk menciptakan sebuah papan gambar digital yang interaktif menggunakan layar LCD touchscreen ILI9341 dan pengontrol sentuh kapasitif FT6206. Dengan proyek ini, pengguna dapat menggambar di layar menggunakan jari mereka dan memilih berbagai warna untuk menggambar.

## Alat dan Bahan
1. Arduino Uno
2. Modul LCD Touchscreen ILI9341
3. Pushbutton

## Gambar Rangkaian
![alt text](https://github.com/kevinhardiansites/arduinoproject3/blob/main/Daftar%20Gambar/Gambar%20Rangkaian.png?raw=true)

Link Simulasi: https://wokwi.com/projects/399602176504280065

## Penjelasan Program
Library yang digunakan
```cpp
#include <Adafruit_GFX.h>    
#include <SPI.h>       
#include <Adafruit_ILI9341.h>
#include <Wire.h>      
#include <Adafruit_FT6206.h>
```

Mendeklarasikan objek ctp dari kelas Adafruit_FT6206 untuk mengontrol layar sentuh kapasitif.
```cpp
Adafruit_FT6206 ctp = Adafruit_FT6206();
```

Mendefinisikan pin yang digunakan untuk kontrol SPI pada layar ILI9341.
```cpp
#define TFT_CS 10
#define TFT_DC 9
Adafruit_ILI9341 tft = Adafruit_ILI9341(TFT_CS, TFT_DC);
```

Mendefiniskan ukuran kotak warna (BOXSIZE), raduis kuas (PENRADIUS) dan mendeklarasikan variable untuk menyimpan warna sebelumnya (oldcolor) dan warna saat ini (currentcolor).
```cpp
#define BOXSIZE 40
#define PENRADIUS 3
int oldcolor, currentcolor;
```

Fungsi setup untuk inisialisasi
```cpp
void setup(void) {
  while (!Serial);     //
  Serial.begin(115200); 
  Serial.println(F("Drawing Board Paint"));
  
  tft.begin();
```

```cpp
//Memulai layar sentuh kapasitif dengan sensitivitas 10.
  if (! ctp.begin(10)) { 
    Serial.println("Couldn't start FT6206 touchscreen controller"); //Jika gagal, mencetak pesan error
    while (1);
  }

  Serial.println("Capacitive touchscreen started"); // Jika berhasil, mencetak pesan "Capacitive touchscreen started".
```

```cpp
  tft.fillScreen(ILI9341_BLACK); // Mengisi layar dengan warna hitam.
```

```cpp
// Membuat kotak-kotak opsi warna kuas
  tft.fillRect(0, 0, BOXSIZE, BOXSIZE, ILI9341_RED);
  tft.fillRect(BOXSIZE, 0, BOXSIZE, BOXSIZE, ILI9341_YELLOW);
  tft.fillRect(BOXSIZE*2, 0, BOXSIZE, BOXSIZE, ILI9341_GREEN);
  tft.fillRect(BOXSIZE*3, 0, BOXSIZE, BOXSIZE, ILI9341_CYAN);
  tft.fillRect(BOXSIZE*4, 0, BOXSIZE, BOXSIZE, ILI9341_BLUE);
  tft.fillRect(BOXSIZE*5, 0, BOXSIZE, BOXSIZE, ILI9341_MAGENTA);
```

```cpp
// Memberi sorotan putih pada kotak warna kuas yang diigunakan.
  tft.drawRect(0, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
  currentcolor = ILI9341_RED;
}
```

```cpp
// Fungsi loop akan dijalankan terus-meneurus
void loop() {
  if (! ctp.touched()) {
    return;
  }
// Jika layar tidak disentuh akan keluar dari kondisi loop
```

```cpp
  TS_Point p = ctp.getPoint(); //Mendapatkan titik sentuh dari layar sentuh kapasitif dan menyimpannya dalam variabel p.
```

Mencetak koordinat X dan Y dari titik sentuh.
```cpp
  Serial.print("X = "); Serial.print(p.x);
  Serial.print("\tY = "); Serial.print(p.y);
  Serial.print(" -> ");
```

Membalik koordinat untuk menyesuaikan dengan orientasi layar pada serial monitor
```cpp
  p.x = map(p.x, 0, 240, 240, 0);
  p.y = map(p.y, 0, 320, 320, 0);
```
Jika titik sentuh berada di dalam area kotak warna (y kurang dari BOXSIZE), menyimpan warna saat ini ke dalam oldcolor.
```cpp
  if (p.y < BOXSIZE) {
     oldcolor = currentcolor;
```

Mengecek kotak warna mana yang disentuh dan memperbarui currentcolor serta menggambar garis putih di sekitar kotak warna yang baru dipilih.
```cpp
     if (p.x < BOXSIZE) { 
       currentcolor = ILI9341_RED; 
       tft.drawRect(0, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     } else if (p.x < BOXSIZE*2) {
       currentcolor = ILI9341_YELLOW;
       tft.drawRect(BOXSIZE, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     } else if (p.x < BOXSIZE*3) {
       currentcolor = ILI9341_GREEN;
       tft.drawRect(BOXSIZE*2, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     } else if (p.x < BOXSIZE*4) {
       currentcolor = ILI9341_CYAN;
       tft.drawRect(BOXSIZE*3, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     } else if (p.x < BOXSIZE*5) {
       currentcolor = ILI9341_BLUE;
       tft.drawRect(BOXSIZE*4, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     } else if (p.x <= BOXSIZE*6) {
       currentcolor = ILI9341_MAGENTA;
       tft.drawRect(BOXSIZE*5, 0, BOXSIZE, BOXSIZE, ILI9341_WHITE);
     }
```

Jika warna saat ini berbeda dari warna sebelumnya, mengisi kembali kotak warna sebelumnya untuk menghapus garis putih di sekitarnya.
```cpp
     if (oldcolor != currentcolor) {
        if (oldcolor == ILI9341_RED) 
          tft.fillRect(0, 0, BOXSIZE, BOXSIZE, ILI9341_RED);
        if (oldcolor == ILI9341_YELLOW) 
          tft.fillRect(BOXSIZE, 0, BOXSIZE, BOXSIZE, ILI9341_YELLOW);
        if (oldcolor == ILI9341_GREEN) 
          tft.fillRect(BOXSIZE*2, 0, BOXSIZE, BOXSIZE, ILI9341_GREEN);
        if (oldcolor == ILI9341_CYAN) 
          tft.fillRect(BOXSIZE*3, 0, BOXSIZE, BOXSIZE, ILI9341_CYAN);
        if (oldcolor == ILI9341_BLUE) 
          tft.fillRect(BOXSIZE*4, 0, BOXSIZE, BOXSIZE, ILI9341_BLUE);
        if (oldcolor == ILI9341_MAGENTA) 
          tft.fillRect(BOXSIZE*5, 0, BOXSIZE, BOXSIZE, ILI9341_MAGENTA);
     }
```

Jika titik sentuh berada di bawah kotak warna dan masih dalam batas layar, menggambar lingkaran kecil dengan warna saat ini (currentcolor) di posisi yang disentuh.
```cpp
if (((p.y-PENRADIUS) > BOXSIZE) && ((p.y+PENRADIUS) < tft.height())) {
    tft.fillCircle(p.x, p.y, PENRADIUS, currentcolor);
  }
}
```
