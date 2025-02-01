
2.Изучите файл .gitignore. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
Файл personal.auto.tfvars не отслеживаться Git и его содержимое не попадет в репозиторий.


3.Найдите в terraform.tfstate секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
   
"bcrypt_hash": "$2a$10$Wvc.VscMkVOVCXFDnvaE8uXquEFoHtzGggj6xDeFBFEakhyXNPvbm"
"result": "Fl01G3yX3qVCp9Ni"


4.Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла main.tf. Выполните команду terraform validate. Объясните, в чём заключаются намеренно допущенные ошибки.Исправьте их.
Ошибка 1: Отсутствие имени ресурса

resource "docker_image" {
  name         = "nginx:latest"
  keep_locally = true
}

Ошибка:
Error: Missing name for resource

Объяснение:
В Terraform каждый ресурс должен иметь два идентификатора: тип ресурса и имя ресурса. В данном случае тип ресурса указан как "docker_image", но имя ресурса отсутствует.

Ошибка 2: Некорректное имя ресурса

resource "docker_container" "1nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string_FAKE.resulT}"

  ports {
    internal = 80
    external = 9090
  }
}

Ошибка:
Error: Invalid resource name

Объяснение:
Имя ресурса "1nginx" начинается с цифры, что недопустимо в Terraform. Имена ресурсов должны начинаться с буквы или символа подчеркивания и могут содержать только буквы, цифры, подчеркивания и дефисы

Ошибка 3: Неправильное использование переменной
name  = "example_${random_password.random_string_FAKE.resulT}"

Объяснение:
Не определен ресурс


5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды docker ps.
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "random_password" "random_string" {
  length = 16
  special = false
}

resource "docker_container" "nginx_container" {
  image = docker_image.nginx.image_id
  name  = "hello_world"

![05](https://github.com/user-attachments/assets/2048a9f9-5c62-4fb6-b5fd-f9706d23564e)


6. Замените имя docker-контейнера в блоке кода на hello_world. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest".
resource "docker_container" "nginx_container" {
  image = docker_image.nginx.image_id
  name  = "hello_world"

6.1 Выполните команду terraform apply -auto-approve. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. 
Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды docker ps.
Ну это с лекции, там всё обсуждалось. Дополнительно погуглил, страшно.

![06](https://github.com/user-attachments/assets/88404a88-1910-4fe7-a908-4a67eb5073ae)

Опасность использования -auto-approve:
Отсутствие контроля
-auto-approve отключает интерактивное подтверждение
Риск повреждения инфраструктуры
Если в конфигурации есть ошибки то, они будут автоматически применены.
Безопасность
в CI/CD ошибочный код может внести изменения в инфраструктуру без предупреждений (не секурно особенно на проде)

7. Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены.

terraform destroy


![07](https://github.com/user-attachments/assets/451186c8-acec-4883-9f1b-eb49711c4b84)


8. Объясните, почему при этом не был удалён docker-образ nginx:latest. Ответ ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ, а затем ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ строчкой из документации terraform провайдера docker. (ищите в классификаторе resource docker_image )

Это случайно нашел когда 4ое задание делал. Еще удивился, как удобно.
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true  # Этот параметр предотвращает удаление образа при уничтожении ресурса
}

keep_locally - (Optional, boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.
