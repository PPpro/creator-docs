# 模块

引擎和编辑器通过模块向开发者暴露功能接口，模块以 ECMAScript 模块形式存在。

⚠️ 注意，从 3.0 开始，将不能通过全局变量 `cc` 访问引擎功能！

## 引擎模块

目前，引擎只提供了一个公开模块 `'cc'`。

模块 `'cc'` 的内容是动态的，其内容和 **项目设置** 中的 **功能裁剪** 设置有关。

### 引擎日志输出

示例如下：

```ts
import { log } from 'cc';
log('Hello world!');
```

## 编辑器模块

编辑器模块都在 `'cce:'` 协议下（“cce” 代表 “**C**ocos**C**reator**E**ditor”）。

除模块 `'cce.env'` 外，所有编辑器模块仅在编辑器环境下有效。例如，在预览和构建后的环境中是无法访问编辑器模块的，相反，在 **场景编辑器** 中则可以访问到。

| 模块名称     | 说明             |
| :---------- | :-------------- |
| `'cce.env'` | 用于访问构建时常量 |
<!--
| `'cce:gizmo'` | Gizmo          |
-->

### 构建时的常量

编辑器模块 `'cce.env'` 暴露了一些构建时的 **常量**，这些常量代表执行环境、调试级别或平台标识等。与其它编辑器模块不同，`'cce.env'` 允许在非编辑器环境中访问。

由于这些常量都以 `const` 声明，提供了很好的代码优化机会。

#### 执行环境

| 名称（类型都为 `boolean`）| 说明    |
| :-------- | :------------------- |
| `BUILD`   | 是否正在构建后的环境中运行 |
| `PREVIEW` | 是否正在预览环境中运行    |
| `EDITOR`  | 是否正在编辑器环境中运行  |

#### 调试级别

| 名称（类型都为 `boolean`） | 说明 |
| :------ | :------ |
| `DEBUG` | 是否处于调试模式。仅当构建时未勾选调试选项的情况下为 `false`，其它情况下都为 `true` |
| `DEV`   | 等价于 `DEBUG`/`EDITOR`/`PREVIEW` |

#### 平台标识

下表列出的常量表示是否正在 **某一个** 或 **某一类** 平台上运行，常量的类型都是 `boolean`。
<!-- 下表请按字典序排序 -->

| 名称        | 代表平台      | `MINIGAME` “小游戏” | `RUNTIME_BASED` 基于 Cocos Runtime | `SUPPORT_JIT` 支持 JIT |
| :---------- | :---------- | :----------------- | :----------------- | :----------------- |
| `HTML5`     | Web         | ❌                  | ❌                 | ❌                 |
| `NATIVE`    | 原生平台     | ❌                  | ❌                 | ❌                 |
| `ALIPAY`    | 支付宝小游戏  | ✔️                   | ❌                 | ✔️                 |
| `BAIDU`     | 百度小游戏    | ✔️                   | ❌                | ✔️                  |
| `BYTEDANCE` | 字节跳动小游戏 | ✔️                   | ❌                | ✔️                  |
| `WECHAT`    | 微信小游戏    | ✔️                   | ❌                | ✔️                  |
| `XIAOMI`    | 小米快游戏    | ✔️                   | ❌                | ✔️                  |
| `COCOSPLAY` | Cocos Play  | ❌                   | ✔️                 | ✔️                 |
| `HUAWEI`    | 华为快游戏    | ❌                   | ✔️                 | ✔️                 |
| `OPPO`      | OPPO 小游戏  | ❌                   | ✔️                 | ✔️                 |
| `VIVO`      | vivo 小游戏  | ❌                   | ✔️                 | ✔️                 |

#### 调试模式下的输出

示例如下：

```ts
import { log } from 'cc';
import { DEV } from 'cce.env';

if (DEV) {
    log('I am in development mode!');
}
```