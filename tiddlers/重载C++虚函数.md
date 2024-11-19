和蓝图类似需要标记UFUNCTION为BlueprintImplementableEvent或BlueprintNativeEvent

```cpp
UFUNCTION(BlueprintImplementableEvent)
UFUNCTION(BlueprintNativeEvent)
```

#### 示例：
.cpp

```cpp
UCLASS()
class AMyActor : public AActor
{
	GENERATED_BODY()
	
public:	
	// Sets default values for this actor's properties
	AMyActor();

protected:
	// Called when the game starts or when spawned
	virtual void BeginPlay() override;

public:	
	// Called every frame
	virtual void Tick(float DeltaTime) override;

	UFUNCTION(BlueprintImplementableEvent)
	void VirtualFunc();
};
```

.as

```cpp
class AASMyActor : AMyActor
{
    UFUNCTION(BlueprintOverride)
    void VirtualFunc()
    {
    }
}
```

