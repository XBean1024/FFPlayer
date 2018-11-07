cmake_minimum_required(VERSION 3.4.1)
find_library(log-lib log)
find_library(android-lib android)

add_library(native-lib
        SHARED
        src/main/cpp/native-lib.cpp)

set(JNI_LIBS_DIR ${CMAKE_SOURCE_DIR}/src/main/jniLibs)

#引入第三方库

#第一个参数和add_library的第一个参数一样，
#不过这里的库别名要和add_library的库别名要一致，
#要不然在编译时会报找不到库的错误。
#第二个参数是固定的，都是写“ PROPERTIES IMPORTED_LOCATION”主要用来指定库的引入方式。都是通过本地引入。
#第三个就是库的具体路径，这个不能写错，如果写错了，编译时也同样会找不到库的。
#只要是引入第三方的库使用add_library就要使用set_target_propeties这个组合，所以它们是成对出现的。

#添加第三库
add_library(avutil#引入库的别名
        SHARED#库类型
        IMPORTED#引入方式
        )
#设置so库详细信息
set_target_properties(avutil
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libavutil.so)

add_library(swresample
        SHARED
        IMPORTED)
set_target_properties(swresample
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libswresample.so)

add_library(swscale
        SHARED
        IMPORTED)
set_target_properties(swscale
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libswscale.so)

add_library(avcodec
        SHARED
        IMPORTED)
set_target_properties(avcodec
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libavcodec.so)

add_library(avformat
        SHARED
        IMPORTED)
set_target_properties(avformat
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libavformat.so)

add_library(avfilter
        SHARED
        IMPORTED)
set_target_properties(avfilter
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libavfilter.so)

add_library(avdevice
        SHARED
        IMPORTED)
set_target_properties(avdevice
        PROPERTIES IMPORTED_LOCATION
        ${JNI_LIBS_DIR}/${ANDROID_ABI}/libavdevice.so)


include_directories(${JNI_LIBS_DIR}/includes)

target_link_libraries(native-lib
        avutil
        swresample
        swscale
        avcodec
        avformat
        avfilter
        avdevice
        ${log-lib}
        ${android-lib}
        )
