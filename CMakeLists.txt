cmake_minimum_required(VERSION 3.13)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(nice_tft C CXX ASM)
add_definitions(-DARDUINO_ARCH_RP2040)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -felide-constructors") # For String class optimization


pico_sdk_init()

add_executable(nice_tft nice_tft.cpp)

pico_set_program_name(nice_tft "nice_tft")
pico_set_program_version(nice_tft "0.1")

pico_enable_stdio_uart(nice_tft 1)
pico_enable_stdio_usb(nice_tft 1) # Needed for picotool auto upload


target_include_directories(nice_tft PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${PROJECT_SOURCE_DIR}/TFT_eSPI
)


target_link_libraries(nice_tft
        pico_stdlib
        hardware_spi
        hardware_dma
        hardware_pio
        TFT_eSPI
        )
        
add_subdirectory(TFT_eSPI)


pico_add_extra_outputs(nice_tft)
string(APPEND CMAKE_EXE_LINKER_FLAGS "-Wl,--print-memory-usage")
