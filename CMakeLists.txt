cmake_minimum_required(VERSION 3.7 FATAL_ERROR)

set (CMAKE_C_COMPILER cl.exe)
set (CMAKE_CXX_COMPILER cl.exe)

project(tinyflow LANGUAGES C CXX)

set (LUA_INCDIR "${CMAKE_CURRENT_SOURCE_DIR}/lua")
set (LUALIB "lua")
set (LUA "luac")
set (LUA_LIBDIR "${CMAKE_CURRENT_BINARY_PATH}/lua")

#if(MSVC)
#  foreach(flag_var
#        CMAKE_CXX_FLAGS CMAKE_CXX_FLAGS_DEBUG CMAKE_CXX_FLAGS_RELEASE
#        CMAKE_CXX_FLAGS_MINSIZEREL CMAKE_CXX_FLAGS_RELWITHDEBINFO)
#    if(${flag_var} MATCHES "/MD")
#      string(REGEX REPLACE "/MD" "/MT" ${flag_var} "${${flag_var}}")
#    endif(${flag_var} MATCHES "/MD")
#  endforeach(flag_var)
#  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHsc")
#endif(MSVC)

add_subdirectory(lua)
add_subdirectory(nnvm)
add_subdirectory(dmlc-core)
add_subdirectory(torch7)

add_library(libtinyflow SHARED "")

target_sources(libtinyflow
  PRIVATE
    src/c_api.cc
    src/op_nn.cc
    src/op_special.cc
    src/session.cc
    src/rtc/op_fusion.cc
    src/torch/op_nn_torch.cc
    src/torch/op_special_torch.cc
    src/torch/op_tensor_torch.cc
    src/tinyflow.def
)

include_directories("${PROJECT_SOURCE_DIR}/include")
include_directories("${PROJECT_SOURCE_DIR}/nnvm/include")
include_directories("${PROJECT_SOURCE_DIR}/dmlc-core/include")
include_directories("${PROJECT_SOURCE_DIR}/lua")
include_directories("${PROJECT_SOURCE_DIR}/torch7/lib/luaT")

#target_link_libraries(libtinyflow "lua/lua")
#target_link_libraries(libtinyflow "nnvm/nnvm")
target_link_libraries(libtinyflow lua)
target_link_libraries(libtinyflow nnvm)
