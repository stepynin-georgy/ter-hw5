# Домашнее задание к занятию «Использование Terraform в команде»

### Цели задания

1. Научиться использовать remote state с блокировками.
2. Освоить приёмы командной работы.


### Чек-лист готовности к домашнему заданию

1. Зарегистрирован аккаунт в Yandex Cloud. Использован промокод на грант.
2. Установлен инструмент Yandex CLI.
3. Любые ВМ, использованные при выполнении задания, должны быть прерываемыми, для экономии средств.

------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
Убедитесь что ваша версия **Terraform** ~>1.8.4
Пишем красивый код, хардкод значения не допустимы!

------
### Задание 0
1. Прочтите статью: https://neprivet.com/
2. Пожалуйста, распространите данную идею в своем коллективе.

------

### Задание 1

1. Возьмите код:
- из [ДЗ к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/src),
- из [демо к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1).
2. Проверьте код с помощью tflint и checkov. Вам не нужно инициализировать этот проект.

Для ДЗ к лекции 4 проверил с помощью tflint:

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_154.png)

Для демо к лекции 4 проверил с помощью checkov:

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_155.png)
![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_156.png)
![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_157.png)
  
3. Перечислите, какие **типы** ошибок обнаружены в проекте (без дублей).

------

### Задание 2

1. Возьмите ваш GitHub-репозиторий с **выполненным ДЗ 4** в ветке 'terraform-04' и сделайте из него ветку 'terraform-05'.

[terraform-05](https://github.com/stepynin-georgy/ter-hw5/tree/terraform-05)

2. Повторите демонстрацию лекции: настройте YDB, S3 bucket, yandex service account, права доступа и мигрируйте state проекта в S3 с блокировками. Предоставьте скриншоты процесса в качестве ответа.

- Создаем бакет:
    - В yc заходим в Object Storage
    - Нажимаем Новый бакет
    - Указываем уникальное глобально среди всех бакетов имя. Было указано tfstate-developer
    - Указываем предельный бесплатный предел занимаемого пространства - 1Гб

    ![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_158.png)

- Создаем таблицу блокировок
    - Создаем БД ydb. Заходим в Managed Services for YDB и нажимаем Создать базу данных. Указываем имя БД - tfstate-develop. Задаем максимальный бесплатный размер БД - 1Гб
 
    ![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_159.png)

    - В списке БД нажимаем по названию БД и проваливаемся внутрь БД.
    - Выбираем слева пункт меню Навигация
    - Нажимаем кнопку Создать и в выпадающем списке команд выбираем Таблица (можно еще создать Директории и Потоки данных)
    - Задаем название таблицы(например tfstate-locks) и выбираем тип таблицы - Документальная(в отличие от строковой таблицы каждая запись может иметь свой собственный набор атрибутов, но также имеет уникальный идентификатор)
    - Указываем один столбец - LockID типа строка
 
    ![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_160.png)

- Назначаем акканту права на бакет
    - В списке бакетов нажимаем рядом с бакетом на троеточие и в выпадающем списке выбираем команду ACL бакета
    - Появится диалоговое окно где выбираем аккаунт и задаем ему права Read And Write

    ![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_163.png)

3. Закоммитьте в ветку 'terraform-05' все изменения.
4. Откройте в проекте terraform console, а в другом окне из этой же директории попробуйте запустить terraform apply.
5. Пришлите ответ об ошибке доступа к state.

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_164.png)

6. Принудительно разблокируйте state. Пришлите команду и вывод.

Получилось разблокировать state следующей командой: ```root@terraform-hw4:/opt/hw5/ter-hw5/hw# terraform apply -lock=false```


------
### Задание 3  

1. Сделайте в GitHub из ветки 'terraform-05' новую ветку 'terraform-hotfix'.

[terraform-hotfix](https://github.com/stepynin-georgy/ter-hw5/tree/terraform-hotfix)

2. Проверье код с помощью tflint и checkov, исправьте все предупреждения и ошибки в 'terraform-hotfix', сделайте коммит.

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_167.png)

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_168.png)

После исправления ошибок:

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_168.png)

3. Откройте новый pull request 'terraform-hotfix' --> 'terraform-05'. 
4. Вставьте в комментарий PR результат анализа tflint и checkov, план изменений инфраструктуры из вывода команды terraform plan.
5. Пришлите ссылку на PR для ревью. Вливать код в 'terraform-05' не нужно.

[pull-request](https://github.com/stepynin-georgy/ter-hw5/pull/1) с комментариями

------
### Задание 4

1. Напишите переменные с валидацией и протестируйте их, заполнив default верными и неверными значениями. Предоставьте скриншоты проверок из terraform console. 

Добавил следующие переменные:

```
variable "ip_address" {
  type        = string
  description = "ip-адрес"
  default = "192.168.0.1"

  validation {
    condition     = can(regex("^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$", var.ip_address))
    error_message = "Неверный IP-адрес."
  }
}

variable "ip_addresses" {
  type        = list(string)
  description = "список ip-адресов"
  default     = ["192.168.0.1", "1.1.1.1", "127.0.0.1"]

  validation {
    condition = alltrue([
      for address in var.ip_addresses : can(regex("^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$", address))
    ])
    error_message = "Один или несколько адресов в списке неверны."
  }
}
```

- type=string, description="ip-адрес" — проверка, что значение переменной содержит верный IP-адрес с помощью функций cidrhost() или regex(). Тесты:  "192.168.0.1" и "1920.1680.0.1";

При корректных значениях:

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_173.png)

- type=list(string), description="список ip-адресов" — проверка, что все адреса верны. Тесты:  ["192.168.0.1", "1.1.1.1", "127.0.0.1"] и ["192.168.0.1", "1.1.1.1", "1270.0.0.1"].

При некорректных значениях:

![изображение](https://github.com/stepynin-georgy/ter-hw5/blob/main/hw/img/Screenshot_172.png)

## Дополнительные задания (со звёздочкой*)

**Настоятельно рекомендуем выполнять все задания со звёздочкой.** Их выполнение поможет глубже разобраться в материале.   
Задания со звёздочкой дополнительные, не обязательные к выполнению и никак не повлияют на получение вами зачёта по этому домашнему заданию. 
------
### Задание 5*
1. Напишите переменные с валидацией:
- type=string, description="любая строка" — проверка, что строка не содержит символов верхнего регистра;
- type=object — проверка, что одно из значений равно true, а второе false, т. е. не допускается false false и true true:
```
variable "in_the_end_there_can_be_only_one" {
    description="Who is better Connor or Duncan?"
    type = object({
        Dunkan = optional(bool)
        Connor = optional(bool)
    })

    default = {
        Dunkan = true
        Connor = false
    }

    validation {
        error_message = "There can be only one MacLeod"
        condition = <проверка>
    }
}
```
------
### Задание 6*

1. Настройте любую известную вам CI/CD-систему. Если вы ещё не знакомы с CI/CD-системами, настоятельно рекомендуем вернуться к этому заданию после изучения Jenkins/Teamcity/Gitlab.
2. Скачайте с её помощью ваш репозиторий с кодом и инициализируйте инфраструктуру.
3. Уничтожьте инфраструктуру тем же способом.


------
### Задание 7*
1. Настройте отдельный terraform root модуль, который будет создавать YDB, s3 bucket для tfstate и сервисный аккаунт с необходимыми правами. 

### Правила приёма работы

Ответы на задания и необходимые скриншоты оформите в md-файле в ветке terraform-05.

В качестве результата прикрепите ссылку на ветку terraform-05 в вашем репозитории.

**Важно.** Удалите все созданные ресурсы.

### Критерии оценки

Зачёт ставится, если:

* выполнены все задания,
* ответы даны в развёрнутой форме,
* приложены соответствующие скриншоты и файлы проекта,
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку работу отправят, если:

* задание выполнено частично или не выполнено вообще,
* в логике выполнения заданий есть противоречия и существенные недостатки. 
