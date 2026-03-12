# tymk-front模块技术栈与核心逻辑分析

## 一、模块概述

### 1.1 模块定位
`tymk-front`是微厅业务系统的前端业务服务模块，主要负责微信公众号/小程序的H5页面展示、用户交互、业务办理等核心功能。作为用户直接接触的前端服务层，该模块整合了账单查询、余额充值、套餐办理、5G业务、教育服务等多种业务功能。

### 1.2 基本信息
- **模块名称**: tymk-front
- **项目组**: com.tymk.front
- **版本**: 0.0.1-SNAPSHOT
- **服务端口**: 9095
- **上下文路径**: /front
- **JDK版本**: 1.8
- **构建工具**: Gradle

## 二、技术栈详情

### 2.1 核心框架
| 技术 | 版本 | 用途 |
|------|------|------|
| Spring Boot | 2.0.1.RELEASE | 基础框架 |
| Spring MVC | 内置 | Web框架 |
| MyBatis | 通过Spring Boot集成 | ORM持久层框架 |

### 2.2 模板引擎
| 技术 | 版本 | 用途 |
|------|------|------|
| FreeMarker | 通过Spring Boot集成 | 服务端模板引擎，生成H5页面 |

### 2.3 数据库相关
| 技术 | 版本 | 用途 |
|------|------|------|
| MySQL | 通过Spring Boot集成 | 关系型数据库 |
| Redis | 通过Spring集成 | 缓存中间件 |
| HikariCP | 内置 | 数据库连接池 |

### 2.4 工具库
| 技术 | 版本 | 用途 |
|------|------|------|
| Fastjson | 1.2.54 | JSON序列化/反序列化 |
| Lombok | 1.18.6 | 代码简化（自动生成getter/setter等） |
| Commons Net | 3.6 | 网络工具库（FTP等） |

### 2.5 日志框架
| 技术 | 版本 | 用途 |
|------|------|------|
| SLF4J | 1.7.28 | 日志门面 |
| Logback Core | 1.1.3 | 日志核心实现 |
| Logback Classic | 1.1.3 | 日志经典实现 |

### 2.6 API文档
| 技术 | 版本 | 用途 |
|------|------|------|
| Springfox Swagger2 | 2.6.1 | API文档生成 |
| Springfox Swagger UI | 2.6.1 | API文档界面 |

### 2.7 代码生成工具
| 技术 | 版本 | 用途 |
|------|------|------|
| MyBatis Generator | 1.3.2 | MyBatis代码生成器 |
| mybatis-generator-plugin | 1.4 | Gradle插件版本 |

### 2.8 模块依赖
- **tymk-password**: 密码加密/验证、用户拦截器模块
- **tymk-common**: 公共工具模块（枚举、帮助类等）

## 三、项目结构

```
tymk-front/
├── src/main/
│   ├── java/com/tymk/front/
│   │   ├── TymkFrontApplication.java         # 主启动类
│   │   ├── boot/                              # 启动配置包
│   │   │   └── FreeMarkerConfig.java         # FreeMarker配置
│   │   ├── controller/                        # 控制器层（25个）
│   │   │   ├── ApiController.java            # API总控制器
│   │   │   ├── PageRouteController.java      # 页面路由控制器
│   │   │   ├── UserController.java           # 用户管理
│   │   │   ├── BillController.java           # 账单管理
│   │   │   ├── BalanceController.java        # 余额管理
│   │   │   ├── BindController.java           # 绑定管理
│   │   │   ├── InvoiceController.java        # 发票管理
│   │   │   ├── OfferController.java          # 套餐管理
│   │   │   ├── Wx5GController.java           # 5G业务
│   │   │   ├── EduController.java            # 教育服务
│   │   │   ├── PayController.java            # 支付管理
│   │   │   ├── WxMpController.java           # 微信公众号
│   │   │   └── ...                           # 其他控制器
│   │   ├── service/                           # 服务层（27个）
│   │   │   ├── BaseService.java              # 基础服务类
│   │   │   ├── UserService.java              # 用户服务
│   │   │   ├── BillService.java              # 账单服务
│   │   │   ├── BalanceService.java           # 余额服务
│   │   │   ├── BindService.java              # 绑定服务
│   │   │   ├── InvoiceService.java           # 发票服务
│   │   │   ├── OfferService.java             # 套餐服务
│   │   │   ├── Wx5GService.java              # 5G业务服务
│   │   │   ├── EduService.java               # 教育服务
│   │   │   └── ...                           # 其他服务
│   │   ├── task/                              # 定时任务包
│   │   │   └── HallSyncTask.java             # 营业厅同步任务
│   │   ├── mapper/                            # MyBatis Mapper接口
│   │   ├── model/                             # 数据模型
│   │   ├── helper/                            # 帮助类
│   │   └── enums/                             # 枚举类
│   └── resources/
│       ├── application.yml                    # 主配置文件
│       ├── logback.xml                        # 日志配置
│       ├── spy.properties                     # SQL监控配置
│       ├── generatorConfig.xml                # MyBatis Generator配置
│       ├── web-pattern.yaml                   # Web模式配置
│       ├── mapper/                            # MyBatis XML映射文件
│       ├── props/                             # 多环境配置文件
│       │   ├── local/front.properties
│       │   ├── dev/front.properties
│       │   ├── test/front.properties
│       │   ├── ocn/front.properties
│       │   └── prod/front.properties
│       ├── static/                            # 静态资源（CSS、JS、图片等）
│       └── templates/                         # FreeMarker模板（H5页面）
│           ├── acct/                          # 账户相关页面
│           ├── bill/                          # 账单相关页面
│           ├── business/                      # 业务办理页面
│           ├── cust/                          # 客户相关页面
│           ├── edu/                           # 教育服务页面
│           ├── 5G/                            # 5G业务页面
│           ├── member/                        # 会员相关页面
│           ├── offer/                         # 套餐相关页面
│           ├── remind/                        # 提醒相关页面
│           ├── selfService/                   # 自助服务页面
│           └── ...                            # 其他业务页面
├── build.gradle                               # Gradle构建配置
└── settings.gradle                            # Gradle设置
```

## 四、核心业务逻辑分析

### 4.1 主启动类配置

**TymkFrontApplication.java** 关键配置：

```java
@Slf4j
@EnableSwagger2                                    // 启用Swagger2
@SpringBootApplication
@EntityScan(basePackageClasses = {TymkFrontApplication.class})
@ComponentScan(basePackages ={"com.tymk"})        // 扫描com.tymk包下所有组件
@MapperScan({"com.tymk.front","com.tymk.common"}) // MyBatis Mapper扫描
@EnableTransactionManagement                       // 启用事务管理
@EnableAsync                                       // 启用异步任务
public class TymkFrontApplication {
    
    // 环境参数配置
    private static final String ENV_EX = "--spring.profiles.active=";
    private static final String ENV_PROPERTY_NAME ="user.env";
    
    public static void main(String[] args) {
        // 解析环境参数
        String env = Env.LOCAL.name();
        if (args != null && args.length > 0) {
            for (String arg : args) {
                if (null != arg && arg.startsWith(ENV_EX)) {
                    env = arg.substring(ENV_EX.length());
                }
            }
        }
        
        // 设置系统属性
        System.setProperty(ENV_PROPERTY_NAME, env.toLowerCase());
        System.setProperty("sun.net.client.defaultConnectTimeout", "2000");
        System.setProperty("sun.net.client.defaultReadTimeout", "1500");
        System.setProperty("user.timezone", "GMT+8");
        TimeZone.setDefault(TimeZone.getTimeZone("GMT+8"));
        
        SpringApplication.run(TymkFrontApplication.class, args);
    }
    
    // Swagger UI配置
    @Bean
    public Docket api() {
        List<Parameter> pars = new ArrayList<Parameter>();
        
        // 添加全局Header参数
        ParameterBuilder Authorization = new ParameterBuilder();
        Authorization.name("Authorization")
            .defaultValue("NUYzOTEzOTU2QjFFQUVFQUUyNzE2QzYyRjBBMDM0OTk=")
            .description("用户token")
            .modelRef(new ModelRef("string"))
            .parameterType("header")
            .required(false)
            .build();
            
        ParameterBuilder zx = new ParameterBuilder();
        zx.name("zx")
            .defaultValue("1")
            .description("公众号标识")
            .modelRef(new ModelRef("string"))
            .parameterType("header")
            .required(false)
            .build();
            
        pars.add(Authorization.build());
        pars.add(zx.build());
        
        // 生产环境禁用Swagger
        if("prod".equals(System.getProperty("user.env"))){
            return new Docket(DocumentationType.SWAGGER_2)
                .groupName("tymk")
                .apiInfo(metadata())
                .enable(false)
                .select()
                .paths(PathSelectors.none())
                .apis(RequestHandlerSelectors.basePackage("com.tymk"))
                .build();
        } else {
            return new Docket(DocumentationType.SWAGGER_2)
                .groupName("tymk")
                .apiInfo(metadata())
                .enable(false)
                .select()
                .paths(regex("/*.*"))
                .apis(RequestHandlerSelectors.basePackage("com.tymk"))
                .build()
                .globalOperationParameters(pars);
        }
    }
    
    public ApiInfo metadata() {
        return new ApiInfoBuilder()
            .title("微信front接口api")
            .description("接口url= host:port/front/${path}")
            .version("1.0")
            .build();
    }
}
```

### 4.2 核心控制器分类（25个）

#### 4.2.1 用户与账户管理
- **UserController**: 用户管理（绑定手机号、解绑、查询费用、充值记录等）
- **BindController**: 客户地址绑定管理
- **TempAuthController**: 临时授权管理
- **SignController**: 签到功能

#### 4.2.2 账单与支付
- **BillController**: 账单查询与管理
- **BalanceController**: 余额查询与充值
- **PayController**: 支付管理（微信支付、订单管理）
- **InvoiceController**: 发票申请与下载

#### 4.2.3 业务办理
- **OfferController**: 套餐办理与查询
- **Wx5GController**: 5G业务（号码预占、套餐办理、密码管理等）
- **BusinessOutletsReserveController**: 营业网点预约
- **EcodeController**: 会员权益码管理

#### 4.2.4 特色服务
- **EduController**: 教育服务（空中课堂、签到打卡）
- **UserRightsController**: 用户权益中心
- **ShortcutController**: 快捷入口管理
- **KfMsgController**: 客服消息管理

#### 4.2.5 系统功能
- **ApiController**: API总控制器
- **PageRouteController**: 页面路由控制器（FreeMarker页面跳转）
- **WxMpController**: 微信公众号接口
- **HealthController**: 健康检查
- **CommonFuncController**: 通用功能
- **SettingController**: 系统设置
- **SmsController**: 短信发送
- **PicCheckCodeCtrl**: 图片验证码
- **GwController**: 网关控制器

### 4.3 核心服务层分析（27个）

#### 4.3.1 BaseService（基础服务类）
提供所有Service的基础功能：
- `postAppJson()`: 发送JSON格式的HTTP POST请求
- `passThirdFail()`: 处理第三方接口失败响应

#### 4.3.2 用户与绑定服务

**UserService** 核心功能：
- 手机号绑定/解绑
- 实时费用查询
- 剩余流量套餐查询
- 充值记录查询
- 历史账单查询
- 积分商城跳转

**BindService** 核心功能（30+个方法）：
- 发送验证码短信
- 查询电子账单客户地址
- 修改电子账单状态
- 查询用户绑定的客户地址列表
- 查询默认绑定地址
- 绑定/解绑客户地址
- 更新默认地址
- 客户信息查询与缓存
- 用户卡片信息查询
- 5G加油包查询
- 数据脱敏处理（姓名、地址、手机号）

#### 4.3.3 账单与支付服务

**BillService** 核心功能：
- 查询OCN账单列表
- 缓存账单项查询

**BalanceService** 核心功能：
- 查询账户余额
- 多维度余额查询（按账户ID、客户地址ID等）
- 余额信息VO转换

**InvoiceService** 核心功能：
- 查询所有发票（电子发票、纸质发票、缴费通知单）
- 生成电子发票申请
- 发票下载

**RechargeOrderService** 核心功能：
- 生成订单号/流水号
- 微信JSAPI统一下单
- 创建支付订单
- 保存充值订单（账单、余额、套餐）
- 订单状态更新
- 订单错误日志记录

#### 4.3.4 套餐与业务服务

**OfferService** 核心功能（30+个方法）：
- 预约装机
- 套餐变更订单
- 机顶盒装机
- 重跑订单
- 查询套餐列表（按类型、会员套餐等）
- 查询已订购套餐
- 工单查询
- 套餐资格校验
- 优惠券校验
- 套餐费用计算
- 合约套餐查询

**PackageService** 核心功能：
- 套餐信息管理

**FeePackageService** 核心功能：
- 查询费用套餐
- 查询剩余套餐

**FlowPckService** 核心功能：
- 流量包管理（查询、添加）

**FlowOrderService** 核心功能：
- 流量订购
- 流量订单取消

#### 4.3.5 5G业务服务

**Wx5GService** 核心功能（50+个方法）：
- 证件类型枚举查询
- 实名认证准入检查
- 本地黑名单检查
- 客户鉴权
- 号码查询与预占
- 释放预占号码
- 套餐查询与办理
- 套餐变更校验
- 账单查询（话费、账单、话单）
- 可选包查询
- 套餐详情查询
- 密码校验/重置/修改
- 发票信息查询
- 用户资源查询
- 融合产品查询
- 违约金查询
- 客户信息查询
- 充值订单创建
- 支付订单处理
- 营业网点预约/取消

#### 4.3.6 教育服务

**EduService** 核心功能（40+个方法）：
- 学生/教师信息保存与查询
- 空中课堂用户缓存
- 学生/教师身份校验
- 产品实例ID校验
- 签到时长查询（按日期段、按天）
- 签到记录插入
- 每日签到分段查询
- 注册信息保存与更新
- 打卡功能
- MAC地址查询产品实例ID
- 客户信息查询
- MAC签名校验
- 数据脱敏
- 教师区域名称查询
- 二维码查询
- 冠军榜查询

#### 4.3.7 其他业务服务

**CommonFuncService** 核心功能：
- 报修故障信息查询
- 故障上报
- 授权刷新
- 营业网点查询
- 区域查询
- 自检视频查询
- 会员等级查询
- 会员促销活动查询
- 客服系统对接
- 雅医配置查询

**SettingService** 核心功能：
- 查询中心菜单配置
- 查询产品订单配置

**SmsService** 核心功能：
- 发送短信验证码
- 发送次数限制校验

**EcodeService** 核心功能：
- 会员权益码查询
- 领取会员权益码
- 会员子等级定义查询
- BOSS套餐信息查询

**AdvertService** 核心功能：
- 广告位查询

**BusinessOutletsReserveService** 核心功能：
- 营业网点查询
- 预约日期查询
- 预约记录查询
- 预约取消
- 预约数量统计

**ResetOrCheckPasswordService** 核心功能：
- 密码重置
- 密码校验

**UploadAndDownloadService** 核心功能：
- Base64图片上传
- 通用图片上传

**UserBindPostProcessService** 核心功能：
- 绑定后置处理
- 积分商城注册

### 4.4 定时任务

**HallSyncTask** (营业厅同步任务)：
- 定时同步营业网点数据
- 使用Spring的`@Scheduled`注解实现定时调度

### 4.5 FreeMarker模板页面

模板按业务分类存放在`templates/`目录下，主要包括：

- **acct/**: 账户相关页面
- **bill/**: 账单查询页面
- **business/**: 业务办理页面
- **cust/**: 客户信息页面
- **edu/**: 教育服务页面（空中课堂）
- **5G/**: 5G业务办理页面
- **member/**: 会员中心页面
- **offer/**: 套餐办理页面
- **remind/**: 账单提醒、总账单列表页面
- **selfService/**: 自助服务页面（报修受理等）
- **stb/**: 机顶盒相关页面
- **dongao/**: 东方有线专题页面

## 五、配置说明

### 5.1 应用配置（application.yml）

```yaml
web:
  port: 9095              # 服务端口
  path: /front            # 上下文路径

spring:
  application:
    name: tymk-front
  profiles:
    active: local         # 激活的环境配置（local/dev/test/ocn/prod）
  
  http:
    encoding:
      charset: UTF-8      # 字符编码
      force: true
      enabled: true
  
  datasource:
    hikari:
      connection-init-sql: set names utf8mb4;  # 连接初始化SQL
  
  servlet:
    multipart:
      enabled: true
      max-file-size: 5MB          # 单个文件最大5MB
      max-request-size: 30MB      # 总请求最大30MB
  
  mvc:
    static-path-pattern: /static/**
    view:
      prefix: /templates/
  
  # FreeMarker配置
  freemarker:
    allow-request-override: false
    cache: true
    check-template-location: true
    charset: UTF-8
    content-type: text/html
    expose-request-attributes: false
    expose-session-attributes: false
    expose-spring-macro-helpers: false
    template-loader-path=classpath: /templates
    request-context-attribute: request
    suffix: .ftl                  # 模板文件后缀
    settings:
      locale: zh_CN
      default_encoding: UTF-8
      number_format: 0.##########
      datetime_format: yyyy-MM-dd HH:mm:ss
      classic_compatible: true
      template_exception_handler: ignore

mybatis:
  mapper-locations: classpath*:mapper/*.xml
```

### 5.2 多环境配置

配置文件按环境分目录存放：
- `props/local/front.properties`: 本地开发环境
- `props/dev/front.properties`: 开发环境
- `props/test/front.properties`: 测试环境
- `props/ocn/front.properties`: OCN环境
- `props/prod/front.properties`: 生产环境

### 5.3 业务配置（front.properties示例）

```properties
# 积分商城配置
pointStore.url=https://club.10010.com/member/authlogin
pointStoreIndex.url=https://club.10010.com/index.html
pointStore.key=3a2778c2
pointStore.channel=beijingltwechat
pointStore.secret=b865468ab174f303b058ff61

# 渠道标识
CANNEL_KEY=wxbjcc12

# 域名配置
baseDomain=http://weixin.6smarthome.com/
miscDomain=http://weixin.6smarthome.com/

# 支付成功/失败跳转URL
paySuccessUrl=http://127.0.0.1:9095/front/toPhoneRechargeCompleteSuccess
payFailUrl=http://127.0.0.1:9095/front/toPhoneRechargeCompleteError

# 权益中心配置
rightsCenter.url=http://qy.chinaunicom.cn/mobile/api/getTokenWithPhone
rightsCenter.rightSendUrl=http://qy.chinaunicom.cn/mobile-h5/main/userarea.html
rightsCenter.channel=285933
rightsCenter.key=f3e8e7f95f694275b94eb5a5927671ac

# 知识库中心配置
kbCenter.url=http://dloc.bjunicom.com.cn/queryCenter/web-phone/web-out-order/page/order-search-validate.html
kbCenter.invoke_code=1qaz@WSX

# 沃钱包配置
sslPath=http://
publicKey=12

# 教育服务配置
edu.url=http://casadmintest.edu.sh.cn/casadmin
edu.clientId=922db71c2e4a460e874e86b851c64c58
edu.key=d4e3697b
edu.vod.key=ocntest123456

# 断链配置
cutlink.seconds=180       # 断链时长（秒）
cutlink.counter=20        # 断链计数器

# 其他域名配置
fastDomain=http://ocnweixin.96877.net/ocnwx/
downloadDomain=http://127.0.0.1:9095/front/invoice/download?url=

# 营业网点FTP配置
outlets.path=C:/Users/13974/Desktop/invoivePdf/
outlets.server=192.168.82.189
outlets.port=22
outlets.username=ftpuser0
outlets.password=ftp@189
```

### 5.4 日志配置（logback.xml）

- **日志级别**: INFO（生产环境）/ DEBUG（开发环境）
- **日志路径**: ./logs/front/
- **滚动策略**: 按天滚动，保留30天
- **日志格式**: `%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{50} - %msg%n`

## 六、构建与打包

### 6.1 Gradle配置要点

```gradle
// 插件配置
apply plugin: 'java'
apply plugin: 'idea'
apply plugin: 'eclipse'
apply plugin: 'maven'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
apply plugin: "com.arenagod.gradle.MybatisGenerator"  // MyBatis代码生成器插件

// 依赖配置
dependencies {
    compile project(':tymk-password')              // 依赖密码模块
    compile project(':tymk-common')                // 依赖公共模块
    
    compile('org.springframework.boot:spring-boot-starter-freemarker')  // FreeMarker
    compile('org.springframework.boot:spring-boot-starter-web')
    
    compile('io.springfox:springfox-swagger-ui:2.6.1')
    compile('io.springfox:springfox-swagger2:2.6.1')
    
    compile group: 'com.alibaba', name: 'fastjson', version: '1.2.54'
    compile group: 'org.mybatis.generator', name: 'mybatis-generator-core', version:'1.3.2'
    compile group: 'commons-net', name: 'commons-net', version: '3.6'
    
    compileOnly 'org.projectlombok:lombok:1.18.6'
    annotationProcessor 'org.projectlombok:lombok:1.18.6'
    
    compile 'org.slf4j:slf4j-api:1.7.28'
    compile 'ch.qos.logback:logback-core:1.1.3'
    compile 'ch.qos.logback:logback-classic:1.1.3'
}

// 打包配置
jar {
    baseName = 'tymk-front-' + releaseTime()  // 添加时间戳
    manifest {
        attributes(
            'Class-Path': configurations.compile.collect { it.getName() }.join(' '),
            'Main-Class': 'com.tymk.front.TymkFrontApplication'
        )
    }
}

// 时间戳函数（Asia/Shanghai时区）
def releaseTime() {
    return new Date().format("yyyyMMddHHmmss", TimeZone.getTimeZone("Asia/Shanghai"))
}

// MyBatis Generator配置
mybatisGenerator {
    verbose = true
    configFile = 'src/main/resources/generatorConfig.xml'
}
```

### 6.2 打包命令

```bash
# 构建项目
gradle build

# 跳过测试构建
gradle build -x test

# 生成MyBatis代码
gradle mybatisGenerate

# 生成的JAR包名称格式
tymk-front-20260310172530.jar  # 包含时间戳
```

## 七、关键技术特点

### 7.1 FreeMarker模板引擎
- 服务端渲染H5页面，SEO友好
- 支持模板继承、宏定义、循环、条件判断等
- 配置中文locale和日期格式化
- 模板异常处理设置为ignore，提升用户体验

### 7.2 多环境配置
- 支持5种环境：local、dev、test、ocn、prod
- 通过`spring.profiles.active`切换环境
- 配置文件分目录管理，便于维护
- 生产环境自动禁用Swagger

### 7.3 全局Header参数
- Swagger UI支持全局Header参数（Authorization、zx）
- 方便API测试时统一添加认证token和公众号标识

### 7.4 文件上传支持
- 单个文件最大5MB
- 总请求最大30MB
- 支持Base64图片上传
- 支持FTP文件传输（营业网点文件）

### 7.5 数据库连接优化
- 使用HikariCP连接池（高性能）
- 连接初始化设置UTF8MB4字符集
- 支持Emoji等4字节UTF-8字符

### 7.6 MyBatis代码生成
- 集成MyBatis Generator插件
- 一键生成Mapper、Model、XML
- 提升开发效率

### 7.7 异步任务支持
- 使用`@EnableAsync`启用异步处理
- 适用于耗时操作（如发送短信、调用第三方接口等）

### 7.8 定时任务调度
- 使用Spring `@Scheduled`注解
- 营业厅数据定时同步

### 7.9 数据脱敏
- 用户姓名脱敏（保留首尾字符）
- 手机号脱敏（中间4位显示*）
- 地址脱敏（部分隐藏）
- 保护用户隐私

## 八、部署与运行

### 8.1 环境要求
- JDK 1.8+
- MySQL数据库
- Redis服务
- 可访问BOSS系统、教育平台等外部系统

### 8.2 启动命令

```bash
# 开发环境启动
java -jar tymk-front-20260310172530.jar --spring.profiles.active=local

# 生产环境启动
java -jar tymk-front-20260310172530.jar --spring.profiles.active=prod

# 指定JVM参数
java -Xms512m -Xmx2048m -jar tymk-front-20260310172530.jar
```

### 8.3 健康检查
- 端口检查：`http://localhost:9095/front`
- Swagger UI：`http://localhost:9095/front/swagger-ui.html`（非生产环境）

## 九、业务功能模块

### 9.1 用户管理
- 手机号绑定/解绑
- 客户地址绑定管理
- 默认地址设置
- 用户信息查询
- 临时授权管理

### 9.2 账单与支付
- 实时费用查询
- 历史账单查询
- 账单详情查看
- 电子发票申请
- 发票下载
- 微信支付集成
- 余额充值
- 充值记录查询

### 9.3 套餐管理
- 套餐列表查询（按类型、会员等级）
- 套餐详情查看
- 套餐办理
- 套餐变更
- 已订购套餐查询
- 工单查询
- 优惠券使用
- 流量包订购

### 9.4 5G业务
- 5G号码查询
- 号码预占/释放
- 5G套餐办理
- 套餐变更
- 密码管理（校验、重置、修改）
- 话费账单查询
- 充值订购
- 发票查询
- 违约金查询
- 营业网点预约

### 9.5 教育服务（空中课堂）
- 学生/教师注册
- 身份认证
- 签到打卡
- 签到记录查询
- 观看时长统计
- MAC地址绑定
- 冠军榜查询

### 9.6 会员服务
- 会员等级查询
- 会员权益展示
- 权益码领取
- 积分商城跳转
- 会员促销活动

### 9.7 自助服务
- 故障报修
- 自检视频查看
- 营业网点查询
- 营业网点预约
- 客服系统对接
- 区域查询

### 9.8 其他服务
- 签到功能
- 广告展示
- 快捷入口
- 系统设置
- 短信验证码
- 图片验证码

## 十、模块特色与价值

### 10.1 核心价值
1. **前端业务枢纽**: 作为微信公众号/小程序的H5前端服务，直接面向C端用户
2. **业务整合**: 整合账单、余额、套餐、5G、教育等多种业务功能
3. **用户体验优化**: FreeMarker服务端渲染，页面加载快，SEO友好
4. **多端适配**: 支持微信公众号、小程序等多种前端场景

### 10.2 技术亮点
1. **模板引擎**: 使用FreeMarker实现灵活的页面渲染
2. **RESTful API**: 标准的RESTful接口设计，便于前后端分离
3. **多环境配置**: 灵活的配置管理，支持快速环境切换
4. **代码生成**: 集成MyBatis Generator，提升开发效率
5. **数据脱敏**: 完善的隐私保护机制
6. **异步处理**: 提升系统性能和用户体验
7. **定时任务**: 自动同步营业网点等基础数据
8. **完善的日志**: Logback日志框架，便于问题排查

### 10.3 业务覆盖广度
- **25个控制器**: 覆盖用户、账单、支付、套餐、5G、教育等各类业务
- **27个服务类**: 完善的服务层设计，业务逻辑清晰
- **100+个业务方法**: 满足各类业务场景需求
- **多套FreeMarker模板**: 按业务分类的页面模板，便于维护

## 十一、与其他模块的关系

### 11.1 依赖关系
- **tymk-password**: 提供用户认证拦截器、密码加密等功能
- **tymk-common**: 提供公共工具类、枚举定义、帮助类等

### 11.2 调用关系
- 调用**tymk-third**模块的OCN接口对接BOSS系统
- 调用**tymk-wx**模块的微信接口（如有）
- 调用外部系统：教育平台、积分商城、权益中心、客服系统等

### 11.3 数据交互
- 与MySQL数据库交互（用户数据、订单数据、配置数据等）
- 与Redis交互（缓存用户信息、账单信息等）
- 与外部系统HTTP接口交互（BOSS、教育平台等）

## 十二、总结

`tymk-front`模块是微厅业务系统的前端核心服务，采用Spring Boot + FreeMarker + MyBatis技术栈，提供了完善的H5页面展示、用户交互、业务办理等功能。模块设计合理，业务覆盖全面，包含用户管理、账单支付、套餐办理、5G业务、教育服务等多种业务场景。通过FreeMarker模板引擎实现灵活的页面渲染，通过多环境配置支持快速部署，是整个微厅系统面向C端用户的重要门户。





# tymk-front 和 tymk-third 的依赖关系分析

**结论：无直接依赖关系，通过 HTTP 接口间接调用**

### 详细说明

#### 1. Gradle 构建依赖分析

__tymk-front 的依赖__（build.gradle）：

```gradle
dependencies {
    compile project(':tymk-password')  // 密码模块
    compile project(':tymk-common')    // 公共模块
    // 没有依赖 tymk-third
}
```

__tymk-third 的依赖__（build.gradle）：

```gradle
dependencies {
    compile project(':tymk-password')  // 密码模块
    // 没有依赖 tymk-common 和 tymk-front
}
```

__结论__：从 Gradle 构建配置看，两个模块之间__没有直接的 Maven/Gradle 依赖关系__。

#### 2. 架构层面的关系

__模块定位对比__：

```table
| 维度 | tymk-front | tymk-third | |------|-----------|-----------| | 定位 | 前端业务服务层 | 第三方系统对接层 | | 端口 | 9095 | 9093 | | 上下文路径 | /front | /third | | 主要职责 | H5页面展示、用户交互、业务办理 | 对接BOSS、河南AOP等外部系统 | | 面向对象 | C端用户（微信公众号/小程序） | 后端系统集成 |
```



__架构关系图__：

```javascript
┌─────────────────┐
│  微信公众号/小程序  │
└────────┬────────┘
         │ HTTP请求
         ↓
┌─────────────────┐
│   tymk-front    │  (端口9095, 前端业务服务)
│  - 用户管理      │
│  - 账单支付      │
│  - 套餐办理      │
│  - 5G业务       │
│  - 教育服务      │
└────────┬────────┘
         │ HTTP调用
         ↓
┌─────────────────┐
│   tymk-third    │  (端口9093, 第三方对接)
│  - OCNService   │
│  - OCNController│
│  - BOSS对接     │
│  - 河南AOP对接   │
└────────┬────────┘
         │ WebService/HTTP
         ↓
┌─────────────────┐
│   外部系统       │
│  - BOSS系统     │
│  - 河南AOP平台  │
│  - 客服系统     │
└─────────────────┘
```

#### 3. 调用关系分析

__tymk-front 如何调用 tymk-third__：

tymk-front 通过 __HTTP 接口调用__ tymk-third 的服务，主要体现在：

1. __BaseService.postAppJson() 方法__：

```java
// tymk-front/service/BaseService.java
public class BaseService {
    public GlobalResponse postAppJson(String url, String json, String header) {
        // 发送HTTP POST请求到第三方服务
        // url 可能指向 tymk-third 的接口
    }
}
```

2. __调用场景示例__：

```java
// tymk-front 的服务层调用 tymk-third 的 OCN 接口
String thirdUrl = "http://localhost:9093/third/ocn/queryBillSummary";
GlobalResponse response = postAppJson(thirdUrl, jsonData, header);
```

3. __配置文件中的域名配置__：

```properties
# tymk-front/props/local/front.properties
baseDomain=http://weixin.6smarthome.com/
miscDomain=http://weixin.6smarthome.com/

# 这些域名可能指向 tymk-third 服务
```

#### 4. 为什么设计成无直接依赖？

__优势__：

1. __服务解耦__：

   - 两个模块可以独立部署、独立升级
   - 一个模块的故障不会直接影响另一个模块的编译和启动

2. __技术栈独立__：

   - tymk-front 使用 FreeMarker 模板引擎
   - tymk-third 使用 Apache CXF WebService框架
   - 各自选择最适合自己的技术栈

3. __水平扩展__：

   - 可以根据负载分别扩展 tymk-front 和 tymk-third 实例
   - 例如：前端访问量大时，多部署几个 tymk-front 实例

4. __职责清晰__：

   - tymk-front：专注于前端业务逻辑和用户体验
   - tymk-third：专注于第三方系统集成和接口适配

__劣势__：

1. __网络开销__：HTTP 调用比进程内调用慢
2. __调试复杂__：需要同时启动两个服务才能完整测试
3. __接口契约__：需要维护接口文档，确保接口兼容性

#### 5. 共同依赖的模块

两个模块虽然不直接依赖，但都依赖以下公共模块：

__tymk-password__：

- 提供密码加密/解密功能
- 提供用户认证拦截器（TymkInterceptor）
- 两个模块都需要用户认证功能

__tymk-common__（tymk-front 依赖）：

- 提供公共工具类（AssertHelper等）
- 提供枚举定义（Env等）
- 提供帮助类

#### 6. 实际部署场景

__开发环境__：

```bash
# 启动 tymk-third（端口9093）
java -jar tymk-third-xxx.jar --spring.profiles.active=local

# 启动 tymk-front（端口9095）
java -jar tymk-front-xxx.jar --spring.profiles.active=local

# tymk-front 通过 http://localhost:9093 调用 tymk-third
```

__生产环境__：

```bash
# 可能通过Nginx做反向代理
# 前端服务：http://weixin.example.com/front
# 第三方服务：http://weixin.example.com/third（内网访问）
```

### 总结

tymk-front 和 tymk-third 是__松耦合的微服务架构__：

- __无直接依赖__：通过 HTTP 接口通信，而非 Maven/Gradle 依赖
- __各司其职__：前端业务层与第三方对接层职责分离
- __独立部署__：可以分别部署、扩展和升级
- __共享基础__：共同依赖 tymk-password 等基础模块

这种设计符合__微服务架构__的最佳实践，提供了良好的可维护性和可扩展性。







# tymk-front 调用 tymk-third 接口依赖关系分析

## 1. 概述

**tymk-front** 模块通过HTTP方式调用 **tymk-third** 模块提供的RESTful接口，实现业务功能的解耦与分层。

### 1.1 服务配置

- **tymk-third服务地址**：`http://127.0.0.1:9093/third/`（本地环境）
- **配置位置**：`tymk-password/src/main/resources/props/local/domain.properties`
- **配置项**：`thirdDomain=http://127.0.0.1:9093/third/`
- **调用方式**：通过`@Value("${thirdDomain}")`注入到各Service类中

### 1.2 调用统计

通过代码分析，共发现 **106处** tymk-front调用tymk-third的接口调用点，涉及 **16个Service类** 和 **60+个接口**。

---

## 2. 接口依赖详细清单

### 2.1 用户信息服务（UserService）

**调用的tymk-third接口**：

| 接口路径                      | 功能说明         | 调用方法                   |
| ----------------------------- | ---------------- | -------------------------- |
| `info/getFeeInfo`             | 查询费用信息     | `getFeeInfo()`             |
| `info/getLeftpackage`         | 查询剩余套餐     | `getLeftPackage()`         |
| `info/getChargeLog`           | 查询充值记录     | `getChargeLog()`           |
| `info/findHistoryBillPackage` | 查询历史账单套餐 | `findHistoryBillPackage()` |
| `flow/searchFlow`             | 查询流量使用情况 | `searchFlow()`             |
| `info/findFee`                | 查询费用明细     | `findFee()`                |
| `info/getResidual`            | 查询余额         | `getResidual()`            |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 示例：查询费用信息
GlobalResponse pass = HttpHelper.postAppJson(
    thirdDomain + "info/getFeeInfo", 
    JsonHelper.toJson(param), 
    "pass"
);
```

---

### 2.2 绑定服务（BindService）

**调用的tymk-third接口**：

| 接口路径                   | 功能说明         | 调用方法                          |
| -------------------------- | ---------------- | --------------------------------- |
| `ocn/sendSmsCode`          | 发送短信验证码   | `sendSmsCode()`                   |
| `ocn/queryEBillStatus`     | 查询电子账单状态 | `queryEBillStatus()`              |
| `ocn/orderEBill`           | 订购电子账单     | `orderEBill()`                    |
| `ocn/queryUserCardList`    | 查询用户卡列表   | `queryUserCardList()`（多处调用） |
| `ocn/findBaseCustAddrList` | 查询客户地址列表 | `findBaseCustAddrList()`          |
| `ocn/findBaseCustInfo`     | 查询客户基本信息 | `findBaseCustInfo()`              |
| `ocn/refuelingBag`         | 加油包业务       | `refuelingBag()`                  |
| `ocn/queryResource`        | 查询资源         | `queryResource()`                 |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 示例：发送短信验证码
GlobalResponse pass = postAppJson(
    thirdDomain + "ocn/sendSmsCode",
    JsonHelper.toJson(smsCodeParam),
    "pass"
);

// 示例：查询用户卡列表（多处使用）
GlobalResponse pass = postAppJson(
    thirdDomain + "ocn/queryUserCardList", 
    JsonHelper.toJson(param), 
    "pass"
);
```

---

### 2.3 套餐服务（OfferService）

**调用的tymk-third接口**：

| 接口路径                     | 功能说明             | 调用方法                               |
| ---------------------------- | -------------------- | -------------------------------------- |
| `ocn/subscribeInstall`       | 预约装机             | `subscribeInstall()`                   |
| `ocn/orderOfferChange`       | 套餐变更订单         | `orderOfferChange()`                   |
| `offer/saveSubscribe`        | 保存预约信息         | `saveSubscribe()`（多处调用）          |
| `ocn/rerun`                  | 重跑订单             | `rerun()`                              |
| `ocn/queryOrderedOfferInfo`  | 查询已订购套餐信息   | `queryOrderedOfferInfo()`              |
| `ocn/queryWorkOrder`         | 查询工单             | `queryWorkOrder()`                     |
| `ocn/offerOrderCheck`        | 套餐订购检查         | `offerOrderCheck()`                    |
| `ocn/offerOrderCouponAndFee` | 查询套餐优惠券和费用 | `offerOrderCouponAndFee()`（多处调用） |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 示例：预约装机
GlobalResponse response = postAppJson(
    thirdDomain + "ocn/subscribeInstall", 
    JsonHelper.toJson(param), 
    "pass"
);

// 示例：套餐变更订单
GlobalResponse response = postAppJson(
    thirdDomain + "ocn/orderOfferChange", 
    JsonHelper.toJson(offerChangeParam), 
    "pass"
);
```

---

### 2.4 5G服务（Wx5GService）

**调用的tymk-third接口**：

| 接口路径                     | 功能说明                | 调用方法               |
| ---------------------------- | ----------------------- | ---------------------- |
| `ocn/*`                      | OCN相关接口（通用调用） | `postAppJsonHelper()`  |
| `payment/createOrder`        | 创建支付订单            | `createOrder()`        |
| `payment/payOrder`           | 支付订单                | `payOrder()`           |
| `payment/doPayment`          | 执行支付                | `doPayment()`          |
| `payment/cancelPaymentOrder` | 取消支付订单            | `cancelPaymentOrder()` |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 通用OCN接口调用
public GlobalResponse postAppJsonHelper(String url, String json) {
    return postAppJson(thirdDomain + "ocn/" + url, json, "pass");
}

// 创建支付订单
return postAppJson(
    thirdDomain + "payment/createOrder", 
    JsonHelper.toJson(param), 
    "pass"
);
```

---

### 2.5 账单服务（BillService）

**调用的tymk-third接口**：

| 接口路径                | 功能说明       | 调用方法              |
| ----------------------- | -------------- | --------------------- |
| `ocn/queryOCNNoPayBill` | 查询未支付账单 | `queryOCNNoPayBill()` |
| `ocn/queryOCNBill`      | 查询账单       | `queryOCNBill()`      |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 查询未支付账单
GlobalResponse passNopay = postAppJson(
    thirdDomain + "ocn/queryOCNNoPayBill",
    JsonHelper.toJson(nopay_bill_param),
    "pass"
);
```

---

### 2.6 发票服务（InvoiceService）

**调用的tymk-third接口**：

| 接口路径                     | 功能说明         | 调用方法                   |
| ---------------------------- | ---------------- | -------------------------- |
| `ocn/queryEInvoice`          | 查询电子发票     | `queryEInvoice()`          |
| `ocn/queryCreateEInvoice`    | 查询创建电子发票 | `queryCreateEInvoice()`    |
| `ocn/generateCreateEInvoice` | 生成创建电子发票 | `generateCreateEInvoice()` |
| `ocn/gwZtInvoice`            | 国网智通发票     | `gwZtInvoice()`            |
| `ocn/gwZtPaymentNotice`      | 国网智通支付通知 | `gwZtPaymentNotice()`      |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 查询电子发票
GlobalResponse response = postAppJson(
    thirdDomain + "/ocn/queryEInvoice",
    JsonHelper.toJson(eInvoiceParam),
    "pass"
);
```

---

### 2.7 短信服务（SmsService）

**调用的tymk-third接口**：

| 接口路径      | 功能说明 | 调用方法    |
| ------------- | -------- | ----------- |
| `sms/sendSms` | 发送短信 | `sendSms()` |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

GlobalResponse pass = HttpHelper.postAppJson(
    thirdDomain + "sms/sendSms", 
    JsonHelper.toJson(param), 
    "pass"
);
```

---

### 2.8 充值订单服务（RechargeOrderService）

**调用的tymk-third接口**：

| 接口路径                     | 功能说明          | 调用方法                   |
| ---------------------------- | ----------------- | -------------------------- |
| `ocn/getSerialNumber`        | 获取流水号        | `querySerialNumber()`      |
| `ocn/queryJSAPIUnifiedOrder` | 查询JSAPI统一下单 | `queryJSAPIUnifiedOrder()` |
| `payment/createNewOrder`     | 创建新订单        | `crateOrderInfo()`         |
| `payment/payNewOrder`        | 支付新订单        | `payNewOrder()`            |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 查询JSAPI统一下单
GlobalResponse resp = postAppJson(
    thirdDomain + "ocn/queryJSAPIUnifiedOrder", 
    JsonHelper.toJson(request), 
    "pass"
);

// 创建新订单
GlobalResponse resp = postAppJson(
    thirdDomain + "payment/createNewOrder", 
    JsonHelper.toJson(request), 
    "pass"
);
```

---

### 2.9 余额服务（BalanceService）

**调用的tymk-third接口**：

| 接口路径           | 功能说明 | 调用方法         |
| ------------------ | -------- | ---------------- |
| `ocn/queryBalance` | 查询余额 | `queryBalance()` |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

GlobalResponse pass = postAppJson(
    thirdDomain + "ocn/queryBalance",
    JsonHelper.toJson(param),
    "pass"
);
```

---

### 2.10 流量订单服务（FlowOrderService）

**调用的tymk-third接口**：

| 接口路径                | 功能说明     | 调用方法        |
| ----------------------- | ------------ | --------------- |
| `flowOrder/order`       | 流量订购     | `order()`       |
| `flowOrder/orderCancel` | 取消流量订单 | `orderCancel()` |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

GlobalResponse pass = HttpHelper.postAppJson(
    thirdDomain + "flowOrder/order", 
    JsonHelper.toJson(paramMaps),
    "pass"
);
```

---

### 2.11 套餐包服务（FeePackageService）

**调用的tymk-third接口**：

| 接口路径               | 功能说明     | 调用方法            |
| ---------------------- | ------------ | ------------------- |
| `info/findFeePackage`  | 查询费用套餐 | `findFeePackage()`  |
| `info/findLeftPackage` | 查询剩余套餐 | `findLeftPackage()` |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

GlobalResponse json = HttpHelper.postAppJson(
    thirdDomain + "info/findFeePackage", 
    JsonHelper.toJson(map), 
    "pass"
);
```

---

### 2.12 历史账单套餐服务（HistoryBillPackageService）

**调用的tymk-third接口**：

| 接口路径                      | 功能说明         | 调用方法                   |
| ----------------------------- | ---------------- | -------------------------- |
| `info/findHistoryBillPackage` | 查询历史账单套餐 | `findHistoryBillPackage()` |

---

### 2.13 密码重置与校验服务（ResetOrCheckPasswordService）

**调用的tymk-third接口**：

| 接口路径             | 功能说明 | 调用方法          |
| -------------------- | -------- | ----------------- |
| `info/resetPassword` | 重置密码 | `resetPassword()` |
| `info/checkPassword` | 校验密码 | `checkPassword()` |

---

### 2.14 通用功能服务（CommonFuncService）

**调用的tymk-third接口**：

| 接口路径                  | 功能说明         | 调用方法                    |
| ------------------------- | ---------------- | --------------------------- |
| `ocn/refreshAuth`         | 刷新授权         | `refreshAuth()`             |
| `ocn/queryMembership`     | 查询会员信息     | `queryMembership()`         |
| `ocn/queryMembershipSale` | 查询会员销售信息 | `queryMembershipSale()`     |
| `kefu/sendMessage`        | 发送客服消息     | `sendMessage()`（多处调用） |

**关键代码片段**：

```java
@Value("${thirdDomain}")
private String thirdDomain;

// 刷新授权
GlobalResponse response = postAppJson(
    thirdDomain + "ocn/refreshAuth",
    JsonHelper.toJson(param),
    "pass"
);

// 发送客服消息
GlobalResponse response = HttpHelper.postAppJson(
    thirdDomain + "kefu/sendMessage",
    JsonHelper.toJson(params),
    "pass"
);
```

---

### 2.15 电子码服务（EcodeService）

**调用的tymk-third接口**：

| 接口路径                    | 功能说明         | 调用方法                  |
| --------------------------- | ---------------- | ------------------------- |
| `ocn/queryMemberOfferLevel` | 查询会员套餐等级 | `queryMemberOfferLevel()` |

---

### 2.16 支付控制器（PayController）

**说明**：虽然注入了`thirdDomain`，但在提供的代码片段中未发现实际调用。

---

## 3. 接口调用模式分析

### 3.1 调用方式

所有Service类通过以下统一模式调用tymk-third接口：

```java
// 1. 注入thirdDomain配置
@Value("${thirdDomain}")
private String thirdDomain;

// 2. 使用HttpHelper或继承自BaseService的postAppJson方法
GlobalResponse response = postAppJson(
    thirdDomain + "接口路径",
    JsonHelper.toJson(参数对象),
    "pass"  // 密码/令牌参数
);

// 3. 处理响应
if (response.isSuccess()) {
    // 业务逻辑处理
}
```

### 3.2 BaseService基类

tymk-front的所有Service类继承自`BaseService`，该基类提供了统一的HTTP调用方法：

```java
public abstract class BaseService {
    protected GlobalResponse postAppJson(String url, String json, String pass) {
        // 统一的HTTP POST调用逻辑
        return HttpHelper.postAppJson(url, json, pass);
    }
}
```

### 3.3 响应处理

所有接口调用返回统一的`GlobalResponse`对象：

```java
public class GlobalResponse {
    private boolean success;      // 是否成功
    private Object data;          // 返回数据
    private int statusCode;       // HTTP状态码
    private String alertMsg;      // 提示消息
    
    public boolean isSuccess() {
        return success;
    }
}
```

---

## 4. 接口分类统计

### 4.1 按业务领域分类

| 业务领域 | 接口数量 | 主要接口前缀            |
| -------- | -------- | ----------------------- |
| OCN业务  | 25+      | `ocn/*`                 |
| 支付业务 | 8        | `payment/*`             |
| 用户信息 | 10       | `info/*`                |
| 流量业务 | 3        | `flow/*`, `flowOrder/*` |
| 短信服务 | 2        | `sms/*`                 |
| 客服服务 | 2        | `kefu/*`                |
| 套餐业务 | 3        | `offer/*`               |

### 4.2 按调用频率分类

| 调用频率 | Service类            | 接口数量  |
| -------- | -------------------- | --------- |
| 高频     | BindService          | 8个接口   |
| 高频     | OfferService         | 8个接口   |
| 高频     | UserService          | 7个接口   |
| 高频     | Wx5GService          | 5个接口   |
| 中频     | InvoiceService       | 5个接口   |
| 中频     | CommonFuncService    | 4个接口   |
| 中频     | RechargeOrderService | 4个接口   |
| 低频     | 其他Service          | 1-2个接口 |

---

## 5. 技术要点

### 5.1 配置管理

- **配置位置**：`tymk-password`模块（共享配置模块）
- **配置文件**：`props/{env}/domain.properties`
- **支持环境**：local、dev、test、ocn、prod
- **本地配置**：`thirdDomain=http://127.0.0.1:9093/third/`

### 5.2 HTTP客户端

- **工具类**：`HttpHelper`
- **序列化**：Fastjson（`JsonHelper.toJson()`）
- **认证参数**：统一使用"pass"作为第三个参数

### 5.3 异常处理

```java
GlobalResponse response = postAppJson(thirdDomain + "接口路径", json, "pass");
if (response.isSuccess()) {
    // 成功处理
} else {
    // 失败处理，记录日志或返回错误信息
    log.error("调用third接口失败: {}", response.getAlertMsg());
}
```

---

## 6. 依赖关系图

```
┌─────────────────────────────────────────────────────────┐
│                    tymk-front (9095)                     │
│  ┌──────────────────────────────────────────────────┐  │
│  │           16个Service类                           │  │
│  │  - UserService                                    │  │
│  │  - BindService                                    │  │
│  │  - OfferService                                   │  │
│  │  - Wx5GService                                    │  │
│  │  - BillService                                    │  │
│  │  - InvoiceService                                 │  │
│  │  - SmsService                                     │  │
│  │  - RechargeOrderService                           │  │
│  │  - BalanceService                                 │  │
│  │  - FlowOrderService                               │  │
│  │  - FeePackageService                              │  │
│  │  - HistoryBillPackageService                      │  │
│  │  - ResetOrCheckPasswordService                    │  │
│  │  - CommonFuncService                              │  │
│  │  - EcodeService                                   │  │
│  │  - PayController                                  │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                        │
                        │ HTTP调用
                        │ thirdDomain=${thirdDomain}
                        ▼
┌─────────────────────────────────────────────────────────┐
│                    tymk-third (9093)                     │
│  ┌──────────────────────────────────────────────────┐  │
│  │           60+ RESTful接口                         │  │
│  │  ┌────────────────────────────────────────────┐  │  │
│  │  │  OCN业务接口 (ocn/*)                        │  │  │
│  │  │  - 用户管理、账单查询、套餐办理              │  │  │
│  │  │  - 工单查询、会员信息                        │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────────┐  │  │
│  │  │  支付业务接口 (payment/*)                   │  │  │
│  │  │  - 订单创建、支付、取消                      │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────────┐  │  │
│  │  │  信息查询接口 (info/*)                      │  │  │
│  │  │  - 费用、余额、套餐、密码                    │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  │  ┌────────────────────────────────────────────┐  │  │
│  │  │  其他接口 (flow/*, sms/*, kefu/*, offer/*) │  │  │
│  │  └────────────────────────────────────────────┘  │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
                        │
                        │ WebService/HTTP
                        ▼
┌─────────────────────────────────────────────────────────┐
│              外部系统（BOSS、AOP、客服等）                 │
└─────────────────────────────────────────────────────────┘
```

---

## 7. 接口完整清单

### OCN业务接口（ocn/*）

1. `ocn/sendSmsCode` - 发送短信验证码
2. `ocn/queryEBillStatus` - 查询电子账单状态
3. `ocn/orderEBill` - 订购电子账单
4. `ocn/queryUserCardList` - 查询用户卡列表
5. `ocn/findBaseCustAddrList` - 查询客户地址列表
6. `ocn/findBaseCustInfo` - 查询客户基本信息
7. `ocn/refuelingBag` - 加油包业务
8. `ocn/queryResource` - 查询资源
9. `ocn/subscribeInstall` - 预约装机
10. `ocn/orderOfferChange` - 套餐变更订单
11. `ocn/rerun` - 重跑订单
12. `ocn/queryOrderedOfferInfo` - 查询已订购套餐信息
13. `ocn/queryWorkOrder` - 查询工单
14. `ocn/offerOrderCheck` - 套餐订购检查
15. `ocn/offerOrderCouponAndFee` - 查询套餐优惠券和费用
16. `ocn/queryBalance` - 查询余额
17. `ocn/queryOCNNoPayBill` - 查询未支付账单
18. `ocn/queryOCNBill` - 查询账单
19. `ocn/queryEInvoice` - 查询电子发票
20. `ocn/queryCreateEInvoice` - 查询创建电子发票
21. `ocn/generateCreateEInvoice` - 生成创建电子发票
22. `ocn/gwZtInvoice` - 国网智通发票
23. `ocn/gwZtPaymentNotice` - 国网智通支付通知
24. `ocn/getSerialNumber` - 获取流水号
25. `ocn/queryJSAPIUnifiedOrder` - 查询JSAPI统一下单
26. `ocn/refreshAuth` - 刷新授权
27. `ocn/queryMembership` - 查询会员信息
28. `ocn/queryMembershipSale` - 查询会员销售信息
29. `ocn/queryMemberOfferLevel` - 查询会员套餐等级

### 支付业务接口（payment/*）

30. `payment/createOrder` - 创建支付订单
31. `payment/payOrder` - 支付订单
32. `payment/doPayment` - 执行支付
33. `payment/cancelPaymentOrder` - 取消支付订单
34. `payment/createNewOrder` - 创建新订单
35. `payment/payNewOrder` - 支付新订单

### 用户信息接口（info/*）

36. `info/getFeeInfo` - 查询费用信息
37. `info/getLeftpackage` - 查询剩余套餐
38. `info/getChargeLog` - 查询充值记录
39. `info/findHistoryBillPackage` - 查询历史账单套餐
40. `info/findFee` - 查询费用明细
41. `info/getResidual` - 查询余额
42. `info/findFeePackage` - 查询费用套餐
43. `info/findLeftPackage` - 查询剩余套餐
44. `info/resetPassword` - 重置密码
45. `info/checkPassword` - 校验密码

### 流量业务接口（flow/*, flowOrder/*）

46. `flow/searchFlow` - 查询流量使用情况
47. `flowOrder/order` - 流量订购
48. `flowOrder/orderCancel` - 取消流量订单

### 短信服务接口（sms/*）

49. `sms/sendSms` - 发送短信

### 客服服务接口（kefu/*）

50. `kefu/sendMessage` - 发送客服消息

### 套餐业务接口（offer/*）

51. `offer/saveSubscribe` - 保存预约信息

---

## 8. 注意事项

### 8.1 接口调用规范

1. **统一认证**：所有接口调用都需要传递"pass"参数
2. **JSON序列化**：使用Fastjson进行参数序列化
3. **响应检查**：必须检查`response.isSuccess()`
4. **日志记录**：关键接口调用需记录日志

### 8.2 异常处理建议

```java
try {
    GlobalResponse response = postAppJson(thirdDomain + "接口路径", json, "pass");
    if (response != null && response.isSuccess()) {
        // 成功处理
    } else {
        log.error("调用third接口失败: url={}, response={}", 
            thirdDomain + "接口路径", 
            JsonHelper.toJson(response));
        // 返回友好的错误信息
    }
} catch (Exception e) {
    log.error("调用third接口异常: url={}, error={}", 
        thirdDomain + "接口路径", 
        e.getMessage(), e);
    // 异常处理
}
```

### 8.3 性能优化建议

1. **连接池配置**：合理配置HTTP连接池大小
2. **超时设置**：设置合适的连接超时和读取超时时间
3. **异步调用**：对于耗时操作考虑使用异步调用
4. **缓存策略**：对频繁查询的数据实施缓存

---

## 9. 总结

### 9.1 依赖关系特点

1. **松耦合**：tymk-front与tymk-third通过HTTP接口通信，无直接代码依赖
2. **服务化**：遵循微服务架构，职责清晰分离
3. **统一规范**：所有Service类遵循统一的调用模式和响应处理
4. **配置集中**：通过tymk-password模块统一管理服务地址配置

### 9.2 接口覆盖范围

tymk-front调用tymk-third的接口覆盖了完整的业务场景：

- 用户管理（绑定、认证、信息查询）
- 账单管理（查询、支付）
- 套餐管理（订购、变更、查询）
- 支付管理（订单、支付、退款）
- 增值服务（5G、流量、发票、客服）

### 9.3 架构优势

1. **水平扩展**：两个模块可独立部署和扩展
2. **技术独立**：可使用不同的技术栈和框架版本
3. **故障隔离**：一个模块故障不直接影响另一个模块
4. **灵活部署**：可根据负载情况灵活调整部署策略

---

**文档生成时间**：2026-03-10  
**分析版本**：tymk-front v0.0.1-SNAPSHOT  
**依赖版本**：tymk-third v0.0.1-SNAPSHOT

