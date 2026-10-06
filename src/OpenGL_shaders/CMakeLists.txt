# 3D render path only: the 2D hardware-accelerated shaders
# (2DLayerPre*/2DSprite*/2DCompositor*) are not part of this build.
set(OPENGL_SHADER_FILES
    3DClearVS.glsl
    3DClearFS.glsl
    3DClearBitmapVS.glsl
    3DClearBitmapFS.glsl
    3DRenderVS.glsl
    3DRenderFS.glsl
    3DFinalPassVS.glsl
    3DFinalPassEdgeFS.glsl
    3DFinalPassFogFS.glsl
)

foreach(SH_INPUT ${OPENGL_SHADER_FILES})
    get_filename_component(SH_NAME ${SH_INPUT} NAME_WE)
    set(SH_OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/${SH_NAME}.h")
    add_custom_command(
        OUTPUT ${SH_OUTPUT}
        DEPENDS ${SH_INPUT}
        COMMAND
            ${CMAKE_COMMAND}
            -D INPUT_FILE=${SH_INPUT}
            -D OUTPUT_FILE=${SH_OUTPUT}
            -D VAR_NAME=${SH_NAME}
            -P "${CMAKE_CURRENT_LIST_DIR}/../../cmake/MakeEmbed.cmake"
        WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
    )
    list(APPEND OPENGL_SHADER_OUT ${SH_OUTPUT})
endforeach()

add_custom_target(OpenGL_shaders ALL DEPENDS ${OPENGL_SHADER_OUT})
add_dependencies(core OpenGL_shaders)
