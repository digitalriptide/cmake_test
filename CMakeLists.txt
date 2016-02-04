cmake_minimum_required( VERSION 3.4.3 )

find_package( PNG REQUIRED )

add_executable( cmake_test main.cpp )

target_include_directories( cmake_test SYSTEM PRIVATE ${PNG_INCLUDE_DIRS} )
