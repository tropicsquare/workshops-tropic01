# Installfest 2026 TROPIC01 workshop (CZ only)

Cílem tohoto workshopu je ukázat základy práce s čipem TROPIC01 na Arduino shield připojený k desce ESP32-DevKitC-V4 sloužící jako Hostovský procesor.

Budeme používat ESP-IDF a SDK pro Host FW k čipu TROPIC01 zvané Libtropic.

Ukázkové příklady jsou napsány v jazyce C (C++). V současné době je ve vývoji také [knihovna v jazyce Rust](https://github.com/tropicsquare/libtropic-rs).

# Příprava

Následujte tyto [kroky](https://tropicsquare.github.io/libtropic/latest/tutorials/esp32/):

* Propojte DevKity
* Nainstalujte (pokud nemáte) ESP IDF, ideálně verze 5.x, přičemž stačí instalace pro čipy esp32
* Naklonujte repozitář libtropic
* Spusťte postupně ukázkové příklady

# Propojte DevKity

Pozor na zrádnou tabulku v hlavní dokumentaci, kde řádky nejsou ve stejném pořadí jako piny na obrázku. Zde je to v pořádku.

![Arduino shield](./arduino-shield-pinout.svg)

| TROPIC01 Arduino Shield Pin | ESP32-DevKitC-V4 Pin |
|:---------------------------:|:--------------------:|
| IOREF                       | 3V3                  |
| +3V3                        | 3V3                  |
| GND                         | GND                  |
| SCK                         | GPIO18               |
| MISO                        | GPIO19               |
| MOSI                        | GPIO23               |
| CS                          | GPIO5                |
| GPO                         | GPIO32               |


# Instalace prostředí

Proveďte kroky 1-3 z [ruční instalace ESP-IDF verze 5.5.3](https://docs.espressif.com/projects/esp-idf/en/v5.5.3/esp32/get-started/linux-macos-setup.html#get-started-get-esp-idf).

Pokud používáte bash (a pro případ instalace závislostí je Vaše distribuce Ubuntu nebo Debian), můžete postupovat přímo takto: 

```
sudo apt-get install git wget flex bison gperf python3 python3-pip python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

```
mkdir -p ~/esp
cd ~/esp
git clone -b v5.5.3 --recursive https://github.com/espressif/esp-idf.git
```

V případě, že pracujete na školních počítačích, klonujte takto:
```
git clone -b v5.5.3 --recursive https://github.com/espressif/esp-idf.git
```

## Mezitím se pobavíme o tom, co to je secure element a konkrétně TROPIC01 a co nabízí...

* Vzájemná autentizace
* Bezpečné úložiště dat a klíčů
* Generování klíčů
* Podepisování
* TRNG a PUF
* MAC And Destroy (ověření PINu)
* To všechno s ochranami proti útokům postranními kanály a vkládáním chyb

## Pokračujeme v instalaci...

```
cd ~/esp/esp-idf
./install.sh esp32
```

# Naklonování a použití libtropic

Naklonujte knihovnu [libtropic](https://github.com/tropicsquare/libtropic). Do domovského adresáře takto:

```
cd ~
git clone https://github.com/tropicsquare/libtropic.git
```


Přejděte do složky s prvním příkladem pro ESP32-DevKitC-V4, spusťte skript pro zviditelnění ESP-IDF a poté zkompilujte a spusťte první příklad.

```
cd ./libtropic/examples/esp32/ESP32-DevKitC-V4/identify_chip/
. $HOME/esp/esp-idf/export.sh
idf.py build flash monitor
```

Poté vyzkoušejte i další příklady, kde updatujete FW čipu TROPIC01 a navážete Secure session nutnou k provádění pokročilejších příkazů.

```
cd ../fw_update/
. $HOME/esp/esp-idf/export.sh
idf.py build flash monitor
```

```
cd ../hello_world/
. $HOME/esp/esp-idf/export.sh
idf.py build flash monitor
```

# TROPIC01 Manager


# Support

Jsme tu, abychom napravili průmysl a rádi odpovídáme na Vaše dotazy!

* **Support Desk**: [support.desk.tropicsquare.com](https://support.desk.tropicsquare.com)
* **Discord**: Funguje v angličtině, ale v DM klidně přepneme do češtiny. [Discord](https://discord.com/invite/d4wNpvXskM)
* **GitHub Issues**: Otevřte issue na našem GitHubu
* **Workshop**: Ptejte se kdykoli během něj

