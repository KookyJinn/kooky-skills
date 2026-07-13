---
name: prd-generator-by-kooky
description: 生成产品需求文档（PRD）HTML文件，包含原型线框图和需求说明。当用户提到PRD、需求文档、产品需求、原型图、需求原型、写需求、出需求时触发。支持多页面目录导航、无限画布布局、HTML/CSS模拟线框图。
version: 1.1.0
---

# PRD 需求文档生成器 by Kooky

## 概述

根据用户描述的产品需求，生成独立的单文件 HTML PRD 文档。文档包含：
- 左侧：用 HTML/CSS 模拟的原型线框图
- 右侧：详细的需求说明文字
- 左侧目录栏：多页面导航，支持收起/展开

## 触发场景

用户说以下任意一种：
- "帮我写一个XX功能的PRD"
- "生成需求文档"
- "出个需求原型"
- "写个PRD"
- `/prd`

## 生成流程

### 1. 收集需求信息

向用户确认：
- **需求名称**：如"工单列表批量操作"
- **所属版本**：如"V4.2.0"
- **需求页面列表**：有哪些子页面（如：需求概述、功能A、功能B、改动范围）
- **每个页面的核心内容**：原型描述 + 需求说明

如果用户一次性给了足够信息，可跳过确认直接生成。

### 2. 读取模板

读取模板文件 `assets/prd-template.html`，基于模板结构生成新 PRD。

### 3. 生成 HTML 文件

**文件命名**：`{项目名}——{版本号}_{需求名称}.html`
**输出位置**：用户指定目录，或默认 `PRD-Demo/` 目录

## 布局规范（严格遵守）

### 整体结构

```
┌─────────────────────────────────────────────────┐
│ 顶部栏 (48px, 深色背景)                           │
├────────┬───────────────────┬────────────────────┤
│ 目录栏  │   原型区           │   说明区            │
│ 240px  │   不压缩            │   不压缩             │
│ 可收起  │   min-width:1400px │   min-width:600px  │
│        │                    │                    │
│        │   ◀ 收起按钮        │                    │
└────────┴───────────────────┴────────────────────┘
```

- **无限画布**：原型区和说明区按内容自然撑开，不固定宽度，超出屏幕时横向滚动
- **目录栏**：240px 固定，支持 ◀/▶ 收起展开，收起后主内容区全宽
- **左对齐**：所有内容左对齐，不居中

### 配色体系（与 Axure 一致）

| 用途 | 颜色 | CSS class |
|------|------|-----------|
| 关键条件 | 深红 #9B0000 | `.c-key` |
| 警告/重要 | 红色 #CA0606 | `.c-warn` |
| 补充说明 | 蓝色 #0076FF | `.c-note` |
| 提示 | 橙色 #D46B08 | `.c-hint` |
| 成功 | 绿色 #2E7D32 | `.c-green` |
| 次要 | 灰色 #999 | `.c-gray` |

### 说明框样式

- **浅黄提示框** `.note-box`：提示性说明
- **浅蓝信息框** `.info-box`：补充说明
- **浅红警告框** `.danger-box`：注意事项

## 原型绘制规范

### 可用 Mock UI 组件

模板中已内置以下组件，直接使用：

| 组件 | class | 用途 |
|------|-------|------|
| 按钮 | `.mock-btn-primary/default/danger` | 操作按钮 |
| 输入框 | `.mock-input` | 搜索框等 |
| 下拉框 | `.mock-select` | 筛选条件 |
| 表格 | `.mock-table` | 列表数据 |
| 复选框 | `.mock-checkbox` / `.mock-checkbox.checked` | 勾选状态 |
| 标签 | `.mock-tag-blue/green/orange` | 状态标签 |
| 链接 | `.mock-link` | 操作链接 |
| 弹窗 | `.mock-modal` | 弹窗交互 |
| 流程节点 | `.flow-node` / `.flow-arrow` | 流程图 |
| 标注 | `.annotation-marker` / `.highlight-box` | 红圈标注 |
| 分页 | `.mock-pagination` | 分页器 |

### 绘制原则

1. **尽量还原真实系统 UI**：表格、按钮、表单用 mock 组件模拟
2. **用标注突出改动点**：`<span class="annotation-marker">新增</span>` + 红色虚线圈 `.highlight-box`
3. **复杂交互用流程图**：`.flow-container` > `.flow-node` + `.flow-arrow`
4. **弹窗用模态框组件**：`.mock-modal` 包含 header/body/footer

### 流程图和泳道图规范

#### 连接线规范
- **正交路由**：流程图连接线应使用正交路由（水平+垂直组合），避免斜线直接连接节点
- **实现方式**：使用 SVG `<path>` 元素，通过 `H`（水平线）和 `V`（垂直线）命令组合绘制
- **示例**：`<path d="M 100,200 H 300 V 400" />` 表示从(100,200)水平到(300,200)再垂直到(300,400)

#### 箭头位置
- **终点箭头**：箭头应该在连线的终点（目标节点处），表示方向
- **实现方式**：使用 SVG `<marker>` 定义箭头，通过 `marker-end` 属性应用到路径终点
- **箭头颜色**：应与连接线颜色一致

#### 泳道图布局
- **纵向布局优先**：优先使用纵向布局（每列一个泳道），而非横向布局（每行一个泳道）
- **布局结构**：顶部为泳道标题行，下方为流程节点区域，流程从上到下流动
- **跨泳道连接**：跨泳道连接使用水平线，同泳道内使用垂直线

#### SVG vs CSS
- **复杂图表用 SVG**：流程图、泳道图等复杂图表建议使用 SVG 而非 CSS，便于精确控制连线和布局
- **简单布局用 CSS**：简单的表格、卡片布局可以使用 CSS

### 列表和表格规范

#### 现有列用途理解
修改列表前，先确认并理解现有列的业务规则和用途，避免错误使用：
- 查看现有列表的完整列结构
- 理解每列的业务含义和使用场景
- 不要随意复用已有列来展示不相关的数据
- 如需新增列，应该作为独立的新列，而非修改现有列的用途

#### 子字段排列方式
分组列组的子字段应该纵向排列在单个单元格内，而非横向平铺：
- **正确做法**：一个列组占一个单元格，子字段在该单元格内纵向排列
- **错误做法**：一个列组的多个子字段横向平铺成多个单元格
- **示例**：
  ```html
  <!-- 正确：子字段纵向排列 -->
  <td>
    <div>子字段1：值1</div>
    <div>子字段2：值2</div>
  </td>
  
  <!-- 错误：子字段横向平铺 -->
  <td>子字段1</td>
  <td>子字段2</td>
  ```

## 需求说明写作规范

### 右侧说明区结构

每个页面的说明区包含：
```html
<div class="desc-header">
    <div class="desc-breadcrumb">版本 > 页面名</div>
    <h2>标题</h2>
</div>
<div class="desc-content">
    <div class="sub-title">1.1 子标题</div>
    <p class="indent">说明文字，用 <span class="c-key">关键条件</span> 标注</p>
    <div class="note-box">提示说明</div>
    <div class="info-box">补充说明</div>
</div>
```

### 写作要点

- 用**编号子标题**组织：1.1、1.2、2.1...
- 关键条件用 `.c-key` 深红标注
- 警告用 `.c-warn` 红色标注
- 补充说明放 `.info-box` 或 `.note-box`
- 有接口定义时附上 API 路径和参数
- 有权限要求时附权限矩阵表格

## 目录导航结构

```html
<div class="nav-group">
    <div class="nav-group-title" onclick="toggleGroup(this)">
        <span class="arrow open">▶</span> 版本名称
    </div>
    <div class="nav-group-items">
        <div class="nav-item active" onclick="switchPage('page-xxx', this)">
            <span class="dot"></span> 页面名称
            <span class="badge">新</span>  <!-- 可选 -->
        </div>
    </div>
</div>
```

- 每个版本一个分组，可折叠
- 新功能页面加 `<span class="badge">新</span>`
- 页面 ID 格式：`page-英文标识`，对应说明区 `desc-英文标识`

## 页面类型和路径规范

### 页面类型明确
每个页面应该明确标注是"配置页"还是"列表页"，避免混淆：
- **配置页**：用于配置规则、设置参数的页面（如"工单配置页"）
- **列表页**：用于展示数据列表的页面（如"工单列表页"）

在 PRD 中应该：
- 在页面标题或说明中明确标注页面类型
- 不同页面类型的原型和说明应该体现其特点

### 路径准确性
配置页和列表页的路径要准确区分，不能混淆：
- 在涉及页面列表中，明确标注每个页面的完整路径
- 配置页和列表页通常是不同的 URL 路径
- 如果不确定路径，应该先查看实际系统或询问用户

## 页面切换 JS

模板已包含 `switchPage()`、`toggleGroup()`、`toggleSidebar()` 函数和 hash 路由支持，无需重写。

## 空格拖拽平移（内置功能）

每个生成的 PRD 必须包含空格+鼠标拖拽平移画布的功能（参考 Axure 原型预览器实现）。

### 实现方案（Axure 方案，严格遵守）

**核心原理**：按空格时创建透明覆盖层 `#dragOverlay`，截获所有鼠标事件；拖拽时用增量计算（每帧差值）更新滚动位置，而非从起点算偏移。

**三个关键点**：
1. **覆盖层**：`position:fixed; z-index:99999` 全屏透明 div，阻止浏览器干预
2. **增量计算**：`changeX = clientX - mouseX`，`scrollLeft -= changeX`，然后 `mouseX = clientX`（逐帧累积，不从起点算）
3. **事件绑定**：mousedown 绑在 overlay 上，mousemove/mouseup 绑在 document 上

### 必须包含的 JS 代码

在 `</script>` 标签前插入以下代码（根据实际页面结构调整滚动容器选择器）：

```javascript
// ===== Space + Drag 画布平移（Axure 方案） =====
(function() {
    var spaceDown = false;
    var dragging = false;
    var mouseX, mouseY;
    var mainArea, protoPanel, descPanel;

    document.addEventListener('keydown', function(e) {
        if (e.key !== ' ') return;
        var t = (e.target.tagName || '').toLowerCase();
        if (t === 'input' || t === 'textarea' || t === 'select') return;
        e.preventDefault();
        if (spaceDown) return;
        spaceDown = true;
        enableDragMode();
    });

    document.addEventListener('keyup', function(e) {
        if (e.key !== ' ') return;
        spaceDown = false;
        disableDragMode();
    });

    function enableDragMode() {
        mainArea = document.querySelector('.main-area');
        protoPanel = document.getElementById('protoPanel');
        descPanel = document.getElementById('descPanel');

        if (!document.getElementById('dragOverlay')) {
            var overlay = document.createElement('div');
            overlay.id = 'dragOverlay';
            overlay.style.cssText = 'position:fixed;top:0;left:0;width:100%;height:100%;z-index:99999;cursor:grab;';
            document.body.appendChild(overlay);
        }

        var overlay = document.getElementById('dragOverlay');
        overlay.addEventListener('mousedown', startDrag);
    }

    function disableDragMode() {
        if (dragging) stopDrag();
        var overlay = document.getElementById('dragOverlay');
        if (overlay) {
            overlay.removeEventListener('mousedown', startDrag);
            overlay.remove();
        }
    }

    function startDrag(e) {
        e.preventDefault();
        dragging = true;
        mouseX = e.clientX;
        mouseY = e.clientY;

        var overlay = document.getElementById('dragOverlay');
        if (overlay) overlay.style.cursor = 'grabbing';

        document.addEventListener('mousemove', onDrag);
        document.addEventListener('mouseup', stopDrag);
    }

    function onDrag(e) {
        if (!dragging) return;
        var changeX = e.clientX - mouseX;
        var changeY = e.clientY - mouseY;

        // 增量更新：每帧只移动差值
        mainArea.scrollLeft -= changeX;
        if (protoPanel) protoPanel.scrollTop -= changeY;
        if (descPanel) descPanel.scrollTop -= changeY;

        mouseX = e.clientX;
        mouseY = e.clientY;
    }

    function stopDrag() {
        dragging = false;
        var overlay = document.getElementById('dragOverlay');
        if (overlay) overlay.style.cursor = 'grab';
        document.removeEventListener('mousemove', onDrag);
        document.removeEventListener('mouseup', stopDrag);
    }

    window.addEventListener('blur', function() {
        spaceDown = false;
        disableDragMode();
    });
})();
```

### 适配说明

- 如果 PRD 布局不是标准三栏（无 `.main-area` / `#protoPanel` / `#descPanel`），需调整 `enableDragMode()` 中的选择器，指向实际的滚动容器
- 横向滚动容器用 `scrollLeft`，纵向滚动容器用 `scrollTop`
- 输入框/文本域内按空格不触发平移（已内置排除）

## 权限设计格式规范

PRD 中的权限设计**不使用"角色×功能"的完整矩阵表**，改为"需控权按钮清单"格式：

- 以表格列出所有需要权限控制的菜单/按钮
- 每行包含：所属页面、控权元素（用 mock 组件可视化）、建议权限标识（如 `crm:tag:group:create`）、说明
- 未列入清单的操作默认可见，无需权限控制
- 开发按清单在 UI 元素上做权限标识，上线后在角色权限模块中配置即可

## 颜色字段展示规范

表格中的颜色字段（如标签颜色列）**只展示色块（color-dot），不加颜色名称文字**：

```html
<td><span class="color-dot" style="background: #F5222D;"></span></td>
```

- 正确：只放色块圆点
- 错误：色块后面加"红色"等文字描述
- 颜色说明文档（如色值对照表）中可以保留文字描述

## 复杂嵌套组件 div 平衡验证

生成包含复杂嵌套结构（流程图、多层弹窗、分支布局等）的 PRD 后，**必须用脚本验证 div 开闭平衡**：

```bash
python3 -c "
import re
with open('FILE_PATH', 'r') as f:
    content = f.read()
opens = len(re.findall(r'<div[\s>]', content))
closes = len(re.findall(r'</div>', content))
print(f'opens={opens}, closes={closes}, diff={opens-closes}')
assert opens == closes, f'DIV MISMATCH: {opens} opens vs {closes} closes'
"
```

- 如果 diff 不为 0，说明有 div 未闭合或多闭合，会导致页面布局错乱（如右侧说明面板被吞入左侧原型区）
- 需逐层排查修复后才能交付

## 输出检查清单

生成后验证：
- [ ] 单文件独立 HTML，无外部依赖
- [ ] 顶部栏含项目名、版本、日期
- [ ] 左侧目录可导航、可收起
- [ ] 每个页面都有原型区 + 说明区
- [ ] 颜色标注体系正确（红/蓝/橙）
- [ ] Mock UI 组件正确使用
- [ ] hash 路由可正常切换页面
- [ ] 横向滚动正常，内容不被压缩
- [ ] 空格+拖拽平移画布功能正常（上下左右均可平移，松手不弹回）
- [ ] div 开闭平衡验证通过（opens == closes）
