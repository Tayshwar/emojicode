file(GLOB S_SOURCES "*.cpp" "*.c" "*.hpp" "*.h")

list(FILTER S_SOURCES EXCLUDE REGEX ".*utf8proc_data.c")

get_filename_component(MAIN_FILE s.emojic ABSOLUTE)
file(GLOB EMOJIC_DEPEND "*.emojic")

add_library(s STATIC ${S_SOURCES} s.o)
set_property(TARGET s PROPERTY POSITION_INDEPENDENT_CODE ON)
target_compile_options(s PUBLIC -Wall -Wno-unused-result -Wno-missing-braces -pedantic)
add_custom_command(OUTPUT s.o COMMAND emojicodec -p s -o s.o -i interface.emojii --color ${MAIN_FILE} -O -c
        DEPENDS emojicodec ${EMOJIC_DEPEND})
