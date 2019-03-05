# 第2节：junit的使用

JUnit是一个java语言的单元测试框架，它是由Erich Gamma和Kent Beck创建。它再测试驱动开发方面有很重要的发展，是起源于Junit的一个统称为xUnit（XUnit是一套基于测试驱动开发的测试框架：PythonUnit、CppUnit、JUnit）的单元测试框架之一。

JUnit促进了“先测试后编码”的理念，强调建立测试数据的一段代码，可以先测试，然后再应用。这增加了程序的稳定性，以及缩短排错的时间。

## Junit 版本差异

目前已知的比较主流的JUnit版本是：3.x、4.x、5.x，它们之间的时间跨度也比较大，5.x是在4.x发布十几年之后才更新的版本，不过5.x提供了**JUnit Vintage** ，用于对JUnit3和Junit4的兼容支持。

### Junit3.x & Junit4.x

- 它们之间的差异简而言之：3.x是通过继承实现，而4.x是通过注解实现。另外Junit4要求方法是静态的，但是Junit5并没有这个限制。
- 举个例子，它们实现一个“HelloWorld”的差异：

``` java
// 3.x  
public class HelloWorld extends TestCase
  {
    public void testMultiplication()
    {
      // Testing if 3*2=6:
      assertEquals ("Multiplication", 6, 3*2);
    }
  }
 // 4.x
  public class HelloWorld
  {
    @Test public void testMultiplication()
    {
      // Testing if 3*2=6:
      assertEquals ("Multiplication", 6, 3*2);
    }
  }
```

### Junit4.x & Junit5.x

Junit4第一个版本在2006年初发布，自2006年初JUnit 4发布之后，11年间陆陆续续更新了13个小版本，最新的4.12版本是在2014年底发布的。

Junit5的第一个版本是在2017年9月10日发布的，相比较其他版本，Junit5由三个不同的子项目及不同的模块组成：

- Junit platform：是在虚拟机上启动Junit测试的基础。
- Junit Jupiter： 是Junit5扩展的新的编程模型和扩展模型，用来编写测试用例，Jupiter子项目为在平台上运行Jupiter的测试提供了一个TestEngine。
- JUnit Vintage：提供了一个在平台上运行JUnit 3 和 JUnit4的TestEngine。

Junit4.x和Junit5.x注解对比

|    Junit4    |       Junit5       | 对比说明                                                     |
| :----------: | :----------------: | :----------------------------------------------------------- |
|    @Test     |       @Test        | 表示该方法是一个测试方法                                     |
| @BeforeClass |     @BeforeAll     | 表示使用了该注解的方法应该再当前类所有方法执行之前执行       |
| @AfterClass  |     @AfterAll      | 表示在类中方法执行后执行                                     |
|   @Before    |    @BeforeEach     | 表示在每一个使用了Test、@RepeatedTest、@ParameterizedTest或者@TestFactory注解的方法前执行 |
|    @After    |     @AfterEach     | 表示在每一个使用了Test、@RepeatedTest、@ParameterizedTest或者@TestFactory注解的方法后执行 |
|   @Ignore    |     @Disabled      | 用于禁用一个测试类或测试方法                                 |
|  @Category   |        @Tag        | 用于声明过滤测试的tags                                       |
| @Parameters  | @ParameterizedTest | 表示该方法是一个参数化测试                                   |
|   @RunWith   |    @ExtendWith     | 放在测试类名之前，用来确定这个类怎么运行                     |
|    @Rule     |    @ExtendWith     | Rule是一组实现了TestRule接口的共享类，提供了验证、监视TestCase和外部资源管理等能力 |
|  @ClassRule  |    @ExtendWith     | @ClassRule用于测试类中的静态变量，必须是TestRule接口的实例，且访问修饰符必须为public。 |

