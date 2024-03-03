cmake_minimum_required(VERSION 3.26)
project(cs149_asst1 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)

include_directories(common)

add_compile_options(-mfma)
add_compile_options(-mavx)
add_compile_options(-mavx2)

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

#[[if (MSVC)
	add_custom_command(OUTPUT ${PROJECT_BINARY_DIR}/calc_min_max_ispc.o ${PROJECT_BINARY_DIR}/include/calc_min_max_ispc.h
			COMMAND E:/tool/ispc-v1.22.0-windows/bin/ispc.exe --target=avx2 --target-os=windows --arch=x86-64 ${CMAKE_CURRENT_SOURCE_DIR}/calc_min_max_ispc.ispc
			-h ${PROJECT_BINARY_DIR}/include/calc_min_max_ispc.h -o ${PROJECT_BINARY_DIR}/calc_min_max_ispc.o
	)
endif (MSVC)]]

add_custom_command(OUTPUT ${PROJECT_BINARY_DIR}/mandelbrot_ispc.o ${PROJECT_BINARY_DIR}/mandelbrot_ispc.h
		COMMAND ispc -O3 --target=avx2-i32x8 --arch=x86-64 --opt=disable-fma --pic ${CMAKE_CURRENT_SOURCE_DIR}/prog3_mandelbrot_ispc/mandelbrot.ispc
		-h ${PROJECT_BINARY_DIR}/mandelbrot_ispc.h -o ${PROJECT_BINARY_DIR}/mandelbrot_ispc.o
)


add_executable(mandelbrot_ispc
		prog3_mandelbrot_ispc/main.cpp
		prog3_mandelbrot_ispc/mandelbrotSerial.cpp
		common/CycleTimer.h
		common/ppm.cpp
		common/tasksys.cpp
		mandelbrot_ispc.o)
target_include_directories(mandelbrot_ispc PRIVATE ${PROJECT_BINARY_DIR})
