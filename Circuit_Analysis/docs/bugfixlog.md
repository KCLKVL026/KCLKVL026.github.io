# Bug Fix Log - 2026-05-11

## 目录结构重构

### 目录重命名
- 为 `Circuit_Analysis/` 下 9 个子目录添加 `01_` ~ `09_` 数字前缀
- 同步修改 `index.html` 中全部 33 个链接路径

### 运放模块注释
- `index.html` 第 9 节"运算放大器"卡片区域整体转为 HTML 注释
- `09_OpAmp/` 子目录及文件保留，留待模拟电子学课程使用
- 页面模块总数从 33 更新为 30

---

## Wheatstone_Bridge.html（惠斯通电桥）

### 柱状图宽度
- 柱状图宽度调整为原来的 50%（`barPercentage: 0.45`）

### ASCII 电路图替换
- 删除"惠斯通电桥拓扑"窗口中的 ASCII 电路图
- 改为插入 `./img/Wheatstone_Bridge.jpg` 图片

### 布局调整
- 将"惠斯通电桥平衡原理"和"灵敏度与检流计"说明卡片从页面底部移入右侧栏
- 与"桥臂电压与检流计"和"平衡状态"组成同一行 2 列网格，宽度对齐

---

## Ohms_Law.html（欧姆定律）

### 电路图卡片
- 在"电路参数调节"和"曲线解读"之间插入电路图卡片
- 插入 `./img/Ohms_Law.jpg`，三张卡片宽度一致

---

## Series_Parallel_Resistors.html（电阻串并联）

### 电路图卡片
- 在柱状图下方增加电路图卡片，插入 `./img/Series_Parallel_Resistors.jpg`
- 图片两侧留有 `px-8` 内边距

### 刷新机制防抖（重点）
**问题**：改变参数时柱状图抖动严重，等效电阻公式不断重绘

**修复**：
1. 柱状图改用 `barChart.update('none')` 就地刷新，不再 destroy/recreate
2. 拆分 `updateUI()` 为两个函数：
   - `updateFullUI()`：模式切换时调用，重建所有 DOM 和 KaTeX
   - `updateValues()`：滑块变化时调用，只更新数值，不动公式和说明文字
3. 分压/分流公式和总电流文字拆为"静态公式 + 数值 span"结构，滑块变化只改 `innerText`
4. 柱状图合并为单个 dataset，柱子与 R1/R2 标签精确对齐
5. 柱子宽度缩至 60%（`barPercentage: 0.54`）

---

## RLC_Step_Response.html（RLC 二阶瞬态响应）

### 删除 s-平面极点图
- 移除 s-plane 极点位置图的 HTML、JS 函数和所有引用

### 响应指标布局
- "响应指标"卡片从左侧栏移至右侧波形图下方
- 改为 `grid-cols-3` 单行水平布局

### Y 轴范围扩展
- 波形图 y 轴负半轴从 -0.25 扩展到 -1.0，防止波形截断

### 临界阻尼按钮
- 在重置按钮旁新增"临界阻尼"按钮
- 点击后根据当前 L、C 计算 `R_crit = 2√(L/C)` 并设为 R 值

---

## Three_Phase_Load_Connection.html（三相负载星三角连接）

### 波形图布局
- Y 接法和 Δ 接法两个波形图改为上下排列（移除 `lg:grid-cols-2`）
- 移除 canvas 固定宽度 `width="400"`，图表自动撑满卡片宽度
- 容器高度从 340px 增至 380px

### 刷新机制防抖
- 波形图改用就地更新 + `chart.update('none')`，不再 destroy/recreate
- x/y 轴范围随参数动态调整，消除右侧空白

### 删除功率因数角滑块
- φ 滑块对电压波形无影响（仅影响电流相位，而图表只显示电压）
- 移除滑块 HTML、JS 变量和事件监听

### 重置参数按钮
- 新增重置按钮，恢复默认值：Vp=220V、f=50Hz、Z=50Ω

---

## Three_Phase_Unbalanced.html（三相不平衡负载）

### 波形图双 y 轴
**问题**：电流（几 A）和电压（~311V 峰值）共用同一 y 轴，电流波形被压成直线

**修复**：
- 左 y 轴显示电压（V），右 y 轴显示电流（A），各自独立缩放
- 电压轴灰色、电流轴紫色，与曲线颜色对应

### 波形图保留曲线精简
- 注释掉 V_B 和 V_C，只保留 V_A、I_A、I_N 三条曲线

### 相量图电流缩放
**问题**：电压（220V）与电流（几 A）在同一尺度下，电流相量只是原点附近的色点

**修复**：
- 定义 `I_SCALE = 50`，电流相量乘以 50 后绘制
- 标签仍显示实际电流值，轴标题注明"电流×50(A)"

### 相量图防抖
- 箭头绘制和标签逻辑提取为模块级插件 `phasorArrowPlugin`
- 首次创建图表，后续只更新坐标轴范围 + `chart.update('none')`

### 电流颜色修改
**问题**：电流箭头颜色与电压箭头太接近

**修复**：
| 相 | 旧颜色 | 新颜色 |
|----|--------|--------|
| I_A | 橙色 #f97316 | 洋红 #d946ef |
| I_B | 黄绿 #84cc16 | 粉红 #f472b6 |
| I_C | 青色 #06b6d4 | 琥珀 #f59e0b |

---

## Mutual_Inductance.html（互感耦合线圈）

### KaTeX 公式不渲染
**问题**：`setMode()` 用 `innerText` 写入含 `\\(...\\)` 的文本，KaTeX 从未渲染，公式显示为原始 LaTeX 文本

**修复**：
- `modeDesc.innerText` 改为 `modeDesc.innerHTML`
- 更新后调用 `renderMathInElement(modeDesc, ...)` 局部渲染

### T 型等效电路公式退化
**问题**：`updateTEquiv()` 每次用 `innerHTML` 重建 DOM，破坏了初始 KaTeX 渲染节点，且未重新渲染，公式退化为原始文本

**修复**：
- `updateTEquiv()` 末尾增加 `renderMathInElement(tEquivDisplay, ...)` 局部渲染

### 柱状图标签乱码
**问题**：柱状图 labels 使用了破损的 LaTeX 分隔符（`\(` 开头、`$` 结尾），显示为乱码

**修复**：
- 改为 Unicode 下标纯文本：`相助 (L₁+L₂+2M)` / `相消 (L₁+L₂-2M)`

### 图表动画抖动
- `waveformChart.update()` 和 `barChart.update()` 改为 `update('none')`

---

## Ideal_Transformer.html（理想变压器）

### 图表动画抖动
**问题**：`voltageChart.update()` 和 `currentChart.update()` 使用默认动画，滑块拖动时波形有过渡动画导致视觉抖动

**修复**：
- 改为 `update('none')` 禁用动画

---

## Thevenin_Norton.html 电路拓扑图替换

**文件**: `02_Circuit_Theorems/Thevenin_Norton.html`

**变更内容**:

1. **原电路拓扑卡片** — 删除 ASCII 电路拓扑图，替换为贴图 `./img/Thevenin_Norton_Origin.jpg`
2. **戴维南等效电路卡片** — 删除 ASCII 电路拓扑图，替换为贴图 `./img/Thevenin.jpg`
3. **诺顿等效电路卡片** — 删除 ASCII 电路拓扑图，替换为贴图 `./img/Norton.jpg`
4. **JS `switchMode` 函数** — 戴维南/诺顿模式切换时动态生成的 ASCII 图同步替换为对应贴图
5. 移除了等效电路图下方的公式文字说明（`equiv_diagram_formula`）

---

## Superposition.html（叠加定理）

### ASCII 电路图替换
- 删除三个电路示意图卡片中的 ASCII 电路拓扑图
- "U_s 单独作用"替换为 `./img/superposition-Us.jpg`
- "I_s 单独作用"替换为 `./img/superposition-Is.jpg`
- "完整电路 (叠加)"替换为 `./img/superposition-origin.jpg`
- 三张卡片下方各添加一行说明文字："ab之间的电压 \(U_{out1}\)"、"ab之间的电压 \(U_{out2}\)"、"ab之间的电压 \(U_{out}\)"

### 删除 R3 相关内容
- 移除 R3 滑条 HTML（参数调节卡片中的 \(R_3\) 滑条）
- 移除 JS 变量 `r3Slider`、`r3Display`
- 移除 `par3()` 辅助函数
- `getParams()`、`computeAll()`、`updateDisplay()` 中删除 R3 参数
- 移除 `r3Slider` 事件监听
- 更新理论公式：\(U_{out1}\) 和 \(U_{out2}\) 的公式中去掉 R3 引用

# Bug Fix Log

## 2026-05-14


### Max_Power_Transfer.html — 图表曲线不更新

**问题**：调整 V_th、R_th、R_L 滑条后，右上角功率曲线无任何变化。

**根因**：
1. `updateChart()` 的更新分支中 `chartInstance.options.plugins = chartInstance.options.plugins || {};` 覆盖了 Chart.js 内部插件配置，导致 `update()` 调用时无法正确重绘
2. `rthLine` 插件通过闭包捕获 `Rth`，图表首次创建后闭包变量不变，参考线不随滑块移动

**修复**：
- 删除 `chartInstance.options.plugins = ...`，保留插件配置完整
- `update()` 改为 `update('none')` 跳过动画，滑块拖动即时重绘
- `rthLine` 插件新增 `_rth` 属性存储当前 Rth 值
- 更新分支同步更新 `chartInstance.options.plugins.rthLine._rth = Rth`
- `afterDraw` 回调改为读取 `rthVal = chart.config.options.plugins.rthLine._rth`

### Max_Power_Transfer.html — 公式 KaTeX 分隔符缺失

**问题**：多处公式文本缺少 `\(` 和 `\)` 分隔符，KaTeX 不渲染，显示为纯文本。

**涉及位置**：
- "匹配特性"卡片的静态 HTML（`R_L`、`R_th` 等）
- `switchMode()` 函数中 DC/AC 模式的 `theory_text`、`character_text`
- `updateDisplay()` 中 `matchDisplay` 的匹配状态显示（DC 和 AC 模式）
- `updateDisplay()` 中 `rlPeakDisplay`（AC 模式的复共轭匹配公式）

**修复**：
- 以上所有位置的公式文本添加 `\(` 和 `\)` 分隔符
- `switchMode()` 和 `updateDisplay()` 末尾添加 `renderMathInElement()` 调用，确保动态插入的公式被 KaTeX 渲染


### Max_Power_Transfer.html — 图表 R_th 参考线不更新

**问题**：拉表窗口中修改 V_th、R_th、R_L 后，右上角的曲线没有任何改动。

**根因**：自定义 Chart.js 插件 `rthLine` 的 `afterDraw()` 回调通过闭包捕获了 `updateChart()` 函数中的 `Rth` 变量。图表首次创建时插件被注册，后续滑块变化只更新数据数组，插件不会被重建，闭包中的 `Rth` 始终保持初始值，导致绿色虚线参考线位置和标签永不更新。

**修复**：
- 将 Rth 值存储在 chart 实例的自定义属性 `chart.$rth` 上
- 插件 `afterDraw()` 改为读取 `chart.$rth` 而非闭包变量
- 每次更新时同步 `chartInstance.$rth = Rth`
- `chart.update()` 改为 `chart.update('none')` 避免动画干扰快速拖拽
- 移除无效代码 `chartInstance.options.plugins = ...`

---

### Max_Power_Transfer.html — 新增「匹配最大功率」按钮

**需求**：增加一键匹配最大功率的按钮，直流/交流模式均适用。

**实现**：
- 在 \(R_L\) 滑块与 AC 控件之间插入绿色按钮
- DC 模式：点击设 \(R_L = R_{th}\)
- AC 模式：点击设 \(R_L = R_{th}\)、\(X_L = -X_{th}\)（复共轭匹配）
- 按钮文字随模式切换（DC: \(R_L = R_{th}\) / AC: \(Z_L = Z_{th}^*\)）

---

### Max_Power_Transfer.html — 新增电路示意图

**需求**：在功率曲线窗口下方增加电路图窗口，宽度与功率曲线相同。

**实现**：在功率曲线卡片与理论卡片网格之间插入图片卡片，引用 `./img/Max_Power_Transfer.jpg`，使用与图表区一致的圆角阴影样式。

---

### Ideal_Transformer.html — 关系验证卡片布局调整

**需求**：将关系验证卡片移到电流波形图下方，宽度与电流波形图相同；三个验证公式改为一行排列。

**实现**：
- 从左侧面板移除验证卡片，移至右侧面板电流波形图与理论特性之间
- 卡片使用与波形图一致的 `rounded-2xl shadow-xl` 样式
- 三个公式改为 `flex justify-between gap-2` 横向一行排列，各占等宽

