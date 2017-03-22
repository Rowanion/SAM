cmake_minimum_required(VERSION 2.8.0 FATAL_ERROR)

project(SAM)

###################################################################################################
## global lib configs ##

# enable virtual folders in VC++
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
###################################################################################################
## compiler settings

IF(CMAKE_GENERATOR MATCHES "NMake.*")
    add_definitions( -DECLIPSE )    
ENDIF()

# enable nmakes multi-processor usage and adds general build flags
if(CMAKE_BUILD_TOOL MATCHES "(msdev|devenv|nmake)")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP ")
endif()

#######################################################################
## Libs and project settings

set(${PROJECT_NAME}_HEADERS
	src/debug.h
  src/reciter.h
  src/ReciterTabs.h
  src/render.h
  src/RenderTabs.h
  src/sam.h
  src/SamTabs.h
)

set(${PROJECT_NAME}_SOURCES

  src/debug.c
  src/main.c
  src/reciter.c
  src/render.c
  src/sam.c
)

set(${PROJECT_NAME}_LINK_LIBS
	${Boost_LIBRARIES}
)
message(STATUS ${Boost_LIBRARIES})

add_executable(${PROJECT_NAME} ${BINARY_PROPERTIES}
	${${PROJECT_NAME}_SOURCES}
	${${PROJECT_NAME}_HEADERS}
)

# enable virtual folders (for Visual Studio)
SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES ENABLE_EXPORTS TRUE)

target_link_libraries(${PROJECT_NAME} 
    ${${PROJECT_NAME}_LINK_LIBS}
)

INSTALL(CODE "FILE(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/install)")
INSTALL(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_BINARY_DIR}/install/)

