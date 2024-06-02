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
LIbrary yang digunakan 
```cpp
#include <Adafruit_GFX.h>    
#include <SPI.h>       
#include <Adafruit_ILI9341.h>
#include <Wire.h>      
#include <Adafruit_FT6206.h>
```
