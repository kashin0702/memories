# tymk-third模块技术栈与核心逻辑分析

## 一、模块概述

### 1.1 模块定位
`tymk-third`是微厅业务系统中的第三方对接服务模块，主要负责与BOSS系统、河南联通AOP平台、客服系统等外部系统的集成对接。

### 1.2 基本信息
- **模块名称**: tymk-third
- **项目组**: com.tymk.third
- **版本**: 0.0.1-SNAPSHOT
- **服务端口**: 9093
- **上下文路径**: /third
- **JDK版本**: 1.8
- **构建工具**: Gradle

## 二、技术栈详情

### 2.1 核心框架
| 技术 | 版本 | 用途 |
|------|------|------|
| Spring Boot | 2.0.1.RELEASE | 基础框架 |
| Spring MVC | 内置 | Web框架 |
| MyBatis | 通过Spring Boot集成 | ORM持久层框架 |

### 2.2 WebService框架
| 技术 | 版本 | 用途 |
|------|------|------|
| Apache CXF | 3.1.6 | WebService客户端/服务端 |
| cxf-rt-frontend-jaxws | 3.1.6 | JAX-WS前端支持 |
| cxf-rt-transports-http | 3.1.6 | HTTP传输支持 |

### 2.3 数据库相关
| 技术 | 版本 | 用途 |
|------|------|------|
| Oracle JDBC | ojdbc6.jar | Oracle数据库驱动 |
| Redis | 通过Spring集成 | 缓存中间件 |

### 2.4 工具库
| 技术 | 版本 | 用途 |
|------|------|------|
| Fastjson | 1.2.54 | JSON序列化/反序列化 |
| Lombok | 1.18.6 | 代码简化（自动生成getter/setter等） |
| Dom4j | 2.0.1 | XML解析 |
| Commons Codec | 1.18.0 | 编解码工具（Base64、MD5等） |

### 2.5 日志框架
| 技术 | 版本 | 用途 |
|------|------|------|
| SLF4J | 1.7.28 | 日志门面 |
| Logback | 通过Spring Boot集成 | 日志实现 |

### 2.6 API文档
| 技术 | 版本 | 用途 |
|------|------|------|
| Springfox Swagger2 | 2.6.1 | API文档生成 |
| Springfox Swagger UI | 2.6.1 | API文档界面 |

### 2.7 第三方JAR包
- `hnaopapi-1.0.jar`: 湖南联通AOP平台对接SDK

### 2.8 模块依赖
- **tymk-password**: 密码加密/验证模块
- **tymk-common**: 公共工具模块（通过tymk-password间接依赖）

## 三、项目结构

```
tymk-third/
├── src/main/
│   ├── java/com/tymk/third/
│   │   ├── TymkThirdApplication.java         # 主启动类
│   │   ├── boot/                              # 启动配置包
│   │   │   └── BootConfig.java               # 配置文件加载
│   │   ├── controller/                        # 控制器层
│   │   │   └── OCNController.java            # OCN平台API控制器
│   │   ├── service/                           # 服务层
│   │   │   └── OCNService.java               # OCN核心业务服务
│   │   └── util/                              # 工具类包
│   │       └── HnAopHelper.java              # 河南联通AOP对接工具
│   └── resources/
│       ├── application.yml                    # 主配置文件
│       ├── logback.xml                        # 日志配置
│       ├── spy.properties                     # SQL监控配置
│       ├── lib/                               # 第三方JAR包
│       │   ├── hnaopapi-1.0.jar
│       │   └── ojdbc6.jar
│       └── props/                             # 多环境配置文件
│           ├── third-local.properties
│           ├── third-dev.properties
│           ├── third-test.properties
│           ├── third-ocn.properties
│           ├── third-prod.properties
│           ├── oracle-*.properties
│           └── spy-*.properties
├── build.gradle                               # Gradle构建配置
└── settings.gradle                            # Gradle设置
```

## 四、核心业务逻辑分析

### 4.1 主启动类配置

**TymkThirdApplication.java** 关键配置：

```java
@SpringBootApplication
@EnableSwagger2                          // 启用Swagger2
@MapperScan("com.tymk.third.mapper")    // MyBatis Mapper扫描
@EnableTransactionManagement            // 启用事务管理
@EnableAsync                            // 启用异步任务
public class TymkThirdApplication {
    
    // 异步任务线程池配置
    @Bean
    public Executor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(asyncNum);      // 核心线程数
        executor.setMaxPoolSize(asyncNum);       // 最大线程数
        executor.setQueueCapacity(200);          // 队列容量
        executor.setThreadNamePrefix("async-");   // 线程名前缀
        executor.initialize();
        return executor;
    }
    
    // Swagger UI配置
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.tymk.third.controller"))
                .paths(PathSelectors.any())
                .build();
    }
}
```

### 4.2 OCN核心业务服务（OCNService.java）

OCNService是与BOSS系统对接的核心服务类，包含60+个业务方法，主要功能分类如下：

#### 4.2.1 账单管理
- `queryBillSummary()`: 查询账单汇总
- `queryBillDetail()`: 查询账单明细
- `downloadBill()`: 下载账单
- `queryBillPaymentResult()`: 查询账单支付结果
- `billPaymentNotify()`: 账单支付通知

#### 4.2.2 余额与充值
- `queryAccountBalance()`: 查询账户余额
- `rechargeAccount()`: 账户充值
- `queryRechargeRecord()`: 查询充值记录
- `queryRechargeResult()`: 查询充值结果

#### 4.2.3 发票管理
- `queryInvoiceList()`: 查询发票列表
- `applyInvoice()`: 申请发票
- `queryInvoiceDetail()`: 查询发票详情
- `downloadInvoice()`: 下载发票

#### 4.2.4 订单管理
- `queryOrderList()`: 查询订单列表
- `queryOrderDetail()`: 查询订单详情
- `cancelOrder()`: 取消订单
- `submitOrder()`: 提交订单

#### 4.2.5 5G业务
- `query5GNumber()`: 查询5G号码
- `occupy5GNumber()`: 预占5G号码
- `release5GNumber()`: 释放5G号码
- `query5GPackage()`: 查询5G套餐
- `handle5GPackage()`: 办理5G套餐

#### 4.2.6 客户鉴权与密码
- `customerAuth()`: 客户鉴权
- `queryCustomerInfo()`: 查询客户信息
- `modifyPassword()`: 修改密码
- `resetPassword()`: 重置密码
- `validatePassword()`: 验证密码

#### 4.2.7 其他业务
- `queryProductCatalog()`: 查询产品目录
- `queryServiceStatus()`: 查询服务状态
- `handleComplaint()`: 处理投诉
- `queryComplaintList()`: 查询投诉列表

### 4.3 API控制器（OCNController.java）

OCNController提供80+个RESTful API接口，路径前缀为`/ocn`，所有接口集成Swagger文档。

#### 4.3.1 接口命名规范
- **查询接口**: `/query*` (GET)
- **操作接口**: `/handle*`, `/submit*`, `/apply*` (POST)
- **通知接口**: `/*Notify` (POST)

#### 4.3.2 典型接口示例

```java
@RestController
@RequestMapping("/ocn")
@Api(tags = "OCN平台接口")
public class OCNController {
    
    @Autowired
    private OCNService ocnService;
    
    @GetMapping("/queryBillSummary")
    @ApiOperation("查询账单汇总")
    public ResponseEntity<BillSummaryVO> queryBillSummary(
            @RequestParam String accountId,
            @RequestParam String month) {
        return ocnService.queryBillSummary(accountId, month);
    }
    
    @PostMapping("/billPayment")
    @ApiOperation("账单支付")
    public ResponseEntity<PaymentResultVO> billPayment(
            @RequestBody BillPaymentDTO dto) {
        return ocnService.billPayment(dto);
    }
}
```

### 4.4 河南联通AOP对接（HnAopHelper.java）

负责与湖南联通AOP平台的HTTP通信和签名验证。

#### 4.4.1 核心功能
1. **签名生成**: 使用MD5算法生成请求签名
2. **HTTP请求**: 发送POST请求到AOP平台
3. **响应解析**: 解析JSON响应数据
4. **异常处理**: 统一异常处理机制

#### 4.4.2 关键方法
```java
public class HnAopHelper {
    
    // 生成签名
    public static String generateSign(Map<String, String> params, String token) {
        // 参数排序 + token拼接 + MD5加密
    }
    
    // 发送HTTP请求
    public static String sendRequest(String url, String jsonData, String sign) {
        // 使用Apache HttpClient发送请求
    }
    
    // 解析响应
    public static Map<String, Object> parseResponse(String response) {
        // 使用Fastjson解析JSON响应
    }
}
```

## 五、配置说明

### 5.1 应用配置（application.yml）

```yaml
server:
  port: 9093              # 服务端口
  servlet:
    context-path: /third  # 上下文路径

spring:
  profiles:
    active: local         # 激活的环境配置（local/dev/test/ocn/prod）
  
  redis:
    # Redis配置（具体配置在对应环境的third-*.properties中）
  
  datasource:
    # 数据源配置（具体配置在对应环境的oracle-*.properties中）

mybatis:
  mapper-locations: classpath:mapper/*.xml
  type-aliases-package: com.tymk.third.entity
```

### 5.2 多环境配置

通过`BootConfig.java`加载不同环境的配置文件：

```java
@Configuration
@PropertySource({
    "classpath:props/third-${spring.profiles.active}.properties",
    "classpath:props/oracle-${spring.profiles.active}.properties",
    "classpath:props/spy-${spring.profiles.active}.properties"
})
public class BootConfig {
    // 配置文件加载
}
```

### 5.3 BOSS系统配置（third-local.properties示例）

```properties
# BOSS系统接口配置
boss.url=http://10.1.192.100:8080/boss
boss.app.id=OCN_TYMK
boss.app.token=xxxxxxxxxxxxxxxx
boss.timeout=30000

# 河南郑州系统配置
henan.aop.url=http://xxx.xxx.xxx.xxx:8080/aop
henan.aop.app.id=HENAN_OCN
henan.aop.app.token=yyyyyyyyyyyyyyyy

# 客服系统配置
customer.service.url=http://10.1.192.200:8080/cs
customer.service.timeout=10000

# 东方有线消息通知配置
dfyx.notify.url=http://xxx.xxx.xxx.xxx:8080/notify
dfyx.notify.enabled=true

# Mock开关（开发环境使用）
mock.enabled=true
mock.boss.enabled=true
mock.henan.enabled=false
```

### 5.4 日志配置（logback.xml）

- **日志级别**: INFO（生产环境）/ DEBUG（开发环境）
- **日志路径**: ./logs/third/
- **滚动策略**: 按天滚动，保留30天
- **日志格式**: `%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{50} - %msg%n`

## 六、构建与打包

### 6.1 Gradle配置要点

```gradle
// 依赖配置
dependencies {
    compile project(':tymk-password')              // 依赖密码模块
    compile('org.apache.cxf:cxf-rt-frontend-jaxws:3.1.6')
    compile('org.apache.cxf:cxf-rt-transports-http:3.1.6')
    compile group: 'com.alibaba', name: 'fastjson', version: '1.2.54'
    compile('io.springfox:springfox-swagger-ui:2.6.1')
    compile('io.springfox:springfox-swagger2:2.6.1')
    compile files('src/main/resources/lib/hnaopapi-1.0.jar',
                  'src/main/resources/lib/ojdbc6.jar')
    compile group: 'org.dom4j', name: 'dom4j', version: '2.0.1'
    compileOnly 'org.projectlombok:lombok:1.18.6'
    compile("commons-codec:commons-codec:1.18.0")
}

// 打包配置
jar {
    baseName = 'tymk-third-' + releaseTime()  // 添加时间戳
    manifest {
        attributes(
            'Class-Path': configurations.compile.collect { it.getName() }.join(' '),
            'Main-Class': 'com.tymk.third.TymkThirdApplication'
        )
    }
}

// 时间戳函数（Asia/Shanghai时区）
def releaseTime() {
    return new Date().format("yyyyMMddHHmmss", TimeZone.getTimeZone("Asia/Shanghai"))
}
```

### 6.2 打包命令

```bash
# 构建项目
gradle build

# 跳过测试构建
gradle build -x test

# 生成的JAR包名称格式
tymk-third-20260310171358.jar  # 包含时间戳
```

## 七、关键技术特点

### 7.1 异步任务处理
- 使用`@EnableAsync`和`@Async`注解实现异步任务
- 自定义线程池配置，可通过配置文件调整线程数
- 适用于耗时的BOSS系统调用、消息通知等场景

### 7.2 多环境支持
- 支持5种环境：local、dev、test、ocn、prod
- 通过`spring.profiles.active`切换环境
- 配置文件分离，便于维护和部署

### 7.3 WebService集成
- 使用Apache CXF框架
- 支持SOAP协议的WebService调用
- 便于与传统企业系统（如BOSS）对接

### 7.4 API文档自动化
- 集成Swagger UI
- 访问路径：`http://localhost:9093/third/swagger-ui.html`
- 支持在线测试API接口

### 7.5 安全性
- 依赖tymk-password模块进行密码加密
- 使用签名机制验证请求合法性（河南AOP对接）
- 支持Token认证

## 八、部署与运行

### 8.1 环境要求
- JDK 1.8+
- Oracle数据库
- Redis服务
- 可访问BOSS系统和河南AOP平台

### 8.2 启动命令

```bash
# 开发环境启动
java -jar tymk-third-20260310171358.jar --spring.profiles.active=local

# 生产环境启动
java -jar tymk-third-20260310171358.jar --spring.profiles.active=prod

# 指定JVM参数
java -Xms512m -Xmx1024m -jar tymk-third-20260310171358.jar
```

### 8.3 健康检查
- 端口检查：`http://localhost:9093/third`
- Swagger UI：`http://localhost:9093/third/swagger-ui.html`

## 九、模块特色与价值

### 9.1 核心价值
1. **第三方系统集成枢纽**: 统一管理与BOSS、河南AOP、客服系统等外部系统的对接
2. **业务解耦**: 将第三方对接逻辑独立出来，降低主业务模块的复杂度
3. **可扩展性**: 通过配置化和模块化设计，便于新增第三方系统对接

### 9.2 技术亮点
1. **WebService支持**: 使用Apache CXF框架，适配企业级系统集成
2. **异步处理**: 提升系统性能，避免阻塞主业务流程
3. **完善的API文档**: Swagger UI提供可视化API文档和在线测试
4. **多环境配置**: 灵活的配置管理，支持快速环境切换

## 十、总结

`tymk-third`模块是微厅业务系统中的关键第三方对接服务，采用Spring Boot + MyBatis + Apache CXF技术栈，提供了完善的BOSS系统对接、河南联通AOP平台集成、客服系统调用等功能。模块设计合理，配置灵活，具备良好的可维护性和扩展性，是整个系统与外部系统交互的重要桥梁。
