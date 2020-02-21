project(projecttest)

set(projecttest_SRCS
    projecttest.cpp
    main.cpp
    editproject.ui
)

add_executable(projecttest ${projecttest_SRCS})

target_link_libraries(projecttest
    ${QT_QTCORE_LIBRARY}
    ${QT_QTGUI_LIBRARY}
    ${ATTICA_LIB_SONAME}
)
