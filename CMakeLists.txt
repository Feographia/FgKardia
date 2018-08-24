# ****************************************************************************
#  Project:  Feographia
#  Purpose:  The application to work with the biblical text
#  Author:   NikitaFeodonit, nfeodonit@yandex.com
# ****************************************************************************
#    Copyright (c) 2017-2018 NikitaFeodonit
#
#    This file is part of the Feographia project.
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published
#    by the Free Software Foundation, either version 3 of the License,
#    or (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
#    See the GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with this program. If not, see <http://www.gnu.org/licenses/>.
# ****************************************************************************

cmake_minimum_required(VERSION 3.3)

project(FgKardia CXX)
set(lib_NAME "fgkardia")

include(GNUInstallDirs)


#-----------------------------------------------------------------------
# Set vars to FgKardia
#-----------------------------------------------------------------------


#-----------------------------------------------------------------------
# FgKardia
#-----------------------------------------------------------------------

set(private_src_DIR "${CMAKE_CURRENT_LIST_DIR}/src")
set(interface_src_DIR "${CMAKE_INSTALL_INCLUDEDIR}")
set(build_src_DIR
  "$<BUILD_INTERFACE:${private_src_DIR}>"
)
set(install_src_DIR
  "$<INSTALL_INTERFACE:${interface_src_DIR}>"
)
set(public_src_DIR "${build_src_DIR}${install_src_DIR}")

add_library(${lib_NAME} "")
set_target_properties(${lib_NAME} PROPERTIES
  CXX_STANDARD 14
  C_STANDARD 11
)
# Export FgKardia include dirs.
target_include_directories(${lib_NAME} PUBLIC ${public_src_DIR})

target_sources(${lib_NAME}
  PRIVATE
#    ${private_src_DIR}/fgkardia/fg.cpp

  PUBLIC
#    ${public_src_DIR}/fgkardia/fg.h
)


#-----------------------------------------------------------------------
# Link to the libraries
#-----------------------------------------------------------------------

find_package(Threads REQUIRED)
find_package(STLCache REQUIRED)
find_package(ICU REQUIRED COMPONENTS i18n uc data)
find_package(SQLite3 REQUIRED)
find_package(SQLiteORM REQUIRED)
find_package(Boost REQUIRED COMPONENTS regex)

# Boost
# Boost deps libs will also be added as IMPORT library.
#target_link_libraries(${lib_NAME} PRIVATE Boost::boost Boost::filesystem)
target_link_libraries(${lib_NAME} PRIVATE Boost::regex)

# SQLiteORM
target_include_directories(${lib_NAME} PRIVATE ${SQLITEORM_INCLUDE_DIR})

# SQLite3
target_include_directories(${lib_NAME} PRIVATE ${SQLITE3_INCLUDE_DIR})
target_link_libraries(${lib_NAME} PRIVATE ${SQLITE3_LIBRARY})

# ICU
target_link_libraries(${lib_NAME} PRIVATE ICU::i18n ICU::uc ICU::data)

# STLCache
target_include_directories(${lib_NAME} PRIVATE ${STLCACHE_INCLUDE_DIR})

# Threads
target_link_libraries(${lib_NAME} PRIVATE Threads::Threads)


#-----------------------------------------------------------------------
# Testing
#-----------------------------------------------------------------------

add_subdirectory(test)
