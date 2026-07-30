# Разделение работы между участниками

Модули разделены по двум вертикальным функциональным частям. Все файлы Erston из `src` компилируются в единое пространство имён, поэтому после слияния `expand ИтоговоеЗадание` формирует тот же тип документа.

## Общая основа

Эти файлы следует добавить в первоначальный коммит пустого проекта до создания рабочих веток:

```text
.project/AppDescription.xml
.project/Configuration/**
.project/OnRemoteInstall.xml
.project/OnUpdate.xml
.devcontainer/**
src/app.erst
src/global.erst
src/metadata/domain.erst
src/documents/core/final_assignment.erst
docs/TEAM_SPLIT.md
```

`final_assignment.erst` содержит только единственное объявление типа документа. Повторно объявлять `type ИтоговоеЗадание` в рабочих ветках нельзя.

## Участник A: товары и коробки

Рекомендуемая ветка: `feature/box-packing`.

```text
src/documents/core/main_menu.erst
src/documents/boxes/**
src/metadata/settings.erst
README.md
docs/DEMO.md
```

Ответственность:

- главное меню, настройки и выход;
- проверка и создание коробки;
- разбор DataMatrix, поиск GTIN и запись марки;
- отображение содержимого коробки;
- удаление марки и очистка коробки;
- автоматическое закрытие коробки по глубине.

## Участник B: паллеты и сервер

Рекомендуемая ветка: `feature/pallet-server`.

```text
src/documents/pallets/**
src/operations/packages/**
src/operations/completion/**
src/metadata/tables.erst
.project/Settings/HYDB.*
.project/Settings/ServerEvents.xml
.project/Settings/Connectivity.ServerOperationConnector.xml
.project/Settings_Default/HYDB.*
.project/Settings_Default/ServerEvents.xml
.project/Settings_Default/Connectivity.ServerOperationConnector.xml
docs/DOCUMENTATION.md
docs/TEST_SCENARIOS.md
```

Ответственность:

- сканирование паллет и коробок;
- загрузка содержимого паллеты с сервера;
- добавление и перемещение серверных коробок;
- просмотр паллет, коробок и товаров;
- серверные таблицы;
- выгрузка завершённого документа;
- обработка конфликтов марок.

## Порядок объединения

1. Создать обе ветки от одного базового коммита.
2. Каждый участник добавляет только назначенные файлы.
3. Слить `feature/box-packing` в `main`.
4. Обновить `feature/pallet-server` из `main` и проверить межмодульные вызовы.
5. Слить `feature/pallet-server`.
6. Выполнить `Erston: Build and publish`.
7. Запустить эмулятор через `F5` и пройти сценарии P0 из `TEST_SCENARIOS.md`.

Не следует распределять между ветками `output`, `output.mstmpl`, `.data` и содержимое `.erston`: это локальные или сгенерированные артефакты.
