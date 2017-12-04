---
title: GameManager
date: 2017-10-03 16:20:41
tags: unity
---
## 静态管理类的初始化

```csharp
    static public GameManager gm;

    gm=GetComponent<GameManager>();
```

## 通过tag获取GameObject

```csharp
    player=GameObject.FindGameObjectWithTag("Player");
```

## 获取绑定在对象上的脚本

```csharp
    private PlayerHealth playerHealth;
    playerHealth=player.GetComponent<PlayerHealth>();
```

## 获取键盘/鼠标输入

```csharp
    float h=Input.GetAxisRaw("Horizontal");//水平输入
    float v=Input.GetAxisRaw("Vertical");//竖直输入

    float rv=rotateSpeed*Input.GetAxis("Mouse X");
    float rh=rotateSpeed*Input.GetAxis("Mouse Y");

    //兼容电脑和移动端
    float h=CrossPlatformInputManager.GetAxisRaw('Horizontal');//水平输入
    float h=CrossPlatformInputManager.GetAxisRaw('Vertical');//垂直输入
```

## 对象的方向

```csharp
    Vector3.forward//正前方
    Vector3.right//右
    Vector3.up//上
```

## 对象旋转

```csharp
    transform.Rotate(0,rv,0);//根据水平方向的平移旋转

    //摄像机的旋转 欧拉角
    myCamera.transform.localEulerAngles=new Vector3(mouseRotateX,0.0f,0.0f);
```

## 加载场景/退出游戏

```csharp
    SenceManager.loadScene('GamePlay');
    Application.Quit();
```

## 禁用鼠标光标

```csharp
    Cursor.visiable=false;
```