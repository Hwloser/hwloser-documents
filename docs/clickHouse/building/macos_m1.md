# 在m1中编译clickhoust

---

- 首先，clone项目
- 准备基础库
- 解决编译问题
- 运行

## 克隆以及准备基础库

```bash
git clone --recursive git@github.com:ClickHouse/ClickHouse.git
# ...alternatively, you can use https://github.com/ClickHouse/ClickHouse.git as the repo URL.

brew update
brew install ccache cmake ninja libtool gettext llvm gcc binutils grep findutils

# if you have been installed this lib, reinstall them
brew reinstall ccache cmake ninja libtool gettext llvm gcc binutils grep findutils
```

现在我们可以开始构建项目了。

```bash
cd ClickHouse
mkdir build
export PATH=$(brew --prefix llvm)/bin:$PATH
export CC=$(brew --prefix llvm)/bin/clang
export CXX=$(brew --prefix llvm)/bin/clang++
cmake -G Ninja -DCMAKE_BUILD_TYPE=RelWithDebInfo -S . -B build
cmake --build build
# The resulting binary will be created at: build/programs/clickhouse
```

## 解决编译问题

### Cannot find objcopy

请确保binutils被安装正确，如果PATH中无法检索，请重启bash。或将brew安装binutils之后提示的export，进行手动调用。

```bash
# for example
export PATH="/opt/homebrew/opt/binutils/bin:$PATH"
```

### '/opt/homebrew/opt/z3/lib/libz3.4.11.dylib' (no such file)

请重新安装z3库，（确保您已经执行了`brew update`）

```bash
# 请注意，这里使用的是reinstall
brew reinstall z3
```

### 代码问题

请确认是否更新到了最新的代码。

```bash
git pull --recursive-submodule
```
