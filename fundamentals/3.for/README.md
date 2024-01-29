# First Head Benchmark

- 使用 `task f3` 執行 3.for 所有測試檔案
- 使用 benchmark 來測試 function 的效能
  - 當執行 benchmark code 時，它會運行 `b.N` 次並測量所需的時間。
  - `115.7 ns/op` 表示我們的函數在我的電腦上，平均需要 `115.7 奈秒` 運行。為了測試這一點，它運行了 10000000 次。（預設情況下會按順序執行）

  ```go
  func BenchmarkRepeat(b *testing.B) {
    for i := 0; i < b.N; i++ {
      Repeat("a")
    }
  }
  ```

  ```
  goos: darwin
  goarch: amd64
  pkg: github.com/jasonLuFa/learn-go-with-test/fundamentals/3.for
  BenchmarkRepeat-12       9893356               115.7 ns/op
  PASS
  ```