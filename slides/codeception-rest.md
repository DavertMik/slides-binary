## Тестирование REST

* отправка запроса
* ~~проверка ответа на равенство строке~~
* проверка структуры данных
* проверка на вхождение определенных значений 

---

```php
<?php
$I->sendPOST('/users', ['name' => 'davert', 'email' => 'davert@codeception.com']);
$I->seeResponseCodeIs(200);
$I->seeResponseIsJson();
$I->seeResponseMatchesJsonType([
  'user' => [
    'name' => 'string',
    'email' => 'string:email',
    'created_at' => 'datetime'
]]);3
$I->seeResponseContainsJson([
    'user' => [
      'name' => 'davert',
      'email' => 'davert@codeception.com',
]]);
```