```c#
using System;  
using System.Collections;  
using System.Collections.Generic;  
using UnityEngine;  
using UnityEngine.InputSystem;  

public class PlayerController : MonoBehaviour  
{  
    // 用于存储玩家的输入控制器，通过输入系统生成的类管理所有输入操作  
    public PlayerInputControl inputControl;  
    // 玩家的刚体组件，用于处理角色的物理运动  
    private Rigidbody2D rb;  
    // 保存玩家输入的方向向量（例如移动方向）  
    public Vector2 inputDirection;  

    [Header("基本参数")]  
    // 玩家水平移动速度  
    public float speed;  
    // 玩家跳跃的力量  
    public float jumpForce;  

    // 在游戏对象实例化时调用，用于初始化变量和绑定输入事件  
    void Awake()  
    {  
        // 实例化输入控制器  
        inputControl = new PlayerInputControl();  
        // 获取当前游戏对象上的 Rigidbody2D 组件  
        rb = GetComponent<Rigidbody2D>();  
        // 订阅跳跃动作的开始事件，按下跳跃按键时调用 Jump 方法  
        inputControl.Gameplay.Jump.started += Jump;  
    }  

    // 在脚本启用时调用，激活输入控制器  
    void OnEnable()  
    {  
        inputControl.Enable(); // 启用输入控制  
    }  

    // 在脚本禁用时调用，禁用输入控制器  
    void OnDisable()  
    {  
        inputControl.Disable(); // 停用输入控制  
    }  

    // 每帧调用，用于更新玩家输入方向  
    void Update()  
    {  
        // 读取玩家的移动输入（例如键盘 WASD 或手柄摇杆）  
        inputDirection = inputControl.Gameplay.Move.ReadValue<Vector2>();  
    }  

    // 每固定时间间隔调用，用于处理物理相关的更新（推荐用于物理计算）  
    void FixedUpdate()  
    {  
        Move(); // 调用移动逻辑  
    }  

    // 定义玩家移动逻辑  
    void Move()  
    {  
        // 设置玩家的水平速度，同时保留垂直方向的速度（如跳跃和重力）  
        rb.velocity = new Vector2(speed * Time.deltaTime * inputDirection.x, rb.velocity.y);  

        // 获取玩家当前的朝向（通过对象的缩放值判断）  
        int faceDir = (int)transform.localScale.x;  
        
        // 如果玩家向右移动且朝向不为右，则调整朝向为右  
        if (inputDirection.x > 0 && faceDir != 1)  
        {  
            transform.localScale = new Vector3(1, 1, 1); // 设置玩家朝向右  
        }  
        // 如果玩家向左移动且朝向不为左，则调整朝向为左  
        else if (inputDirection.x < 0 && faceDir != -1)  
        {  
            transform.localScale = new Vector3(-1, 1, 1); // 设置玩家朝向左  
        }  
    }  

    // 跳跃逻辑，绑定到 Jump 动作的 started 事件  
    private void Jump(InputAction.CallbackContext context)  
    {  
        // 在角色上施加向上的瞬时力（Impulse 模式）实现跳跃  
        rb.AddForce(transform.up * jumpForce, ForceMode2D.Impulse);  
    }  
}

``` 