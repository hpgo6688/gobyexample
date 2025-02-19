这段 Go 代码展示了如何通过通道（channels）在 goroutine 之间通信。以下是代码的执行顺序和流程：

1. **创建通道**：
- `jobs` 是一个缓冲通道，容量为 5，用于传递整数。
- `done` 是一个无缓冲通道，用于通知所有工作已接收完毕。

2. **启动 goroutine**：
- 一个匿名函数在新的 goroutine 中运行。它从 `jobs` 通道中接收数据。
- 使用 `for` 循环不断从 `jobs` 通道中接收数据。
- `j, more := <-jobs`：从 `jobs` 接收数据，`more` 表示通道是否关闭。
- 如果 `more` 为 `true`，表示还有数据，打印接收到的工作。
- 如果 `more` 为 `false`，表示通道已关闭，打印“received all jobs”，并向 `done` 通道发送 `true`，然后返回以结束 goroutine。

3. **发送数据到通道**：
- 主函数中，循环发送 1 到 3 的整数到 `jobs` 通道，每发送一个打印“sent job X”。
- 发送完所有数据后，调用 `close(jobs)` 关闭通道，并打印“sent all jobs”。

4. **等待 goroutine 完成**：
- 主函数通过 `<-done` 阻塞等待，直到从 `done` 通道接收到数据，表示 goroutine 已完成所有工作。

**输出顺序**：

- "sent job 1"
- "received job 1"
- "sent job 2"
- "received job 2"
- "sent job 3"
- "received job 3"
- "sent all jobs"
- "received all jobs"

由于 goroutine 的调度，接收和发送的顺序可能稍有不同，但整体逻辑保持不变。