cmake_minimum_required(VERSION 3.5)
project(more_interfaces)

# Encontra os pacotes necessários
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rosidl_default_generators REQUIRED)

# 1. GERA A INTERFACE (do Passo 5)
# Este bloco deve vir ANTES de adicionar o executável.
set(msg_files "msg/AddressBook.msg")
rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
)

# 2. ADICIONA O EXECUTÁVEL (do Passo 7)
add_executable(publish_address_book src/publish_address_book.cpp)
ament_target_dependencies(publish_address_book rclcpp)

# 3. ENCONTRA O ALVO DA INTERFACE GERADA (do Passo 8)
# Este comando agora encontrará o alvo criado no passo 1.
rosidl_get_typesupport_target(cpp_typesupport_target
  ${PROJECT_NAME} rosidl_typesupport_cpp)

# 4. VINCULA A INTERFACE AO EXECUTÁVEL (do Passo 8)
target_link_libraries(publish_address_book
  "${cpp_typesupport_target}")

# Instala o executável
install(TARGETS
  publish_address_book
  DESTINATION lib/${PROJECT_NAME}
)

# Instala as interfaces e finaliza o pacote
ament_export_dependencies(rosidl_default_runtime)
ament_package()