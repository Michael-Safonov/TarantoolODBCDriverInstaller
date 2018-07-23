cmake_minimum_required (VERSION 3.8)

project(TarantoolODBCDriverInstaller)

#---------- Find Wix path------------------------------
IF(DEFINED $ENV{WIX_DIR})
  SET(WIX_DIR "$ENV{WIX_DIR}")
ENDIF(DEFINED $ENV{WIX_DIR})

FIND_PATH(WIX_DIR candle.exe
	$ENV{WIX_DIR}/bin
	$ENV{WIX}/bin
	"$ENV{ProgramFiles}/wix/bin"
	"$ENV{ProgramFiles}/Windows Installer */bin")

IF (WIX_DIR)
	MESSAGE(STATUS "Wix found. Path: ${WIX_DIR}")
ELSE (WIX_DIR)
	IF ($ENV{WIX_DIR})
		MESSAGE(FATAL_ERROR "Can not find Wix in $ENV{WIX_DIR}")
	ELSE ($ENV{WIX_DIR})
		MESSAGE(FATAL_ERROR "Wix not found. Please installation wix")
	ENDIF ($ENV{WIX_DIR})
ENDIF (WIX_DIR)
#------------------------------------------------------

#-------- Set cmake variables -------------------------
set(MSI_NAME "TarantoolODBCDriverInstaller.msi")
set(CULTURE_CODE "ru-RU")
set(TEMP_FILES_PATH "${CMAKE_CURRENT_SOURCE_DIR}/obj/wix/")
set(RESOURSE_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
set(MSI_OUTPUT_PATH "${CMAKE_CURRENT_SOURCE_DIR}/bin/wix")

#-----------------------------------------------------


ADD_CUSTOM_TARGET(TarantoolODBCDriverInstaller ALL
				DEPENDS ${MSI_OUTPUT_PATH}/${MSI_NAME})

ADD_CUSTOM_COMMAND(
		OUTPUT ${MSI_OUTPUT_PATH}/${MSI_NAME}
		DEPENDS
			${TEMP_FILES_PATH}/Features.wixobj
			${TEMP_FILES_PATH}/InstallationComponents.wixobj
			${TEMP_FILES_PATH}/Product.wixobj
			${TEMP_FILES_PATH}/WixUI_CustomMondo.wixobj
        COMMAND ${WIX_DIR}/light.exe 
		-out ${MSI_OUTPUT_PATH}/${MSI_NAME}
		-cultures:${CULTURE_CODE}
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_en-US.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_neutral.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_ru-RU.wxl 
		-sice:ICE80 
#		-contentsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindContentsFileListen-US.txt 
#		-outputsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindOutputsFileListen-US.txt 
#		-builtoutputsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindBuiltOutputsFileListen-US.txt 
#		-wixprojectfile ${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller.wixproj
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		-b ${RESOURSE_PATH}
		${TEMP_FILES_PATH}/Features.wixobj
		${TEMP_FILES_PATH}/InstallationComponents.wixobj
		${TEMP_FILES_PATH}/Product.wixobj
		${TEMP_FILES_PATH}/WixUI_CustomMondo.wixobj)

ADD_CUSTOM_COMMAND(
		OUTPUT
			${TEMP_FILES_PATH}/Features.wixobj
			${TEMP_FILES_PATH}/InstallationComponents.wixobj
			${TEMP_FILES_PATH}/Product.wixobj
			${TEMP_FILES_PATH}/WixUI_CustomMondo.wixobj
        COMMAND ${WIX_DIR}/candle.exe 
		-out ${TEMP_FILES_PATH}
		-arch x86
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		${CMAKE_CURRENT_SOURCE_DIR}/Features.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/InstallationComponents.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/Product.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/WixUI_CustomMondo.wxs)