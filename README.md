Автомаматизация повседневных операций 1С разработчика
==
 
[![Gitter](https://badges.gitter.im/silverbulleters/vanessa-runner.svg)](https://gitter.im/silverbulleters/vanessa-runner?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
[![SonarQube Tech Debt](https://img.shields.io/sonar/https/sonar.silverbulleters.org/vanessa-runner/tech_debt.svg)](https://sonar.silverbulleters.org/dashboard?id=vanessa-runner)

Описание 
===

Библиотека `oscript.io` для автоматизации различных операции для работы с `cf/cfe/epf` файлами и простой запуск `vanessa-behavior` и `xunitfor1c` тестов. 

Предназначена для организации разработки 1С в режиме когда работа в git идет напрямую с исходниками.

Позволяет обеспечить единообразный запуск команд "локально" и на серверах сборки `CI-CD`


Установка
===

используйте пакетный менеджер `opm` из стандартной поставки дистрибутива `ocript.io`

```
opm install vanessa-runner
```

при установке будет создан исполняемый файл `runner` в каталоге `bin` интерпретатора `oscript`, после чего доступно выполнение команд через командную строку `runner <имя команды>`


Использование
===

Основной код сосредоточен в `tools/runner.os` , ключ `--help` покажет справку по параметрам.

```
runner --help
``` 

В папке tools так же расположенны примеры bat файлов для легкого запуска определенных действий. 
Основной принцип - запустили bat файл с настроенными командами и получили результат.

К папке epf несколько обработок позволяющих упростить деплой для конфигураций основанных на БСП. 

Основной пример это передача через параметры /C комманды "ЗапуститьОбновлениеИнформационнойБазы;ЗавершитьРаботуСистемы" и одновременно выполняем через /Execute"ЗакрытьПредприятие.epf", т.е. при запуске с такими ключами подключается обработчик ожидания проверят наличие формы с заголовком обновление и при окончании обновления завершает предприятие. Данное действие необходимо, для полного обновления предприятия, пока действует блокировка на фоновые задачи и запуск пользователей.

**ЗагрузитьРасширение** позволяет подключать разрешение в режиме предприятия и получать результат ошибки. Предназначенно для подключения в конфигурациях основаных на БСП. В параметрах /C передается путь к расширению и путь к файлу лога подключения. 
**ЗагрузитьВнешниеОбработки** позволяет загрузить все внешние обработки и подключить в справочник "Дополнительные отчеты и обработки", т.к. их очень много то первым параметром идет каталог, вторым параметром путь к файлу лога. Все обработки обновляются согласно версиям.

Сборка
===

Для сборки обработок необходимо иметь установленный oscript в переменной PATH и платформу выше 8.3.8 , в коммандной строке перейти в каталог с проектом и выполнить ```tools\compile_epf.bat``` по окончанию в каталоге build\epf должны появиться обработки.
Вся разработка в конфигураторе делается в каталоге build, по окончанию доработок запускаем ```tools\decompile_epf.bat``` 

Обязательно наличине установленного v8unpack версии не ниже 3.0.38 в переменной PATH. Установку можно взять https://github.com/dmpas/v8unpack#build
