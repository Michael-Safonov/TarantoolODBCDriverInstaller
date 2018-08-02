cmake_minimum_required (VERSION 3.8)

project(TarantoolODBCDriverBootstrapper)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/Variables.wxi
               ${CMAKE_CURRENT_SOURCE_DIR}/FilledVariables.wxi @ONLY)

ADD_CUSTOM_TARGET(TarantoolODBCDriverBootstrapper ALL
				DEPENDS ${BootstrapperApplicationPath}/${BootstrapperApplicationName})

ADD_CUSTOM_COMMAND(
		OUTPUT
		${BootstrapperApplicationPath}/${BootstrapperApplicationName}
		DEPENDS
		${TempFilePathForBootstrapper}/Bundle.wixobj
		COMMAND ${WixDir}/light.exe 
		-out ${BootstrapperApplicationPath}/${BootstrapperApplicationName} 
		-b ${SearchBootstrapperResoursesPath}
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixBalExtension.dll
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUtilExtension.dll
		${TempFilePathForBootstrapper}/Bundle.wixobj)

ADD_CUSTOM_COMMAND(
		OUTPUT
		${TempFilePathForBootstrapper}/Bundle.wixobj
		COMMAND ${WixDir}/candle.exe
		-out ${TempFilePathForBootstrapper}
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixBalExtension.dll
		-ext ${CMAKE_CURRENT_SOURCE_DIR}/Lib/WixUtilExtension.dll
		${CMAKE_CURRENT_SOURCE_DIR}/Bundle.wxs)
