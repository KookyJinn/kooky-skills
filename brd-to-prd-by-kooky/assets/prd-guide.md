# PRD 生成参考指南

当 BRD 确认后需要生成 PRD 时，读取此文件和 `assets/prd-template.html` 模板，基于 BRD 功能清单生成完整 PRD。

## PRD 文件信息

- **文件命名**：`{项目名}——{版本号}_{需求名称}.html`
- **版本**：从 BRD 中提取或询问用户
- **读取模板**：`assets/prd-template.html`

## 布局规范

```
┌─────────────────────────────────────────────────┐
│ 顶部栏 (48px, 深色背景)                           │
├────────┬───────────────────┬────────────────────┤
│ 目录栏  │   原型区           │   说明区            │
│ 240px  │   不压缩            │   不压缩             │
│ 可收起  │   min-width:1400px │   min-width:600px  │
└────────┴───────────────────┴────────────────────┘
```

- 无限画布：原型区和说明区按内容自然撑开，超出时横向滚动
- 目录栏：240px 固定，支持收起展开

## 配色体系

| 用途 | 颜色 | CSS class |
|------|------|-----------|
| 关键条件 | 深红 #9B0000 | `.c-key` |
| 警告/重要 | 红色 #CA0606 | `.c-warn` |
| 补充说明 | 蓝色 #0076FF | `.c-note` |
| 提示 | 橙色 #D46B08 | `.c-hint` |
| 成功 | 绿色 #2E7D32 | `.c-green` |

## 说明框

- `.note-box`：浅黄提示框
- `.info-box`：浅蓝信息框
- `.danger-box`：浅红警告框

## Mock UI 组件

模板内置组件，直接使用：

| 组件 | class |
|------|-------|
| 按钮 | `.mock-btn-primary/default/danger` |
| 输入框 | `.mock-input` |
| 下拉框 | `.mock-select` |
| 表格 | `.mock-table` |
| 复选框 | `.mock-checkbox` / `.mock-checkbox.checked` |
| 标签 | `.mock-tag-blue/green/orange` |
| 链接 | `.mock-link` |
| 弹窗 | `.mock-modal` |
| 流程节点 | `.flow-node` / `.flow-arrow` |
| 标注 | `.annotation-marker` / `.highlight-box` |
| 分页 | `.mock-pagination` |

## 原型绘制原则

1. 尽量还原真实系统 UI：用 mock 组件模拟
2. 用标注突出改动点：`<span class="annotation-marker">新增</span>` + `.highlight-box`
3. 复杂交互用流程图
4. 弹窗用 `.mock-modal`

## 流程图规范

- 正交路由：连接线用水平+垂直组合（SVG `<path>` 的 H/V 命令），不用斜线
- 泳道图纵向布局优先（每列一个泳道，从上到下流动）
- 复杂图表用 SVG 而非 CSS

## 需求说明写作

- 用编号子标题：1.1、1.2、2.1...
- 关键条件用 `.c-key`，警告用 `.c-warn`
- 补充说明放 `.info-box` 或 `.note-box`

## 目录导航

```html
<div class="nav-group">
    <div class="nav-group-title" onclick="toggleGroup(this)">
        <span class="arrow open">▶</span> 版本名称
    </div>
    <div class="nav-group-items">
        <div class="nav-item active" onclick="switchPage('page-xxx', this)">
            <span class="dot"></span> 页面名称
            <span class="badge">新</span>
        </div>
    </div>
</div>
```

## 权限设计格式

不使用"角色×功能"矩阵表，改为"需控权按钮清单"：
- 表格列出需权限控制的菜单/按钮
- 每行：所属页面、控权元素、建议权限标识（如 `crm:tag:group:create`）、说明
- 未列入清单的操作默认可见

## 颜色字段

表格中颜色字段只展示色块：`<span class="color-dot" style="background: #F5222D;"></span>`，不加文字描述。

## 列表规范

- 子字段纵向排列在单个单元格内，不横向平铺
- 新增列作为独立新列，不修改现有列用途
- 修改列表前先确认现有列的业务规则

## 空格拖拽平移 JS

生成 PRD 后必须在 `</script>` 前插入以下代码（调整选择器匹配实际布局）：

```javascript
(function() {
    var spaceDown = false, dragging = false, mouseX, mouseY;
    var mainArea, protoPanel, descPanel;
    document.addEventListener('keydown', function(e) {
        if (e.key !== ' ') return;
        var t = (e.target.tagName || '').toLowerCase();
        if (t === 'input' || t === 'textarea' || t === 'select') return;
        e.preventDefault();
        if (spaceDown) return;
        spaceDown = true;
        mainArea = document.querySelector('.main-area');
        protoPanel = document.getElementById('protoPanel');
        descPanel = document.getElementById('descPanel');
        if (!document.getElementById('dragOverlay')) {
            var o = document.createElement('div');
            o.id = 'dragOverlay';
            o.style.cssText = 'position:fixed;top:0;left:0;width:100%;height:100%;z-index:99999;cursor:grab;';
            document.body.appendChild(o);
        }
        document.getElementById('dragOverlay').addEventListener('mousedown', function(e) {
            e.preventDefault(); dragging = true;
            mouseX = e.clientX; mouseY = e.clientY;
            document.getElementById('dragOverlay').style.cursor = 'grabbing';
            document.addEventListener('mousemove', function(e) {
                if (!dragging) return;
                mainArea.scrollLeft -= (e.clientX - mouseX);
                if (protoPanel) protoPanel.scrollTop -= (e.clientY - mouseY);
                if (descPanel) descPanel.scrollTop -= (e.clientY - mouseY);
                mouseX = e.clientX; mouseY = e.clientY;
            });
            document.addEventListener('mouseup', function() {
                dragging = false;
                document.getElementById('dragOverlay').style.cursor = 'grab';
            });
        });
    });
    document.addEventListener('keyup', function(e) {
        if (e.key !== ' ') return;
        spaceDown = false;
        var o = document.getElementById('dragOverlay');
        if (o) o.remove();
    });
    window.addEventListener('blur', function() {
        spaceDown = false;
        var o = document.getElementById('dragOverlay');
        if (o) o.remove();
    });
})();
```

## div 平衡验证

生成后必须验证：

```bash
python3 -c "
import re
with open('FILE_PATH', 'r') as f:
    content = f.read()
opens = len(re.findall(r'<div[\s>]', content))
closes = len(re.findall(r'</div>', content))
print(f'opens={opens}, closes={closes}, diff={opens-closes}')
assert opens == closes, f'DIV MISMATCH'
"
```

## 输出检查清单

- [ ] 单文件独立 HTML，无外部依赖
- [ ] 顶部栏含项目名、版本、日期
- [ ] 左侧目录可导航、可收起
- [ ] 每个页面都有原型区 + 说明区
- [ ] 颜色标注体系正确
- [ ] Mock UI 组件正确使用
- [ ] 空格+拖拽平移画布功能正常
- [ ] div 开闭平衡验证通过
