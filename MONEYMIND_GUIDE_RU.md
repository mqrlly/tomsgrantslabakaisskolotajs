# MoneyMind — инструкция по запуску и демонстрации

## Что реализовано

- Регистрация, вход и выход через Laravel Breeze. Все страницы управления финансами защищены middleware `auth`.
- Модели `Category` и `Transaction` связаны: категория имеет много транзакций, каждая транзакция относится к одной категории. Транзакция также принадлежит пользователю.
- Полный CRUD для транзакций: добавить, посмотреть, отредактировать и удалить.
- Категории `Salary`, `Food`, `Transport`, `Shopping`, `Other` заполняются через `CategorySeeder`. `DatabaseSeeder` вызывает его автоматически.
- Удаление транзакции на странице списка выполняется через AJAX (`fetch`) без полной перезагрузки страницы.
- Серверная валидация проверяет название, сумму, тип, категорию, дату и описание; сообщения об ошибках показываются рядом с полями.
- Доступ к каждой транзакции ограничен текущим пользователем; нельзя просматривать, менять или удалять чужие записи.
- Blade-шаблоны используют единый адаптивный интерфейс MoneyMind на английском языке. Стили финансовых страниц хранятся в `public/css/moneymind.css` и не зависят от запущенного Vite.

## Запуск в Windows PowerShell

Открой PowerShell и выполни по одной команде:

```powershell
cd C:\Users\sobol\Desktop\laravel-project
php artisan --version
php artisan migrate
php artisan db:seed --class=CategorySeeder
php artisan serve
```

Открой `http://localhost:8000`. Зарегистрируй свой аккаунт через **Get started / Register**, войди и открой **Transactions**.

`php artisan db:seed --class=CategorySeeder` безопасно запускать повторно: существующие категории не дублируются. Не запускай `php artisan migrate:fresh` на своей основной базе — эта команда удаляет таблицы и все данные.

Если ты распаковываешь именно полный архив проекта в новую папку, а файлов `.env` ещё нет, сначала создай его и установи зависимости:

```powershell
Copy-Item .env.example .env
composer install
php artisan key:generate
npm install
php artisan migrate
php artisan db:seed
php artisan serve
```

Для этих команд нужно открыть PowerShell в корне `laravel-project`. Если PowerShell сообщает, что `php` или `composer` не найдены, добавь соответствующие XAMPP/PHP и Composer пути в Windows PATH либо используй тот терминал, где команды уже работали.

## Проверка вручную перед сдачей

1. Зарегистрируй аккаунт, войди и открой **Transactions**.
2. Нажми **Add transaction**, введи заголовок, сумму, тип, категорию и дату, затем **Save transaction**. Запись должна появиться в таблице.
3. Открой **View**, затем **Edit**, поменяй описание или сумму и сохрани изменения.
4. Попробуй сохранить пустое название или сумму `0`: форма должна показать сообщение валидации.
5. Удали тестовую запись: строка и суммы должны обновиться без перезагрузки страницы.
6. Создай второй аккаунт и проверь, что он не видит транзакции первого аккаунта.

## Краткий текст для демонстрации на латышском

**MoneyMind ir tīmekļa lietotne personīgo finanšu uzskaitei. Lietotājs var reģistrēties un pieslēgties savam kontam, pievienot ienākumus un izdevumus, izvēlēties kategoriju, apskatīt, rediģēt un dzēst darījumus. Dati tiek glabāti SQLite datubāzē. Projektā ir divi saistīti modeļi — Category un Transaction — un darījumu CRUD funkcionalitāte. Kategorijas tiek pievienotas ar seeder, ievaddati tiek pārbaudīti servera pusē, bet dzēšana darījumu sarakstā notiek ar AJAX bez pilnas lapas pārlādes. Lietotāja dati ir aizsargāti ar autentifikāciju, un katrs lietotājs var piekļūt tikai saviem darījumiem. Saskarne ir veidota ar Laravel Blade veidnēm.**

## Automātiskās pārbaudes

Projektam pievienots `tests/Feature/TransactionFeatureTest.php`, kas pārbauda piekļuvi bez autentifikācijas, CRUD, validāciju, piekļuves ierobežošanu starp lietotājiem un AJAX dzēšanu. Izstrādes pārbaudes vidē PHP trūkst `dom`, `mbstring` un `pdo_sqlite` paplašinājumu, tāpēc pilnu PHPUnit testu izpildi šajā vidē nevarēja pabeigt. Pēc projekta atvēršanas XAMPP PHP vidē testus var palaist ar `php artisan test`.
