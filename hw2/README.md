*Задание:запустить контейнер с ubuntu, используя механизм LXC, ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает*
1. Устанавливаем необходимые пакеты, создаем lxc контейнер
```
apt-get install lxc debootstrap bridge-utils lxc-templates
apt-get install lxd-installer
lxd init
lxc storage list
lxc-create -n test01 -t ubuntu -f /usr/share/doc/lxc/example/lxc-veth.conf
```
![](/hw2/img/1.png)

2. Открываем файл конфигурации  /var/lib/lxc/test123/config
```
 nano /var/lib/lxc/test01/config
```
![](/hw2/img/2.png)

3. Вставляем в файл конфигурации
```
lxc.cgroup2.memory.max = 256M
```
Сохраняем. Выходим. Запускаем контейнер и заходим в него.
```
lxc-start -n test01 
lxc-attach -n test01 
```
Проверяем память.
```
free -m 
```
И... ничего не изменилось. Выходим из контейнера и ищем другие пути решения.
```
exit 
```
4. Вариант из интернета. Просмотр лимита.
```
 lxc-cgroup -n test01 memory.limit_in_bytes
```
![](/hw2/img/4.png)

5. Устанавливаем свое значение.
  
![](/hw2/img/3.png)