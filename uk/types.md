# Типи

Zephir поєднує в собі статичну та динамічну типізацію. У цьому розділі ми підкреслимо підтримувані типи та їхню поведінку.

<a name='dynamic-types'></a>

## Динамічні типи

Динамічних змінні працюють так само, як і в PHP. Їм можна призначати та перепризначати значення різних типів без обмежень.

Динамічна змінна має бути оголошеною з ключовим словом `var`. Поведінка майже така сама, як і в PHP:

    var a, b, c;
    

##### Ініціалізація змінних

    let a = "hello", b = false;
    

##### Зміна їхнього значення

    let a = "hello", b = false;
    let a = 10, b = "140";
    

##### Виконання операцій

    let c = a + b;
    

Вони можуть бути восьми типів:

| Тип              | Опис                                                                                                |
| ---------------- | --------------------------------------------------------------------------------------------------- |
| `array`          | Масив є впорядкованою мапою. Мапа - це тип, який встановлює відповідність між значеннями і ключами. |
| `boolean`        | Булевий тип виражає значення істини. Він може бути `істиною` або `хибністю`                         |
| `float`/`double` | Число з рухомою комою. Розмір числа залежить від платформи.                                         |
| `integer`        | Цілі числа. Розмір числа залежить від платформи.                                                    |
| `null`           | Особливе значення NULL, яке помічає змінну в якої немає значення.                                   |
| `object`         | Об'єкт абстракції як у PHP.                                                                         |
| `resource`       | Ресурс містить посилання на зовнішній ресурс.                                                       |
| `string`         | Рядок це серія символів, де символ є таким самим, як байт.                                          |

Більше про типи ви можете дізнатися в [Документації PHP](http://www.php.net/manual/en/language.types.php).

<a name='dynamic-types-arrays'></a>

### Масиви

Реалізація масивів у Zephir в основному така сама як у PHP: впорядковані мапи оптимізовані для деяких випадків; можна розглядати як масив список (вектор) хеш-таблицю (реалізація мапи), словник, колекція, стек, черги. Значеннями масиву можуть бути інші масиви, дерева, та багатовимірні масиви.

Синтаксис оголошення масиву дещо відрізняється від PHP:

##### Для оголошення повинні використовуватися квадратні дужки

    let myArray = [1, 2, 3];
    

##### Для оголошення масиву з ключами повинна використовуватися двокрапка

    let myHash = ["first": 1, "second": 2, "third": 3];
    

Ключами масиву можуть лише цілі числа та рядки:

    let myHash = [0: "first", 1: true, 2: null];
    let myHash = ["first": 7.0, "second": "some string", "third": false];
    

<a name='dynamic-types-boolean'></a>

### Логічний тип

Булевий тип виражає значення істини. Він може бути `true` або `false`:

    var a = false, b = true;
    

<a name='dynamic-types-float-double'></a>

### Двійкові числа/Числа з подвійною точністю

Числа з рухомою комою (також відомі як "двійкові", "числа з подвійною точністю", "дійсні числа"). Літерали з рухомою точкою - це вирази з однією або кількома цифрами, а потім - період (.), після нього одна або кілька цифр. Розмір числа після крапки залежить від платформи, хоча максимум ~1.8e308 з точністю приблизно 14 десяткових цифр мають загальне значення (64-біт сумісні з IEEE форматі).

    var number = 5.0, b = 0.014;
    

Floating point numbers have limited precision. Although it depends on the system, Zephir uses the same IEEE 754 double precision format used by PHP, which will give a maximum relative error due to rounding in the order of 1.11e-16.

<a name='dynamic-types-integer'></a>

### Ціле число

Цілі числа. Розмір числа залежить від платформи, хоча максимальним значенням є число трохи більше за 2 мільярди (2,147,483,647). 64-розрядні платформи зазвичай мають максимальне значення близько 9E18. PHP не підтримує цілі числа без знаку, тому Zephir теж має це обмеження:

    var a = 5, b = 10050;
    

<a name='dynamic-types-integer-overflow'></a>

### Програмне переповнення цілих чисел

На відміну від PHP Zephir автоматично не перевіряє рівень заповненості цілого числа. Як і в C, якщо ви виконуєте операції які можуть повернути велике число ви повинні використати такі типи як `unsigned long` або ж `float`:

    unsigned long my_number = 2147483648;
    

<a name='dynamic-types-objects'></a>

### Об'єкти

Zephir дозволяє створювати екземпляр класу, маніпулювати, викликати методи, читати константи класу та інші речі, які дозволяють PHP-об'єкти:

    let myObject = new stdClass(),
        myObject->someProperty = "my value";
    

<a name='dynamic-types-string'></a>

### Рядок

Рядок це серія символів, де кожен символ є таким самим, як байт. Як і PHP, Zephir підтримує лише 256 набір символів, а отже не дає вбудованої підтримки Юнікоду.

    var today = "friday";
    

У Zephir рядкові літерали можна задавати лише взявши їх у подвійні лапки (як у C або Go). Одинарні лапки зарезервовані для символів.

У рядках підтримуються наступні символи екранування:

| Послідовність | Опис                   |
| ------------- | ---------------------- |
| `\\t`       | Горизонтальний відступ |
| `\\n`       | Переведення рядка      |
| `\\r`       | Повернення каретки     |
| `\\ \`   | Бекслеш                |
| `\\"`       | Подвійні лапки         |

    var today    = "\tfriday\n\r",
        tomorrow = "\tsaturday";
    

Zephir не підтримує парсинг змінних у рядках як це було в PHP; ви повинні використовувати конкатенацію:

    var name = "peter";
    
    echo "hello: " . name;
    

<a name='static-types'></a>

## Статичні типи

Статичні типи дозволяють розробнику оголосити та використовувати змінні з певними типами, які доступні у C. Змінна, яка оголошена з статичним типом не може змінювати свій тип. Проте, це дозволяє компілятору провести кращу оптимізацію. Підтримуються наступні типи:

| Тип                | Опис                                                                                |
| ------------------ | ----------------------------------------------------------------------------------- |
| `array`            | Структура, яка може бути використана як хеш, мапа, словник, колекції, стек, і т. д. |
| `boolean`          | Булевий тип виражає значення істини. Він може бути `true` або `false`.              |
| `char`             | Smallest addressable unit of the machine that can contain basic character set.      |
| `float`/`double`   | Double precision floating-point type. The size is platform-dependent.               |
| `integer`          | Signed integers. At least 16 bits in size.                                          |
| `long`             | Long signed integer type. At least 32 bits in size.                                 |
| `string`           | A string is a series of characters, where a character is the same as a byte.        |
| `unsigned char`    | Same size as char, but guaranteed to be unsigned.                                   |
| `unsigned integer` | Unsigned integers. At least 16 bits in size.                                        |
| `unsigned long`    | Same as long, but unsigned.                                                         |

<a name='static-types-boolean'></a>

### Логічний тип

Булевий тип виражає значення істини. Він може бути `true` або `false`. На відміну від поведінки динамічного типу, який описаний вище - статичні булеві типи залишаються булевими, не залежно від того, яке значення їм призначається:

    boolean a;
    let a = true;
    

##### автоматично змінюється на `true`

    let a = 100;
    

##### автоматично змінюється на `false`

    let a = 0
    

##### кидає виняток компіляції

    let a = "hello";
    

<a name='static-types-char-unsigned'></a>

### Char/Unsigned Char

Char variables are the smallest addressable unit of the machine that can contain the basic character set (generally 8 bits). Змінна типу `char` може використовуватися для зберігання будь-яких символів у рядку:

    char ch, string name = "peter";
    

##### stores 't'

    let ch = name[2];
    

##### `char` literals must be enclosed in single quotes

    let ch = 'Z';
    

<a name='static-types-integer-unsigned'></a>

### Integer/Unsigned Integer

Integer values are like the integer member in dynamic values. Values assigned to integer variables remain integer:

    int a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

Unsigned integers are like integers but they don't have sign, this means you can't store negative numbers in these sort of variables:

    uint a;
    
    let a = 50;
    

##### automatically casted to 70

    let a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

Unsigned integers are twice bigger than standard integers. Assigning unsigned integers to standard (signed) integers may result in loss of data:

##### potential loss of data for `b`

    uint a, int b;
    
    let a = 2147483648,
        b = a;
    

<a name='static-types-long-unsigned'></a>

### Long/Unsigned Long

Long variables are twice bigger than integer variables, thus they can store bigger numbers. As with integers, values assigned to long variables are automatically casted to this type:

    long a;
    
    let a = 50,
        a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

Unsigned longs are like longs but they are not signed, this means you can't store negative numbers in these sort of variables:

    ulong a;
    
    let a = 50;
    

##### automatically casted to 70

    let  a = -70;
    

##### automatically casted to 100

    let a = 100.25;
    

##### automatically casted to 0

    let a = null;
    

##### automatically casted to 0

    let a = false;
    

##### кидає виняток компіляції

    let a = "hello";
    

Unsigned longs are twice bigger than standard longs; assigning unsigned longs to standard (signed) longs may result in loss of data:

##### potential loss of data for `b`

    ulong a, long b;
    
    let a = 4294967296,
        b = a;
    

<a name='static-types-string'></a>

### String

A string is series of characters, where a character is the same as a byte. As in PHP it only supports a 256-character set, and hence does not offer native Unicode support.

Коли змінна оголошується як `string` вона ніколи не змінить свого типу:

    string a;
    
    let a = "";
    

##### string literals must be enclosed in double quotes

    let  a = "hello";
    

##### converted to string "A"

    let a = 'A';
    

##### automatically casted to ""

    let a = null;