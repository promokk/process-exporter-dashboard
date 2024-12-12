# Process-exporter-dashboard
Дашборд Grafana для мониторинга процессов Linux.  
Для работы дашборда используется Prometheus, [Process-exporter](https://github.com/ncabatoff/process-exporter)
и [Node-exporter](https://github.com/prometheus/node_exporter).  
Node-exporter необходим для панелей CPU.

Дашборд доступен в [Grafana Labs](https://grafana.com/grafana/dashboards/22161-process-exporter-dashboard/).

---
# Оглавление
* [Описание дашборда](#dashboardDescription)
* [Настройка Prometheus, Process-exporter и Node-exporter](#settings)

---
## Описание дашборда <a id="dashboardDescription"></a>
Дашборд позволяет отслеживать процессы в Linux.  
Список метрик:
* CPU % / CPU (millicore) - утилизация CPU процессом.  
* Resident Memory % / Resident Memory - фактическое использование памяти процессом.
  ![Summary - картинка](https://raw.githubusercontent.com/promokk/process-exporter-dashboard/main/data/CPU_Memory.png)
* Virtual Memory - общий объем памяти, который может потребоваться процессу.  
* Number Threads - количество потоков.  
* Voluntary Context Switches - добровольное переключение контекста.  
  Происходит, когда поток выполняет запись в IO или запрашивает ресурс, который недоступен.  
* Nonvoluntary Context Switches - непроизвольное переключение контекста.  
  Происходит, когда поток превышает временной интервал, назначенный планировщиком.
  ![Summary - картинка](https://raw.githubusercontent.com/promokk/process-exporter-dashboard/main/data/Context.png)
* Group File Descriptors - количество открытых файловых дескрипторов для группы.  
* Total File Descriptors - 
  количество открытых файловых дескрипторов / максимальное количество открытых файловых дескрипторов.  
* Read Bytes - количество байтов, прочитанных группой.  
* Write Bytes - количество байтов, записанных группой.
  ![Summary - картинка](https://raw.githubusercontent.com/promokk/process-exporter-dashboard/main/data/File_Descriptors.png)
* Minor Page Faults - незначительные ошибки страницы памяти.  
* Major Page Faults - основные ошибки страницы памяти.  
* Number Processes - количество процессов в группе.
  ![Summary - картинка](https://raw.githubusercontent.com/promokk/process-exporter-dashboard/main/data/Page_Faults.png)

---
Настройка Prometheus, Process-exporter и Node-exporter <a id="settings"></a>
1. Установить и настроить [Prometheus](https://prometheus.io/docs/prometheus/latest/getting_started/)  
   Установить на Linux можно с помощью bash-скрипта -->
   [install-prometheus.sh](https://github.com/promokk/bash-scripts/blob/main/install-scripts/install-prometheus.sh)
    * Добавить новый job в файл конфигурации prometheus /etc/prometheus/prometheus.yml.  
      Вместо {host} необходимо указать свои сервера.

~~~shell
  # prometheus.yml
  scrape_configs:
    - job_name: 'process-exporter'
      scrape_interval: 10s
      static_configs:
        - targets: [
          '{host}:9256',
          '{host}:9256'
        ]
        
    - job_name: 'node_exporter'
      scrape_interval: 10s
      static_configs:
        - targets: [
          '{host}:9100',
          '{host}:9100'
          ]
 ~~~

2. Установить и настроить [Process-exporter](https://github.com/ncabatoff/process-exporter?tab=readme-ov-file)  
   Установить на Linux можно с помощью bash-скрипта -->
   [install-process-exporter.sh](https://github.com/promokk/bash-scripts/blob/main/install-scripts/install-process-exporter.sh)
3. Установить и настроить [Node-exporter](https://github.com/prometheus/node_exporter)  
   Установить на Linux можно с помощью bash-скрипта -->
   [install-node-exporter.sh](https://github.com/promokk/bash-scripts/blob/main/install-scripts/install-node-exporter.sh)
