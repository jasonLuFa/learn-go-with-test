# :bookmark_tabs: Intro

- 此專案目的為藉由測試驅動開發(TDD, Test-Driven Development )的方式，先寫測試再開始的方式進行
- 此專案使用 go 官方的標準測試套件 ( [testing package](https://pkg.go.dev/testing) )，當然實務上可能友會搭配其他好用的第三方套件 ( [stretchr/testify](https://github.com/stretchr/testify) )
- TDD 流程 :

  - 寫測試程式，並執行測試程式，讓他不通過，檢查錯誤訊息是否有意義 ( Write a failing test )
  - 再回去撰寫程式碼，讓測試通過 ( Make the test pass )
  - 重構程式碼 ( Refactor )

    ![TDD](https://marsner.com/wp-content/uploads/test-driven-development-TDD.png)

# :triangular_ruler: 測試規則

1. 必須是 `_test.go` 結尾 ( 範例 : hello_world_test.go )
2. 測試函數的名稱必須是 `Test` 開頭 ( 範例 : TestHelloWorld )
3. 測試函數只能傳入 `t *testing.T`
4. `go test ./...`：測試當前目錄下所有測試檔案
   - `-v` : 顯示詳細資訊
   - `-cover` : 可以看測試程式覆蓋率

# 🏃‍♂️ 執行測試
- 測試指定的 package : `go test <PACKAGE_PATH>`
- 測試當專案的所有檔案( directory tree ) : `go test ./...`
- 測試當下資料夾所有 tests : `go test .`
- `go test --run TestFunctionName` ：測試單一測試程式用，測試 TestFunctionName 此測試程式
- 測試時常帶參數
	- `-v` : verbose, log all tests as they are run. Also print all text from log and fmt calls even if the test succeeds
	- `-cover` : Enable coverage analysis
	- `--race` : 測試時偵測 [Race Condition](https://github.com/ardanlabs/gotraining/blob/f4355fce6fb0a161c7d01e39f166065085a26b6a/topics/go/concurrency/data_race/README.md)

# :bug: 錯誤處理方式

### 1. `t.Fail()`

- 是導致當前測試失敗並立即停止執行的函數，不會返回錯誤或提供有關失敗的任何其他信息

### 2. `t.Errorf(<format error message>)`

- 跟 `t.Fail()` 類似，但是允許描述失敗的錯誤訊息
- 格式化訊息方式跟 `fmt.Printf(<format message>)` 一樣

# :computer: 在測試程式檔案中，將重複程式碼寫到新的函數

- 可使用 [t.Helper()](./fundamentals/4.slice_array/README.md) 在新的函數中，告訴測試套件此函數是 helper，在測試失敗時，錯誤訊息的行數會落在呼叫此函數的地方，這樣方便除錯
- 範例 :

  ```go
  // 傳入 testing.TB 原因是，此介面可讓我們使用 *testing.T 或 *testing.B
  // *testing.B 是用於測試效能(performance)用的
  func assertCorrectMessage(t testing.TB, got, want string) {
    t.Helper()
    if got != want {
      t.Errorf("got %q want %q", got, want)
    }
  }
  ```

# :test_tube: 批次測試不同測試案例

- 使用 `t.Run("\<測試內容說明\>", func(t *testing.T){...})` 方式，可在測試函數中，一次測試多種案例
- **Note** : 衡量每個測試案例價值，目標不應該是進行所有的測試案例，而應該是對程式碼盡可能有更多的信心，測試一些核心的程式和案例，過多的測試可能會增加更多的維護開銷，請記住 ***每次測試都有成本***。

  ```go
  testcase := []struct {
      name string
      test func(t *testing.T)
    }{
      {
      name: "..."
      test: func( t *testing.T){...}
      },
      {
        ...
      },
      ...
    }

  for i := range testcase {
    tc := testcase[i]
    t.Run(tc.name, tc.test)
  }
  ```

# :link: Reference

- [Learn Go with tests](https://quii.gitbook.io/learn-go-with-tests/)
