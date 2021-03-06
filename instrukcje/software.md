# Smogomierz - instalacja oprogramowania

Poniższy opis krok po kroku, przeprowadzi Cię przez procedurę instalacji oprogramowania wymaganego do wgrania nowego systemu na Twój mierniki smogu. Całe oprogramowanie jest w pełni darmowe, a jego pobranie nie wymaga żadnej rejestracji i podawania swoich danych. Poniższy opis jest pomocny jeśli chcesz samemu skompilować oprogramowanie dla Smogomierza. Jeśli chcesz użyć gotowej, przygotowanej przez nas paczki, zajrzyj do opisu ["Wgrywanie pliku .bin z oprogramowaniem"](https://github.com/hackerspace-silesia/Smogomierz/blob/master/instrukcje/software-bin.md)

## Arduino - instalacja

Na początku musisz pobrać program ArduinoIDE. Pozwala on na programowanie płytek z rodziny Arduino. W późniejszym kroku, dodamy do niego obsługę wybranej przez nas płytki ESP(ESP32 lub ESP8266). To właśnie na niej oparty jest nasz mierniki smogu.

1. Pobieramy oprogramowanie ze strony https://www.arduino.cc/en/Main/Software. W tym celu musimy wybrać jaki system operacyjny mamy zainstalowany na komputerze, a następnie kliknąć na napis "Download the Arduino IDE". W przypadku korzystania z systemu Windows, zalecamy wybranie wersji "Installer, for Windows XP and up"
![ArduinoIDE1](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE1.jpg)

2. Klikamy napis "JUST DOWNLOAD". W tym momencie rozpocznie się pobieranie instalatora ArduinoIDE.
![ArduinoIDE2](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE2.jpg)

3. Instalujemy pobrany program. W przypadku Windowsa klikamy na niego 2 razy i instalujemy. W systemie macOS przenosimy do foldery Aplikacje. Na Linuksie uruchamiamy skrypt install.sh.

## Arduino - konfiguracja

Po zainstalowaniu ArduinoIDE, musisz dodać w nim obsługę płytek ESP.

1. Uruchamiamy aplikację ArduinoIDE i wybieramy "Plik > Preferencje" lub "Arduino > Preferences…" w przypadku systemu macOS.
![ArduinoIDE3_1](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE3_1.jpg)

2. W okienku „Dodatkowe adresy URL do menedżera płytek:” wpisujemy adres: http://arduino.esp8266.com/stable/package_esp8266com_index.json a linijkę niżej https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
![ArduinoIDE3_2](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE3_2.jpg)

3. Po dodaniu adresu, skąd program może pobrać informacje o nowych płytkach, musimy je jeszcze pobrać. W tym celu klikamy "Narzędzia > Płytka > Menedżer płytek…".
![ArduinoIDE3_3](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE3_3.jpg)

4. W wyszukiwarce wpisujemy frazę: "esp8266" lub "esp32"(w zależności od tego jakiej wersji ESP użyliśmy do budowy Smogomierza) i instalujemy nowe płytki.
![ArduinoIDE4](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE4.jpg)

5. Teraz pozostaje nam tylko ustawić w ArduinoIDE, że nasz kod ma być dostosowany do płytek ESP8266, a nie standardowego Arduino. W tym celu musimy wybrać "Narzędzia > Płytka > NodeMCU 1.0 (ESP-12E Module)". W przypadku ESP32, wybieramy płytkę "Narzędzia > Płytka > ESP32 Dev Module"
![ArduinoIDE5](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE5.jpg)

6. Na koniec pozostaje nam wybranie portu szeregowego, go którego podłączona jest nasza płytka. W tym celu podpinamy ESP do portu USB w komputerze i wybieramy "Narzędzia > Port > COM3". W przypadku systemu innego niż Windows, zamiast "COM3" będziemy mieć inną opcję do wyboru np. "/dev/cu.wchusbserial1410". Jeśli pomimo podłączenia płytki do portu USB, na liście nie ma żadnej pozycji do wyboru, musimy jeszcze doinstalować sterownik odpowiedni sterownik. NodeMCu V3 korzysta z układy **CH340**. Sterowniki do niego można pobrać [tutaj](https://sparks.gogo.co.nz/ch340.html)(W przypadku systemu macOS, zalecamy instalację wersji 1.5 lub nowszej dostępnej [tutaj](https://github.com/adrianmihalko/ch340g-ch34g-ch34x-mac-os-x-driver)). Istnieje jednak możliwość, że Twoja płytka ma układ **CP2102**. Sterowniki do niego dostępnej są [tutaj](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers). Jeśli nie jesteś pewien jaki układ znajduje się na Twojej płytce ESP, zobacz informacje na spodzie płytki. Powinno być tam to napisane. Po pobraniu i instalacji odpowiednich sterowników, powinniśmy uruchomić ponownie komputer. Arduino powinno widzieć już nasza płytkę podłączoną do portu szeregowego.
![ArduinoIDE6](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE6.jpg)

7. Smogomierz zapisuje w pamięci wewnętrznej(SPIFFS) płytki ESP ustawienia. Robi to, aby po nonownym uruchomienie nie było konieczne wpisywanie wszystkich danych na nowo. Musimy się upewnić, że w ArduinoIDE wybraliśmy opcję stworzenia przestrzeni w pamięci na nasz plik konfiguracyjny. W tym celu dla ESP8266 musimy zaznaczyć opcję **4M (FS:1M OTA: ~1019KB)** z menu "Narzędzia > Flash Size > 4M (FS:1M OTA: ~1019KB)". Natomiast w przupadku ESP32, opcję **Minimal SPIFFS (1.9MB APP with OTA/190KB SPIFFS)** z menu "Narzędzia > Partition Scheme".
![ArduinoIDE7](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE7_2.jpg)

8. Kod, który będziemy wgrywać na płytki ESP wymaga do poprawnego działania, dwóch zewnętrznych bibliotek. Aby je dodać wystarczy wybrać z menu ArduinoIDE "Szkic > Dołącz bibliotekę > Zarządzaj bibliotekami...".
![ArduinoIDE8](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE8.jpg)

9. W wyszukiwarce wpisujemy nazwę biblioteki i następnie ją instalujemy. Biblioteki, które musimy doinstalować to: **ArduinoJson(w wersji 6.5.0 lub nowszej)** oraz najnowsze wersje bibliotek: **Adafruit BMP280 Library**, **DHT sensor library by Adafruit**, **ThingSpeak**, **Adafruit Unified Sensor** oraz **PubSubClient**.
![ArduinoIDE10](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE10.jpg)

10. Musimy jeszcze doinstalować kilka bibliotek, poprzed dodanie plików .zip. Tymi bibliotekami są: [ESPAsyncWebServer](https://github.com/me-no-dev/ESPAsyncWebServer) oraz [ESPAsyncTCP](https://github.com/me-no-dev/ESPAsyncTCP). W przypadku ESP32 musimy doinstalować jeszcze [AsyncTCP](https://github.com/me-no-dev/AsyncTCP). W przypadku ESP32 musimy również doinstalować Pythona3 oraz [pyserial](https://pypi.org/project/pyserial/).

11. Teraz wszystko jest już gotowe do obsługi naszych mierników smogu. Możemy przejść do pobrania oprogramowania i sprawdzenia czy działa.

## Smogomierz - pobranie i wgranie oprogramowania na płytkę ESP

1. Wchodzimy na stronę projektu Smogomierz w serwisie github i pobieramy pliki: https://github.com/hackerspace-silesia/Smogomierz/
![Github1](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/Github1.jpg)

2. Rozpakowujemy pobraną paczkę. Zmieniamy nazwę folderu ze "Smogomierz-master" na "Smogomierz" i otwieramy plik Smogomierz.ino, który znajduje się w środku.

3. Otworzy się nam okienko programu ArduinoIDE z kodem naszej aplikacji Smogomierza.  
![ArduinoIDE7](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE7.jpg)

1 – Sprawdzenie poprawności oprogramowania

2 – Wgranie oprogramowania na płytkę

3 – Postęp weryfikacji/wgrywania oprogramowania i ewentualne błędy

4 – Płytka oraz port, które mamy ustawione

4. Zanim wgramy nasz kod na płytkę ESP, możemy w nim ustawić kilka wartości. Wśród nich np. miejsce, gdzie będzie znajdował się nasz miernik. W tym celu musimy otworzyć plik defaultConfig.h. Wszystkie ustawenia można w dowolnej chwili modyfikować przez przeglądarkę internetową, dlatego zmiana czegokolwiek w defaultConfig.h jest opcją dodatkową.

5. Wartości, które możemy zmienić, to szerokość(LATITUDE) i długość(LONGITUDE) geograficzna oraz wysokość na jakiej będzie znajdował się nasz miernik(MYALTITUDE). Wszystkie te dane możemy uzyskać na stronie https://www.wspolrzedne-gps.pl. Wystarczy tam wpisać adres, gdzie miernik zostanie umieszczony. Obecnie wpisane dane, to współrzędne lokalu Hackerspace Silesia. Długość i Szerokość geograficzna jest wymagana, aby nasze dane pojawiły się we właściwym miejscu w serwisie [mapa.airmonitor.pl](http://mapa.airmonitor.pl). Dodatkowo musimy wypełnić [formularz](https://docs.google.com/forms/d/e/1FAIpQLSdw72_DggyrK7xnSQ1nR11Y-YK4FYWk_MF9QbecpOERql-T2w/viewform), aby punkt z naszymi współrzędnymi pojawił się na mapie(Model sensora: PMS7003).
![ArduinoIDE12](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE12.jpg)

6. Miernik pozwala na wysyłanie danych do serwisu [ThingSpeak](https://thingspeak.com). ThingSpeak wymaga rejestracji i założenia darmowego konta. Pozwoli to na przesyłanie naszych pomiarów do tego serwisu i uzyskanie wykresów zanieczyszczeń oraz danych o temperaturze, wilgotności i ciśnieniu powietrza. Opis rejestracji i konfiguracji kanału w serwisie ThingSpeak znajdziecie [TUTAJ](https://github.com/hackerspace-silesia/Smogomierz/blob/master/instrukcje/thingspeak.md). W celu dodania obsługi serwisu ThingSpeak, w pliku defaultConfig.h musimy zmienić trzy parametry: Wpisać odpowiednie dane do THINGSPEAK_API_KEY(parametr "Key" ze strony ThingSpeak) oraz THINGSPEAK_CHANNEL_ID(parametr "Channel ID"), a w THINGSPEAK_ON zmienić słowo "false" na "true".
![ArduinoIDE13](https://raw.githubusercontent.com/hackerspace-silesia/Smogomierz/master/instrukcje/screens/ArduinoIDE13.jpg)

7.  Miernik jest również w stanie wysyłać dane do serwisu [aqi.eco](https://aqi.eco). Instrukcja konfiguracji aqi.eco znajduje się na [osobnej stronie](aqi-eco.md).

8. Teraz możemy zweryfikować nasz kod(Punkt 3., podpunkt 1.). Jeśli wszystko poprawnie się skompiluje możemy przejść do składania fizycznych części naszego miernika smogu. Dokładny opis w [Instrukcja podłączenia elektroniki](https://github.com/hackerspace-silesia/Smogomierz/blob/master/instrukcje/hardware.md).

9.  Wszystkie wartości podane w pliku defaultConfig.h są domyślnymi wartościami potrzebnymi przy pierwszym uruchomieniu miernika. Każdą z tych pozycji można w dowolnej chwili zmienić w zakładce Ustawienia w panelu Smogomierza.
