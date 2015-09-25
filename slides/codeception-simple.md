## Простой тест

```php
<?php
$I = new AcceptanceTester($scenario);
$I->amOnPage('/');
$I->click('About');
$I->see('About Team');
$I->seeInTitle('About');
$I->seeCurrentEquals('/about');
```