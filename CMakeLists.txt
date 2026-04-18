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

# CMake minimum version
cmake_minimum_required(VERSION 3.5)

# Project name
project(Pcap_DNSProxy)

# Policy settings for newer CMake versions
if(POLICY CMP0069)
    cmake_policy(SET CMP0069 NEW) # INTERPROCEDURAL_OPTIMIZATION
endif()

# C++ standard
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)
set(CMAKE_CXX_EXTENSIONS OFF)

# Build type default
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build." FORCE)
endif()

# Sub directories
add_subdirectory(Source/Pcap_DNSProxy)
