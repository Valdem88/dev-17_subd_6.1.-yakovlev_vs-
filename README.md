# Домашнее задание к занятию "6.1. Типы и структура СУБД" dev-17_subd_6.1.-yakovlev_vs

## Задача 1

Архитектор ПО решил проконсультироваться у вас, какой тип БД 
лучше выбрать для хранения определенных данных.

Он вам предоставил следующие типы сущностей, которые нужно будет хранить в БД:

- Электронные чеки в json виде
- Склады и автомобильные дороги для логистической компании
- Генеалогические деревья
- Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации
- Отношения клиент-покупка для интернет-магазина

Выберите подходящие типы СУБД для каждой сущности и объясните свой выбор.

#### Решение

- Электронные чеки в json виде
```
Документо-ориентированные - данные хранятся в виде документов как ключ-значение по аналогии со структурой json
```

- Склады и автомобильные дороги для логистической компании
```
Реляционные - будем хранить данные в таблицах с соотвествующими ссылками между таблицами. Логистику можно хранить в графовых (отношения машин, пунктов и дорог).
```

- Генеалогические деревья
```
Иерархические - потому что здесь данные организованы в виде дерева
```

- Кэш идентификаторов клиентов с ограниченным временем жизни для движка аутенфикации
```
Ключ-значение - данные представлены в виде уникально ключа и его значения.
```

- Отношения клиент-покупка для интернет-магазина
```
Графовые - здесь узлами будут клиенты, а ребрами - что купил.
```



## Задача 2

Вы создали распределенное высоконагруженное приложение и хотите классифицировать его согласно 
CAP-теореме. Какой классификации по CAP-теореме соответствует ваша система, если 
(каждый пункт - это отдельная реализация вашей системы и для каждого пункта надо привести классификацию):

- Данные записываются на все узлы с задержкой до часа (асинхронная запись)
- При сетевых сбоях, система может разделиться на 2 раздельных кластера
- Система может не прислать корректный ответ или сбросить соединение

А согласно PACELC-теореме, как бы вы классифицировали данные реализации?

#### Решение

#### Согласно CAP-теореме:
- Данные записываются на все узлы с задержкой до часа (асинхронная запись)
CP обеспечивается согласованность и устойчивость к разделению в ущерб доступности.
- При сетевых сбоях, система может разделиться на 2 раздельных кластера
AP обеспечивается доступность и устойчивость к разделению в ущерб согласованности.
- Система может не прислать корректный ответ или сбросить соединение
CP обеспечивается согласованность и устойчивость.

#### Согласно PACELC-теореме:
- Данные записываются на все узлы с задержкой до часа (асинхронная запись)
PC/EC
- При сетевых сбоях, система может разделиться на 2 раздельных кластера
PA/EL
- Система может не прислать корректный ответ или сбросить соединение
PC/EC

## Задача 3

Могут ли в одной системе сочетаться принципы BASE и ACID? Почему?

#### Решение

Не могут. BASE ориентирован на производительность системы (доступность), ACID на сохранность данных (стойкость к разделению и сохранность данных). Согласно CAP-теореме системы с такими качествами будут находиться на разных ребрах треугольника.

## Задача 4

Вам дали задачу написать системное решение, основой которого бы послужили:

- фиксация некоторых значений с временем жизни
- реакция на истечение таймаута

Вы слышали о key-value хранилище, которое имеет механизм [Pub/Sub](https://habr.com/ru/post/278237/). 
Что это за система? Какие минусы выбора данной системы?

#### Решение 

Сам не сталкивался с подобной задачей. Нашел в инете что Redis подходит. Плюсы и минусы, что о нем пишут.

Redis - это СУБД типа key-value. Может использоваться для реализации кэшей, брокеров сообщений (механизм pub/sub). Ориентирована на высокую производительность. В отличие от классических key-value систем в том, что позволяет хранить значения ключей не только в виде строк, но и в виде списков, хэш-таблиц, упорядоченных множеств. 
Redis хранит базу данных в опреативной памяти, из-за этого, если система не имеет реплик, возможны потери части данных при падении сервера, но Redis снабжена механизмами снимков и журналирования для обеспечения постоянного хранения (на дисках, твердотельных накопителях). 
Минус системы репликации Redis в том, что сама по себе она не поддерживает автоматическую отказоустойчивость: если ведущий узел выходит из строя, необходимо вручную выбрать нового ведущего среди ведомых узлов. Но имеется система Redis Sentinel, обеспечивающая мониторинг и автоматическое переключение.