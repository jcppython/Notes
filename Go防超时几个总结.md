# Go 防超时

## 利用 Timer AfterFunc

```go
cmd := exec.Command("ping", "127.0.0.1", `-t`)
if err := cmd.Start(); err != nil {
    log.Fatal(err)
}

timer := time.AfterFunc(3 * time.Second, func() {
    cmd.Process.Kill()
})

err := cmd.Wait()
timer.Stop()
```

AfterFunc 等待相应时间后, 执行它自己的goroutine，返回一个Timer以便可以被stop



## 利用select

```go
done := make(chan error)
go func() { 
    done <- cmd.Wait()
}()

var err error
select {
case <-time.After(timeout):
    // timeout
    if err = cmd.Process.Kill(); err != nil {
        log.Error("failed to kill: %s, error: %s", cmd.Path, err)
    }
    go func() {
        <-done // allow goroutine to exit
    }()
    log.Info("process:%s killed", cmd.Path)
    return err, true
case err = <-done:
    return err, false
}
```



## ref

http://ulricqin.com/post/set-script-timeout-use-golang/

https://github.com/golang/go/issues/9580





```go
// error mismatched types int and time.Duration
// TIMEOUT := 12
var timeout time.Duration = TIMEOUT * time.Second
// right
var timeout time.Duration = time.Duration(TIMEOUT) * time.Second
// right
var timeout time.Duration = 11 * time.Second

```

time.Duration定义为type Duration int64

time.Second也是Duration的类型

相乘的也必须是Duration的类型