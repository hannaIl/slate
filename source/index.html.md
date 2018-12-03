---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - bash
  - python
  - ruby

toc_footers:
  - <a href='https://jira.wargaming.net/projects/WGCMDB/issues/WGCMDB-1643?filter=allopenissues'>Связаться с разработчиками</a>


includes:
  - errors

search: true
---

# Introduction

**CMDB (Configuration Management Database, база данных управления конфигурациями)** — это база данных для хранения информации о конфигурационных единицах (CIs) и их взаимосвязях. Используется для управления сервисами (изменениями, релизами, инцидентами, конфигурациями, проблемами, запросами на обслуживание и т.д.) в соответствии с ITIL. 

# Base URL
Здесь и далее все URL указаны относительно базового URL:

* Тестовая площадка: [https://test.cfg.iv/services/rest/v3](https://test.cfg.iv/services/rest/v3)
* "Продакшен" площадка: [https://cfg.wargaming.net/services/rest/v3](https://cfg.wargaming.net/services/rest/v3)

# Authentication

## Basic Auth

> Тестовая площадка:

```shell
curl -ik -u j_doe:secret https://test.cfg.iv/services/rest/v3

curl -ik -u 'corp\j_doe':secret https://test.cfg.iv/services/rest/v3

curl -ik -u 'j_doe@wargaming.net':secret https://test.cfg.iv/services/rest/v3
```

> "Продакшен" площадка:

```shell
curl -u j_doe:secret https://cfg.wargaming.net/services/rest/v3

curl -u 'corp\j_doe':secret https://cfg.wargaming.net/services/rest/v3

curl -u 'j_doe@wargaming.net':secret https://cfg.wargaming.net/services/rest/v3
```
При запросах напрямую к урлам сервиса поддерживается HTTP-заголовок `Authorization` (либо опция `-u`) с basic кредами пользователя.

<aside class="notice">
При написании запросов к тестовым урлам, в заголовок запроса необходимо добавлять опцию `-ik`, отвечающую за запросы к тестовому серверу без верификации сертификата.
</aside>

## Session

> Тестовая площадка:

```shell
curl -ik -u j_doe:secret https://test.cfg.iv/services/rest/v3

curl -ik -u 'corp\j_doe':secret https://test.cfg.iv/services/rest/v3

curl -ik -u 'j_doe@wargaming.net':secret https://test.cfg.iv/services/rest/v3
```

> "Продакшен" площадка:

```shell
curl -u j_doe:secret https://cfg.wargaming.net/services/rest/v3

curl -u 'corp\j_doe':secret https://cfg.wargaming.net/services/rest/v3

curl -u 'j_doe@wargaming.net':secret https://cfg.wargaming.net/services/rest/v3
```

Авторизация через сервис CMDB.  URLs авторизации:

* [cfg.wargaming.net/services/rest/v3/sessions](cfg.wargaming.net/services/rest/v3/sessions)
* [test.cfg.iv/services/rest/v3/sessions](cfg.wargaming.net/services/rest/v3/sessions)

<aside class="notice">
При написании запросов к тестовым урлам, в заголовок запроса необходимо добавлять опцию `-ik`, отвечающую за запросы к тестовому серверу без верификации сертификата.
</aside>

# Classes API

## Получение списка классов

> Sample request

```shell
curl -ik \
    -H 'CMDBuild-Authorization: ecf46766-3b7f-4d1e-bd86-1e361c395fe6' \
    -H 'Accept: application/json' \
    -X GET https://test.cfg.iv/services/rest/v3/class
```

> Sample response:

```shell
[
    "Service",
    "Configurations",
    "AppConfig",
    "Components",
    ...
    "WGDPHost",
    "WGDPServiceInstance",
    "VitrualizationClusters"
]
```

Получение списка всех <a href='https://confluence.wargaming.net/display/GLMNT/%5BWGCMDB%5D+General+Overview#id-[WGCMDB]GeneralOverview-Class(%D0%9A%D0%BB%D0%B0%D1%81%D1%81'>классов</a>, созданных в CMDB. 

### HTTP Request

`GET /class`

<!-- <aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside> -->

## Получение атрибутов класса

> Sample request:

```shell
curl -ik \
    -H 'CMDBuild-Authorization: ecf46766-3b7f-4d1e-bd86-1e361c395fe6' \
    -H 'Accept: application/json' \
    -X GET https://test.cfg.iv/services/rest/v3/class/Application
```

> Sample response:

```shell
[
    {
        "code":"Description",
        "description":"Name",
        "apiName":"name",
        "type":"StringAttribute"
    },
    {
        "code":"Notes",
        "description":"Notes",
        "apiName":"notes",
        "type":"TextAttribute"
    },
    {
        "code":"Game",
        "description":"Game",
        "apiName":"game",
        "type":"ReferenceAttribute"
    },
    ...
    {
        "code":"ProjectVersion",
        "description":"Project Version",
        "apiName":"project_version",
        "type":"ReferenceAttribute"
    }
]
```

Получение списка атрибутов выбранного класса. 

<!-- <aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside> -->

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
{classname} | Имя класса, список атрибутов которого необходимо получить.

### Body Parameters

Parameter | Description | Data type
--------- | ----------- | -------
code | Уникальное имя атрибута. | String
description | Имя, под которым атрибут отображается в пользовательском интерфейсе CMDBuild. | String
apiName | Имя атрибута для упрощенных запросов. | String
type | Тип атрибута. Побдоробнее о типах атрибутов см. <a href='https://confluence.wargaming.net/display/GLMNT/%5BWGCMDB%5D+General+Overview#id-[WGCMDB]GeneralOverview-Class(%D0%9A%D0%BB%D0%B0%D1%81%D1%81'>Терминология CMDB</a>. | String

## Удаление карточки

> Sample request

```shell
curl -ik \
    -H 'Accept: application/json' \
    -H 'X-HTTP-Method-Override: DELETE' \
    -H 'CMDBuild-Authorization: ecf46766-3b7f-4d1e-bd86-1e361c395fe6' \
    -X DELETE https://test.cfg.iv/services/rest/v3/class/Realm/card/test_4
```
> Sample response

```shell
HTTP/1.1 200 OK
```

Удаление карточки из выбранного класса.

### HTTP Request

`DELETE /class/{classname}/card/{key}`

### Path Parameters

Parameter | Description
--------- | -----------
{classname} | Имя класса, которому принадлежит карточка.
{key} | Ключ карточки — уникальное значение атрибута ID, Code или Description.

<aside class="warning">
Удаленные карточки не исчезают из базы данных: вместо этого, их статус меняются с A (Active) или U (Updated) на N (Inactive/Removed).
</aside>

# Cards API

# History API

