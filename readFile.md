# Python 讀取文字檔：一步一步練習

這份練習會帶你從開啟文字檔開始，學會讀取全部內容、逐行讀取，以及處理檔案不存在的情況。

## 開始前

請確認 `sample.txt` 和你執行的 Python 程式放在同一個資料夾。這份練習附的 `sample.txt` 內容如下：

```text
今天開始學 Python 檔案讀取。
第一步：開啟檔案。
第二步：讀取內容。
第三步：記得安全地關閉檔案。
```

建立一個新的 Python 檔案，例如 `read_file.py`，並把下面的程式依序貼上、執行和修改。也可以在 `lesson1.py` 中練習。

## 練習一：讀取整個檔案

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    content = file.read()

print(content)
```

重點：

- `open(...)` 用來開啟檔案。
- `"sample.txt"` 是檔案名稱。這種寫法會從程式的「目前工作資料夾」尋找檔案；初學時先把文字檔和程式放在同一個資料夾最簡單。
- `"r"` 表示讀取（read）。
- `encoding="utf-8"` 可以正確讀取中文。
- `with` 區塊結束時會自動關閉檔案，不需要自己呼叫 `close()`。
- `read()` 會把整個檔案讀成一個字串。

**試試看：**在 `sample.txt` 最後加一行，再執行程式。新的一行有出現在輸出中嗎？

## 練習二：一次讀一行

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    first_line = file.readline()

print(first_line)
```

`readline()` 每呼叫一次就讀取下一行。請試著再呼叫一次，看看讀到的內容如何改變：

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    first_line = file.readline()
    second_line = file.readline()

print("第一行：", first_line, end="")
print("第二行：", second_line, end="")
```

## 練習三：逐行讀完整個檔案

檔案很大時，逐行處理通常比一次讀取全部內容更合適：

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    for line in file:
        print(line, end="")
```

每次迴圈會取得一行。文字行通常已經包含換行字元，所以使用 `end=""`，避免 `print()` 再額外加一個空行。

**試試看：**修改程式，讓它在每行前面加上行號：

```python
with open("sample.txt", "r", encoding="utf-8") as file:
    for number, line in enumerate(file, start=1):
        print(f"{number}: {line}", end="")
```

`enumerate(..., start=1)` 會同時提供行號和該行內容。

## 練習四：找出包含指定文字的行

例如，找出含有「第二步」的那一行：

```python
keyword = "第二步"

with open("sample.txt", "r", encoding="utf-8") as file:
    for line in file:
        if keyword in line:
            print(line, end="")
```

**練習題：**把 `keyword` 改成「Python」，找出包含它的行。

## 練習五：處理檔案找不到的情況

如果檔案名稱或路徑不正確，Python 會發生 `FileNotFoundError`。你可以用 `try` 和 `except` 顯示清楚的提示：

```python
try:
    with open("not_found.txt", "r", encoding="utf-8") as file:
        print(file.read())
except FileNotFoundError:
    print("找不到檔案，請確認檔名和檔案位置。")
```

請先執行一次，確認會看到提示；接著把 `"not_found.txt"` 改成 `"sample.txt"` 再執行。

## 小挑戰

1. 在 `sample.txt` 新增三個你喜歡的句子。
2. 逐行讀取檔案，並印出每一行的行號。
3. 設定一個關鍵字，只印出包含該關鍵字的行。
4. 把檔案名稱改錯，練習顯示「找不到檔案」的提示；完成後改回正確檔名。

## 常見問題

**出現 `FileNotFoundError` 怎麼辦？**  
確認 `sample.txt` 是否存在，以及 Python 執行時的目前工作資料夾是否放有這個檔案。若不確定，可在 Python 中執行 `import os; print(os.getcwd())` 查看目前工作資料夾。

**中文顯示亂碼怎麼辦？**  
開啟檔案時加上 `encoding="utf-8"`，並確認文字檔使用 UTF-8 編碼。

**讀完一定要關閉檔案嗎？**  
使用 `with open(...) as file:` 時，離開 `with` 區塊後檔案會自動關閉。
