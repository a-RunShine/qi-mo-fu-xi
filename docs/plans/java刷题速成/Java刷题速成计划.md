# Java刷题速成计划

> 用户画像:有 C++ 刷题基础(STL 50% 熟、OOP 会、Java 与 C++ 差异点 OK),目标**速成**上手 Java 刷题,平台**LeetCode + 牛客(ACM 模式)双覆盖**。
> 文档定位:C++ → Java 刷题迁移指南,以**对照表 + 模板代码**为主,跳过工程向理论。
> 速成期望:目标「LeetCode 中等题通过率 70%+」,难题仍需补算法,**速成 ≠ 精通**。
> v2:经 explore 审查后优化,采纳 21 条建议(6 高 / 8 中 / 7 低)。主要变更:8→10 大算法(补回溯/贪心);10→14 大坑(补 String==/数组长度语法/默认初始化/栈深度);02 清理杂货铺;03 对照表加 API 映射列;阶段三去 PDF(用户已说暂不生成)。

## 一、目标与原则

### 核心目标
2-3 天看完即可上手 Java 刷题,重点解决三件事:
1. **输入输出**:Scanner / BufferedReader / StreamTokenizer / StringTokenizer / PrintWriter 选型,知道 ACM 模式怎么写(`throws IOException` 必会)
2. **容器 API 替换**:把 `vector/map/set/queue/priority_queue` 映射到 `ArrayList/HashMap/HashSet/ArrayDeque/PriorityQueue`
3. **刷题模板**:10 大算法(排序/二分/双指针/BFS-DFS/DP/并查集/单调栈/前缀和/**回溯/贪心**)+ 常用技巧(HashMap+前缀和、位运算)直接抄

### 三条原则
1. **对照优先**:所有 Java 概念都贴 C++ 写法,降低迁移成本
2. **代码为王**:每个 API 至少一段可直接抄的最小代码
3. **坑点先行**:常见 15 大坑前置到第 6 章,避免重复踩雷

## 二、目录与产物

```
Java刷题速成/
├── 00_总览与速成路线.md          # 路线图 + C++→Java 总览对照 + 速查表使用指南
├── 01_输入输出篇.md              # Scanner / BufferedReader / StreamTokenizer / StringTokenizer / PrintWriter
├── 02_语法差异速览.md            # 纯语法:引用/字符串/数组/final/static/类与对象/char vs String
├── 03_容器与STL对照.md           # 12 个容器对照表(深浅两档,带 API 映射)
├── 04_常用工具类.md              # Arrays/Collections/Math/String/StringBuilder + Java 8 必备 API
├── 05_刷题模式与模板.md          # ACM + LeetCode 模式 + 10 大算法模板 + 常用技巧
├── 06_常见坑点速查.md            # 15 大经典坑
├── 07_实战例题.md                # 7 道典型题完整流程(含字符串)
└── 速查表.md                     # 单页 CheatSheet
```

## 三、各篇核心内容

### 00_总览与速成路线
- **速成期望**:**LeetCode 中等题通过率 70%+**,难题仍需补算法,速成 ≠ 精通
- 一张总览表:C++ 知识点 → Java 对应位置 → 文档引用
- 推荐阅读顺序:
  - **2-3 天速成**(零基础):00 → 01 → 02 → 03 → 04 → 05 → 06 → 07
  - **1 天速成**(有 STL 底子):00 → 03 → 05(只看 4 段骨架)→ 速查表
- **速查表使用指南**:
  - 用法 1:做题中遇到不熟的 API → 翻速查表查 API 名称
  - 用法 2:读完全部 8 篇后,速查表作为"考前总复习"一页过
  - 速查表在阶段二末尾(随 05/06/07 写完生成)

### 01_输入输出篇(重点)
- **Scanner 家族**:`nextInt/next/nextLine` 区别 + 换行符陷阱(`nextInt` 不会读走换行符)
- **BufferedReader + StreamTokenizer**:推荐,比 Scanner 快 5-10 倍
- **BufferedReader + StringTokenizer**:比 StreamTokenizer 简单且够用,很多题解用这个
- **PrintWriter / BufferedWriter**:大输出必须用,`System.out` 直接 println 会 TLE(牛客第一坑)
- **输出**:`print/println/printf` + `StringBuilder` 拼 + `PrintWriter.flush()`
- **异常声明**:`main` 函数必须 `throws IOException`,否则 `BufferedReader.readLine()` 编译报错(Java 特有,无 C++ 对应)
- 模板代码(全部配最小可跑示例):
  - **ACM 模式完整骨架 v2**:`import java.util.*; import java.io.*;` + `public static void main(String[] args) throws IOException`
  - 单组输入
  - 多组 T 输入(while + 哨兵)
  - 一行 N 个数(StreamTokenizer 版 + StringTokenizer + parseInt 版,两套都列)
  - N 行字符串(用 `StringBuilder` 拼 + `PrintWriter` 一次性 flush)
  - 链表输入(LeetCode 常用,无显式输入)
  - 二叉树层序输入(LeetCode 常用)

### 02_语法差异速览
只讲刷题**纯语法**差异(装箱/lambda 迁出到 03/04):
- **引用 vs C++ 指针**(`Person p = new Person()`)
- **字符串不可变** → 改用 `StringBuilder` / `char[]`(API 列表放 04,这里只讲"为什么不可变")
- **数组定长 vs `vector`**(`int[] a` vs `ArrayList<Integer>` 的选型判断)
- **`final` 关键字**(`final int[] a` 引用不可变,内容可变)
- **`static` 关键字**(类共享,`static ListNode head` 刷题常见)
- **类与对象**(对比 `struct` / `class`,**最小示例**:`class ListNode { int val; ListNode next; }` ——刷题 80% 都要写链表节点类)
- **接口**(`Comparable` / `Comparator`,只讲"是什么",写法放 04)
- **`char` vs `String`**(`'a'` 是 char,`"a"` 是 String,`String.charAt(i)` 取字符,`'9' - '0'` 转数字)
- **异常处理最小化**:只讲 `throws IOException`(01 模板里要用),不展开 try-catch-finally
- **for-each 增强 for 循环**

> **迁出说明**:
> - **装箱/拆箱 + Integer 缓存池**:迁到 03 `ArrayList<Integer>` 必看小框(因为主要在容器 API 涉及)
> - **lambda**:迁到 04 Comparator 小节(因为是 `PriorityQueue` / `sort` 比较器的工具)

### 03_容器与STL对照 ← 核心
对照表形式,一眼能查(5 列):

| C++ STL | Java | 关键差异 | 常用 API 映射 | 深度 |
|---|---|---|---|---|
| `vector` | `ArrayList<E>` | 只能装对象,基本类型用 `Integer` | `push_back→add` / `size→size` / `empty→isEmpty` / `[]→get/set` | 深 |
| `list` | `LinkedList<E>` | 也可当队列/栈,但慢,推荐 `ArrayDeque` | `push_back→add` / `push_front→addFirst` / `front→peek` | 中 |
| `stack` | **`ArrayDeque<E>` 当栈用** | `java.util.Stack` 性能差且过时,**别用** | `push→push` / `top→peek` / `pop→pop` | 深 |
| `queue` | `ArrayDeque<E>` / `LinkedList` | 默认 `ArrayDeque` | `push→offer` / `front→peek` / `pop→poll` | 深 |
| `priority_queue` | `PriorityQueue<E>` | **默认小顶堆**(C++ 默认大顶堆),注意! | `push→offer` / `top→peek` / `pop→poll` | 深 |
| `deque` | `ArrayDeque<E>` | 滑动窗口标配 | `push_back→addLast` / `push_front→addFirst` / `[]→peekFirst/peekLast` | 深 |
| `unordered_map` | `HashMap<K,V>` | 允许 null key/value | `[]→get/put` / `find→containsKey` / `count→size` | 深 |
| `unordered_set` | `HashSet<E>` | 基于 `HashMap` | `insert→add` / `find→contains` / `count→size` | 中 |
| `map` | `TreeMap<K,V>` | 红黑树,有序 | `[]→get/put` / `lower_bound→lowerKey` / `upper_bound→higherKey` | 简 |
| `set` | `TreeSet<E>` | 红黑树,有序 | `insert→add` / `find→contains` / `lower_bound→lower` | 简 |
| `pair` | `int[]` / `Map.Entry` / 自定义类 | Java 无内建 pair,见下方 | 见下方 | 简 |
| `sort` | `Arrays.sort` / `Collections.sort` | | `sort→sort` / `stable_sort→Arrays.sort(对象)` | 中 |

**`pair` 展开**:
- `pair<T1,T2>` → `int[]` (2 元素时,如 `new int[]{a, b}`) / `Map.Entry<K,V>` / 自定义类(3+ 字段时,如 `class Edge { int u, v, w; }`)
- `vector<pair<int,int>>` → `int[][]`(最常用,性能好)/ `List<int[]>`
- 配最小代码示例

每条深讲条目配:**API 列表 + 最小代码 + 1-2 个坑点**。
简讲条目配:**一行对照 + 一行代码**。

> **装箱小框**(从 02 迁入):`Integer a = 5; int b = a;` 自动装箱/拆箱,`ArrayList<Integer>` 必须用包装类。**`Integer` 缓存池陷阱**:`Integer a = 100; Integer b = 100; a == b` 是 true(在 -128~127 内),但 `a = 200; b = 200; a == b` 是 false(超出范围)。**永远用 `equals`**。

### 04_常用工具类
- `Arrays`:`sort` / `binarySearch` / `fill` / `toString` / `asList` / `stream` / `copyOf` / `equals`
- `Collections`:`sort` / `reverse` / `max` / `min` / `frequency` / `swap` / `binarySearch`
- `Math`:`max/min/abs/pow/sqrt/log` / 三角函数 / `PI/E`
- `String`:`split/charAt/indexOf/substring/equals/hashCode` 陷阱
- `StringBuilder`:`append/reverse/toString/deleteCharAt/setCharAt`
- `Integer/Long/Double`:`parseInt/valueOf/bitCount/MAX_VALUE/MIN_VALUE`
- `Comparator` 写法(从 02 迁入):匿名内部类 / lambda / `Comparator.comparingInt` / `.reversed` / `.thenComparing` 链式调用
  > 本节讲"写法全集",在 05 排序和堆里"怎么用"另讲
- **Java 8 必备 API(深讲)**:
  - `Map.getOrDefault(key, defaultValue)` —— LeetCode 1 两数之和、347 前 K 高频**天天用**
  - `Map.computeIfAbsent(key, k -> new ArrayList<>())` —— BFS/图遍历建邻接表标准写法
  - `List.sort(Comparator)`(Java 8 新增,`Collections.sort` 的替代,更快)
  - `String.join(separator, elements)` —— LeetCode 打印列表标准写法
  - `String.chars() / String.codePoints()` —— 字符流式处理(了解即可)

### 05_刷题模式与模板
- **ACM 模式完整骨架**(与 01 模板对齐,引用 01 的 v2 骨架):
  - 单组 / 多组 T / 哨兵 0
  - `throws IOException` 异常处理模板
  - `PrintWriter` 输出模板
- **LeetCode 模式注意点**:
  - 类结构、import、`main` 不写
  - 自定义 `Comparator` 用 `PriorityQueue` 时
  - `Arrays.sort` 与 `Collections.sort` 区别
- **10 大算法 Java 模板**:
  1. **排序**:快排 / 归并 / 堆排
  2. **二分**:标准 / 找左边界 / 找右边界
  3. **双指针**:对撞 / 快慢 / 滑动窗口
  4. **BFS/DFS**:`ArrayDeque` 队列 / 递归 / 记忆化
  5. **DP**:一维 / 二维 / 0-1 背包 / 完全背包
  6. **并查集**:`int[] parent` + 路径压缩 + 合并
  7. **单调栈 / 单调队列**:`ArrayDeque`
  8. **前缀和 / 差分**
  9. **回溯**(新增):模板骨架(`result / path / backtrack(选择列表)` 三件套) + 经典例题(LeetCode 46 全排列 / 78 子集) + 剪枝技巧(`start` 索引去重、`used[]` 数组、排序后剪枝)
  10. **贪心**(新增):模板要点(局部最优→全局最优的证明思路,不给完整证明) + 经典例题(LeetCode 55 跳跃游戏 / 435 无重叠区间) + 排序+贪心标准模式
- **常用技巧(新增)**:
  - **HashMap + 前缀和**:解决「和为 K 的子数组」「两数之和」类问题(配合 04 `getOrDefault`)
  - **位运算技巧**:`n & (n-1)` 去最低位 1 / `n & (-n)` 提取最低位 1 / 异或找唯一或缺失 / Lowbit
  - **Trie(★★★进阶,速成可不学)**:在 05 提一句,后续需补再展开

### 06_常见坑点速查
Java 刷题 15 大经典坑(每条配:坑点 + 错误示例 + 正确写法 + 原因):
1. `Stack` 用 `ArrayDeque` 替
2. `PriorityQueue` 默认小顶堆
3. `Integer a == b` 在 128 内才相等(永远用 `equals`)
4. `for (E x : list) { list.remove(x); }` 抛 `ConcurrentModificationException` → 用迭代器
5. `Arrays.asList` 返回定长 list,不能 `add`/`remove`
6. `String` 拼接 → 用 `StringBuilder`
7. 浮点精度 → `BigDecimal` 或 eps 比较
8. 数组排序 + 自定义比较器 `Comparator`(尤其 `int[]` 二维)
9. `int[]` vs `Integer[]` 在 `Arrays.asList` 行为不同
10. `HashMap` 多线程 → 单线程够用,但要知道 `ConcurrentHashMap`
11. **【新增】`String` 比较用 `==` 还是 `equals`**:`String s1 = "abc"; String s2 = new String("abc"); s1 == s2` 是 false(比的是引用),C++ 选手第 1 坑
12. **【新增】数组长度 / 字符串长度 / 集合大小语法不同**:`arr.length`(无括号)/ `s.length()`(有括号)/ `list.size()`(有括号)——C++ 全靠 `arr.size()`
13. **【新增】Java 数组默认初始化**:`int[] a = new int[5]` 默认全 0,`boolean[]` 全 false,`String[]` 全 `null`(C++ 是垃圾值,行为完全不同)
14. **【新增】递归栈深度限制**(`StackOverflowError`):Java 默认栈远小于 C++,二叉树/链表递归深一点就爆栈,必须前置提醒
15. **【新增】ACM 模式 `main` 函数必须 `throws IOException`**(与 C++ 完全不同,Java 特有语法陷阱)

### 07_实战例题
7 道题,展示**从输入到提交**完整流程(每题注明 LeetCode / 牛客):
1. **A+B**(基础 I/O + ACM 模式 + 多组输入,牛客)
2. **字符串反转 / 最长公共前缀**(`StringBuilder.reverse()` / `String.charAt(i)` 等字符串操作,LeetCode 344 / 14)——补的字符串题
3. **链表反转**(指针操作 + 哑节点,LeetCode 206)
4. **二叉树层序遍历**(`ArrayDeque` + BFS,LeetCode 102)
5. **两数之和**(`HashMap` 单遍遍历,LeetCode 1)
6. **滑动窗口最大值**(`ArrayDeque` 单调队列,LeetCode 239)
7. **跳跃游戏**(贪心,LeetCode 55)——配 05 贪心模板

每题配:**题目 → 思路 → Java 完整代码 → C++ 对照写法 → 提交要点**(LeetCode/牛客分别)。

### 速查表.md
单页 CheatSheet,可独立打印(在阶段二末尾生成):
- 输入输出 3 套模板(ACM 单组 / ACM 多组 / LeetCode)
- 容器 API 一行对照(12 个,带关键 API 映射)
- 常用代码片段:排序 / 二分 / BFS / DP / 并查集 / 单调栈 / 回溯 / 贪心
- 关键字速查:`final` / `static` / `this` / `super` / `@Override`
- 14 大坑点一句话提醒(精简为一行一坑,挑 14 个最核心,`throws IOException` 放 01 提)

## 四、风格规范

- **语言**:中文行文,口语化,**所有思考与输出使用中文**
- **结构**:
  - 顶部 `> 说明` 块描述本章目标
  - 知识块用 `## / ###` 分级
  - C++ ↔ Java 用双栏对比或对照表
  - 代码块占 30%+ 内容
  - 章节末尾留"练习建议"块(指向 LeetCode/牛客具体题目)
- **符号**:
  - ★ 标记高频必会
  - 不用 ⭐✓✗⊂ 等 Unicode(PDF 兼容)
  - 错误用 `错误`,正确用 `正确`
- **代码**:
  - 全部能编译通过(语法层面验证)
  - 类名 `Main`(ACM)/ `Solution`(LeetCode)
  - 关键 API 旁注中文释义

## 五、推进节奏

### 阶段一:试看三篇(当前)
> **为什么选这 3 篇试看**:它们覆盖了文档的三种典型形态——**00 是路线图(导航型)**、**01 是教程(API 讲解型)**、**03 是对照表(查询型)**。试看这 3 篇,可以一次性确认文档的**导航风格、讲解深度、表格密度**三种核心风格,避免后续批量返工。
- 00_总览与速成路线.md
- 01_输入输出篇.md
- 03_容器与STL对照.md
- 交付后**风格确认清单**(用户回答以下 5 个 yes/no 后再继续):
  1. 表格列数(03 用了 5 列)是否够用?还是太密?
  2. 代码注释风格(关键 API 旁注中文释义)是否合适?
  3. 章节深度比例(深讲占 60%、简讲占 40%)是否合理?
  4. 坑点写法(错误示例 + 正确写法 + 原因)是否清楚?
  5. 对照表密度(每章至少 1 个对照表)是否够?还是太密?

### 阶段二:补完剩余 5 篇 + 速查表
- 02_语法差异速览.md
- 04_常用工具类.md
- 05_刷题模式与模板.md
- 06_常见坑点速查.md
- 07_实战例题.md
- **速查表.md**(随 05/06/07 写完立即生成,阶段二末尾)
- **整体校对**:术语一致、链接交叉引用(00 ↔ 各章)正确

### 阶段三:整体校对(用户确认)
- 整体文档串读
- 修复试看与批量间的术语/链接不一致
- 收集用户反馈,二轮修订

## 六、验证清单

每篇 .md 完成后:
- [ ] 标题、目录、说明块齐全
- [ ] 代码块语法正确(Java 高亮,小段 C++ 高亮)
- [ ] C++ ↔ Java 对照完整
- [ ] 没有 ⭐✓✗⊂ 等 PDF 不兼容符号
- [ ] 中文标点统一(全角逗号、句号)
- [ ] **交叉引用 00 ↔ 各章** 正确(00 指向 01-07,07 引用 03/05 的 API)
- [ ] **术语一致**(优先队列/堆/PriorityQueue 统一,二叉树/树统一)
- [ ] 章节末尾"练习建议"块(指向 LeetCode/牛客具体题目)

阶段一交付后追加(用户回答):
- [ ] **风格确认 5 题**(表格列数/代码注释/深度比例/坑点写法/对照表密度)全部通过

阶段二完成后追加:
- [ ] 速查表内容与正文一致(坑点对齐 06,容器 12 个对齐 03,算法 10 个对齐 05)
- [ ] 7 道例题全部有 LeetCode 题号,可点击跳转
- [ ] 整体串读无错别字

## 七、风险与依赖

| 风险 | 影响 | 应对 |
|---|---|---|
| Java 17/21 新特性(records/var)与 Java 8 冲突 | API 文档不一致 | 全文统一 Java 8 兼容语法,新特性标注"Java X+" |
| 容器 API 误用 | 编译/运行错误 | 重点条目配**最小可跑示例**自检 |
| **LeetCode 模式 vs ACM 模式混淆** | 用户刷题时切换平台出错 | 01 显式分两节讲、07 每题注明是 LeetCode 还是牛客 |
| **用户期望过高(速成 ≠ 精通)** | 文档无法满足 | 00 开头明确"速成目标=LeetCode 中等题通过率 70%+,难题仍需补算法" |
| 用户风格不接受 | 重写成本 | 先试看三篇,确认后再批量 |
| C++ 对照写错 | 误导用户 | 对照表由 C++ 经验辅助 + 用户 review 试看篇 |
| 未来 PDF 生成的字体兼容 | 跨平台用户无法阅读 | macOS 字体路径硬编码,Windows/Linux 需替换字体,待用户提出时再处理 |

## 八、不做清单(明确边界)

- ❌ Java 工程向(Spring / 反射 / 注解 / 多线程深入)
- ❌ 完整 Java 语法(只讲刷题会碰到的)
- ❌ 算法详解(模板给到,原理点到为止)
- ❌ IDE 配置(假定用户会用 VSCode/IDEA)
- ❌ 单元测试 / 调试技巧(刷题场景不必要)
- ❌ Java 8 之后的新特性深入(Stream 深入、record、sealed、var 等只提一句不展开)
- ❌ JVM / 垃圾回收 / 内存模型(刷题用不到)
- ❌ NIO / 并发包深入(刷题用 `ArrayDeque` 即可)
- ❌ 泛型擦除 / 协变逆变 / 通配符细节(仅 `ArrayList<Integer>` 这种简单泛型)
- ❌ 模块系统 / JPMS

## 九、参考资料

- 工作区技能: `.opencode/skills/md-pdf/`
- MD 转 PDF 脚本: `.opencode/skills/md-pdf/scripts/convert_md_to_pdf.py`(备用,排版确定后再用)
- 字体路径(未来 PDF 用):`/System/Library/Fonts/STHeiti Medium.ttc`
- Java 8 API 文档:`https://docs.oracle.com/javase/8/docs/api/`
- LeetCode 官方题解:刷题时遇到不熟 API 优先查
- 牛客 ACM 模式输入输出题解:参考典型 I/O 模板

## 十、版本与变更记录

| 版本 | 日期 | 变更 | 触发 |
|---|---|---|---|
| v1 | 初版 | 8 篇 .md 草案 + 速查表 + PDF 计划 | 用户首版需求 |
| v2 | 审查后 | 采纳 explore 21 条建议:8→10 大算法、10→14 大坑、02 清理杂货铺、03 对照表加 API 映射、阶段三去 PDF、改命名(03/06 中文化) | explore 审查报告 |
