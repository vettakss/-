# Лабораторная работа №6: Коллекции в Java

## Описание работы программ

### Задание 1. Топ-10 самых частых слов в файле
Программа читает текстовый файл `text.txt`, построчно разбивает его на слова, приводит их к нижнему регистру и удаляет знаки препинания. Для подсчёта количества вхождений используется `HashMap<String, Integer>`, где ключ — слово, значение — количество повторений. После обработки всего файла записи сортируются по убыванию частоты, и на экран выводится топ-10 самых частых слов.

### Задание 2. Обобщённый стек
Реализован обобщённый класс `Stack<T>`, который работает по принципу LIFO (последним пришёл — первым вышел). Внутри — массив объектов типа `T`. Метод `push` добавляет элемент в вершину стека, `pop` удаляет и возвращает верхний элемент, `peek` возвращает верхний элемент без удаления. При переполнении стека или попытке извлечь элемент из пустого стека выбрасывается исключение `RuntimeException`. В классе `Main` показан пример использования.

### Задание 3. Учёт продаж в магазине
Программа хранит информацию о проданных товарах в `TreeSet`. Класс `Product` содержит название, цену и количество проданных единиц. `TreeSet` автоматически сортирует товары по названию (благодаря реализации `Comparable`). `SalesManager` добавляет продажи (если товар уже есть, увеличивается его счётчик), выводит список всех товаров с количеством продаж, считает общую сумму и находит самый популярный товар.

---

## Коды программ

### Задание 1

```java
package laba61;
import java.io.File;
import java.io.FileNotFoundException;
import java.util.*;

public class TopWords {
    public static void main(String[] args) {
        String filePath = "text.txt";
        File file = new File(filePath);
        Scanner scanner = null;
        try {
            scanner = new Scanner(file);
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        Map<String, Integer> wordCount = new HashMap<>();

        while (scanner != null && scanner.hasNext()) {
            String word = scanner.next().toLowerCase().replaceAll("[^a-zа-я0-9]", "");
            if (word.isEmpty()) continue;
            wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
        }

        if (scanner != null) scanner.close();

        List<Map.Entry<String, Integer>> list = new ArrayList<>(wordCount.entrySet());
        Collections.sort(list, new Comparator<Map.Entry<String, Integer>>() {
            @Override
            public int compare(Map.Entry<String, Integer> o1, Map.Entry<String, Integer> o2) {
                int cmp = o2.getValue().compareTo(o1.getValue());
                if (cmp != 0) return cmp;
                return o1.getKey().compareTo(o2.getKey());
            }
        });

        System.out.println("Топ-10 слов:");
        for (int i = 0; i < Math.min(10, list.size()); i++) {
            System.out.println((i + 1) + ". " + list.get(i).getKey() + ": " + list.get(i).getValue());
        }
    }
}
Задание 2
java
// Stack.java
package laba62;

public class Stack<T> {
    private T[] data;
    private int size;

    @SuppressWarnings("unchecked")
    public Stack(int capacity) {
        data = (T[]) new Object[capacity];
        size = 0;
    }

    public void push(T element) {
        if (size == data.length) {
            throw new RuntimeException("Stack overflow");
        }
        data[size++] = element;
    }

    public T pop() {
        if (size == 0) {
            throw new RuntimeException("Stack is empty");
        }
        return data[--size];
    }

    public T peek() {
        if (size == 0) {
            throw new RuntimeException("Stack is empty");
        }
        return data[size - 1];
    }
}
java
// Main.java
package laba62;

public class Main {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>(10);
        stack.push(1);
        stack.push(2);
        stack.push(3);
        System.out.println(stack.pop());  // 3
        System.out.println(stack.peek()); // 2
        stack.push(4);
        System.out.println(stack.pop());  // 4
    }
}
Задание 3
java
// Product.java
package laba63;

public class Product implements Comparable<Product> {
    String name;
    double price;
    int count;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
        this.count = 1;
    }

    @Override
    public int compareTo(Product o) {
        return this.name.compareTo(o.name);
    }

    @Override
    public String toString() {
        return name + " - " + price + " руб. (продано " + count + " раз)";
    }
}
java
// SalesManager.java
package laba63;
import java.util.*;

public class SalesManager {
    private TreeSet<Product> soldProducts = new TreeSet<>();

    public void addSale(String name, double price) {
        for (Product p : soldProducts) {
            if (p.name.equals(name)) {
                p.count++;
                return;
            }
        }
        soldProducts.add(new Product(name, price));
    }

    public void printSales() {
        System.out.println("Список проданных товаров:");
        for (Product p : soldProducts) {
            System.out.println(p);
        }
    }

    public double totalSum() {
        double sum = 0;
        for (Product p : soldProducts) {
            sum += p.price * p.count;
        }
        return sum;
    }

    public Product mostPopular() {
        Product best = null;
        for (Product p : soldProducts) {
            if (best == null || p.count > best.count) best = p;
        }
        return best;
    }
}
java
// MainStore.java
package laba63;

public class MainStore {
    public static void main(String[] args) {
        SalesManager manager = new SalesManager();

        manager.addSale("Хлеб", 40);
        manager.addSale("Молоко", 70);
        manager.addSale("Хлеб", 40);
        manager.addSale("Сок", 120);
        manager.addSale("Хлеб", 40);

        manager.printSales();

        System.out.println("Общая сумма: " + manager.totalSum());
        System.out.println("Самый популярный товар: " + manager.mostPopular());
    }
}
text
