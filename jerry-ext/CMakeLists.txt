# Copyright JS Foundation and other contributors, http://js.foundation
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required (VERSION 2.8.12)
set(JERRY_EXT_NAME jerry-ext)
project (${JERRY_EXT_NAME} C)

# Include directories
set(INCLUDE_EXT_PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")
set(INCLUDE_EXT_PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/common")

set(INCLUDE_EXT_PUBLIC ${INCLUDE_EXT_PUBLIC} PARENT_SCOPE) # for jerry-port

if(FEATURE_INIT_FINI)
  set(DEFINES_EXT ${DEFINES_EXT} ENABLE_INIT_FINI)
endif()

# Source directories
file(GLOB SOURCE_EXT_ARG        arg/*.c)
file(GLOB SOURCE_EXT_COMMON     common/*.c)
file(GLOB SOURCE_EXT_DEBUGGER   debugger/*.c)
file(GLOB SOURCE_EXT_HANDLER    handler/*.c)
file(GLOB SOURCE_EXT_MODULE     module/*.c)

set(SOURCE_EXT
    ${SOURCE_EXT_ARG}
    ${SOURCE_EXT_COMMON}
    ${SOURCE_EXT_DEBUGGER}
    ${SOURCE_EXT_HANDLER}
    ${SOURCE_EXT_MODULE})

add_library(${JERRY_EXT_NAME} ${SOURCE_EXT})

target_include_directories(${JERRY_EXT_NAME} PUBLIC ${INCLUDE_EXT_PUBLIC})
target_include_directories(${JERRY_EXT_NAME} PRIVATE ${INCLUDE_EXT_PRIVATE})
target_compile_definitions(${JERRY_EXT_NAME} PUBLIC ${DEFINES_EXT})
target_link_libraries(${JERRY_EXT_NAME} jerry-core)

install(TARGETS ${JERRY_EXT_NAME} DESTINATION lib)
install(DIRECTORY ${INCLUDE_EXT_PUBLIC}/ DESTINATION include)
