cmake_minimum_required(VERSION 3.8)

project(linux)

set(CMAKE_C_STANDARD 11)

# Make Targets
add_definitions(-std=c11)

# add_custom_target(vmlinux COMMAND $(MAKE) -C ${linux_SOURCE_DIR} PWD=${linux_SOURCE_DIR})

#set(LINUX_HEADERS_VERSION 4.4.0-83)
#set(LINUX_HEADERS_PATH /usr/src/linux-headers-${LINUX_HEADERS_VERSION})
set(LINUX_DEV_PATH /home/Storage/SSD/Development/Software/Github/eliaskousk/linux)
set(LINUX_PATH ${LINUX_DEV_PATH})

set(LINUX_HEADERS
        arch/x86/include
        arch/x86/include/generated
        arch/x86/include/generated/uapi
        arch/x86/include/uapi
        include
        include/generated
        include/generated/uapi
        include/uapi
        )

file(GLOB MOD_SOURCE_FILES
        "arch/x86/boot/*.c"
        "arch/x86/entry/*.c"
        "arch/x86/kernel/*.c"
        "init/*.c"
        "fs/*.c"
        "kernel/*.c"
        "kernel/*.h"
        "virt/kvm/*.c"
        "virt/kvm/*.h"
        "arch/x86/kvm/*.c"
        "arch/x86/kvm/*.h")

add_executable(dummy_mod ${MOD_SOURCE_FILES})

TARGET_INCLUDE_DIRECTORIES(dummy_mod PUBLIC ${LINUX_HEADERS})

TARGET_SOURCES(dummy_mod PUBLIC ${MOD_SOURCE_FILES})

TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC __KERNEL__)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC MODULE)
TARGET_COMPILE_DEFINITIONS(dummy_mod PUBLIC CONFIG_X86_32)

TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -nostdinc)
TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -isystem /usr/lib/gcc/x86_64-linux-gnu/7/include)
TARGET_COMPILE_OPTIONS(dummy_mod PUBLIC -include ${LINUX_PATH}/include/linux/kconfig.h)
