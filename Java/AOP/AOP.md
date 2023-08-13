**一、AOP**

AOP(Aspect-Oriented Programming)面向切面编程，是OOP(Object-Oriented Programming)面向对象编程的补充 。OOP是以类为单元，OOP是以切面为单元。

**二、概念及用法**

**2.1、Aspect(切面)**

横切逻辑类，包含了切点的定义，增强的业务逻辑。由Spring AOP负责将增强逻辑织入到切点中。

```java
@Component
@Aspect
public class ParamValidateAspect { 
    //TODO
}
```

**2.2、Join point(连接点)**

程序运行中的一些特定时间点，例如在方法执行时等。

**2.3、Point cut(切点)**

满足特定条件的连接点。pointcut用AspectJ pointcut expression language来提供一套规则去匹配joinpoint,来给这些满足规则的连接点 添加Advice。切点由两部分组成：

①一个方法签名，包括方法名和相关参数；

②一个pointcut表达式，用来指定具体的连接点(需要拦截的方法)；

```java
@Component
@Aspect
public class ParamValidateAspect {
    //切点 
    @Pointcut("@annotation(com.stone.tms.refector.annotation.ParamValidate)")
    public void pointcut() {
    }
}

```

**2.4、Advice(增强 )**

advice的类型如下：

1. @Before：前置增强

   增强方法会在目标方法调用之前调用。它不能阻止匹配连接点的方法的执行，除非发生了异常。

   不能阻止业务模块的执行，除非通知方法抛出异常。

2. @Around：环绕增强

   增强方法会在目标方法调用前后调用，可以决定是否执行匹配的方法 。通过proceed()方法去执行目标方法。

3. @After：后置增强

   增强 方法会在目标方法返回或者异常后调用。无论是正常退出还是发生了异常，都会执行。

4. @AfterReturning：后置增强

   增强方法会在目标方法正常返回后调用。

5. @AfterThrowing：后置增强

   增强方法会在目标方法异常后调用。

```java
@Component
@Aspect
public class ParamValidateAspect {
    @Pointcut("@annotation(com.stone.tms.refector.annotation.ParamValidate)")
    public void pointcut() {
    }

    /**
     * 前置增强
     *
     * @param point 切点
     */
    @Before("pointcut()")
    public void before(JoinPoint point) {
        //TODO
    }

    /**
     * 环绕增强
     *
     * @param point 切点
     * @return Object
     * @throws Throwable Throwable
     */
    @Around("pointcut()")
    public Object around1(ProceedingJoinPoint point) throws Throwable {
        //前置增强
        Object obj = point.proceed();
        //后置增强
        return obj;
    }

    /**
     * 环绕增强
     *
     * @param point 切点
     * @return Object
     * @throws Throwable Throwable
     */
    @Around("execution(* com.stone.control.*.*(..)")
    public Object around2(ProceedingJoinPoint point) throws Throwable {
        //前置增强
        Object obj = point.proceed();
        //后置增强
        return obj;
    }

    /**
     * 后置这个群
     *
     * @param point 切点
     */
    @After("pointcut()")
    public void after(JoinPoint point) {
        //TODO
    }

    /**
     * 后置正常返回增强
     *
     * @param point 切点
     */
    @AfterReturning("pointcut()")
    public void afterReturning(JoinPoint point) {
        //TODO
    }

    /**
     * 后置异常增强
     *
     * @param point 切点
     */
    @AfterThrowing("pointcut()")
    public void afterThrowing(JoinPoint point) {
        //TODO
    }
}

```

**2.5、Target(目标对象)**

需要织入advice的对象称之为目标对象。目标对象也 称为advised object。Spring AOP使用代理的方式实现增强，因此adviced object是一个代理对象(proxy object)，adviced object不是原来的类，而是织入advice后所产生的代理类。

**2.6、AOP Proxy**

一个类被AOP织入advice产生了一个代理类，代理类融合了 原类和增强逻辑的类。Spring AOP中的 代理类要么是JDK动态代理，要么就是CGLIB代理，取决是是否有接口，有接口都是JDK代理。

**2.7、Weaving(织入)**

创建代理类，并将切面(Aspect)中的增强逻辑织入代理类的过程。

**2.8、引入 (Introduction)**

引入允许我们向现有的类添加新的方法 或者属性。



