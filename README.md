# I. Crear y Registrar un Workspace en ROS 2


## 1. Compilacion automatica del workspace con colcon

```bash
colcon build --symlink-install
```

## 2. Cargar el workspace en la terminal actual

```bash
source install/setup.bash
```

## 3. Registrar el workspace en el archivo `.bashrc` (Alternativo al paso 2 con efecto permanente)

Abrir el archivo:

```bash
code ~/.bashrc
```

Agregar al final del archivo:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
source ~/ros2_robot_ws2/install/setup.bash
```


## 2. Simulacion con Gazebo
Terminal1: Correr nodo de visualizacion de Gazebo (luego cerrar)
```bash
ros2 launch ros_gz_sim gz_sim.launch.py
```

Terminal2: Lanzar launch de nodos de simulacion (Crea un nuevo world vacio)
```bash
ros2 launch articubot_one launch_sim.launch.py
```