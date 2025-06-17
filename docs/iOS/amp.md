# AMP

## monitor point

### Crashes & Exception

#### Exception by NSException

`func NSGetUncaughtExceptionHandler() -> ((NSException) -> Void)?`

#### Exception by C/C++

```c
#include <exception>
unexpected_handler set_unexpected (unexpected_handler f) throw();
terminate_handler set_terminate (terminate_handler f) noexcept;
```

#### mach-o exception

-[Refence](https://github.com/kstenerud/KSCrash/blob/master/Sources/KSCrashRecording/Monitors/KSCrashMonitor_MachException.c)
- Key API
  - `task_set_exception_ports`
  - `task_get_exception_ports`

### Signals

```C
static const int g_fatalSignals[] = {
    SIGABRT, SIGBUS, SIGFPE, SIGILL, SIGPIPE, SIGSEGV, SIGSYS, SIGTRAP, SIGTERM,
};
```


### Deadlock monitor

Use a new thread to monitor the main thread by install a event callback to main event looper

```oc
- (void)watchdogPulse
{
    __block id blockSelf = self;
    self.awaitingResponse = YES;
    dispatch_async(dispatch_get_main_queue(), ^{
        [blockSelf watchdogAnswer];
    });
}

- (void)watchdogAnswer
{
    self.awaitingResponse = NO;
}
```

- [Reference](https://github.com/kstenerud/KSCrash/blob/master/Sources/KSCrashRecording/Monitors/KSCrashMonitor_Deadlock.m)



## Get Stack information by thread

### Current thread

```swift
Thread.callStackSymbols()
```

