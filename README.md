import pythreejs as p3js
from IPython.display import display
import numpy as np

# 创建一个场景
scene = p3js.Scene()

# 创建一个地板
floor = p3js.Mesh(
    p3js.PlaneGeometry(width=20, height=20),
    p3js.MeshStandardMaterial(color='gray', side=p3js.DoubleSide)
)
floor.rotation = [np.pi / 2, 0, 0]  # 将平面旋转成水平面
scene.add(floor)

# 创建墙壁
def create_wall(x, y, z, width, height):
    wall = p3js.Mesh(
        p3js.PlaneGeometry(width=width, height=height),
        p3js.MeshStandardMaterial(color='lightgrey', side=p3js.DoubleSide)
    )
    wall.position = [x, y, z]
    return wall

scene.add(create_wall(0, 5, -10, 20, 10))  # 后墙
scene.add(create_wall(-10, 5, 0, 20, 10).rotateY(np.pi / 2))  # 左墙
scene.add(create_wall(10, 5, 0, 20, 10).rotateY(-np.pi / 2))  # 右墙

# 创建桌子和椅子
def create_table(x, y, z):
    table = p3js.Mesh(
        p3js.BoxGeometry(3, 0.1, 1.5),
        p3js.MeshStandardMaterial(color='saddlebrown')
    )
    table.position = [x, y, z]
    return table

def create_chair(x, y, z):
    chair_seat = p3js.Mesh(
        p3js.BoxGeometry(1, 0.1, 1),
        p3js.MeshStandardMaterial(color='darkorange')
    )
    chair_back = p3js.Mesh(
        p3js.BoxGeometry(1, 0.6, 0.1),
        p3js.MeshStandardMaterial(color='darkorange')
    )
    chair_back.position = [0, 0.3, -0.45]
    chair = p3js.Group(children=[chair_seat, chair_back])
    chair.position = [x, y, z]
    return chair

for i in range(5):
    scene.add(create_table(i * 4 - 8, 1, -3))
    scene.add(create_chair(i * 4 - 8, 0.5, -1.5))

# 创建光源
light = p3js.DirectionalLight(position=[10, 10, 10], intensity=1)
scene.add(light)

# 创建摄像机
camera = p3js.PerspectiveCamera(position=[10, 10, 20], fov=20)
controller = p3js.OrbitControls(controlling=camera)

# 创建渲染器
renderer = p3js.Renderer(camera=camera, scene=scene, controls=[controller], width=800, height=600)

# 显示场景
display(renderer)
