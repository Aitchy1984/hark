# CMakeLists.txt
#
# Wireshark - Network traffic analyzer
# By Gerald Combs <gerald@wireshark.org>
# Copyright 1998 Gerald Combs
#
# SPDX-License-Identifier: GPL-2.0-or-later
#

include(WiresharkPlugin)

# Plugin name and version info (major minor micro extra)
set_module_info(g729 0 1 0 0)

set(CODEC_SRC
	G729decode.c
)

set(PLUGIN_FILES
	plugin.c
	${CODEC_SRC}
)

set_source_files_properties(
	${PLUGIN_FILES}
	PROPERTIES
	COMPILE_FLAGS "${WERROR_COMMON_FLAGS}"
)

register_codec_files(plugin.c 1
	"G.729"
	${CODEC_SRC}
)

add_wireshark_codec_plugin_library(g729)

target_include_directories(g729 PRIVATE ${CMAKE_SOURCE_DIR}/codecs)

target_link_libraries(g729 wsutil ${BCG729_LIBRARIES})

target_include_directories(g729 SYSTEM PRIVATE ${BCG729_INCLUDE_DIRS})

install_plugin(g729 codecs)

file(GLOB CODEC_HEADERS RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" "*.h")
CHECKAPI(
	NAME
	  g729
	SWITCHES
	SOURCES
	  ${CODEC_SRC}
	  ${CODEC_HEADERS}
)

#
# Editor modelines  -  https://www.wireshark.org/tools/modelines.html
#
# Local variables:
# c-basic-offset: 8
# tab-width: 8
# indent-tabs-mode: t
# End:
#
# vi: set shiftwidth=8 tabstop=8 noexpandtab:
# :indentSize=8:tabSize=8:noTabs=false:
#
