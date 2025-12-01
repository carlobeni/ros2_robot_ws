# I. Crear y Registrar un Workspace en ROS 2
## 1. Crear un nuevo workspace

```bash
mkdir -p ~/ros2_robot_ws2/src
cd ~/ros2_robot_ws2
```
## 2. Clonar repositorio en src
```bash
git clone https://github.com/carlobeni/articubot_one.git
```

## 3. Compilacion automatica del workspace con colcon

```bash
colcon build --symlink-install
```

## 4. Cargar el workspace en la terminal actual

```bash
source install/setup.bash
```

## 5. Registrar el workspace en el archivo `.bashrc` (Alternativo al paso 4 con efecto permanente)

Abrir el archivo:

```bash
code ~/.bashrc
```

Agregar al final del archivo:

```bash
source /opt/ros/$ROS_DISTRO/setup.bash
source ~/ros2_robot_ws2/install/setup.bash
```


# 2. Simulacion con Gazebo
Terminal 1: Correr nodo de visualizacion de Gazebo (luego cerrar)
```bash
ros2 launch ros_gz_sim gz_sim.launch.py
```

Terminal 2: Lanzar launch de nodos de simulacion (Crea un nuevo world vacio)
```bash
ros2 launch articubot_one launch_sim.launch.py
```