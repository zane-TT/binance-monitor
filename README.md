# binance-monitor

币安代币上新监控

## 功能简介

定时监控币安公告页面，发现新上线的代币公告后，自动抓取合约地址，并通过 Bark 推送到手机。

## 环境要求

- Python 3.8+（推荐 3.10+）
- 可用的代理 IP（用于访问币安站点）
- Bark App（iOS 推送通知）

## 安装依赖

```bash
pip install -r requirements.txt
```

安装 Playwright 浏览器内核：

```bash
playwright install chromium
```

## 配置说明

### 1. 代理 IP

打开 [monitor.py](file:///workspace/monitor.py#L15-L25)，在 `proxies` 列表中填入你的代理信息：

```python
proxies = [
    {
        "ip": "你的代理IP",
        "port": "端口",
        "username": "用户名",
        "password": "密码"
    },
    # 可继续添加更多代理...
]
```

代理 IP 可在此购买：<https://app.proxy-cheap.com/>

### 2. Bark 推送密钥

打开 [monitor.py](file:///workspace/monitor.py#L169)，将 `apiKey="your_key"` 替换为你自己的 Bark 密钥：

```python
asyncio.run(SendMsg(data=data, apiKey="你的Bark密钥"))
```

Bark 下载地址：<https://bark.day.app/#/>

### 3. app 模块

[monitor.py](file:///workspace/monitor.py#L11) 从 `app` 模块导入了 `SendMsg`，请确保项目根目录下存在 `app.py` 并实现该函数，或自行实现推送逻辑。

## 启动

```bash
python monitor.py
```

## 工作流程

1. 程序启动时会获取当前所有公告列表（避免把已有文章当作新通知）
2. 循环抓取币安公告 API，检测新文章
3. 发现新公告后使用 Playwright 访问详情页，提取合约地址
4. 通过 Bark 推送通知到手机
