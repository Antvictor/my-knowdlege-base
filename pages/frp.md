tags:: note
first:: [[2025_01_24]]
alias:: 
status::

-
- ## What
- frp 是一个内网穿透工具，是高性能的方向代理应用，专注于内网穿透。支持多种协议，TUP、UDP、HTTP、HTTPS，并具备P2P通信能力。
- ## Why
- 通过在具有公网 IP 的节点上部署 frp 服务端，您可以轻松地将内网服务穿透到公网，并享受以下专业特性：
	- 多种协议支持：客户端服务端通信支持 TCP、QUIC、KCP 和 Websocket 等多种协议。
	- TCP 连接流式复用：在单个连接上承载多个请求，减少连接建立时间，降低请求延迟。
	- 代理组间的负载均衡。
	- 端口复用：多个服务可以通过同一个服务端端口暴露。
	- P2P 通信：流量不必经过服务器中转，充分利用带宽资源。
	- 客户端插件：提供多个原生支持的客户端插件，如静态文件查看、HTTPS/HTTP 协议转换、HTTP、SOCKS5 代理等，以便满足各种需求。
	- 服务端插件系统：高度可扩展的服务端插件系统，便于根据自身需求进行功能扩展。
	- 用户友好的 UI 页面：提供服务端和客户端的用户界面，使配置和监控变得更加方便。
- ## How
	- ## 安装
	- ### 环境
	- 首先需要安装[GO环境](https://go.dev/doc/install#requirements)。 [GO安装包下载](https://github.com/fatedier/frp/releases)
	  下载后解压到/usr/local/  会自动解压为go文件夹，然后配置环境：export PATH=$PATH:usr/local/go/bin. 
	  mac是安装包，安装完后直接配置环境即可
	- ### 下载
	- 您可以从 GitHub 的 [Release](https://github.com/fatedier/frp/releases) 页面中下载最新版本的客户端和服务器二进制文件。所有文件都打包在一个压缩包中，还包含了一份完整的配置参数说明。
	- ### 部署
	- 编写配置文件，目前支持的文件格式包括 TOML/YAML/JSON，旧的 INI 格式仍然支持，但已经不再推荐。
	- 使用以下命令启动服务器：`./frps -c ./frps.toml`。
	- 使用以下命令启动客户端：`./frpc -c ./frpc.toml`。
	- 如果需要在后台长期运行，建议结合其他工具，如 [systemd](https://gofrp.org/zh-cn/docs/setup/systemd/) 和 `supervisor`。
	- ### 代理类型
	  frp 支持多种代理类型，以适应不同的使用场景。以下是一些常见的代理类型：
		- **TCP**：提供纯粹的 TCP 端口映射，使服务端能够根据不同的端口将请求路由到不同的内网服务。
		- **UDP**：提供纯粹的 UDP 端口映射，与 TCP 代理类似，但用于 UDP 流量。
		- **HTTP**：专为 HTTP 应用设计，支持修改 Host Header 和增加鉴权等额外功能。
		- **HTTPS**：类似于 HTTP 代理，但专门用于处理 HTTPS 流量。
		- **STCP**：提供安全的 TCP 内网代理，要求在被访问者和访问者的机器上都部署 frpc，不需要在服务端暴露端口。
		- **SUDP**：提供安全的 UDP 内网代理，与 STCP 类似，需要在被访问者和访问者的机器上都部署 frpc，不需要在服务端暴露端口。
		- **XTCP**：点对点内网穿透代理，与 STCP 类似，但流量不需要经过服务器中转。
		- **TCPMUX**：支持服务端 TCP 端口的多路复用，允许通过同一端口访问不同的内网服务。
	-
	- #### TCP连接
		- 首先在服务器的frp中配置frps.toml 绑定需要代理的端口号
		  `bindPort=7000`
		  启动服务器端：`./frps -c ./frps.toml`
		- 然后在客户端修改frpc.toml
		  ```
		  serverAddr="服务器公网地址｜解析到服务器的域名"
		  serverPort="服务器bindPort的端口"
		  [[proxies]]
		  name = "ssh"
		  type = "tcp"
		  localIP = "127.0.0.1"
		  localPort = 22
		  remotePort = 6000
		  ```
		  localIP 和 localPort是需要被代理的内网服务器地址和端口
		  remotePort 是frp监听的端口，访问此端口的流量将被转发的到本地服务的相应端口上
		- 使用`ssh -o Part=6000 本地服务用户名@公网地址` 进行连接，密码也是本地服务的密码。
		- 如果是Mac需要在设置->共享中打开远程登录，如果是Linux需要打开对应端口的防火墙。
	- #### Http
	-
- [[使用 systemd]]
-