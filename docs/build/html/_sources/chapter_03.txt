=============
 What's New?
=============
:tags: java, oop
:category: java

.. contents::

----------------------------------------

PizzaD
------
.. code-block:: java

   abstract class PizzaD { //比萨饼
       abstract PizzaD remA(); //去除比萨饼中的凤尾鱼顶料(因为太咸了)
       abstract PizzaD topAwC(); //在凤尾鱼顶料上加上奶酪顶料(这样会盖住凤尾鱼的咸味)
       abstract PizzaD subAbC(); //将所有的凤尾鱼顶料换成奶酪顶料
   }

   class Crust extends PizzaD { //面包皮
       PizzaD subAbC(){
           return new Crust();
       }
       PizzaD topAwC(){
           return new Crust();
       }
       PizzaD subAbC(){
           return new Crust();
       }
   }

   // 下面是各种顶料
   class Cheese extends PizzaD { //奶酪
       PizzaD p;
       Cheese (PizzaD _p) {
           p = _p;
       }
       PizzaD remA(){
           return new Cheese(p.remA());
       }
       PizzaD topAwC(){
           return new Cheese(p.topAwC());
       }
       PizzaD subAbC(){
           return new Cheese(p.subAbC());
       }
   }

   class Olive extends PizzaD { //橄榄
       PizzaD p;
       Olive (PizzaD _p) {
           p = _p;
       }
       PizzaD remA(){
           return new Olive(p.remA());
       }
       PizzaD topAwC(){
           return new Olive(p.topAwC());
       }
       PizzaD subAbC(){
           return new Olive(p.subAbC());
       }
   }

   class Anchovy extends PizzaD { //凤尾鱼
       PizzaD p;
       Anchovy (PizzaD _p) {
           p = _p;
       }
       PizzaD remA(){
           return p.remA();
       }
       PizzaD topAwC(){
           return new Cheese(new Anchovy(p.topAwC()));
       }
       PizzaD subAbC(){
           return new Cheese(p.subAbC());
       }
   }

   class Sausage extends PizzaD { //香肠
       PizzaD p;
       Sausage (PizzaD _p) {
           p = _p;
       }
       PizzaD remA(){
           return new Sausage(p.remA());
       }
       PizzaD topAwC(){
           return new Sausage(p.topAwC());
       }
       PizzaD subAbC(){
           return new Sausage(p.subAbC());
       }
   }

如果想要在比萨饼上面添加额外的顶料怎么办？

很简单，再从 `PizzaD` 扩展出一个新的子类型就可以了。

.. code-block:: java

   class Spinach extends PizzaD { //菠菜
       PizzaD p;
       Spinach (PizzaD _p) {
           p = _p;
       }
       PizzaD remA(){
           return new Spinach(p.remA());
       }
       PizzaD topAwC(){
           return new Spinach(p.topAwC());
       }
       PizzaD subAbC(){
           return new Spinach(p.subAbC());
       }
   }

但是每添加一个新的变体类型都要加上三个方法，好累的说。

有什么比较好的办法解决这个问题呢？

  下一章节给你答案。

**第三条建议**

  When writing a function that returns values of a datatype,

  use *new* to create these values.
