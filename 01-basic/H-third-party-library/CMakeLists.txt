cmake_minimum_required(VERSION 3.5)


project(third_party_include)

find_package(Boost 1.46.1 REQUIRED COMPONENTS filesystem system)

if(Boost_FOUND)
    message("boost found!")
else()
    message("boost can not be found!")
endif()

add_executable(third_party_include main.cpp)


target_link_libraries(third_party_include
    PRIVATE
        Boost::filesystem
)
