add_custom_target(aleph_dirhtml_docs ALL
    COMMAND ${Python_EXECUTABLE} -m sphinx-build -M dirhtml ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_BINARY_DIR}
    DEPENDS 
        "src/index.md"
)

if(Aleph_INSTALL)
    include(GNUInstallDirs)
    install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/dirhtml DESTINATION "${CMAKE_INSTALL_DOCDIR}")
endif()