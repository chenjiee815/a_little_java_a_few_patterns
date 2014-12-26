=======================
 Like Father, Like Son
=======================
:tags: java, oop
:category: java

.. contents::

这一章节主要讲到了继承，从章节题目也可以看出来: 父子。

----------------------------------------


上一章节的最后表明了一个问题：
::

   由于自定义类型不支持运算，只能Java的内置类型有运算功能，

   所以在进行运算时，需要先转换成内置类型然后运算完后，

   再转换成特定的类型。

为了解决这个问题，就需要定义自定义类型的相关操作方法了。 

连运算操作也进行面向对象化。

.. code-block:: java

   interface ExprVisitorI {
       Object forPlus(ExprD l, ExprD r); // 相加
       Object forDiff(ExprD l, ExprD r); // 相减
       Object forProd(ExprD l, ExprD r); // 相乘
       Object forConst(Object c); // 常量
   }

   abstract class ExprD {
       abstract Object accept(ExprVisitorI ask);
   }

   class Plus extends ExprD {
       ExprD l;
       ExprD r;
       Plus(ExprD _l, ExprD _r) {
           l = _l;
           r = _r;
       }
       Object accept(ExprVisitorI ask){
           return ask.forPlus(l, r);
       }
   }

   class Diff extends ExprD {
       ExprD l;
       ExprD r;
       Diff(ExprD _l, ExprD _r) {
           l = _l;
           r = _r;
       }
       Object accept(ExprVisitorI ask){
           return ask.forDiff(l, r);
       }
   }

   class Prod extends ExprD {
       ExprD l;
       ExprD r;
       Prod(ExprD _l, ExprD _r) {
           l = _l;
           r = _r;
       }
       Object accept(ExprVisitorI ask){
           return ask.forProd(l, r);
       }
   }

   class Const extends ExprD {
       Object c;
       Const(Object _c){
           c = _c;
       }
       Object accept(ExprVisitorI ask){
           return ask.forConst(c);
       }
   }

.. code-block:: java

   class IntEvalV implements ExprVisitorI {
       public Object forPlus(ExprD l, ExprD r){
           return plus(l.accept(this), r.accept(this));
       }
       public Object forDiff(ExprD l, ExprD r){
           return diff(l.accept(this), r.accept(this));
       }
       public Object forProd(ExprD l, ExprD r){
           return prod(l.accept(this), r.accept(this));
       }
       public Object forConst(Object c){
           return c;
       }
       Object plus(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              +
                              ((Integer)r).intValue());
       }
       Object diff(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              -
                              ((Integer)r).intValue());
       }
       Object prod(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              *
                              ((Integer)r).intValue());
       }       
   }

再实现，Set（集合）类型的运算操作。

.. code-block:: java

   abstract class SetD {
       SetD add(Integer i){
           if (mem(i))
               return this;
           else
               return new Add(i, this);
       }
       abstract boolean mem(Integer i);
       abstract SetD plus(SetD s);
       abstract SetD diff(SetD s);
       abstract SetD prod(SetD s);
   }

   class Empty extends SetD {
       boolean mem(Integer i){
           return false;
       }
       SetD plus(SetD s){
           return s;
       }
       SetD diff(SetD s){
           return new Empty();
       }
       SetD prod(SetD s){
           return new Empty();
       }
   }

   class Add extends SetD {
       Integer i;
       SetD s;
       Add(Integer _i, SetD _s){
           i = _i;
           s = _s;
       }
       boolean mem(Integer n){
           if (i.equals(n))
               return true;
           else
               return s.mem(n);
       }
       SetD plus(SetD t){
           return s.plus(t.add(i));
       }
       SetD diff(SetD t){
           if (t.mem(i))
               return s.diff(t);
           else
               return s.diff(t).add(i);
       }
       SetD prod(SetD t){
           if (t.mem(i))
               return s.prod(t).add(i);
           else
               return s.prod(t);
       }
   }

   class SetEvalV extends IntEvalV {
       Object plus(Object l, Object r){
           return ((SetD)l).plus((SetD)r);
       }
       Object diff(Object l, Object r){
           return ((SetD)l).diff((SetD)r);
       }
       Object prod(Object l, Object r){
           return ((SetD)l).prod((SetD)r);
       }
   }

`SetEvalV` 直接继承  `IntEvalV` 有点不合理，好，我们改一下。

再抽象一个基类出来，将两者的公共代码放在基类里，然后让两者继承该基类。

.. code-block:: java

   abstract class EvalD implements ExprVisitorI {
       public Object forPlus(ExprD l, ExprD r){
           return plus(l.accept(this), r.accept(this));
       }
       public Object forDiff(ExprD l, ExprD r){
           return diff(l.accept(this), r.accept(this));
       }
       public Object forProd(ExprD l, ExprD r){
           return prod(l.accept(this), r.accept(this));
       }
       public Object forConst(Object c){
           return c;
       }
       abstract Object plus(Object l, Object r);
       abstract Object diff(Object l, Object r);
       abstract Object prod(Object l, Object r);
   }

   class IntEvalV extends EvalD {
       Object plus(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              +
                              ((Integer)r).intValue());
       }
       Object diff(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              -
                              ((Integer)r).intValue());
       }
       Object prod(Object l, Object r){
           return new Integer(((Integer)l).intValue()
                              *
                              ((Integer)r).intValue());
       }                 
   }

   class SetEvalV extends EvalD {
       Object plus(Object l, Object r){
           return ((SetD)l).plus((SetD)r);
       }
       Object diff(Object l, Object r){
           return ((SetD)l).diff((SetD)r);
       }
       Object prod(Object l, Object r){
           return ((SetD)l).prod((SetD)r);
       }
   }

还记得第6章的 `SubstV` 和 `LtdSubstV` 么？

它们有很多相似的地方，能否采用上面的办法，合并起来？

.. code-block:: java

   abstract class SubstD implements PieVisitorI {
       Object n;
       Object o;
       SubstD(Object _n, Object _o) {
           n = _n;
           o = _o;
       }
       public PieD forBot() {
           return new Bot();
       }
       public abstract PieD forTop(Object t, PieD r);
   }

   class SubstV extends SubstD {
       SbustV(Object _n, Object _o) {
           super(_n, _o);
       }
       public PieD forTop(Object t, PieD r) {
           if (o.equals(t))
               return new Top(n, r.accept(this));
           else
               return new Top(t, r.accept(this));
       }
   }

   class LtdSubstV extends SubstD {
       int c;
       LtdSubstV(int _c, Object _n, Object _o) {
           super(_n, _o);
           c = _c;
       }
       public PieD forTop(Object t, PieD r) {
           if (c == 0)
               return new Top(t, r);
           else
               if (o.equals(t))
                   return new Top(n, r.accept(LtdSubstV(c-1, n, o)));
               else
                   return new Top(t, r.accept(this));           
       }
   }

.. tip::

   **第八条建议**

   When extending a class, use overriding

   to enrich its functionality.

根据以上建议， `LtdSubstV` 可以直接在 `SubstV` 类上进行继承和扩展。

.. code-block:: java

   class SbustV implements PieVisitorI {
       Object n;
       Object o;
       SubstV(Object _n, Object _o) {
           n = _n;
           o = _o;
       }
       public PieD forBot() {
           return new Bot();
       }
       public PieD fotTop(Object t, PieD r) {
           if (o.equals(t))
               return new Top(n, r.accept(this));
           else
               return new Top(t, r.accept(this));
       }
   }

   class LtdSubstV extends SubstV {
       int c;
       Object n;
       Object o;
       LtdSubstV(int _c, Object _n, Object _o) {
           super(_n, _o);
           c = _c;
       }
       public PieD forTop(Object t, PieD r) {
           if (c == 0)
               return new Top(t, r);
           else
               if (o.equals(t))
                   return new Top(n, r.accept(LtdSubstV(c-1, n, o)));
               else
                   return new Top(t, r.accept(this));
       }
   }

.. tip::

   到此章结束，自定义类型及其相关运算操作都完全的面向对象化了。
