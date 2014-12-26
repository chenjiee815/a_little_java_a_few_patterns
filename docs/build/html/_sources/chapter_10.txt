=============================
 The State of Things to Come
=============================
:tags: java, oop
:category: java

.. contents::

----------------------------------------

之前的章节每次都是生成新的实例，然后再对该实例进行操作。

这一章节主要讲到如何修改实例中的属性，然后再进行操作。

.. code-block:: java

   interface PiemanI {
       int addTop(Object t);
       int remTop(Object t);
       int substTop(Object n, Object o);
       int occTop(Object o);
   }

   class PiemanM implements PiemanI {
       PieD p = new Bot();
       public int addTop(Object t) {
           p = new Top(t, p);
           return occTop(t);
       }
       public int remTop(Object t) {
           p = (PieD)p.accept(new RemV(t));
           return occTop(t);
       }
       public int substTop(Object n, Object o) {
           p = (PieD)p.accept(new SubstV(n o));
           return occTop(n);
       }
       public int occTop(Object o) {
           return ((Integer)p.accept(new OccursV(o))).intValue();
       }
   }

   interface PieVisitorI {
       Object forBot();
       Object forTop(Object t, PieD r);
   }

   abstract class PieD {
       abstract Object accept(PieVisitorI ask);
   }

   class Bot extends PieD {
       Object accept(PieVisitorI ask) {
           return ask.forBot();
       }
   }

   class Top extends PieD {
       Object t;
       PieD r;
       Top(Object _t, Object _r) {
           t = _t;
           r = _r;
       }
       Object accept(PieVisitorI ask) {
           return ask.forTop(t, r);
       }
   }

   class OccursV implements PieVisitorI {
       Object a;
       OccursV(Object _a) {
           a = _a;
       }
       public Object forBot() {
           return new Integer(0);
       }
       public Object forTop(Object t, PieD r) {
           if (t.equals(a))
               return new Integer(((Integer)(r.accept(this))).intValue()
                                  +
                                  1);
           else
               return r.accept(this);
       }
   }

   class SubstV implements PieVisitorI {
       Object n;
       Object o;
       SubstV(Object _n, Object _o) {
           n = _n;
           o = _o;
       }
       public Object forBot() {
           return new Bot();
       }
       public Object forTop(Object t, PieD r) {
           if (o.equals(t))
               return new Top(n, (PieD)r.accept(this));
           else
               return new Top(t, (PieD)r.accept(this));
       }
   }

   class RemV implements PieVisitorI {
       Object o;
       RemV(Object _o) {
           o = _o;
       }
       public Object forBot() {
           return new Bot();
       }
       public Object forTop(Object t, PieD r) {
           if (o.equals(t))
               return r.accept(this);
           else
               return new Top(t, (PieD)r.accept(this));
       }
   }

修改 `PieVisitorI` 中的方法签名，传入本身。

这样两者就完全相通，原对象可以请求访问者中对应的方法，

访问者可以在其内部的方法中修改原对象。

.. tip::

   这一点也可以从 `this`  `that` 来理解。

.. code-block:: java

   interface PieVisitorI {
       Object forBot(Bot that);
       Object forTop(Top that);
   }

同步修改其它相关代码。

.. code-block:: java

   abstract class PieD {
       abstract Object accept(PieVisitorI ask);
   }

   class Bot extends PieD {
       Object accept(PieVisitorI ask) {
           return ask.forBot(this);
       }
   }

   class Top extends PieD {
       Object t;
       PieD r;
       Top(Object _t, Object _r) {
           t = _t;
           r = _r;
       }
       Object accept(PieVisitorI ask) {
           return ask.forTop(this);
       }
   }
   
   class OccursV implements PieVisitorI {
       Object a;
       OccursV(Object _a) {
           a = _a;
       }
       public Object forBot(Bot that) {
           return new Integer(0);
       }
       public Object forTop(Top that) {
           if (that.t.equals(a))
               return new Integer(((Integer)(that.r.accept(this))).intValue()
                                  +
                                  1);
           else
               return that.r.accept(this);
       }
   }

   class SubstV implements PieVisitorI {
       Object n;
       Object o;
       SubstV(Object _n, Object _o) {
           n = _n;
           o = _o;
       }
       public Object forBot(Bot that) {
           return new Bot();
       }
       public Object forTop(Top that) {
           if (o.equals(that.t))
               return new Top(n, (PieD)(that.r).accept(this));
           else
               return new Top(that.t, (PieD)(that.r).accept(this));
       }
   }

   class RemV implements PieVisitorI {
       Object o;
       RemV(Object _o) {
           o = _o;
       }
       public Object forBot(Bot that) {
           return new Bot();
       }
       public Object forTop(Top that) {
           if (o.equals(that.t))
               return that.r.accept(this);
           else
               return new Top(that.t, (PieD)(that.r).accept(this));
       }
   }

上面的代码还是有新的对象实例生成，

接下来，咱们通过彻底地修改实例的属性，而不是重新生成新的实例，来实现一个访问者。

.. code-block:: java

   class SubstV implements PieVisitorI {
       Object n;
       Object o;
       SubstV(Object _n, Object _o) {
           n = _n;
           o = _o;
       }
       public Object forBot(Bot that) {
           return that;
       }
       public Object forTop(Top that) {
           if (o.equals(that.t))
               that.t = n;
               that.r.accept(this);
               return that;
           else
               that.r.accept(this);
               return that;
       }
   }

.. tip::

   **第十条建议**

   When modifcations to objects are

   needed, use a class to insulate the

   operations that modify objects.

   Otherwise, beware the consequences of

   your actions.

咱们再一个例子，加深一下理解。

.. code-block:: java

   abstract class PointD{
       int x;
       int y;
       PointD(int _x, int _y){
           x = _x;
           y = _y;
       }
       boolean closerTo0(PointD p) {
           return distanceTo0() <= p.distanceTo0();
       }
       PointD minus(PointD p) {
           return CartesianPt(x - p.x, y - p.y);
       }
       int moveBy(int tx, int ty) {
           x = x + tx;
           y = y + ty;
           return distanceTo0();
       }
       abstract int distanceTo0();
   }

   class CartesianPt extends PointD{ //笛卡尔坐标
       CartesianPt(int _x, int _y){
           super(_x, _y);
       }
       int distanceTo0(){
           return (int)Math.sqrt(x * x + y * y);
       }
   }

   class ManhattanPt extends PointD{ //曼哈顿坐标
       ManhattanPt(int _x, int _y){
           super(_x, _y);
       }
       int distanceTo0(){
           return x + y;
       }
   }
   
   class ShadowedManhattanPt extends ManhattanPt{ //曼哈顿坐标
       int tx;
       int ty;
       ManhattanPt(int _x, int _y, int _tx, int _ty){
           super(_x, _y);
           tx = _tx;
           ty = _ty;
       }
       int distanceTo0(){
           return super.distanceTo0() + tx + ty; 
       }
   }

