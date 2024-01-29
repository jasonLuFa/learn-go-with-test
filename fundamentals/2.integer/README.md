# First Head Example Code

- 使用 `task f2` 執行 2.integer 所有測試檔案
- 除了使用 `TestAdder` 作為測試程式，也可以使用 `ExampleAdd` 來當作使用該 function 的範例

  ```go
  func TestAdder(t *testing.T) {
    sum := Add(2, 2)
    expected := 4

    if sum != expected {
      t.Errorf("expected '%d' but got '%d'", expected, sum)
    }
  }
  ```

  ```go
  func ExampleAdd() {
    sum := Add(1, 5)
    // Please note that the example function will not be executed if you remove the comment // Output: 6. Although the function will be compiled, it won't be executed.
    fmt.Println(sum) // Output: 6
  }
  ```