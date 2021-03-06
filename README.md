# look up! - monitorowanie pokrywy chmur
Projekt detektora chmur oparty o sieć czujników IoT w ramach przedmiotu PBL-3.


Zespół: Michał Ciesielski, Karol Duszczyk, Kamil Kmieć\
Opiekun: mgr inż. Marcin Kołakowski

## O projekcie
Celem projektu jest stworzenie rozproszonego systemu IoT umożliwiającego ocenę stopnia zachmurzenia nieba na danym obszarze. Obecnie taka ocena pokrywy chmur jest dokonywana poprzez analizę zdjęć satelitarnych lub wykorzystanie zaawansowanych czujników laserowych. Zdjęcia satelitarne wydają się wystarczającym źródłem informacji dla działań związanych z prognozowaniem pogody, jednak są niewystarczająco szczegółowe, aby spełnić wymagania osób zajmujących się zawodowo lub hobbystycznie obserwacjami astronomicznymi. Duża dokładność pomiarów dokonanych na obszarze prowadzenia takich obserwacji jest kwestią istotną, ponieważ nawet niewielkie zachmurzenie może skutecznie uniemożliwić prawidłowe ich wykonanie. Problem ten mogłoby rozwiązać opracowanie sieci czujników zbierających dane o aktualnie panujących warunkach pogodowych na danym obszarze połączonej z aplikacją prezentującą użytkownikowi interesujące go informacje w przystępnej formie.

W projekcie wykorzystywane są technologie, narzędzia i języki: Python, LoRaWAN (The Things Stack), MQTT (Paho), InfluxDB, Grafana.

Szczegółowe informacje można znaleźć w dokumentacji projektu.

![image](https://user-images.githubusercontent.com/43970304/151995187-c69b519c-d302-428d-97f4-19f94b06221f.png)

## Oprogramowanie
W projekcie wykorzystywane jest oprogramowanie dla węzła pomiarowego (sensor_node) i serwera (server).

### sensor_node
- `main.py` - główny plik
- `lora.py` - funkcje odpowiedzialne za obsługę komunikacji LoRaWAN
- `sensors.py` - funkcje odpowiedzialne za obsługę czujników (przykładowe zastosowanie znajduje się w `sensors_example.py`)
- `sensors_test.py` - program wykorzystywany podczas testowania czujników, zapisuje wyniki do katalogu `data`
- `message_format.py` - funkcje do kodowania widomości uplink i rozkodowywania wiadomości downlink wg. formatu opisanego w raporcie 3
- `json_handler.py` - funkcje pomocnicze do obsługi plików json
- `configuration.json` - plik z danymi konfiguracyjnymi
- `requirements.txt` - wymagane zależności
- `node_init.bash` - skrypt instalujący wymagane biblioteki i konfigurujący wstępne ustawienia modułu LoRa-E5 mini (kod nieukończony z braku czasu, ale nie jest wymagany do działania)

## The Things Stack
W pliku `payload_formatter.js` znajduje się formater, którego można użyć w TTS do rozkodowywania wiadomości uplink, aby w konsoli TTS widzieć wartości temperatury (nie trzeba tego robić).

### server
- `main.py` - główny plik
- `mqtt_client.py` - funkcje odpowiedzialne za obsługę MQTT (Paho) i przetwarzanie otrzymanych wiadomości uplink (z braku czasu nie zaimplementowano innych typów wiadomości oraz wysyłanie wiadomości downlink jest zaimplementowane słabo)
- `influx_bridge.py` - funkcje do obsługi bazy danych InfluxDB (inicjowanie połączenia i zapis do bazy; tutaj powinien zostać zaimplementowany też odczyt z bazy, ale nie starczyło na to czasu)
- `influx_add_dev.py` - funkcja do dodawania nowych urządzeń z ich lokalizacją do bazy danych (rozwiązanie tymczasowe)
- `json_handler.py` - funkcje pomocnicze do obsługi plików json
- `signal_handler.py` - funkcje pomocnicze do obsługi signal (po wciśnięciu CTRL+C)
- `configuration.json` - plik z danymi konfiguracyjnymi
- `keys.json` - dane (username i password) do połączenia z klientem MQTT **MUSZĄ BYĆ ZACHOWANE W TAJEMNICY** (dlatego tutaj są tylko przykładowe)
- `requirements.txt` - wymagane zależności
-  `grafana_panel.json` - ustawienia panelu w Grafanie

Dodatkowo na serwerze powinny być zainstalowane i skonfigurowane:
- baza danych InfluxDB. Nazwę bazy, adres ip i dane dostępowe należy umieścić w pliku `influx_bridge.py`. 
- Grafana (narzędzie do wizualizacji danych) z ustawionym panelem zgodnie z plikiem `grafana_panel.json`

> Istalacja i konfiguracja tych narzędzi była przeprowadzana wg. [tej instrukcji](https://diyi0t.com/visualize-mqtt-data-with-influxdb-and-grafana/).

Kod był tworzony tak, aby było go łatwo rozbudowywać o dalsze funkcjonalności. Część rozwiązań nie jest skończona i wymagałaby dopracowania, gdyby projekt był dalej rozwijany.
