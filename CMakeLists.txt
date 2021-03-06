CMAKE_MINIMUM_REQUIRED (VERSION 2.8.3)

PROJECT (GLYPHICONS)
FIND_PROGRAM(RCC_PATH rcc)

# Download
SET (ICONS_ZIPFILE glyphicons_free.zip)
IF (NOT EXISTS ${ICONS_ZIPFILE})
	FILE (DOWNLOAD http://glyphicons.com/files/${ICONS_ZIPFILE} ${CMAKE_BINARY_DIR}/${ICONS_ZIPFILE}
		SHOW_PROGRESS)
ENDIF ()

# Unzip
IF (NOT EXISTS glyphicons_free)
	IF(EXISTS ${ICONS_ZIPFILE})
		EXECUTE_PROCESS(COMMAND unzip ${ICONS_ZIPFILE} WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
	ELSE ()
		MESSAGE(FATAL_ERROR "Aborting: zipfile not found!")
	ENDIF() # EXISTS ${ICONS_ZIPFILE}
ENDIF () # NOT EXISTS

IF (EXISTS glyphicons_free)
	# Move to a better location
	IF (NOT EXISTS icons)
		EXECUTE_PROCESS(COMMAND mv glyphicons_free/glyphicons/png icons WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
	ENDIF() # condition
	IF (NOT EXISTS ${CMAKE_BINARY_DIR}/icons.qrc)
		CONFIGURE_FILE (${CMAKE_SOURCE_DIR}/icons.qrc ${CMAKE_BINARY_DIR}/icons.qrc COPYONLY)
	ENDIF () # NOT EXISTS ${CMAKE_BINARY_DIR}/icons.qrc

	# Compile
	EXECUTE_PROCESS(COMMAND ${RCC_PATH} icons.qrc -o icons.rcc WORKING_DIRECTORY ${CMAKE_BINARY_DIR})
	
ELSE ()
	MESSAGE(FATAL_ERROR "Aborting: icons dir not found!")
ENDIF ()