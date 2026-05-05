# Лабораторная работа №2

## Иерархия классов

Абстрактный класс `Monster` и дочерние классы: `Goblin`, `Mermaid`, `Dragon`.

---

## Monster (абстрактный класс)

Поля: name, health, damage. Статический счётчик count отслеживает количество созданных монстров.

Методы: геттеры/сеттеры, InfoMonster() выводит информацию, абстрактные attack() и specialAbility().

---

## Goblin (наследуется от Monster)

Поля: stealth (скрытность), collectedCoins (собранные монеты).

Методы:
- `attack()` — атака копьём
- `specialAbility()` — скрытное перемещение
- `GoblinCheck()` — проверяет, заметили ли гоблина
- `CollectCoin()` — собирает монету

---

## Mermaid (наследуется от Monster)

Поля: charm (очарование).

Методы:
- `attack()` — атака зубами
- `specialAbility()` — очарование
- `startSinging()` — начинает петь
- `createFog()` — создаёт туман, увеличивает урон на 5

---

## Dragon (наследуется от Monster)

Поля: flightSpeed (скорость полёта), isFlying (состояние полёта).

Методы:
- `attack()` — атака огненными шарами
- `specialAbility()` — отображает скорость полёта
- `checkFly()` — проверяет, в небе ли дракон
- `isAttacking()` — оценивает скорость дракона

---

## Запуск программы

```bash
javac laba2/*.java
java laba2.Main
