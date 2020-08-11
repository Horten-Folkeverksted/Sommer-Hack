# Hjemmeautomasjon

Presentert av Robin Smidsrød 2020-08-08 kl. 16:00.

Programmene som ble demonstrert var [Home
Assistant](https://home-assistant.io) og [Node-RED](https://nodered.org).

## Bruh Multisensor

Den hjemmelagde sensoren er en [Bruh
Multisensor](https://www.youtube.com/watch?v=jpjfVc-9IrQ) med noen
endringer.

Bruh Multisensor komponentliste:
* [Female-to-Female Jumper Wires (40x)](https://www.aliexpress.com/item/40pcs-lot-10cm-2-54mm-1pin-Female-to-Female-jumper-wire-Dupont-cable/32378478740.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [RGB LED (10x)](https://www.aliexpress.com/item/Led-Light-Lighting-diodes-Diode-10pc-RGB-common-cathode-5mm-RGB-LED-Common-Cathode-4-Pin/32606955472.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [Infrared PIR Motion Sensor](https://www.aliexpress.com/item/Mini-IR-Pyroelectric-Infrared-PIR-Motion-Human-Sensor-Automatic-Detector-Module-high-reliability-12mm-x-25mm/32749737125.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [DHT22 Temperature Sensor](https://www.aliexpress.com/item/Digital-Temperature-and-Humidity-Sensor-DHT11-DHT22-AM2302-AM2301-AM2320-sensor-and-module-For-Arduino-electronic/32769460765.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [NodeMCU v3 (ESP8266)](https://www.aliexpress.com/item/Wireless-module-CH340-CP2102-NodeMcu-V3-V2-Lua-WIFI-Internet-of-Things-development-board-based-ESP8266/32656401198.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [KY-018 Photoresistor](https://www.aliexpress.com/item/KY-018-photosensitive-Optical-Sensitive-Resistance-Light-module-detects-resistor-module-for-arduino-diy-kit-sensor/32701608104.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [USB Charger](https://www.aliexpress.com/item/Crouch-Universal-Fast-USB-Charger-EU-US-UK-Plug-Travel-Wall-Mobile-Phone-Charger-Adapter-For/33013018599.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)
* [USB Cable (3M)](https://www.aliexpress.com/item/3M-Long-Android-Charger-Cable-Fast-Charge-USB-to-Micro-USB-Cable-White-Micro-USB-2/33047057270.html?spm=a2g0s.9042311.0.0.4b844c4dS3K8bx)

Den benytter [ESPHome](https://esphome.io/), med en konfigurasjon som er
nesten slik som beskrevet her: [ESPHome Bruh Multisensor
Cookbook](https://esphome.io/cookbook/bruh.html)

Det er gjort noen endringer fra cookbook nevnt over med funksjonen for å
beregne lux-verdi fra fotoresistoren, og det finner du beskrevet i den
vedlagte filen.

Den benytter også et 3D-printet enclosure, som ligger vedlagt. Jeg har også
lagt med 3D-modell til noen små plastplugger som brukes for å feste NodeMCU
inni boksen. Disse er ikke absolutt nødvendige, men hendige.

## Shelly WiFi-komponenter

Diverse Shelly-komponenter ble nevnt som du finner informasjon om her:

* [Shelly 1PM (16A)](https://shelly.cloud/products/shelly-1pm-smart-home-automation-relay/)
* [Shelly Button](https://shelly.cloud/products/shelly-button-smart-home-automation-device/)
* [Shelly Temperature Addon](https://shop.shelly.cloud/temperature-sensor-addon-for-shelly-1-1pm-wifi-smart-home-automation)
* [Shelly Plug (16A)](https://shelly.cloud/products/shelly-plug-smart-home-automation-device/)
* [Shelly Plug S (10A)](https://shelly.cloud/products/shelly-plug-s-smart-home-automation-device/)

For å styre varmtvannsbereder benytter jeg en Shelly 1PM med en Temperature
Addon og en DS18B20 temperatursensor (kjøpes sammen med Temperature Addon).
Temperatursensoren er teipet fast med gaffateip på innsiden av isolasjonen
nederst i berederen. Dette måler temperaturen på det kaldeste vannet i
tanken, og gir et godt utgangspunkt for å lage en automasjon for å unngå
legionella hvis vannet er under 70 grader i mer enn en uke.

Shelly Button sammen med en Shelly 1 eller Shelly 1PM egner seg veldig godt
til å styre f.eks. lamper med vanlige skyvebrytere. Klipp kabelen og koble
til Shelly Button + 1PM så har man en smartlampe.

Shelly Plug og Plug S bruker jeg til å måle strømbruk på f.eks. kjøleskap,
fryser og varmepumper, for å få et helhetlig bilde av hvor mye strøm som
blir brukt i hele huset.

## Dekoding av data fra AMS-HAN-port

Det ble snakket mye om dekoding av AMS HAN port fra norske smartmålere.
Programvaren jeg har skrevet i Perl for å dekode MBUS-signalene finner du
her: https://github.com/robinsmidsrod/ams-han-decoder

For å konvertere signalene til seriell benytter jeg en [PL2303-basert
MBUS-til-USB-slave
(FC722)](https://www.aliexpress.com/item/32719562958.html?spm=a2g0s.9042311.0.0.27424c4dIAuZla)
som kan handles her. Pass på at du ikke kjøper den basert på TSS721 (se
kommentar i kildekoden), da den ikke klarer å håndtere store MBUS-meldinger
og vil gi sjekksumfeil under dekoding.
