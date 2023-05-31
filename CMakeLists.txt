cmake_minimum_required(VERSION 3.15.3)

project(nucleo-f303re)

enable_language(C ASM)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)


set(STM32CUBEMX_GENERATED_FILES

        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_tim_ex.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_uart.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_rcc_ex.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_flash_ex.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_gpio.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_dma.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pwr_ex.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_cortex.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_exti.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pcd.c
        Drivers/STM32F3xx_HAL_Driver/Src/stm32f3xx_hal_pcd_ex.c

        Inc/main.h
        Inc/stm32f3xx_hal_conf.h
        Inc/stm32f3xx_it.h

        Src/main.c
        Src/stm32f3xx_hal_msp.c
        Src/stm32f3xx_it.c
        Src/system_stm32f3xx.c
        startup_stm32f303xe.s
)
set(EXECUTABLE ${PROJECT_NAME}.out)

add_executable(${EXECUTABLE} ${STM32CUBEMX_GENERATED_FILES})

target_compile_definitions(${EXECUTABLE} PRIVATE
        -DUSE_HAL_DRIVER
        -DSTM32F303xE
        )

target_include_directories(${EXECUTABLE} PRIVATE
        Inc
        Drivers/STM32F3xx_HAL_Driver/Inc
        Drivers/CMSIS/Device/ST/STM32F3xx/Include
        Drivers/CMSIS/Include
        )

target_compile_options(${EXECUTABLE} PRIVATE
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard

        -fdata-sections
        -ffunction-sections

        -Wall

        $<$<CONFIG:Debug>:-Og>
        )

target_link_options(${EXECUTABLE} PRIVATE
        -T${CMAKE_SOURCE_DIR}/STM32F303RETx_FLASH.ld
        -mcpu=cortex-m4
        -mthumb
        -mfpu=fpv4-sp-d16
        -mfloat-abi=hard
        -specs=nano.specs
        -lc
        -lm
        -lnosys
        -Wl,-Map=${PROJECT_NAME}.map,--cref
        -Wl,--gc-sections
        )
