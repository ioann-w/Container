## Работа с Docker ##
1. Проверяем установку Docker
```
docker run docker/whalesay cowsay -f elephant "Hello, Docker!"
```
![](/hw3/img/1.png)

2. Запустим контейнер из образа Ubuntu и войдем в него:
```
docker run -it -h GB --name gb-test ubuntu:22.10
```
![](/hw3/img/2.png)

Создадим новую директорию в корне:
```
mkdir /example
```
Создадим файл "passwords.txt" и добавим в него какие-либо данные
```
touch /example/passwords.txt
echo "123test" >> /example/passwords.txt
exit
```
![](/hw3/img/3.png)

3. Остановим контейнер и  запустим его снова.
```
docker stop gb-test
docker start gb-test
docker exec -it gb-test bash
cat /example/passwords.txt
exit
```

![](/hw3/img/4.png)

Наши данные сохранятся, так как мы не пересоздавали контейнер.

4. Удалим контейнер и создадим его заново, используя те же команды.
```
docker stop gb-test
docker rm gb-test
docker run -it -h GB --name gb-test ubuntu:22.10
ls -l
exit
docker stop gb-test
docker rm gb-test
```
![](/hw3/img/5.png)

В этот раз наши данные будут утеряны, так как контейнер был удален.

5.  Создадим директорию и подмонтируем ее к контейнеру.
```
mkdir /test/folder
docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10
```
![](/hw3/img/6.png)

6. Добавим данные в подмонтированную директорию
```
echo "$HOSTNAME" >> /otherway/test.txt
```
![](/hw3/img/7.png)

7. Удалим контейнер и создадим его снова, подмонтировав директорию.
```
docker stop gb-test
docker rm gb-test
docker run -it -h GB --name gb-test -v /test/folder:/otherway ubuntu:22.10
cat /otherway/test.txt
exit
docker stop gb-test
docker rm gb-test
```
![](/hw3/img/8.png)

Самый надежный способ хранения данных в контейнерах - использование внешних хранилищ.

8. Хранение данных в контейнерах Docker.

Создаем две папки с разным содержимым для будущего монтирования. Создаем контейнер из образа ubuntu:22.10.
```
mkdir ~/docker-mount-example
echo "This is the host test.txt file" > ~/docker-mount-example/test.txt
echo "This is the root test.txt file" > ~/test.txt
docker run -it -h GB --name gb-test -v ~/docker-mount-example:/container-mount -v ~/test.txt:/container-mount/test.txt ubuntu:22.10
```
![](/hw3/img/9.png)

Содержимое `/container-mount/test.txt` перезаписалось вторым монтированием из файла `~/test.txt`.