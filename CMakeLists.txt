cmake_minimum_required(VERSION 3.16)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
	message(FATAL_ERROR "Do not build in-source. Please remove CMakeCache.txt and the CMakeFiles/ directory. Then build out-of-source.")
endif()

set(CMAKE_CXX_STANDARD			20)
set(CMAKE_CXX_STANDARD_REQUIRED	True)

project(
	lucid_arena_sdk
		LANGUAGES	CXX
)

set_property(GLOBAL PROPERTY USE_FOLDERS ON)

include(GNUInstallDirs)

execute_process(COMMAND python -m pip install gdown)
execute_process(COMMAND python -m pip install --upgrade pip)

if(${CMAKE_SYSTEM_PROCESSOR} STREQUAL "x86_64")
	set(FILE_NAME "ArenaSDK_v0.1.68_Linux_x64.tar.gz")
	if(NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build/${FILE_NAME})
		set(URL "https://drive.google.com/u/0/uc?id=1obC4WLuOSkcu8kXv6upRB3CSvpiwepqX&export=download")
		execute_process(
			COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/download.py ${URL}
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()
	if(NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build/ArenaSDK_Linux_x64)
		execute_process(
			COMMAND tar -xzf ${FILE_NAME}
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()
	if(NOT EXISTS ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK.conf)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_x64/lib64 > ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_x64/GenICam/library/lib/Linux64_x64 >> ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_x64/ffmpeg >> ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()

elseif(${CMAKE_SYSTEM_PROCESSOR} STREQUAL "aarch64")
	set(FILE_NAME "ArenaSDK_v0.1.49_Linux_ARM64.tar.gz")
	if(NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build/${FILE_NAME})
		set(URL "https://drive.google.com/u/0/uc?id=1_7vfq4Yp8be0RCzx4Sgmc7jPzd0LqflY&export=download")
		execute_process(
			COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/download.py ${URL}
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()
	if(NOT EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build/ArenaSDK_Linux_ARM64)
		execute_process(
			COMMAND tar -xzf ${FILE_NAME}
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()
	if(NOT EXISTS ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK.conf)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_ARM64/lib > ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_ARM64/GenICam/library/lib/Linux64_ARM >> ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
		execute_process(
			COMMAND sh -c "echo ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/ArenaSDK_Linux_ARM64/ffmpeg >> ArenaSDK.conf"
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/build)
	endif()

else()
	message(FATAL_ERROR ${CMAKE_SYSTEM_PROCESSOR} " architecture is not supported yet")
endif()

if(${CMAKE_SYSTEM_PROCESSOR} STREQUAL "x86_64")
	install(
		DIRECTORY 	${CMAKE_CURRENT_SOURCE_DIR}/build/ArenaSDK_Linux_x64
		DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
	)
elseif(${CMAKE_SYSTEM_PROCESSOR} STREQUAL "aarch64")
	install(
		DIRECTORY 	${CMAKE_CURRENT_SOURCE_DIR}/build/ArenaSDK_Linux_ARM64
		DESTINATION ${CMAKE_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}
	)
endif()

install(
    FILES
		${CMAKE_CURRENT_SOURCE_DIR}/build/ArenaSDK.conf
    DESTINATION
		/etc/ld.so.conf.d
)
