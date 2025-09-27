# OpenWrt 24.x 打印包编译指南

## 1. 准备 OpenWrt 源码环境

```bash
git clone https://github.com/openwrt/openwrt.git
cd openwrt
git checkout v24.10.3
cp feeds.conf.default feeds.conf
sed -i '1i\src-git xxx https://github.com/woniuzfb/openwrt-24-printing-packages.git' feeds.conf
sed -i '/--keep-system-cflags\|--keep-system-libs/d' tools/pkgconf/files/pkg-config
```

## 2. 下载配置文件

下载对应版本架构(比如 x86_64)的 config.buildinfo：

```bash
wget https://downloads.openwrt.org/releases/24.10.3/targets/x86/64/config.buildinfo
cp config.buildinfo .config
```

## 3. 编译打印包

```bash
./script/feeds update -a
./script/feeds install -a

# 在 Network => Printing 都选上 M，退出并保存 .config
make menuconfig

make dirclean
make toolchain/compile -j $(nproc) V=s
make target/compile -j $(nproc) V=s

# 生成全部 ipk 包
make package/hplip/compile -j $(nproc) V=s
```

## 4. 安装到 OpenWrt 设备

编译完成的文件在 `bin/packages/x86_64/xxx/` 目录中。

将 ipk 文件上传到 OpenWrt 设备并执行：

```bash
opkg remove kmod-usb-printer
opkg update
opkg install *.ipk

# 惠普打印机安装插件
hp-plugin -i

# 连接打印机到 openwrt 后执行
hp-setup -i
```

## 注意事项

- 旧版 openwrt(gcc8) 用[这个](https://github.com/woniuzfb/openwrt-21-printing-packages)
