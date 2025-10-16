# GPAVMwareESXi
Grafana Prometheus Alertmanager VMwareESXi Monitoring 
Копіюємо у каталог /opt  на Linux server весь каталог з усіма файлами та каталогами.
Стартуємо коли внесли зміни в конфігураційних файлах командою ./start_containers.sh
потрібно буди в каталозі з розташуванням файлу, в каталозі проєкту.
Після запуску файла в каталозі Config створюються фали конфігурації
alertmanager.yml, prometheus_rules.yml, prometheus.yml, я додав ці створені фали тут.
Їх бажано видалити перед запуском, це лиже для зразка. Вони створяться автоматично.
В кінці файлу start_containers.sh додав -d, щоб контейнери запускались в режимі daemon.
Якщо не налаштувати алерти, то контейнер алертів не запуститься, але працює.

З файлу власника опис як що робити.
https://github.com/DawtCom/VMware-ESXI-Monitoring-Tools

Инструменты мониторинга ESXI — Kickstart
Описание: 
Этот проект — пример быстрой настройки среды мониторинга для VMWare ESXi с использованием Prometheus и Grafana для сбора метрик, оповещения и визуализации. В этом проекте используется простой файл docker-compose.yml для создания четырёх контейнеров, составляющих всю среду. Для быстрой настройки необходимо задать или передать необходимые переменные среды при запуске start_containers.sh.
Метрики Prometheus / Grafana / VMWare ESXi для Prometheus Контейнеры, развернутые с помощью этого проекта Prometheus Service (сервис сбора метрик) 
Сервис Grafana (инструмент визуальной панели) 
Служба Prometheus Alert Manager (доставка оповещений и управление ими) 
VMWare Exporter (служба сбора метрик из VMWare и их предоставления для Prometheus)
Инструкции по запуску 
Сначала клонируйте этот репозиторий на хост, на котором установлен докер.
На этой странице не рассматривается установка докера, и предполагается, что вы уже знаете, как это сделать, или уже установили его на целевом компьютере. 
Установку докера можно найти здесь: https://docs.docker.com/get-docker/
##################################################################################################################################
Або можна слідувати інструкціі.
To install Docker Engine and Docker Compose on Ubuntu 22.04, follow these steps:
1. Update System Packages:
sudo apt update && sudo apt upgrade -y
2. Install Dependencies:
sudo apt install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
3. Add Docker's GPG Key and Repository:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture)] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
4. Install Docker Engine:
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
5. Add Your User to the Docker Group (Optional, but recommended for non-root usage):
sudo usermod -aG docker $USER
newgrp docker
Note: You may need to log out and log back in for the group changes to take effect.
6. Install Docker Compose Plugin:
sudo apt install -y docker-compose-plugin
7. Verify Installation:
Check the Docker version:
docker --version
Check the Docker Compose version:
docker compose version
You should see output similar to:
Docker version 28.5.1, build e180ab8 build 28.5.1-0ubuntu1~22.04.1
Docker Compose version v2.17.2
################################################################################################################################
Чтобы запустить контейнерную среду, которая будет отслеживать ваши ESXI, Prometheus, менеджер предупреждений Prometheus и хосты Grafana, выполните эту простую команду в оболочке Linux.
Заменив значения, указанные в скобках ниже, на ваши конкретные данные.
Не включайте скобки в заменяемое значение.
В этой командной строке предполагается, что вы используете Gmail в качестве поставщика электронной почты.
SMTP_HELLO=[yourdomain.com] SMTP_TO=[first.last@gmail.com] SMTP_FROM=[prometheus@yourdomain.com] SMTP_SMARTHOST=smtp.gmail.com:587 SMTP_USER=[your_gmail_user@gmail.com] SMTP_PASS=[your_google_app_password] HOST_IP=[host_ip_running_docker_compose] ESXI_HOSTNAME=[esxi.yourdomain.com] VSPHERE_HOST=[esxi_host_ip_or_hostname] VSPHERE_USER=[vsphere_user] VSPHERE_PASS=[vsphere_pass] ./start_containers.sh
Если вы хотите запустить контейнеры в режиме демона, измените команду docker-compose up в файле start_containers.sh, включив опцию -d. 
Например: VSPHERE_HOST=$VSPHERE_HOST VSPHERE_USER=$VSPHERE_USER VSPHERE_PASS=$VSPHERE_PASS docker-compose up -d
Чтобы уничтожить или остановить контейнеры, вы можете выполнить команду docker-compose down в каталоге, в котором находится файл docker-compose.yml. 
Если вы запустили контейнеры в режиме без демона, который используется по умолчанию, просто нажмите CTRL-C.
Переменные среды, используемые в упомянутом выше файле start_containers.sh:
==================================================================================================================================
|      Environment      |               Variable            |                      	Example	Notes                                |
==================================================================================================================================
|      SMTP_HELLO	    |             yourdomain.com        |     This value is used in the HELO message to the email            |
|                       |                                   |  provider. Replaces token {{SMTP_HELLO}} in alertmanager.yml.tpl.  |
==================================================================================================================================
|        SMTP_TO	    |        first.last@gmail.com       |   This is the email address you want the alerts sent               |
|                       |                                   |   to when they are triggered.                                      |
|                       |                                   |   Replaces token {{SMTP_TO}} in alertmanager.yml.tpl               |
==================================================================================================================================
|     SMTP_FROM         |     prometheus@yourdomain.com     | This is what the from line will be in the received mai             |
|                       |                                   |  from the alerts manager. Replaces token                           |
|                       |                                   |     {{SMTP_FROM}} in alertmanager.yml.                             |
==================================================================================================================================
|     SMTP_SMARTHOST    |         smtp.gmail.com:587        |  This should be the server and port number hat                     |
|                       |                                   |   receives the SMTP email from the alerts anager.                  |
|                       |                                   |   Replaces token {{SMTP_SMARTHOST}} in                             |
|                       |                                   |   alertmanager.yml.tpl                                             |
==================================================================================================================================
|                       |                                   | The user that will authenticate with the SMTP(email)               |
|      SMTP_USER        |       smtp.user@gmail.com         |   server when sending an email from the alert manager.             |
|                       |                                   |   Replaces token {{SMTP_USER}} in alertmanager.yml.tpl             |
==================================================================================================================================
|                       |                                   |   The password to use when authenticating with theSMTP(email)      |
|     SMTP_PASS         |    your_smtp_secret_password      |  server when sending an email from the Replaces token              |
|                       |                                   |      {SMTP_PASS}} in alertmanager.yml.tpl                          |
==================================================================================================================================