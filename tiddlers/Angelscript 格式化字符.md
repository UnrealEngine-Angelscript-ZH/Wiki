

## f-string 格式化

Angelscript的格式化字符串参考了Python f-string
![](https://chinese.freecodecamp.org/news/content/images/size/w2000/2023/01/py-fstrings.png){ width=300 height=200 }



具体案例如下：

```cpp
// Format Strings begin with f" and can hold expressions
// inside braces to replace within the string.
Print(f"Called from actor {GetName()} at location {ActorLocation}");

// Adding a = at the end of the expression will print the expression first
// For example:
Print(f"{DeltaSeconds =}");
// This prints:
//   DeltaSeconds = 0.01

// Format specifiers can be added following similar syntax to python's f-strings:
Print(f"Three Decimals: {ActorLocation.Z :.3}"); // Format float at three decimals of precision

Print(f"Extended to 10 digits with leading zeroes: {400 :010d}"); // 0000000400
Print(f"Hexadecimal: {20 :#x}"); // 0x14
Print(f"Binary: {1574 :b}"); // 11000100110
Print(f"Binary 32 Bits: {1574 :#032b}"); // 0b00000000000000000000011000100110

// Alignment works too
Print(f"Aligned: {GetName() :>40}"); // Adds spaces to the start of GetName() so it is 40 characters
Print(f"Aligned: {GetName() :_<40}"); // Adds underscores to the end of GetName() so it is 40 characters

// You can combine the equals with a format specifier
Print(f"{DeltaSeconds =:.0}");
// This prints:
//   DeltaSeconds = 0

// Enums by default print a full debug string
Print(f"{ESlateVisibility::Collapsed}"); // "ESlateVisibility::Collapsed (1)"
// But the 'n' specifier prints only the name of the value:
Print(f"{ESlateVisibility::Collapsed :n}"); // "Collapsed"
```

**实现原理：**

`FAngelscriptPreprocessor::GenerateFormatString`对f前缀的字符串进行了处理，将其替换为FString的一些列函数调用

**使用建议：** 
不建议在性能敏感的地方调用，f-string展开会频繁申请销毁字符串，性能不友好

## FString::Format

```cpp
auto s = FString::Format("Hello {0}, {1} ,{2}", "abc", 1, 0.1);
```

**绑定地址：**

`Engine/Plugins/Angelscript/Source/AngelscriptCode/Private/Binds/Bind_FString.cpp`最多只能使用五个可变参数，默认只绑定了5个。有需要可自行绑定

## 相关参考

[PEP 498 – Literal String Interpolation | peps.python.org](https://peps.python.org/pep-0498/) Python f-string



