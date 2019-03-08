# 第3节：TestNG

TestNG的灵感来自于JUnit和NUnit，但是比他们更强大，尤其是支持Group以及测试依赖等。它突破了以前一些框架的限制，为开发人员和测试人员提供了方便强大的编写和测试，也成为用途最广泛的单元测试框架之一。

## TestNG VS Junit4

第二节我们了解了JUnt4与JUnit5，这里我们了解下TestNG与JUnit4的主要差别是什么：

- JUnit4中BeforeClass等修饰的方法，必须声明为public static的，而TestNG则灵活的多，可以声明为private且不需要static修饰。
- TestNG支持依赖测试，dependOnMethods，用这个属性来应对测试的依赖性问题。而JUnit4并没有类似的机制。
- 参数化测试：TestNG可以在xml配置中配置parameter，另外TestNG还提供了@DataProvider注释处理这样的情况。
- TestNG提供了分组的概念，可以细分到只执行某一个分组的测试用例。

## TestNG特性

接下来我们来了解下TestNG的一些特性，在之后的工作中我们可以逐步的使用起来。

### 基本注解

- BeforeSuite/AfterSuite  在套件开始结束时执行
- BeforeClass/AfterClass  在测试类开始结束时执行
- BeforeTest/AfterTest  注释的方法将在属于`<test>`标签内的类的所有测试方法运行之前/后运行
- BeforeGroups/AfterGroups 在运行组列表前/后执行。
- BeforeMethod/AfterMethod  每个测试方法之前/后运行。
- DataProvider  标记一种提供测试数据
- Factory ： 将一个方法标记为工厂，返回`TestNG`将被用作测试类的对象。 该方法必须返回`Object []`。
- Listeners ： 定义测试类上的侦听器。
- Parameters：参数传递给Test方法
- Test：类或方法标记为测试。

### 预期异常测试

- 直接通过各例子看异常如何使用

``` java
package com.yiibai;
import org.testng.annotations.Test;
public class TestRuntime {
    @Test(expectedExceptions = ArithmeticException.class)
    public void divisionWithException() {
        int i = 1 / 0;
        System.out.println("After division the value of i is :"+ i);
    }
}
```

### 忽略测试

``` java
package com.yiibai;

import org.testng.Assert;
import org.testng.annotations.Test;

public class TestIgnore {

    @Test(enabled = false)
    public void test3() {
        Assert.assertEquals(true, true);
    }

}
```

### 超时测试

``` java
package com.yiibai;

import org.testng.annotations.Test;

public class TestTimeout {

    @Test(timeOut = 5000) // time in mulliseconds
    public void testThisShouldPass() throws InterruptedException {
        Thread.sleep(4000);
    }

    @Test(timeOut = 1000)
    public void testThisShouldFail() {
        while (true){
            // do nothing
        }
    }
}
```

### 分组

- 使用@Test(groups = "database")注解可以对类或者测试方法标注。
- 另外一个类或者方法可以属于多个分组。@Test(groups = {"mysql","database"})
- 在testng.xml中可以指定运行哪个分组。

### 套件

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<suite name="TestAll">
  <test name="database">
    <groups>
        <run>
            <exclude name="brokenTests" />
            <include name="db" />
        </run>
    </groups>
    <classes>
        <class name="com.yiibai.TestDatabase" />
    </classes>
  </test>
</suite>
```

### 依赖测试

- dependOnMethods  使用注解   @Test(dependsOnMethods = { "method1" })
- dependsOnGroups  使用注解   @Test(dependsOnGroups={"deploy","db"})

