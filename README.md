# I. Crear y Registrar un Workspace en ROS 2

## 1. Crear un nuevo workspace

```bash
mkdir -p ~/ros2_robot_ws2/src
cd ~/ros2_robot_ws2
```

## 2. Compilacion automatica del workspace con colcon

```bash
colcon build --symlink-install
```

## 3. Cargar el workspace en la terminal actual

```bash
source install/setup.bash
```

## 4. Registrar el workspace en el archivo `.bashrc`

Abrir el archivo:

```bash
code ~/.bashrc
```

Agregar al final del archivo:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
source ~/ros2_namews_ws/install/setup.bash
```

Recargar cambios:

```bash
source ~/.bashrc
```

# II. Crear package con launch
## 1. Crear package con cmake
```bash
ros2 pkg create --build-type ament_cmake name_package
```
## 2. Crear directorio lanch y script name.launch.py

```bash
mkdir -p ~/ros2_test/src/name_package/launch
touch ~/ros2_test/src/name_package/launch/name.launch.py
```

## 3. Completar name.launch.py

```python
import launch
from launch_ros.actions import Node

def generate_launch_description():
    return launch.LaunchDescription([
        # Nodo de prueba
        Node(
            package='demo_nodes_cpp',
            executable='listener',
        )
        # Template para lanzar nodo
        Node(
            package='name_package',
            executable='name_node',
            output='screen',
            name='name_node',
            parameters=[{'name': 'name'}]
        )
    ])
```
## 4. Actualizar CMakeLists.txt del package
Agregar directorio lanch al CMakeLists.txt para ser compilado por colcon

```cmake
install(DIRECTORY launch DESTINATION share/${PROJECT_NAME})
```

## 5. Actualizar package.xml
Agregar las dependencias necesarias para compilar name_package como otros packages

```xml
<exec_depend>demo_nodes_cpp</exec_depend>
```

## 6. Compilar package

```bash
colcon build --symlink-install
```

# III. Sincronizar workspace con con otra maquina con github

# IV. Frames y tf2
Las transformaciones de posicionamiento (x,y,z), giro (roll, pitch, yaw) sobre cada sistema de referencia o frame se realizar con la libreria tf2 incorporable a los nodos

Las trasformaciones son publicadas en los topicos /tf y /tf_static por el nodo que emplea transformaciones llamdo nodo broadcaster y pueden ser leidas por los nodos listener si emplean la informacion de las transformaciones.

Ejemplo para establecer child_frame relativo a parent_frame con comandos
```bash
ros2 run tf2_ros static_transform_publisher x y z yaw pitch roll parent_frame child_frame
```

# V. Visualizacion con RVIZ
Correr nodo de visualizacion de RVIZ
```bash
ros2 run rviz2 rviz2
```
Ejemplo de incorporacion de dos frames al word
```bash
# TF: world -> robot_1
ros2 run tf2_ros static_transform_publisher \
  --x 2 --y 1 --z 0 \
  --roll 0 --pitch 0 --yaw 0.785 \
  --frame-id world \
  --child-frame-id robot_1 &

# TF: robot_1 -> robot_2
ros2 run tf2_ros static_transform_publisher \
  --x 1 --y 0 --z 0 \
  --roll 0 --pitch 0 --yaw 0 \
  --frame-id robot_1 \
  --child-frame-id robot_2 &

```
Verificar isntalacion de xacro y joint_state_publisher
```bash
dpkg -l | grep ros-rolling-xacro
dpkg -l | grep ros-rolling-joint-state-publisher-gui
ros2 pkg list | grep xacro
ros2 pkg list | grep joint_state_publisher_gui
apt-cache search xacro | grep rolling
apt-cache search joint-state-publisher-gui | grep rolling
```
# VI.