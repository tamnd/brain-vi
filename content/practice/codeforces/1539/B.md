---
title: "CF 1539B - Bản tình ca"
description: "Chúng ta được cung cấp một chuỗi chữ thường thể hiện bài hát và nhiều truy vấn về các chuỗi con liền kề của chuỗi đó. Mỗi chữ cái đóng góp một giá trị tương ứng với vị trí của nó trong bảng chữ cái. Ví dụ: a = 1, b = 2, c = 3, v.v."
date: "2026-06-10T14:42:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1539
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 727 (Div. 2)"
rating: 800
weight: 1539
solve_time_s: 502
verified: false
draft: false
---

[CF 1539B - Bản tình ca](https://codeforces.com/problemset/problem/1539/B) 

**Đánh giá:** 800 
**Tags:** dp, triển khai, chuỗi 
**Thời gian giải:** 8m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường thể hiện bài hát và nhiều truy vấn về các chuỗi con liền kề của chuỗi đó. 

Mỗi chữ cái đóng góp một giá trị tương ứng với vị trí của nó trong bảng chữ cái. Ví dụ,`a = 1`,`b = 2`,`c = 3`, vân vân. Đối với một truy vấn`[l, r]`, chúng ta nhìn vào chuỗi con từ vị trí`l`để định vị`r`. Mỗi ký tự được lặp lại nhiều lần bằng giá trị bảng chữ cái của nó và chúng ta cần độ dài của chuỗi mở rộng thu được. 

Thay vì thực sự xây dựng chuỗi mở rộng, chúng ta có thể nghĩ xem mỗi ký tự đóng góp bao nhiêu vào độ dài cuối cùng. Nếu một nhân vật là`c`, nó góp phần`3`. Nếu một nhân vật là`z`, nó góp phần`26`. Câu trả lời cho một chuỗi con chỉ đơn giản là tổng các giá trị này trên tất cả các ký tự trong phạm vi đó. 

Những hạn chế là phần then chốt của vấn đề. Độ dài chuỗi và số lượng truy vấn đều có thể đạt tới`100000`. Một giải pháp quét toàn bộ chuỗi con cho mỗi truy vấn sẽ yêu cầu tới`100000 × 100000 = 10^10`operations in the worst case, which is far beyond what can run within two seconds. We need a way to answer each query in constant or logarithmic time after some preprocessing.

 One easy mistake is to interpret the task as counting distinct letters. Coi như:```
s = "aaa"
query = [1, 3]
```Mỗi`a`đóng góp`1`, vậy đáp án là:```
1 + 1 + 1 = 3
```Đầu ra đúng là`3`, không`1`. 

Một lỗi phổ biến khác là sử dụng các chỉ mục truy vấn dựa trên số 0 mặc dù đầu vào sử dụng các vị trí dựa trên một. 

Ví dụ:```
s = "ab"
query = [1, 1]
```Chuỗi con là`"a"`, vậy câu trả lời là`1`. 

Nếu chúng tôi vô tình coi truy vấn là dựa trên số 0, chúng tôi sẽ truy cập sai ký tự và tạo ra kết quả không chính xác. 

Trường hợp tinh vi thứ ba xảy ra khi truy vấn bao trùm toàn bộ chuỗi:```
s = "zzz"
query = [1, 3]
```Câu trả lời là:```
26 + 26 + 26 = 78
```Bất kỳ lỗi sai sót nào trong ranh giới tổng tiền tố sẽ ngay lập tức tạo ra kết quả sai trên các phạm vi đó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý mọi truy vấn một cách độc lập. Đối với một truy vấn`[l, r]`, chúng ta lặp qua tất cả các ký tự trong chuỗi con đó, chuyển đổi từng chữ cái thành vị trí trong bảng chữ cái của nó, tính tổng các giá trị và in kết quả. 

Cách tiếp cận này đúng vì câu trả lời cần có chính xác là số tiền đó. Vấn đề là hiệu quả. Một chuỗi con có thể chứa tối đa`100000`các ký tự, và có thể có`100000`truy vấn. Trong trường hợp xấu nhất, chúng tôi sẽ thực hiện khoảng`10^10`lượt truy cập của nhân vật, quá chậm. 

Cấu trúc của vấn đề gợi ý một cách tiếp cận tốt hơn. Mọi truy vấn đều yêu cầu tổng các giá trị trên một phân đoạn liền kề của cùng một mảng. Khi chúng tôi chuyển đổi từng ký tự thành giá trị bảng chữ cái của nó, tác vụ sẽ trở thành một vấn đề truy vấn tổng phạm vi cổ điển. 

Đối với các truy vấn tổng phạm vi trên một mảng tĩnh, tổng tiền tố là công cụ tiêu chuẩn. Nếu như`pref[i]`lưu trữ tổng giá trị của lần đầu tiên`i`ký tự, sau đó tính tổng trên bất kỳ khoảng nào`[l, r]`có thể thu được bằng cách trừ hai tổng tiền tố:```
pref[r] - pref[l - 1]
```Quá trình tiền xử lý mất thời gian tuyến tính và mọi truy vấn sẽ trở thành một phép tính theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`q`, và chuỗi`s`. 
2. Tạo mảng tổng tiền tố`pref`chiều dài`n + 1`, với`pref[0] = 0`. 
3. Di chuyển chuỗi từ trái sang phải. 
4. Với mỗi ký tự, hãy tính vị trí bảng chữ cái của nó bằng cách sử dụng:```
ord(character) - ord('a') + 1
```5. Lưu trữ tổng tích lũy trong mảng tiền tố.`pref[i]`phải chứa tổng giá trị của giá trị đầu tiên`i`nhân vật. 
6. Đối với mọi truy vấn`[l, r]`, tính:```
answer = pref[r] - pref[l - 1]
```7. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Mảng tiền tố duy trì sự bất biến`pref[i]`bằng tổng giá trị ký tự ở các vị trí`1`bởi vì`i`. 

Khi chúng tôi tính toán`pref[r] - pref[l - 1]`, sự đóng góp của các vị trí`1`bởi vì`l - 1`xuất hiện trong cả hai điều khoản và hủy bỏ. Những gì còn lại chính xác là tổng giá trị từ vị trí`l`thông qua vị trí`r`, là độ dài cần thiết của chuỗi mở rộng. Vì mỗi ký tự đóng góp độc lập vào độ dài cuối cùng nên tổng này chính xác là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)

    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + (ord(s[i - 1]) - ord('a') + 1)

    ans = []

    for _ in range(q):
        l, r = map(int, input().split())
        ans.append(str(pref[r] - pref[l - 1]))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc dữ liệu đầu vào và chuyển đổi chuỗi thành tổng giá trị bảng chữ cái tích lũy. biểu hiện`ord(s[i - 1]) - ord('a') + 1`bản đồ`a`ĐẾN`1`,`b`ĐẾN`2`, vân vân. 

Mảng tiền tố sử dụng chỉ mục dựa trên một. Lựa chọn này phù hợp với định dạng truy vấn và làm cho công thức phạm vi trở nên đơn giản. Giá trị tại`pref[0]`bằng 0, cho phép truy vấn bắt đầu tại vị trí`1`được xử lý không có trường hợp đặc biệt. 

Đối với mỗi truy vấn, câu trả lời thu được bằng một phép trừ duy nhất. Không có ký tự nào từ chuỗi con được quét lại. 

Một lỗi triển khai phổ biến là trộn lẫn các chỉ mục chuỗi và chỉ mục tiền tố. Bản thân chuỗi đó dựa trên số 0, vì vậy ký tự`i`trong việc xây dựng tiền tố tương ứng với`s[i - 1]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7 3
abacaba
1 3
2 5
1 7
```Giá trị ký tự: 

| Vị trí | Nhân vật | Giá trị | Tiền tố Tổng | 
| --- | --- | --- | --- | 
| 0 | - | - | 0 | 
| 1 | một | 1 | 1 | 
| 2 | b | 2 | 3 | 
| 3 | một | 1 | 4 | 
| 4 | c | 3 | 7 | 
| 5 | một | 1 | 8 | 
| 6 | b | 2 | 10 | 
| 7 | một | 1 | 11 | 

Truy vấn: 

| tôi | r | Tính toán | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 3 | 4 - 0 | 4 | 
| 2 | 5 | 8 - 1 | 7 | 
| 1 | 7 | 11 - 0 | 11 | 

Đầu ra:```
4
7
11
```Ví dụ này cho thấy mọi truy vấn trở thành phép trừ của hai tổng tiền tố, bất kể độ dài chuỗi con. 

### Ví dụ tùy chỉnh 

đầu vào:```
5 2
abcde
1 5
3 4
```Tổng tiền tố: 

| Vị trí | Nhân vật | Giá trị | Tiền tố Tổng | 
| --- | --- | --- | --- | 
| 0 | - | - | 0 | 
| 1 | một | 1 | 1 | 
| 2 | b | 2 | 3 | 
| 3 | c | 3 | 6 | 
| 4 | d | 4 | 10 | 
| 5 | e | 5 | 15 | 

Truy vấn: 

| tôi | r | Tính toán | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 5 | 15 - 0 | 15 | 
| 3 | 4 | 10 - 3 | 7 | 

Đầu ra:```
15
7
```Dấu vết này chứng tỏ rằng các phạm vi bên trong hoạt động giống hệt như các phạm vi chạm vào ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Xây dựng mảng tiền tố lấy O(n), mỗi truy vấn lấy O(1) | 
| Không gian | O(n) | Mảng tổng tiền tố lưu trữ n + 1 số nguyên | 

Với`n`Và`q`cả hai đều lớn như`100000`, thuật toán chỉ thực hiện vài trăm nghìn thao tác. Điều này vừa vặn thoải mái trong giới hạn thời gian và mức sử dụng bộ nhớ nhỏ so với 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, q = map(int, input().split())
    s = input().strip()

    pref = [0] * (n + 1)

    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + (ord(s[i - 1]) - ord('a') + 1)

    ans = []

    for _ in range(q):
        l, r = map(int, input().split())
        ans.append(str(pref[r] - pref[l - 1]))

    return "\n".join(ans)

# provided sample
assert run(
"""7 3
abacaba
1 3
2 5
1 7
"""
) == "4\n7\n11", "sample"

# minimum size
assert run(
"""1 1
a
1 1
"""
) == "1", "single character"

# all equal letters
assert run(
"""4 2
bbbb
1 4
2 3
"""
) == "8\n4", "repeated letters"

# entire range and suffix range
assert run(
"""3 2
zzz
1 3
2 3
"""
) == "78\n52", "large letter values"

# off-by-one boundary check
assert run(
"""2 2
ab
1 1
2 2
"""
) == "1\n2", "boundary positions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`, truy vấn`[1,1]`|`1`| Đầu vào hợp lệ tối thiểu | 
|`bbbb`với nhiều truy vấn |`8`,`4`| Các chữ cái lặp đi lặp lại và tổng phạm vi | 
|`zzz`|`78`,`52`| Giá trị bảng chữ cái lớn | 
|`ab`với các truy vấn một vị trí |`1`,`2`| Xử lý ranh giới và lập chỉ mục chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1
a
1 1
```Mảng tiền tố trở thành:```
pref = [0, 1]
```Câu trả lời là:```
pref[1] - pref[0] = 1
```Thuật toán xử lý chính xác các phạm vi bắt đầu ở vị trí`1`bởi vì`pref[0]`tồn tại. 

Bây giờ hãy xem xét một phạm vi bao gồm toàn bộ chuỗi:```
3 1
zzz
1 3
```Mảng tiền tố là:```
pref = [0, 26, 52, 78]
```Kết quả truy vấn là:```
78 - 0 = 78
```Không cần xử lý đặc biệt cho các truy vấn đầy đủ. 

Cuối cùng, hãy xem xét một truy vấn kết thúc ở ký tự cuối cùng nhưng không bắt đầu ở ký tự đầu tiên:```
5 1
abcde
3 5
```Tổng tiền tố là:```
[0, 1, 3, 6, 10, 15]
```Câu trả lời trở thành:```
15 - 3 = 12
```cái nào bằng`3 + 4 + 5`. Điều này xác nhận rằng việc trừ`pref[l - 1]`loại bỏ chính xác tiền tố không mong muốn và chỉ để lại phần đóng góp chuỗi con được yêu cầu.
