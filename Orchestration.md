Задача 1

https://hub.docker.com/repository/docker/belcev1985/custom-nginx/general

--------------------------

Задача 2


![Zd2 1](https://github.com/user-attachments/assets/9fa60d40-39b1-41ce-b4d2-20445aa899b5)

![Zd2 2](https://github.com/user-attachments/assets/a45a3697-c86d-4a1f-97b3-c79f39583fcf)

--------------------------

Задача 3

1. Когда я подключаюсь к контейнеру с помощью docker attach и нажимаю Ctrl-C, я отправляю сигнал SIGINT в основной процесс контейнера. Если основной процесс не обрабатывает этот сигнал или не настроен его игнорировать, контейнер завершает свою работу, так как основной процесс завершился. Это происходит потому, что жизненный цикл контейнера в Docker напрямую связан с жизненным циклом его основного процесса. Когда основной процесс завершает работу, контейнер также останавливается. (допилито неройсетью)

![Zd3 1](https://github.com/user-attachments/assets/e481d781-6dc9-4787-9be6-a9536bf0c7f6)

![Zd3 2](https://github.com/user-attachments/assets/9aaa5c18-44c3-41ff-9e86-a00204aea57c)

![Zd3 3](https://github.com/user-attachments/assets/2267f7b8-2360-4c1b-9c41-67defa78ae1f)

![Zd3 3 1](https://github.com/user-attachments/assets/ed0da3a1-abdf-4f63-94f6-0ba4c34dea4c)

11. Суть проблемы заключается в том, что после изменения порта в конфигурации Nginx с 80 на 81 и перезагрузки Nginx, порт 81 на контейнере не был проброшен на хостовую машину. По умолчанию, контейнеры Docker не пробрасывают порты автоматически, и если порт не проброшен, то внешние запросы не могут достучаться до контейнера по новому порту.
12. sudo docker rm -f custom-nginx-t2 - (забыл сделать скрин)

--------------------------

Задача 4

![Zd4](https://github.com/user-attachments/assets/1ee70848-52bb-4467-b129-8a19c3d605d3)

--------------------------

Задача 5

https://github.com/BelcEV1985/EDU/blob/main/docker-compose.yml

![Zd5 1](https://github.com/user-attachments/assets/ffca8044-280b-4ec9-a558-09f775274464)

![Zd5 2](https://github.com/user-attachments/assets/ecb742e7-9c34-4c0a-891d-72e0d0f92718)

![Zd5 3](https://github.com/user-attachments/assets/82f89322-083d-4bbd-80ff-c964353f8512)

![Zd5 4](https://github.com/user-attachments/assets/8dc7b79d-c187-434b-8759-839fe479766d)

![Zd5 6](https://github.com/user-attachments/assets/60b8de78-cf24-4b55-8e6e-a73a66c46b3d)

![Zd5 5](https://github.com/user-attachments/assets/c458e1dc-5fcb-4404-b853-690e058cb3d4)
