macro(ATTICA_TESTS)
    foreach(_testname ${ARGN})
        add_executable(${_testname} ${_testname}.cpp)
        target_link_libraries(${_testname} ${QT_QTTEST_LIBRARY} ${QT_QTCORE_LIBRARY} ${ATTICA_LIB_SONAME})
        add_test(${_testname} ${CMAKE_CURRENT_BINARY_DIR}/${_testname})
    endforeach(_testname)
endmacro()

ATTICA_TESTS(
    # attributestest
    configtest
    persontest
    providertest
)
