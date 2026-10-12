---
title: "CF 105030A - \u0420\u0430\u0441\u043f\u0440\u0435\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043f\u0440\u044f\u043d\u043e\u0441\u0442\u0438"
description: "Mỗi đơn vị gia vị được dán nhãn bằng một chủ đề số nguyên. Chúng ta phải phân phối tất cả các đơn vị cho càng ít học sinh càng tốt. Một học sinh có thể nhận được nhiều đơn vị, nhưng bất kỳ hai chủ đề nào được giao cho học sinh đó phải khác nhau hơn k."
date: "2026-06-28T01:33:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105030
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105030
solve_time_s: 77
verified: true
draft: false
---

[CF 105030A - \u0420\u0430\u0441\u043f\u0440\u0435\u0434\u0435\u043b\u0435\u043d\u0438\u0435 \u043f\u0440\u044f\u043d\u043e\u0441\u0442\u0438](https://codeforces.com/problemset/problem/105030/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đơn vị gia vị được dán nhãn bằng một chủ đề số nguyên. Chúng ta phải phân phối tất cả các đơn vị cho càng ít học sinh càng tốt. 

Một học sinh có thể nhận được nhiều đơn vị bài học, nhưng bất kỳ hai chủ đề nào được giao cho học sinh đó phải khác nhau nhiều hơn`k`. Nếu hai số chủ đề khác nhau nhiều nhất`k`, chúng được coi là tương tự nhau và không thể thuộc về cùng một học sinh. 

Nhiệm vụ là tính số học sinh tối thiểu cần thiết. 

Giá trị của`n`đạt tới`2 · 10^5`, do đó, bất kỳ thuật toán nào so sánh từng cặp chủ đề hoặc thử các bài tập khác nhau đều ngay lập tức quá đắt. Thuật toán bậc hai yêu cầu khoảng`4 · 10^10`trong trường hợp xấu nhất vượt xa giới hạn thực tế. Chúng ta nên nhắm tới`O(n log n)`hoặc tốt hơn. 

Một số tình huống đáng được quan tâm đặc biệt. 

Khi`k = 0`, chỉ riêng các chủ đề giống nhau đều bị cấm trong bài tập của một học sinh. Ví dụ,```
3 0
3 1 1
```Câu trả lời đúng là`2`. Một học sinh có thể nhận đề tài`1`Và`3`, trong khi học sinh thứ hai nhận phần còn lại`1`. Chỉ cần đếm các giá trị riêng biệt sẽ tạo ra sai số`1`. 

Khoảng cách lớn giữa các giá trị loại bỏ hoàn toàn xung đột. Ví dụ,```
4 5
1 10 20 30
```Câu trả lời đúng là`1`, bởi vì mỗi cặp khác nhau nhiều hơn`5`. Một giải pháp chỉ xem xét tổng số phần tử mà không tính đến khoảng cách sẽ đánh giá quá cao câu trả lời. 

Nhiều giá trị bằng nhau cùng với các giá trị gần đó là một cái bẫy khác. Coi như```
5 2
5 5 5 6 7
```Câu trả lời đúng là`5`. Mỗi bản sao của`5`xung đột với mọi bản sao khác và cả hai`6`Và`7`cũng xung đột với mọi`5`. Một bài tập tham lam chỉ theo dõi giá trị trước đó trong chuỗi của một học sinh có thể dễ dàng trở thành sai nếu bỏ qua bội số. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử giao mọi chủ đề cho các học sinh hiện có và tạo một học sinh mới bất cứ khi nào không ai có thể chấp nhận được. Nếu được thực hiện cẩn thận, điều này cuối cùng sẽ tìm ra nhiệm vụ tối thiểu, bởi vì mọi quyết định đều kiểm tra tất cả các học sinh trước đó. Thật không may, mỗi trong số`n`chủ đề có thể quét tới`n`sinh viên, sản xuất`O(n²)`công việc. 

Quan sát quan trọng là sau khi sắp xếp các giá trị chủ đề, mọi xung đột sẽ trở thành cục bộ. Một chủ đề chỉ xung đột với các giá trị nằm trong một khoảng thời gian`k`. Thay vì nghĩ về từng học sinh, chúng ta có thể hỏi một câu hỏi khác. 

Giả sử chúng ta xem xét mọi khoảng giá trị có giá trị lớn nhất trừ giá trị nhỏ nhất nhiều nhất là`k`. Mỗi cặp trong khoảng thời gian như vậy xung đột với nhau, vì vậy không ai trong số họ có thể thuộc về cùng một học sinh. Nếu một khoảng chứa`m`ít nhất là các phần tử`m`sinh viên là cần thiết. 

Điều đáng ngạc nhiên là giới hạn dưới này cũng đủ. 

Sau khi sắp xếp, biểu đồ xung đột là biểu đồ khoảng. Đồ thị khoảng là đồ thị hoàn hảo, có nghĩa là số màu bằng kích thước của cụm lớn nhất. Ở đây, một cụm chính xác là một tập hợp các giá trị được chứa trong một khoảng độ dài nào đó`k`. Do đó, số lượng sinh viên tối thiểu bằng số lượng phần tử được sắp xếp tối đa có trong bất kỳ cửa sổ nào thỏa mãn`a[r] - a[l] <= k`. 

Việc tìm kích thước cửa sổ tối đa này chính xác là kỹ thuật hai con trỏ cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(1) bổ sung, không bao gồm sắp xếp | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào và sắp xếp tất cả các giá trị chủ đề. 
2. Duy trì hai chỉ số,`l`Và`r`, mô tả cửa sổ hiện tại trong mảng được sắp xếp. 
3. Gia hạn`r`từ trái sang phải. Sau khi chèn giá trị mới, kiểm tra xem cửa sổ có còn đáp ứng không`a[r] - a[l] <= k`. 
4. Trong khi chênh lệch vượt quá`k`, di chuyển`l`chuyển tiếp cho đến khi điều kiện trở lại đúng. 

Mỗi lần`l`di chuyển, cửa sổ trở nên hẹp hơn, do đó cuối cùng tình trạng được khôi phục. 
5. Cửa sổ hiện tại chứa hậu tố lớn nhất kết thúc tại`r`tất cả các giá trị của nó đều nằm trong một phạm vi độ dài`k`. 
6. Cập nhật câu trả lời với độ dài cửa sổ hiện tại,`r - l + 1`. 
7. Sau khi xử lý mọi vị trí, xuất kích thước cửa sổ tối đa. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, cửa sổ chứa chính xác các phần tử có giá trị nằm trong một khoảng độ dài`k`. Mỗi cặp bên trong cửa sổ đó khác nhau nhiều nhất`k`, nên chúng xung đột với nhau. Họ phải chiếm giữ các sinh viên khác nhau, làm cho độ dài cửa sổ trở thành giới hạn dưới cần thiết. 

Ngược lại, biểu đồ xung đột được hình thành bởi các khoảng trên một đường là biểu đồ khoảng. Đồ thị khoảng luôn có thể được tô màu bằng cách sử dụng chính xác kích thước của nhóm lớn nhất của chúng. Vì mỗi cụm tương ứng với một cửa sổ như vậy nên kích thước cửa sổ lớn nhất cũng đủ. Thuật toán tính toán chính xác đại lượng này nên câu trả lời là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    ans = 0
    l = 0

    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = max(ans, r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc sắp xếp sẽ chuyển đổi những khác biệt về giá trị thành các phân đoạn liền kề nhau, cho phép cửa sổ trượt hoạt động chính xác. 

Con trỏ bên trái chỉ di chuyển về phía trước, không bao giờ lùi lại. Thuộc tính này đảm bảo công việc tuyến tính sau khi sắp xếp. Mỗi phần tử vào cửa sổ một lần và rời khỏi nó một lần. 

Sự so sánh bên trong`while`vòng lặp phải được`> k`. Windows có sự khác biệt chính xác`k`vẫn hợp lệ vì những giá trị đó được coi là tương tự và thuộc cùng một nhóm. 

Câu trả lời được cập nhật sau khi khôi phục bất biến`a[r] - a[l] <= k`. Cập nhật trước đó sẽ tính các cửa sổ không hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào```
3 2
1 2 4
```Sau khi sắp xếp, mảng không thay đổi. 

| r | Giá trị | l sau khi thu nhỏ lại | Cửa sổ hiện tại | Kích thước cửa sổ | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 1 | 
| 1 | 2 | 0 | [1,2] | 2 | 2 | 
| 2 | 4 | 1 | [2,4] | 2 | 2 | 

Cửa sổ hợp lệ lớn nhất chứa hai phần tử, vì vậy hai học sinh là đủ và cũng cần thiết. 

### Mẫu 2 

đầu vào```
9 2
7 1 2 8 5 4 9 3 6
```Sau khi sắp xếp,```
1 2 3 4 5 6 7 8 9
```| r | Giá trị | l sau khi thu nhỏ lại | Cửa sổ hiện tại | Kích thước cửa sổ | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | [1] | 1 | 1 | 
| 1 | 2 | 0 | [1,2] | 2 | 2 | 
| 2 | 3 | 0 | [1,2,3] | 3 | 3 | 
| 3 | 4 | 1 | [2,3,4] | 3 | 3 | 
| 4 | 5 | 2 | [3,4,5] | 3 | 3 | 
| 5 | 6 | 3 | [4,5,6] | 3 | 3 | 
| 6 | 7 | 4 | [5,6,7] | 3 | 3 | 
| 7 | 8 | 5 | [6,7,8] | 3 | 3 | 
| 8 | 9 | 6 | [7,8,9] | 3 | 3 | 

Mỗi cửa sổ hợp lệ có kích thước tối đa là ba và nhiều cửa sổ đạt được kích thước đó. Vậy số học sinh tối thiểu là`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, cửa sổ trượt tuyến tính | 
| Không gian | O(1) thêm | Chỉ một số chỉ mục được lưu trữ ngoài mảng đã sắp xếp | 

Thuật toán dễ dàng xử lý`2 · 10^5`các phần tử. Việc sắp xếp nhiều số nguyên này rất nhanh và hai con trỏ chỉ quét mảng một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    l = 0
    ans = 0
    for r in range(n):
        while a[r] - a[l] > k:
            l += 1
        ans = max(ans, r - l + 1)
    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue()

# provided samples
assert run("3 2\n1 2 4\n") == "2\n"
assert run("9 2\n7 1 2 8 5 4 9 3 6\n") == "3\n"
assert run("3 0\n3 1 1\n") == "2\n"

# custom cases
assert run("1 100\n42\n") == "1\n"
assert run("5 0\n7 7 7 7 7\n") == "5\n"
assert run("4 5\n1 10 20 30\n") == "1\n"
assert run("5 2\n1 3 3 5 7\n") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100 / 42`|`1`| Kích thước đầu vào tối thiểu | 
|`5 0 / 7 7 7 7 7`|`5`| Tất cả các giá trị bằng nhau | 
|`4 5 / 1 10 20 30`|`1`| Không có xung đột nào cả | 
|`5 2 / 1 3 3 5 7`|`3`| Giá trị trùng lặp cùng với sự khác biệt về ranh giới | 

## Vỏ cạnh 

Khi nào`k = 0`, chỉ có các giá trị giống hệt nhau xung đột. 

đầu vào:```
3 0
3 1 1
```Sau khi sắp xếp chúng ta thu được`[1, 1, 3]`. Cửa sổ hợp lệ tối đa là`[1, 1]`, có kích thước là`2`. Đầu ra của thuật toán`2`, đủ số lượng học sinh theo yêu cầu. 

Khi tất cả các giá trị cách xa nhau, mọi cửa sổ sẽ nhanh chóng co lại để bao gồm tất cả các phần tử được xử lý vì mọi chênh lệch đều vượt quá`k`. 

đầu vào:```
4 5
1 10 20 30
```Mọi cửa sổ hợp lệ đều có kích thước`1`, vì vậy câu trả lời trở thành`1`. Một học sinh có thể nhận được mọi đơn vị gia vị vì không có cặp chủ đề nào giống nhau. 

Khi nhiều giá trị bằng nhau xuất hiện cùng với các giá trị lân cận, các giá trị trùng lặp sẽ được tính một cách tự nhiên vì cửa sổ trượt hoạt động trên các phần tử chứ không phải các giá trị riêng biệt. 

đầu vào:```
5 2
5 5 5 6 7
```Toàn bộ mảng nằm gọn trong một cửa sổ vì`7 - 5 = 2`. Kích thước cửa sổ là`5`, vậy cần có 5 học sinh. Mỗi bản sao của`5`xung đột với mọi bản sao khác và cả hai`6`Và`7`cũng xung đột với họ, khiến bất kỳ nhiệm vụ nhỏ hơn nào cũng không thể thực hiện được.
