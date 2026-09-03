## Задание 1

### 1. Инициализация проекта
Выполнена команда `terraform init`. Провайдеры `kreuzwerker/docker` v4.6.0 и `hashicorp/random` v3.9.0 успешно загружены.

### 2. Файл для секретных данных
Согласно `.gitignore`, секретную информацию (логины, пароли, ключи, токены) допустимо хранить в файле `personal.auto.tfvars`. Этот файл:
- добавлен в `.gitignore` и не попадает в репозиторий;
- автоматически подхватывается Terraform при запуске (суффикс `*.auto.tfvars`).

### 3. Секретное содержимое random_password
После выполнения `terraform apply` в файле `terraform.tfstate` найден ресурс `random_password`:
- **Ключ:** `result`
- **Значение:** `JaeUL9jO5ri9wYlc`

### 4. Ошибки в раскомментированном блоке и их исправление
После раскомментирования блока (строки 29–42) и выполнения `terraform validate` были выявлены следующие ошибки:

1. **У `docker_image` отсутствовало имя ресурса.** Строка `resource "docker_image" {` не содержала второго аргумента. Исправлено: `resource "docker_image" "nginx" {`.
2. **Неверная ссылка на образ.** `docker_image.nginx.image_id` — такого атрибута не существует. Исправлено на `docker_image.nginx.name`.
3. **Имя ресурса контейнера начиналось с цифры.** `resource "docker_container" "1nginx"` — недопустимо. Исправлено на `nginx_example`.
4. **Опечатка в ссылке на пароль.** `random_password.random_string_FAKE.resulT` — несуществующее имя ресурса и опечатка в атрибуте. Исправлено на `random_password.random_string.result`.

После исправлений `terraform validate` прошёл успешно: `Success! The configuration is valid.`

Исправленный фрагмент кода:

```hcl
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx_example" {
  image = docker_image.nginx.name
  name  = "example_
