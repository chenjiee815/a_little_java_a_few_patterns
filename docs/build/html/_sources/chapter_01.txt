=============
 Modern Toys
=============
:tags: java, oop
:category: java

.. contents::

----------------------------------------

这一章节，作者通过一系列的对话，\
让读者了解到Java中基本类型（只介绍了int, boolean类型），\
然后引申到如何使用Java自定义类型（引用类型）。

类型是什么？
::

   A type is a name for a collection of values

SeasoningD
----------
自定义 `SeasoningD` 类型，以其它的四个子类型。

.. code-block:: java

   abstract class SeasoningD{} //调味品

   class Salt extends SeasoningD{} //盐

   class Pepper extends SeasoningD{} //胡椒粉

   class Thyme extends SeasoningD{} //百里香

   class Sage extends SeasoningD{} //鼠尾草

虽然四个子类型没有定义 `构造函数` ，\
但是Java会自动添加一个默认的构造函数。

PointD
------
再自定义个 `PointD` 类型，和它的两个子类型。

.. code-block:: java

   abstract class PointD{} //坐标

   class CartesianPt extends PointD{ //笛卡尔坐标
       int x;
       int y;
       CartesianPt(int _x, int _y){
           x = _x;
           y = _y;
       }
   }

   class ManhattanPt extends PointD{ //曼哈顿坐标
       int x;
       int y;
       ManhattanPt(int _x, int _y){
           x = _x;
           y = _y;
       }
   }

PointD的两个子类型就手动添加了构造函数，\
因为它们需要有额外的属性传入构造函数。

当使用 `new` 关键字时， \
Java会通过调用类的构造函数来生成其对应的实例。

对抽象类直接使用 `new` 关键字是不行的，\
因为抽象类是一个未完全定义的类，无法实例化。

NumD
----
再定义个 `NumD` 类型，和它的两个子类型。

.. code-block:: java

   abstract class NumD{}

   class Zero extends NumD{}

   class OneMoreThan extends NumD{
       NumD predecessor;
       OneMoreThan(NumD _d){
           predecessor = _d;
       }
   }

使用这两个子类型，就可以表示一个整数系统。

1. `Zero` 表示 `0` 

2. `new OneMoreThan(new Zero())` 表示 `1`

3. `new OneMoreThan(new OneMoreThan(new Zero()))` 表示 `2`

4. ... ...

.. tip::

   上面的概念和 `Church encoding`_ 很类似了。

`abstract` 、 `class` 、 `extends` 各代表什么？
::

  `abstract` 定义类型

  `class` 定义子类型

  `extends` 将以上两者联系起来

**第一条建议**

  When specifying a collection of data,

  use *abstract* classes for datatypes and

  *extended* classes for variants.

LayerD
------
.. code-block:: java

   abstract class LayerD{}

   class Base extends LayerD{
       Object o;
       Base(Object _o){
           o = _o;
       }
   }

   class Slice extends LayerD{
       LayerD l;
       Slice(LayerD _l){
           l = _l;
       }
   }

书中通过一系列对话和示例来揭示\
自定义类型与Java提供的基本类型的不同之处，\
给读者一个基本印象：基本类型不能直接作用于自定义类型，\
而是将之先转换为类似自定义类型的形式，然后才能使用。

接下来的章节会其进行进一步的揭示。

.. tip::

   Java不能说是完全的面向对象。为了性能考虑，Java的基本类型并非对象。

   如果需要将之转换为对象，需要使用Java提供的包装类才行。

   如果想深入了解：请Google `Java 基本类型 引用类型`

.. _`Church encoding`: http://en.wikipedia.org/wiki/Church_encoding
