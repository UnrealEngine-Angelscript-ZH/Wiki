**问题1:** Angelscript种继承UStateTreeEvaluatorBlueprintBase后，状态机编辑器的下拉列表中不显示这个class


StateTreeNodeClassCache.cpp:L245 is:

```cpp
if (TestClass->HasAnyClassFlags(CLASS_Native) && TestClass->IsChildOf(Class))
{
    RootClass->ClassData.Add(MakeShareable(new FStateTreeNodeClassData(TestClass)));
    ClassNameToRootIndex.Add(TestClass->GetName(), RootClass.GetIndex());
}
```

Should be:

```cpp
// AS FIX: remove CLASS_Native filter so that AS classes can be used
if (TestClass->IsChildOf(Class))
// END AS FIX
{
    RootClass->ClassData.Add(MakeShareable(new FStateTreeNodeClassData(TestClass)));
    ClassNameToRootIndex.Add(TestClass->GetName(), RootClass.GetIndex());
}
```

or：

Normally we replace checks for `CLASS_Native` with `Class->HasAnyClassFlags(CLASS_Native) || Class->bIsScriptClass` so it checks for both but doesn't let through BPs like it did before

**问题2：** 这样做之后每次修改代码下拉列表中都会增加一个相同名字的类
可能的解决：You probably just need to check that the class doesn't have `CLASS_NewerVersionExists`

最终解决：

```cpp
if ((TestClass->HasAnyClassFlags(CLASS_Native) || TestClass->bIsScriptClass) && TestClass->IsChildOf(Class) && !TestClass->HasAnyClassFlags(CLASS_NewerVersionExists))
```



#### 相关参考

discord 中的讨论 https://discord.com/channels/551756549962465299/1310546805145997313

[UE5 StateTree についてのメモ #ue5 - Qiita](https://qiita.com/unknown_ds/items/84608d1385be91ac2cc1)

[【UE5】StateTreeをC++で実装しよう！ - SPARKCREATIVE Tech Blog (spark-creative.co.jp)](https://tech.spark-creative.co.jp/entry/2024/01/26/152142)

[UE5 StateTreeで状態管理｜株式会社ヒストリア (historia.co.jp)](https://historia.co.jp/archives/34074/)