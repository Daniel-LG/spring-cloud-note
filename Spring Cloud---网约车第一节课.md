#### Spring Cloud Context

##### Bootstrap 上下文

Spring Cloud启动时创建，应用上下文的父级上下文。

负责从外部资源（Git等）加载配置，优先级高，不会被本地配置覆盖。

可以在bootstrap .yml中禁用Bootstrap引导

##### 加载顺序

- 启动命令中的配置项： Java - 参数
- 配置中心（Git）等配置文件
- 本地application.properties(yml)
- 本地bootstrap.properties(yml)

优先级递减