### **Nested_Theory**

1) На какие две группы разделяются классы, объявленные внутри другого класса?
   Как они называются на инглише?
   
**Ответ:** В Java классы, объявленные внутри другого класса, разделяются на 2 группы:
-  Вложенные классы (англ. nested classes) - классы, являющиеся статическими классами-членами.
-  Внутренние классы (англ. inner classes) - не статические классы-члены, анонимные классы, локальные классы.
   
**Источник:** Дж. Блох “Эффективное программирование” Статья 22

2) Для каких целей они используются?
   
**Ответ:** Неопровержимые причины использования вложенных классов включают следующее:
   •	Это способ логической группировки классов, которые используются только в одном месте : если класс полезен только для одного другого класса, то логично встроить его в этот класс и сохранить их вместе. Вложение таких «вспомогательных классов» делает их пакет более оптимизированным.
   •	Это увеличивает инкапсуляцию : рассмотрим два класса верхнего уровня, A и B, где B требуется доступ к членам A, которые в противном случае были бы объявлены private. Скрывая класс B внутри класса A, члены A могут быть объявлены частными, и B может получить к ним доступ. Кроме того, сам B может быть скрыт от внешнего мира.
   •	Это может привести к более удобочитаемому и поддерживаемому коду : вложение небольших классов в классы верхнего уровня помещает код ближе к тому месту, где он используется.
   
**Источник.** https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

3) Какие уровни доступа применяются к таким классам?
   
**Ответ:** К внутренним и вложенным классам применяются модификаторы private, protected,  public или их отсутствие (package-private).
   Анонимные и локальные классы объявляются без модификаторов доступа.
   
**Источник.** https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.1.1


4) Какие существуют варианты внутренних классов?
   
**Ответ:**
   ●     Внутренние классы (inner classes) - существующие только внутри содержащего его внешнего класса.
   а также два специальных дополнительных типа внутренних (inner) классов:
   ●     Локальные классы (local classes) - существующие только внутри тела метода
   ●      Анонимные классы (anonymous classes) - неименованные, существующие только внутри тела метода.
   
**Источник.** https://docs.oracle.com/javase/tutorial/java/javaOO/innerclasses.html

5) Пусть объявлен класс Outer, а внутри него публичный вложенный класс NestedPublic и публичный внутренний класс InnerPublic. Создайте экземпляры каждого класса:
   а) внутри класса Outer,
   б) извне класса Outer?
   
**Ответ:**
   а)
   public class Outer {
   InnerPublic innerPublic = new InnerPublic();
   NestedPublic nestedPublic = new NestedPublic();

   public class InnerPublic {}

   public static class NestedPublic {}
   }
   б)
   public class Runner {
   public static void main(String[] args) {
   Outer.InnerPublic innerPublic =  new Outer().new InnerPublic();
   Outer.NestedPublic nestedPublic = new Outer.NestedPublic();
   }
   }


6) Пусть объявлен класс Outer, а внутри него приватный вложенный класс NestedPrivate и приватный внутренний класс InnerPrivate. Создайте экземпляры каждого класса:
   а) внутри класса Outer,
   б) извне класса Outer?
   Ответ:
   а) внутри класса Outer
   public class Outer {
   private static class NestedPrivate { }
   private class InnerPrivate { }
   InnerPrivate innerPrivate = new InnerPrivate();
   NestedPrivate nestedPrivate = new NestedPrivate();
   }

б) извне класса Outer
Ошибка компиляции, т.к. внутренний класс объявлен как private, что обеспечивает его полную невидимость вне класса-владельца и надежное сокрытие реализации. Создать объект такого класса можно только в методах и логических блоках внешнего класса, например:
public class Outer {
private static class NestedPrivate {
@Override
public String toString() {
return "NestedPrivate";
}

 	private class InnerPrivate { 	}
 
 	public NestedPrivate getNestedPublic() {
 	   	return new NestedPrivate();
 	}
 	public InnerPrivate getInnerPrivate() {
 	   	return new InnerPrivate();
 	}
}
public class Runner {
public static void main(String[] args) {
Object nestedPrivate = new Outer().getNestedPublic();
Object innerPrivate = new Outer().getInnerPrivate();
System.out.println(nestedPrivate); // Вывод "NestedPrivate"
}
}
Но использование любых методов, кроме наследуемых от класса Object, невозможно, так как среда вне класса Outer не знает внутреннего устройства созданных объектов.

**Источник:** И.Н.Блинов, В.С.Романчик  Java. Методы программирования. - Учебно-методическое пособие. - Минск, 2013, c.133

7) Пусть объявлен класс Outer, а внутри него внутренний класс Inner. Как обратиться внутри класса Inner:
   а) к экземпляру класса Inner,
   б) к объемлющему экземпляру класса Outer?
   
**Ответ:**
   а) внутри класса Inner к экземпляру класса Inner
   public class Outer {
   Inner  inner  = new Inner ();
   public class Inner {
   void print () {
   System.out.println(inner); // к экземпляру,созданному вне класса
   System.out.println(this); // текущий экземпляр
   }
   }
   }
   б) внутри класса Inner к объемлющему экземпляру класса Outer
   public class Outer {
   public class Inner {
   void print () {
   // Outer.this - ссылка на объемлющий экземпляр
   System.out.println(Outer.this);
   }
   }
   }

8) Пусть объявлен класс Outer, а внутри него вложенный класс Nested. Как обратиться внутри класса Nested:
   а) к экземпляру класса Nested,
   б) к объемлющему экземпляру класса Outer?
   
**Ответ:**
   а) внутри класса Nested к экземпляру класса Nested
   ●       this.[член Nested класса]
   Если нет перекрытия имен, можно и без this. Для статики только напрямую.

б) внутри класса Nested к объемлющему экземпляру класса Outer
●       new Outer().[член Outer класса]
Т.е. только через новый объект.

**Источник:**
https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

9) Можно ли из вложенного класса обратиться к членам внешнего класса?
   Если да, то приведите пример.
   
**Ответ:** Статический вложенный класс для доступа к не статическим членам и методам внешнего класса должен создавать объект внешнего класса и посредством этого объекта обращаться к его членам. А напрямую имеет доступ только к статическим полям и методам внешнего класса.
   public class Outer {
   String outer_x = “nonStaticMember”;
   static String outer_y = “staticMember”;

   static class Nested {
   void display() {
   System.out.println(outer_x);       	// ошибка компиляции
   System.out.println(new Outer().outer_x);
   System.out.println(outer_y);
   }
   }
   }
   
**Источник.** https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

10) Можно ли из внутреннего класса обратиться к экземпляру внешнего класса?
    Если да, то приведите пример.
    
**Ответ:**
    Внутренний класс имеет доступ ко всем переменным и методам своего внешнего класса и может непосредственно ссылаться на них, даже если они объявлены закрытыми.
    class Outer {
    class Inner {
    Outer outer = new Outer();
    }
    }
    
**Источник.** «И.Н. Блинов, В. С. Романчик, Java. Методы программирования, стр. 133»

11) Можно ли определить экземпляр вложенного класса, не определяя экземпляры внешнего класса?
    Если да, то приведите пример.

12) Есть ли ограничения на объявление локальных переменных в локальных внутренних классах?
    Есть ли да, то какие?
    
**Ответ:** Локальный класс наделён особенностями внутренних классов, но имеет отличительные черты, а именно:
1.	он имеет доступ только к финальным полям и аргументам обрамляющего метода, а также ко всем полям обрамляющего класса, в том числе приватным и статическим;
2.	локальный класс виден и может создаваться только в блоке, в котором описан;
3.	у локального класса не ставится модификатор доступа;
4.	не может иметь статических полей, методов, классов (за исключением финальных);
5.	локальный класс, объявленный в статическом блоке может обращаться только к статическим полям внешнего класса.
      Но! Начиная с Java8 мы можем обращаться в локальных классах к не финальным локальным переменным, если они не были изменены до момента инициализации класса. Также теперь стало возможным обращение к не финальным параметрам метода.
      
**Источник:** https://javarush.ru/groups/posts/1553-urovenjh-24-otvetih-na-voprosih-k-sobesedovaniju-po-teme-urovnja

13) Можно ли наследовать вложенные классы?
    Если да, то приведите пример.
    
**Ответ.** Можно, но подкласс вложенного класса не способен унаследовать возможность доступа к членам внешнего класса, которыми наделен его суперкласс.
    public class Outer {
    public static class Nested {
    }
    public class InhNested extends Outer.Nested {
    }
    }
    
**Источник.** И.Н. Блинов, В.С. Романчик  Java. Методы программирования. - Учебно-методическое пособие. - Минск, 2013, c.134.

14) Можно ли из подкласса обратиться к методу вложенного суперкласса?
    Если да, то приведите пример.
    
**Ответ:**
    public class Outer {  	
    static class Nested{         	
    public void sayHelloJava() {
    System.out.println("Hello Java!");
    }
    }  	
    }
    public class SubNested extends Outer.Nested{
    public void sayHelloFromSuper() {
    super.sayHelloJava();
    }  	
    }
    public class Runner {
    public static void main(String[] args) {
    SubNested sn = new SubNested();
    sn.sayHelloFromSuper();
    }
    }
    
**Источник.** https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html

15) Какие существуют варианты внутренних интерфейсов?
    
**Ответ:**
1) Интерфейс, вложенный в другой интерфейс.
2) Интерфейс, вложенный в класс.
   Вложенный в класс интерфейс может быть объявлен как public, private или protected. Это отличает его от интерфейса верхнего уровня или интерфейса вложенного в другой интерфейс, который должен быть либо объявлен как public, либо должен использовать уровень доступа, заданный по умолчанию.
   Когда вложенный в класс интерфейс используется вне содержащей его области определения, он должен определяться именем класса, членом которого он является. То есть вне класса, в котором объявлен вложенный интерфейс, его имя должно быть полностью определено. К интерфейсу, вложенному в другой интерфейс таких требований нет, так как они всегда имеют модификатор public.
   Вложенный в класс интерфейс, который объявлен с модификатором private не может быть имплементирован каким-либо классом. Он может использоваться только внутри того класса где был объявлен.
   
**Источник:**  http://pr0java.blogspot.com/2015/07/5.html

16) Можно ли объявить класс внутри интерфейса?
    Если да, то есть ли ограничения? Приведите пример.
    
**Ответ:** Да можно.
    Причём любой класс или интерфейс, объявленный внутри интерфейса неявно static. На него не накладывается никаких особых ограничений, и он может содержать поля и методы как статические, так и нестатические.
    Такой вложенный класс может быть объявлен с любым уровнем доступа кроме private.
    public interface University {
    int NUMBER_FACULTY = 20;
    class LearningDepartment {            // static по умолчанию
    public int idChief;
    public static void assignPlan(int idFaculty) {
    // реализация
    }
    public void acceptProgram() {
    // реализация
    }
    }
    }
    
**Источник:** Блинов И.Н., Романчик В.С. Java. Методы программирования. 2013. Стр.140


17) Можно ли создать экземпляр анонимного класса на основе:
    а) абстрактного класса?
    б) интерфейса?
    в) неабстрактного класса?
    г) финального класса?
    Если да, то приведите пример.
    
**Ответ:**
    а, б, в - можно.
    abstract class Abstract {
    void method() {}
    }
    interface Interface {
    void method();
    }
    class NonAbstract {
    void method() {}
    }

public class Runner {
public static void main(String[] args) {
Abstract obj1 = new Abstract() {
void method() {
System.out.println("Abstract");
}
};
obj1.method();
Interface obj2 = new Interface() {
public void method() {
System.out.println("Interface");
}
};
obj2.method();
NonAbstract obj3 = new NonAbstract() {
void method() {
System.out.println("Normal");
}
};
obj3.method();
}
}
г) Экземпляр анонимного класса на основе финального класса создать нельзя. Финальный класс подразумевает отсутствие наследования, поэтому анонимный класс, который по своей сути наследует (расширяет) какой-либо класс или имплементирует какой-нибудь интерфейс, не может быть использован для создания экземпляра на основе такого класса.


18) Дан следующий java-файл.
    //-------------- begin --------------
    class Runner {
    public static void main(String[] args){
    Something something = new Something();
    something.doSomething(...);		//1
    }
    }
    interface Smthable {
    void doSmth();
    }
    class Something {
    void doSomething(...) {			//2
    smth.doSmth();
    }
    }
    //--------------- end ---------------
1. Замените многоточия в строках 1 и 2 на такой код, чтобы приложение после запуска с помощью экземпляра анонимного класса, порожденного от интерфейса Smthable, вывело на консоль текст Hello, World.
2. Получите тот же результат, переместив:
   а) интерфейс Smthable внутрь класса Something,
   б) класс Something внутрь интерфейса Smthable.

19) Дан следующий java-файл.
    //-------------- begin --------------
    abstract class AbstractRunner {
    abstract int getYear();
    abstract class AbstarctInner {
    abstract int getYear();
    }
    public static void main(String[] args) {
    ... //1
    ... //2
    ... //3
    }
    }
    //--------------- end ---------------
    Создайте в строке 1 ссылку runner на экземпляр подкласса класса AbstractRunner.
    Создайте в строке 2 ссылку inner на экземпляр подкласса класса AbstractInner.
    Выведите на консоль в строке 3 текст 2010;2015, используя данные ссылки.
    
**Ответ:**
    abstract class AbstractRunner {
    abstract int getYear();

    abstract class AbstractInner {
    abstract int getYear();
    }

    public static void main(String[] args) {
    AbstractRunner runner = new AbstractRunner() {          	//1  
    @Override
    int getYear() {
    return 2010;
    }
    };

         	AbstractInner inner = runner.new AbstractInner() {    	  //2
                	@Override
                	int getYear() {
                      	return 2015;
                	}
         	};

         	System.out.println(runner.getYear()+";"+inner.getYear());   //3
    }
    }

20) Дан следующий java-файл.
    //-------------- begin --------------
    class Runner {
    public static void main(String[] args) {
    ... 	//1
    }
    }
    class Outer {
    class Inner {
    void go() {
    System.out.println("Gone!");
    }
    }
    }
    //--------------- end ---------------
1. С помощью функционала классов Outer и Inner выведите на консоль в строке 1 текст Gone!.
2. Переместив класс Outer внутрь класса Runner, получите тот же результат:
   а) не изменяя строку 1.
   б) изменяя строку 1,

21) Что представляют собой элементы перечисления?
    Подсказка. Откомпилируйте фабричный класс из задачи inheritance1 и посмотрите, какие получились .class-файлы
    
**Ответ:** Элементы перечисления, в зависимости от объявления, могут представлять собой экземпляры типа перечисления, либо подклассы типа перечисления. Второй способ как раз использован в перечислении ниже. При этом подклассы являются анонимными классами, судя по .class файлам. Однако вызывать методы этих классов можно и через имя константы PurchaseKind.GENERAL_PURCHASE.getPurchase(scanner);
    Так что можно сказать, что имена констант выполняют роль имен классов, хотя технически это не так.

22) Как образуются имена вложенных и внутренних .class-файлов после компиляции?
    Приведите примеры.
    
**Ответ.** Имена файлов после компиляции строятся по определенной схеме: имя внешнего класса, затем символ ‘$’ и после – имя внутреннего класса. При компиляции анонимных классов после символа ‘$’ идет порядковый номер анонимного класса в классе, в котором он находится.
    public class Outer {
    static class Nested {
    }
    }	Outer.class
    Outer$Nested.class
    public class Outer {
    class Inner{
    class SubInner {
    }
    }
    }

public class Outer {

        new Clazz() {                  //анонимный класс           
        };
        new Clazz(){                   //анонимный класс           
        };
}	Outer.class
Outer$Inner.class
Outer$Inner$SubInner.class

Outer.class

Outer$1.class

Outer$2.class
public class Outer {
void check() {
class Local {}         // локальный класс
}
}	Outer.class

Outer$1Local.class

23) Может ли вложенный класс быть раннер-классом?
    Если да, то приведите пример, иначе поясните, почему нет.
    
**Ответ:** Да можно.
    public class Boeing737 {

private int manufactureYear;
private static int maxPassengersCount = 300;

public Boeing737(int manufactureYear) {
this.manufactureYear = manufactureYear;
}

public int getManufactureYear() {
return manufactureYear;
}

public static class Drawing {

       private int id;

       public Drawing(int id) {
           this.id = id;
       }

       public static int getPassengersCount() {

           return maxPassengersCount;
       }

       @Override
       public String toString() {
           return "Drawing{" +
                   "id=" + id +
                   '}';
       }

       public static void main(String[] args) {

           for (int i = 1; i < 6; i++) {

               Boeing737.Drawing drawing = new Boeing737.Drawing(i);
               System.out.println(drawing);
           }
       }
}
}

**Источник:** https://javarush.ru/groups/posts/2183-staticheskie-vlozhennihe-klassih

24) Может ли внутренний класс быть раннер-классом?
    Если да, то приведите пример, иначе поясните, почему нет.
    
**Ответ:** Да можно.
    private class Seat {

//методы
}

public class Main {

public static void main(String[] args) {

       Bicycle bicycle = new Bicycle("Peugeot", 120, 40);

       //Bicycle.Seat has a private access in 'Bicycle'
       Bicycle.Seat seat = bicycle.new Seat();
}
}

**Источник:** https://javarush.ru/groups/posts/2181-vlozhennihe-vnutrennie-klassih

25) Может ли интерфейс иметь раннер-класс?
    Если да, то приведите пример, иначе поясните, почему нет.
    
**Ответ:**
    Да, мы можем предоставить различную реализацию main(), объявленной в интерфейсе, и классы, реализующие этот интерфейс, путем переопределения метода и могут перегружать статический основной метод, если он определен в интерфейсе.
    Дополнительная информация об изменениях интерфейса в Java 8.

До Java 8 невозможно было использовать методы DEFINE внутри интерфейса.

Но есть изменения, внесенные в Java 8 по отношению к интерфейсу, где можно установить DEFINE по умолчанию и статический метод внутри интерфейса. Следовательно, ниже код будет работать нормально без каких-либо проблем.

public interface TestInterfaces {
public static void main(String[] a){    
System.out.println("I am a static main method inside Inteface !!");
}
}

**Источник:** https://progi.pro/mozhem-li-mi-imet-main-v-interfeyse-i-raznie-realizacii-dlya-main-v-klassah-realizuyushih-etot-interfeys-1113632
