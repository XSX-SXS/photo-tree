# 在 Google Colab 上运行照片圣诞树项目

## 完整操作流程

### 步骤 1：创建 Colab Notebook 并配置 Node.js 环境

1. 打开 [Google Colab](https://colab.research.google.com/)
2. 创建一个新的空白 notebook
3. 运行以下命令安装 Node.js 和 npm：

```bash
%%bash
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt-get install -y nodejs
```

4. 验证 Node.js 和 npm 安装成功：

```bash
%%bash
node -v
npm -v
```

### 步骤 2：从 GitHub 克隆照片圣诞树项目

运行以下命令克隆仓库：

```bash
%%bash
git clone https://github.com/XSX-SXS/-.git 照片圣诞树
cd 照片圣诞树
```

### 步骤 3：安装项目依赖

```bash
%%bash
cd 照片圣诞树
npm install --legacy-peer-deps
```

> 注意：使用 `--legacy-peer-deps` 可以避免依赖冲突问题

### 步骤 4：安装和配置 ngrok

由于 Colab 环境的网络限制，我们需要使用 ngrok 来暴露本地服务器：

```bash
%%bash
cd 照片圣诞树
wget -q https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
tar -xf ngrok-v3-stable-linux-amd64.tgz
```

### 步骤 5：启动开发服务器

1. 首先，在一个单元格中启动 Vite 开发服务器：

```bash
%%bash
cd 照片圣诞树
npm run dev -- --host 0.0.0.0 --port 5173 > server.log 2>&1 &
```

2. 然后检查服务器是否成功启动：

```bash
%%bash
cd 照片圣诞树
sleep 5
cat server.log
```

### 步骤 6：使用 ngrok 暴露服务器

1. 访问 [ngrok官网](https://ngrok.com/) 注册账号并获取 Auth Token
2. 在 Colab 中设置你的 ngrok Auth Token：

```bash
%%bash
cd 照片圣诞树
./ngrok authtoken YOUR_NGROK_AUTH_TOKEN
```

3. 启动 ngrok 隧道：

```bash
%%bash
cd 照片圣诞树
./ngrok http 5173 > ngrok.log 2>&1 &
```

4. 获取 ngrok 访问链接：

```bash
%%bash
cd 照片圣诞树
sleep 5
cat ngrok.log | grep -o 'https://[0-9a-z\-]*\.ngrok-free\.app'
```

### 步骤 7：访问应用

复制上一步输出的 ngrok URL（类似于 https://xxxx-xx-xxx-xx-xx.ngrok-free.app），在浏览器中打开即可访问照片圣诞树应用。

## 常见问题及解决方案

### 1. 依赖安装失败

- **问题**：npm install 失败，出现依赖冲突
- **解决方案**：使用 `npm install --legacy-peer-deps` 或 `npm install --force`

### 2. 服务器启动失败

- **问题**：端口 5173 被占用
- **解决方案**：使用其他端口，如 `npm run dev -- --host 0.0.0.0 --port 3000`

### 3. ngrok 连接问题

- **问题**：ngrok 隧道无法建立
- **解决方案**：
  - 确保 ngrok Auth Token 正确
  - 尝试重新启动 ngrok：
    ```bash
    %%bash
    cd 照片圣诞树
    pkill -f ngrok
    ./ngrok http 5173 > ngrok.log 2>&1 &
    ```

### 4. 应用加载缓慢

- **问题**：Colab 环境网络延迟较高
- **解决方案**：考虑使用 `npm run build` 构建生产版本，然后使用静态文件服务器

## 生产版本部署（可选）

如果需要更好的性能，可以构建生产版本并使用静态文件服务器：

```bash
%%bash
cd 照片圣诞树
npm run build
npm install -g serve
serve -s dist -l 5173 > server.log 2>&1 &
```

然后使用 ngrok 暴露 5173 端口即可。

## 注意事项

1. Colab 实例会在一段时间不活动后自动断开，请及时保存工作
2. 每次重启 Colab 实例都需要重新安装依赖和配置环境
3. 手势识别功能需要摄像头访问权限，但由于 Colab 是远程环境，可能无法正常工作
4. 如果遇到 TensorFlow Lite 相关错误，可能是由于 Colab 环境缺少必要的硬件加速支持

## 完整 Colab Notebook 代码

```python
# Google Colab 照片圣诞树项目运行指南
# 作者: AI Assistant

# 步骤 1: 安装 Node.js
!curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
!apt-get install -y nodejs

# 步骤 2: 检查 Node.js 和 npm 版本
!node -v
!npm -v

# 步骤 3: 克隆 GitHub 仓库
!git clone https://github.com/XSX-SXS/-.git 照片圣诞树

# 步骤 4: 安装项目依赖
%cd 照片圣诞树
!npm install --legacy-peer-deps

# 步骤 5: 安装 ngrok
!wget -q https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
!tar -xf ngrok-v3-stable-linux-amd64.tgz

# 步骤 6: 启动 Vite 开发服务器
!npm run dev -- --host 0.0.0.0 --port 5173 > server.log 2>&1 &

# 步骤 7: 检查服务器状态
sleep(5)
!cat server.log

# 步骤 8: 设置 ngrok Auth Token
# 请将 YOUR_NGROK_AUTH_TOKEN 替换为你自己的 ngrok Auth Token
!./ngrok authtoken YOUR_NGROK_AUTH_TOKEN

# 步骤 9: 启动 ngrok 隧道
!./ngrok http 5173 > ngrok.log 2>&1 &

# 步骤 10: 获取访问链接
sleep(5)
!cat ngrok.log | grep -o 'https://[0-9a-z\-]*\.ngrok-free\.app'
```

---

## 最后提示

- 请确保你已经获取了自己的 ngrok Auth Token
- 手势识别功能在 Colab 环境中可能无法正常工作（需要摄像头访问）
- 如果遇到问题，可以查看 `server.log` 和 `ngrok.log` 获取详细错误信息
- 享受你的照片圣诞树体验！
