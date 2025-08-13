# CMakeLists.txt at root
set(COMPONENT_SRCS
    "Sensors/sensor.c"
    "Display/display.c"
)

set(COMPONENT_ADD_INCLUDEDIRS
    "Sensors"
    "Display"
)

# Register all components in this directory
idf_component_register(
    SRCS ${COMPONENT_SRCS}
    INCLUDE_DIRS ${COMPONENT_ADD_INCLUDEDIRS}
)
