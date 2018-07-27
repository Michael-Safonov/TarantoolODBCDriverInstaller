cmake_minimum_required (VERSION 3.8)

project(TarantoolODBCDriverInstaller)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/Variables.wxi
               ${CMAKE_CURRENT_SOURCE_DIR}/FilledVariables.wxi @ONLY)

ADD_CUSTOM_TARGET(TarantoolODBCDriverInstaller ALL
				DEPENDS 
				${MsiOutputPathFor64bit}/${MsiNameFor64bit}
				${MsiOutputPathFor32bit}/${MsiNameFor32bit}
				)

ADD_CUSTOM_COMMAND(
		OUTPUT
		${MsiOutputPathFor64bit}/${MsiNameFor64bit}
		DEPENDS
			${TempFilesPathFor64bit}/Features.wixobj
			${TempFilesPathFor64bit}/InstallationComponents.wixobj
			${TempFilesPathFor64bit}/Product.wixobj
			${TempFilesPathFor64bit}/WixUI_CustomMondo.wixobj
        COMMAND ${WixDir}/light.exe 
		-out ${MsiOutputPathFor64bit}/${MsiNameFor64bit}
		-cultures:${CultureCode}
		-dWixUILicenseRtf=${LicenseSourcePath}/${LicenseFile}
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_en-US.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_neutral.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_ru-RU.wxl 
		-sice:ICE80
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		-b ${SearchMsiResoursesPath}
		${TempFilesPathFor64bit}/Features.wixobj
		${TempFilesPathFor64bit}/InstallationComponents.wixobj
		${TempFilesPathFor64bit}/Product.wixobj
		${TempFilesPathFor64bit}/WixUI_CustomMondo.wixobj)

ADD_CUSTOM_COMMAND(
		OUTPUT
		${MsiOutputPathFor32bit}/${MsiNameFor32bit}
		DEPENDS
			${TempFilesPathFor32bit}/Features.wixobj
			${TempFilesPathFor32bit}/InstallationComponents.wixobj
			${TempFilesPathFor32bit}/Product.wixobj
			${TempFilesPathFor32bit}/WixUI_CustomMondo.wixobj
        COMMAND ${WixDir}/light.exe 
		-out ${MsiOutputPathFor32bit}/${MsiNameFor32bit}
		-cultures:${CultureCode}
		-dWixUILicenseRtf=${LicenseSourcePath}/${LicenseFile}
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_en-US.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_neutral.wxl 
		-loc ${CMAKE_CURRENT_SOURCE_DIR}/StandardUI_ru-RU.wxl 
		-sice:ICE80
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		-b ${SearchMsiResoursesPath}
		${TempFilesPathFor32bit}/Features.wixobj
		${TempFilesPathFor32bit}/InstallationComponents.wixobj
		${TempFilesPathFor32bit}/Product.wixobj
		${TempFilesPathFor32bit}/WixUI_CustomMondo.wixobj)

ADD_CUSTOM_COMMAND(
		OUTPUT
			${TempFilesPathFor64bit}/Features.wixobj
			${TempFilesPathFor64bit}/InstallationComponents.wixobj
			${TempFilesPathFor64bit}/Product.wixobj
			${TempFilesPathFor64bit}/WixUI_CustomMondo.wixobj
        COMMAND ${WixDir}/candle.exe 
		-out ${TempFilesPathFor64bit}
		-dTargetPlatform="x64"
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		${CMAKE_CURRENT_SOURCE_DIR}/Features.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/InstallationComponents.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/Product.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/WixUI_CustomMondo.wxs)

ADD_CUSTOM_COMMAND(
		OUTPUT
			${TempFilesPathFor32bit}/Features.wixobj
			${TempFilesPathFor32bit}/InstallationComponents.wixobj
			${TempFilesPathFor32bit}/Product.wixobj
			${TempFilesPathFor32bit}/WixUI_CustomMondo.wixobj
        COMMAND ${WixDir}/candle.exe 
		-out ${TempFilesPathFor32bit}
		-dTargetPlatform="x86"
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUIExtension.dll
		${CMAKE_CURRENT_SOURCE_DIR}/Features.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/InstallationComponents.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/Product.wxs 
		${CMAKE_CURRENT_SOURCE_DIR}/WixUI_CustomMondo.wxs)