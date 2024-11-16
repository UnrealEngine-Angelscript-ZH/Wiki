

- 抓取作用域范围内的渲染事件
- 需要开启Renderdoc
- 没自动打开在`[Project]\Saved\RenderDocCaptures`可以找到

```cpp
#include "RenderCaptureInterface.h"
```

游戏线程

```cpp
RenderCaptureInterface::FScopedCapture RenderCapture(Toogle, TEXT("[EventName]"), *FDateTime::Now().ToString(TEXT("[EventName]-%Y%m%d%H%M%S")));
```

渲染线程

```cpp
RenderCaptureInterface::FScopedCapture RenderCapture(Toogle, RHICommandList，TEXT("[EventName]"), *FDateTime::Now().ToString(TEXT("[EventName]-%Y%m%d%H%M%S")));
```

UE5 新增:可以使用RDG
