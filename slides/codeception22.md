## Поддержка Gherkin 

* запускайте feature-файлы с остальными тестами
* feature != test
* тесты могут зависеть от feature

---

## Модуль для AngularJS

* Сделан на основе Protractor
* Легковесная надстройка над WebDriver

---

## Модуль DataFactory

```php
<?php
$I->have('User', ['active' => true]);
```

* генерирует данные для теста
* сохраняет и очищает БД
* использует ORM (Doctrine, Eloquent, Yii AR, ...)

---

## Dependencies

```java
@depends UserCest:login
@depends Registreation:user should be created
```

* Любой тест может зависеть от другого теста
* Тесты сотируются в нужном порядке

---

## Examples

```java 
@example statusCode [200, 400, 404]
@example request ['/valid', 'invalid', 'redirect']
```
* Альтернатива к DataProvider
* Поддерживается во всех форматах тестов