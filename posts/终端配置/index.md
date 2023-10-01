# 终端配置


Alacritty + Zsh + Oh-My-Zsh + Starship

<!--more-->

## Alacritty

1. [下载](https://alacritty.org/)。
2. [字体](https://www.nerdfonts.com/)，推荐`JetBrainsMono Nerd Font`。
3. [主题](https://github.com/alacritty/alacritty-theme)，推荐`gruvbox_dark`。
4. 配置文件。
   - Windows:`C:\Users\backt\AppData\Roaming\alacritty\alacritty.yml`
   - MacOS:

```yaml
# 自动刷新
live_config_reload: true


# Tab 缩进
tabspaces: 4


# 默认终端
shell:
  # WSL
  program: C:/WINDOWS/system32/wsl.exe
  args:
    - -d Ubuntu


# 默认目录
working_directory: C:/Users/backt


# 字体配置
font:
  size: 10
  normal:
    family: JetBrainsMono Nerd Font
    style: Medium
  bold:
    family: JetBrainsMono Nerd Font
    style: Bold
  italic:
    family: JetBrainsMono Nerd Font
    style: Italic


# 光标配置
cursor:
  style:
    # - Block: 方块
    # - Underline: 下划线
    # - Beam: 竖线
    shape: Beam


# 窗口设置
window:
  # 背景透明度
  opacity: 0.9

  # 窗口大小
  dimensions:
    columns: 100
    lines: 30

  # 窗口位置
  position:
    x: 100
    y: 100

  # 窗口内边距
  padding:
    x: 5
    y: 5

  # 显示模式
  # - Windowed: 窗口化
  # - Maximized: 最大化
  # - Fullscreen: 全屏
  startup_mode: Windowed

  # 窗口修饰
  # - full: 有边界 + 标题栏
  # - none: 无边界 + 标题栏
  decorations: full
  dynamic_padding: true
  dynamic_title: true


# 滚动
scrolling:
  history: 100000       # 历史保留行数
  multiplier: 3         # 每次滚动行数
  faux_multiplier: 100  # 每次滚动行数（分屏时）
  auto_scroll: true     # 自动滚动至最新行


# 主题 (Gruvbox dark)
colors:
  # Default colors
  primary:
    # hard contrast: background = '0x1d2021'
    background: '0x282828'
    # soft contrast: background = '0x32302f'
    foreground: '0xebdbb2'

  # Normal colors
  normal:
    black:   '0x282828'
    red:     '0xcc241d'
    green:   '0x98971a'
    yellow:  '0xd79921'
    blue:    '0x458588'
    magenta: '0xb16286'
    cyan:    '0x689d6a'
    white:   '0xa89984'

  # Bright colors
  bright:
    black:   '0x928374'
    red:     '0xfb4934'
    green:   '0xb8bb26'
    yellow:  '0xfabd2f'
    blue:    '0x83a598'
    magenta: '0xd3869b'
    cyan:    '0x8ec07c'
    white:   '0xebdbb2'
```

<br>

## Zsh

```bash
# 查看是否安装zsh
cat /etc/shells | grep zsh

# 安装zsh
sudo apt install zsh

# 设置zsh为默认终端
sudo chsh -s /bin/zsh
```

<br>

## Oh-My-Zsh

```bash
# 安装Oh-My-Zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 插件
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions

# 配置
vim ~/.zshrc
# plugins=(
#     git
#     zsh-autosuggestions
#     zsh-syntax-highlighting
#     zsh-completions
# )
source ~/.zshrc
```

<br>

## Starship

```bash
# 安装starship
curl -sS https://starship.rs/install.sh | sh

# 配置
vim ~/.zshrc
# eval "$(starship init zsh)"
source ~/.zshrc

# [主题](https://starship.rs/presets/#plain-text-symbols)
starship preset plain-text-symbols -o ~/.config/starship.toml
```

