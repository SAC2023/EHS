if (POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif ()

project(dash)
cmake_minimum_required(VERSION 3.11)
include(FetchContent)
include(ExternalProject)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wno-long-long -fPIC -march=native")
set(CMAKE_ENABLE_COMPILE_COMMANDS "ON")

if (${CMAKE_BUILD_TYPE} STREQUAL "Release")
  message("-- Release mode, all optimizations enabled")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -DNDEBUG -Ofast")
endif ()

##################### PMDK ####################
set(PMDK_PREFIX "${CMAKE_CURRENT_BINARY_DIR}/pmdk")
ExternalProject_Add(PMDK
        GIT_REPOSITORY https://github.com/HaoPatrick/pmdk.git
        GIT_TAG addr-patch
	BUILD_IN_SOURCE 1
        BUILD_COMMAND $(MAKE)
        PREFIX ${PMDK_PREFIX}
        CONFIGURE_COMMAND ""
        INSTALL_COMMAND ""
        LOG_DOWNLOAD ON
        LOG_CONFIGURE ON
        LOG_BUILD ON
        )

include_directories(${PMDK_PREFIX}/src/PMDK/src/include)
if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
  link_directories(${PMDK_PREFIX}/src/PMDK/src/debug)
else ()
  link_directories(${PMDK_PREFIX}/src/PMDK/src/nondebug)
endif ()


##################### Epoch Reclaim ####################
FetchContent_Declare(
        epoch_reclaimer
        GIT_REPOSITORY https://github.com/XiangpengHao/epoch-reclaimer.git
        GIT_TAG master
)
if (NOT epoch_reclaimer_POPULATED)
  FetchContent_Populate(epoch_reclaimer)
  include_directories(${epoch_reclaimer_SOURCE_DIR})
endif ()

option(USE_PMEM "enable persistent memory support" ON)

set(libs_to_link
        pthread gflags)

if (USE_PMEM MATCHES "ON")
  message(STATUS "persistent memory support enabled, going to build with PMDK")
  add_definitions(-DPMEM)
  list(APPEND libs_to_link pmemobj pmem)
endif ()

if (USE_PMEM MATCHES "ON")
  add_executable(test_pmem src/test_pmem.cpp)
  add_executable(test_pmem2 src/test_pmem2.cpp)
  add_executable(example2 src/example2.cpp)

  add_executable(exampledash_1gb src/exampledash_1gb.cpp)
  add_executable(exampledash_5gb src/exampledash_5gb.cpp)
  add_executable(exampledash_10gb src/exampledash_10gb.cpp)
  add_executable(exampledash_20gb src/exampledash_20gb.cpp)
  add_executable(exampledash_30gb src/exampledash_30gb.cpp)

  add_executable(examplecceh_1gb src/examplecceh_1gb.cpp)
  add_executable(examplecceh_5gb src/examplecceh_5gb.cpp)
  add_executable(examplecceh_10gb src/examplecceh_10gb.cpp)
  add_executable(examplecceh_20gb src/examplecceh_20gb.cpp)
  add_executable(examplecceh_30gb src/examplecceh_30gb.cpp)
  add_executable(examplecceh src/examplecceh.cpp)

  add_executable(exampleacceh_1gb src/exampleacceh_1gb.cpp)
  add_executable(exampleacceh_5gb src/exampleacceh_5gb.cpp)
  add_executable(exampleacceh_10gb src/exampleacceh_10gb.cpp)
  add_executable(exampleacceh_20gb src/exampleacceh_20gb.cpp)
  add_executable(exampleacceh_30gb src/exampleacceh_30gb.cpp)
  add_executable(exampleacceh src/exampleacceh.cpp)

  add_executable(example src/example.cpp)
  target_link_libraries(test_pmem PRIVATE ${libs_to_link})
  target_link_libraries(test_pmem2 PRIVATE ${libs_to_link})
  target_link_libraries(example PRIVATE ${libs_to_link})
  target_link_libraries(example2 PRIVATE ${libs_to_link})

  target_link_libraries(exampledash_1gb PRIVATE ${libs_to_link})
  target_link_libraries(exampledash_5gb PRIVATE ${libs_to_link})
  target_link_libraries(exampledash_10gb PRIVATE ${libs_to_link})
  target_link_libraries(exampledash_20gb PRIVATE ${libs_to_link})
  target_link_libraries(exampledash_30gb PRIVATE ${libs_to_link})


  target_link_libraries(examplecceh PRIVATE ${libs_to_link})
  target_link_libraries(examplecceh_1gb PRIVATE ${libs_to_link})
  target_link_libraries(examplecceh_5gb PRIVATE ${libs_to_link})
  target_link_libraries(examplecceh_10gb PRIVATE ${libs_to_link})
  target_link_libraries(examplecceh_20gb PRIVATE ${libs_to_link})
  target_link_libraries(examplecceh_30gb PRIVATE ${libs_to_link})

  target_link_libraries(exampleacceh PRIVATE ${libs_to_link})
  target_link_libraries(exampleacceh_1gb PRIVATE ${libs_to_link})
  target_link_libraries(exampleacceh_5gb PRIVATE ${libs_to_link})
  target_link_libraries(exampleacceh_10gb PRIVATE ${libs_to_link})
  target_link_libraries(exampleacceh_20gb PRIVATE ${libs_to_link})
  target_link_libraries(exampleacceh_30gb PRIVATE ${libs_to_link})

  add_dependencies(test_pmem PMDK)
  add_dependencies(test_pmem2 PMDK)
  add_dependencies(example PMDK)
  add_dependencies(example2 PMDK)

  add_dependencies(examplecceh PMDK)
  add_dependencies(examplecceh_1gb PMDK)
  add_dependencies(examplecceh_5gb PMDK)
  add_dependencies(examplecceh_10gb PMDK)
  add_dependencies(examplecceh_20gb PMDK)
  add_dependencies(examplecceh_30gb PMDK)

  add_dependencies(exampledash_1gb PMDK)
  add_dependencies(exampledash_5gb PMDK)
  add_dependencies(exampledash_10gb PMDK)
  add_dependencies(exampledash_20gb PMDK)
  add_dependencies(exampledash_30gb PMDK)

  add_dependencies(exampleacceh PMDK)
  add_dependencies(exampleacceh_1gb PMDK)
  add_dependencies(exampleacceh_5gb PMDK)
  add_dependencies(exampleacceh_10gb PMDK)
  add_dependencies(exampleacceh_20gb PMDK)
  add_dependencies(exampleacceh_30gb PMDK)

else ()
endif ()
