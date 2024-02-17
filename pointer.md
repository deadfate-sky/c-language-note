# pointer

來自 ostep ch5.3 的 [`p3.c` 程式](https://github.com/remzi-arpacidusseau/ostep-code/blob/master/cpu-api/p3.c)。此處須釐清一些自己對 pointer 的理解。搭配[
李根逸博士](https://feis.studio/#/c)的線上課程影片。

- `char *myargs[3];` 是一個指標陣列 (array of pointers)，代表說是一個長度為 3 的陣列，每一個位置中，儲存的是型別為 `char *` 的指標。
- 這邊的目的，是希望將 `wc p3.c NULL` 這個指令，拆成三個字串，分別儲存在 `myargs` 這個指標陣列中，方便後續 `execvp()` 使用。
- 根據 GNU C 語言的說明，`strdup` 的 input 也是一個字串指標 `char *`，`strdup("wc")` 會為 input 的 string 分配空間，並複製 string 至新空間 (於此處是 `"wc"`)，最後回傳新的位置的 pointer。
- 至於為何需要使用 `strdup` 這麼做，應是出於「確保我們使用的字串可以被自由修改」的可能性。在 `p3.c` 中並不是傳入一個在其他地方已經定義好的 pointer variable，例如 `char *command = "wc"`，而是直接傳入 `"wc"` 本身。如此即使我們直接使用 `myargs[0] = "wc";` 也不會造成影響。但假如我們使用 `myargs[0] = command;`，則會造成 `command` 這個 pointer 所指向的內容，在其他處因為對 `myargs[0]` 的修改而改變，這可能會造成未知的風險，並非我們在開發時所樂見。基於良好的開發習慣，使用 `strdup` 是一個比較好的做法！
