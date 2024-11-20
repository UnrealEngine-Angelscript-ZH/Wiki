

```cpp
class ATest : AActor
{
    UPROPERTY(DefaultComponent, RootComponent)
    USceneComponent Root;

    UFUNCTION(BlueprintOverride)
    void BeginPlay()
    {
        float F = 0.0f;
        FVector V = FVector(0.0f, 0.0f, 0.0f);

        auto X = V * F;
        // 下面这行会报错, 原因是FVector没有实现 float * FVector 运算符
        // auto y = F * V;
    }
};
```

- 暂时解决方法为 自己写下重载函数，或者修改写法（一般来讲修改写法问题不大）

- 重载操作符请查看 [绑定重载的操作符](#绑定重载的操作符)

