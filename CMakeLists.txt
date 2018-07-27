cmake_minimum_required (VERSION 3.8)

#---------- Find Wix path------------------------------
If(defined $ENV{WIX_DIR})
  set(WixDir "$ENV{WIX_DIR}")
Endif(defined $ENV{WIX_DIR})

find_path(WixDir candle.exe
	$ENV{WIX_DIR}/bin
	$ENV{WIX}/bin
	"$ENV{ProgramFiles}/wix/bin"
	"$ENV{ProgramFiles}/Windows Installer */bin")

If (WixDir)
	message(STATUS "Wix found. Path: ${WixDir}")
Else (WixDir)
	If ($ENV{WIX_DIR})
		message(fatal_error "Can not find Wix in $ENV{WIX_DIR}")
	Else ($ENV{WIX_DIR})
		message(fatal_error "Wix not found. Please installation wix")
	Endif ($ENV{WIX_DIR})
Endif (WixDir)
#------------------------------------------------------


#-------- Set cmake variables -------------------------
set(ProductName "TarantoolODBCDriver")
set(ProductVersion "1.0.1.0")
set(Manufacturer "Mail.ru")

set(SearchMsiResoursesPath "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller/")
set(SearchBootstrapperResoursesPath "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverBootstrapper/")

set(LanguageBuild "RU")

#--------- Set msi variables --------------------------
set(Driver64bitSourcePath "${CMAKE_CURRENT_SOURCE_DIR}/DriverSource")
set(Driver32bitSourcePath "${CMAKE_CURRENT_SOURCE_DIR}/DriverSource/x32")
set(Driver64bitFilename "tnt_odbc.dll")
set(Driver32bitFilename "tnt_odbc.dll")

set(BitmapSourcePath "${CMAKE_CURRENT_SOURCE_DIR}/BitmapSource")
set(BannerBitmapFilename "bannrbmp_tarantool.bmp")
set(DialogBitmapFilename "dlgbmp_tarantool.bmp")
set(AddRemoveProgramsIconFilename "tarantool_icon.ico")

set(LicenseSourcePath "${CMAKE_CURRENT_SOURCE_DIR}/LicenseSource")
set(LicenseFileRu "bsd_license_Ru.rtf")
set(LicenseFileEn "bsd_license_En.rtf")

set(TempFilesPathFor64bit "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller/obj/x64/")
set(TempFilesPathFor32bit "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller/obj/x32/")
set(MsiOutputPathFor64bit "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller/bin/x64")
set(MsiOutputPathFor32bit "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverInstaller/bin/x32")
set(MsiNameFor64bit "TarantoolODBCDriverInstaller_x64.msi")
set(MsiNameFor32bit "TarantoolODBCDriverInstaller_x86.msi")

If(LanguageBuild STREQUAL "RU")
	set(CultureCode "ru-RU")
	set(LicenseFile ${LicenseFileRu})
Else(LanguageBuild STREQUAL "RU")
	set(CultureCode "en-US")
	set(LicenseFile ${LicenseFileEn})
Endif(LanguageBuild STREQUAL "RU")

#---------- Set bootstrapper variables -------------------
set(64bitMsiPath "${MsiOutputPathFor64bit}/${MsiNameFor64bit}")
set(32bitMsiPath "${MsiOutputPathFor32bit}/${MsiNameFor32bit}")

set(BootstrapperApplicationPath "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverBootstrapper/bin")
set(BootstrapperApplicationName "TarantoolSetup.exe")
set(TempFilePathForBootstrapper "${CMAKE_CURRENT_SOURCE_DIR}/TarantoolODBCDriverBootstrapper/obj/")

add_subdirectory(TarantoolODBCDriverInstaller)
add_subdirectory(TarantoolODBCDriverBootstrapper)
add_dependencies(TarantoolODBCDriverBootstrapper TarantoolODBCDriverInstaller)