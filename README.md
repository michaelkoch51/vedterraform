## Выполнение Задания 1

### Создание random_password
Ресурс `random_password.random_string` успешно создан.  
Ключ: `result`  
Значение: `JaeUL9jO5ri9wYlc`

### Развёртывание Docker-ресурсов через Terraform
Terraform создал:
- образ `nginx:latest` через ресурс `docker_image`,
- контейнер `example_JaeUL9jO5ri9wYlc` с пробросом порта `9090 → 80`.

Проверка через `curl http://localhost:9090` подтвердила, что Nginx отвечает HTTP/1.1 200 OK и отдаёт стандартную страницу.

### Переименование контейнера в hello_world
Имя контейнера изменено в `main.tf` на `hello_world`. Terraform удалил старый контейнер и создал новый.  
Вывод `docker ps` подтверждает: контейнер называется `hello_world`, порт `9090` проброшен, сервис доступен.

### Удаление ресурсов и поведение образа
Выполнена команда `terraform destroy`.  
- Контейнер удалён (подтверждено `docker ps`).  
- Образ `nginx:latest` остался в локальном кэше (подтверждено `docker images`).

**Причина:** в ресурсе `docker_image` установлен параметр `keep_locally = true`:

```hcl
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}
