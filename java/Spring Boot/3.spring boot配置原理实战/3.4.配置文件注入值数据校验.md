# 一、如何对注入的属性进行校验

比如：我们希望family里面爸爸的年龄不能小于21岁，小于21就是不合理的配置。

在需要校验的属性装配类上加@Validated注解

```java
@Data
@Component
@Validated
@ConfigurationProperties(prefix = "family")
public class Family {
```

在需要校验的属性装配类上加@Validated注解,只有加上才会触发数据校验
@Validated注解的在此处的作用是配合@ConfigurationProperties进行属性校验。

# 二、数据校验注解列表（部分常用）

| 限制                      | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| @Null                     | 限制只能为null                                               |
| @NotNull                  | 限制必须不为null                                             |
| @AssertFalse              | 限制必须为false                                              |
| @AssertTrue               | 限制必须为true                                               |
| @DecimalMax(value)        | 限制必须为一个不大于指定值的数字                             |
| @DecimalMin(value)        | 限制必须为一个不小于指定值的数字                             |
| @Digits(integer,fraction) | 限制必须为一个小数，且整数部分的位数不能超过integer，小数部分的位数不能超过fraction |
| @Future                   | 限制必须是一个将来的日期                                     |
| @Max(value)               | 限制必须为一个不大于指定值的数字                             |
| @Min(value)               | 限制必须为一个不小于指定值的数字                             |
| @Past                     | 限制必须是一个过去的日期                                     |
| @Pattern(value)           | 限制必须符合指定的正则表达式                                 |
| @Size(max,min)            | 限制字符长度必须在min到max之间                               |
| @Past                     | 验证注解的元素值（日期类型）比当前时间早                     |
| @NotEmpty                 | 验证注解的元素值不为null且不为空（字符串长度不为0、集合大小不为0） |
| @NotBlank                 | 验证注解的元素值不为空（不为null、去除首位空格后长度为0），不同于@NotEmpty，@NotBlank只应用于字符串且在比较时会去除字符串的空格 |
| @Email                    | 验证注解的元素值是Email，也可以通过正则表达式和flag指定自定义的email格式 |

- 校验familyName，必须不能为空

```java
@NotEmpty
private String familyName;
```

- 校验父亲的年龄，必须大于21岁

```java
public class Father {
    private String name;
    @Min(21)
    private Integer age;
}
```

# 三、其他例子：

- @size (min=6, max=20, message="密码长度只能在6-20之间")
- @pattern (regexp="[a-za-z0-9._%+-]+@[a-za-z0-9.-]+\.[a-za-z]{2,4}", message="请输入正确的邮件格式")
- @Length(min = 5, max = 20, message = "用户名长度必须位于5到20之间")
- @Email(message = "请输入正确的邮箱")
- @NotNull(message = "用户名称不能为空")
- @Max(value = 100, message = "年龄不能大于100岁")
- @Min(value= 18 ,message= "必须年满18岁！" )
- @AssertTrue(message = "bln4 must is true")
- @AssertFalse(message = "blnf must is falase")
- @DecimalMax(value="100",message="decim最大值是100")
- @DecimalMin(value="100",message="decim最小值是100")
- @NotNull(message = "身份证不能为空")
- @Pattern(regexp="^(\d{18,18}|\d{15,15}|(\d{17,17}[x|X]))$", message="身份证格式错误")

# 四、测试

针对Family的属性校验，只需要写一个测试类，将Family类注入就可以。

```java
@Resource
Family family;
```

如果校验失败，会有如下异常。

![](https://cdn.jsdelivr.net/gh/krislinzhao/IMGcloud/img/20200417165917.png)