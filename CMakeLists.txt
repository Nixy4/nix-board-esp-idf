set(INCLUDE_DIRS "src")
set(REQUIRES_ESP_IDF "")
set(REQUIRES_ESP_REGISTRY "")
set(REQUIRES_PROJECT
  "nix-wrapper-esp-idf"
)

set(REQUIRES ${REQUIRES_ESP_IDF} ${REQUIRES_ESP_REGISTRY} ${REQUIRES_PROJECT})
set(PRIV_REQUIRES "")

set(EMBED_FILES "")

set(SOURCES "")

# Board specific sources
if(IDF_TARGET STREQUAL "esp32s3")
  if(CONFIG_NIX_BOARD_ESP32_BOARD_M5STACK_CORE_S3)
    list(APPEND SOURCES "src/board/m5stack/core-s3.cpp")
  elseif(CONFIG_NIX_BOARD_ESP32_BOARD_M5STACK_CARDPUTER)
    list(APPEND SOURCES "src/board/m5stack/cardputer.cpp")
  elseif(CONFIG_NIX_BOARD_ESP32_BOARD_LILYGO_T_LORA_PAGER)
    list(APPEND SOURCES "src/board/lilygo/t-lora-pager.cpp")
  endif()
elseif(IDF_TARGET STREQUAL "esp32p4")
  if(CONFIG_NIX_BOARD_ESP32_BOARD_M5STACK_TAB5)
    list(APPEND SOURCES "src/board/m5stack/tab5.cpp")
  endif()
endif()

# M5Stack PowerHub 是独立模块/开发板（板级代码），实现尚不完善
# （powerhub.hpp 空单例与 powerhub.cpp 的 PowerHubI2c 脱节），默认不编译。
if(CONFIG_NIX_BOARD_ESP32_BOARD_M5STACK_POWERHUB)
  list(APPEND SOURCES "src/board/m5stack/powerhub.cpp")
endif()

idf_component_register(
  SRCS ${SOURCES}
  INCLUDE_DIRS ${INCLUDE_DIRS}
  REQUIRES ${REQUIRES}
  PRIV_REQUIRES ${PRIV_REQUIRES}
  EMBED_FILES ${EMBED_FILES}
)
