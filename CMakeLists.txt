cmake_minimum_required(VERSION 3.23)
project(async_profiler_upstream)

set(CMAKE_CXX_STANDARD 17)

include_directories(src)
include_directories(src/fdtransfer)
include_directories(src/jattach)
include_directories(src/helper)
include_directories(src/res)
find_package(JNI)
find_package(folly REQUIRED)
find_package(gflags COMPONENTS nothreads_static)
#find_package(inotify-cpp CONFIG 0.2.0 REQUIRED COMPONENTS shared)
include_directories(${JNI_INCLUDE_DIRS})


set(PROFILER_VERSION 2.8.3)
set(JATTACH_VERSION ${PROFILER_VERSION}-ap)

add_executable(jattach
        src/jattach/jattach.c
        src/jattach/jattach_hotspot.c
        src/jattach/jattach_openj9.c
        src/jattach/psutil.c
        src/jattach/psutil.h)

add_executable(fdtransfer src/fdtransfer/fdtransfer.h
        src/fdtransfer/fdtransferServer.cpp)

add_library(asyncProfiler SHARED
        src/allocTracer.cpp
        src/allocTracer.h
        src/arch.h
        src/arguments.cpp
        src/arguments.h
        src/callTraceStorage.cpp
        src/callTraceStorage.h
        src/codeCache.cpp
        src/codeCache.h
        src/dictionary.cpp
        src/dictionary.h
        src/dwarf.cpp
        src/dwarf.h
        src/engine.cpp
        src/engine.h
        src/event.h
        src/fdtransferClient.h
        src/fdtransferClient_linux.cpp
        src/flameGraph.cpp
        src/flameGraph.h
        src/flightRecorder.cpp
        src/flightRecorder.h
        src/frameName.cpp
        src/frameName.h
        src/incbin.h
        src/instrument.cpp
        src/instrument.h
        src/itimer.cpp
        src/itimer.h
        src/j9Ext.cpp
        src/j9Ext.h
        src/j9ObjectSampler.cpp
        src/j9ObjectSampler.h
        src/j9StackTraces.cpp
        src/j9StackTraces.h
        src/j9WallClock.cpp
        src/j9WallClock.h
        src/javaApi.cpp
        src/javaApi.h
        src/jfrMetadata.cpp
        src/jfrMetadata.h
        src/linearAllocator.cpp
        src/linearAllocator.h
        src/lockTracer.cpp
        src/lockTracer.h
        src/log.cpp
        src/log.h
        src/mutex.cpp
        src/mutex.h
        src/objectSampler.cpp
        src/objectSampler.h
        src/os.h
        src/os_linux.cpp
        src/os_macos.cpp
        src/perfEvents.h
        src/perfEvents_linux.cpp
        src/perfEvents_macos.cpp
        src/profiler.cpp
        src/profiler.h
        src/safeAccess.h
        src/spinLock.h
        src/stackFrame.h
        src/stackFrame_aarch64.cpp
        src/stackFrame_arm.cpp
        src/stackFrame_i386.cpp
        src/stackFrame_ppc64.cpp
        src/stackFrame_x64.cpp
        src/stackWalker.cpp
        src/stackWalker.h
        src/symbols.h
        src/symbols_linux.cpp
        src/symbols_macos.cpp
        src/threadFilter.cpp
        src/threadFilter.h
        src/trap.cpp
        src/trap.h
        src/tsc.cpp
        src/tsc.h
        src/vmEntry.cpp
        src/vmEntry.h
        src/vmStructs.cpp
        src/vmStructs.h
        src/wallClock.cpp
        src/wallClock.h)

target_link_libraries(asyncProfiler Folly::folly gflags inotify-cpp)
target_compile_options(asyncProfiler PRIVATE -fPIC -fno-omit-frame-pointer -fvisibility=hidden)
target_compile_definitions(asyncProfiler PRIVATE JATTACH_VERSION="${JATTACH_VERSION}")
target_compile_definitions(asyncProfiler PRIVATE PROFILER_VERSION="${PROFILER_VERSION}")