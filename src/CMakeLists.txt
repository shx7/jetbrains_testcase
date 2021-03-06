set(FILE_CLASSIFIER_SRC FileClassifier.cpp)
set(UTIL_SRC Util.cpp)
set(PROJECT_INCLUDE_DIR "../include/")

find_package(Boost 1.55 COMPONENTS
        filesystem program_options system
        REQUIRED)
if (Boost_FOUND)
    message(STATUS "Boost include dir: ${Boost_INCLUDE_DIR}")
    message(STATUS "Boost libraries path: ${Boost_LIBRARY_DIRS}")
    message(STATUS "Boost libraries path: ${Boost_LIBRARIES}")
    set(Boost_USE_STATIC_LIBS OFF)
    set(Boost_USE_STATIC_RUNTIME OFF) 
else()
    message(FATAL_ERROR "Boost not found") 
endif()

find_package(OpenSSL)
if (OPENSSL_FOUND)
    message(STATUS "OpenSSL found. Version ${OPENSSL_VERSION}") 
    message(STATUS "OpenSSL include dir: ${OPENSSL_INCLUDE_DIR}")
    message(STATUS "OpenSSL libraries path: ${OPENSSL_LIBRARIES}")
else()
    message(FATAL_ERROR "OpenSSL not found") 
endif()

add_executable(${MAIN_EXE} ${MAIN_SRC} ${FILE_CLASSIFIER_SRC} ${UTIL_SRC})
set_target_properties(${MAIN_EXE} PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
    ) 

include_directories(${Boost_INCLUDE_DIR})
link_directories(${Boost_LIBRARY_DIRS})

include_directories(${OPENSSL_INCLUDE_DIR})
link_directories(${OPENSSL_LIBRARY_DIRS})

include_directories(${PROJECT_INCLUDE_DIR})
target_link_libraries(${MAIN_EXE} ${Boost_LIBRARIES} ${OPENSSL_LIBRARIES})
