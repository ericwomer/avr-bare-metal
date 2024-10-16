# Bare Metal AVR Programming Project

(Arduino Uno R3)

## Configure/Build/Push (upload/burn)

#### Confgure

```bash
  meson setup --cross-files profiles/atmega328p.ini build
```

#### Compile the elf binary

```bash
  meson compile -C build
```

#### Generate Intel Hex File

```bash
  meson compile -C build hex
```

#### Push (upload/burn) To The Device

```bash
  meson compile -C build push
```

Project Code. [^1]  
Meson Build System. [^2]  

[^1]: Code was taken from [Low Level](https://www.youtube.com/watch?v=5HgQkHzQc3o) on youtube  
[^2]: Elements were taken from [jpakkane/mesonarduino](https://github.com/jpakkane/mesonarduino/tree/master) as a template  
