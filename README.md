# Базы данных, их типы.

### Задание 1. СУБД

### Крупная строительная компания, которая так же занимается проектированием и девелопментом, решила создать правильную архитектуру для работы с данными. Ниже представлены задачи, которые необходимо решить для каждой предметной области.

### Какие типы СУБД, на Ваш взгляд, лучше всего подойдут для решения этих задач и почему?

1.1 Бюджетирование проектов с дальнейшим формированием финансовых аналитических отчетов и прогнозирования рисков (СУБД должна гарантировать целостность и четкую структуру данных).

*Поскольку целостность важнее прочих параметров, лучше всего использовать СУБД реляционного типа, где данные защищены от порчи или разрывов связей. Подойдет Postgresql. Четкую структуру данных могут дать и другие СУБД, даже и нереляционные, но целостность - нет. Поэтому только реляционная.*


1.2 Под каждый девелоперский проект создается отдельный лендинг и все данные по лидам стекаются в CRM к маркетологам и менеджерам по продажам. Какой тип СУБД лучше использовать для лендингов и для CRM? (СУБД должны быть гибкими и быстрыми).

*В случае потребности в быстрой и гибкой СУБД реляционные не подойдут. Подходят или документо-ориентированные, либо графовые. В данном случае сложно выбрать одну идеальную СУБД, но я бы остановился на документо-ориентированной. Можно сделать json с ключом верхнего уровня (проект), ключ второго уровня - лид, и по нему производить поиск.*


1.3 Отдел контроля качества решил создать базу по корпоративным нормам и правилам, обучающему материалу и так далее, сформированную согласно структуры компании (СУБД должна иметь простую и понятную структуру).

*Для описания структуры компания целостность не столь критична, скорость работы СУБД также, информация не ветвится хаотично, все данные зависят друг от друга иерархично. Поэтому СУБД лучше всего подойдет тоже иерархичная.*


1.4 Департамент логистики нуждается в решении задач по быстрому формированию маршрутов доставки материалов по объектам и распределению курьеров по маршрутам с доставкой документов (СУБД должна уметь быстро работать со связями).

*Поскольку все адреса доставки и все курьеры так или иначе могут пересекаться в течение рабочего дня, лучше всего использовать графовую СУБД, потому что информация очень быстро ветвится, и чаще всего не иерархично, а более хаотично, поэтому связь "родитель-потомок" здесь не подойдет.*

---
### Задание 2. Транзакции.

### 2.1 Пользователь пополняет баланс счета телефона, распишите пошагово какие действия должны произойти для того, чтобы транзакция завершилась успешно (ориентируйтесь на 6 действий).

1. Занесение информации о сумме платежа в переменную N (все переменные условные).
2. Чтение информации из базы счетов пользователей о количестве средств на карте/счете пользователя и занесение информации в переменную A.
3. Сравнение N и А, должно быть A>=N.
4. Изменение данных (UPDATE) о средствах на карте/счете пользователя (операция вычитания A-N).
5.  Чтение информации из базы балансов пользователей и занесение информации в переменную  В.
6. Изменение данных (UPDATE) о балансе пользователя (операция сложения B+N).
7. Повторное чтение информации из базы счетов пользователей о количестве средств на карте/счете пользователя и занесение информации в переменную A2.
8. Повторное чтение информации из базы балансов пользователей и занесение информации в переменную  В2.
9. Проверка на равенство значений (сравнение результатов операций A - A2 и B2 - B), в случае TRUE транзакция завершается успешно.

---
### Задание 3. SQL vs NoSQL

### 3.1 Напишите 5 преимуществ SQL систем по отношению к NoSQL.
1. SQL системы предполагают целостность данных. Данные не могут удалиться из базы, посколько они связаны внешними ключами.
2. SQL системы обычно подразумевают первичные ключи, которые уникальны, следовательно поиск по ним выдаст гарантированно нужный результат.
3. SQL системы соответствуют всем требованиям ACID.
4. SQL системы подразумевают правило "schema first", что дает гарантию того, что данные запишутся в нужную схему, а в NoSQL нет заранее определенной схемы.
5. SQL системы позволяют делать сложные запросы, использовать join, группировки, агрегации и тд, в отличие от NoSQL, где можно делать только простые запросы. 

---
### Задание 4. Кластеры

### Необходимо производить большое количество вычислений при работе с огромным количеством данных, под эту задачу выделено 1000 машин.

### На основе какого критерия будете выбирать тип СУБД и какая модель распределенных вычислений здесь справится лучше всего и почему?

Важно понимать, какого рода данные будут обрабатываться, будут ли запросы сложными или простыми, и что будет производиться больше - накопление данных или их чтение. Если сложные запросы, то SQL, простые - NoSQL. Опять же, если данные друг с другом не связаны, чтение будет частым и важна скорость чтения, лучше NoSQL, а если редко и скорость чтения не принципиальна, то SQL. Поскольку в задаче указано, что вычислений будет много, значит скорость критична, скорее всего нужен NoSQL. Без понимания природы данных сложно сказать что-то конкретное.

Если данные однотипные, то технология MapReduce отлично подойдет здесь, посколько она поддерживается многими языками программирования. Она принимает на вход данные, и две функции высшего порядка map и reduce сначала обрабатывают распараллеленные данные, а потом схлопывают обработанные данные по нужным алгоритмам.
