# This code is part of Pcap_DNSProxy
# Pcap_DNSProxy, a local DNS server based on WinPcap and LibPcap
# Copyright (C) 2012-2019 Chengr28
# 
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either
# version 2 of the License, or (at your option) any later version.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.

# List all source files.
set(MAIN_SRC_FILES
		"Base.h"
		"Capture.h"
		"Configuration.h"
		"Definition.h"
		"DNSCurveControl.h"
		"DNSCurveRequest.h"
		"Include.h"
		"Initialization.h"
		"Main.h"
		"Monitor.h"
		"Network.h"
		"PacketData.h"
		"Platform.h"
		"PrintLog.h"
		"Process.h"
		"Protocol.h"
		"Proxy.h"
		"Request.h"
		"Service.h"
		"Structure.h"
		"Template.h"
		"TransportSecurity.h"
		"Type.h"
		"Base.cpp"
		"Capture.cpp"
		"Configuration.cpp"
		"DNSCurveControl.cpp"
		"DNSCurveRequest.cpp"
		"Initialization.cpp"
		"Main.cpp"
		"Monitor.cpp"
		"Network.cpp"
		"PacketData.cpp"
		"PrintLog.cpp"
		"Process.cpp"
		"Protocol.cpp"
		"Proxy.cpp"
		"ReadCommand.cpp"
		"ReadHosts.cpp"
		"ReadIPFilter.cpp"
		"ReadParameter.cpp"
		"Request.cpp"
		"Service.cpp"
		"TransportSecurity.cpp")

add_executable(Pcap_DNSProxy ${MAIN_SRC_FILES})

# Compiling and linking flags
IF(NOT PLATFORM_OPENWRT)
	# Interprocedural Optimization (LTO)
	IF(CMAKE_VERSION VERSION_LESS "3.9")
		target_compile_options(Pcap_DNSProxy PRIVATE -flto)
	ELSE()
		set_target_properties(Pcap_DNSProxy PROPERTIES INTERPROCEDURAL_OPTIMIZATION TRUE)
	ENDIF()

	set_target_properties(Pcap_DNSProxy PROPERTIES POSITION_INDEPENDENT_CODE TRUE)

	target_compile_definitions(Pcap_DNSProxy PRIVATE
		_FORTIFY_SOURCE=2
		_GLIBCXX_ASSERTIONS)

	target_compile_options(Pcap_DNSProxy PRIVATE
		-fasynchronous-unwind-tables
		-fexceptions
		-fstack-protector-strong
		-g
		-grecord-gcc-switches
		-march=native
		-pipe
		-O3)

	IF(NOT CMAKE_SYSTEM_NAME MATCHES "Darwin")
		target_compile_options(Pcap_DNSProxy PRIVATE -fopenmp)
	ENDIF()
ENDIF()

# Platform and library definitions
IF(PLATFORM_OPENWRT)
	target_compile_definitions(Pcap_DNSProxy PRIVATE PLATFORM_OPENWRT)
	install(TARGETS Pcap_DNSProxy RUNTIME DESTINATION sbin)
ENDIF()

IF(ENABLE_LIBSODIUM)
	target_compile_definitions(Pcap_DNSProxy PRIVATE ENABLE_LIBSODIUM)
ENDIF()

IF(ENABLE_PCAP)
	target_compile_definitions(Pcap_DNSProxy PRIVATE ENABLE_PCAP)
ENDIF()

IF(ENABLE_TLS)
	target_compile_definitions(Pcap_DNSProxy PRIVATE ENABLE_TLS)
ENDIF()

# Static libraries linking if needed
IF(STATIC_LIB)
	set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")
	set_target_properties(Pcap_DNSProxy PROPERTIES LINK_FLAGS "-s -static -Wl,--whole-archive -lpthread -Wl,--no-whole-archive")
ENDIF()

# Libraries linking
find_package(Threads REQUIRED)
target_link_libraries(Pcap_DNSProxy PRIVATE ${CMAKE_THREAD_LIBS_INIT})

find_library(LIBEVENT_LIBRARIES event_core)
IF(CMAKE_SYSTEM_NAME MATCHES "FreeBSD")
	target_include_directories(Pcap_DNSProxy PRIVATE "/usr/local/include")
ENDIF()
target_link_libraries(Pcap_DNSProxy PRIVATE ${LIBEVENT_LIBRARIES})

IF(ENABLE_LIBSODIUM)
	find_library(LIBSODIUM_LIBRARIES sodium)
	target_link_libraries(Pcap_DNSProxy PRIVATE ${LIBSODIUM_LIBRARIES})
ENDIF()

IF(ENABLE_PCAP)
	find_library(LIBPCAP_LIBRARIES pcap)
	target_link_libraries(Pcap_DNSProxy PRIVATE ${LIBPCAP_LIBRARIES})
ENDIF()

IF(ENABLE_TLS)
	find_package(OpenSSL REQUIRED)
	target_link_libraries(Pcap_DNSProxy PRIVATE OpenSSL::SSL OpenSSL::Crypto)
	IF(STATIC_LIB)
		find_library(LIBDL_LIBRARIES dl)
		target_link_libraries(Pcap_DNSProxy PRIVATE ${LIBDL_LIBRARIES})
		find_library(LIBZ_LIBRARIES z)
		target_link_libraries(Pcap_DNSProxy PRIVATE ${LIBZ_LIBRARIES})
	ENDIF()
ENDIF()
