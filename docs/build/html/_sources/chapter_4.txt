======================
 Come to Our Carousel
======================
:tags: java, oop
:category: java

.. contents::

----------------------------------------

这一章节就针对上一章节暴露的问题，提出解决方法：

  **访问者模式**

不过作者没有一上来就给出完美的实现，它是通过一步步来引导的。

下面的两个例子只是第一步：
::

   先将类似的方法聚合到访问者类中

ShishD
------
.. code-block:: java

   // 访问者
   class OnlyOnionsV {
       boolean forSkewer() {
           return true;
       }
       boolean forOnion(ShishD s) {
           return s.onlyOnions();
       }
       boolean forLamb(ShishD s) {
           return false;
       }
       boolean forTomato(ShishD s) {
           return false;
       }
   }

   class IsVegetarianV {
       boolean forSkewer() {
           return ture;
       }
       boolean forOnion(ShishD s) {
           return s.IsVegetarian();
       }
       boolean forLamb(ShishD s) {
           return false;
       }
       boolean forTomato(ShishD s) {
           return s.IsVegetarian();
       }
   }

.. code-block:: java

   // 使用访问者模式的ShishD
   // 这里可以对比一下第二章节的ShishD代码
   abstract class ShishD { //羊肉
       OnlyOnionsV ooFn = new OnlyOnionsV();
       IsVegetarianV ivFn = new IsVegetarianV();
       abstract boolean onlyOnions();
       abstract boolean IsVegetarian();
   }

   class Skewer extends ShishD { //串
       boolean onlyOnions() {
           return ooFn.forSkewer();
       }
       boolean IsVegetarian() {
           return ivFn.forSkewer();
       }
   }

   class Onion extends ShishD { //洋葱
       ShishD s;
       Onion (ShishD _s) {
           s = _s;
       }
       boolean onlyOnions() {
           return ooFn.forOnion(s);
       }
       boolean IsVegetarian() {
           return ivFn.forOnion(s);
       }
   }

   class Lamb extends ShishD { //羔羊肉
       ShishD s;
       Lamb (ShishD _s) {
           s = _s;
       }
       boolean onlyLambs() {
           return ooFn.forLamb(s);
       }
       boolean IsVegetarian() {
           return ivFn.forLamb(s);
       }
   }

   class Tomato extends ShishD { //西红柿
       ShishD s;
       Tomato (ShishD _s) {
           s = _s;
       }
       boolean onlyTomatos() {
           return ooFn.forTomato(s);
       }
       boolean IsVegetarian() {
           return ivFn.forTomato(s);
       }
   }

**第四条建议**

  When writing several functions for the

  same self-referential datatype, use

  visitor protocols so that all methods for

  a function can be found in a single class.

PizzaD
------
.. code-block:: java

   abstract class PizzaD { //披萨饼
       RemAV remFn = new RemAV();
       TopAwCV topFn = new TopAwCV();
       SubAbCV subFn = new SubAbCV();
       abstract PizzaD remA();
       abstract PizzaD topAwC();
       abstract PizzaD subAbC();
   }

   class Crust extends PizzaD { //面包皮
       PizzaD remA() {
           return remFn.forCrust();
       }
       PizzaD topAwC() {
           return topFn.forCrust();
       }
       PizzaD subAbC() {
           return subFn.forCrust();
       }
   }

   class Cheese extends PizzaD { //奶酪
       PizzaD p;
       Cheese(PizzaD _p) {
           p = _p;
       }
       PizzaD remA() {
           return remFn.forCheese(p);
       }
       PizzaD topAwC() {
           return topFn.forCheese(p);
       }
       PizzaD subAbC() {
           return subFn.forCheese(p);
       }

   }

   Classr Olive extends PizzaD { //橄榄
       PizzaD p;
       Olive(PizzaD _p) {
           p = _p;
       }
       PizzaD remA() {
           return remFn.forOlive(p);
       }
       PizzaD topAwC() {
           return topFn.forOlive(p);
       }
       PizzaD subAbC() {
           return subFn.forOlive(p);
       }
   }

   class Anchovy extends PizzaD { //凤尾鱼
       PizzaD p;
       Anchovy(PizzaD _p) {
           p = _p;
       }
       PizzaD remA() {
           return remFn.forAnchovy(p);
       }
       PizzaD topAwC() {
           return topFn.forAnchovy(p);
       }
       PizzaD subAbC() {
           return subFn.forAnchovy(p);
       }
   }

   class Sausage extends PizzaD { //香肠
       PizzaD p;
       Sausage(PizzaD _p) {
           p = _p;
       }
       PizzaD remA() {
           return remFn.forSausage(p);
       }
       PizzaD topAwC() {
           return topFn.forSausage(p);
       }
       PizzaD subAbC() {
           return subFn.forSausage(p);
       }
   }

.. code-block:: java

   class RemAV {
       PizzaD forCrust() {
           return new Crust();
       }
       PizzaD forCheese(PizzaD p) {
           return new Cheese(p.remA());
       }
       PizzaD forOlive(PizzaD p) {
           return new Olive(p.remA());
       }
       PizzaD forAnchovy(PizzaD p) {
           return p.remA();
       }
       PizzaD forSausage(PizzaD p) {
           return new Sausage(p.remA());
       }
   }

   class TopAwCV {
       PizzaD forCrust() {
           return new Crust();
       }
       PizzaD forCheese(PizzaD p) {
           return new Cheese(p.topAwC());
       }
       PizzaD forOlive(PizzaD p) {
           return new Olive(p.topAwC());
       }
       PizzaD forAnchovy(PizzaD p) {
           return new Cheese(new Anchovy(p.topAwC()));
       }
       PizzaD forSausage(PizzaD p) {
           return new Sausage(p.topAwC());
       }
   }

   class SubAbCV {
       PizzaD forCrust() {
           return new Crust();
       }
       PizzaD forCheese(PizzaD p) {
           return new Cheese(p.subAbC());
       }
       PizzaD forOlive(PizzaD p) {
           return new Olive(p.subAbC());
       }
       PizzaD forAnchovy(PizzaD p) {
           return new Cheese(p.subAbC());
       }
       PizzaD forSausage(PizzaD p) {
           return new Sausage(p.subAbC());
       }
   }
