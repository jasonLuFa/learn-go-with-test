# Intro

- 此專案目的為藉由測試驅動開發(TDD, Test-Driven Development )的方式，先寫測試再開始的方式進行
- 此專案使用 go 官方的標準測試套件 ( [testing package](https://pkg.go.dev/testing) )，當然實務上可能友會搭配其他好用的第三方套件 ( [stretchr/testify](https://github.com/stretchr/testify) )
- TDD 流程 :

  - 寫測試程式，並執行測試程式，讓他不通過，檢查錯誤訊息是否有意義 ( Write a failing test )
  - 再回去撰寫程式碼，讓測試通過 ( Make the test pass )
  - 重構程式碼 ( Refactor )

    ![TDD](https://marsner.com/wp-content/uploads/test-driven-development-TDD.png)

# 測試規則

1. 必須是 `\_test.go` 結尾 ( 範例 : hello_world_test.go )
2. 測試函數的名稱必須是 `TestHelloWorld` 開頭 ( 範例 : Test )
3. 測試函數只能傳入 `t *testing.T`

# 錯誤處理方式

## `t.Fail()`

- 是導致當前測試失敗並立即停止執行的函數，不會返回錯誤或提供有關失敗的任何其他信息

## `t.Errorf("\<format error message\>")`

- 跟 `t.Fail()` 類似，但是允許描述失敗的錯誤訊息
- 格式化訊息方式跟 `fmt.Printf("\<format message\>")` 一樣

# 在測試程式檔案中，將重複程式碼寫到新的函數

- 可使用 `t.Helper()` 在新的函數中，告訴測試套件此函數是 helper，在測試失敗時，錯誤訊息的行數會落在呼叫此函數的地方，這樣方便除錯
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

# 批次測試不同測試案例

- 使用 `t.Run("\<測試內容說明\>", func(t *testing.T){...})` 方式，可在測試函數中，一次測試多種案例

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

# Reference

- [Learn Go with tests](https://quii.gitbook.io/learn-go-with-tests/)
