# GPAVMwareESXi
Grafana Prometheus Alertmanager VMwareESXi Monitoring 
Копіюємо у каталог /opt  на Linux server весь каталог з усіма файлами та каталогами.

Стартуємо коли внесли зміни в конфігураційних файлах командою
./start_containers.sh
потрібно буди в каталозі з розташуванням файлу, в каталозі проєкту.
Після запуску файла в каталозі Config створюються фали конфігурації
alertmanager.yml, prometheus_rules.yml, prometheus.yml, я додав ці створені фали тут.
Їх бажано видалити перед запуском, це лиже для зразка. Вони створяться автоматично.
