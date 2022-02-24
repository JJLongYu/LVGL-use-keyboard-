# LVGL7.10中键盘（物理按键的移植和使用）
## 前言
由于之前在进行J-WATCH的GUI开发时使用到物理按键，所以考虑到在做界面开发时，在特定情况下会使用到物理按键，所以上传记录一下我的移植的一些方法（**水平有限，仅供参考 ε=ε=ε=(~￣▽￣ )~**）

- 硬件要求：正点原子STM32F07开发板
- LVGL版本：7.10
 
## 1.keypad的移植

 在**lv_port_indev.c**里面修改下列代码
 - 取消下列代码的注释
 
 ![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad1.bmp)
 
 - 在 ``` static void keypad_init(void) ```函数中添加你的按键的初始化代码

 ![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad2.bmp)
 
 - 在``` static uint32_t keypad_get_key(void)```中添加自己的按键响应代码
 
 ![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad3.bmp)
 
 通过以上三步，按键的驱动就已经移植完成，但是要怎么去使用他们去控制我们的控件呢？o(￣▽￣)ｄ
 ## 2.物理按键的使用方法
 - 在**lv_port_indev.c**中会发现以下一个函数``` static bool keypad_read(lv_indev_drv_t * indev_drv, lv_indev_data_t * data)```
  这个函数会告诉我们对应按键的具体操作（LV_KEY_NEXT,LV_KEY_ENTER等等）
  - 创建一个group``` lv_group_t * g = lv_group_create();```来包含需要控制的部件```lv_group_add_obj(g, obj); ```，然后将group与输入设备相关联```lv_indev_set_group(indev, g); ```,其中**indev**是```lv_indev_set_group(indev, g)```的返回值。（具体为indev_keypad，即使上面的1.1的最后一段代码）
 ## 3.添加按键定义
**例子：**

![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad5.bmp)
 case 后的值与`lv_group.h`中的
 ![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad6.bmp)
**一一对应**
## 演示例程

![enter image description here](https://raw.githubusercontent.com/JJLongYu/LVGL-use-keyboard-/main/Picture/keypad7.bmp)
