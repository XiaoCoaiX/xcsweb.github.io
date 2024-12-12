```c#
using System;
using System.Collections;
using System.Collections.Generic;
using System.Runtime.InteropServices.ComTypes;
using UnityEditor.Rendering;
using UnityEngine;
using UnityEngine.InputSystem;
public class PlayerController : MonoBehaviour
{
    public PlayerInputControl inputControl;  // 用于存储玩家的输入控制器
    private Rigidbody2D rb; // 用于存储玩家的刚体组件，用于物理运动
    private Vector2 inputDirection; // 用于存储玩家输入的方向向量
    private CapsuleCollider2D coll;
    private PhysicsCheck physicsCheck; //隔壁的PhysicsCheck类

    private Vector2 originalOffset;
    private Vector2 originalSize;
    [Header("状态显示")]
    public bool isCrouch;

    [Header("基本参数")]
    public float speed; // 玩家移动速度
    private float runSpeed;
    private float walkSpeed => speed / 3f; // lambda表达式
    public float jumpForce; // 玩家跳跃受力

    void Awake() // Awake(): 在游戏对象创建时调用
    {
        physicsCheck = GetComponent<PhysicsCheck>(); // 获取PhysicsCheck的数据
        rb = GetComponent<Rigidbody2D>(); // 获取Rigidbody2D的数据
        coll = GetComponent<CapsuleCollider2D>(); // 获取CapsuleCollider2D数据

        originalOffset = coll.offset; // 获取CapsuleCollider2D.offset初始值
        originalSize = coll.size; // 获取CapsuleCollider2D.size初始值

        inputControl = new PlayerInputControl(); //实例化PlayerInputControl于inputControl

        inputControl.Gameplay.Jump.started += Jump; // 订阅事件Jump.started，每次事件触发启动Jump（）

        #region 强制步行
        runSpeed = speed;
        inputControl.Gameplay.WalkButton.performed += ctx => // performed: 持续按下
        {
            if (physicsCheck.isGround) // 若角色在地面上时按下Alt键
            {
                speed = walkSpeed;
            }
        };
        inputControl.Gameplay.WalkButton.canceled += ctx => // canceled: 取消按下
        {
            if (physicsCheck.isGround)
            {
            speed = runSpeed;
            }
        };
        #endregion
    }
    void OnEnable() // OnEnable(): 启用inputControl时调用
    {
        inputControl.Enable(); // 启用输入控制
    }
    void OnDisable() // OnDisable(): 禁用inputControl时调用
    {
        inputControl.Disable(); // 禁用输入控制
    }
    void Update() // Update(): 每帧调用
    {
        inputDirection = inputControl.Gameplay.Move.ReadValue<Vector2>();
    }
    void FixedUpdate() // FixedUpdate(): 每固定帧调用
    {
        Move();
    }

    private void Move() // 定义移动方法
    {
        #region 人物移动
        if (!isCrouch)
        {
        rb.linearVelocity = new Vector2(speed * inputDirection.x, rb.linearVelocity.y); // 人物移动 velocity：速度 isWalk模拟轻推摇杆
        }
        // 实现人物朝向改变
        int faceDir = (int)transform.localScale.x; // 获取人物朝向
        if (inputDirection.x > 0 && faceDir != 1) // 改变人物朝向
        {
            transform.localScale = new Vector3(1, 1, 1);
        }
        else if (inputDirection.x < 0 && faceDir != -1)
        {
            transform.localScale = new Vector3(-1, 1, 1);
        }
        #endregion

        isCrouch = inputDirection.y < -0.5f && physicsCheck.isGround;// 下蹲
        if (isCrouch) // 碰撞箱缩小
        {
            coll.offset = new Vector2(-0.05f, 0.75f);
            coll.size = new Vector2(0.66f, 1.5f);
        }
        else
        {
            coll.size = originalSize;
            coll.offset = originalOffset;
        }
    }
    private void Jump(InputAction.CallbackContext context) // 定义跳跃方法
    {
        if(physicsCheck.isGround)
        {
            rb.AddForce(transform.up * jumpForce, ForceMode2D.Impulse); // Rigitbody2D.AddForce(Vector2 force, ForceMode2D mode) transform.up为坐标轴正上方向，值通常为1。
        }
    }
}


``` 