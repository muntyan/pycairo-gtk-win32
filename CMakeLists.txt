cmake_minimum_required(VERSION 3.0)

set(PythonLibs_FIND_VERSION TRUE)
set(PythonLibs_FIND_VERSION_MAJOR 2)
find_package(PythonLibs REQUIRED)

include_directories(
    ${PYTHON_INCLUDE_DIR}
    ${GTK_DIR}/include
    ${GTK_DIR}/include/cairo
)
link_directories(${GTK_DIR}/lib)

set(MOO_PYTHON_LIB_DIR ${CMAKE_INSTALL_PREFIX}/bin/Lib/site-packages)

add_library(_cairo MODULE 
    src/cairomodule.c
    src/context.c
    src/font.c
    src/matrix.c
    src/path.c
    src/pattern.c
    src/surface.c
    src/_cairo.pyd.def
)

target_compile_definitions(_cairo PRIVATE HAVE_CONFIG_H=1)
target_link_libraries(_cairo cairo ${PYTHON_LIBRARY})
set_target_properties(_cairo PROPERTIES
    PROJECT_LABEL pycairo
    PREFIX ""
    SUFFIX ".pyd"
    VERSION "1.8.10")
install(TARGETS _cairo DESTINATION ${MOO_PYTHON_LIB_DIR}/cairo)
install(FILES src/__init__.py DESTINATION ${MOO_PYTHON_LIB_DIR}/cairo)
install(FILES src/pycairo.h DESTINATION include/pycairo)
