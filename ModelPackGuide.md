# **从模型到模型包——让你的模型加入游戏**

## **原版Minecraft实现拓展**

### **拓展物品模型**

通过物品谓词Damage或者CustomModelData拓展物品模型。

[物品标签damage | 森罗万象](http://sqwatermark.com/resguide/vanilla/model/item_tags.html#damage用法)

[物品标签custom-model-data | 森罗万象 ](http://sqwatermark.com/resguide/vanilla/model/item_tags.html#custom-model-data用法)

#### **通过展示框展示**

在Blockbench中完成模型后，可以点击右上角的“显示调整”，然后在左边的槽位中选择物品展示框，调整模型的相对位置。也可以将模型的位置调整至方块“内部”，这样在屏障方块的配合下，可以让模型拥有1X1X1碰撞箱。

物品展示框拥有可以通过命令获取的“透明”的形式，以减少使用时对模型观感的影响，而新版的荧光展示框还可以避免模型被覆盖时变黑。

可以通过以下命令来获取透明的物品展示框的物品形式，需要使用透明的荧光物品展示框只需要将item_frame替换成glow_item_frame即可：

```mcfunction
give @p minecraft:item_frame{EntityTag:{Invisible:1b}}
```

需要注意的是，透明的物品展示框与普通的物品展示框相比，物品会向方块内部移动1像素（1/16方块），为普通物品展示框设计的位移用在透明物品展示框上有可能会出现意料之外的偏差。这可以在BlockBench的预览界面中进行选择。

#### **通过盔甲架显示物品模型**

盔甲架的头部可以显示物品戴在头部的模型，手部可以显示物品在手部的模型。

利用盔甲架的骑乘可以制作堆叠的模型。

其他生物如村民头部、僵尸手部等也可以展示物品模型。

#### **通过刷怪笼展示（有碰撞箱）**

利用停止旋转的刷怪笼内部生物头戴物品，可以实现类似于方块的展示效果。

```mcfunction
setblock ~ ~ ~ minecraft:spawner{MaxNearbyEntities:0,RequiredPlayerRange:0,SpawnData:{entity:{id:"minecraft:armor_stand",ShowArms:0b,Small:1b,Invisible:1b,Pose:{Head:[30f,0f,0f]},ArmorItems:[{},{},{},{id:"minecraft:potion",Count:1b,tag:{CustomModelData:1,CustomPotionColor:0}}]}}}
```



#### **头部展示**

添加南瓜头的物品模型CustomModelData可以直接穿戴展示。

或者使用命令replaceitem(1.16) item(1.17+)

#### **配合数据包实现原版方块**

[Minecraft 原版模组入门教程 (gitee.io)](https://zhangshenxing.gitee.io/vanillamodtutorial/#方块设计)

或者最简单的方式：

自定义物品模型套入（胡萝卜钓杆）右键检测->视线追踪到某个方块->生成上述的盔甲架/刷怪笼/展示框等等

自定义物品模型套入（盔甲架物品模型带nbt/刷怪蛋物品模型带nbt）->放置

```mcfunction
give @a minecraft:armor_stand{EntityTag:{ArmorItems:[{},{},{},{id:"armor_stand",Count:1b}]}}
```

```mcfunction
give @a minecraft:bat_spawn_egg{EntityTag:{id:"minecraft:armor_stand",ArmorItems:[{},{},{},{id:"armor_stand",Count:1b}]}}
```



### **拓展方块模型**

利用BlockState（方块状态）的多样性，可以拓展有碰撞箱的方块模型。

#### **修改现成的模型**

例如利用按钮可以贴于底面、墙壁的特性，修改为挂灯。

也可以修改一些原版地形没有用到的方块，或者生存中无法合成的方块用于拓展。例如彩色带釉陶瓦（带有4个朝向，共16个方块，如果设计无朝向模型可以设计4*16种），以及生存中不存在的石化橡木台阶（3个状态和2个含水状态）不过这样拓展的模型毕竟数量有限，而且遵循方块的渲染规则()，且除了部分的半透明方块（冰、霜冰、下界传送门、染色玻璃与遮光玻璃、黏液块、蜂蜜块、气泡柱）可以使用半透明纹理外，其他的都受到光照遮蔽的影响，且接受面剔除。

#### **利用未使用的方块状态**

可以使用一些除了调试棒或者命令才能获得的方块作为我们模型的载体。[WIKI](https://minecraft.fandom.com/zh/wiki/Java%E7%89%88%E6%9C%AA%E4%BD%BF%E7%94%A8%E7%89%B9%E6%80%A7#.E6.96.B9.E5.9D.97.E7.8A.B6.E6.80.81.E7.BB.84.E5.90.88)

#### **拼接模型**

拥有多对方块状态的方块可以进行模型的组装。通过调试棒或者命令或者自然更新可以调出需要的模型。值得注意的是，如果利用的方块状态是根据周围方块而发生变化的，通过此方式得到的模型也接受状态更新。也就是，如果旁边有方块发生变化，则自己也会发生变化。

例如原版的蘑菇方块（3种）支持6个方块状态，是由6个面拼接而成的。在周围是本种方块时，更新状态。（例如蘑菇柄，在周围是蘑菇柄时，相连的面变成蘑菇截面贴图）。以此可以制作原版的连接纹理。

### **通过数据包盔甲架动画实现模型动画**

[【Blockbench】插件Animated Java简介](https://www.mcbbs.net/thread-1289868-1-1.html)

使用Blockbench插件Animated Java可以在Blockbench中制作模型动画，继而导出实现动画的数据包。详见教程。



### **通过着色器添加obj模型**

[Godlander/objmc: python script to convert .OBJ files into Minecraft, rendering them in game with a core shader. (github.com)](https://github.com/Godlander/objmc)

原理是导入有限制的模型后，通过着色器对顶点的拉伸实现多面模型。使用工具objmc，详见项目README。



## **利用OptiFine实现拓展**

### **利用CIT拓展物品模型**

[【教程】纯净游戏内添加更多物品 ](https://www.mcbbs.net/thread-782790-1-1.html)

通过编写CIT属性文件，根据nbt(附魔)/物品堆叠/lore/物品名称，替换物品模型。常见的做法是通过物品名称替换模型，这样在铁砧上修改物品名就可以获得物品。



值得注意的是，非方块模型的物品模型支持半透明而方块模型的物品模型仅有（冰、染色玻璃与遮光玻璃、黏液块、蜂蜜块）支持半透明。

### **利用CTM拓展方块模型**

[【PCD】Optifine 进阶技巧: 令方块在特定情况下显示特定模型](https://www.mcbbs.net/thread-1016264-1-1.html)

通过编写CTM属性文件，根据方块实体名称，替换漏斗/酿造台的模型。

值得一提的是，由于漏斗有红石激活的特性，还可以以此来制作一些电器方块。例如风扇。当红石激活时，风扇开始转动；当红石熄灭时，风扇停止转动。

### **利用CEM拓展生物模型**

[【教程】奈子的超杂项材质教程](https://www.mcbbs.net/thread-961666-1-1.html)

[【教程】通过资源包修改实体的模型和动画！(Optifine CEM) | Minecraft | Blockbench](https://www.bilibili.com/video/BV19k4y1978n)