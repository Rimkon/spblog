
cmake_minimum_required ( VERSION 3.5 )

               project ( my_flesson VERSION 0.0.1$ENV{TRAVIS_BUILD_NUMBER} )

          #find_package (  spdlog  REQUIRED )
          find_package ( Threads  REQUIRED )
        # find_package (   Boost  COMPONENTS unit_test_framework REQUIRED )

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

cpack -G DEB
