cmake_minimum_required(VERSION 3.26)
project(cs149_asst1)

set(CMAKE_CXX_STANDARD 17)

include_directories(common)

add_executable(mandelbrot prog1_mandelbrot_threads/main.cpp
        prog1_mandelbrot_threads/mandelbrotSerial.cpp
        prog1_mandelbrot_threads/mandelbrotThread.cpp
        common/CycleTimer.h
		common/ppm.cpp
		common/tasksys.cpp)

add_executable(myexp
		prog2_vecintrin/main.cpp
		prog2_vecintrin/CS149intrin.cpp
		prog2_vecintrin/CS149intrin.h
		prog2_vecintrin/logger.cpp
		prog2_vecintrin/logger.h)
