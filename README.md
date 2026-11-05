# lz4 1.9.4 on loongarch64

## 构建

    make -j4 BUILD_STATIC=yes BUILD_SHARED=no CFLAGS="-fPIC -O2"
    PREFIX=$P make BUILD_STATIC=yes BUILD_SHARED=no install

零补丁、零 loongarch 改动。产物 `installed/lib/liblz4.a`。

## 头文件必须放到 lz4/ 子目录

Doris 用 `#include <lz4/lz4.h>`，而 make install 把头装在 `include/` 根下，需要额外拷一份：

    mkdir -p $P/include/lz4
    cp $P/include/lz4.h $P/include/lz4hc.h $P/include/lz4frame.h $P/include/lz4/

## 验证

已链接进 `doris_be`，`util/block_compression.cpp` 编译通过，0 undefined symbol。
