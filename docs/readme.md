



## 快速开始
本项目使用现代CMake预设工作流。需提前安装cmake3.23.2及以上版本。

### 配置CMake

```bash
cmake --preset dev-windows

# Linux系统使用
cmake --preset dev

```
如果你使用 VScode，也可以在安装cmake tool插件之后，使用`CMake：配置`命令进行配置。

### 构建编译

```bash
# debug模式
cmake --build --preset dev-windows-debug --parallel
# release模式
cmake --build --preset dev-windows-release --parallel

# Linux系统
cmake --build --preset dev-debug

```


### 安装

```bash
cmake --install out/build/dev-windows --config release

```