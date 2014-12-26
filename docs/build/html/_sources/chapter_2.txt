========================
 Methods to Our Madness
========================
:tags: java, oop
:category: java

.. contents::

----------------------------------------

上一章讲解了如何在Java中的定义类型。

这一章主要讲如何向这些类型添加方法。

PointD
------
.. code-block:: java

   abstract class PointD{
       abstract int distanceTo0();
   }

   class CartesianPt extends PointD{ //笛卡尔坐标
       int x;
       int y;
       CartesianPt(int _x, int _y){
           x = _x;
           y = _y;
       }
       int distanceTo0(){
           return (int)Math.sqrt(x * x + y * y);
       }
   }

   class ManhattanPt extends PointD{ //曼哈顿坐标
       int x;
       int y;
       ManhattanPt(int _x, int _y){
           x = _x;
           y = _y;
       }
       int distanceTo0(){
           return x + y;
       }
   }

当子类型（具体类）从类型（抽象类）继承时，\
需要同时实现抽象类中的抽象方法。

ShishD
------
.. code-block:: java

   // 书上的例子中各个类一层层的套在一起，可以理解成一个烤串
   abstract class ShishD { //羊肉串
       abstract boolean onlyOnions(); //烤串上是不是只有洋葱
       abstract boolean isVegetarian(); //烤串上是不是全是蔬菜
   }

   class Skewer extends ShishD { //串，烤肉叉子
       boolean onlyOnions(){
           return true;
       }

       boolean isVegetarian(){
           return true;
       }
   }

   class Onion extends ShishD { //洋葱
       ShishD s;
       Onion(ShishD _s) {
           s = _s;
       }

       boolean onlyOnions(){
           return s.onlyOnions();
       }

       boolean isVegetarian(){
           return s.isVegetarian();
       }
   }

   class Lamb extends ShishD { //羔羊肉
       ShishD s;
       Lamb(ShishD _s) {
           s = _s;
       }

       boolean onlyOnions(){
           return false;
       }

       boolean isVegetarian(){
           return false;
       }
   }

   class Tomato extends ShishD { //西红柿
       ShishD s;
       Tomato(ShishD _s) {
           s = _s;
       }

       boolean onlyOnions(){
           return false;
       }

       boolean isVegetarian(){
           return s.isVegetarian();
       }
   }

**第二条建议**

  When writing a function over a datatype,

  place a method in each of the variants that make up the datatype.

  If a field of a variant belongs to the same datatype,

  the method may call the corresponding method of the field in

  computing the function.

KebabD
------
.. code-block:: java

   abstract class KebabD { //烤肉
       abstract boolean isVeggie(); //是否以纯蔬菜为辅料的烤肉
       abstract Object whatHolder(); //烤肉的摆放工具是什么
   }

   class Holder extends KebabD { //烤肉摆放工具（意译）
       Object o;
       Holder (Object _o) {
           o = _o;
       }
       boolean isVeggie(){
           return true;
       }
       Object whatHolder(){
           return o;
       }
   }

   class Shallot extends KebabD { //葱
       KebabD k;
       Shallot(KebabD _k) {
           k = _k;
       }
       boolean isVeggie(){
           return k.isVeggie();
       }
       Object whatHolder(){
           return k.whatHolder();
       }
   }

   class Shrimp extends KebabD { //小虾
       KebabD k;
       Shrimp(KebabD _k) {
           k = _k;
       }
       boolean isVeggie(){
           return false;
       }
       Object whatHolder(){
           return k.whatHolder();
       }
   }

   class Radish extends KebabD { //萝卜
       KebabD k;
       Radish(KebabD _k) {
           k = _k;
       }
       boolean isVeggie(){
           return k.isVeggie();
       }
       Object whatHolder(){
           return k.whatHolder();
       }
   }

   class Pepper extends KebabD { //胡椒粉
       KebabD k;
       Pepper(KebabD _k) {
           k = _k;
       }
       boolean isVeggie(){
           return k.isVeggie();
       }
       Object whatHolder(){
           return k.whatHolder();
       }
   }

   class Zucchini extends KebabD { //西葫芦
       KebabD k;
       Zucchini(KebabD _k) {
           k = _k;
       }
       boolean isVeggie(){
           return k.isVeggie();
       }
       Object whatHolder(){
           return k.whatHolder();
       }
   }

定义一下烤肉摆放的工具。

大致分成两种:

* 一种是将烤肉串起来的工具

  .. code-block:: java
  
     abstract class RodD{} //杆，用于将烤肉串起来
  
     class Dagger extends RodD{} //匕首
  
     class Sabre extends RodD{} //军刀
  
     class Sword extends RodD{} //剑
  
* 一种将烤肉平铺的工具。

  .. code-block:: java
  
     abstract class PlateD{} //盘子
  
     class Gold extends PlateD{} //金盘子
  
     class Silver extends PlateD{} //银盘子
  
     class Brass extends PlateD{} //黄铜盘子
  
     class Copper extends PlateD{} //镀铜盘子
  
     class Wood extends PlateD{} //木盘子

PointD
------
.. code-block:: java

   abstract class PointD{
       abstract int distanceTo0();
   }

   class CartesianPt extends PointD{ //笛卡尔坐标
       int x;
       int y;
       CartesianPt(int _x, int _y){
           x = _x;
           y = _y;
       }
       int distanceTo0(){
           return (int)Math.sqrt(x * x + y * y);
       }
       boolean closerTo0(CartesianPt p){
           return distanceTo0() <= p.distanceTo0();
       }
   }

   class ManhattanPt extends PointD{ //曼哈顿坐标
       int x;
       int y;
       ManhattanPt(int _x, int _y){
           x = _x;
           y = _y;
       }
       int distanceTo0(){
           return x + y;
       }
       boolean closerTo0(ManhattanPt p){
           return distanceTo0() <= p.distanceTo0();
       }
   }

抽取变体类型中公共的部分到抽象类型中。

.. code-block:: java

   abstract class PointD{
       int x;
       int y;
       PointD(int _x, int _y){
           x = _x;
           y = _y;
       }
       abstract int distanceTo0();
       boolean closerTo0(PointD p){
           return distanceTo0() <= p.distanceTo0();
       }
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
