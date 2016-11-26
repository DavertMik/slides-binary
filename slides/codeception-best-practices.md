## Приемочные тесты

* Проверяют работу системы вцелом
* Опираются на UI (в том числе JavaScript)
* Требуют БД, веб-сервер
* Работают через браузер, Selenium

---

### Selenium Webdriver Example

```php
// Custom method
$I->loginAsAdmin();
// Navigate to the Media Library
$I->amOnPage( '/wp-admin/media-new.php' );
$I->waitForText( 'Upload New Media' );

// Add new file
$I->attachFile( 'input[type="file"]', 'team.jpg' );

// Wait for upload
$I->waitForElement( '.edit-attachment', 20 );

// Assertion
$I->see( 'Edit Media' );
```

via [@polevaultweb](https://deliciousbrains.com/codeception-automate-wordpress-plugin-testing/)

---


![](img/codeception.gif)

via [@polevaultweb](https://deliciousbrains.com/codeception-automate-wordpress-plugin-testing/)

---

### Функциональные тесты

* Тестируют работу приложения без реального UI.
* Эмулируют запросы-ответы к приложению
* Интегрируются с фреймворком, ORM, ...
* Описаны в виде сценариев

---

```php
// входим как админ
$I->amLoggedInAs(User::findByUsername('admin'));
// используем внутренний роут Yii2
$I->amOnRoute('tasks/new');
// отправляем форму (эмулируем запрос к приложению)
$I->submitForm($this->formId, [
    'TaskForm[title]' => 'Fix bug',
    'TaskForm[priroity]' => 'urgent',
]);
// проверяем значения в базе
$I->seeRecord('common\models\Task', [
    'task' => 'Fix bug'
]);
```

---

## Модульные тесты

* Тестируют компоненты системы в изоляции
* С использованием Dependency Injection
* Такие же как в PHPUnit

---

```php
public function testCanReturnAnArrayOfStatusUpdates()
{
  $f = M::mock('FeedReaders\FeedReaderInterface');
  $f->shouldReceive('getMessages')
      ->once()
      ->andReturn(array('foo', 'bar'));
  $s = new SocialFeed($f);
  $v = $s->getArray();
  $this->assertCount(5, $v);
  $this->assertEquals($v[0], 'foo');
  $this->assertEquals($v[1], 'bar');
}
```

---

### Интеграционные тесты

* Тестируют компоненты системы
* Используют БД, другие компоненты
* Стандартный PHPUnit + модули Codeception

---

```php
class UserTest extends \Codeception\Test\Unit
{
    /**
     * @var \UnitTester
     */
    protected $tester;
    public function testRegister()
    {
        $email = 'johndoe@example.com';
        $password = Hash::make('password');
        User::register(['email' => $email, 'password' => $password]);
        $this->tester->seeRecord('users', ['email' => $email, 'password' => $password]);
    }
}
```

---

### API тесты

* Могут тестировать изнутри или снаружи
* Используют фреймворк + REST модули
* Позволяют тестировать JSON/XML ответы

---

 GET /api/tickets/3

```json
{
  "ticket": {
    "id": 3,
    "from": "web",
    "description": "Lorem ipsum...",
    "report": {
      "user_agent": "Mozilla...",
      "url": "/tasks",      
    },
    "reporter_info": {
      "email": "davert@codeception.com"
    },
    "created_at": "2016-08-21T20:16:37Z",
    "updated_at": "2016-09-11T15:13:47Z"    
  }
}
```

---

```php
$I->sendGET('/api/tickets/3');
$I->seeResponseCodeIs(HttpCode::OK); // 200
$I->seeResponseIsJson();
// проверка данных
$I->seeResponseContainsJson(['ticket' => [
  'id' => 3,
  'report' => [
    'url' => '/tasks'
  ]
]);
// проверка структуры
$I->seeResponseMatchesJsonType(['ticket' => [
  'id' => 'integer',
  'created_at' => 'string:date',
  'reporter_info' => [
    'email' => 'string:email'
  ]
]]);
```

---

# Принципы хороших тестов

---

## Разделение ответственности

* делайте тесты атомарными
* тест должен быть сосредоточен на одной фиче/баге
* вместо комментариев сделайте отдельный тест

---

## Уровни тестирования

* тестируйте изнутри и снаружи
* тщательно тестируйте бизнес-правила
* не усердствуйте в мелочах 
* проверяйте систему на безопасность

