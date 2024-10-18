<img src="../dox.png" width="150" height="100" alt="YOMG Lab Documentation">
# DOX - A Computer Scientists NoteBook

## Monitoring tools (Prometheus, Grafana, Datadog, New Relic)
<img src="../image_2024-10-19_071815187.png" alt="YOMG Lab Documentation">

For full source visit [github](https://github.com/youroldmangaming/Grafana-Telegraf-Prometheus-Promtail.git).

<img src="../du0.png" alt="YOMG Lab Documentation">
<img src="../du1.png" alt="YOMG Lab Documentation">
<img src="../du2.png" alt="YOMG Lab Documentation">
<img src="../du4.png" alt="YOMG Lab Documentation">
<img src="../du5.png" alt="YOMG Lab Documentation">
<img src="../du6.png" alt="YOMG Lab Documentation">
<img src="../du7.png" alt="YOMG Lab Documentation">
<img src="../du8.png" alt="YOMG Lab Documentation">
<img src="../du9.png" alt="YOMG Lab Documentation">


### Importance of Monitoring in DevOps

In DevOps, **monitoring** plays a critical role as it provides visibility into the health, performance, and availability of applications and infrastructure. It allows teams to track key performance indicators (KPIs), identify issues early, and ensure that systems are meeting the desired service level agreements (SLAs). Here’s why monitoring is vital in a DevOps environment:

```c
version: "2.1"
services:
  duplicati:
    image: lscr.io/linuxserver/duplicati:latest
    container_name: duplicati
    environment:
      - PUID=0
      - PGID=0
      - TZ=Pacific/Auckland
      - CLI_ARGS= #optional
    volumes:
      - ./config:/config
      - ./backups:/backups
      - /:/source
    ports:
      - 8200:8200
    restart: unless-stopped
```


<div style="font-size: 50%;">
  <pre><code>
  ┌────────────────────────────────────────────────────────────────────────┐   
  │                                                                        │   
  │ @@@@@@@@@@@@@@@@@@@@          %@@@@@@@@%          %@@            %@@   │   
  │ @@@@@@@@@@@@@@@@@@@@       %@@@@@@@@@@@@@@%     %@@@@@@        *@@@@@@ │   
  │ @@@@@@@@@@@@@@@@@@@@      @@@@@@@@@@@@@@@@@@     @@@@@@@%     @@@@@@%  │   
  │ @@@@@          %@@@@     %@@@@@%       @@@@@@      @@@@@@@% @@@@@@%#   │   
  │ @@@@@          %@@@@    %@@@@@          @@@@@@       @@@@@@@@@@@@#     │   
  │ @@@@@          %@@@@    @@@@@@          #@@@@@         @@@@@@@@%       │   
  │ @@@@@          %@@@@    @@@@@@          %@@@@@        %@@@@@@@@@       │   
  │ @@@@@          %@@@@    *@@@@@*         @@@@@%      #@@@@@@@@@@@@@     │   
  │ @@@@@@@@@@@@@@@@@@@@     @@@@@@@%     @@@@@@@     #@@@@@@@  @@@@@@@@   │   
  │ @@@@@@@@@@@@@@@@@@@@      *@@@@@@@@@@@@@@@@%    *@@@@@@@      @@@@@@@% │   
  │ @@@@@@@@@@@@@@@@@@@@        %@@@@@@@@@@@@@       @@@@@%         @@@@@% │   
  │                                %@@@@@@%            @%             @%   │   
  │                                                                        │   
  └────────────────────────────────────────────────────────────────────────┘
                                           A Computer Scientist's Notebook
                                                            Y0MG 1990-2024
GitHub Repository https://github.com/youroldmangaming/DOX
Documentation Site https://dox.youroldmangaming.com
  </code></pre>
</div>


