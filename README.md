# beacon-lite-modern

> Java Maven 基础脚手架 — 拿来即用，开箱即用。

基于 **JDK 21** 的轻量级 Java 项目脚手架，预置日志组件、Lombok 支持、常用工具库，集成一键打包，克隆下来即可直接开始写业务代码。

---

## 🚀 快速开始

```bash
# 克隆项目
git clone <your-repo-url>
cd beacon-lite-modern

# 编译并打包（生成 fat JAR）
mvn clean package

# 直接运行
java -jar target/beacon-lite-modern.jar
```

---

## 📦 技术栈

| 组件 | 版本 | 说明 |
|---|---|---|
| **JDK** | 21 | 最低运行环境（LTS） |
| **Maven** | ≥3.6 | 构建工具 |
| **SLF4J** | 2.0.18 | 日志门面 |
| **Logback** | 1.5.34 | 日志实现（JDK 11+，适配 JDK 21） |
| **Lombok** | 1.18.46 | 简化样板代码 |
| **Hutool** | 5.8.46 | Java 工具类库 |

---

## 📝 日志说明

日志配置位于 `src/main/resources/logback.xml`，无需额外配置即可使用。

### 双模式输出

| 模式 | 输出目标 | 格式 |
|---|---|---|
| **控制台** | `System.out` | 带 ANSI 颜色高亮，适合开发调试 |
| **滚动文件** | `logs/beacon-lite-modern.log` | 纯文本，适合生产环境日志采集 |

### 日志滚动策略

- 按天滚动归档（`beacon-lite-modern.yyyy-MM-dd.log`）
- 保留 **30 天**
- 归档总大小上限 **3GB**，超限自动清理

### 日志级别

```xml
<!-- 项目包：开发期 DEBUG，发布后可调为 INFO -->
<logger name="com.beacon" level="DEBUG"/>

<!-- 第三方库：INFO -->
<root level="INFO">
    <appender-ref ref="CONSOLE"/>
    <appender-ref ref="FILE"/>
</root>
```

### 使用示例

```java
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class BeaconMain {
    public static void main(String[] args) {
        log.info("are you ok?");
        log.debug("调试信息");
        log.error("异常信息", new RuntimeException("示例异常"));
    }
}
```

通过 Lombok 的 `@Slf4j` 注解自动注入 `log` 对象，无需手动声明 Logger。

---

## 🔨 构建 & 打包

### 常用命令

```bash
# 清理 + 编译 + 打包
mvn clean package

# 跳过测试打包
mvn clean package -DskipTests

# 仅编译
mvn compile

# 运行主类（开发调试）
mvn exec:java -Dexec.mainClass="com.beacon.BeaconMain"
```

### Fat JAR 打包

项目使用 `maven-assembly-plugin`，打包后直接生成包含所有依赖的可执行 JAR：

```
target/beacon-lite-modern.jar   ← 可直接 java -jar 运行
```

JAR 已配置 `Main-Class` 清单，无需指定主类。

---

## 🗂 项目结构

```
beacon-lite-modern/
├── pom.xml                                # Maven 配置
├── README.md
├── .gitignore
│
├── src/
│   └── main/
│       ├── java/com/beacon/
│       │   └── BeaconMain.java            # 程序入口
│       └── resources/
│           └── logback.xml                # 日志配置
│
└── logs/                                  # 运行日志输出（已 gitignore）
```

---

## ⚙ 项目特性

- **JDK 21 LTS**：基于 Java 21 长期支持版本，可使用虚拟线程、Record、模式匹配等新特性
- **零配置启动**：克隆即跑，日志、打包全部预置
- **开发/生产双模式日志**：开发有颜色、生产纯文本，无缝切换
- **Maven Resource Filtering**：`logback.xml` 支持 Maven 属性占位符（如 `${project.build.finalName}`）
- **日志安全**：Logback 使用 JDK 11+ 最新安全版本（1.5.34），修复 CVE-2026-10532 等已知漏洞
- **实用工具库**：内置 Hutool，涵盖字符串、日期、集合、IO 等常用操作

---

## 🆚 与 JDK 8 版本的主要区别

| 项目 | beacon-lite-jdk8 | beacon-lite-modern |
|---|---|---|
| **JDK** | 8 | 21 (LTS) |
| **Logback** | 1.3.16（JDK 8 最后兼容版） | 1.5.34（JDK 11+） |
| **SLF4J** | 2.0.9 | 2.0.18 |
| **Lombok** | 1.18.34 | 1.18.46 |
| **虚拟线程** | ❌ | ✅ |
| **Record 类** | ❌ | ✅ |
| **模式匹配** | ❌ | ✅ |

---

## 📄 License

MIT
