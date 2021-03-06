# Структуры и объединения.
## Структуры
Если нам надо объенить множетсво элементов имеющих одинаковый тип, то мы можем их хранить в массиве. А что делать, если элементы имеют разный тип? На помощь приходят структуры. По сути, структура, так же как и массив, представляет из себя кусок памяти, в которой подряд (почти вплотную, зависит от выравнивания) лежат элементы.
### Основные основы
Пример описания структуры:
````C
struct Hero {
    char name[255];
    int posX;
    int posY;
    double health;
};
````
Данная структура содержит в себе поля:

`char name[255]`   - имя героя

`int posX, posY`   - положение на карте

`double health`    - здоровье

Важно отметить, что структура - это тип, то есть после данного выше кода не создается каких-либо объектов.

После описания структуры мы можем создать объект:
````C
struct Hero batman;
struct Hero superman;
````

Обращение к полям структуры производится через точку:
````C
strcpy(batman.name, "bruce");
batman.health = 100;
batman.posX   = 228;
batman.posY   = 322;
````
При создании и использовании указателей, для обращения к элементам помимо полного синтаксиса (`(*p).data`) можно использовать стрелочную нотацию (`p->data`)
````C
struct Hero *batman_ptr;
(*batman_ptr).health = 100;
batman_ptr->health   = 100; // Идентично предыдущей строчке
````
Обратите внимание на точку с запятой после закрывающей фигурной скобки в описании структуры. Почему она здесь нужна? На самом деле, после описания структуры мы можем сразу создать экземпляры.
````C
struct point {
    int x;
    int y;
} a, b;

a.x = 10;
b.x = a.x;
````
В данном примере можно даже убрать имя типа (если вы не собираетесь дальше создавать объекты такого же типа):
````C
struct {
    int x;
    int y;
} a, b;

a.x = 10;
b.x = a.x;
````
### Приколюха с `typedef`
Как вы могли заметить, в Си, в отличие от плюсов, нельзя создать объект напрямую:
````C
Hero batman;
````
Вместо этого в Си приходится писать так:
````C
struct Hero batman;
````
Это ограничение можно легко обойти с помощью `typedef`:
````C
typedef struct {
    char name[255];
    int posX;
    int posY;
    double health;
} Hero;
Hero batman; // Работает в Си!
````
Почему это работает? Мы создаем псевдоним Hero имени типа нашей структуры. То есть, при написании `Hero` по факту будет "подставляться" описание нашей структуры.

### Выравнивание
Как расположены поля структуры в памяти? Как я уже писал, они расположены подряд, НО, между ними могут образовываться небольшие зазоры - нужные для выравнивания данных. В результате, порядок описания полей может влиять на размер структуры.

Выравнивание в структурах можно отключить с помощью всякой компиляторно-зависимой магии (это может быть удобно, например, при записи структур в файлы или передаче их по сети).
### Инициализация
При создании объекта мы можем сразу проинициализировать его поля некоторыми значениями

````C
typedef struct {
    int x, y, z;
} point;

point a = {0, 0, 0};
````
Обратите внимание: фигурные скобочки допустимы только при инициализации, но не при присваивании значения
````C
point a = {0, 0, 0}; // OK!
point b;
b = {0, 0, 0}; // Ошибка!
````
В новых версиях стандарта можно также явно указывать имя поля, которое инициализируется. Если указать меньше инициализаторов, чем количество полей – то остальные поля будут инициализированы в 0.
````C
point a = {.x = 0, .y = 1}; // z = 0;
````
### Безымянные структуры
В Си (но не в плюсах) можно создавать безымянные объекты типа структуры таким образом:
`(имя типа){инициализаторы}`. Это удобно при присваивании значений структурам.
````C
point a;
a = (point){.x = 0};
````
### Еще одно отличие от С++
В С++ следующая конструкция позволяет задавать значения по умолчанию полям структуры:
````C
struct point {
    int x = 0;
    int y = 0;
}

point a; // a.x = a.y = 0 
````
В С такого нет :c

## Объединения

Объединения - это почти то же самое, что и структуры, и отличаются только одним: в объединениях все поля лежат друг поверх друга. То есть, размер объединения равен размеру его самого большого поля.
````C
union lol {
    int x;
    double y;
};

struct kek {
    int x;
    double y;
};
````

В данном примере `sizeof(lol) = max(sizeof(lol.x), sizeof(lol.y))`, а `sizeof(kek) = sizeof(kek.x) + sizeof(kek.y) + выравнивание`

Зачем нужны объединения? А хз.