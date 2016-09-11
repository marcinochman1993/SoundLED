list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/CMake)
set(STM32_CHIP STM32F411xE)
set(STM32Cube_DIR  .)

include(gcc_stm32)

enable_language(ASM)

find_package(CMSIS REQUIRED)
find_package(STM32HAL REQUIRED)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMSIS_INCLUDE_DIRS}
    ${STM32HAL_INCLUDE_DIR}
    Inc
    Middlewares/Third_Party/FreeRTOS/Source/include/
    Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F/
    Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS
    Drivers/CMSIS/RTOS/Template/
)

file(GLOB_RECURSE PROJECT_SOURCES
  Src/*.c
  )

file(GLOB RTOS_SOURCES
  Middlewares/Third_Party/FreeRTOS/Source/*.c)

list(APPEND RTOS_SOURCES
  Middlewares/Third_Party/FreeRTOS/Source/CMSIS_RTOS/cmsis_os.c
  Middlewares/Third_Party/FreeRTOS/Source/portable/GCC/ARM_CM4F/port.c
  Middlewares/Third_Party/FreeRTOS/Source/portable/MemMang/heap_1.c
  )

set(STM32_LINKER_SCRIPT ${CMSIS_LINKER_SCRIPT})

message(INFO ${CMSIS_LINKER_SCRIPT})

add_executable(${CMAKE_PROJECT_NAME}
  ${PROJECT_SOURCES} ${CMSIS_SOURCES} ${STM32HAL_SOURCES} ${RTOS_SOURCES})

stm32_set_target_properties(${CMAKE_PROJECT_NAME})
stm32_add_hex_bin_targets(${CMAKE_PROJECT_NAME})
stm32_print_size_of_targets(${CMAKE_PROJECT_NAME})
