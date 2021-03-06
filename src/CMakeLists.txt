INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../lib
  ${CMAKE_CURRENT_BINARY_DIR}/..)

CHECK_FUNCTION_EXISTS(getopt HAVE_GETOPT)
IF(NOT HAVE_GETOPT)
  SET(SRC_EXTRA_FILES getopt.c)
ENDIF(NOT HAVE_GETOPT)

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")

ADD_EXECUTABLE(zipcmp zipcmp.c ${SRC_EXTRA_FILES})
TARGET_LINK_LIBRARIES(zipcmp zip)
INSTALL(TARGETS zipcmp RUNTIME DESTINATION bin)

ADD_EXECUTABLE(zipmerge zipmerge.c ${SRC_EXTRA_FILES})
TARGET_LINK_LIBRARIES(zipmerge zip)
INSTALL(TARGETS zipmerge RUNTIME DESTINATION bin)

ADD_EXECUTABLE(ziptorrent ziptorrent.c ${SRC_EXTRA_FILES})
TARGET_LINK_LIBRARIES(ziptorrent zip)
INSTALL(TARGETS ziptorrent RUNTIME DESTINATION bin)

set(CompilerFlags
      CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
      CMAKE_CXX_FLAGS_MINSIZEREL CMAKE_CXX_FLAGS_RELWITHDEBINFO
      CMAKE_C_FLAGS CMAKE_C_FLAGS_DEBUG CMAKE_C_FLAGS_RELEASE
      CMAKE_C_FLAGS_MINSIZEREL CMAKE_C_FLAGS_RELWITHDEBINFO)
foreach(CompilerFlag ${CompilerFlags})
  string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
endforeach()
