cmake_minimum_required(VERSION 2.8)
project(COLLADA2GLTF)

set(WITH_IN_SOURCE_BUILD "ON")
set(COLLADA2GLTF_BINARY_DIR, COLLADA2GLTF_SOURCE_DIR)
set(BUILD_SHARED_LIBS OFF)

if (NOT WIN32)
    set(CMAKE_FIND_LIBRARY_SUFFIXES .a .dylib)
endif()

include_directories(${COLLADA2GLTF_SOURCE_DIR}/GLTF)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/shaders)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/helpers)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/convert)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/dependencies/rapidjson/include/rapidjson)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/dependencies/OpenCOLLADA/COLLADAFramework/include)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/dependencies/OpenCOLLADA/COLLADABaseUtils/include)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/dependencies/OpenCOLLADA/COLLADASaxFrameworkLoader/include)
include_directories(${COLLADA2GLTF_SOURCE_DIR}/dependencies/OpenCOLLADA/GeneratedSaxParser/include)

if (NOT WIN32)
    find_package(PNG REQUIRED)
    include_directories(${PNG_INCLUDE_DIR})
endif()

link_directories(${COLLADA2GLTF_BINARY_DIR}/lib)

if (WIN32)
    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
    add_definitions(-DWIN32)
    add_definitions(-EHsc)
endif()

add_subdirectory(dependencies/OpenCOLLADA)

add_executable(collada2gltf main.cpp 
    COLLADA2GLTFWriter.cpp
    GLTFConverterContext.cpp
    GLTF/JSONArray.cpp
    GLTF/JSONNumber.cpp
    GLTF/JSONObject.cpp
    GLTF/JSONString.cpp
    GLTF/JSONValue.cpp
    GLTF/GLTFAnimation.cpp
    GLTF/GLTFMeshAttribute.cpp
    GLTF/GLTFBuffer.cpp
    GLTF/GLTFEffect.cpp
    GLTF/GLTFIndices.cpp
    GLTF/GLTFMesh.cpp
    GLTF/GLTFPrimitive.cpp
    GLTF/GLTFUtils.cpp
    GLTF/GLTFWriter.cpp
    COLLADA2GLTFWriter.h
    GLTF-OpenCOLLADA.h
    GLTF/GLTF.h
    GLTF/GLTFTypesAndConstants.h
    GLTFConverterContext.h
    GLTF/JSONArray.h
    GLTF/JSONNumber.h
    GLTF/JSONObject.h
    GLTF/JSONString.h
    GLTF/JSONValue.h
    GLTF/GLTFAnimation.h
    GLTF/GLTFMeshAttribute.h
    GLTF/GLTFBuffer.h
    GLTF/GLTFEffect.h
    GLTF/GLTFIndices.h
    GLTF/GLTFMesh.h
    GLTF/GLTFPrimitive.h
    GLTF/GLTFUtils.h
    GLTF/GLTFWriter.h
    GLTF/GLTFExtraDataHandler.h
    GLTF/GLTFExtraDataHandler.cpp
    shaders/commonProfileShaders.h
    shaders/commonProfileShaders.cpp
    helpers/geometryHelpers.h
    helpers/geometryHelpers.cpp
    helpers/mathHelpers.h
    helpers/mathHelpers.cpp
    convert/meshConverter.cpp
    convert/meshConverter.h
    convert/animationConverter.cpp
    convert/animationConverter.h)

if (WIN32)
target_link_libraries (collada2gltf GeneratedSaxParser_static OpenCOLLADABaseUtils_static UTF_static ftoa_static MathMLSolver_static OpenCOLLADASaxFrameworkLoader_static OpenCOLLADAFramework_static buffer_static)
else ()
target_link_libraries (collada2gltf GeneratedSaxParser_static OpenCOLLADABaseUtils_static UTF_static ftoa_static MathMLSolver_static OpenCOLLADASaxFrameworkLoader_static OpenCOLLADAFramework_static buffer_static ${PNG_LIBRARY} z)
endif()