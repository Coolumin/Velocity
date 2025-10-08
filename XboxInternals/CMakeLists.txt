# XboxInternals library
set(_lib_type "STATIC")
if(BUILD_XBOXINTERNALS_SHARED)
  set(_lib_type "SHARED")
endif()

add_library(XboxInternals ${_lib_type}
  Account/Account.cpp
  Account/AccountHelpers.cpp
  AvatarAsset/AssetHelpers.cpp
  AvatarAsset/AvatarAsset.cpp
  AvatarAsset/Ytgr.cpp
  Cryptography/XeCrypt.cpp
  Cryptography/XeKeys.cpp
  Disc/Gdfx.cpp
  Disc/ISO.cpp
  Disc/Svod.cpp
  Fatx/FatxDrive.cpp
  Fatx/FatxDriveDetection.cpp
  Fatx/XContentDevice.cpp
  Fatx/XContentDeviceItem.cpp
  Fatx/XContentDeviceProfile.cpp
  Fatx/XContentDeviceSharedItem.cpp
  Fatx/XContentDeviceTitle.cpp
  Gpd/AvatarAwardGpd.cpp
  Gpd/DashboardGpd.cpp
  Gpd/GameGpd.cpp
  Gpd/GpdBase.cpp
  Gpd/Xdbf.cpp
  Gpd/XdbfHelpers.cpp
  IO/BaseIO.cpp
  IO/DeviceIO.cpp
  IO/FatxIndexableMultiFileIO.cpp
  IO/FatxIO.cpp
  IO/FileIO.cpp
  IO/IndexableMultiFileIO.cpp
  IO/IsoIO.cpp
  IO/JoinedMultiFileIO.cpp
  IO/LocalIndexableMultiFileIO.cpp
  IO/MemoryIO.cpp
  IO/StfsIO.cpp
  IO/SvodIO.cpp
  IO/XexAesIO.cpp
  IO/XexBaseIO.cpp
  IO/XexZeroBasedCompressionIO.cpp
  Stfs/IXContentHeader.cpp
  Stfs/StfsDefinitions.cpp
  Stfs/StfsPackage.cpp
  Stfs/XContentHeader.cpp
  Xex/Xex.cpp
  Xex/Xuiz.cpp
)

target_include_directories(XboxInternals
  PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/..
)

# Export macro for Windows DLLs
target_compile_definitions(XboxInternals
  PUBLIC
    $<$<BOOL:${BUILD_XBOXINTERNALS_SHARED}>:XBOXINTERNALS_LIBRARY>
)

# Link to Qt and our custom Botan target
target_link_libraries(XboxInternals
  PUBLIC
    Qt6::Core
    Qt6::Xml
    BotanAmalgamation
)

# Optional second build as static if requested
if(BUILD_XBOXINTERNALS_STATIC)
  add_library(XboxInternalsStatic STATIC EXCLUDE_FROM_ALL
    $<TARGET_OBJECTS:XboxInternals>
  )
  target_link_libraries(XboxInternalsStatic PUBLIC Qt6::Core Qt6::Xml BotanAmalgamation)
  target_include_directories(XboxInternalsStatic PUBLIC ${CMAKE_CURRENT_LIST_DIR} ${CMAKE_CURRENT_LIST_DIR}/..)
endif()