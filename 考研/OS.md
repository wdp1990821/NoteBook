# OS
> 同步与异步
- 在微机原理中，有同步通信和异步通信
    - 同步通信 => 是一种比特同步通信技术，要求发收双方具有同频同相的同步时钟信号，只需在传送报文的最前面附加特定的同步字符，使发收双方建立同步，此后便在同步时钟的控制下逐位发送/接收。
    - 异步通信 => 异步通信是指通信中两个字符（8位）之间的时间间隔是不固定的，而在一个字符内各位的时间间隔是固定的。
- 在OS中
    - 同步按队列输出
    - 异步不按队列输出
## 1 OS introduction
### 1.1 OS 的目标和作用
- OS => 是管理计算机系统的全部硬件资源以及软件资源；控制程序运行；改善人机界面；为其它应用软件提供支持等，使计算机系统所有资源最大限度地发挥作用，为用户提供方便的、有效的、友善的服务界面。
- OS目标
    - 方便性 由汇编到高级语言 / shell
    - 有效性 提高系统资源的利用率和系统的吞吐量（单位时间内处理工作的能力）
    - 可扩展性 适应硬件发展 eg 微内核
    - 开放性 可移植性  or 可兼容
- OS作用
    - OS作为用户与硬件的接口 shell
    - OS作为计算机系统资源（CPU,memory,I/O,数据和程序）的管理者 
    - OS实现了对计算机资源的抽象 将物理接口汇编语言抽象为高级语言指令 如 read/write
- OS发展的动力
    - 提高计算机资源的利用率 有效性
    - 方便用户 方便性
    - 器件的更新 开放性
    - 计算机体系结构的发展
    - 新的应用需求
### 1.2 OS的发展过程
- 无OS 
    - 人工 打孔纸带
    - 脱机输入/输出（Off-Line I/O）I => 外围机加纸带将其转为磁带，高速加入内存 O => 内存到磁带，磁带由外围机到纸带
        - 减少CPU空闲 外围机写纸带
        - 提高I/O速度 从磁带取到内存
#### 有OS 
- 单道批处理的处理过程 simple batch processing system
    - （Off-Line I）多个作业加入，监控程序（monitor）将作业顺序放入内存，并将控制权给作业
    - 特点 
        - 独占性 一个作业独占内存和CPU
        - 一致性 输出按队列
        - ?
- 多道批处理的处理过程 multi programmed batch processing system
    - 在外存的作业队列多个进入内存，当 A I/O操作时的CPU空挡运行 B
    - 特点 
        - 共享性
        - 异步性 输出不按队列
        - 不可再现 不知用户交互时，运行那个程序 要输出A，可能输出了B
        - 系统吞吐量大 CPU I/O 内存同时工作 
- 分时系统 time sharing system
    - input 时多路卡和缓冲区
    - 轮转运行 split 一个时间片运行一个程序，下一个时间片运行下一个程序
    - 特点
        - 多路性
        - 独立性
        - 及时性
        - 交互性
        - 异步性
- 实时系统 real time system
    - 实时即及时，计算不但要正确，还要在规定时间完成
> 微机OS的发展 单用户单任务 / 单用户多任务/ 多用户多任务
### 1.3 OS的基本特性
#### 并发（concurrence）最基本
- 并行性 多事件同时间点发生
- 并发性 多事件同时间段发生
- 进程 能独立运行的并作为资源分配的基本单位
    - 为计算程序和I/O程序分别建立进程就可以，当 A I/O操作时的 CPU空挡运行 B
#### 共享sharing
- 时间和地点上共享 二人在同一地点同时看一本书
    - 互斥共享 A看完B看
    - 同时访问 A看段时间B看再A
#### 虚拟virtual
- 将一个物理实体变成若干个逻辑上的对应物
    - 分时复用 虚拟处理机和I/O 虚拟存储技术（逻辑上扩大内存）
    - 空分复用 
        - 电信业将宽的信道变窄
        - 用存储器的空闲空间存放运行其他多道程序（提高内存利用率）
#### 异步asynchronism 
- 进程的异步性，进程以人们不可预知的速度向前推进
#### 1.4 OS的主要功能
- 处理机管理功能
    - 进程控制
    - 进程同步
    - 进程通信
    - 调度
        - 作业调度
        - 进程调度
- 存储器管理功能
    - 内存分配
    - 内存保护
    - 地址映射
    - 内存扩展
- 设备管理功能 I/O缓存
    - 缓存管理
    - 设备分配
    - 设备处理
- 文件管理功能
- 接口功能
#### 1.5 OS设计
- 第一代 无结构OS
- 第二代 模块化结构OS 
    - 按1.4的功能分模块成树状，会死锁
- 第三代 分层式结构OS 
    - 只为其后提供服务，只让其前提高服务/自底向上
- C/S 客户机 服务器 网络系统 
    - 客户发送请求，服务器接收并发送消息，客户机接收信息
- B/S 
- 面对对象的程序设计
    - 类/对象/继承
- 现代OS 微内核OS结构  
## 2 进程的描述与控制
### 2.1 
- 进程
    - 进程是程序的一次执行
    - 进程是程序及其数据在处理机上的顺序执行所发生的活动
    - 进程是具有独立功能的程序在数据集合上运行的过程，它是系统进行资源分配和调度的一个独立单元
    - 特征
        - 动态性 并发性 独立性 异步性
### 2.2 PCB 
- PCB process control block
    - 标识符
    - 进程基本状态 就绪ready 执行running 阻塞block
    - 地址空间，数据 封闭
    - 优先级 调度先后顺序
    - cpu的状态 
        - 两状态模型 
        - 三状态模型
        - 第一种五状态模型
        - 第二种五状态模型
        - 七状态模型
    - 同步
- 进程实体 => PCB + 程序 + 数据
- 进程控制过程
    - 创建与终止
    - I/O 请求（阻塞）与 I/O 完成（唤醒）
    - 挂起与激活
    - 调度 时间片完
    - 所有进程都是可管理的 （增删改查）
- 进程树规定了进程管理的权限
- PCB 的组织方式
    - 线性 连接 索引
### 2.3 进程控制
- 创建进程
    - who OS 父进程 用户
    - how
        - 申请PCB在空白PCB队列
        - 初始化PCB
        - 申请资源
        - 将PCB插入状态队列
- 终止进程
    - who OS 父进程 用户
    - how
        - 修改PCB状态
        - 释放资源
        - 清除PCB数据
        - 插入空的PCB队列
- 阻塞进程
    - who 自身 程序中有阻塞指令 sleep
    - how 
        - 阻塞原语
        - 找PCB修改状态 执行->阻塞
        - 将PCB放入阻塞队列
- 唤醒
    - who 其他进程
    - how 
        - 唤醒原语
        - 找PCB修改状态
        - 将PCB插入就绪队列
- 挂起
    - who OS 父进程 用户
    - how 
        - 找PCB，修改状态
        - 将PCB放入挂起队列
        - 拆分实体
            - 释放资源
            - 将程序和数据调入外存
## 第三章 处理机调度与死锁
### 3.1 处理机调度的层次和调度算法的目标
> 处理机调度的层次
- 高级调度（high level scheduling）
    - 又称长程调度或作业调度 用于多道批处理系统
    - 根据某个算法，将外存上处于后备队列中的哪几个作业调入内存的，并为其创建进程，分配必要资源和放入就绪队列
- 低级调度（low level scheduling）
    - 又称进程调度或短程调度，调度对象是进程或内核级线程 用于多道批处理系统，分时和实时系统
    - 根据某个算法，决定就绪队列中哪个进程获得处理机，并分配程序将处理机分配给选中的进程
- 中级调度（intermediate scheduling） 
    - 又称内存调度，为提高内存利用率和系统吞吐量
    - 把那些暂时不能运行的进程调入外存，此时进程的状态成为就绪驻外存状态或挂起状态
    - 当它们已具备运行条件且内存又稍有空间时，由中级调度来决定，把外存的上的那些已具备运行条件的就绪进程在重新调入内存，并修改其状态为就绪状态，挂在就绪队列上等待。
    - 实际上是存储器管理中的对换功能，见第四章
> 处理机调度算法的目标
- 共同目标
    - 资源利用率
        - CPU的利用率 = CPU有效工作时间/CPU有效工作时间 + CPU 空闲等待时间
    - 公平性 不会饥饿现象 但要考虑优先级
    - 平衡性 不同的类型进程有需求 如计算型 I/O型 需要使资源利用率都高
    - 策略强制执行
- 批处理系统的目标
    - 平均周转时间短
        - 周转时间：从作业被提交给系统开始到作业完成为止的时间间隔
            - 作业在外存后备队列上等待的时间
            - 进程在就绪队列上等待的时间
            - 进程在CPU上执行的时间
            - 进程等待 I/O 操作完成的时间
        - 平均周转时间 每个作业的周转时间/作业数
    - 系统吞吐量高
        - 单位时间内系统所完成的作业数
        - 多选短作业运行这一指标高
    - 处理机利用率高
        - CPU 空闲等待时间少
        - 多选长作业运行这一指标高
- 分时系统的目标
    - 响应时间快
        - 用户提交请求开始到输出处理结果为止
    - 均衡性
        - 复杂任务响应时间可以长，但简单任务响应时间要短
        - 均衡性，是指系统响应时间快慢与用户请求的复杂程度相对应
- 实时系统的目标
    - 截止时间的保证
        - 截止时间：某任务必须开始的最迟时间或必须完成的最迟时间
        - HRT硬实时任务 必须保证 SRT 软实时任务 基本保证
    - 可预测性
        - 在多媒体系统（SRT）中视频应连续播放，这就提供了请求的可预测性
        - 若系统采用了双缓冲，即实现了第 i 帧的播放和第 i+1 帧的读取并行处理，进而提高其实时性
### 3.2 JCB 作业与作业调度
#### 3.2.1 批处理系统中的作业

- 作业管理
    - JCB引入 cpu memory/disk I/O
    - 作业与作业实体（JCB+程序）
    - 作业状态与控制
        - 作业控制引起作业状态转化的操作
        - 创建 调度 阻塞 唤醒 终止
        - 调度 依据一定的策略，将资源分配给某个作业，或将资源从某个作业移除的过程
        - 调度算法 资源分配或移除所依据的策略
- JCB 结构 job control block
    - 标识符
    - cpu 信息
        - 调度信息
        - 账户
        - 状态 running block ready
        - 资源需求请求
    - address pointer
    - I/O well
    - 时间指标 => 度量性能
    - 资源性能
- JCB ad
    - 简单整体执行
- JCB disad
    - 资源需求大，资源利用率低
    - 内存需求大不支持虚拟内存
- 调度算法的指标
    - 登录时间 disk -> memory
    - 开始时间 memory -> cpu
    - 结束时间 cpu -> memory
    - 登出时间 memory -> disk
- 面对一个 JCB 的指标
    - 周转时间 = 结束时间 - 登录时间 
    - 执行时间 = 结束时间 - 开始时间(小于周转时间)
    - 加权周转时间 = 周转时间/执行时间
- 面对一组 JCB 的指标
- 平均周转时间 = （作业1的周转时间+作业2的周转时间+...+作业n的周转时间）/n
- 平均带权周转时间 = （作业1的加权周转时间+作业2的加权周转时间+...+作业n的加权周转时间）/n
- 先来先服务 FCFS first come first service
    - 有利于于长作业不利于短作业
    - 简单 执行序列和到达序列一致
    - 大数优先
- 短作业优先 SJF short job first
    - 小数优先
    - 优化了FCFS 降低了CPU等待时间
    - 但有可能出现饥饿状态 长作业永远不执行
    - 执行序列和到达序列不一致
- 优先级调度算法priority-scheduling algorithm PSA
- 高响应比优先调度算法 highest response ratio next HRRN
    - 平衡了 SJF （考虑运行时间） 和 FCFS （考虑等待时间）
    - 优先级可变
        - 优先级 = 响应比 = 1 + 等待时间/要求服务时间
        - 等待时间 + 要求服务时间 = 响应时间
### 3.3 进程调度
#### 3.3.1 进程调度的任务，机制和方式
- 进程调度的任务
    - 保存处理机的现场信息 断点保护
    - 按某种算法选出进程
    - 把处理器分配给进程 断点恢复
- 进程调度机制
    - 排队器
    - 分派器
    - 上下文切换器
- 进程调度的方式
    - 非抢占方式nonpreemptive mode
        - 把处理机分配给某进程后，就一直让它运行到结束或发生阻塞
        - 难以满足交互性作业和实时任务
        - 在该方式下，引起进程调度的因素可以归结为
            - 进程结束或阻塞
            - I/O请求
            - 在进程通信或同步过程中
    - 抢占方式preemptive mode
        - 允许暂停某个正在执行的进程，将已分配给该进程的处理机重新分配给另一个进程
        - 可以防止一个长进程长时间的占用处理机，更加公平
        - 在分时系统中，只有抢占方式可能实现人机交互
        - 在实时系统中抢占方式能够满足实时任务的需求。但复杂
    - 抢占方式的原则
        - 优先权原则
        - 短进程优先原则
        - 时间片原则
#### 3.3.2 轮转调度算法
> 在分时系统中是基于时间片的轮转round robin RR 调度算法
- 轮转法的基本原理
    - 根据 FCFS 排成一个就绪队列，并可以设置每隔一定时间间隔即产生一个中断 
- 轮转法切换机制
    - 若一个时间片尚未用完，正在运行的进程便已经完成，就立即激活调度程序，将它从就绪队列删除，再调度就绪队列中对首的进程运行，并启动新的一个时间片
    - 再一个时间片用完时，计时器中断处理程序被激活。如果进程尚未运行完毕，调度程序将把它送到就绪队列的队尾
- 时间片大小的确定
    - 时间片小有利短作业 时间片大退化成 FCFS 
> 其隐含所有进程的紧迫性一致
#### 3.3.3 优先级调度算法
- 优先级调度算法的类型
    - 非抢占式优先级调度算法
    - 抢占式优先级调度算法
- 优先级的类型
    - 静态优先级
        - 在创建进程时确定的，在整个运行期间保持不变
        - 依据 
            - 进程类型 系统进程高于用户
            - 进程对资源的需求 资源要去少的高
            - 用户要求
        - 这种方式简单易行系统开销小，但不精确
    - 动态优先级
        - 在创建进程时确定的，然后其值随进程的推进或等待时间的增加而改变，以便获得更好的调度性能
#### 3.3.4 多队列调度算法
- 将进程就绪队列拆分成若干个，将不同类型或性质固定在不同的队列中，不同的队列使用不同的调度算法
#### 3.3.5 多级反馈队列
- 设置多个就绪队列并为其设置不同的优先级
    - 第一最高时间片最小第二次之
- 每个队列都采用 FCFS 算法
    - 当新进程进入内存后，首先将它放入第一队列末尾
    - 若其完成则退出
    - 若其未完成进入第二队列未尾，若还未完成进入第三队列
    - 重复上一个，直到最后一个队列使用 RR 
#### 3.3.6 基于公平原则的调度算法
- 保证调度算法
    - 若系统中有 n 个相同类型的进程同时运行 保证每个进程都获得相同的处理机时间 1/n
- 公平分享调度算法
    - 各个用户拥有不同的进程数，要让用户获得相同的处理机时间，或所要求的时间比例
### 3.4 实时调度
#### 3.4.1 实现实时调度的基本条件
- 提供必要的信息
    - 就绪时间：某任务成为就绪状态的起始时间，
    - 开始截止时间 因为一个系统中有多个用户/进程，则二者有所不同
    - 完成截止时间
    - 处理时间
    - 资源要求
    - 优先级
- 系统处理能力强
    - 提供方法 采用单处理机系统增强其处理能力 或采用多处理系统
- 采用抢占式调度机制
    - 在有 HRT 中广泛采用
- 具有快速切换机制
    - 对中断的快速响应能力
    - 快速的任务分派能力
- 实时调度算法的分类
    - 非抢占式调度算法
    - 抢占式调度算法
        - 基于时钟中断的抢占式优先级调度算法
        - 立即抢占的优先级调度算法
#### 3.4.2 最早截止时间优先 EDF earliest deadline first
- 根据任务的截止时间确定任务的优先级
    - 非抢占式调度方式用于非周期实时任务
    - 抢占式调度方式用于周期实时任务
#### 3.4.4 最低松弛度优先 LLF least laxity first 小数优先


## 第四章
### 4.1 存储器的层次结构
#### 4.1.1 多层结构的存储器系统
- 存储器的多层结构
    - CPU 寄存器 速度最快，价格最高
    - 主存
        - 高速缓存
        - 主存储器
        - 磁盘缓存
    - 辅存
        - 固定磁盘
        - 可移动存储介质
- 可执行存储器
    - 寄存器和主存储器称为可执行存储器
    - 对可执行存储器访问时，进程可以使用很少的时钟周期使用一条 load 和 store 指令，而其他的需要 I/O 相差 3个数量级
    - 操作系统的存储管理负责对可执行存储器的分配回收，以及不同层次的数据移动管理 
#### 4.1.2 主存储器与寄存器
- 主存储器
    - 又称内存，主存和可执行存储器，用于保存进程运行时的程序和数据，
    - 处理机从主存储器中取得指令和数据，并将其所取得的指令放入指令寄存器，将其所取
    得的数据放入数据寄存器
- 寄存器
    - 具有和处理机相同的速度，故最快但贵
#### 4.1.3 高速缓存和磁盘缓存
- 高速缓存
    - 其在寄存器和存储器之间的存储器，主要用于备份主存中常用数据，以减少处理机对主存储器访问次数，以提高程序执行速度
- 磁盘缓存
    - 因为磁盘的 I/O 速度远低于对主存的访问速度，为匹配二者速度，而设置磁盘缓存，存放频繁使用的一部分磁盘数据和信息，以减少磁盘访问
    - 但磁盘是内存的部分存储空间
### 4.2 程序的装入和链接
- 用户程序要在系统中运行，必须先将它装入内存，然后再将其转变为可执行程序 其步骤
    - 编译 形成若干目标模块
    - 链接 将目标模块以及库函数链接在一起，形成完整的装入模块
    - 装入 将程序装入内存
#### 4.2.1 程序装入
- 绝对装入方式
    - 只能运行单道程序时，完全有可能知道程序将驻留在内存的什么位置，采用绝对装入，将产生绝对地址（物理地址）的目标代码
    - 由于程序的相对地址和实际内存地址完全相同，故不需对程序和数据的地址进行修改
    - 程序使用的绝对地址可以在编译或汇编时给出，也可直接赋值，但最好用前者
- 可重定向的装入方式
    - 在运行多道程序的处理机时，程序的初始地址时从 0 ，但内存具体情况将装入模块的合适位置
    - 装入模块中的所有的逻辑地址与实际装入内存后物理地址不同

















## 第五章
### 
## 第六章 输入输出系统
### 6.1 I/O 系统的功能，模型和接口
> I/O 系统管理的主要对象是 I/O 设备和相应的设备控制器。
- 主要任务 -> 
    - 完成用户提出的 I/O 请求
    - 提高I/O 速率
    - 提高设备的利用率
    - 方便高层进程使用 I/O 设备
#### 6.1.1 I/O 系统的基本任务
- 方便用户使用 I/O
    - 隐藏物理设备的细节
        - 不同设备有不同命令
    - 与设备的无关性
        - 抽象 I/O 命令 和 提高程序可移植性
- 提高 cpu 和 I/O 设备的利用率
    - 提高处理机和 I/O 设备的利用率
        - 提高其之间的并行
    - 对 I/O 设备进行控制
        - 轮询 中断 直接存储器 I/O通道
- 方便用户共享
    - 确保设备的正确共享
        - 互斥独占（打印机，磁带机） 非互斥共享（磁盘）
    - 错误处理
        - 机械电器会有错误（临时->重试/永久->报告上层）
#### 6.1.2 I/O 系统层次结构和模型
> I/O向下与硬件有关，向上与文件系统，虚拟存储器系统和用户交互有关故须分层
- I/O 软件的层次结构
    - 用户层 I/O 软件
    - 设备独立性软件 用户和驱动的统一接口
    - 设备驱动软件 
    - 中断程序 保护 cpu 中断环境
- I/O 系统中各种模块之间的层次视图
    - I/O 系统的上下接口
        - I/O 系统接口， I/O 与上层系统（如文件系统，虚拟存储器系统和用户交互等）之间的接口，提供抽象命令
    - 软硬件（RW/HW）接口
        - 其上为对应的中断处理程序，其下为对应的设备控制器
- I/O 系统的分层
    - 底层的中断处理程序，与硬件交互
    - 次底层的设备驱动程序，负责进程与进程与设备控制器之间的通信，抽象命令->具体命令
    - 设备独立性软件 ，I/O 独立与具体使用的物理设备
#### 6.1.3 I/O 系统接口
- 块设备接口
    - 块设备管理程序和高层之间的接口，管理磁盘和光盘
    - 块设备 
        - 数据存储和传输以数据块为单位 每秒数 MB 到数十 MB
        - 可寻址，既能指定输入源地址和输出目的地址，也可随机读写，磁盘的I/O 采用 DMA 方式
    - 隐藏磁盘的二维结构
        - 编号 二维 -> 线性 扇区地址需要磁道号和扇区号
    - 将抽象命令映射为底层操作
        - 抽象命令读，写，开，关
        - 逻辑块号转换为磁盘盘面，磁道和扇区
- 流设备接口
    - 流设备管理程序和高层之间的接口，又称字符设备接口
    - 字符设备
        - 数据存储和传输以字符为单位 如键盘打印机，速率低 不可寻址 使用中断
    - get 和 put 操作，因其不可不可寻址，故只使用顺序存储方式 建立一个字符缓存区
    - in-control 指令，字符设备的类型非常多，且差异大为统一使用 in-control 指令
    - 流设备属于独占设备，故互斥
- 网络接口
### 6.2 I/O 设备和设备控制器
- I/O 设备
    - 执行I/O 操作的机械部分，一般I/O 设备
    - 执行控制I/O 的电子部件，称设备控制器或适配器（adapter）
        - 如网卡 I/O 通道和I/O 处理机
#### 6.2.1 I/O 设备
- 分类
    - 按使用特性
        - 存储设备 叫外存
        - I/O 设备
            - 输入 键盘鼠标扫描仪视频摄像
            - 输出 打印机绘图仪
            - 交互式 显示器
    - 按传输速率
        - 低速 每秒几字节到数百字节 键鼠
        - 中速 每秒数千到数十万字节 行式打印机 激光打印机
        - 高速 数十万到千兆字节 磁带机 磁盘机 光盘机
- 设备与控制器之间的接口
    - 数据信号线 缓冲器 -> 设备控制器
    - 控制信号线 
    - 状态信号线
#### 6.2.3 设备控制器
> 控制 I/O 设备实现 I/O 设备与计算机的数据交换
- 设备控制器的基本功能
    - 接收和识别命令
    - 数据交换 cpu和控制器 ，控制器和设备之间的数据交换 数据寄存器
    - 标识和报告设备状态
    - 地址识别 地址译码器
    - 数据缓冲区 
    - 差错控制
- 设备控制器的组成
    - 设备控制器与处理机的接口
    - 设备控制器与设备的接口
    - I/O 逻辑 cpu -> 设备控制器 -> 设备
#### 6.2.3 内存映像 I/O 
> 驱动程序将抽象命令转换后装入设备控制器，设备控制器执行命令控制设备，这一工作可如下完成
- 利用特定的 I/O 指令
    - 为实现 cpu 和 设备控制器的通信，为每个控制寄存器分配一个 I/O 端口
    - 缺点访问内外存需要两种指令
- 内存映像 I/O 
    - 不区分内存单元和设备控制器中寄存器地址
#### 6.2.4 I/O 通道
- I/O 通道的引入
    - 有了设备控制器大大减少 cpu 对 I/O 的干扰，但外设多时 cpu仍然负担很重
    - I/O 通道 是具有执行 I/O 能力的特殊处理机
        - 其指令类型单一
        - 没有内存，共享 cpu 的内存
- 通道类型
    - 字节多路通道 低速
        - 分出多个子通道，按时间片轮转共享主通道
    - 数组选择通道 高速
        - 只有一个子通道，独占
    - 数组多路通道 高速
        - 多个子通道，非独占
- “瓶颈”问题
    - 通道贵则少，系统吞吐量下降
    - 需增加设备到主机间的通路而不增加通道，一个设备控制器连接多个通道
### 6.3 中断机构和中断处理程序
#### 6.3.1 中断简介
- 中断与陷入
    - 中断
        - cpu 对 I/O 设备的中断信号的响应
    - 陷入
        - cpu 内部事件引起的中断
- 中断向量表和中断优先级
    - 中断向量表
        - 中断处理程序的入口地址 和 中断号
    - 中断优先级
- 对多个中断源的处理方式
    - 屏蔽禁止中断和嵌套中断
#### 6.3.2 中断处理程序
- 测定是否有未响应的中断信号
- 保护被中断的进程的 cpu 环境
- 转入相应的设备处理程序
- 中断处理
- 恢复cpu 现场并退出
### 6.4 设备驱动程序
> 设备处理程序又称设备驱动程序 是 I/O系统高层和设备控制器之间的通道程序，接受程序命令转为具体要求发送给设备控制器，反之成立
#### 6.4.1 设备驱动程序概述
- 设备驱动程序的功能
    - 接收与设备无关的软件发来的命令和参数，并将命令中抽象要求转换为与设备相关的低层操作
    - 检查用户 I/O 请求的合法性
    - 发出 I/O 命令
    - 响应由设备控制器发出的中断请求
- 设备驱动程序的特点
    - 驱动程序允许多次调用
    - 一部分写入 rom
    - 驱动程序与 I/O 设备采用的 I/O 控制方式有关
    - 与硬件特性有关
- 设备处理方式
    - 为每一类设备设置一个进程，专门用于执行系统中这类I/O 操作
    - 在整个系统中设置一个I/O 进程专门执行系统中所有各类设备的I/O 操作
    - 不设置专门的设备处理进程，而只为各类设备设置相应的设备驱动程序
#### 6.4.2 设备驱动程序的处理过程
> 设备驱动程序的主要任务是启动指定设备，完成上层指定的工作。但在启动之前应在先完成准备工作
- 将抽象要求转换为具体要求。
- 对服务请求进行校验。 打印机是否可以读磁盘
- 检查设备的状态。
- 传送必要的参数。波特率 奇偶检验，停止位数目及数据字节长度
- 启动I/O 设备
#### 6.4.3 对 I/O 设备的控制方式
> 尽量减少主机对I/O 控制干预
- 使用轮询的可编程 I/O 方式 用于时间频率短的
- 使用中断的可编程 I/O 方式 用于时间频率长的中断仍以字节为单位进行 I/O 
- 直接存储器访问的方式（DMA）
    - 接存储器访问的方式引入
        - 数据传输的基本单位为数据块
        - 所传送的数据是从设备直接到内存或反之
        - 传送一个或多个数据块的开始和结束时才需要cpu干预
    - DMA存储器的组成
        - 主机与DMA 控制器的接口
            - 命令/状态寄存器 CR 接收从 cpu 来的I/O 命令，控制信息或设备状态
            - 内存寄存器 MAR 输入时存放设备传送到内存的起始目标地址，输出时存放由内存到设备的内存源地址
            - 数据寄存器 DR 用于暂存从设备到内存，或内存到设备的数据
            - 数据计数器 DC 存放本次 cpu 要读写的字节数
        - DMA 与块设备的接口
        - I/O 控制逻辑
    - DMA 工作过程
        - 设置 MAR 和 DC 初值
        - 启动 DMA 传送命令
        - 挪用存储器周期传送数据字，存储器地址加一 DC 减一
        - DC = 0 ? 执行三：请求中断
- I/O 通道控制方式
    - 引入
        - 需要一次读取多个数据块并且将它们分别传送到不同的内存区
        - 可以实现 cpu 通道 I/O 三者并行操作
    - 通道程序
        - 通道通过执行通道程序与设备控制器共同实现对I/O 设备的控制
        - 每个指令中都包含下列信息
            - 操作码，它规定了指令所执行的操作
            - 内存地址，标明字符送入内存和从内存取出时内存首地
            - 计数，表明本条指令所要读或写数据的字节数
            - 通道程序结束位 P，用于表示通道程序是否结束，P=1表示其为最后一条指令
            - 记录结束标志 R,R= 0 表示本通道的指令与下条指令所处理的数据时同属于一个记录，R = 1 表示此为最后一条指令
### 6.5 与设备无关的 I/O 软件
#### 6.5.1 与设备无关的软件的基本概念
- 以物理设备名使用设备
- 引入了逻辑设备名
    - I/O 重定向 return / cout
- 逻辑设备名到物理设备名的转换
    - 逻辑设备表
#### 6.5.2 与设备无关的软件
- 与设备无关的软件时 I/O 系统的最高层软件，在它下面的是设备驱动程序。
    - 设备驱动程序的统一接口
    - 缓冲管理
    - 差错控制
        - 暂时性错误
        - 持久性错误
    - 在独立设备的分配与回收
    - 独立与设备的逻辑数据块
##### 6.5.3 设备分配
- 设备分配中的数据结构
    - 设备控制表 DCT 
        - 设备类型
        - 设备标识符
        - 设备状态 等待/不等待 忙/闲
        - 指向控制器表的指针 指向该设备所连接的控制器的控制表
        - 重复执行次数或时间 看是暂时性错误还是持久性错误
        - 设备队列的队首指针 请求本设备而未得到满足的进程排成设备请求队列
    - 控制器控制表，通道控制表和系统设备表
        - 控制器控制表（COCT）
        - 通道控制表（CHCT）
        - 系统设备表（SDT）
    - 设备分配时应考虑的因素
        - 设备的固有属性
            - 独占 共享 虚拟
        - 设备分配的算法
            - 先来先服务
            - 优先级高的优先
        - 设备分配中的安全性
            - 安全分配方式
                - 请求时阻塞
            - 不安全分配方式
                - 占用时阻塞
        - 独占设备的分配程序
            - 分配设备 分配控制器 分配通道
#### 6.5.4 逻辑设备名到物理设备名映射的实现
- 逻辑设备表 LUT （logical unit table）
    - 逻辑设备名 物理设备名 设备驱动程序的入口地址
- 逻辑设备表的设置问题
    - 第一种方式是整个系统中只设置一张LUT 
    - 第二种方式是每个用户设置一张LUT
### 6.6 用户层的 I/O 软件
#### 6.6.1 系统调用与库函数
- 系统调用
    - 应用程序通过系统调用间接调用 I/O 
    - 系统调用是用户提供所有的功能
- 库函数
    - 内核提供了 OS 的基本功能，而库函数扩展了 OS 的内核
#### 6.6.2 假脱机（spooling）系统
> 通过假脱机技术，则可将一台物理 I/O 设备虚拟为多台I/O 设备
- 缓和 cpu 高速性与 I/O 设备低速性间的矛盾
- spooling
    - 输入井和输出井
    - 输入缓冲区和输出缓冲区
    - 输入进程和输出进程
    - 井管理程序
- spooling 系统的特点
    - 提高了 I/O 的速度
    - 将独占设备改造成共享设备
    - 实现了虚拟设备功能
- 守护进程daemon 理发师
### 缓冲区管理
#### 6.7.1 缓冲的引入
- 缓和cpu 与 I/O 设备间速度不匹配的矛盾
- 减少对 cpu 的中断频率
- 解决数据粒度不匹配的问题
- 提高 cpu 和 I/O 设备之间的并行性
#### 单缓冲和双缓冲区和环形缓冲区
- 单缓冲区
    - I/O 设备 -> 缓冲区 -> 工作区
- 双缓冲区
    - 双刀单置
- 环形缓冲区
    - 多个缓冲区
    - 空缓冲区 以装满缓冲区 计算进程正在现行工作缓冲区 各一个指针
#### 缓冲池
- 组成
    - 空白缓冲队列
    - 输入队列
    - 输出队列
### 6.8 磁盘存储器的性能和调度
#### 6.8.1 磁盘性能简述
- 磁盘的组织和格式
- 磁盘的类型
    - 固定头磁盘
    - 移动头磁盘
- 磁盘访问时间
    - 寻道时间 T = m*n + s s磁臂启动的时间 与 磁头移动 n 条磁道所花费的时间
    - 旋转延迟时间
    - 传输时间
        - T = b / r * N r 为磁盘每秒的转数， N 为一条磁道的字节数 ，每次所读写的字节数
    - 集中数据，可以提高传输速率
#### 6.8.2 磁盘调度算法
- 先来先服务
- 最短寻道时间优先
- 扫描算法 scan
    - 优先考虑磁头移动方向，然后考虑磁道与当前磁道的距离
- 循环扫描算法 cscan
    - 规定磁头单向移动
- NStep算法
    - 把磁盘请求队列分成若干长度为 N 的子队列，按 FCFS 处理子队列，当处理子队列时，出现磁盘请求，将其放入新队列
- FSCAN算法
    - 把磁盘请求队列分成两个子队列 
        - 一个是由当前所有的请求磁盘 I/O 的进程形成的队列，由磁盘调度按 SCAN 算法进行处理
        - 在扫描期间，将新出现的所有请求磁盘 I/O 的进程放入等待处理的请求队列
## 第七章 文件管理
### 7.1 文件和文件系统
#### 7.1.1 数据项，记录和文件
- 数据项 => 最低的数据组织形式
    - 基本数据项 属性
    - 组合数据项 类
- 记录 => 相关数据项的集合，描述一个对象某方面属性
- 文件 => 创建者定义，有文件名的一组相关元素集合
    - 有结构文件 => 若干相关记录
    - 无结构文件 => 字符流
    - 文件属性
        - 文件类型
        - 文件长度
        - 文件的物理位置
        - 文件的建立时间
#### 7.1.2 文件名和类型
- 文件名和扩展名
    - 文件名长度 大小写
    - 扩展名 后缀名 1~4字符
- 文件类型
    - 按用途
        - 系统文件 用户文件 库文件
    - 按形式
        - 源文件 目标文件 可执行文件
    - 按存取控制属性
        - 只执行 只读 读写
    - 按组织形式和处理方式
        - 普通 目录（文件夹）特殊
#### 7.1.3 文件系统的层次结构
- 对象属性
    - 文件
    - 目录 方便用户 提高存取速度
    - 磁盘存储空间 提高存取速度
- 对对象的操纵和管理的软件集合
    - 功能
        - 对文件存储空间的管理
        - 对文件目录的管理
        - 将文件的逻辑地址转换为物理地址的机制
        - 对文件读写的管理
        - 对文件的共享和保护等功能
    - 层次
        - I/O 控制层 磁盘驱动程序组成
        - 基本文件系统层，内存 磁盘数据块的交换
        - 基本 I/O 管理程序 文件逻辑块号转换为物理块号 管理磁盘空闲块 I/O缓冲区
        - 逻辑文件系统 处理与记录和文件相关操作
- 文件系统的接口
    - 命令接口
    - 程序接口 open
#### 7.1.4 文件操作
- 基本文件操作
    - 创建 分配外存建立目录项，其中记录文件名和地址
    - 删除文件 删除目录项，回收存储空间
    - 读文件 查目录 得到外存地址 使用读指针
    - 写文件 查目录 找到文件目录项 使用写指针
    - 设置文件读写位置 改顺序存储为随机存储
- 文件 open / close 操作
    - 为避免重复检索目录，内存的文件表
- 其他文件操作
    - 设置和获取文件属性
    - 目录操作
    - 文件共享
### 7.2 文件的逻辑结构
- 逻辑文件，由一系列逻辑记录组成 用户可见 用户可以直接处理
- 物理结构 存储结构
- 二者影响文件检索
#### 7.2.1 文件逻辑结构和类型
- 对文件逻辑结构所提出的基本要求
    - 提高文件检索速度 大批记录组成的文件，应采用一种提高检索记录速度和效率的逻辑结构形式
    - 该结构应方便文件修改，增删改
    - 降低文件存放在外存上的存储费用
- 按文件有无结构
    - 有结构
        - 定长 记录定长
        - 不定长 
    - 无结构 长度以字节为单位
- 按文件组织方式
    - 顺序文件 
    - 索引文件
    - 索引顺序文件 为一组记录第一个记录建立索引
#### 7.2.2 顺序文件
- 顺序文件的排列方式
    - 串结构 按存入时间先后排序
    - 顺序结构 指定关键字标识 用折半 插值 跳步查找
- 顺序文件的优缺点
    - 顺序文件用于对文件中记录进行批量存取时
    - 交互式场合，查找修改的单个记录逐个查找性能低
    - 增加删除困难，解决方法为其配置运行文件把增加删除记录其中，每隔一段时间将其与主文件合并
#### 7.2.3 记录寻址
- 隐式寻址
> 对定长记录顺序文件，若已知当前记录逻辑地址，可以找到下一个记录的逻辑地址。
> 在读文件时，设置读指针令其指向下一个记录的首地址，当读完一个记录时执行 readpioter+=1 
- 
> 对变长记录的顺序文件，需要 readpioter += i ,i 是 刚读完的记录长度
- 显式寻址
    - 用于定长记录文件实现直接或随机访问
    - 实现随机访问
        - 定长记录通过文件记录的位置，但变长需要顺序查找每个记录获得记录长度
        - 利用关键字
#### 7.2.4 索引文件
- 按关键字建立索引
> 变长记录文件查找必须从第一个开始，耗时长

    - 可以为变长记录建立索引表，为主文件中每个记录在索引表中分别设置一个表项，记录指向记录指针以及记录长度，
    - 索引表按关键字排序，因为其为定长文件的顺序文件，可以使用关键字折半查找
- 具有多个索引表的索引文件
#### 7.2.5 索引顺序文件
- 索引顺序文件的特征
    - 引入文件索引表
    - 增加溢出文件
- 一级索引顺序文件
    - 建立方法
        将变长记录顺序文件所有记录分成若干组，然后为顺序文件记录索引表并为每组中的第一个记录在索引表中记录索引项，含有关键字和指向该记录的指针
    - 若顺序文件记录数为 N 顺序检索平均查找 N/2 记录 索引顺序查找 开平方 2 个记录，效率高了 开平方 2 /2 倍
- 二级索引顺序文件
    - 为索引文件建立索引
    - 若顺序文件记录数为 N 顺序检索平均查找 N/2 记录 二级索引顺序查找 （3/2） 开立方 2 个记录，效率高了 根号 2 /2 倍
    - eg 100万记录，顺序 50 万 一级 1000 二级 150
#### 7.2.6 直接文件和哈希文件
- 直接文件
    - 键值转换 dict
- 哈希文件
### 7.3 文件目录
> 文件管理要求
    - 实现“按名存取”
    - 提高对目录的检索速度
    - 文件共享
    - 允许文件重名
#### 7.3.1 文件控制块和索引结点
> 文件控制块 描述和控制文件的数据结构 为了对一个文件正确存取，也是文件目录项
- 文件控制块的信息
    - 基本信息类
        - 文件名 
        - 物理位置 （外存位置 设备名 文件在外存的起始盘块号 指示文件所占用的盘块数 或字节数的文件长度）
        - 逻辑结构 （记录式 记录数 是否定长）
        - 物理结构（顺序 链接 索引）
    - 存取控制信息
        - 文件主的存取权限，核准用户的存取权限，以及一般用户的存取权限
    - 使用信息类
        - 建立时间日期 上次修改日期和时间当前使用信息
- 索引结点
    - 引入
        - 文件目录存放在外存，当文件很多时，文件目录要占用大量盘块
        - 查找目录时，将第一个盘块的目录调入内存，然后将用户所给定的文件名，与目录项中的文件名比较，未找到导入下个盘
        - 若目录文件占用盘块数为 N 按此查找平均调入盘块 （N+1）/2 次
        - 但检索目录文件，只用文件名，只有找到采用其他信息
        - 故将文件名与文件描述信息分开，使文件描述信息单独形成一个称为索引结点的数据结构，在文件目录中目录项只有文件名和索引结点的指针
        - 启动次数减少到之前的 1/4
    - 磁盘索引结点的内容
        - 文件主标识，标识 ownor
        - 文件类型 正规文件 目录文件 特别文件
        - 文件存取权限 各类用户对该文件的权限
        - 文件物理地址
        - 文件长度
        - 文件连接数
        - 文件存取时间
    - 内存索引结点
        - 在以上添加了
        - 索引结点编号
        - 状态
        - 访问计数
        - 文件所属文件系统的逻辑设备号
        - 链接指针
#### 7.3.2 简单的文件目录
- 单级文件目录
    - 在整个文件系统中只建立一张目录表，每个文件占一个目录项，
    - 目录项中含文件名，文件扩展名，文件长度，文件物理地址及其其他文件属性和每个目录项是否空闲的状态位
    - 插入新文件时，必须检索所有目录项，以保证新文件名在目录中唯一，填入新文件名及其其他说明信息并状态位置 1
    - 删除文件时，先从目录中找到该文件的目录项，回收该文件所占用的存储空间然后再清除该目录项
    - 单级目录优点是简单，可以按名存取，但查找慢N/2 不允许重名 不便于文件共享
- 二级文件目录
    - 为每个用户建立单独的用户文件，在系统中再建立一个主文件目录，在主文件目录中，每个用户目录都占一个目录项
    - 其目录项中包括用户名和指向该用户目录的文件的指针，
#### 7.3.3 树形结构目录
- 树形结构
    - 提高目录检索速度和文件系统性能
- 路径结构和当前目录
    - 路径名（path name）
        - 从根目录到任何数据文件都只有一条唯一的通路。绝对路径
    - 当前目录
        - 用户家目录 相对路径
    - 树形目录中查找一个文件，需要按路径名逐级访问中间结点，增加了磁盘访问次数
- 目录查找
    - 创建目录
        - 建立 ufd，并创建子目录。在用户要创建新文件时，只需查看 ufd 及其子目录中有无与新建目录有无重名
    - 删除文件
        - 空目录直接删除
        - 非空目录 不删除
        - 非空目录 其文件和子目录都删除
    - 改变目录
    - 移动目录
    - 链接操作
    - 查找
#### 7.3.4 目录查询检索
> 访问以存文件，根据文件名对目录查询。找到该文件的文件控制块或对应索引结点。
> 然后根据文件控制块或索引结点中的记录的文件物理位置换算出磁盘物理位置，再通过磁盘驱动程序将所需文件读入内存
- 线性检索
- 哈希索引文件目录
    - 冲突处理
        - 目录表相应目录项为空，无此文件
        - 匹配一个找到
        - 匹配多个 hash值+常数
### 7.4 文件共享
> 多用户共享同一文件，系统只保留该共享文件的一份文件副本
#### 7.4.1 基于有向无循环图实现文件共享
- 有向无循环图
> 树形结构不合适文件共享，故给一个文件多个父目录，不需通过其属主目录来访问
> 但更新之后的文件难以找到
- 利用索引结点
> 文件物理位置不放在目录项中，放在索引结点，文件目录中只设置文件名及指向索引结点的指针
> 索引结点还有一个链接计数 count ，用于表示链接到本索引结点的用户目录项数目
#### 7.4.2 利用符号链接实现文件共享
- 基本思想
> 允许一个文件或子目录有多个父目录，但其中仅有一个主目录，其他的几个父目录都是通过符号链接方式与之相链接的
> 好处是属主结构还是树，对文件删除，查找更为方便
- 如何利用符号链实现共享
> 创建 LINK 类型新文件 将其父目录，
- 利用符号链实现共享的优点
> 删除无悬空索引结点指针
- 利用符号链的共享存在问题
> 根据文件名逐个查找目录，直到找到该文件索引结点，多次读盘，
> 需要为链接文件，配置索引结点
### 7.5 文件保护
- 影响文件安全的因素
    - 人为因素 系统因素 自然因素
- 安全措施
    - 通过存取控制机制
    - 采取系统容错技术
    - 建立后备系统
#### 7.5.1 保护域
- 访问域
    - 进程对某个对象执行操作的权力，访问权
- 保护域
    - 是进程对一组对象访问权的集合
- 进程与域之间的静态关系
    -一个进程只类型一个域
- 进程与域之间的动态联系方式
    - 进程与域一对多
#### 7.5.2访问矩阵
- 基本的访问矩阵
    - 行代表域，列代表对象，矩阵的每一项是由一组访问权组成
- 具有域切换权的访问矩阵
    - 为了进程和域之间的动态联系，，应能够将进程从一个保护域切换到另一个
#### 7.5.3 访问矩阵的修改
- 拷贝权
- 所有权
- 控制权 改变矩阵内同一行中的各项访问权
#### 7.5.4 访问矩阵的实现
- 访问控制表 访问矩阵按列划分
- 访问权限表 访问矩阵按行划分
## 第 8 章 磁盘存储器的管理
### 8.1 外存的组织方式
- 连续组织方式
- 链接组织方式
- 索引组织方式
#### 8.1.1 连续组织方式
- 优点
    - 顺序访问容易
    - 顺序访问速度块
- 缺点
    - 要求为一个文件分配连续的存储空间 => 外部碎片
    - 必须事先知道文件的长度
    - 不能灵活的删除和插入记录
    - 对于动态增长的文件，难以分配空间，若预估大小会使大量空间浪费
#### 8.1.2 链接组织方式
- 优点
    - 消除磁盘的外部碎片。提高外存利用率
    - 对插入，删除和修改记录非常容易
    - 能适应文件的动态增长
- 隐式链接
    - 在文件目录的每个目录项中，都须含有指向链接文件的第一个和最后一个盘块的指针
    - 只适合顺序访问
    - 可以将几个盘块组成一个簇
- 显式链接的
    - 吧用于连接的文件个物理块的指针显式地存放在内存的一张连接表中，整个磁盘只设置一张
#### 8.1.3 FAT 技术
- FAT12
    - 早期
        - 以盘块为基本分配单位，两张一样的文件分配表，
    - 以簇单位的 FAT12 
        - 簇为连续的扇区
- FAT16
    - 12 对表中的表项有限制
    - 16 增加了表宽 16位了
- FAT32
    - 32 增加了表宽，允许管理更多的更小的簇
    - 但运行速度变慢，不兼容，不支持小容量的分区
#### 8.1.4 NTFS 的文件组织方式
- 新特性
    - 64 位的磁盘地址
    - 支持长文件名 0~255
    - 系统容错
    - 数据一致性
    - 文件加密压缩
- 磁盘组织
    - NTFS 是以簇作为磁盘空间分配和回收的基本单位，一个文件占若干簇，一个簇只属于一个文件
    - NTFS 实现了与磁盘物理块大小的无关的独立性
- 文件的组织
#### 8.1.5 索引组织方式
> 连接组织不能支持高效的直接存取 而且FAT 需要占用较大的内存空间
- 单级索引组织方式
    - 为每个文件分配一个索引块，把分配给该文件的所有盘块号都记录在该索引块中，在建立一个文件时，只需在为之建立的目录项中填上指向该索引块的指针
    - 可以用于大文件
- 多级索引组织
    - 加快了对大型文件的查找速度
    - 但 启动磁盘的次数随索引的级数增加而增加
- 增量式索引组织方式
    - 基本思路
        - 小文件 将盘块放入文件控制块
        - 中文件 采用单级索引组织方式
        - 大型特大型 采用多级
### 8.2 文件存储空间的管理
#### 8.2.1 空闲表法和空闲链表法
- 空闲表法
    - 空闲表 连续分配
    - 存储空间分配和回收
        - 采用首次适应算法和最佳适应算法
- 空闲链表法
    - 空闲盘块链
        - 从链首开始，取下合适数目的盘块，回收的盘块分配与队尾
    - 空闲盘区链
        - 将磁盘上所有的空闲盘区的拉成一条链，在每个盘区上除含有用于下一个空闲盘区的指针为，还指明本盘区大小的信息
#### 8.2.2 位示图法
- 位示图
    - 位示图是利用二进制的一位来表示磁盘中一个盘块的使用情况。0空闲 1以分配
- 盘块的分配
    - 顺序扫描位示图 找出一个或一组其值位“0”的
    - 转换为盘块号
    - 修改位示图
- 盘块回收
    - 盘块号转换为位示图中的行号和列号
    - 修改位示图
- 成组连接法
    - 空闲盘的组织
        - 空闲盘号栈
        - 文件区中的所有空闲盘分为若干个组
        - 将每一组含有的盘块总数 N 和该组所有的盘块号记入其前一组的第一个盘块的 
        - 将第一组盘块号总数和所有盘块号计入空闲盘块号栈
        - 最后只有 99 个可用盘块
- 空闲盘块的分配和回收
### 8.3 提高磁盘 I/O 速度的途径
- 改进文件的目录结构以及检索目录的方法减少对目录的查找时间
- 选取好的文件存储结构
- 提高磁盘的 I/O 速度
#### 8.3.1 磁盘高速缓存
- 磁盘和内存间的 cache，保存某些盘的副本
- 数据交付方式
    - 指将磁盘高速缓存中的数据传送到请求者的进程
    - 数据交付
    - 指针交付
- 置换算法
    - 最近最久未使用
    - 访问频率
    - 可预见性
    - 数据一致性
- 周期性写回磁盘
    - 将高速缓存中已修改的盘块数据写回磁盘
#### 8.3.2 提高磁盘 I/O 速度的其他方法
- 提前读
    - 顺序访问可以
- 延迟写
    - 加如空闲队列尾，满将头写入
- 优化物理块的分布
- 虚拟盘
    - 内存做磁盘
#### 8.3.3 廉价磁盘冗余阵列
- 并行交叉存取
    - 一个数据分成块，存储到不同的磁盘
- RAID的分级
    - RAID0 提供并行交叉
        - 实现高速
        - 但无冗余校验
    - RAID1 具有磁盘镜像
        - 4 个数据盘 4 个镜像盘，数据分别写入主盘和镜像盘 
        - 可靠性好但容量利用率 50%
    - RAID3 并行传输磁盘阵列 6个数据盘 一个校验盘 利用率 6/7
    - RAID6 专用可快速访问的异步校验盘 其有独立的数据访问通路
    - RAID7 所有磁盘都有较高的传输速率
    - 优点
        - 除 RAID0 都有容错 磁盘 I/O 速度高，性价比高
### 8.4 提高磁盘可靠性的技术
#### 8.4.1 第一级容错技术 SFT-I
- 双份目录和双份文件分配表
    - 主目录和主 FAT 备份目录和备份 FAT 
- 热修复重定向和写后读校验
    - 写完，读出写入另一个区域，对比二者
#### 8.4.2 第二级容错技术 SFT-II
- 磁盘镜像 
    - 数据写两次
- 磁盘双工
    - 双磁盘控制器和通道，可并行写入
#### 8.4.3 基于集群技术的容错技术
> 集群是一组互连的自主计算机组成的计算机系统
- 双机热备份
- 双机互为备份
- 公共磁盘模式
#### 8.4.3 后备系统
> 磁盘不够大，防止感染,重要数据后备
- 磁带机 GB ~ 10GB 
- 硬盘 
    - 移动硬盘 固定硬盘驱动器
- 光盘驱动器
### 8.5 数据一致性
> 多个文件中的同一数据
#### 8.5.1 事务
- 定义
    - 访问和修改数据项的一个程序单位
    - 只要一个读写修改失败需要回滚
    - 事务原子性 全做全不做 不可做不全
    - 事务一致性
    - 事务隔离性
    - 事务持久性
- 事务记录
    - 事务名
    - 数据项名
    - 旧值
    - 新址
- 恢复算法
    - undo 去旧数据
    - redo 再去新数据
#### 8.5.2 检查点
- 检查点的作用
    - 使事务记录表中的事务记录清理工作经常化
#### 8.5.3 并发控制
- 事务修改互斥，顺序性
- 利用互斥锁
- 利用互斥锁和共享锁
    - 只有互斥锁效率不高
    - 互斥锁只允许一个事务对象的对象执行读写
    - 共享锁允许多个事务对象读，但不允许任何一个事务对象写
#### 8.5.4 重复数据的一致性
- 文件一致性
- 链接数一致性