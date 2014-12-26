==========
 Java小用
==========
:tags: java, oop
:category: java

.. contents::

----------------------------------------

下面就是一些有关Java体验的提示：

1. 在一个文件中给出类的完整层次。

2. 对于每个命名不以上标D、V、I、M结尾的类，\
   添加 `toString` 方法，并遵守以下规则：

   a) 如果一个类没有属性

      .. code-block:: java

         public String toString() {
             return "new " + getClass().getName() + "()";
         }

   b) 如果一个类只有一个属性，比如叫 `x`

      .. code-block:: java

         public String toString() {
             return "new " + getClass().getName() + "(" + x + ")";
         }

   c) 如果一个类有两个属性，比如叫 `x` 和 `y`

      .. code-block:: java

         public String toString() {
             return "new " + getClass().getName() + "(" + x + ", " + y + ")";
         }

3. 在文件的底部添加如下类：

   .. code-block:: java

      class Main {
          public static void main(String args[]) {
              DataType_or_Interface y = new ______;
              System.out.println( ... ... );
          }
      }

`DataType_or_Interface y = new ______;` 是用来创建你想尝试的对象。

`System.out.println( ... ... );` 是用来填写你想尝试的表达式。

比如，你想尝试第2章定义的 `ManhattanPt` 的 `distanceTo0` 方法，\
你就可以添加如下代码到你文件的最后：

.. code-block:: java

   public class Main{
       public static void main(String args[]) {
           PointD y = new ManhattanPt(2, 8);
           System.out.println(y.distanceTo0());
       }
   }

如果你想尝试多条表达式，就修改 `y` ，就像第10章里，
::

   y._ _ _ _ _ _;
   y._ _ _ _ _ _;
   y._ _ _ _ _ _

替换成
::

   y._ _ _ _ _ _ + "\n" +
   y._ _ _ _ _ _ + "\n" +
   y._ _ _ _ _ _

如果你想尝试第10章中定义的 `PiemanM` 的多个方法，\
那么你就将以下代码写在文件的最后面：

.. code-block:: java

   class Main {
       public static void main(String args[]) {
           PiemanI y = new PiemanM();
           System.out.println(
               y.addTop(new Anchovy()) + "\n" +
               y.addTop(new Anchovy()) + "\n" +
               y.substTop(new Tuna(), new Anchovy())
           );
       }
   }

4. 最后，编译文件并且执行 `Main` 类。

   .. tip::

      按照上面的要求，保存的代码的文件名同时也要为 `Main.java` 。\
      (因为Java会根据文件名来查找其文件内同名的类，再执行该类的main方法)

      然后使用 `javac Main.java` 来进行编译生成 `Main.class` 中间码文件。

      最后， `java Main` 来执行程序。

      建议大家上网搜索一下 `Java helloworld` 看一下相关教程就可以了。\
      暂时不用深入学习 `Java` 。
