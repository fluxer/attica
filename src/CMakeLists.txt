configure_file(
    version.h.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/version.h
)
configure_file(
    atticaclient_export.h.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/atticaclient_export.h
)

set(ocsclient_SRCS
    accountbalance.cpp
    accountbalanceparser.cpp
    achievement.cpp
    achievementparser.cpp
    buildservice.cpp
    buildserviceparser.cpp
    buildservicejob.cpp
    buildservicejobparser.cpp
    buildservicejoboutput.cpp
    buildservicejoboutputparser.cpp
    activity.cpp
    activityparser.cpp
    atticabasejob.cpp
    atticautils.cpp
    privatedata.cpp
    privatedataparser.cpp
    category.cpp
    categoryparser.cpp
    comment.cpp
    commentparser.cpp
    config.cpp
    configparser.cpp
    content.cpp
    contentparser.cpp
    deletejob.cpp
    distribution.cpp
    distributionparser.cpp
    downloaddescription.cpp
    downloaditem.cpp
    downloaditemparser.cpp
    event.cpp
    eventparser.cpp
    folder.cpp
    folderparser.cpp
    forum.cpp
    forumparser.cpp
    getjob.cpp
    homepageentry.cpp
    homepagetype.cpp
    homepagetypeparser.cpp
    icon.cpp
    itemjob.cpp
    knowledgebaseentry.cpp
    knowledgebaseentryparser.cpp
    license.cpp
    licenseparser.cpp
    listjob_inst.cpp
    message.cpp
    messageparser.cpp
    metadata.cpp
    parser.cpp
    person.cpp
    personparser.cpp
    platformdependent_v2.cpp
    postfiledata.cpp
    postjob.cpp
    project.cpp
    projectparser.cpp
    putjob.cpp
    remoteaccount.cpp
    remoteaccountparser.cpp
    provider.cpp
    providermanager.cpp
    publisher.cpp
    publisherparser.cpp
    publisherfield.cpp
    publisherfieldparser.cpp
    qtplatformdependent.cpp
    topic.cpp
    topicparser.cpp
)

if (ATTICA_STATIC_BUILD)
    add_library(${ATTICA_LIB_SONAME} STATIC ${ocsclient_SRCS})
    set_target_properties(${ATTICA_LIB_SONAME} PROPERTIES
        VERSION ${CMAKE_LIBATTICA_VERSION_STRING}
        SOVERSION ${CMAKE_LIBATTICA_VERSION_MAJOR}
    )
else (ATTICA_STATIC_BUILD)
    add_library(${ATTICA_LIB_SONAME} SHARED ${ocsclient_SRCS})
    set_target_properties(${ATTICA_LIB_SONAME} PROPERTIES
        VERSION ${CMAKE_LIBATTICA_VERSION_STRING}
        SOVERSION ${CMAKE_LIBATTICA_VERSION_MAJOR}.${CMAKE_LIBATTICA_VERSION_MINOR}
        DEFINE_SYMBOL ATTICA_LIB_MAKEDLL
    )
endif(ATTICA_STATIC_BUILD)

target_link_libraries(${ATTICA_LIB_SONAME}
    ${QT_QTCORE_LIBRARY}
    ${QT_QTXML_LIBRARY}
    ${QT_QTNETWORK_LIBRARY}
)
target_include_directories(${ATTICA_LIB_SONAME}
    INTERFACE $<INSTALL_INTERFACE:${INCLUDE_DESTINATION}>
)

install(
    TARGETS ${ATTICA_LIB_SONAME}
    EXPORT LibAtticaExport
    ARCHIVE DESTINATION ${LIB_DESTINATION}
    LIBRARY DESTINATION ${LIB_DESTINATION}
)

set(ATTICA_HEADERS
    activity.h
    accountbalance.h
    achievement.h
    atticabasejob.h
    ${CMAKE_CURRENT_BINARY_DIR}/atticaclient_export.h
    atticautils.h
    comment.h
    content.h
    category.h
    deletejob.h
    distribution.h
    downloaddescription.h
    downloaditem.h
    event.h
    folder.h
    forum.h
    getjob.h
    homepageentry.h
    homepagetype.h
    icon.h
    itemjob.h
    knowledgebaseentry.h
    listjob.h
    license.h
    message.h
    metadata.h
    person.h
    #interface for external platform plugins
    platformdependent.h
    platformdependent_v2.h
    privatedata.h
    postjob.h
    project.h
    putjob.h
    remoteaccount.h
    buildservice.h
    buildservicejob.h
    buildservicejoboutput.h
    providermanager.h
    provider.h
    publisher.h
    publisherfield.h
    topic.h
)

install(
    FILES
    ${ATTICA_HEADERS}
    ${CMAKE_CURRENT_BINARY_DIR}/version.h
    DESTINATION ${INCLUDE_DESTINATION}/attica
    COMPONENT Devel
)

# Install package config file
if(NOT WIN32)
    configure_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/cmake/libattica.pc.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/lib${ATTICA_LIB_SONAME}.pc
    )

    install(
        FILES ${CMAKE_CURRENT_BINARY_DIR}/lib${ATTICA_LIB_SONAME}.pc
        DESTINATION ${LIB_DESTINATION}/pkgconfig
    )
endif(NOT WIN32)

