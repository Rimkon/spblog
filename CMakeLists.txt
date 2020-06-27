
cmake_minimum_required ( VERSION 3.5 )

               project ( my_flesson VERSION 0.0.1$ENV{TRAVIS_BUILD_NUMBER} )

          #find_package (  spdlog  REQUIRED )
          find_package ( Threads  REQUIRED )
        # find_package (   Boost  COMPONENTS unit_test_framework REQUIRED )

    #     find_package ( Boost 1.65.1 REQUIRED)
 # include_directories ( SYSTEM ${Boost_INCLUDE_DIRS})
#target_link_libraries ( myapp ${Boost_LIBRARIES})


    set( BOOST_COMPONENTS_NEEDED serialization )

    # The following verifyies that BOOST_ROOT is set properly.
    if(NOT BOOST_ROOT AND NOT $ENV{BOOST_ROOT} STREQUAL "")
        FILE( TO_CMAKE_PATH $ENV{BOOST_ROOT} BOOST_ROOT )
        if( NOT EXISTS ${BOOST_ROOT} )
            MESSAGE( STATUS  ${BOOST_ROOT} " does not exist. Checking if BOOST_ROOT was a quoted string.." )
            STRING( REPLACE "\"" "" BOOST_ROOT ${BOOST_ROOT} )
            if( EXISTS ${BOOST_ROOT} )
                MESSAGE( STATUS "After removing the quotes " ${BOOST_ROOT} " was now found by CMake" )
            endif( EXISTS ${BOOST_ROOT})
        endif( NOT EXISTS ${BOOST_ROOT} )

    # Save the BOOST_ROOT in the cache
        if( NOT EXISTS ${BOOST_ROOT} )
            MESSAGE( WARNING ${BOOST_ROOT} " does not exist." )
        else(NOT EXISTS ${BOOST_ROOT})
            SET (BOOST_ROOT ${BOOST_ROOT} CACHE STRING "Set the value of BOOST_ROOT to point to the root folder of your boost install." FORCE)
            #SET (BOOST_INCLUDEDIR ${BOOST_ROOT}/Include)
            #SET (BOOST_LIBRARYDIR ${BOOST_ROOT}/lib)
        endif( NOT EXISTS ${BOOST_ROOT} )

    endif(NOT BOOST_ROOT AND NOT $ENV{BOOST_ROOT} STREQUAL "")

    if( WIN32 AND NOT BOOST_ROOT )
        MESSAGE( WARNING "Please set the BOOST_ROOT environment variable." )
    endif( WIN32 AND NOT BOOST_ROOT )

    set(Boost_ADDITIONAL_VERSIONS "1.47" "1.47.0")
    set(Boost_DEBUG ON)
    set(Boost_USE_STATIC_LIBS       OFF)
    set(Boost_USE_MULTITHREADED      ON)
    set(Boost_USE_STATIC_RUNTIME    OFF)
    FIND_PACKAGE(Boost 1.47.0 COMPONENTS ${BOOST_COMPONENTS_NEEDED})
    if(Boost_FOUND)
        MESSAGE( STATUS "Setting up boost." )
        include_directories(${Boost_INCLUDE_DIRS})
        if(Boost_DEBUG)
            MESSAGE( STATUS "BOOST Libraries " ${Boost_LIBRARIES} )
            FOREACH(BOOST_COMPONENT ${BOOST_COMPONENTS_NEEDED})
                STRING( TOUPPER ${BOOST_COMPONENT} BOOST_COMPONENT_UPCASE )
                MESSAGE( STATUS "Boost " ${BOOST_COMPONENT} ": " ${Boost_${BOOST_COMPONENT_UPCASE}_LIBRARY} )
                MESSAGE( STATUS "Boost " ${BOOST_COMPONENT} " Debug: " ${Boost_${BOOST_COMPONENT_UPCASE}_LIBRARY_DEBUG} )
                MESSAGE( STATUS "Boost " ${BOOST_COMPONENT} " Release: " ${Boost_${BOOST_COMPONENT_UPCASE}_LIBRARY_RELEASE} )
            ENDFOREACH(BOOST_COMPONENT)
        endif(Boost_DEBUG)
    endif(Boost_FOUND)




add_executable ( hello_cli   main.cpp )

 set_target_properties ( hello_cli
                         PROPERTIES
                         CXX_STANDARD 17
                         CXX_STANDARD_REQUIRED ON
                         COMPILE_OPTIONS "-Wall;-Wpedantic;-Wextra"     
                        )

 target_link_libraries ( hello_cli
                         Threads::Threads
                       )

               install ( TARGETS  hello_cli
                         RUNTIME DESTINATION  bin )               
                     
set ( CPACK_GENERATOR DEB )                     

set ( CPACK_PACKAGE_VERSION_MAJOR   "${PROJECT_VERSION_MAJOR}" )
set ( CPACK_PACKAGE_VERSION_MINOR   "${PROJECT_VERSION_MINOR}" )
set ( CPACK_PACKAGE_VERSION_PATCH   "${PROJECT_VERSION_PATCH}" )

set ( CPACK_PACKAGE_CONTACT  rassant@gmail.com )

set ( CPACK_DEBIAN_PACKAGE_SHLIBDEPS  ON )
