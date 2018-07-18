cmake_minimum_required (VERSION 3.8)

project(TarantoolODBCDriverInstaller)

#-------------- finding wix directory------------------
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
#-----------------------------------------------

ADD_CUSTOM_TARGET(TarantoolODBCDriverInstaller ALL
				DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/bin/TarantoolODBCDriverInstaller.msi)

ADD_CUSTOM_COMMAND(
		OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/bin/TarantoolODBCDriverInstaller.msi
		DEPENDS
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Features.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/InstallationComponents.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Product.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/WixUI_CustomMondo.wixobj
        COMMAND ${WIX_DIR}/light.exe 
		-out ${CMAKE_CURRENT_SOURCE_DIR}/bin/TarantoolODBCDriverInstaller.msi
		-cultures:en-US 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_en-US.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_neutral.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_ru-RU.wxl 
		-sice:ICE80 
		-contentsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindContentsFileListen-US.txt 
		-outputsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindOutputsFileListen-US.txt 
		-builtoutputsfile ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/TarantoolODBCDriverInstaller.wixproj.BindBuiltOutputsFileListen-US.txt 
		-wixprojectfile ${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller.wixproj
		${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Features.wixobj
		${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/InstallationComponents.wixobj
		${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Product.wixobj
		${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/WixUI_CustomMondo.wixobj
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		-b ${CMAKE_CURRENT_SOURCE_DIR})

ADD_CUSTOM_COMMAND(
		OUTPUT
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Features.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/InstallationComponents.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/Product.wixobj
			${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/WixUI_CustomMondo.wixobj
        COMMAND ${WIX_DIR}/candle.exe 
		-out ${CMAKE_CURRENT_SOURCE_DIR}/obj/Debug/
		-arch x86
		${CMAKE_CURRENT_SOURCE_DIR}/Features.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/InstallationComponents.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/Product.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/WixUI_CustomMondo.wxs
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll)