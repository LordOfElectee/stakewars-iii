Добро пожаловать в Stake Wars: Episode III A New Validator

Этот гайд позволит вам выполнить первые 4 задания из тестнета Stake Wars: Episode III

Stake Wars - это программа, которая помогает сообществу познакомиться с тем, что такое быть валидатором в сети NEAR, а так же даст возможность получить ранний доступ к производству чанкво.

Stake Wars предлагает награды новым участникам сети, которые хотят присоединиться как валидаторы, начиная с конца сентября 2022 года.

Первое задание вы можете найти в оффициальном репозитории по адресу:
https://github.com/near/stakewars-iii/blob/main/challenges/001.md

Или продолжайте читать эту статью;)

# Создание кошелька

Перейдите по адресу:
https://wallet.shardnet.near.org/

Вы увидите главну страницу:
![image](https://user-images.githubusercontent.com/55095076/183408111-d1b9b2f4-25dc-4ecc-8b95-9ee964ba42e6.png)

Нажмите на "Создать учетную запись".

Введите желаемое имя пользователя, аналогично изображению ниже:
![image](https://user-images.githubusercontent.com/55095076/183408882-21e5b8b1-9651-44c8-bc44-96ea606d88ed.png)

Нажмите "Reserve My Account ID".

Далее вам будут предложены методы подтверждения владения вашим кошельком. Можно выбрать мнемоническую фразу:

![image](https://user-images.githubusercontent.com/55095076/183409233-a3afd63c-d573-4a94-b78e-5e3561e3f89a.png)

Нажмите "Продолжить".

На экране появятся 12 слов секретной фразы для восстановления кошелька. Эти слова нужно записать в правильном порядке. Они могут понадобиться в дальнейшем.

Нажмите "Продолжить".

Система задаст вам несколько вопросов, например, попросит повторить какое-то конкретное слово.

Укажите слово и нажмите "Проверить и завершить". После этого система создаст кошелек и войдет в него.

#Установка NEAR-CLI

NEAR-CLI это интерфейс командной строки, который взаимодействует с блокчейном NEAR через процедуру вызовов RPC.

Для начала обновимся:

```
sudo apt update && sudo apt upgrade -y
```

###Установим инструменты разработчика:
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -  
sudo apt install build-essential nodejs
PATH="$PATH"
node -v
npm -v
sudo npm install -g near-cli
```
Это всё, что нам неоходимо для установки NEAR-CLI

Далее установим переменную окружения для работы в нужной нам тестовой сети Shardnet:
```
echo 'export NEAR_ENV=shardnet' >> ~/.bashrc
echo 'export NEAR_ENV=shardnet' >> ~/.bash_profile
source $HOME/.bash_profile
```

#Установка ноды
Минимальные системные требования:
CPU 4 ядра с поддержкой AVX
ОЗУ 8GB DDR4
Хранилище 500GB SSD

Для проверки того, подходит ли ваш процессор, запустите команду:
```
lscpu | grep -P '(?=.*avx )(?=.*sse4.2 )(?=.*cx16 )(?=.*popcnt )' > /dev/null \
  && echo "Supported" \
  || echo "Not supported"
```

Вывод должен быть "Supported"

Устанавливаем инструменты:
```
sudo apt install -y git binutils-dev libcurl4-openssl-dev zlib1g-dev libdw-dev libiberty-dev cmake gcc g++ python docker.io protobuf-compiler libssl-dev pkg-config clang llvm cargo jq
```

Если возникли трудности с python или docker.io, попробуйте использовать эти команды:
```
sudo apt install python3
sudo apt install docker-ce
```

Установите Python pip:
```
sudo apt install python3-pip
```

Установите переменные:
```
USER_BASE_BIN=$(python3 -m site --user-base)/bin
export PATH="$USER_BASE_BIN:$PATH"
```

Установите переменные для сборки:
```
sudo apt install clang build-essential make
```

Установите Rust и Cargo
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Во время установки вы увидите следующее сообщение:

![image](https://user-images.githubusercontent.com/55095076/183910041-de83c08f-c410-4555-9c1e-415038e775b7.png)

Нажмите 1 и Enter.

Активируйте переменные:
```
source $HOME/.cargo/env
```

Клонируйте репозиторий <code>nearcore</code> с GitHub
```
git clone https://github.com/near/nearcore
cd nearcore
git fetch
```

Переключите на необходимый коммит
```
git checkout <commit>
```

Соберите бинарный файл
Внутри папки nearcore запустите следующую команду:
```
cargo build -p neard --release --features shardnet
```

После того, как исполяемый файл соберется, он будет располагаться по адресу <code>target/release/neard</code>

Инициализируем рабочую директорию командой:
```
./target/release/neard --home ~/.near init --chain-id shardnet --download-genesis
```

Заменим файл <code>config.json</code>
```
rm ~/.near/config.json
wget -O ~/.near/config.json https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/config.json
```

Установим AWS ClI
```
sudo apt-get install awscli -y
```

Скачиваем файл <code>genesis.json</code>
```
cd ~/.near
wget https://s3-us-west-1.amazonaws.com/build.nearprotocol.com/nearcore-deploy/shardnet/genesis.json
```

В случае возникновения каких-либо проблем с установкой AWS CLI, попробуйте следующее:
```
pip3 install awscli --upgrade
```

#Запуск ноды
Для запуска запустите следующие команды:
```
cd ~/nearcore
./target/release/neard --home ~/.near run
```


Далее ждем поиска пиров и полной синхронизации.

#Активация ноды

Для начала необходимо локально авторизовать кошелек. Выполните
```
near login
```


#<details><summary>Commands</summary>
<p>

To see all proposals to become a validator
```
near proposals  
```

To see current list of validators
```
near validators current
```

To see list of validators in next epoch
```
near validators next
```
</p>
</details>
