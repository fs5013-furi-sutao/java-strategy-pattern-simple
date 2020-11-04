# Strategy パターンとは
インターフェースを利用することで、複数ある実装クラスを実行時に選べるパターンのこと。

## コーディング例
クラス設計が見やすいように、1つのクラスにすべてのインタフェース、クラスをまとめた。

```java
import java.util.ArrayList;
import java.util.List;

public class App {
    public static void main(String[] args) {
        App app = new App();
        app.startFightScene();
    }

    private void startFightScene() {
        IStrategy attackStrategy = new AttackStrategy();
        IStrategy defenceStrategy = new DefenceStrategy();

        List<Monster> monsters = new ArrayList<>();

        monsters.add(new Goblin("Joe", attackStrategy));
        monsters.add(new Dragon("Shane", attackStrategy));
        monsters.add(new Goblin("Smith", defenceStrategy));

        for (Monster monster : monsters) {
            monster.action();
            System.out.println();
        }
    }

    interface IStrategy {
        void action(Monster monster);
    }

    class AttackStrategy implements IStrategy {
        @Override
        public void action(Monster monster) {
            monster.attack();
        }
    }

    class DefenceStrategy implements IStrategy {
        @Override
        public void action(Monster monster) {
            monster.defence();
        }
    }

    interface Monster {
        void action();
        void attack();
        void defence();

        String getName();
    }

    class Goblin implements Monster {

        IStrategy strategy;
        String name;

        Goblin(String name, IStrategy strategy) {
            this.strategy = strategy;
            this.name = name;
        }

        @Override
        public void action() {
            System.out.printf("I'm Goblin %s.%n", this.name);
            System.out.println("Woof!");
            this.strategy.action(this);
        }

        @Override
        public void attack() {
            System.out.printf("Goblin %s attacked.%n", this.name);
        }

        @Override
        public void defence() {
            System.out.printf("Goblin %s defenced.%n", this.name);
        }

        @Override
        public String getName() {
            return this.name;
        }
    }

    class Dragon implements Monster {

        IStrategy strategy;
        String name;

        Dragon(String name, IStrategy strategy) {
            this.strategy = strategy;
            this.name = name;
        }

        @Override
        public void action() {
            System.out.printf("I'm Dragon %s.%n", this.name);
            System.out.println("Whirr...");
            this.strategy.action(this);
        }

        @Override
        public void attack() {
            System.out.printf("Dragon %s attacked.%n", this.name);
        }

        @Override
        public void defence() {
            System.out.printf("Dragon %s defenced.%n", this.name);
        }

        @Override
        public String getName() {
            return this.name;
        }
    }
}
```
