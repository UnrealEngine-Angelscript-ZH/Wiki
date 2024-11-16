

跟 Delegate 一起用
--------------

```cpp
FTimerDelegate TimerCallback;
TimerCallback.BindLambda([]
    {
        // callback;
    });

FTimerHandle Handle;
GetWorld()->GetTimerManager().SetTimer(Handle, TimerCallback, 5.0f, false);
```

UI 回调事件
-------

```cpp
SNew(SButton).OnClicked_Lambda([&]()
    {
     GEngine->AddOnScreenDebugMessage(-1, 5.f, FColor::Red, TEXT("Button Clicked!"));
     return FReply::Handled();
    })
```

跨线程执行
-----

```cpp
FFunctionGraphTask::CreateAndDispatchWhenReady([=]()
    {
        // game thread code
    }
    , TStatId(), nullptr, ENamedThreads::GameThread);
```

批量执行
----

```cpp
void USkeletalMeshComponent::SetAllBodiesBelowSimulatePhysics( const FName& InBoneName, bool bNewSimulate, bool bIncludeSelf )
{
	int32 NumBodiesFound = ForEachBodyBelow(InBoneName, bIncludeSelf, /*bSkipCustomPhysicsType=*/ false, [bNewSimulate](FBodyInstance* BI)
        {
            BI->SetInstanceSimulatePhysics(bNewSimulate);
        });
...
}
```

数组遍历删除
------

```cpp
ReceivedParticles.RemoveAll([](UParticleSystemComponent* Particle)
    {
        return !::IsValid(Particle);
    });
```

