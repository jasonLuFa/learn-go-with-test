# First Head Slice And Array

- 使用 `task f4` 執行 4.slice_array 所有測試檔案
- t.Helper() 在函數中，告訴測試套件此函數是 helper，在測試失敗時，錯誤訊息的行數會落在呼叫此函數的地方 ( `checkSums(t, got, want)` ) 而非 checkSums 裡的 `t.Errorf("got %v want %v", got, want)`

  ```go
  func TestSumAllTails(t *testing.T) {

    checkSums := func(t *testing.T, got, want []int) {
      t.Helper()
      if !reflect.DeepEqual(got, want) {
        t.Errorf("got %v want %v", got, want)
      }
    }

    // ...
    t.Run("failed the test and see how t.Helper() work ", func(t *testing.T) {
      got := SumAllTails([]int{}, []int{3, 4, 5})
      want := []int{}
      checkSums(t, got, want)
    })

  }
  ```

- 在 go test 後面多加個 `-cover` 可以看測試程式覆蓋率