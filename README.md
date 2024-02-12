Sample-CppWinRT-DispatcherQueue-build-regression
---

This is a repository that contains a sample project that shows a build regression after updating [CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT) from 2.0.221104.6, which build this repository, to 2.0.221117.1, which no longer builds the repository, with the following error:

```
1>------ Build started: Project: TestBuildCppWinRT, Configuration: Debug x64 ------
1>pch.cpp
1>C:\prog\janeasystems\TestBuildCppWinRT\x64\Debug\Generated Files\winrt\impl\Windows.Graphics.Capture.0.h(27,12): error C3312: no callable 'await_resume' function found for type 'winrt::Windows::System::DispatcherQueue'
1>(compiling source file 'pch.cpp')
1>Done building project "TestBuildCppWinRT.vcxproj" -- FAILED.
========== Build: 0 succeeded, 1 failed, 0 up-to-date, 0 skipped ==========
```

This sample is updated all the way up until the latest CppWinRt version at this time, 2.0.240111.5, which still doesn't build this code:
```cpp
        inline winrt::fire_and_forget ShutdownAndThenPostQuitMessage(winrt::Windows::System::DispatcherQueueController const& controller, int exitCode)
        {
            auto queue = controller.DispatcherQueue();
            co_await controller.ShutdownQueueAsync();
            co_await queue; // This line no longer builds.
            PostQuitMessage(exitCode);
            co_return;
        }
```

The code is taken from https://www.nuget.org/packages/robmikh.common/0.0.22-beta package, from this file: https://github.com/robmikh/robmikh.common/blob/aaa31a1bcd4fac19ce4c544d2cfd415612add7f0/robmikh.common/include/robmikh.common/dispatcherqueue.desktop.interop.h

CppWinRT 2.0.221104.6 builds this code. This repo can be build at this commit: https://github.com/jaimecbernardo/Sample-CppWinRT-DispatcherQueue-build-regression/commit/c1a7221f9eeb6dab8a16fa6b812d97eb5132902c

The next version available (2.0.221117.1) https://github.com/jaimecbernardo/Sample-CppWinRT-DispatcherQueue-build-regression/commit/7742b755e2c4c6dd9293c85bdcc2cf8dfd543c8b , and the lastest (2.0.240111.5) https://github.com/jaimecbernardo/Sample-CppWinRT-DispatcherQueue-build-regression/commit/c1a7221f9eeb6dab8a16fa6b812d97eb5132902c , no longer allow build this repository.
