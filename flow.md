mongodb (https://open5gs.org/open5gs/docs/guide/01-quickstart/)
———————————————————————————————————————————————————————————————
sudo apt update
sudo apt install gnupg
curl -fsSL https://pgp.mongodb.com/server-8.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-8.0.list
sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
sudo add-apt-repository ppa:open5gs/latest
sudo apt update
sudo apt install open5gs

# WebUI
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

sudo apt update
sudo apt install nodejs -y

curl -fsSL https://open5gs.org/open5gs/assets/webui/install | sudo -E bash -

sudo systemctl restart open5gs-mmed
sudo systemctl restart open5gs-sgwud
sudo systemctl restart open5gs-nrfd
sudo systemctl restart open5gs-amfd
sudo systemctl restart open5gs-upfd
———————
sudo nano /etc/open5gs/amf.yaml
change plmn_id:
	mcc 250
	mnc 54
	для amf 127.0.0.5
	для бс 127.0.0.1

то же самое для nrf.yaml
то же самое ? для upf.yaml

sudo systemctl restart open5gs-nrfd
sudo systemctl restart open5gs-amfd
sudo systemctl restart open5gs-upfd
TODO: попросить конфиги
———————
тоже как-то обновить mme.yaml
теперь залезаем в веб-морду и:
пишем IMSI: 250540000121191, по скрину в чате меняем opc, k, amf
идём по https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/develop/doc/BUILD.md
-------- блок ниже НЕ РАБОТАЕТ
git clone git@gitlab.eurecom.fr:oai/openairinterface5g.git
cd openairinterface5g
mkdir build
cd build
(может потребоваться установить sudo apt install cmake)
cmake .. -GNinja
*здесь возникла проблема со сборкой*
_сделали кору, осталось сделать бс_
sudo sysctl -w net.ipv4.ip_forward=1
----------
---------- пробуем клонирование с гитхаба
git clone https://github.com/duranta-project/openairinterface5g.git
cd openairinterface5g
cd
cd cmake_targets
./build_oai -I
./build_oai --gNB
cd ..
mkdir build
cd build
cmake .. -GNinja
ninja
sudo apt install -y libforms-dev libforms-bin
продолжаем выполнять ninja
cd ..
cmake -B build -G Ninja
cmake --build build
cd build (в доке не указано)
cmake .. -GNinja -DENABLE_TELNETSRV=ON
ninja telnetsrv
cd openairinterface5g/cmake_targets/ran_build/build
cmake ../../.. (тут по доке -GNinja, но мы сделали без этого флага)
sudo apt install cmake-qt-gui
ccmake ../../..
cmake-gui ../../..
(либо есть другой вариант:
cd cmake_targets/ran_build/build
ninja)
cd openairinterface5g/cmake_targets/
./build_oai -I --install-optional-packages -w USRP
——————
export BUILD_UHD_FROM_SOURCE=True
export UHD_VERSION=4.2.0.1 (в доке написано 4.10.0.0, но надо ставить 4.2.0.1 по ссылке: https://github.com/EttusResearch/uhd/releases/tag/v4.2.0.1)
./build_oai -I -w USRP
*сборка закончилась failed*
((проверить бы версию тут... )
cd openairinterface5g/cmake_targets/
./build_oai -w USRP --eNB --UE --nrUE --gNB
./build_oai --build-lib all # build all
./build_oai --build-lib telnetsrv  # build only telnetsrv
./build_oai --build-lib "telnetsrv enbscope uescope nrscope", всё это в скобках свят не ставил)
——————
cd /usr/local/share/uhd
mkdir -p images
sudo unzip -o /home/backlly/Загрузки/uhd-images_4.2.0.1.zip -d /usr/local/share/uhd/images/
sudo mv /usr/local/share/uhd/images/uhd-images_4.2.0.1/* /usr/local/share/uhd/images
sudo rmdir /usr/local/share/uhd/images/uhd-images_4.2.0.1
cd openairinterface5g/cmake_targets/
cd ran_build/build
sudo uhd_find_devices (должно отобразиться при подключенном usrp)
sudo chrt -f 99 taskset -c 0-15 ./nr-softmodem -O ~/openairinterface5g/targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf -E -d --thread-pool 2,4,6,8,10,12,14 --continuous-tx --usrp-tx-thread-config 1 (команда для запуска БС, она сейчас ищет версию 4.10.0.0, но нужна 4.2.0.1)
sudo add-apt-repository ppa:ettusresearch/uhd
sudo apt update
sudo apt install uhd-host libuhd-dev
cd openairinterface5g/cmake_targets/ran_build/build
sudo uhd_find_devices (должно отобразиться при подключенном usrp)
git clone https://github.com/TelecomDep/uhd_images/
unzip uhd_images-большое-название.zip .
sudo UHD_IMAGES_DIR=~/uhd_images chrt -f 99 taskset -c 0-7 ./nr-softmodem -O ~/openairinterface5g/targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf -E -d --thread-pool 2,4,6 --continuous-tx --usrp-tx-thread-config 1 (запускать из openairinterface5g/cmake_targets/ran_build/build, должно заработать)
nano gnb.sa.band78.fr1.106PRB.usrpb210.conf:
	plmn_list: mcc = 250, mnc = 54
	amf_ip_address = 127.0.0.5
	GNB_IPV4... = 127.0.0.5/24
	GNB_IPV4... = 127.0.0.5/24
sudo apt update
sudo apt install linux-image-5.15.0-1032-realtime linux-modules-5.15.0-1032-realtime linux-headers-5.15.0-1032-realtime
sudo update-grub
sudo reboot