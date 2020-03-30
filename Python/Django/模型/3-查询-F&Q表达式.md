## F表达式和Q表达式：

### F表达式：

`F表达式`是用来优化`ORM`操作数据库的。

比如我们要将公司所有员工的薪水都增加1000元，如果按照正常的流程，应该是先从数据库中提取所有的员工工资到Python内存中，然后使用Python代码在员工工资的基础之上增加1000元，最后再保存到数据库中。这里面涉及的流程就是，首先从数据库中提取数据到Python内存中，然后在Python内存中做完运算，之后再保存到数据库中。示例代码如下：

```python
employees = Employee.objects.all()
for employee in employees:
    employee.salary += 1000
    employee.save()
```

而我们的`F表达式`就可以优化这个流程，他可以不需要先把数据从数据库中提取出来，计算完成后再保存回去，他可以直接执行`SQL语句`，就将员工的工资增加1000元。示例代码如下：

```python
from djang.db.models import F
Employee.object.update(salary=F("salary")+1000)
```

**F表达式并不会马上从数据库中获取数据，而是在生成`SQL`语句的时候，动态的获取传给F表达式的值。**

比如如果想要获取作者中，`name`和`email`相同的作者数据。如果不使用`F表达式`，那么需要使用以下代码来完成：

```python
    authors = Author.objects.all()
    for author in authors:
        if author.name == author.email:
            print(author)
```

如果使用`F表达式`，那么一行代码就可以搞定。示例代码如下：

```python
    from django.db.models import F
    authors = Author.objects.filter(name=F("email"))
```

### Q表达式：

**使用Q表达式包裹查询条件，可以在条件之间进行多种操作。与/或 非等，从而实现一些复杂的查询操作**

如果想要实现所有价格高于100元，并且评分达到9.0以上评分的图书。那么可以通过以下代码来实现：

```python
books = Book.objects.filter(price__gte=100,rating__gte=9)
```

以上这个案例是一个并集查询，可以简单的通过传递多个条件进去来实现。
但是如果想要实现一些复杂的查询语句，比如要查询所有价格低于10元，或者是评分低于9分的图书。那就没有办法通过传递多个条件进去实现了。这时候就需要使用`Q表达式`来实现了。示例代码如下：

```python
from django.db.models import Q
books = Book.objects.filter(Q(price__lte=10) | Q(rating__lte=9))
```

以上是进行或运算，当然还可以进行其他的运算，比如有`&`和`~（非）`等。一些用`Q`表达式的例子如下：

```python
from django.db.models import Q
# 获取id等于3的图书
books = Book.objects.filter(Q(id=3))
# 获取id等于3，或者名字中包含文字"记"的图书
books = Book.objects.filter(Q(id=3)|Q(name__contains("记")))
# 获取价格大于100，并且书名中包含"记"的图书
books = Book.objects.filter(Q(price__gte=100)&Q(name__contains("记")))
# 获取书名包含“记”，但是id不等于3的图书
books = Book.objects.filter(Q(name__contains='记') & ~Q(id=3))
```

## 使用 Q 对象进行复杂查询

在 `filter()` 中使用多个关键字参数查询，对应到数据库中是使用 `AND` 连接起来的。如果你想使用更复杂的查询（例如，使用 `OR` 语句的查询），可以使用 `Q` 对象。

`Q` 对象（`django.db.models.Q`）是一个用于封装关键字参数集合的对象。这些关键字参数在上面的 “字段查找” 中指定。

例如，这个 `Q` 对象封装了一个 `LIKE` 查询：

```
from django.db.models import Q Q(question__startswith='What')
```

`Q` 对象可以使用 `＆` 和 `|` 进行组合运算。当一个操作符用于两个 `Q` 对象时，它会生成一个新的 `Q` 对象。

例如，这个语句产生一个 `Q` 对象，它表示两个 `"question__startswith"` 查询的 `"OR"`：

```
Q(question__startswith='Who') | Q(question__startswith='What')
```

这相当于以下 SQL WHERE 子句：

```
WHERE question LIKE 'Who%' OR question LIKE 'What%'
```

通过将 `Q` 对象与 `＆` 和 `|` 相结合，您可以编写任意复杂的语句运算符并使用括号分组。此外，可以使用 `〜` 运算符来否定 `Q` 对象，从而允许将普通查询和否定（`NOT`）查询组合在一起的组合查找：

```
Q(question__startswith='Who') | ~Q(pub_date__year=2005)
```

每个使用关键字参数的查找函数（例如 `filter()`，`exclude()`，`get()`）也可以传递一个或多个 `Q` 对象作为位置（未命名）参数。如果您为查找函数提供多个 `Q` 对象参数，则参数将一起 “`AND`”。例如：

```
SELECT * from polls WHERE question LIKE 'Who%'
    AND (pub_date = '2005-05-02' OR pub_date = '2005-05-06')
```

底层的 SQL 大概是这样：

```
Poll.objects.get(
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6)),
    question__startswith='Who',
)
```

查找函数可以混合使用 `Q` 对象和关键字参数。提供给查找函数的所有参数（不管它们是关键字参数还是 `Q` 对象）都是 “`AND`” 一起编辑的。如果提供了一个 `Q` 对象，它必须在任何关键字参数的定义之前。例如：

```
Poll.objects.get(  Q(pub_date=date(2005, 5, 2))|Q(pub_date=date(2005, 5, 6)), 
question__startswith='Who', )
```

...将是一个有效的查询，相当于前面的例子;但：

```
# INVALID QUERY
Poll.objects.get(
    question__startswith='Who',
    Q(pub_date=date(2005, 5, 2)) | Q(pub_date=date(2005, 5, 6))
)
```

...将无效。

> Django 单元测试中的 `OR` 查找示例显示了 `Q` 的一些可能的用法。

## `