include_directories(${QTINCLUDES} ${TAGLIB_INCLUDES} ${CMAKE_SOURCE_DIR})
set(CANTATA_TAGS_SRCS main.cpp taghelper.cpp tags.cpp filetyperesolver.cpp ../mpd-interface/song.cpp)
set(CANTATA_TAGS_MOC_HDRS taghelper.h)

QT5_WRAP_CPP(CANTATA_TAGS_MOC_SRCS ${CANTATA_TAGS_MOC_HDRS})

if (WIN32)
    set(CMAKE_BUILD_TYPE "Release")
    ADD_EXECUTABLE(cantata-tags WIN32 ${CANTATA_TAGS_SRCS} ${CANTATA_TAGS_MOC_SRCS})
    install(TARGETS cantata-tags DESTINATION ${CMAKE_INSTALL_PREFIX})
elseif (APPLE)
    set(CMAKE_BUILD_TYPE "Release")
    ADD_EXECUTABLE(cantata-tags ${CANTATA_TAGS_SRCS} ${CANTATA_TAGS_MOC_SRCS})
    #install(TARGETS cantata-tags DESTINATION ${MACOSX_BUNDLE_APP_DIR})
else ()
    ADD_EXECUTABLE(cantata-tags ${CANTATA_TAGS_SRCS} ${CANTATA_TAGS_MOC_SRCS})
    install(TARGETS cantata-tags RUNTIME DESTINATION ${LINUX_LIB_DIR}/cantata)
endif ()

add_definitions(-DCANTATA_TAG_SERVER -DCANTATA_NO_UI_FUNCTIONS)
target_link_libraries(cantata-tags ${TAGLIB_LIBRARIES} ${QTGUILIBS} ${QTCORELIBS} ${QTNETWORKLIBS})
if (TAGLIB-EXTRAS_FOUND)
    target_link_libraries(cantata-tags ${TAGLIB-EXTRAS_LIBRARIES})
    include_directories(${TAGLIB-EXTRAS_INCLUDES})
endif ()
