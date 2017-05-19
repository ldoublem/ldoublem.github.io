---
title: 抽象类（abstract class）和接口（Interface）的理解
date: 2016-05-18 11:15
tags: java
---
```
public abstract class TestAbstract extends AppCompatActivity {

    public abstract String OnCallTestAbstract();

    public void CallInf(TestInterface t) {
        t.OnCallTestInterface("------回调接口，返回子类现实抽象类的值:"
        +OnCallTestAbstract());
    }


}
```
```
public interface TestInterface {

    void OnCallTestInterface(String s);

}
```
```
public class Test extends TestAbstract implements TestInterface {
    @Override
    public String OnCallTestAbstract() {
        return "hello world";
    }

    @Override
    public void OnCallTestInterface(String s) {
        System.out.println(s);
    }

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        CallInf(this);
    }
}
```
运行后输出*------回调接口，返回子类现实抽象类的值:hello world*

上面简单的列子是Test继承了TestAbstract，现实了接口TestInterface。
1，子类实现了父类的抽象方法*OnCallTestAbstract*，理解是不同的子类可以做不同的操作。即父类发送指令给子类去完成任务。
2，子类实现回调接口*TestInterface*，理解不同的子类需要得到父类相同的操作，来保证一致性。即子类发送给父类指令，父类完成后告诉子类。当然不同的类可以通过接口来做一些回调。