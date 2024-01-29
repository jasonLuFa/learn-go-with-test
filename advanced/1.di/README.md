# Dependency Injection

- 使用 `task a1` 執行 1.di 所有測試檔案
- 使用 Dependency Injection 好處
  - 讓我們的 function 更容易測試
  - 以此範例解耦( decoupling )資料的去向，不在單一只輸出在 termial，而是藉由 注入 `io.writer` 去選擇資料的去向
