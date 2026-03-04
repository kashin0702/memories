### ‌**一、基础与配置管理**

- `openclaw --help`：查看所有可用命令
- `openclaw --version`：查看当前版本
- `openclaw setup` / `openclaw onboard`：初始化配置（推荐新手使用）
- `openclaw config get` / `set` / `unset`：查看、设置或删除配置项
- `openclaw configure`：交互式配置向导
- `openclaw dashboard`：打开 Web 控制面板（默认地址：[http://127.0.0.1:18789](http://127.0.0.1:18789/)）
- `openclaw tui`：启动终端交互界面

------

### ‌**二、网关（Gateway）服务控制**‌

网关是 OpenClaw 的核心服务，负责消息路由和智能体调度。

- `openclaw gateway start`：启动网关（默认端口 18789）
- `openclaw gateway start --port 19000`：自定义端口启动
- `openclaw gateway start --force`：强制启动（占用被占端口）
- `openclaw gateway stop` / `restart`：停止或重启网关
- `openclaw gateway status`：查看运行状态
- `openclaw logs` / `logs --follow`：实时查看日志
- `openclaw logs --filter error`：仅查看错误日志

> 生产环境建议使用 systemd 管理：
>
> ```
> bashCopy Codesudo systemctl start openclaw-gateway
> sudo systemctl enable openclaw-gateway  # 开机自启
> ```

------

### ‌**三、模型与认证管理**‌

- `openclaw models list`：列出可用模型
- `openclaw models set <model>`：设置默认模型（如 `mistral:mixtral-8x7b`）
- `openclaw models status`：查看模型状态与认证情况
- `openclaw models auth setup-token`：配置 Anthropic 等模型的 API Key

------

### ‌**四、消息与聊天通道**‌

支持 WhatsApp、Telegram、Discord、Slack、飞书等。

- `openclaw channels login`：登录 WhatsApp（生成 QR 码）
- `openclaw channels add --channel telegram --token <token>`：添加 Telegram 机器人
- `openclaw message send --target "+123" --message "Hi"`：直接发送消息
- `openclaw channels status`：查看通道连接状态

------

### ‌**五、诊断与维护**‌

- `openclaw doctor`：运行健康检查
- `openclaw doctor --fix`：自动修复常见问题
- `openclaw doctor --deep`：深度诊断（适合复杂故障）
- `openclaw reset`：重置本地配置（保留 CLI）
- `openclaw uninstall`：卸载服务和数据

------

### ‌**六、高级功能**‌

- ‌**定时任务（Cron）**‌：
  `openclaw cron list` / `add` / `run <job-id>`
- ‌**浏览器自动化**‌：
  `openclaw browser start` / `screenshot` / `open https://example.com`
- ‌**技能管理**‌：
  `clawhub search <skill>` / `install <skill>`（需先安装 ClawHub）
- ‌**多实例隔离**‌：
  使用 `--profile <name>` 启动独立实例（如 `openclaw --profile work dashboard`）







minimax密钥
openclaw1：sk-api-jAu-eyGE1GVcEOzwEcBiBd_sJt2Zd62vL_T__Vt2-dhQNf0yPXe6-3Cxfa6B1ykyBIHFUhePEOro5my50zKfh_SiJMG9zh8GUed3F0ez-H-pwslY2-sSrOw



月之暗面

sk-8c8st7iIZ165x1kJiMz4bv8JPR9xm86A8656279aEtNcj0C8



### 飞书配置openClaw流程

添加飞书： 
appid:  cli_a92d6d31e9235cb2
appSecret:  n52HIrQfkuZJV8c2xGgRicH3PKcyQX8i

1.创建应用
2.添加权限
3.配置事件和回调
4.应用发布
5.第一次用飞书bot聊天会提示：
OpenClaw: access not configured.
Your Feishu user id: ou_d133275df3e41a81c9c22d75adc968ce
Pairing code: 2GMLDP2V
Ask the bot owner to approve with:
openclaw pairing approve feishu 2GMLDP2V

根据提供的数字，在openclaw执行：openclaw pairing approve feishu 2GMLDP2V



### **openclaw命令：**

openclaw gateway start  开启网关服务
openclaw channels add 添加聊天工具

### 


```bash
#停止当前网关进程

openclaw gateway stop

#以 root 权限启动网关（解决文件操作/Shell 执行权限）

sudo openclaw gateway start --port 18789 --verbose

#验证权限：执行需要 root 的 Shell 命令（示例）

openclaw exec --cmd "echo 'test' > /etc/openclaw-test.txt && cat /etc/openclaw-test.txt"

```

