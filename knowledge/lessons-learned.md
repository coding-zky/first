# 经验教训

## 构建相关

### Lombok 与 Java 25 不兼容
- **问题**: Lombok 注解处理器在 Java 25 下编译失败（@Data, @Slf4j 等全部不生效）
- **解决**: 移除 Lombok 依赖，所有 POJO 手写 getter/setter/构造函数
- **教训**: 脚手架级别的项目应优先保证编译稳定性，避免对编译期注解处理器过度依赖

### MyBatis-Plus 版本兼容
- **问题**: MyBatis-Plus 3.5.9 找不到 `PaginationInnerInterceptor` 类
- **解决**: 降级到 3.5.5 稳定版本
- **教训**: ORM 框架选择 LTS/稳定版本，避免最新版 API 变动

## 架构相关

### 全局异常处理器冲突
- **问题**: 多个 `@RestControllerAdvice` 类存在时，异常处理顺序不确定，导致验证异常被通用 Exception handler 截获
- **解决**: 将所有异常处理合并到单一 `GlobalExceptionHandler` 类
- **教训**: 全局异常处理器应该集中在一个类中，按从具体到通用的顺序排列

### Axios 响应类型不匹配
- **问题**: 响应拦截器返回 `res.data`（解包后），但 TypeScript 类型仍是 `AxiosResponse<T>`
- **解决**: 创建 typed `Request` 接口，声明 `get<T>(): Promise<T>` 等方法
- **教训**: 拦截器修改响应结构后，必须同步更新类型声明

## 开发体验

### 子模块 Maven 构建
- **问题**: `mvn spring-boot:run -pl api` 找不到 service/dao/common 模块
- **解决**: 先执行 `mvn install -DskipTests` 安装子模块到本地仓库
- **教训**: 多模块项目启动前必须先 install，README 中需要明确此步骤
