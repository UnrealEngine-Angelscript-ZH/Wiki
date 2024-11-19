开发过程中不单单可以通过IDE添加断点，还可以内置断点逻辑在代码中，方便定位问题

通过代码中加入UE_DEBUG_BREAK，用CVar控制，可以自动在固定位置停止

```cpp
#if UE_BUILD_SHIPPING
#define UE_DEBUG_BREAK() ((void)0)
#else
#define UE_DEBUG_BREAK() ((void)(FPlatformMisc::IsDebuggerPresent() && ([] () { UE_DEBUG_BREAK_IMPL(); } (), 1)))
#endif
```

UE中使用的地方，可以用来调试RDG

```cpp
int32 GRDGBreakpoint = 0;
FAutoConsoleVariableRef CVarRDGBreakpoint(
	TEXT("r.RDG.Breakpoint"),
	GRDGBreakpoint,
	TEXT("Breakpoint in debugger when certain conditions are met.\n")
	TEXT(" 0: off (default);\n")
	TEXT(" 1: On an RDG warning;\n")
	TEXT(" 2: When a graph / pass matching the debug filters compiles;\n")
	TEXT(" 3: When a graph / pass matching the debug filters executes;\n")
	TEXT(" 4: When a graph / pass / resource matching the debug filters is created or destroyed;\n"),
	ECVF_RenderThreadSafe);
```





相关参考

[C/C++调试技巧-debugbreak (aicdg.com)](http://aicdg.com/debugbreak/)

[__debugbreak | Microsoft Learn](https://learn.microsoft.com/en-us/cpp/intrinsics/debugbreak?view=msvc-170)