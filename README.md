# Go-Tools

Go-Tools 是一个综合性的 Go 语言工具集合，提供了多种常用组件和工具，帮助开发者快速构建高质量的应用程序。

## 主要组件

### Kafka 工具包

基于 [IBM/sarama](https://github.com/IBM/sarama) 实现的 Kafka 客户端封装，提供简单易用的生产者和消费者 API。

#### 主要特性

- 简化的 API 设计，易于使用
- 支持同步生产者
- 支持消费者组
- 支持 TLS 和 SASL 认证
- 支持消息头和自定义分区
- 支持函数选项模式配置
- 完善的错误处理和上下文支持

#### 使用示例

```go
// 创建生产者
producer, err := kafka.NewProducer(
    kafka.WithBrokers("localhost:9092"),
    kafka.WithClientID("my-producer"),
    kafka.WithProducerRequireACK(true),
)

// 发送消息
err = producer.Send(ctx, "example-topic", []byte("key"), []byte("value"))

// 创建消费者
consumer, err := kafka.NewConsumer(
    kafka.WithBrokers("localhost:9092"),
    kafka.WithClientID("my-consumer"),
    kafka.WithConsumerGroupID("my-group"),
)

// 订阅主题
handler := func(ctx context.Context, msg *kafka.ConsumerMessage) error {
    fmt.Printf("Received: %s\n", string(msg.Value))
    return nil
}
err = consumer.Subscribe(ctx, []string{"example-topic"}, handler)
```

### Logger 日志工具包

基于 [go.uber.org/zap](https://github.com/uber-go/zap) 实现的日志系统，提供灵活、可扩展的日志记录功能。

#### 主要特性

1. 支持多级别日志：debug、info、warn、error、panic、fatal
2. 支持多种输出适配器：控制台、文件、Kafka 和 Elasticsearch 等
3. 支持结构化日志输出，便于与各种日志分析工具集成
4. 文件日志按日自动切换，按月归档
5. 分布式系统支持（节点 ID、模块名称和 IP 字段）
6. 支持错误详情记录，包括堆栈跟踪
7. 灵活的配置方式（结构体配置和函数选项模式）

#### 使用示例

```go
// 初始化日志
err := logger.InitWithOptions(
    logger.WithLevel("info"),
    logger.WithPath("./logs"),
    logger.WithNodeID("node-001"),
    logger.WithModule("api-server"),
    logger.WithBothOutput(),  // 同时输出到文件和终端
)

// 使用全局日志函数
logger.Info("应用启动成功")
logger.Infof("服务监听端口: %d", 8080)
logger.Error("发生错误", err)

// 创建模块专用日志
moduleLogger, err := logger.NewWithOptions(
    logger.WithLevel("debug"),
    logger.WithModule("auth-service"),
    logger.WithFileOutput(),  // 仅输出到文件
)
moduleLogger.Debug("认证模块初始化")
```

## 使用方法

### Kafka 工具包

```go
import "github.com/qishenonly/kafka"
```

详细使用方法请参考 [Kafka 工具包文档](./kafka/README.md)。

### Logger 日志工具包

```go
import "github.com/qishenonly/logger"
```

详细使用方法请参考 [Logger 工具包文档](./logger/README.md)。

## 示例

完整的示例代码可以在各个组件的 `examples` 目录下找到：

- Kafka 示例: [kafka/examples](./kafka/examples)
- Logger 示例: [logger/example](./logger/example)

## 特性

- 统一的错误处理机制
- 完善的文档和示例
- 高性能设计
- 完整的测试覆盖
- 支持分布式环境

## 贡献

欢迎提交 Issue 和 Pull Request 来帮助改进这个项目。

## 许可证

本项目采用 MIT 许可证，详情请参阅 [LICENSE](LICENSE) 文件。 