有些情况下发现vscode提示会和应该返回的类型不匹配

比如：

```cpp
auto D = FDateTime::Now() - FDateTime::Today() // opSub(FDateTime); 
```

这里auto会被提示为FTimespan

原因是这里有多个操作符重载，vscode没能选择正确的那个，实际影响不大，可以改变下写法