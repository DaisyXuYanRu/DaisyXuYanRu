### JMeter

#### JMeter 环境安装 （具体 DeepSeek）

1. 安装 JDK
   1. 官网下载：http://www.oracle.com/
   2. 配置环境变量
   3. 校验 cmd -》 java -version
2. 安装 JMeter
   1. 官网下载地址:http://jmeter.apache.org/download_jmeter.cgi **JMeter 版本与 JDK 版本匹配**
   2. 配置环境变量
   3. 校验
      1. 双击 jmeter.bat 打开
      2. windows 目录行 输入 cmd -》java -jar ApacheJMeter.jar
3. JMeter 目录

   1. lib 目录 存放 JMeter 依赖的 jar 包和用户扩展所依赖的 jar 包

4. 汉化
   1. 启动 JMeter -> 选择菜单‘Options’ -> Choose Language->Chinese (Simplified)
   2. 第二种： 找到 jmeter 安装目录下的 bin 目录 -》 打开 jmeter.properties 文件，把第 37 行修改为“language=zh_CN” -》 重启 JMeter
5. 修改主题： 选项 -》 外观 -》 主题

#### JMeter 元件（比喻编程的类）和组件（比喻编程的类中的函数）

![元件](元件.png)

1. 作用域的原则：

- 取样器:核心，没有作用域
- 逻辑控制器: 只对其子节点中的取样器和逻辑控制器起作用
- 其他元件:
  - 如果是某个取样器的子节点，则该元件只对其父节点起作用
  - 如果其父节点不是取样器，则其作用域是该元件父节点下的其他所有后代节点(包括子节点，子节点的子节点等)

2. 元件执行顺序:
   1. 同一作用域下不同元件: 配置元件 - 前置处理程序 - 定时器 - 取样器 - 后置处理程序 - 断言 - 监听器
   2. 同一作用域下相同元件: 从上到下的顺序依次执行

![执行顺序](执行顺序.png)

`定时器1 - HTTP请求1 - 定时器1 - 定时器2 - HTTP请求2 - 定时器1 - 定时器3 - HTTP请求3`

#### 线程组

![线程组](线程组.png)

- `线程组：控制Jmeter用于执行测试的一组用户`
- 线程组的分类
  - Setup 线程组：预测试操作，所有脚本之前执行
  - 普通线程组：执行测试用例，可以有 1 个或者多个（并行/串行）
  - Teardown 线程组：测试后操作，所有脚本之后执行

#### HTTP 请求

![HTTP 请求](HTTP请求.png)

#### 查看结果树

#### JMeter 参数化

`本质：使用参数的方式来替代脚本中的固定的测试数据`

##### 用户定义的变量

`测试计划 --> 线程组--> 配置元件 --> 用户定义的变量`
`引用定义的变量名。格式：${变量名}`

##### 前置处理器 --> 用户参数

1. `作用：针对同一组参数，当不同的用户来访问时，可以获取到不同的值`
2. `引用定义的变量名。格式：${变量名}`
3. 线程组的线程数 就是 用户数

##### CSV 数据文件设置

1. 作用：让不同用户在多次循环时，可以取到不同的值
2. 位置：测试计划 --> 线程组--> 配置元件 --> CSV 数据文件设置
   ![CSV 数据文件设置](CSV数据文件设置.png)
3. `引用定义的变量名。格式：${变量名}`
4. 操作步骤：3 行数据

   1. 定义 CSV 数据文件
   2. 添加线程组，设置循环次数为 3
   3. 添加 CSV 数据文件设置
   4. 添加 HTTP 请求
   5. 添加查看结果树

##### 函数的方式

1. 作用：保证不同的用户及多次循环时，都可以取到不同的值，不需要提前设置
2. 位置：在菜单中选择--> 选项 --> 函数助手对话框

#### JMeter 断言

`断言：让程序自动判断预期结果和实际结果是否一致。`

##### 响应断言

1. 作用：对 HTTP 请求的任意格式的响应结果进行断言
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 断言 --> 响应断言
3. 参数配置详细介绍

   - 响应文本: 来自服务器的响应文本，即主体
   - 响应代码: 响应的状态码，例如：200
   - 响应信息: 响应的信息，例如：OK
   - Response Headers: 响应头部
   - Request Headers: 请求头部
   - Request Data: 请求数据
   - URL 样本: 请求 URL
   - Document(text): 响应的整个文档
   - 忽略状态：忽略返回的响应状态码

4. 比较方式 `注意：Equals和Substring模式是普通字符串，而包括和匹配模式是正则表达式`
   - 包括：文本包含指定的正则表达式
   - 匹配：整个文本匹配指定的正则表达式
   - Equals：整个返回结果的文本等于指定的字符串(区分大小写)
   - Substring：返回结果的文本包含指定字符串(区分大小写)
   - 否：取反
   - 或者：如果存在多个测试模式，勾选代表逻辑或（只要有一个模式匹配，则断言就是 OK），不勾选代表逻辑与（所有都必须匹配，断言才是 OK）

##### JSON 断言

1. 作用：对 HTTP 请求的 JSON 格式的响应结果进行断言
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 断言 --> JSON 断言
3. 参数介绍
   - Assert JSON Path exists：用于断言的 JSON 元素的路径（实际结果）
   - Additionally assert value：如果您想要用某个值生成断言，请选择复选框 `$.data.name`
   - Match as regular expression：使用正则表达式断言
   - Expected Value：期望值（期望结果）
   - Expect null：如果希望为空，请选择复选框
   - Invert assertion (will fail if above conditions met)：反转断言(如果满足以上条件则失败)

##### 持续时间断言(Duration Assertion)

1. 作用：检查 HTTP 请求的响应时间是否超出要求范围
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 断言 --> 断言持续时间
3. 参数介绍：持续时间（毫秒）：HTTP 请求允许的最大响应时间（单位：毫秒）。 超过则认为失败

#### JMeter 依赖关系的请求 - 关联

`关联：当请求之间有依赖关系，比如一个请求的入参是另一个请求返回的数据，这时候就需要用到关联处理。`

1. JMeter 中常用的关联方法：

- 正则表达式提取器
- XPath 提取器
- JSON 提取器

##### 正则表达式提取器

1. 作用：针对任意格式的响应数据进行提取
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 后置处理器 --> 正则表达式提取器
3. 参数介绍：

- 引用名称：存放提取出的值的参数名称，供下一个请求引用，如填写 title，则可用${title}引用它
- 正则表达式: 左边界(.\*?)右边界
  - ()：括起来的部分就是要提取的。
  - .：匹配任何字符串。
  - \*：0 次或多次。
  - ?：在找到第一个匹配项后停止。
- 模板：用$$引用起来，如果在正则表达式中有多个提取值，则可以是$2$$3$等等，表示解析到的第几个值给 title。如：$1$表示解析到的第 1 个值
- 匹配数字：0 代表随机取值，-1 代表全部取值，1 代表取第一个值
- 缺省值：如果参数没有取得到值，那默认给一个值让它取。
  ![正则表达式提取器](正则表达式提取器.png)

##### xpath 提取器

1. 作用：针对 HTML 格式的响应结果数据进行提取
2. 位置：添加方式：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 后置处理器--> XPath 提取器
3. 参数介绍

   - Use Tidy (tolerant parser)：

     - 当需要处理的页面是 HTML 格式时，必须选中该选项
     - 当需要处理的页面是 XML 或 XHTML 格式时，取消选中该选项。

   - 引用名称：存放提取出的值的参数名称
   - XPath Query：用于提取值的 XPath 表达式
   - 匹配数字：如果 XPath 路径查询出许多结果，则可以选择提取哪个。 0：表示随机，-1：表示提取所有结果，1 表示第一个值
   - 缺省值：参数的默认值

##### JSON 提取器

1. 作用：针对 JSON 格式的响应结果数据进行提取
2. 位置：添加方式：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 后置处理器--> JSON 提取器
3. 参数介绍：
   - Names of created variables：存放提取出的值的参数名称
   - JSON Path Expressions：JSON 路径表达式 `$.data.name`
   - Match No：如果 JSON 路径匹配出许多结果，则可以选择提取哪个。0：表示随机，-1：表示提取所有结果，1 表示第一个值
   - Default Values：参数的默认值

##### JMeter 属性

`不同线程组中，数据传递方式`
`__setProperty函数执行(保存JMeter属性):需要通过BeanShell取样器来执行`
`__property函数（读取属性） : 在其他线程组中使用property函数`

#### 自动录制脚本

`在没有接口文档的旧项目当中，通过录制http接口请求的方式，来快速编写接口测试脚本。`

1.  添加 HTTP 代理服务器，并进行配置
    - 设置端口：代理服务器的端口号
    - 目标控制器：录制的脚本放到哪个容器（线程组）中
    - Requests Filtering（过滤条件 – url 匹配正则表达式）:
      - 包含模式：包含此项。如：._localhost._ ；
      - 排除模式：不包含此项 如：._.jpg._.png .\*.js
2.  开启 windows 操作系统的浏览器代理

    - 配置浏览器代理：JMeter 代理服务器的 IP、端口号

3.  启动代理服务器，开始录制
4.  在浏览器页面中进行操作，成功后，就能在 JMeter 当中看到抓取到的接口请求了

#### JMeter 直连数据库

1. 直连数据库的使用场景
   - 用作请求的参数化 - 登录时需要的用户名，可以从数据库中查询获取
   - 用作结果的断言 - 添加购物车下订单，检查接口返回的订单号，是否与数据库中生成的订单号一致
   - 准备测试数据 - 例如：通过数据库来准备大量（几十万条）的性能测试数据
   - 清理垃圾数据
2. 直连数据库的关键配置：添加 MySQL 驱动 jar 包
   - 方式一：在测试计划面板点击“浏览…“按钮，将你的 JDBC 驱动添加进来
   - 方式二：将 MySQL 驱动 jar 包放入到 lib/ext 目录下，重启 JMeter
3. 配置数据库连接信息 `添加方式：测试计划 --> 线程组--> (右键添加) 配置元件 --> JDBC Connection Configuration`
4. 参数介绍：
   - Variable Name: mysql 数据库连接池名称（JDBC 请求时要引用）
   - Database URL: jdbc:mysql://localhost:3306/数据库名称
     - 组成：协议 + 数据库 IP + 数据库端口+ 连接的数据库名称
   - JDBC DRIVER class: com.mysql.jdbc.Driver（MySQL 驱动包位置固定格式 —— 下拉框选择）
   - Username: root(连接数据库用户名，如实填写)
   - Password：（MySQL 数据库密码，如实填写，如果密码为空不写）
5. 添加 JDBC 请求 `添加方式：测试计划 --> 线程组--> 取样器 --> JDBC Request`
6. 参数介绍：
   - Variable Name：数据库连接池的名字，需要与 JDBC Connection Configuration 的 Variable Name Bound Pool 名字保持一致
   - Query Type：
     - 查询操作：选择“Select Statement”
     - 增加、删除、修改操作：选择“Update Statement”
   - Query：填写的 SQL 语句，未尾不要加“;”
   - Variable names：保存 SQL 语句返回结果的变量名

![JDBC Connection](JDBCConnection.png)
![Query](Query.png)

#### JMeter 逻辑控制器

`常用的逻辑控制器：`

- 如果（If）控制器
- 循环控制器
- ForEach 控制器

##### （If）控制器

1. 作用：If 控制器用来控制它下面的测试元素是否运行
2. 位置：测试计划 --> 线程组--> (右键添加) 逻辑控制器 --> 如果（If）控制器
3. 参数介绍
   - JS 语法形式
   - 函数形式 - 勾选 Interpret Condition...

##### 循环控制器

##### ForEach 控制器

1. 作用：一般和用户自定义变量或者正则表达式提取器一起使用，读取返回结果中一系列相关的变量值。该控制器下的取样器都会被执行一次或多次，每次读取不同的变量值。
2. 位置：测试计划 --> 线程组--> (右键添加) 逻辑控制器 --> ForEach 控制器
3. 参数介绍：
   - 输入变量前缀：要读取的输入变量的固定前缀 （name）
   - 开始循环字段：要读取的输入变量后缀数字的最小值-1 (0)
   - 结束循环字段：要读取的输入变量后缀数字的最大值 (2)
   - 输出变量名称：读取输入变量的值后保存的新变量名，(varName)
4. 添加用户定义变量 name_1,name_2,name_3; 引用 ForEach 控制器中保存的新变量名，如：${varName}

#### JMeter 定时器

##### 同步定时器

1. 阻塞线程（累积一定的请求），当在规定的时间内达到一定的线程数量，这些线程会在同一个时间点一起释放，瞬间产生很大的压力。
   `提示：在JMeter中叫做同步定时器，在Loadrunner中又叫集合点`
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 定时器 --> Synchronizing Timer
3. 参数介绍
   - Number of Simulated Users to Group by：模拟用户的数量，即指定同时释放的线程数数量。
     - 若设置为 0，等于设置为线程组中的线程数量
   - Timeout in milliseconds：超时时间，即超时多少毫秒后同时释放指定的线程数；
     - 如果设置为 0，该定时器将会等待线程数达到了设置的线程数才释放，若没有达到设置的线程数会一直死等
     - 如果大于 0，那么如果超过 Timeout in milliseconds 中设置的最大等待时间后还没达到设置的线程数，Timer 将不再等待，释放已到达的线程。默认为 0

##### 常数吞吐量定时器

1. 作用：让 JMeter 按指定的吞吐量执行，以每分钟为单位。
2. 位置：测试计划 --> 线程组--> HTTP 请求 --> (右键添加) 定时器 --> Constant Throughput Timer
3. 参数介绍：
   - Target throughput（in samples per minute）：目标吞吐量。注意这里是每个用户每分钟发送的请求数
4. 案例

- 模拟用户真实的业务场景要求：20 QPS
- 如果线程数设置为 1，则目标吞吐量设置为 20 \* 60 = 1200
- 如果线程数设置为 2，则目标吞吐量设置为 20 \* 60 / 2 = 600

#### JMeter 分布式

JMeter 分布式测试时，选择其中一台作为控制机(Controller)，其它机器做为代理机(Agent)。

- 执行时，控制机会把脚本发送到每台代理机上
- 代理机拿到脚本后就开始执行，代理机执行时不需要启动 JMeter 界面，可以理解它是通过命令行模式执行的。
- 执行完成后，代理机会把结果回传给控制机，控制机会收集所有代理机的信息并汇总。

`单台测试机无法满足用户要求的负载量时，使用多台机器来模拟`

#### JMeter 测试报告

##### 聚合报告

1. 作用：收集性能测试结束后，系统的各项性能指标。如：响应时间、并发数、吞吐量、错误率等
2. 位置：测试计划->右键->监听器->聚合报告
3. 参数介绍：
   - Label：每个请求的名称
   - 样本：各请求发出的数量
   - 平均值：平均响应时间（单位：毫秒）
   - 中位数：中位数，50% <= 时间
   - 90%百分比：90% <= 时间
   - 95%百分比：95% <= 时间
   - 99%百分比：99% <= 时间
   - 最小值：最小响应时间
   - 最大值：最大响应时间
   - 异常%：请求的错误率
   - 吞吐量：吞吐量。默认情况下表示每秒完成的请求数，一般认为它为 TPS。
   - 接收 KB/sec：每秒接收到的千字节数
   - 发送 KB/sec：每秒发送的千字节数

##### html 测试报告

1. 作用：JMeter 支持生成 HTML 测试报告，以便从测试计划中获得图表和统计信息
2. 命令：

```shell
# jmeter -n -t [jmx file] -l [result file] -e -o [html report folder]

# eg:
jmeter -n -t hello.jmx -l result.jtl -e -o ./report
```

3. 参数描述：

- -n：非 GUI 模式执行 JMeter
- -t [jmx file]：测试计划保存的路径及.jmx 文件名，路径可以是相对路径也可以是绝对路径
- -l [result file]：保存生成测试结果的文件，jtl 文件格式
- -e：测试结束后，生成测试报告
- -o [html report folder]：存放生成测试报告的路径，路径可以是相对路径也可以是绝对路径

`注意：result.jtl和report会自动生成，如果在执行命令时result.jtl和report已存在，必须用先删除，否则在运行命令时就会报错`

#### 稳定性测试并发数 TPS 的计算方法

`普通计算方法：TPS = 总请求数 / 总时间`

1. 案例
   `PV:（Page View）即页面访问量，每打开一次页面PV计数+1，刷新页面也是。PV只统计页面访问次数。`
   `数据分析：`
   - 根据数据统计，在 2019 年第 32 周，日均 PV 为 4.13 万，可以估算为 1 天有 4.13 万请求（1 次浏览都至少对应 1 个请求）总请求数 = 4.13 万请求数 = 41300
   - 总时间 = 1 天 = 1 _ 24 小时 = 24 _ 3600 秒
     `套入公式：`
   - TPS = 41300 请求数/24\*3600 秒 = 0.48 请求数/秒
   - 结论：按照普通计算方法，理论上每秒能够处理 0.48 请求，就可以满足线上的需要。
2. 二八原则计算方法：
   `二八原则就是指80%的请求在20%的时间内完成。`
   `计算公式：TPS = 总请求数 * 80% / (总时间*20%)`
   `套入公式：TPS = 41300 * 0.8请求数 / 24*3600*0.2秒 = 1.91 请求数/秒`
   `二八原则的估算结果会比平均值的计算方法更能满足用户需求`

#### 压力测试的并发数 TPS: 峰值请求数/峰值时间 \* 系数(3)

#### JMeter 下载第三方插件

1. 下载插件管理器
2. 下载包管理工具 jar 包
3. 将包管理工具 jar 包添加到 JMeter 放入到 lib\ext 目录下
4. 重启 JMeter
5. 下载第三方插件：
   1. 打开 Plugins Manager 插件管理器
   2. 选择 Available Plugins，当前可用的插件
   3. 选择需要下载的插件（等待右方文本内容展示出来）
   4. 下载右下角的下载按钮，自动的完成下载，JMeter 会自动重启

#### 性能测试常用图表

##### Concurrency Thread Group 线程组

1. 阶梯线程组：作用是阶梯加压；图形界面显示运行状态
2. 添加方式：测试计划 --> 线程（用户）--> Concurrency Thread Group
3. 参数介绍：
   - Target Concurrency：目标并发（线程数）
   - Ramp Up Time：加速时间- Ramp-Up Steps Count：加速步骤计数
   - Hold Target Rate Time：运行时间
   - Time Unit：时间单位（分钟或者秒）
   - Thread Iterations Limit：线程循环次数
   - Log Threads Status into File：日志记录

##### Transactions per Second

1. 每秒完成事务数：作用是统计各个事务每秒钟成功的事务个数
2. 添加方式：测试计划 --> 线程组--> 监听器-->Transactions per Second
3. Transactions per Second 和聚合报告中的 TPS 在性能测试时的作用有何不同，以哪个为准
   1. 性能测试的结果统计，以聚合报告的结果为准
   2. 每秒性能指标的作用是：查看系统长时间运行过程中是否有异常出现，有则进一步分析

##### Bytes Throughput per Second

1. 每秒字节吞吐量：作用是查看服务器吞吐流量（单位/字节）
2. 添加方式：测试计划 --> 线程组--> 监听器-->Bytes Throughput Over Time

##### PerfMon 组件监控服务器资源

1. 作用：用来监控服务端的性能资源指标的工具，包括 cpu、内存、磁盘、网络等性能数据
2. 添加方法：线程组->监听器->jp@gc - PerfMon Metrics Collector
3. 注意：使用之前需要在服务器端安装监听服务程序并启动
4. 监控服务器资源指标的步骤：
   1. 下载安装包 ServerAgent-2.2.3.zip，链接地址：https://github.com/undera/perfmon-agent
   2. 上传到服务器上，并解压 ServerAgent-2.2.3.zip
   3. 启动，如果是 windows 运行 startAgent.bat，如果是 linux 运行 startAgent.sh
   4. 启动这个工具后，jmeter 的插件 jp@gc - PerfMon Metrics Collector 就可以收集服务端的资源使用率，并在 jmeter 中查看了
