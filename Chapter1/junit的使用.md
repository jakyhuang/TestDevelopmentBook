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

## Junit5实战

- junit5推出也有3年时间，且随着java8的普及，用junit5写单元测试用例也成为了趋势所在。下面会介绍一些用Junit5做单元测试的实践思路。

### java lambdas表达式

- 这里稍微花一点时间介绍下lambdas表达式，因为在junit5中可能会经常用到。

- lambdas表达式，也被称之为闭包，它是java8发布的重要特性，lambdas允许把函数作为一个方法的参数（函数作为参数传递进方法中）。使用lambda表达式可以使代码更加简洁紧凑。

- lambda表达式的重要特征：

  - **可选类型声明：**不需要声明参数类型，编译器可以统一识别参数值。
  - **可选的参数圆括号：**一个参数无需定义圆括号，但多个参数需要定义圆括号。
  - **可选的大括号：**如果主体包含了一个语句，就不需要使用大括号。
  - **可选的返回关键字：**如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。

  ``` java
  // 1. 不需要参数,返回值为 5  
  () -> 5  
    
  // 2. 接收一个参数(数字类型),返回其2倍的值  
  x -> 2 * x  
    
  // 3. 接受2个参数(数字),并返回他们的差值  
  (x, y) -> x – y  
    
  // 4. 接收2个int型整数,返回他们的和  
  (int x, int y) -> x + y  
    
  // 5. 接受一个 string 对象,并在控制台打印,不返回任何值(看起来像是返回void)  
  (String s) -> System.out.print(s)
  ```

- 另外需要注意：lambda表达式只能引用标记了final的外层局部变量，这就是说不能再lambda内部修改定义在域外的局部变量。

### 实用方法

在github上找了一个demo[工程](https://github.com/howtoprogram/junit5-examples)，基本所有的操作都可以在这个工程里找到，这个demo工程还比较贴合实际的使用场景，推荐大家跟着他一起写写单元测试。

我下面主要介绍一些junit5常用的方法，供大家参考和学习。

#### 注解

``` java
// assert all
  assertAll("Do many assertions.Java 8 Lambdas style", () -> {
            assertNotNull(actual, () -> "The actual is NULL");
            assertEquals(expAge, actual,
                    () -> "The expected is: " + expAge + " while the actual is:" + actual);
        });

// AssertThrows
 assertThrows(NumberFormatException.class, () -> {
           StringUtils.convertToDouble(age);
        });
// assertSame
assertNotSame(defaultValue, actual);
assertSame(defaultValue, actual);

// assertNull
assertNull(actual);
assertNotNull(actual);

// assertEquals
assertEquals(expected , actual)
assertNotEquals(expected, actual)
// assertTrue
assertTrue(value)
assertNotTrue(value)
// 判断数组是否相等
 assertArrayEquals(array1, array2)
// 直接使测试失败
 fail("this should fail")

```

#### junit5 前置条件

 JUnit 5 中的前置条件（assumptions）类似于断言，不同之处在于不满足的断言会使得测试方法失败，而不满足的前置条件只会使得测试方法的执行终止。

``` java
@DisplayName("Assumptions")
public class AssumptionsTest {
 private final String environment = "DEV";
 
 @Test
 @DisplayName("simple")
 public void simpleAssume() {
    assumeTrue(Objects.equals(this.environment, "DEV"));
    assumeFalse(() -> Objects.equals(this.environment, "PROD"));
 }
 
 @Test
 @DisplayName("assume then do")
 public void assumeThenDo() {
    assumingThat(
       Objects.equals(this.environment, "DEV"),
       () -> System.out.println("In DEV")
    );
 }
}
```

#### 嵌套测试

JUnit5可以通过java中的内部类和@Nested注解实现嵌套测试，从而可以将用例更好的组织在一起，在内部类中可以使用@BeforeEach 和@AfterEach 注解，而且嵌套的层次没有限制。个人感觉这种方式使用应该不会特别多。

#### 依赖注入

在 JUnit 5 之前，标准的测试类和测试方法是不允许有额外的参数的。这个限制在 JUnit 5 被取消了。JUnit 5 除了提供内置的标准参数之外，还可以通过扩展机制来支持额外的参数。

``` java
@Test
@DisplayName("test info")
// 通过 TestInfo 接口，可以获取到当前测试的相关信息，包括显示名称、标签、测试类和测试方法
public void testInfo(final TestInfo testInfo) { 
 System.out.println(testInfo.getDisplayName());
}

@Test
@DisplayName("test reporter")
// 当参数的类型是 org.junit.jupiter.api.TestReporter 时，在运行测试时，通过作为参数传入的 TestReporter 接口对象，来输出额外的键值对信息。这些信息可以被测试执行的监听器 TestExecutionListener 处理，也可以被输出到测试结果报告中，如清单 12 所示。
public void testReporter(final TestReporter testReporter) {
 testReporter.publishEntry("name", "Alex");
}
```

#### 动态测试

有些测试用例可能依赖运行时的变量，有时候会需要生成上百个不同的测试用例。这些场景都是动态测试可以发挥其长处的地方。动态测试是通过新的@TestFactory 注解来实现的。测试类中的方法可以添加@TestFactory 注解的方法来声明其是创建动态测试的工厂方法。这样的工厂方法需要返回 org.junit.jupiter.api.DynamicTest 类的集合，可以是 Stream、Collection、Iterable 或 Iterator 对象。每个表示动态测试的 DynamicTest 对象由显示名称和对应的 Executable 接口的实现对象来组成。

``` java
@TestFactory
public Collection<DynamicTest> simpleDynamicTest() {
 return Collections.singleton(dynamicTest("simple dynamic test", () -> assertTrue(2 > 1)));
}

@TestFactory
public Stream<DynamicTest> streamDynamicTest() {
 return stream(
       Stream.of("Hello", "World").iterator(),
       (word) -> String.format("Test - %s", word),
       (word) -> assertTrue(word.length() > 4)
 );
}
```



