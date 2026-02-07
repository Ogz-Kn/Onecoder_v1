# nice!nano nRF52840 + AS5600 (ZMK, tuşsuz kullanım)

Bu klasör, **nice!nano (nRF52840)** için ZMK konfigürasyon şablonudur.
Hedef: fiziksel klavye switch'i olmadan AS5600 ile dönme girdisi almak.

## Neden "dummy" kscan var?

ZMK pratikte bir `kscan` tanımı beklediği için, tek pinli bir **suni (dummy) kscan** eklendi.
Bu pin fiziksel switch'e bağlı olmak zorunda değil; sadece firmware'in beklentisini karşılar.

- Dummy tuş: `F24`
- İsterseniz `&none` benzeri davranışla değiştirebilirsiniz; güvenli test için `F24` bırakıldı.

## Dosyalar

- `config/nice_nano_v2.overlay`
  - `zmk,kscan` için dummy giriş
  - `i2c0` altında `as5600@36`
- `config/nice_nano_v2.keymap`
  - 1 adet dummy binding (`&kp F24`)
  - sensör dönüşü için `&inc_dec_kp RIGHT LEFT`

## Bağlantı

AS5600:
- VCC -> 3V3
- GND -> GND
- SDA -> overlay'de verdiğiniz `sda-pin`
- SCL -> overlay'de verdiğiniz `scl-pin`

> `sda-pin` / `scl-pin` değerlerini kendi nice!nano pin planınıza göre mutlaka güncelleyin.

## ZMK konfigürasyon reposuna taşıma

Bu repo bir örnek içerik sağlar. Kendi `zmk-config` deponuzda:

1. `config/nice_nano_v2.overlay` dosyanızı bu örnekle birleştirin.
2. `config/nice_nano_v2.keymap` içine dummy key + sensor binding ekleyin.
3. Build target olarak `nice_nano_v2` seçip derleyin.

## Önemli not

`compatible = "ams,as5600"` satırı, kullandığınız ZMK/Zephyr sürümünde hazır driver yoksa
tek başına yeterli olmayabilir. Bu durumda ek sensor driver/modül eklemek gerekir.
Bu şablon, **nRF52840 + ZMK + kscan dummy** mimarisini netleştirmek için hazırlanmıştır.
