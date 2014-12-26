========
 Oh My!
========
:tags: java, oop
:category: java

.. contents::

----------------------------------------

.. code-block:: java

   abstract class FruitD {} // 水果

   class Peach extends FruitD {  //桃
       public boolean equals(Object o) {
           return (o instanceof Peach);
       }
   }

   class Apple extends FruitD { //苹果
       public boolean equals(Object o) {
           return (o instanceof Apple);
       }
   }

   class Pear extends FruitD { //梨
       public boolean equals(Object o) {
           return (o instanceof Pear);
       }
   }

   class Lemon extends FruitD { //柠檬
       public boolean equals(Object o) {
           return (o instanceof Lemon);
       }
   }

   class Fig extends FruitD { //无花果
       public boolean equals(Object o) {
           return (o instanceof Fig);
       }
   }

.. code-block:: java

   abstract class TreeD { //树
       abstract boolean accept(bTreeVisitorI ask);
       abstract int accept(iTreeVisitorI ask);
       abstract TreeD accept(tTreeVisitorI ask);
   }

   class Bud extends TreeD { //芽
       boolean accept(bTreeVisitorI ask) {
           return ask.forBud();
       }
       int accept(iTreeVisitorI ask) {
           return ask.forBud();
       }
       TreeD accept(tTreeVisitorI ask) {
           return ask.forBud();
       }
   }

   class Flat extends TreeD { //平顶
       FruitD f;
       TreeD t;
       Flat(FruitD _f, TreeD _t) {
           f = _f;
           t = _t;
       }
       boolean accept(bTreeVisitorI ask) {
           return ask.forFlat(f, t);
       }
       int accept(iTreeVisitorI ask) {
           return ask.forFlat(f, t);
       }
       TreeD accept(tTreeVisitorI ask) {
           return ask.forFlat(f, t);
       }
   }

   class Split extends TreeD { //分枝
       TreeD l;
       TreeD r;
       Split(Treed _l, TreeD _r) {
           l = _l;
           r = _r;
       }
       boolean accept(bTreeVisitorI ask) {
           return ask.forSplit(l, r);
       }
       int accept(iTreeVisitorI ask) {
           return ask.forSplit(l, r);
       }
       TreeD accept(tTreeVisitorI ask) {
           return ask.forFlat(l, r);
       }
   }
   
.. code-block:: java

   interface bTreeVisitorI {
       boolean forBud();
       boolean forFlat(FruitD f, TreeD t);
       boolean forSplit(TreeD l, TreeD r);
   }

   class bIsFlatV implements bTreeVisitorI {
       public boolean forBud() {
           return true;
       }
       public boolean forFlat(FruitD f, TreeD t) {
           return t.accept(this);
       }
       public boolean forSplit(TreeD l, TreeD r) {
           return false;
       }
   }

   class bIsSplitV implements bTreeVisitorI {
       public boolean forBud() {
           return true;
       }
       public boolean forFlat(FruitD f, TreeD t) {
           return false;
       }
       public boolean forSplit(TreeD l, TreeD r) {
           return l.accept(this) && r.accept(this);
       }
   }

   class bHasFruitV implements bTreeVisitorI {
       public boolean forBud() {
           return false;
       }
       public boolean forFlat(FruitD f, TreeD t) {
           return true;
       }
       public boolean forSplit(TreeD l, TreeD r) {
           return l.accept(this) || r.accept(this);
       }
   }

.. code-block:: java

   interface iTreeVisitorI {
       int forBud();
       int forFlat(FruitD f, TreeD t);
       int forSplit(TreeD l, TreeD r);
   }

   class iHeightV implements iTreeVisitorI {
       public int forBud() {
           return 0;
       }
       public int forFlat(FruitD f, TreeD t) {
           return t.accept(this) + 1;
       }
       public int forSplit(TreeD l, TreeD r) {
           return (l.accept(this) |_| r.accept(this)) + 1;
       }
   }

   class iOccursV implements iTreeVisitorI {
       FruitD a;
       iOccursV(FruitD _a) {
           a = _a;
       }
       public int forBud() {
           return 0;
       }
       public int forFlat(FruitD f, TreeD t) {
           if (f.equals(a))
               return t.accept(this) + 1;
           else
               return t.accept(this);
       }
       public int forSplit(TreeD l, TreeD r) {
           return l.accept(this) + r.accept(this);
       }
   }

.. code-block:: java

   interface tTreeVisitorI {
       TreeD forBud();
       TreeD forFlat(FruitD f, TreeD t);
       TreeD forSplit(TreeD l, TreeD r);
   }

   class tSubstV implements tTreeVisitorI {
       FruitD n;
       FruitD o;
       tSubstV(FruitD _n, FruitD _o) {
           n = _n;
           o = _o;
       }
       public TreeD forBud() {
           return new Bud();
       }
       public TreeD forFlat(FruitD f, TreeD t) {
           if (o.equals(f))
               return new Flat(n, t.accept(this));
           else
               return new Flat(f, t.accept(this));
       }
       public TreeD forSplit(TreeD l, TreeD r) {
           return new Split(l.accept(this), r.accept(this));
       }
   }

上面的三个接口是不是有点繁琐？那么将它合并起来。

.. code-block:: java

   interface TreeVisitorI {
       Object forBud();
       Object forFlat(FruitD f, TreeD t);
       Object forSplit(TreeD l, TreeD r);
   }

   abstract class TreeD {
       abstract Object accept(TreeVisitorI ask);
   }

   class Bud extends TreeD {
       Object accept(TreeVisitorI ask) {
           return ask.forBud();
       }
   }

   class Flat extends TreeD {
       FruitD f;
       TreeD t;
       Flat(FruitD _f, TreeD _t) {
           f = _f;
           t = _t;
       }
       Object accept(TreeVisitorI ask) {
           return ask.forFlat(f, t);
       }
   }

   class Split extends TreeD {
       TreeD l;
       TreeD r;
       Split(Treed _l, TreeD _r) {
           l = _l;
           r = _r;
       }
       Object accept(TreeVisitorI ask) {
           return ask.forSplit(l, r);
       }
   }

   class IsFlatV implements TreeVisitorI {
       public Object forBud() {
           return new Boolean(true);
       }
       public Object forFlat(FruitD f, TreeD t) {
           return t.accept(this);
       }
       public Object forSplit(TreeD l, TreeD r) {
           return new Boolean(false);
       }
   }

   class bIsSplitV implements bTreeVisitorI {
       public boolean forBud() {
           return new Boolean(true);
       }
       public boolean forFlat(FruitD f, TreeD t) {
           return new Boolean(false);
       }
       public boolean forSplit(TreeD l, TreeD r) {
           if (((Boolean)(l.accept(this))).booleanValue())
               return r.accept(this);
           else:
               return new Boolean(false);
       }
   }

.. tip::
   
   **第七条建议**

   When designing visitor protocols for

   many different types, create a unifying

   protocol using `Object` .

但是这有一个不好的地方，如果返回的是Java的内置类型，

那内部在进行处理时，就要先进行转换，有时候甚至要令人发指的程度。

比如下面的代码：

.. code-block:: java

   class OccursV implements TreeVisitorI {
       FruitD a;
       iOccursV(FruitD _a) {
           a = _a;
       }
       public Object forBud() {
           return new Integer(0);
       }
       public Object forFlat(FruitD f, TreeD t) {
           if (f.equals(a))
               return new Integer(((Integer)(t.accept(this))).intValue() + 1);
           else
               return t.accept(this);
       }
       public int forSplit(TreeD l, TreeD r) {
           return new Integer(((Integer)(l.accept(this))).intValue()
                              +
                              ((Integer)(r.accept(this))).intValue());
       }
   }

.. tip::

   难道只能这样？下面的章节给你答案。
