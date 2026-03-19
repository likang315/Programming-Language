### Python 环境管理器

------

[TOC]

##### 01：AnaConda

- 是跨平台、跨语言的包管理器和环境管理器，不止管 Python，还能管理 R、C++ 等语言的包和依赖。
- 收费了；

###### 核心功能

- **包管理**：安装、升级、卸载软件包，自动解决包之间的依赖冲突（比 pip 更智能，尤其跨语言依赖）。
- **环境管理**：创建独立的虚拟环境（比如 conda create -n py38 python=3.8），隔离不同项目的依赖（避免 “一个项目装的包影响另一个项目”）。
- 常见发行版：Anaconda（带 conda + 150+ 预装包，体积大）、Miniconda（带 conda + 最小依赖，体积小）。

##### 02：miniforge

- 是由社区维护的**轻量级 conda 发行版**，核心是 “只包含 conda 必需组件 + 开源的包源管理”，无冗余预装包。

###### 核心特点

- 轻量小巧：安装包体积远小于 Anaconda（仅几十 MB），默认不装多余包，按需安装更灵活。
- 开源纯净：默认使用**开源的 `conda-forge` 包源**（包更全、更新更快）。
- 包含 mamba：内置 `mamba`（conda 的替代品），包安装速度比原生 conda 快很多（解决 conda 安装慢的痛点）。

<img src="/Users/likang/Downloads/miniforge.png" alt="miniforge" style="zoom:45%;" />

##### 03：mamba 命令

- conda 与 mamba 命令可直接替换。

###### 环境管理

- 创建新环境

  - `conda create -n 环境名 python=版本号`（版本号可选，如 3.8、3.9、3.10）

  - 如果不创建环境，直接安装，默认安装到 base 环境。

  - ```shell
    conda create -n data_sci python=3.9  # 回车后输入 y 确认安装依赖
    ```

- 激活环境

  - ```shell
    conda activate env_name  # 同样，终端前缀会显示环境名
    ```

- 切换 / 退出环境

  - 切换到另一个环境：先激活目标环境即可，如 `conda activate 其他环境名`

  - 退出当前环境（回到 base 环境）：

    - ```shell
      conda deactivate
      ```

- 查看所有环境

  - ```shell
    conda env list  # 或 conda info --envs，星号 * 表示当前激活的环境
    ```

- 删除环境（无需时清理）

  - ```shell
    conda env remove -n env_name  # 输入 y 确认删除，删除后无法恢复
    ```

###### 包管理（安装 / 卸载依赖）

- 安装包（推荐用 mamba，比 conda 快 10 倍）

  - ```shell
    # 安装包
    conda install numpy pandas matplotlib  # 自动解决依赖，输入 y 确认
    ```

  - 指定包版本：`mamba install pandas=1.5.3`（安装 1.5.3 版本，避免版本冲突）、

  - 从 conda-forge 源安装（包更全）：`mamba install -c conda-forge 包名`（默认已优先用该源，无需额外配置）

- 卸载包

  - ```shell
    conda remove pandas  # 卸载 pandas，输入 y 确认
    ```

- 查看当前环境已安装的包

  - ```shell
    conda list  # 列出当前环境所有包及版本
    ```

- 通过 requirements.txt 文件安装，管理软件包

  - ```shell
    mamba list --export > requirements.txt  # 导出当前环境所有包
    mamba install --file requirements.txt
    ```





