# 错误处理模板

## Controller 层

```java
@PostMapping
public Result<UserDTO> create(@Valid @RequestBody UserDTO dto) {
    // 直接调用 service，异常由 GlobalExceptionHandler 统一处理
    return Result.success(userService.create(dto));
}
```

## Service 层 — 业务异常

```java
public UserDTO update(Long id, UserDTO dto) {
    User user = userMapper.selectById(id);
    if (user == null) {
        throw new BusinessException("用户不存在");
    }
    // 业务逻辑...
    return toDTO(user);
}
```

## GlobalExceptionHandler — 异常分类

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    // 1. 业务异常 (最具体)
    @ExceptionHandler(BusinessException.class)
    public Result<Void> handleBusinessException(BusinessException e) { ... }

    // 2. 参数校验异常
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public Result<Void> handleValidationException(...) { ... }

    // 3. 系统异常 (最通用，兜底)
    @ExceptionHandler(Exception.class)
    public Result<Void> handleException(Exception e) { ... }
}
```

## 注意事项

- 所有异常处理集中在一个 `@RestControllerAdvice` 类中
- 从具体到通用排列 handler 方法
- 不要在 Controller 中 try-catch，交给全局处理器
- 业务异常使用 `BusinessException`，不要抛 `RuntimeException`
