# 卡片视觉规范与 HTML 模板

本文件规定 maker 读书图解的配色、卡片样式和 HTML renderer 结构。生成时直接复制模板改文字。

## 配色（年轻时尚多巴胺风，紫粉主调）

| 用途 | 色值 | 说明 |
|---|---|---|
| 主色-紫 | `#8B5CF6` / `#7C3AED` | 主强调、认知金句卡、信息卡标题 |
| 主色-靛蓝 | `#6366F1` / `#4F46E5` | 行动金句卡、标签pill、卡片顶条 |
| 信息卡背景 | `linear-gradient(135deg,#C4B5FD 0%,#F9A8D4 100%)` | 开头书籍信息卡，鲜艳紫粉渐变 |
| 信息卡边框 | `#DDD6FE` | 配合信息卡背景的淡紫边 |
| 信息卡标题 | `#4C1D95` | 深紫，在紫粉背景上高对比 |
| 强调-珊瑚橙 | `#F97316` / `#EA580C` | 中间工具/杠杆/边界提示，小面积点缀 |
| 正向-薄荷绿 | `#10B981` / `#059669` | 目标/最优路径/落地动作 |
| 警示-红 | `#EF4444` / `#DC2626` | 陷阱/旧杠杆 |
| 浅底-绿 | `#ECFDF5` | 目标卡片浅底 |
| 浅底-橙 | `#FFF7ED` | 工具卡片浅底 |
| 浅底-红 | `#FEF2F2` | 陷阱卡片浅底 |
| 浅底-紫 | `#F5F3FF` | 认知卡片浅底 |
| 最优路径底 | `linear-gradient(135deg,#ECFDF5,#D1FAE5)` | 绿色渐变高亮 |
| 金句块底 | `linear-gradient(135deg,#8B5CF6,#EC4899)` | 紫→粉渐变，白字，和信息卡呼应 |
| 正文 | `#1A1B1C` | 主文字 |
| 次文字 | `#374151` / `#4B5563` | 说明文字 |
| 卡片底 | `#FFFFFF` | 默认白卡 |

## 卡片样式规范

- 圆角：信息卡 16px，普通卡片 10px，提示块 8-12px
- 边框：默认 `1px solid #E5E7EB`；强调卡片用 `1.5px solid` 对应色
- 内边距：卡片 12px；信息卡 18px
- 间距：卡片之间 gap 8px，段落之间 margin-bottom 14px
- 字体：`'PingFang SC','Segoe UI',Arial,sans-serif`
- 标题字号：信息卡书名 20px，段标题 13px，卡片小标题 11px bold
- 正文：11px，行高 1.6-1.9
- 标签 pill：`padding:4px 12px; border-radius:14px; font-size:11px; font-weight:600; color:#fff`，背景用对应色
- 多列布局：`display:flex; flex-wrap:wrap; gap:8px`，子项 `flex:1 1 200px; min-width:0`
- 封面图：宽 110px，圆角 8px，阴影 `0 4px 14px rgba(139,92,246,0.3)`
- 作者头像：宽 90px 圆形，白边 3px，紫色调阴影；搜到合适公开照才放，搜不到整块删掉，不要用同名同姓的其他人照片

## HTML renderer 硬约束

- 起始行逐字：```html type="renderer"
- 首块 `<html style="margin:0;padding:0;">` 后紧跟 `<title>书名 拆解</title>`
- 无 DOCTYPE/head/body/TEXT 标签
- CSS 全部内联，无 `<style>` 标签
- 根容器自然撑高，禁 100vh
- 多列必须 flex-wrap，窄屏单列
- 每个 renderer 块 ≤300 行，超出拆分
- 静态无脚本（除非用户明确要求交互）

## 完整模板示例

三个 renderer 块，每块 ≤300 行。首块带 `<title>`，后两块不重复。

### 第一块：信息卡 + 混淆概念 + 核心模型

```html type="renderer"
<html style="margin:0;padding:0;">
<title>《书名》拆解</title>
<div style="background-color:transparent;box-sizing:border-box;font-family:'PingFang SC','Segoe UI',Arial,sans-serif;color:#1A1B1C;">

  <!-- 0. 书籍信息卡 -->
  <div style="padding:18px;background:linear-gradient(135deg,#C4B5FD 0%,#F9A8D4 100%);border-radius:16px;margin-bottom:16px;display:flex;gap:18px;align-items:flex-start;flex-wrap:wrap;border:1px solid #DDD6FE;">
    <div style="flex:0 0 auto;width:110px;flex-shrink:0;">
      <img src="封面URL" alt="书名封面" style="width:100%;height:auto;border-radius:8px;box-shadow:0 4px 14px rgba(139,92,246,0.3);display:block;">
    </div>
    <div style="flex:1 1 260px;min-width:0;">
      <div style="font-size:20px;font-weight:800;line-height:1.3;color:#4C1D95;">《书名》副标题</div>
      <div style="font-size:12px;color:#4B5563;margin-top:6px;line-height:1.7;">作者 · 出版社 年份</div>
      <div style="font-size:12px;color:#1A1B1C;margin-top:8px;line-height:1.7;">
        <strong>一句话定位：</strong>……<br>
        <strong>核心公式：</strong>……
      </div>
      <div style="display:flex;flex-wrap:wrap;gap:6px;margin-top:10px;">
        <span style="padding:4px 12px;background:#6366F1;color:#fff;border-radius:14px;font-size:11px;font-weight:600;">标签1</span>
        <span style="padding:4px 12px;background:#F97316;color:#fff;border-radius:14px;font-size:11px;font-weight:600;">标签2</span>
        <span style="padding:4px 12px;background:#10B981;color:#fff;border-radius:14px;font-size:11px;font-weight:600;">标签3</span>
      </div>
    </div>
    <!-- 作者头像：搜到作者本人公开照才放，搜不到整块删除。90px圆形白边 -->
    <div style="flex:0 0 auto;width:90px;flex-shrink:0;">
      <img src="作者头像URL" alt="作者照片" style="width:90px;height:90px;border-radius:50%;object-fit:cover;border:3px solid #fff;box-shadow:0 4px 12px rgba(139,92,246,0.3);display:block;">
    </div>
  </div>

  <!-- 1. 混淆概念：绿目标/橙工具/红陷阱 -->
  <div style="font-size:13px;font-weight:700;margin-bottom:8px;">① 三个最容易混淆的概念</div>
  <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px;">
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#ECFDF5;border-left:4px solid #10B981;border-radius:8px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#059669;">XX ✅ 真正目标</div>
      <div style="font-size:11px;color:#374151;margin-top:4px;line-height:1.6;">说明……</div>
    </div>
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#FFF7ED;border-left:4px solid #F97316;border-radius:8px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#EA580C;">XX ⚠️ 中间工具</div>
      <div style="font-size:11px;color:#374151;margin-top:4px;line-height:1.6;">说明……</div>
    </div>
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#FEF2F2;border-left:4px solid #EF4444;border-radius:8px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#DC2626;">XX ❌ 认知陷阱</div>
      <div style="font-size:11px;color:#374151;margin-top:4px;line-height:1.6;">说明……</div>
    </div>
  </div>

  <!-- 2. 核心模型：白卡+顶部色条 -->
  <div style="font-size:13px;font-weight:700;margin-bottom:8px;">② 核心公式/模型</div>
  <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:6px;">
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#fff;border:1px solid #E5E7EB;border-top:3px solid #8B5CF6;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#7C3AED;">要素A</div>
      <div style="font-size:11px;color:#374151;margin-top:5px;line-height:1.6;">说明……</div>
    </div>
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#fff;border:1px solid #E5E7EB;border-top:3px solid #F97316;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#EA580C;">要素B</div>
      <div style="font-size:11px;color:#374151;margin-top:5px;line-height:1.6;">说明……</div>
    </div>
    <div style="flex:1 1 180px;min-width:0;padding:12px;background:#fff;border:1px solid #E5E7EB;border-top:3px solid #10B981;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#059669;">要素C</div>
      <div style="font-size:11px;color:#374151;margin-top:5px;line-height:1.6;">说明……</div>
    </div>
  </div>
  <div style="font-size:11px;color:#6B7280;line-height:1.6;">要素关系说明……</div>
</div>
</html>
```

### 第二块：路径对比 + 认知原则 + 边界反方

```html type="renderer"
<html style="margin:0;padding:0;">
<div style="background-color:transparent;box-sizing:border-box;font-family:'PingFang SC','Segoe UI',Arial,sans-serif;color:#1A1B1C;">

  <!-- 3. 路径对比：普通人最优路径用绿渐变高亮 -->
  <div style="font-size:13px;font-weight:700;margin-bottom:8px;">③ 多条实现路径横向对比</div>
  <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px;">
    <div style="flex:1 1 200px;min-width:0;padding:12px;background:#fff;border:1px solid #FECACA;border-top:3px solid #EF4444;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#DC2626;">路径A</div>
      <div style="font-size:11px;color:#374151;margin-top:5px;line-height:1.7;">做法/门槛/优劣……</div>
    </div>
    <div style="flex:1 1 200px;min-width:0;padding:12px;background:#fff;border:1px solid #FED7AA;border-top:3px solid #F97316;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#EA580C;">路径B</div>
      <div style="font-size:11px;color:#374151;margin-top:5px;line-height:1.7;">做法/门槛/优劣……</div>
    </div>
    <div style="flex:1 1 200px;min-width:0;padding:12px;background:linear-gradient(135deg,#ECFDF5,#D1FAE5);border:2px solid #10B981;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:800;color:#059669;">路径C ★ 普通人最优</div>
      <div style="font-size:11px;color:#064E3B;margin-top:5px;line-height:1.7;">做法/门槛/为什么最优……</div>
    </div>
  </div>

  <!-- 4. 认知原则：浅色边框卡片 -->
  <div style="font-size:13px;font-weight:700;margin-bottom:8px;">④ 内在认知原则</div>
  <div style="display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px;">
    <div style="flex:1 1 260px;min-width:0;padding:11px 12px;background:#fff;border:1px solid #E9D5FF;border-radius:10px;box-sizing:border-box;">
      <div style="font-size:11px;font-weight:700;color:#7C3AED;">① 原则标题</div>
      <div style="font-size:11px;color:#374151;margin-top:4px;line-height:1.6;">说明……</div>
    </div>
    <!-- 其他原则卡片同理，边框用对应色 -->
  </div>

  <!-- 5. 边界与反方：橙左边框提示块 -->
  <div style="padding:12px 14px;background:#FFF7ED;border-left:4px solid #F97316;border-radius:0 10px 10px 0;">
    <div style="font-size:12px;font-weight:700;color:#EA580C;margin-bottom:6px;">⑤ 这本书的边界与反方</div>
    <div style="font-size:11.5px;color:#374151;line-height:1.8;">
      · 什么时候不适用……<br>
      · 作者局限……<br>
      · 反方观点……
    </div>
  </div>
</div>
</html>
```

### 第三块：金句双栏 + 总结 + 落地

```html type="renderer"
<html style="margin:0;padding:0;">
<div style="background-color:transparent;box-sizing:border-box;font-family:'PingFang SC','Segoe UI',Arial,sans-serif;color:#1A1B1C;">

  <!-- 6. 金句分两类 -->
  <div style="font-size:13px;font-weight:700;margin-bottom:8px;">⑥ 金句分两类</div>
  <div style="display:flex;flex-wrap:wrap;gap:10px;margin-bottom:14px;">
    <div style="flex:1 1 280px;min-width:0;padding:13px 14px;background:#EEF2FF;border:1px solid #C7D2FE;border-radius:12px;">
      <div style="font-size:11px;font-weight:800;color:#4F46E5;margin-bottom:7px;">→ 改变行动的金句</div>
      <div style="font-size:12px;line-height:1.9;color:#1A1B1C;">"金句……"<br>"金句……"</div>
    </div>
    <div style="flex:1 1 280px;min-width:0;padding:13px 14px;background:#FDF4FF;border:1px solid #E9D5FF;border-radius:12px;">
      <div style="font-size:11px;font-weight:800;color:#7C3AED;margin-bottom:7px;">→ 改变认知的金句</div>
      <div style="font-size:12px;line-height:1.9;color:#1A1B1C;">"金句……"<br>"金句……"</div>
    </div>
  </div>

  <!-- 7a. 一句话总结：紫粉渐变 -->
  <div style="padding:16px;background:linear-gradient(135deg,#8B5CF6,#EC4899);border-radius:12px;margin-bottom:10px;">
    <div style="font-size:12px;font-weight:800;color:#FDE68A;margin-bottom:8px;">⑦ 一句话总结</div>
    <div style="font-size:13px;line-height:1.7;color:#fff;">全书一句话……</div>
  </div>

  <!-- 7b. 落地动作：浅绿底绿边框 -->
  <div style="padding:14px 16px;background:#ECFDF5;border:1.5px solid #10B981;border-radius:12px;">
    <div style="font-size:12px;font-weight:800;color:#059669;margin-bottom:7px;">本周就能做的动作</div>
    <div style="font-size:12px;line-height:1.9;color:#1A1B1C;">
      <strong>1.</strong> 具体动作……<br>
      <strong>2.</strong> 具体动作……<br>
      <strong>3.</strong> 具体动作……
    </div>
  </div>
</div>
</html>
```
