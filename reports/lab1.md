### chapter3实验报告

#### 任务要求

```rust
fn sys_task_info(ti: *mut TaskInfo) -> isize
```
syscall ID: 410

查询当前正在执行的任务信息，任务信息包括任务控制块相关信息（任务状态）、任务使用的系统调用及调用次数、系统调用时刻距离任务第一次被调度时刻的时长（单位ms）。

```
struct TaskInfo {
    status: TaskStatus,
    syscall_times: [u32; MAX_SYSCALL_NUM],
    time: usize
}
```

参数：
ti: 待查询任务信息

返回值：执行成功返回0，错误返回-1

#### 任务思路

修改TaskControlBlock，增加两个对应字段并对其进行维护，并且撰写TaskManager的函数对其进行调用，返回查询结果，保证了TaskManager对外部的私密性。

### 简答作业

1. 三个ch2b_bad_*.rs程序在运行后会被内核kill，然后切换到下一个应用程序。报错信息为：

```
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003a4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it. 
[kernel] IllegalInstruction in application, kernel killed it.
```

* **RustSBI 版本**：`0.3.0-alpha.2`
* **RustSBI-QEMU 实现版本**：`0.2.0-alpha.2`

2. 回答
    1. a0保留从陷入之前的上下文中的值。_restore的两种使用情景为：从系统调用返回和从中断或异常返回
    2. 特殊处理了sstatus、sepc和sscratch三个特殊寄存器，其分别保存了内核模式和用户模式的状态信息，异常或中断发生前的程序计数器（PC）地址和用户态的栈指针信息。
    3. x2是sp指针，通常在上下文切换时进行管理。x4是tp指针，只在线程切换时进行更新和其他操作
    4. sp存储用户态栈指针，sscratch存储内核态栈指针
    5. sret，在此之前sp指针已经被切换到用户态栈指针，sret将其他寄存器进行恢复并且切换回用户模式
    6. sp存储内核态栈指针，sscratch存储用户态栈指针
    7. 可能是在call trap_handler中实现的？

### 荣誉准则

1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

    无


2. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

    无

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。