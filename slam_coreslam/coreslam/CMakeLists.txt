cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
rosbuild_init(coreslam)

set(ROS_BUILD_TYPE Release)

# Build CoreSLAM
execute_process(COMMAND cmake -E chdir ${PROJECT_SOURCE_DIR} make -f Makefile.coreslam
                RESULT_VARIABLE _make_failed)
if(_make_failed)
  message(FATAL_ERROR "Build of CoreSLAM failed")
endif(_make_failed)

# Build the ROS wrapper
include_directories(${PROJECT_SOURCE_DIR}/include)
link_directories(${PROJECT_SOURCE_DIR}/lib)
rosbuild_add_executable(bin/slam_coreslam src/slam_coreslam.cpp src/main.cpp)
target_link_libraries(bin/slam_coreslam CoreSLAM.a)
