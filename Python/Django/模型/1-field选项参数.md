### 常用选项参数意义

1. #### null

   数据库中字段是否可以为空（`null=True`），默认是为`False`。在使用字符串相关的`Field`（CharField/TextField）的时候，**官方推荐尽量不要使用这个参数**，也就是保持默认值`False`。因为`Django`在处理字符串相关的`Field`的时候，即使这个`Field`的`null=False`，如果你没有给这个`Field`传递任何值，那么`Django`也会使用一个空的字符串`""`来作为默认值存储进去。因此如果再使用`null=True`，`Django`会产生两种空值的情形（NULL或者空字符串）。如果想要在表单验证的时候允许这个字符串为空，那么建议使用`blank=True`。

   如果你的`Field`是`BooleanField`，那么对应的可空的字段则为`NullBooleanField`。

2. #### blank

   标识这个字段在表单验证的时候是否可以为空。默认是`False`。
   这个和`null`是有区别的，`null`是一个纯数据库级别的。而`blank`是表单验证级别的。

3. #### max_length

   字段长度

4. #### db_column

   数据库中字段的列名(`db_column="test"`)，如果没有设置这个参数，那么将会使用模型中属性的名字。

5. #### db_tablespace

   定义这个 `model` 所使用的数据库 表空间 。如果在项目的 `setting` 中定义了 `DEFAULT_TABLESPACE` 那么它会使用这个值。如果后台数据库不支持 表空间，这个选项会被忽略。

6. #### default

   数据库中字段的默认值

7. #### primary_key

   数据库中字段是否为主键(`primary_key=True`)

8. #### db_index

   数据库中字段是否可以建立索引(`db_index=True`)

9. #### unique

   数据库中字段是否可以建立唯一索引(`unique=True`)

10. #### unique_for_date

    数据库中字段【日期】部分是否可以建立唯一索引

11. #### unique_for_month 

    数据库中字段【月】部分是否可以建立唯一索引

12. #### unique_for_year

    数据库中字段【年】部分是否可以建立唯一索引

13. #### auto_now

    添加或者更新时自动更新当前时间

14. #### auto_now_add

    创建时自动更新当前时间

15. #### verbose_name

    `Admin`中显示的字段名称

16. #### editable

    是否可以编辑

17. #### help_text

    该字段的提示信息

18. #### choices

    显示选择框的内容，用不变动的数据放在内存中从而避免跨表操作
    如：

    ```python
    sex=models.IntegerField(choices=[(0,'男'),(1,'女'),],default=1)
    ```

    `error_messages`自定义错误信息（字典类型），从而定制想要显示的错误信息；
    字典健：`null`,`blank`,`invalid`,`invalid_choice`,`unique`,`andunique_for_date`
    如：`{'null':"不能为空.",'invalid':'格式错误'}`

19. #### validators

    自定义错误验证（列表类型），从而定制想要的验证规则

    ```python
    from django.core.validators import RegexValidator
    from django.core.validators import EmailValidator,URLValidator,DecimalValidator,
    MaxLengthValidator,MinLengthValidator,MaxValueValidator,MinValueValidator
    如：
    test = models.CharField(
        max_length=32,
        error_messages={
        'c1': '优先错信息1',
        'c2': '优先错信息2',
        'c3': '优先错信息3',
    },
    validators=[
        RegexValidator(regex='root_\d+', message='错误了', code='c1'),
        RegexValidator(regex='root_112233\d+', message='又错误了', code='c2'),
        EmailValidator(message='又错误了', code='c3'), ]
    )
    ```

    

