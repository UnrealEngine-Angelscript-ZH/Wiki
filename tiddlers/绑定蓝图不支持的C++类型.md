对于被`BlueprintCallable`标记的UFUNCTION和UPROPERTY，会进行自动绑定，对于蓝图不支持类型的绑定示例如下：

#### .h

```cpp
UCLASS()
class SCRIPTPLUGINCOMPARE_API ATestActor : public AActor, public ITestInterface
{
	GENERATED_BODY()
public:

	UFUNCTION()
	void SetInt8ValueFunction(int8 InInt8Value);

	UFUNCTION()
	int8 GetInt8ValueFunction() const;

	UFUNCTION()
	void SetInt16ValueFunction(int16 InInt16Value);

	UFUNCTION()
	int16 GetInt16ValueFunction() const;

	UFUNCTION()
	void SetUInt16ValueFunction(uint16 InUInt16Value);

	UFUNCTION()
	uint16 GetUInt16ValueFunction() const;

	UFUNCTION()
	void SetUInt32ValueFunction(uint32 InUInt32Value);

	UFUNCTION()
	uint32 GetUInt32ValueFunction() const;

	UFUNCTION()
	void SetUInt64ValueFunction(uint64 InUInt64Value);

public:
	UPROPERTY()
	int8 Int8Value;

	UPROPERTY()
	int16 Int16Value;

	UPROPERTY()
	uint16 UInt16Value;

	UPROPERTY()
	uint32 UInt32Value;

	UPROPERTY()
	uint64 UInt64Value;
};

```

#### .cpp

```cpp
#include "AngelscriptBinds.h"

AS_FORCE_LINK const FAngelscriptBinds::FBind Bind_ATestActor(FAngelscriptBinds::EOrder::Late, []
{
	auto AActor_ = FAngelscriptBinds::ExistingClass("ATestActor");

	AActor_.Property("int8 Int8Value", &ATestActor::Int8Value);
	AActor_.Property("int16 Int16Value", &ATestActor::Int16Value);
	AActor_.Property("uint16 UInt16Value", &ATestActor::UInt16Value);
	AActor_.Property("uint32 UInt32Value", &ATestActor::UInt32Value);
	AActor_.Property("uint64 UInt64Value", &ATestActor::UInt64Value);

	AActor_.Method("void SetInt8ValueFunction(int8 InInt8Value)", METHOD_TRIVIAL(ATestActor, SetInt8ValueFunction));
	AActor_.Method("int8 GetInt8ValueFunction() const", METHOD_TRIVIAL(ATestActor, GetInt8ValueFunction));
	AActor_.Method("void SetInt16ValueFunction(int16 InInt16Value)", METHOD_TRIVIAL(ATestActor, SetInt16ValueFunction));
	AActor_.Method("int16 GetInt16ValueFunction() const", METHOD_TRIVIAL(ATestActor, GetInt16ValueFunction));
	AActor_.Method("void SetUInt16ValueFunction(uint16 InUInt16Value)", METHOD_TRIVIAL(ATestActor, SetUInt16ValueFunction));
	AActor_.Method("uint16 GetUInt16ValueFunction() const", METHOD_TRIVIAL(ATestActor, GetUInt16ValueFunction));
	AActor_.Method("void SetUInt32ValueFunction(uint32 InUInt32Value)", METHOD_TRIVIAL(ATestActor, SetUInt32ValueFunction));
	AActor_.Method("uint32 GetUInt32ValueFunction() const", METHOD_TRIVIAL(ATestActor, GetUInt32ValueFunction));
	AActor_.Method("void SetUInt64ValueFunction(uint64 InUInt64Value)", METHOD_TRIVIAL(ATestActor, SetUInt64ValueFunction));
	AActor_.Method("uint64 GetUInt64ValueFunction() const", METHOD_TRIVIAL(ATestActor, GetUInt64ValueFunction));
});
```

