=================
Be a Good Visitor
=================
:tags: java, oop
:category: java

.. contents::

----------------------------------------

先引入 `super` 关键字。

.. code-block:: java

   class ShadowedCartesianPt extend CartesianPt {
       int tx;
       int ty;
       ShadowedCartesianPt(int _x, int _y, int _tx, int _ty) {
           super(_x, _y);
           tx = _tx;
           ty = _ty;
       }
       int distanceTo0() {
           return super.distanceTo0()
                  +
                  (int)Math.sqrt(tx * tx + ty * ty);
       }
   }


.. code-block:: java

   interface ShapeVisitorI {
       boolean forCircle(int r);
       boolean forSquare(int s);
       boolean forTrans(PointD q, ShapeD s);
   }

   abstract class ShapeD {
       abstract boolean accept(ShapeVisitorI ask);
   }

   class Circle extend ShapeD { // 圆心在坐标原点的圆
       int r;
       Circle(int _r) {
           r = _r;
       }
       boolean accept(ShapeVisitorI ask) {
           return ask.forCircle(r);
       }
   }

   class Square extend ShapeD { //左上角在坐标原点的正方形
       int r;
       Square(int _r) {
           r = _r;
       }
       boolean accept(ShapeVisitorI ask) {
           return ask.forSquare(r);
       }
   }
   
   class Trans extend ShapeD { //在指定位置的图形，
       PointD q;
       ShapeD s;
       Trans(PointD _q, ShapeD _s) {
           q = _q;
           s = _s;
       }
       boolean accept(ShapeVisitorI ask) {
           return ask.forTrans(q, s);
       }
   }

.. code-block:: java

   // 检查某个点是否在图形内部
   class HasPtV implements ShapeVisitorI {
       PointD p;
       HasPtV(PointD _p) {
           p = _p;
       }
       public boolean forCircle(int r) {
           return p.distanceTo0() <= r;
       }
       public boolean forSquare(int s) {
           return p.x <= s && p.y <= s;
       }
       public boolean forTrans(PointD q, ShapeD s) {
           return s.accept(new HasPtV(p.minus(q)));
       }
   }

书中说下面精彩的地方到了。

主要就是揭示了 `interface` 也能够被继承扩展，

和访问者模式的精华所在：兼有灵活性及扩展性。

.. code-block:: java

   class Union extends ShapeD {
       ShapeD s;
       ShapeD t;
       Union(ShapeD _s, ShapeD _t) {
           s = _s;
           t = _t;
       }
       boolean accept(ShapeVisitorI ask) {
           ((UnionVisitorI)ask).forUnion(s, t);
       }
   }

   interface UnionVisitorI extends ShapeVisitorI {
       boolean forUnion(ShapeD s, ShapeD t);
   }

   class UnionHasPtV extends HasPtV implements ShapeVisitorI {
       UnionHasPtV(PointD _p) {
           super(_p);
       }
       public boolean forUnion(ShapeD s, ShapeD t) {
           return s.accept(this) || t.accept(this);
       }
   }

.. code-block:: java

   class HasPtV implements ShapeVisitorI {
       PointD p;
       HasPtV(PointD _p) {
           p = _p;
       }
       ShapeVisitorI newHasPt(PointD p) {
           return new HasPtV(p);
       }
       public boolean forCircle(int r) {
           return p.distanceTo0() <= r;
       }
       public boolean forSquare(int s) {
           return p.x <= s && p.y <= s;
       }
       public boolean forTrans(PointD q, ShapeD s) {
           return s.accept(newHasPtV(p.minus(q)));
       }
   }

.. tip::
  
   **第九条建议**

   If a datatype may have to be extended,

   be forward looking and use a

   constructor-like(override) method

   so that visitors can be extended too.

正是由于上面的提取出了 `newHasPt` 方法，下面直接重载即可。

.. code-block:: python

   class UnionHasPtV extends HasPtV implements UnionVisitorI {
       UnionHasPtV(PointD _p) {
           super(_p);
       }
       ShapeVisitorI newHasPt(PointD p) {
           return new UnionHasPtV(p);
       }
       public boolean forUnion(ShapeD s, ShapeD t) {
           return s.accept(this) || t.accept(this);
       }
   }
