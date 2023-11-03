---

title: Log4cplus 日志库
subtitle: Log4cplus 日志库
date: 2020-11-22 2:00:00
author: huihut
tags:
	- C/C++  
categories: 
	- CS
	
---

## 简介

Log4cplus 是 log4j 的 C++ 实现，其接口和使用逻辑与 log4j 基本保持一致。

* log4cplus 具有线程安全、灵活、以及多粒度控制的特点
* 可以通过将日志划分优先级使其可以面向程序调试、运行、测试、和维护等全生命周期
* 可以选择将日志输出到控制台、调试器、文件、服务器
* 可以通过指定策略对日志进行定期备份

<!-- more -->

## 许可协议

Log4cplus 的每个文件是使用二级BSD许可协议（Two clause BSD license）或者 Apache license 2.0 许可协议，其中的线程池（ThreadPool.h）又是使用另外的协议。

## 重要组成

类|说明
---|---
Filter|过滤器，过滤输出消息
Layout|布局器，控制输出消息的格式
Appender|附加器，将日志输出到所附加的设备终端如控制台、调试器、文件、远程服务器等等
Logger|记录器，保存并跟踪对象日志信息变更的实体，当你需要对一个对象进行记录时，就需要生成一个logger
Hierarchy|分类器，层次化的树型结构，用于对被记录信息的分类，层次中每一个节点维护一个logger的所有信息
LogLevel|优先权，包括TRACE, DEBUG, INFO, WARNING, ERROR, FATAL

### 关系

* Hierarchy -> Logger -> Appender(Layout) -> Filter
* InternalLoggingEvent -> LogLevel

## 源码

### Filter

过滤器，用于过滤日志项，可继承Filter自定义过滤器，也可用自带的过滤器

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/log4cplus-filter.png)

* Filter
* DenyAllFilter：全部过滤
* LogLevelMatchFilter：等级过滤
* LogLevelRangeFilter：等级范围过滤
* StringMatchFilter：字符串过滤
* FunctionFilter：方法函数过滤
* NDCMatchFilter
* MDCMatchFilter

### Layout

布局器，控制输出日志消息的格式

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/log4cplus-layout.png)

* Layout
* SimpleLayout: DEBUG - Hello world
* TTCCLayout(time、thread、category、context)：[0x60004b030] INFO SlowObject \<Thread-0 loop\> - Actually doing something
* PatternLayout

#### class PatternLayout

<ul>
<li>%%a -- Abbreviated weekday name</li>
<li>%%A -- Full weekday name</li>
<li>%%b -- Abbreviated month name</li>
<li>%%B -- Full month name</li>
<li>%%c -- Standard date and time string</li>
<li>%%d -- Day of month as a decimal(1-31)</li>
<li>%%H -- Hour(0-23)</li>
<li>%%I -- Hour(1-12)</li>
<li>%%j -- Day of year as a decimal(1-366)</li>
<li>%%m -- Month as decimal(1-12)</li>
<li>%%M -- Minute as decimal(0-59)</li>
<li>%%p -- Locale's equivalent of AM or PM</li>
<li>%%q -- milliseconds as decimal(0-999) -- <b>Log4CPLUS specific</b>
<li>%%Q -- fractional milliseconds as decimal(0-999.999) -- <b>Log4CPLUS specific</b>
<li>%%S -- Second as decimal(0-59)</li>
<li>%%U -- Week of year, Sunday being first day(0-53)</li>
<li>%%w -- Weekday as a decimal(0-6, Sunday being 0)</li>
<li>%%W -- Week of year, Monday being first day(0-53)</li>
<li>%%x -- Standard date string</li>
<li>%%X -- Standard time string</li>
<li>%%y -- Year in decimal without century(0-99)</li>
<li>%%Y -- Year including century as decimal</li>
<li>%%Z -- Time zone name</li>
<li>%% -- The percent sign</li>
</ul>

```cpp
new PatternLayout(LOG4CPLUS_TEXT("[%D{%Y-%m-%d %H:%M:%S.%q}] [%t] %-5p [%M] %m%n"));
```

```bash
[2020-08-24 01:30:43.650] [14168] DEBUG [main] log test
```

### Appender

附加器，将日志输出到所附加的设备终端如控制台、调试器、文件、远程服务器等等

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/log4cplus-appender.png)

#### class Appender

```cpp
//! Asynchronous append.
bool async;
```

```cpp
log4cplus::helpers::Properties properties;
properties.setProperty(LOG4CPLUS_TEXT("AsyncAppend"), LOG4CPLUS_TEXT("true"));
```

#### class AsyncAppender

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/log4cplus-async-appender.png)


```cpp
log4cplus::helpers::Properties properties;
log4cplus::SharedAppenderPtr fileAppend(new log4cplus::RollingFileAppender(properties));
log4cplus::SharedAppenderPtr asyncAppender(new log4cplus::AsyncAppender(fileAppend, 100));
m_logger.addAppender(asyncAppender);
```

#### class RollingFileAppender

```cpp
RollingFileAppender(const log4cplus::tstring& filename,
                    long maxFileSize = 10*1024*1024,
                    int maxBackupIndex = 1,
                    bool immediateFlush = true,
                    bool createDirs = false);
RollingFileAppender(const log4cplus::helpers::Properties& properties);
```

### Logger

记录器，保存并跟踪对象日志信息变更的实体，当你需要对一个对象进行记录时，就需要生成一个logger

![](https://huihut-img.oss-cn-shenzhen.aliyuncs.com/log4cplus-logger.png)

```cpp
void log(LogLevel ll, const log4cplus::tstring& message,
        const char* file = LOG4CPLUS_CALLER_FILE (),
        int line = LOG4CPLUS_CALLER_LINE (),
        const char* function = LOG4CPLUS_CALLER_FUNCTION ()) const;
void log(spi::InternalLoggingEvent const &) const;
        
void forcedLog(LogLevel ll, const log4cplus::tstring& message,
        const char* file = LOG4CPLUS_CALLER_FILE (),
        int line = LOG4CPLUS_CALLER_LINE (),
        const char* function = LOG4CPLUS_CALLER_FUNCTION ()) const;

void forcedLog(spi::InternalLoggingEvent const &) const;
```

### Hierarchy

分类器，层次化的树型结构，用于对被记录信息的分类，层次中每一个节点维护一个logger的所有信息

```cpp
typedef std::vector<Logger> ProvisionNode;
typedef std::map<log4cplus::tstring, ProvisionNode> ProvisionNodeMap;
typedef std::map<log4cplus::tstring, Logger> LoggerMap;

ProvisionNodeMap provisionNodes;
LoggerMap loggerPtrs;
Logger root;
```

### LogLevel

```cpp
typedef int LogLevel;
```

```cpp
const LogLevel OFF_LOG_LEVEL     = 60000;
const LogLevel FATAL_LOG_LEVEL   = 50000;
const LogLevel ERROR_LOG_LEVEL   = 40000;
const LogLevel WARN_LOG_LEVEL    = 30000;
const LogLevel INFO_LOG_LEVEL    = 20000;
const LogLevel DEBUG_LOG_LEVEL   = 10000;
const LogLevel TRACE_LOG_LEVEL   = 0;
const LogLevel ALL_LOG_LEVEL     = TRACE_LOG_LEVEL;
const LogLevel NOT_SET_LOG_LEVEL = -1;
```

### Properties

属性，用于配置参数

```cpp
log4cplus::helpers::Properties properties;
properties.setProperty(LOG4CPLUS_TEXT("File"), logFile.c_str());
properties.setProperty(LOG4CPLUS_TEXT("CreateDirs"), LOG4CPLUS_TEXT("true"));
properties.setProperty(LOG4CPLUS_TEXT("ImmediateFlush"), LOG4CPLUS_TEXT("true"));
properties.setProperty(LOG4CPLUS_TEXT("MaxFileSize"), std::to_wstring(max_file_size).c_str());
properties.setProperty(LOG4CPLUS_TEXT("MaxBackupIndex"), std::to_wstring(max_file_count).c_str());
properties.setProperty(LOG4CPLUS_TEXT("AsyncAppend"), s_async.c_str());
properties.setProperty(LOG4CPLUS_TEXT("Locale"), LOG4CPLUS_TEXT("en_US.UTF-8"));

log4cplus::SharedAppenderPtr fileAppend(new log4cplus::RollingFileAppender(properties));
```

### 可变参数格式化打印日志

#### 示例

```cpp
LOG4CPLUS_INFO(logger, LOG4CPLUS_TEXT("Hello world"));
```

#### 原理

LOG4CPLUS_INFO_FMT
```cpp
#define LOG4CPLUS_INFO_FMT(logger, ...)  \
    LOG4CPLUS_MACRO_FMT_BODY (logger, INFO_LOG_LEVEL, __VA_ARGS__)
```

LOG4CPLUS_MACRO_FMT_BODY
```cpp
#define LOG4CPLUS_MACRO_FMT_BODY(logger, logLevel, ...)                 \
    LOG4CPLUS_SUPPRESS_DOWHILE_WARNING()                                \
    do {                                                                \
        log4cplus::Logger const & _l                                    \
            = log4cplus::detail::macros_get_logger (logger);            \
        if (LOG4CPLUS_MACRO_LOGLEVEL_PRED (                             \
                _l.isEnabledFor (log4cplus::logLevel), logLevel)) {     \
            LOG4CPLUS_MACRO_INSTANTIATE_SNPRINTF_BUF (_snpbuf);         \
            log4cplus::tchar const * _logEvent                          \
                = _snpbuf.print (__VA_ARGS__);                          \
            log4cplus::detail::macro_forced_log (_l,                    \
                log4cplus::logLevel, _logEvent,                         \
                LOG4CPLUS_MACRO_FILE (), __LINE__,                      \
                LOG4CPLUS_MACRO_FUNCTION ());                           \
        }                                                               \
    } while(0)                                                          \
    LOG4CPLUS_RESTORE_DOWHILE_WARNING()
```

LOG4CPLUS_MACRO_INSTANTIATE_SNPRINTF_BUF
```cpp
#  define LOG4CPLUS_MACRO_INSTANTIATE_SNPRINTF_BUF(var)     \
    log4cplus::helpers::snprintf_buf & var                  \
        = log4cplus::detail::get_macro_body_snprintf_buf ()
```

snprintf_buf::print
```cpp
tchar const *
snprintf_buf::print (tchar const * fmt, ...)
{
    assert (fmt);

    tchar const * str = nullptr;
    int ret = 0;
    std::va_list args;

    do
    {
        va_start (args, fmt);
        ret = print_va_list (str, fmt, args);
        va_end (args);
    }
    while (ret == -1);

    return str;
}
```

snprintf_buf::print_va_list
```cpp
int
snprintf_buf::print_va_list (tchar const * & str, tchar const * fmt,
    std::va_list args)
{
    int printed;
    std::size_t const fmt_len = std::char_traits<tchar>::length (fmt);
    std::size_t buf_size = buf.size ();
    std::size_t const output_estimate = fmt_len + fmt_len / 2 + 1;
    if (output_estimate > buf_size)
        buf.resize (buf_size = output_estimate);
//...

    printed = vsntprintf (&buf[0], buf_size - 1, fmt, args);
    if (printed == -1)
    {
#if defined (EILSEQ)
        if (errno == EILSEQ)
        {
            LogLog::getLogLog ()->error (
                LOG4CPLUS_TEXT ("Character conversion error when printing"));
            // Return zero to terminate the outer loop in
            // snprintf_buf::print().
            return 0;
        }
#endif

        buf_size *= 2;
        buf.resize (buf_size);
    }
    else if (printed >= static_cast<int>(buf_size - 1))
    {
        buf_size = printed + 2;
        buf.resize (buf_size);
        printed = -1;
    }
    else
        buf[printed] = 0;

#endif

    str = &buf[0];
    return printed;
}
```

其实就是使用了C语音的可变参数宏实现参数可变

* va_start
* va_arg
* va_end

vadefs.h
```cpp
#elif defined _M_IX86 && !defined _M_HYBRID_X86_ARM64

    #define _INTSIZEOF(n)          ((sizeof(n) + sizeof(int) - 1) & ~(sizeof(int) - 1))

    #define __crt_va_start_a(ap, v) ((void)(ap = (va_list)_ADDRESSOF(v) + _INTSIZEOF(v)))
    #define __crt_va_arg(ap, t)     (*(t*)((ap += _INTSIZEOF(t)) - _INTSIZEOF(t)))
    #define __crt_va_end(ap)        ((void)(ap = (va_list)0))
```

使用了 _vswprintf_p（sprintf） 做格式化

```cpp
int _vswprintf_p(
   wchar_t *buffer,
   size_t count,
   const wchar_t *format,
   va_list argptr
);
int sprintf ( 
    char * str, 
    const char * format, 
    ... );
```

C语言的函数是从右往左压入栈的，比如一下内存分布

```c
void print_args(int count, ...) {
	int i, value;
	va_list arg_ptr;
	va_start(arg_ptr, count);
	for(i=0; i<count; i++) {
		value = va_arg(arg_ptr,int);
		printf("position %d = %d\n", i+1, value);
	}
	va_end(arg_ptr);
}
int main(int argc, char* argv[]) {
	print_args(5,1,2,3,4,5);
	return 0; 
}
```

```
| 5 |   // 高位地址
| 4 |
| 3 |
| 2 |
| 1 |   // arg_ptr
| 5 |   // count
```

### 日志打印流程

#### 流程

* Logger::log
* LoggerImpl::log：等级
* LoggerImpl::forcedLog：获取 InternalLoggingEvent
* LoggerImpl::callAppenders：遍历父子附加器
* AppenderAttachableImpl::appendLoopOnAppenders：遍历附加器列表
* Appender::doAppend：同步异步
* Appender::syncDoAppend：检查阈值、过滤器、锁
* FileAppenderBase::append：文件打开、锁定（进程同步）、格式化附加、刷新
* SimpleLayout::formatAndAppend：附加

#### 原理

Logger::log
```cpp
void
Logger::log (LogLevel ll, const log4cplus::tstring& message, const char* file,
    int line, const char* function) const
{
    value->log (ll, message, file, line, function ? function : "");
}
```

LoggerImpl::log：等级
```cpp
void
LoggerImpl::log(LogLevel loglevel,
                const log4cplus::tstring& message,
                const char* file,
                int line,
                const char* function)
{
    if(isEnabledFor(loglevel)) {
        forcedLog(loglevel, message, file, line, function ? function : "");
    }
}
```

LoggerImpl::forcedLog：获取 InternalLoggingEvent
```cpp
void
LoggerImpl::forcedLog(LogLevel loglevel,
                      const log4cplus::tstring& message,
                      const char* file,
                      int line,
                      const char* function)
{
    spi::InternalLoggingEvent & ev = internal::get_ptd ()->forced_log_ev;
    assert (function);
    ev.setLoggingEvent (this->getName(), loglevel, message, file, line,
        function);
    callAppenders(ev);
}
```

LoggerImpl::callAppenders：遍历父子附加器
```cpp
void
LoggerImpl::callAppenders(const InternalLoggingEvent& event)
{
    int writes = 0;
    for(const LoggerImpl* c = this; c != nullptr; c=c->parent.get()) {
        writes += c->appendLoopOnAppenders(event);
        if(!c->additive) {
            break;
        }
    }

    // No appenders in hierarchy, warn user only once.
    if(!hierarchy.emittedNoAppenderWarning && writes == 0) {
        helpers::getLogLog().error(
            LOG4CPLUS_TEXT("No appenders could be found for logger (")
            + getName()
            + LOG4CPLUS_TEXT(")."));
        helpers::getLogLog().error(
            LOG4CPLUS_TEXT("Please initialize the log4cplus system properly."));
        hierarchy.emittedNoAppenderWarning = true;
    }
}
```

AppenderAttachableImpl::appendLoopOnAppenders：遍历附加器列表
```cpp
int
AppenderAttachableImpl::appendLoopOnAppenders(const spi::InternalLoggingEvent& event) const
{
    int count = 0;
    thread::MutexGuard guard (appender_list_mutex);
    for (auto & appender : appenderList)
    {
        ++count;
        appender->doAppend(event);
    }
    return count;
}
```

Appender::doAppend：同步异步
```cpp
void
Appender::doAppend(const log4cplus::spi::InternalLoggingEvent& event)
{
#if ! defined (LOG4CPLUS_SINGLE_THREADED)
    if (async)
    {
        event.gatherThreadSpecificData ();
        std::atomic_fetch_add_explicit (&in_flight, std::size_t (1),
            std::memory_order_relaxed);
        try
        {
            enqueueAsyncDoAppend (SharedAppenderPtr (this), event);
        }
        catch (...)
        {
            subtract_in_flight ();
            throw;
        }
    }
    else
#endif
        syncDoAppend (event);
}
```

Appender::syncDoAppend：检查阈值、过滤器、锁
```cpp
void
Appender::syncDoAppend(const log4cplus::spi::InternalLoggingEvent& event)
{
    thread::MutexGuard guard (access_mutex);
    if(closed) {
        helpers::getLogLog().error(
            LOG4CPLUS_TEXT("Attempted to append to closed appender named [")
            + name
            + LOG4CPLUS_TEXT("]."));
        return;
    }
    // Check appender's threshold logging level.
    if (! isAsSevereAsThreshold(event.getLogLevel()))
        return;
    // Evaluate filters attached to this appender.
    if (checkFilter(filter.get(), event) == spi::DENY)
        return;
    // Lock system wide lock.
    helpers::LockFileGuard lfguard;
    if (useLockFile && lockFile.get ())
    {
        try
        {
            lfguard.attach_and_lock (*lockFile);
        }
        catch (std::runtime_error const &)
        {
            return;
        }
    }
    // Finally append given event.
    append(event);
}
```

FileAppenderBase::append：文件打开、锁定（进程同步）、格式化附加、刷新
```cpp
void
FileAppenderBase::append(const spi::InternalLoggingEvent& event)
{
    if(!out.good()) {
        if(!reopen()) {
            getErrorHandler()->error(  LOG4CPLUS_TEXT("file is not open: ")
                                     + filename);
            return;
        }
        // Resets the error handler to make it
        // ready to handle a future append error.
        else
            getErrorHandler()->reset();
    }
    if (useLockFile)
        out.seekp (0, std::ios_base::end);

    layout->formatAndAppend(out, event);
    
    if(immediateFlush || useLockFile)
        out.flush();
}
```

SimpleLayout::formatAndAppend：附加
```cpp
void
SimpleLayout::formatAndAppend(log4cplus::tostream& output,
                              const log4cplus::spi::InternalLoggingEvent& event)
{
    output << llmCache.toString(event.getLogLevel())
           << LOG4CPLUS_TEXT(" - ")
           << event.getMessage()
           << LOG4CPLUS_TEXT("\n");
}
```

## 源码编译 

### 默认编译

编译成动态库，带有很多例子项目

### 去除例子（只编译库）

#### 修改记录

<https://github.com/huihut/log4cplus/commit/5d7e51ac6a43e1eaa623e5d2272651458edf85c6>

#### 修改内容

./CMakeLists.txt

```cmake
option(LOG4CPLUS_BUILD_TESTING "Build the test suite." OFF)
option(LOG4CPLUS_BUILD_LOGGINGSERVER "Build the logging server." OFF)
...
option(WITH_UNIT_TESTS "Enable unit tests" OFF)
```

### 编译成静态库

#### 修改记录

<https://github.com/huihut/log4cplus/commit/4e02f06a5549afca1183801a1424eee221a36bb5>

#### 修改内容

./src/CMakeLists.txt

```cmake
add_compile_definitions (LOG4CPLUS_STATIC)
...
add_library (${log4cplus} STATIC ${log4cplus_sources})
```

## 使用

### 将日志输出到控制台

```cpp
#include <log4cplus/log4cplus.h>
 
int main()
{
    //用Initializer类进行初始化
    log4cplus::Initializer initializer;
 
    //第1步：创建ConsoleAppender
    log4cplus::SharedAppenderPtr appender(new log4cplus::ConsoleAppender());
 
    //第2步：设置Appender的名称和输出格式（SimpleLayout）
    appender->setName(LOG4CPLUS_TEXT("console"));
    appender->setLayout(std::unique_ptr<log4cplus::Layout>(new log4cplus::SimpleLayout));
 
    //第3步：获得一个Logger实例，并设置其日志输出等级阈值
    log4cplus::Logger logger = log4cplus::Logger::getInstance(LOG4CPLUS_TEXT ("test"));
    logger.setLogLevel(log4cplus::INFO_LOG_LEVEL);
 
    //第4步：为Logger实例添加ConsoleAppender
    logger.addAppender(appender);
 
    //第5步：使用宏将日志输出
    LOG4CPLUS_INFO(logger, LOG4CPLUS_TEXT("Hello world"));
 
    return 0;
}
```

### 将日志输出到控制台并写入文件

```cpp
#include <log4cplus/log4cplus.h>
 
int main()
{
    //用Initializer类进行初始化
    log4cplus::Initializer initializer;
 
    //第1步：创建ConsoleAppender和FileAppender(参数app表示内容追加到文件)
    log4cplus::SharedAppenderPtr consoleAppender(new log4cplus::ConsoleAppender);
    log4cplus::SharedAppenderPtr fileAppender(new log4cplus::FileAppender(
                                                  LOG4CPLUS_TEXT("log.txt"),
                                                  std::ios_base::app));
 
    //第2步：设置Appender的名称和输出格式
    //ConsoleAppender使用SimpleLayout
    //FileAppender使用PatternLayout
    consoleAppender->setName(LOG4CPLUS_TEXT("console"));
    consoleAppender->setLayout(std::unique_ptr<log4cplus::Layout>(new log4cplus::SimpleLayout()));
    
    fileAppender->setName(LOG4CPLUS_TEXT("file"));
    log4cplus::tstring pattern = LOG4CPLUS_TEXT("%D{%m/%d/%y %H:%M:%S,%Q} [%t] %-5p %c - %m [%l]%n");
    fileAppender->setLayout(std::unique_ptr<log4cplus::Layout>(new log4cplus::PatternLayout(pattern)));
 
    //第3步：获得一个Logger实例，并设置其日志输出等级阈值
    log4cplus::Logger logger = log4cplus::Logger::getInstance(LOG4CPLUS_TEXT ("test"));
    logger.setLogLevel(log4cplus::INFO_LOG_LEVEL);
 
    //第4步：为Logger实例添加ConsoleAppender和FileAppender
    logger.addAppender(consoleAppender);
    logger.addAppender(fileAppender);
 
    //第5步：使用宏将日志输出
    LOG4CPLUS_INFO(logger, LOG4CPLUS_TEXT("Hello world"));
 
    return 0;
}
```

# log4cplusplus

<https://github.com/huihut/log4cplusplus>

## 简介

log4cplusplus 是 log4cplus 的包装库

* 线程安全
* 支持异步
* 支持中文路径和内容
* 支持输出到文件、控制台、调试器
* 支持格式化打印

## 接口

```cpp
enum Log4CPlusPlusLevel
{
	LogDebugLevel = 10000,
	LogInfoLevel = 20000,
	LogWarnLevel = 30000,
	LogErrorLevel = 40000
};

class Log4CPlusPlus
{
public:
	Log4CPlusPlus() {}
	virtual ~Log4CPlusPlus() {}

public:
	virtual void Release() = 0;

	virtual void AddFileAppender(
		const wchar_t *file_path = DEFALT_LOG_FILE_PATH,
		const wchar_t *file_name = DEFALT_LOG_FILE_NAME,
		unsigned long max_file_size = DEFALT_MAX_FILE_SIZE,
		unsigned long max_file_count = DEFALT_MAX_FILE_COUNT,
		bool is_async = DEFALT_IS_ASYNC
	) = 0;

	virtual void EnableDebuggerAppender(bool enable) = 0;
	virtual void EnableConsoleAppender(bool enable) = 0;

	virtual const wchar_t * GetLogPath() = 0;

	virtual void WriteLog(
		Log4CPlusPlusLevel logLevel,
		const char* file,
		int line,
		const char* function,
		const wchar_t *format, ...) = 0;

};
```

## 使用

```cpp
#include "log4cplusplus.h"

int main(int argc, char *argv[])
{
	log4cplus::Log4CPlusPlus* log = CreateLog4CPlusPlus();
	if (log)
	{
		log->AddFileAppender();
		log->EnableDebuggerAppender(true);
		log->EnableConsoleAppender(true);

		LOG4CPLUSPLUS_DEBUG(log, L"log test");
		LOG4CPLUSPLUS_INFO(log, L"log test %s", L"info");
		LOG4CPLUSPLUS_WARN(log, L"log test %s %d", L"warn", 123);
		LOG4CPLUSPLUS_ERROR(log, L"log test %f", 3.14);
		LOG4CPLUSPLUS_ERROR(log, L"中文日志测试!!");

		log->Release();
	}
	return 0;
}
```