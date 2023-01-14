cmake_minimum_required(VERSION 3.12)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(pico_examples C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.3.0")
    message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.3.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

# Initialize the SDK
pico_sdk_init()


if (NOT PICO_CYW43_SUPPORTED) # set by BOARD=pico-w
	message(WARNING "Skipping Pico W examples as CYW43 not supported (did you pass -DPICO_BOARD=pico_w ?)")
else()
    if (NOT TARGET pico_cyw43_arch)
		message(WARNING "Skipping Pico W examples as support is not available")
    else()
        if (NOT TEST_TCP_SERVER_IP)
			message(WARNING "Skipping tcp_client example as TEST_TCP_SERVER_IP is not defined")
        else()
			add_executable(picow_tcpip_client_background
                    src/picow_tcp_client.c
                    )
            target_compile_definitions(picow_tcpip_client_background PRIVATE
                    TEST_TCP_SERVER_IP=\"${TEST_TCP_SERVER_IP}\"
                    )
            target_include_directories(picow_tcpip_client_background PRIVATE
                    ${CMAKE_CURRENT_LIST_DIR}/src
                    )
            target_link_libraries(picow_tcpip_client_background
                    pico_cyw43_arch_lwip_threadsafe_background
                    pico_stdlib
                    hardware_adc
                    )

            pico_add_extra_outputs(picow_tcpip_client_background)
            pico_enable_stdio_usb(picow_tcpip_client_background 1)
            pico_enable_stdio_uart(picow_tcpip_client_background 0)

            add_executable(picow_tcpip_client_poll
                    src/picow_tcp_client.c
                    )
            target_compile_definitions(picow_tcpip_client_poll PRIVATE
                    TEST_TCP_SERVER_IP=\"${TEST_TCP_SERVER_IP}\"
                    )
            target_include_directories(picow_tcpip_client_poll PRIVATE
                    ${CMAKE_CURRENT_LIST_DIR}/src
                    )
            target_link_libraries(picow_tcpip_client_poll
                    pico_cyw43_arch_lwip_poll
                    pico_stdlib
                    hardware_adc
                    )
            pico_add_extra_outputs(picow_tcpip_client_poll)
            pico_enable_stdio_usb(picow_tcpip_client_poll 1)
            pico_enable_stdio_uart(picow_tcpip_client_poll 0)
        endif()
    endif()
endif()
