# Kuro-AutoSignin

自动化每日任务，轻松管理库街区论坛与游戏签到 

## 本仓库说明

- 此仓库fork自 [Kuro-autosignin](https://github.com/mxyooR/Kuro-autosignin)。本仓库主要用于为其添加、完善功能。
- 另外，本仓库还被作者用于熟悉git相关操作，可能会有各种奇奇怪怪的提交（
- 为便于阅读，readme文件经过删减，仅保留本地运行指引，云部署等内容请前往原项目查看。

## 本仓库开发进程

- 添加kook机器人通知推送功能（2026.2.9 已实现）
- 调试多用户签到

## 注意

仅供学习交流使用，请勿用于非法用途

## 获取 Token

为了更好地管理文件和依赖，获取 Token 功能已迁移至 [Kuro_login](https://github.com/mxyooR/Kuro_login)。请访问该项目以获取登录相关的详细说明和支持。

## 使用说明

1. **配置文件管理**  
   配置文件采用 YAML 格式存放于 `config` 目录中。每个用户对应一个 YAML 文件，文件名即用户的标识（不含扩展名）。  
   - 配置文件包括用户基本信息、游戏信息（如 token、devcode、distinct_id、wwroleId、eeeroleId）以及用户状态（enable）。
   - 如果配置中未完整填写，系统会自动调用填充流程补全缺失信息。

   示例配置文件 `name.yaml.example`：
   ```yaml
   # 控制本 config 文件是否启用
   enable: true
   # token 必填
   token: ""
   # 是否完整，不完整默认系统自动补全
   completed: false
   # 是否自动补签
   auto_reple_sign: true
   
   # 重试次数（失败后再次尝试的总次数，默认 3）
   retry_times: 3

   # 游戏信息
   game_info:
     # distinct_id 和 devCode 可选，系统会随机生成
     distinct_id: ""
     devcode: ""
     # wwroleId：鸣潮 ID（可选，默认系统获取）
     wwroleId: 
     # eeeroleId：战双 ID（可选，默认系统获取）
     eeeroleId: 

   # 用户信息
   user_info:
     # 库街区 bbs ID（可选，默认系统获取）
     userId: ""
   ```

2. **签到流程**  
   主程序通过 `SignInManager` 完成签到工作。  
   - 对于每个用户，系统先读取其 YAML 配置文件，检查是否启用和配置完整性。  
   - 执行游戏签到（鸣潮、战双）以及库街区签到，并根据结果记录日志。  
   - 如果签到返回信息中包含"用户信息异常"或"登录已过期"，系统会自动调用禁用操作，将该用户状态置为禁用。

3. **命令行参数**  
   运行时可添加 `--debug` 或 `--error` 参数以调整日志级别。

4. **签到信息推送**  
   如需要开启请在 `config/push.ini` 中设置 `"enable":true`，并填写信息，填写 `push_level`，推送详细程度：1=只推送总结，2=推送所有人的详细信息（一条），3=推送所有人的详细信息（多条）。具体参照[配置文档](/config/README.md)。




---


## 环境依赖

- Python 3.9 以上  
- 使用 `pip install .` 安装依赖

## 运行方式

### 本地运行

1. **克隆项目**  
   将项目克隆到本地目录：
   ```bash
   git clone https://github.com/mxyooR/Kuro-autosignin.git
   cd Kuro-autosignin
   ```

2. **安装依赖**  
   使用 pip 安装所需依赖：
   ```bash
   pip install .
   ```
> [!IMPORTANT]
> 如果您需要用到Windows的推送功能，请使用以下命令安装依赖：
> ```pip install .[windows]```

3. **配置文件**  
   在 `config` 目录下创建或修改 YAML 配置文件（如 `name.yaml`），填写必要的用户信息。

4. **运行程序**  
   执行以下命令运行主程序：
   ```bash
   python main.py
   ```

5. **调试模式**（可选）  
   如果需要调试日志信息，可以添加 `--debug` 参数：
   ```bash
   python main.py --debug
   ```

6. **查看日志**  
   程序运行后，日志文件会保存在 `logs` 目录下，可通过日志文件查看运行结果。

