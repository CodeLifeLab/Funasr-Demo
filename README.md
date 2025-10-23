# Funasr-Demo
1. 开启一个ubuntu虚拟机（腾讯云）/本地不需要
2. 安装uv([MCP学习一——UV安装使用教程](https://mp.weixin.qq.com/s/G0gkWOhpAi2_4QQUj5mO3Q))
3. 初始化环境服务器环境
```bash
# 安装python
uv python install 3.12

 # 初始化项目
uv init asr-server-learn 
cd asr-client-server-learn 

# 进入虚拟环境
uv venv
.venv\Scripts\activate 

# 克隆
git clone https://github.com/modelscope/FunASR.git

# 复制funasr_wss_server.py --> asr-client-server-learn 
# 复制funasr目录 -->  asr-client-server-learn 或者使用 uv add Funasr

# 下载依赖
uv install #推荐 前提已经下载了pyproject.toml文件
	# 或（不推荐）
	uv add -r requirements_server.txt
	uv add funasr
# 在asr-client-server-learn下运行funasr_wss_server.py
uv run funasr_wss_server.py


```
4. 服务器端代码修改
```python
# 1. 不使用ssl 48-54
parser.add_argument(
    "--certfile",
    type=str,
    default="",
    required=False,
    help="certfile for ssl",
)

# 2.替换 329-345代码
async def main():
    async with websockets.serve(
        ws_serve, args.host, args.port, subprotocols=["binary"], ping_interval=None
    ) as server:
        # 保持服务器运行
        await asyncio.Future()  # 这会让服务器一直运行
asyncio.run(main())
# 3.修改websockts连接参数 138行
async def ws_serve(websocket):

# 4. 运行服务端
uv run funasr_wss_server.py
```
4. 客户端环境搭建
```bash
# 1.安装uv 安装uv([MCP学习一——UV安装使用教程](https://mp.weixin.qq.com/s/G0gkWOhpAi2_4QQUj5mO3Q))

# 2.# 安装python
uv python install 3.12

 # 初始化项目
uv init asr-client-learn 
cd asr-client-server-learn 

# 进入虚拟环境
uv venv
.venv\Scripts\activate 

# 复制funasr_wss_client.py --> asr-client-learn 
# 下载依赖
uv add -r requirements_client.txt

```
5. 客户端代码修改
```python
# 45行
parser.add_argument("--ssl", type=int, default=0, help="1 for ssl connect, 0 for no ssl")
# 329行 为：
uri = "wss://{}".format(args.host)

# 20行 添加服务地址
parser.add_argument(
    "--host", type=str, default="99c23e50c69848ac8c7267085a7b2414--10095.ap-shanghai2.cloudstudio.club", required=False, help="host ip, localhost, 0.0.0.0"
)

# 4. 运行服务器端的代码
uv run funasr_wss_client.py
```

## 备注
pyproject.toml为服务器端的依赖文件，在服务器端可以使用`uv install`这个命令，下载所有依赖。

![image.png](https://starsjsm-images.oss-cn-beijing.aliyuncs.com/202510232029794.png)
                                     公众号

![image.png](https://starsjsm-images.oss-cn-beijing.aliyuncs.com/202510232029460.png)
				 群聊（如果过期了，可以在公众号给我留言）
