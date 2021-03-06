### Wisecracker: A cryptanalysis framework
### Copyright (c) 2011-2012, Vikas Naresh Kumar, Selective Intellect LLC
###    
###	This program is free software: you can redistribute it and/or modify
### it under the terms of the GNU General Public License as published by
### the Free Software Foundation, either version 3 of the License, or
### any later version.
###
### This program is distributed in the hope that it will be useful,
### but WITHOUT ANY WARRANTY; without even the implied warranty of
### MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
### GNU General Public License for more details.
###
### You should have received a copy of the GNU General Public License
### along with this program.  If not, see <http://www.gnu.org/licenses/>.
#########################################################################
### COPYRIGHT: Selective Intellect LLC
### AUTHOR: Vikas Kumar
### DATE: 25th August 2011
#########################################################################
project(wisecracker)

include_directories(${CMAKE_CURRENT_BINARY_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

set(UTIL_SRCS utils.c)
if (NOT WC_GETOPT_H)
	set(UTIL_SRCS ${UTIL_SRCS} getopt.c)
endif (NOT WC_GETOPT_H)

set(RUNTIME_SRCS executor.c mpi.c opencl.c wrapper.cpp)
add_library(wisecracker_s STATIC ${RUNTIME_SRCS} ${UTIL_SRCS})
target_link_libraries(wisecracker_s ${OPENCL_LIBS} ${OS_LIBS})
if (USE_MPI)
	target_link_libraries(wisecracker_s ${MPI_LIBRARIES})
endif (USE_MPI)
set_target_properties(wisecracker_s PROPERTIES COMPILE_FLAGS "-DWC_LINKING_STATIC")
add_library(wisecracker SHARED ${RUNTIME_SRCS} ${UTIL_SRCS})
target_link_libraries(wisecracker ${OPENCL_LIBS} ${OS_LIBS})
if (USE_MPI)
	target_link_libraries(wisecracker ${MPI_LIBRARIES})
endif (USE_MPI)
set_target_properties(wisecracker PROPERTIES COMPILE_FLAGS "-DWC_COMPILING_DLL")
install(TARGETS wisecracker_s ARCHIVE DESTINATION lib)
install(TARGETS wisecracker RUNTIME DESTINATION bin LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)
