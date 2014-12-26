=========================
 Objects Are People, Too
=========================
:tags: java, oop
:category: java

.. contents::

----------------------------------------

PieD
----
.. code-block:: java

   abstract class PieD { //馅饼，派
       RemAV raFn = new RemAV();
       RemFishV rfFn = new RemFishV();
       abstract PieD remA();
       abstract PieD remFish(FishD f);
   }

   class Bot extends PieD { //底料
       PieD remA() {
           return raFn.forBot();
       }
       Pied remFish(FishD f) {
           return rfFn.forBot(f);
       }
   }

   class Top extends PieD { //顶料
       Object t;
       PieD r;
       Top(Object _t, PieD _r) {
           t = _t,
           r = _r,
       }
       PieD remA() {
           return raFn.forTop(t, r);
       }
       PieD remFish(FishD f) {
           return rfFn.forTop(t, r, f);
       }
   }

FishD
-----
.. code-block:: java

   abstract class FishD {}

   class Anchovy extends FishD { //凤尾鱼
       public boolean equals(Object o) {
           return (o instanceof Anchovy);
       }
   }

   class Salmon extends FishD { //鲑鱼
       public boolean equals(Object o) {
           return (o instanceof Salmon);
       }
   }

   class Tuna extends FishD { //金枪鱼
       public boolean equals(Object o) {
           return (o instanceof Tuna);
       }
   }

.. code-block:: java

   // 删除凤尾鱼的方法
   class RemAV {
       PieD forBot() {
           return new Bot();
       }
       PieD forTop(Object t, PieD r) {
           if (new Anchovy().equals(t))
               return r.remA();
           else
               return new Top(t, r.remA());
       }
   }

   // 删除指定鱼的方法，相当在 `RemAV` 的概念上再抽象一层
   class RemFishV {
       PieD forBot(FishD f) {
           return new Bot();
       }
       PieD forTop(Object t, PieD r, FishD f) {
           if (f.equals(t))
               return r.remFish(f);
           else
               return new Top(t, r.remFish(f));
       }
   }

   // 删除指定整数的方法
   class RemIntV {
       PieD forBot(Integer i) {
           return new Bot();
       }
       PieD forTop(Object t, PieD r, Integer i) {
           if (i.equals(t))
               return r.remInt(i);
           else
               return new Top(t, r.remInt(i));
       }
   }

`RemFishV` 和  `RemIntV` 的整个逻辑很类似么，那么将它们重新抽象一下？

.. code-block:: java

   class RemV {
       PieD forBot(Object o) {
           return new Bot();
       }
       PieD forTop(Object t, PieD r, Object o) {
           if (o.equals(t))
               return r.rem(o);
           else
               return new Top(t, r.rem(o));
       }
   }

`PieD` 及它的变种类型 `Bot`  `Top` 也要简单变化一下。

.. code-block:: java

   abstract class PieD {
       RemV remFn = new RemV();
       abstract PieD rem(Object o);
   }

   class Bot extends PieD {
       PieD rem(Object o){
           return remFn.forBot(o);
       }
   }

   class Top extends PieD {
       Object t;
       PieD r;
       Top(Object _t, PieD _r){
           t = _t;
           r = _r;
       }
       PieD rem(Object o){
           return remFn.forTop(t, r, o);
       }
    }

现在的 `Bot`  `Top` 在调用rem时，\
即可以传入 `FishD` 也可以传入 `Integer` 了。

NumD
----
.. code-block:: java

   abstract class NumD {}

   class OneMoreThan extends NumD {
       NumD predecessor;
       OneMoreThan(NumD _p) {
           predecessor = _p;
       }
       public boolean equals(Object o) {
           if (o instanceof OneMoreThan)
               return predecessor.equals(
                   ((OneMoreThan)o).predecessor
               ),
           else
               return false;
       }
   }

   class Zero extends NumD {
       public boolean equals(Object o) {
           return (o instanceof Zero);
       }
   }

.. code-block:: java

   class SubstFishV {
       PieD forBot(FishD n, FishD o) {
           return new Bot();
       }
       PieD forTop (Object t, PieD r, FishD n, FishD o) {
           if (o.equals(t))
               return new Top(n, r.substFish(n, o));
           else
               return new Top(t, r.substFish(n, 0));
       }
   }

   class SubstIntV {
       PieD forBot(Integer n, Integer o) {
           return new Bot();
       }
       PieD forTop (Object t, PieD r, Integer n, Integer o) {
           if (o.equals(t))
               return new Top(n, r.substInt(n, o));
           else
               return new Top(t, r.substInt(n, 0));
       }
   }

   class SubstV {
       PieD forBot(Object n, Object o) {
           return new Bot();
       }
       PieD forTop (Object t, PieD r, Object n, Object o) {
           if (o.equals(t))
               return new Top(n, r.subst(n, o));
           else
               return new Top(t, r.subst(n, 0));
       }
   }

`SubstV` 和  `RemV` 的做法是一样。

再整理一下 `PieD` 

.. code-block:: java

   abstract class PieD {
       RemV remFn = new RemV();
       SubsbV substFn = new SubstV();
       abstract PieD rem(Object o);
       abstract PieD subst(Object n, Object o);
   }

   class Bot extends PieD {
       PieD rem(Object o){
           return remFn.forBot(o);
       }
       PieD subst(Object n, Object o){
           return substFn.forBot(n, o)
       }
   }

   class Top extends PieD {
       Object t;
       PieD r;
       Top(Object _t, PieD _r){
           t = _t;
           r = _r;
       }
       PieD rem(Object o){
           return remFn.forTop(t, r, o);
       }
       PieD subst(Object n, Object o){
           return substFn.forTop(n, o)
       }
   }
