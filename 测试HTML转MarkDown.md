<a name="top"></a>

## [SQL过关](https://www.cnblogs.com/skynet/archive/2010/07/25/1784892.html)

<small>2010-07-25 22:50 by 吴秦, 8825 阅读, 41 评论, [收藏](#), [编辑](https://i.cnblogs.com/EditPosts.aspx?postid=1784892)</small>

#### **引言**

做为一个web开发者，不与数据库打交道几乎是不可能的！由此可见，掌握SQL语句对于一个web开发人员来说是多么的重要。下面是我在整理电脑时，找到的资料，觉得还不错就拿出来与大家分享。不废话了，好不好你看了就知道。进入主题，本文的主要内容如下：

*   问题背景
*   SQL查询54问？

#### **1、问题背景**

本文中的SQL语句都是基于下面几张表的，这也是比较经典的用于数据库教学的数据库例子。

（1）/*员工人事表employee */

| emp_no | char(5) | Not null | **primary key** | 员工编号 |
| emp_name | char(10) | Not null |  | 员工姓名 |
| sex | char(1) | Not null |  | 性别 |
| dept | char(4) | Not null |  | 所属部门 |
| title | char(6) | Not null |  | 职称 |
| date_hired | datetime | Not null |  | 到职日 |
| birthday | datetime | Null |  | 生日 |
| salary | int | Not null |  | 薪水 |
| addr | char(50) | null |  | 住址 |
| Mod­_date | datetime | Default(getdate()) | 操作者 |

（2）/*客户表customer */

| cust_id | char(5) | Not null | **primary key** | 客户号 |
| cust_name | char(20) | Not null, |   | 客户名称 |
| addr | char(40) | Not null, |   | 客户住址 |
| tel_no | char(10) | Not null, |   | 客户电话 |
| zip | char(6) | null |   | 邮政编码 |

（3）/*销售主表sales */

| order_no | int | Not null | **primary key** | 订单编号 |
| cust_id | char(5) | Not null, |   | 客户号 |
| sale_id | char(5) | Not null, |   | 业务员编号 |
| tot_amt | numeric(9,2) | Not null, |   | 订单金额 |
| order_date | datetime | Not null, |   | 订货日期 |
| ship_date | datetime | Not null, |   | 出货日期 |
| invoice_no | char(10) | Not null |   | 发票号码 |

（4）/*销货明细表sale_item */

| order_no | int | Not null, | **primary key** | 订单编号 |
| prod_id | char(5) | Not null, | 产品编号 |
| qty | int | Not null |  | 销售数量 |
| unit_price | numeric(7,2) | Not null |  | 单价 |
| order_date | datetime | null |  | 订单日期 |

（5）/*产品名称表product */

| prod_id | char(5) | Not null | **primary key** | 产品编号 |
| prod_name | char(20) | Not null |   | 产品名称 |

#### **2、各种查询的SQL语句**

问题1、查找员工的编号、姓名、部门和出生日期，如果出生日期为空值，显示日期不详,并按部门排序输出,日期格式为yyyy-mm-dd。

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no ,emp_name ,dept ,
       isnull([convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(10),birthday,120),'日期不详') birthday
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept</pre>

问题2、查找与喻自强在同一个单位的员工姓名、性别、部门和职称

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)请在思考之后展开[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no ,emp_name ,dept ,
       isnull([convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(10),birthday,120),'日期不详') birthday
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept</pre>

问题3、按部门进行汇总，统计每个部门的总工资

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept
</pre>

问题4、查找商品名称为14寸显示器商品的销售情况，显示该商品的编号、销售数量、单价和金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id,qty,unit_price,unit_price*qty totprice
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item a,product b
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id=b.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_name='14寸显示器'</pre>

问题5、在销售明细表中按产品编号进行汇总，统计每种产品的销售数量和金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty) totqty,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty*unit_price) totprice
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id</pre>

问题6、使用convert函数按客户编号统计每个客户1996年的订单总金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) totprice
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(4),order_date,120)='1996'
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id</pre>

问题7、查找有销售记录的客户编号、名称和订单总额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) totprice
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a,sales b
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name</pre>

问题8、查找在1997年中有销售记录的客户编号、名称和订单总额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) totprice
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a,sales b
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(4),order_date,120)='1997'
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name</pre>

问题9、查找一次销售最大的销售记录

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no,cust_id,sale_id,tot_amt
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt=
   ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [max](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=max&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)
    [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales)</pre>

问题10、查找至少有3次销售的业务员名单和销售日期

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_name,order_date
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a,sales b 
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no=sale_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.emp_no [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
  ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id
   [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
   [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id
   [having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(*)>=3)
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_name</pre>

问题11、用存在量词查找没有订货记录的客户名称

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
   ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *
    [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales b
    [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id)</pre>

问题12、使用左外连接查找每个客户的客户编号、名称、订货日期、订单金额；订货日期不要显示时间，日期格式为yyyy-mm-dd；按客户编号排序，同一客户再按订单降序排序输出

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name,[convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(10),order_date,120),tot_amt
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a [left](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=left&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [outer](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=outer&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [join](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=join&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales b [on](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=on&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,tot_amt [desc](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=desc&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)</pre>

问题13、查找16M DRAM的销售情况，要求显示相应的销售员的姓名、性别，销售日期、销售数量和金额，其中性别用男、女表示

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_name 姓名, 性别= [case](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=case&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.sex  [when](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=when&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'm' [then](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=then&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '男'
                                       [when](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=when&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'f' [then](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=then&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '女' 
                                       [else](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=else&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '未'
                                       [end](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=end&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99),
        销售日期= isnull([convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(10),c.order_date,120),'日期不详'),
        qty 数量, qty*unit_price [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 金额
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a, sales b, sale_item c,product d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) d.prod_name='16M DRAM' [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) d.pro_id=c.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
      a.emp_no=b.sale_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) b.order_no=c.order_no</pre>

问题14、查找每个人的销售记录，要求显示销售员的编号、姓名、性别、产品名称、数量、单价、金额和销售日期

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no 编号,emp_name 姓名, 性别= [case](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=case&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.sex [when](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=when&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'm' [then](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=then&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '男'
                                       [when](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=when&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'f' [then](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=then&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '女' 
                                       [else](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=else&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '未'
                                       [end](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=end&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99),
      prod_name 产品名称,销售日期= isnull([convert](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=convert&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([char](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=char&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(10),c.order_date,120),'日期不详'),
      qty 数量, qty*unit_price [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 金额
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a [left](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=left&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [outer](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=outer&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [join](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=join&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales b [on](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=on&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.emp_no=b.sale_id , sale_item c,product d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) d.pro_id=c.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) b.order_no=c.order_no</pre>

问题15、查找销售金额最大的客户名称和总货款

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name,d.cust_sum
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)   customer a,
       ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,cust_sum
        [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id, [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_sum
              [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
              [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id ) b
        [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) b.cust_sum = 
               ( [select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [max](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=max&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(cust_sum)
                 [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id, [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_sum
                       [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
                       [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id ) c )
        ) d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=d.cust_id</pre>

问题16、查找销售总额少于1000元的销售员编号、姓名和销售额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name,d.sale_sum
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)   employee a,
       ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,sale_sum
        [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id, [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_sum
              [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
              [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id ) b
        [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) b.sale_sum <1000               
        ) d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.emp_no=d.sale_id</pre>

问题17、查找至少销售了3种商品的客户编号、客户名称、商品编号、商品名称、数量和金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name,b.prod_id,prod_name,d.qty,d.qty*d.unit_price
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a, product b, sales c, sale_item d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=c.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) d.prod_id=b.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 
      c.order_no=d.order_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) (
      [select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id
      [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,[count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([distinct](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=distinct&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id) prodid
             [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,prod_id
                   [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales e,sale_item f
                   [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) e.order_no=f.order_no) g
             [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id
             [having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([distinct](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=distinct&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id)>=3) h )</pre>

问题18、查找至少与世界技术开发公司销售相同的客户编号、名称和商品编号、商品名称、数量和金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,cust_name,d.prod_id,prod_name,qty,qty*unit_price
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a, product b, sales c, sale_item d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=c.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) d.prod_id=b.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 
      c.order_no=d.order_no  [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
  ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) f.*
   [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer x ,sales e, sale_item f
   [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name='世界技术开发公司' [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) x.cust_id=e.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
         e.order_no=f.order_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
           ( [select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) g.*
             [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item g, sales  h
             [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) g.prod_id = f.prod_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) g.order_no=h.order_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
                   h.cust_id=a.cust_id)
    )</pre>

问题19、查找表中所有姓刘的职工的工号，部门，薪水

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name,dept,salary
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_name [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '刘%'</pre>

问题20、查找所有定单金额高于20000的所有客户编号

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt>20000</pre>

问题21、统计表中员工的薪水在40000-60000之间的人数

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(*)[as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 人数
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) salary [between](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=between&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 40000 [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 60000</pre>

问题22、查询表中的同一部门的职工的平均工资，但只查询＂住址＂是＂上海市＂的员工

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary) avg_sal,dept 
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee 
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) addr [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '上海市%'
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept</pre>

问题23、将表中住址为"上海市"的员工住址改为"北京市"

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[update](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=update&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee  
[set](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=set&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) addr [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '北京市'
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) addr [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '上海市'</pre>

问题24、查找业务部或会计部的女员工的基本信息

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name,dept
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee 
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sex='F'and dept [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ('业务','会计')</pre>

问题25、显示每种产品的销售金额总和，并依销售金额由大到小输出

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id ,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty*unit_price)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item 
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty*unit_price) [desc](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=desc&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)</pre>

问题26、选取编号界于‘C0001’和‘C0004’的客户编号、客户名称、客户地址。

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) CUST_ID,cust_name,addr
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer 
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id [between](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=between&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'C0001' [AND](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=AND&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'C0004'</pre>

问题27、计算出一共销售了几种产品

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([distinct](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=distinct&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id) [as](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=as&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '共销售产品数'
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item</pre>

问题28、将业务部员工的薪水上调3%

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[update](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=update&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[set](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=set&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) salary=salary*1.03
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept='业务'</pre>

问题29、由employee表中查找出薪水最低的员工信息

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) salary=
       ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [min](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=min&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary )
        [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee )</pre>

问题30、使用join查询客户姓名为"客户丙"所购货物的"客户名称","定单金额","定货日期","电话号码"

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id,b.tot_amt,b.order_date,a.tel_no
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a [join](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=join&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales b
[on](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=on&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '客户丙'</pre>

问题31、由sales表中查找出订单金额大于“E0013业务员在1996/10/15这天所接每一张订单的金额”的所有订单

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt>[all](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=all&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
       ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt 
        [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales 
        [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id='E0013'and order_date='1996/10/15')
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt</pre>

问题32、计算'P0001'产品的平均销售单价

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(unit_price)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) prod_id='P0001'</pre>

问题33、找出公司女员工所接的定单

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,tot_amt
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 
([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sex='F')</pre>

问题34、找出同一天进入公司服务的员工

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.emp_no,a.emp_name,a.date_hired
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a
[join](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=join&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee b
[on](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=on&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) (a.emp_no!=b.emp_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.date_hired=b.date_hired)
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.date_hired</pre>

问题35、找出目前业绩超过232000元的员工编号和姓名

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee 
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales 
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id
[having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)<232000)</pre>

问题36、查询出employee表中所有女职工的平均工资和住址在＂上海市＂的所有女职工的平均工资

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sex [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'f'
[union](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=union&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sex [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 'f' [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) addr [like](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=like&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) '上海市%'</pre>

问题37、在employee表中查询薪水超过员工平均薪水的员工信息

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) * [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) salary>([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary)  [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee)</pre>

问题38、找出目前销售业绩超过40000元的业务员编号及销售业绩，并按销售业绩从大到小排序

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id ,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales 
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id 
[having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)>40000
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [desc](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=desc&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)</pre>

问题39、找出公司男业务员所接且订单金额超过2000元的订单号及订单金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no,tot_amt
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales ,employee
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id=emp_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sex='M' [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt>2000</pre>

问题40、查询sales表中订单金额最高的订单号及订单金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no,tot_amt [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt=([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [max](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=max&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)  [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales)</pre>

问题41、查询在每张订单中订购金额超过24000元的客户名及其地址

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name,addr [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a,sales b [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt>24000</pre>

问题42、求出每位客户的总订购金额，显示出客户号及总订购金额，并按总订购金额降序排列

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[Group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id 
[Order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [desc](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=desc&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)</pre>

问题43、求每位客户订购的每种产品的总数量及平均单价，并按客户号，产品号从小到大排列

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,prod_id,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty),[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty*unit_price)/[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty)
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales a, sale_item b
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.order_no=b.order_no
[Group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,prod_id
[Order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id,prod_id</pre>

问题44、查询订购了三种以上产品的订单号

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item
[Group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no
[Having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [count](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=count&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(*)>3
</pre>

问题45、查询订购的产品至少包含了订单10003中所订购产品的订单

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  [distinct](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=distinct&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item a
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  order_no<>'10003'and  [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ( 
       [Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *  [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item b [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_no ='10003'  [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 
              ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *  [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item c [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) c.order_no=a.order_no  [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  c.prod_id=b.prod_id))</pre>

问题46、在sales表中查找出订单金额大于“E0013业务员在1996/11/10这天所接每一张订单的金额”的所有订单，并显示承接这些订单的业务员和该订单的金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,tot_amt 
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt>[all](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=all&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) tot_amt [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id='E0013' [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) order_date='1996/11/10')</pre>

问题47、查询末承接业务的员工的信息

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) *
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [not](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=not&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [exists](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=exists&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) 
         ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) * [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  sales b [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)  a.emp_no=b.sale_id)
</pre>

问题48、查询来自上海市的客户的姓名，电话、订单号及订单金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_name,tel_no,order_no,tot_amt
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a ,sales b
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) addr='上海市'</pre>

问题49、查询每位业务员各个月的业绩，并按业务员编号、月份降序排序

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,[month](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=month&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(order_date), [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) 
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales 
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,[month](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=month&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(order_date)
[order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,[month](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=month&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(order_date) [desc](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=desc&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)</pre>

问题50、求每种产品的总销售数量及总销售金额，要求显示出产品编号、产品名称，总数量及总金额，并按产品号从小到大排列

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id,prod_name,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty),[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(qty*unit_price)
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_item a,product b
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id=b.prod_id 
[Group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id,prod_name
[Order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.prod_id
</pre>

问题51、查询总订购金额超过’C0002’客户的总订购金额的客户号，客户名及其住址

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id, cust_name,addr
[From](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=From&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer
[Where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id  [in](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=in&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales 
[Group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id
[Having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)>
        ([Select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=Select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales  [where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) cust_id='C0002'))
</pre>

问题52、查询业绩最好的的业务员号、业务员名及其总销售金额

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee a,sales b
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.emp_no=b.sale_id
[group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) emp_no,emp_name
[having](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=having&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt)=
         ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [max](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=max&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(totamt)
          [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) ([select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id,[sum](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=sum&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(tot_amt) totamt
               [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sales
               [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) sale_id) c)</pre>

问题53、查询每位客户所订购的每种产品的详细清单，要求显示出客户号，客户名，产品号，产品名，数量及单价

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id, cust_name,c.prod_id,prod_name,qty, unit_price
[from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) customer a,sales b, sale_item c ,product d
[where](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=where&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) a.cust_id=b.cust_id [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) b.order_no=c.order_no [and](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=and&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) c.prod_id=d.prod_id</pre>

问题54、求各部门的平均薪水，要求按平均薪水从小到大排序

<pre>![](https://www.cnblogs.com/Images/OutliningIndicators/ContractedBlock.gif)![](https://www.cnblogs.com/Images/OutliningIndicators/ExpandedBlockStart.gif)请在思考之后展开
[select](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=select&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept,[avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary) [from](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=from&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) employee [group](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=group&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) dept [order](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=order&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [by](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=by&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99) [avg](http://search.microsoft.com/default.asp?so=RECCNT&siteid=us%2Fdev&p=1&nq=NEW&qu=avg&IntlSearch=&boolean=PHRASE&ig=01&i=09&i=99)(salary)</pre>

希望这些资料，能够对你有所帮助！

[](http://www.cnblogs.com/skynet/)

作者：吴秦
出处：http://www.cnblogs.com/skynet/
本文基于[署名 2.5 中国大陆](http://creativecommons.org/licenses/by/2.5/cn/)许可协议发布，欢迎转载，演绎或用于商业目的，但是必须保留本文的署名[吴秦](http://www.cnblogs.com/skynet/)（包含链接）.

[好文要顶](javascript:void(0);) [关注我](javascript:void(0);) [收藏该文](javascript:void(0);) [![](//common.cnblogs.com/images/icon_weibo_24.png)](javascript:void(0); "分享至新浪微博") [![](//common.cnblogs.com/images/wechat.png)](javascript:void(0); "分享至微信") [![](//pic.cnblogs.com/face/u92071.jpg)](http://home.cnblogs.com/u/skynet/) [吴秦](http://home.cnblogs.com/u/skynet/)
[关注 - 18](http://home.cnblogs.com/u/skynet/followees)
[粉丝 - 3714](http://home.cnblogs.com/u/skynet/followers) 荣誉：[推荐博客](http://www.cnblogs.com/expert/) [+加关注](javascript:void(0);) 32 1 currentDiggType = 0; [«](https://www.cnblogs.com/skynet/archive/2010/07/25/1784710.html) 上一篇：[Mongoose源码剖析：核心处理模块](https://www.cnblogs.com/skynet/archive/2010/07/25/1784710.html "发布于2010-07-25 16:41")
[»](https://www.cnblogs.com/skynet/archive/2010/07/31/1789534.html) 下一篇：[Android开发之旅: Intents和Intent Filters（实例部分）](https://www.cnblogs.com/skynet/archive/2010/07/31/1789534.html "发布于2010-07-31 15:38")

*   分类: [数据库](https://www.cnblogs.com/skynet/category/239648.html)

var allowComments=true,cb_blogId=61567,cb_entryId=1784892,cb_blogApp=currentBlogApp,cb_blogUserGuid='8d36b081-9fb2-de11-ba8f-001cf0cd104b',cb_entryCreatedDate='2010/7/25 22:50:00';loadViewCount(cb_entryId);var cb_postType=1;var isMarkdown=false; var m = window.__blog.postRendered; if (m) { m(__$("post")); } var m = window.__blog.postRenderPosts; if (m) { m(); }

---
title: 2019-3-28未命名文件 
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


欢迎使用 **{小书匠}(xiaoshujiang)编辑器**，您可以通过 `小书匠主按钮>模板` 里的模板管理来改变新建文章的内容。