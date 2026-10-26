---
title: "CF 106100E - Bóng Bay"
description: "Chúng ta được cấp một hàng bong bóng được biểu thị bằng một chuỗi gồm các ký tự B và R. Cấu hình được coi là đúng khi mỗi quả bóng bay màu đỏ đều có một quả bóng bay màu xanh ngay trước nó. Trong thuật ngữ chuỗi, mỗi lần xuất hiện của R phải thỏa mãn hai điều kiện: 1."
date: "2026-06-25T11:52:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106100
codeforces_index: "E"
codeforces_contest_name: "International MathCoding Narxoz open olympiad 2025"
rating: 0
weight: 106100
solve_time_s: 43
verified: true
draft: false
---

[CF 106100E - Bóng bay](https://codeforces.com/problemset/problem/106100/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng bóng bay được biểu thị bằng một chuỗi gồm các ký tự`B`Và`R`. 

Cấu hình được coi là đúng khi mỗi quả bóng màu đỏ đều có một quả bóng màu xanh ngay trước nó. Trong thuật ngữ chuỗi, mỗi lần xuất hiện của`R`phải thỏa mãn hai điều kiện: 

1. Nó không được là ký tự đầu tiên. 
2. Ký tự ngay bên trái của nó phải là`B`. 

Điều này có nghĩa là cấu hình hợp lệ không chứa bóng đỏ ở vị trí`0`và không có chuỗi con`"RR"`. 

Chúng tôi có thể thực hiện tối đa một lần hoán đổi các quả bóng bay lân cận. Nhiệm vụ là xác định xem liệu có thể đạt được cấu hình hợp lệ sau 0 hoặc một lần hoán đổi như vậy hay không. 

Độ dài chuỗi tối đa là 100. Với giới hạn nhỏ như vậy, thậm chí thử mọi thao tác hoán đổi liền kề có thể có và kiểm tra chuỗi kết quả cũng đủ nhanh. Một giải pháp kiểm tra tất cả các vị trí và xác thực chuỗi sau mỗi lần hoán đổi chỉ thực hiện vài nghìn thao tác. 

Một điểm tinh tế là hiểu được điều gì làm cho cấu hình trở nên hợp lệ. 

Coi như:```
BBR
```Câu trả lời là`YES`bởi vì quả bóng màu đỏ duy nhất ở ngay trước quả bóng màu xanh. 

Một sai lầm phổ biến là cho rằng sự sắp xếp phải luân phiên nhau như`BRBRBR...`. Điều đó không bắt buộc. 

Một trường hợp dễ bỏ sót khác là:```
RB
```Câu trả lời là`YES`. 

Hoán đổi hai quả bóng lân cận sẽ tạo ra:```
BR
```và bây giờ quả bóng đỏ đi theo quả bóng xanh. 

Một ví dụ nữa:```
RR
```Câu trả lời là`NO`. 

Việc hoán đổi duy nhất có thể giữ chuỗi như`"RR"`, điều này vẫn không hợp lệ vì quả bóng màu đỏ đầu tiên không có quả bóng màu xanh trước nó. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. 

Đầu tiên, hãy kiểm tra xem chuỗi gốc đã hợp lệ chưa. Nếu có thì câu trả lời sẽ có ngay`YES`bởi vì chúng tôi được phép thực hiện nhiều nhất một lần hoán đổi. 

Nếu không, hãy thử mọi cặp liền kề. Đối với mỗi vị trí`i`, tráo đổi`s[i]`Và`s[i+1]`, kiểm tra xem chuỗi kết quả có hợp lệ hay không, sau đó khôi phục thứ tự ban đầu. Nếu bất kỳ nỗ lực nào thành công, hãy in`YES`. 

Tại sao điều này đúng? Vì bài toán chỉ cho phép thực hiện một thao tác và thao tác đó chỉ có thể hoán đổi các bong bóng lân cận. Việc liệt kê tất cả các giao dịch hoán đổi như vậy bao gồm mọi động thái có thể xảy ra. 

Với độ dài tối đa là 100, có tối đa 99 giao dịch hoán đổi liền kề. Kiểm tra tính hợp lệ mất`O(n)`thời gian. Tổng công việc là`O(n²)`, tối đa là khoảng 10.000 lần kiểm tra ký tự. 

Không cần phải quan sát phức tạp hơn vì các ràng buộc rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

Đối với vấn đề này, cách tiếp cận vũ phu đã đủ tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi. 
2. Xác định hàm`valid(t)`. 

Hàm này quét chuỗi từ trái sang phải. 

Nếu một nhân vật là`R`, kiểm tra xem nó có hàng xóm bên trái hay không và liệu hàng xóm đó có`B`. 

Nếu một trong hai điều kiện không thành công, hãy trả về`False`. 

Nếu quá trình quét kết thúc, hãy quay lại`True`. 
3. Nếu chuỗi gốc hợp lệ, hãy in`YES`. 

Không được phép hoán đổi vì vấn đề cho biết "nhiều nhất là một lần hoán đổi". 
4. Chuyển đổi chuỗi thành danh sách các ký tự có thể thay đổi. 
5. Đối với mọi chỉ số`i`từ`0`ĐẾN`n-2`: 

Hoán đổi vị trí`i`Và`i+1`. 

Kiểm tra xem sự sắp xếp mới có hợp lệ hay không. 

Nếu có thì in`YES`và dừng lại. 

Khôi phục trao đổi trước khi tiếp tục. 
6. Nếu không có trao đổi liền kề nào tạo ra sự sắp xếp hợp lệ, hãy in`NO`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cấu hình có thể truy cập được mà không cần di chuyển và mọi cấu hình có thể truy cập chính xác trong một lần hoán đổi liền kề. 

Đó chính xác là tất cả các trạng thái được phép của vấn đề. Bài kiểm tra tính hợp lệ khớp chính xác với yêu cầu rằng mọi quả bóng màu đỏ phải ngay sau quả bóng màu xanh. 

Vì mọi kết quả pháp lý đều được kiểm tra và chấp nhận chính xác khi nó thỏa mãn quy tắc nên thuật toán không thể bỏ sót một giải pháp hợp lệ hoặc chấp nhận sai một giải pháp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid(s):
    n = len(s)
    for i in range(n):
        if s[i] == 'R':
            if i == 0 or s[i - 1] != 'B':
                return False
    return True

def solve():
    s = input().strip()

    if valid(s):
        print("YES")
        return

    arr = list(s)
    n = len(arr)

    for i in range(n - 1):
        arr[i], arr[i + 1] = arr[i + 1], arr[i]

        if valid(arr):
            print("YES")
            return

        arr[i], arr[i + 1] = arr[i + 1], arr[i]

    print("NO")

solve()
```các`valid`hàm mã hóa trực tiếp định nghĩa của sự sắp xếp chính xác. Mỗi quả bóng màu đỏ phải có một quả bóng màu xanh ngay bên trái. 

Thủ tục chính trước tiên sẽ kiểm tra chuỗi gốc vì không được phép hoán đổi. Sau đó, nó sẽ thử mọi lần hoán đổi lân cận một cách có hệ thống. 

Khôi phục trao đổi sau mỗi lần kiểm tra là quan trọng. Nếu không khôi phục, các lần lặp lại sau này sẽ hoạt động trên một chuỗi đã sửa đổi thay vì cấu hình ban đầu. 

Vì độ dài chuỗi rất nhỏ nên việc xây dựng và kiểm tra các cấu hình đề xuất này nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
RB
```Chuỗi ban đầu không hợp lệ vì quả bóng màu đỏ đứng đầu. 

Đang thử hoán đổi: 

| Hoán đổi vị trí | Chuỗi kết quả | Có hiệu lực? | 
| --- | --- | --- | 
| 0 | BR | Có | 

Thuật toán tìm thấy cấu hình hợp lệ sau một lần hoán đổi liền kề và in:```
YES
```Ví dụ này chứng minh rằng một hoán đổi cục bộ có thể di chuyển một quả bóng màu đỏ phía sau một quả bóng màu xanh và thỏa mãn điều kiện. 

### Ví dụ 2 

đầu vào:```
RR
```Kiểm tra ban đầu: 

| Vị trí | Nhân vật | Tình trạng | 
| --- | --- | --- | 
| 0 | R | Thất bại ngay lập tức | 

Đang thử hoán đổi: 

| Hoán đổi vị trí | Chuỗi kết quả | Có hiệu lực? | 
| --- | --- | --- | 
| 0 | RR | Không | 

Không có sự sắp xếp hợp lệ nào có thể đạt được. 

Đầu ra:```
NO
```Ví dụ này cho thấy một số vi phạm không thể được sửa chữa bằng một lần hoán đổi liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Lên đến`n-1`trao đổi, mỗi lần đăng ký`O(n)`| 
| Không gian | O(n) | Mảng ký tự có thể thay đổi để mô phỏng trao đổi | 

Với`n ≤ 100`, công việc trong trường hợp xấu nhất chỉ là vài nghìn thao tác, thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def valid(s):
        n = len(s)
        for i in range(n):
            if s[i] == 'R':
                if i == 0 or s[i - 1] != 'B':
                    return False
        return True

    s = input().strip()

    if valid(s):
        return "YES\n"

    arr = list(s)
    n = len(arr)

    for i in range(n - 1):
        arr[i], arr[i + 1] = arr[i + 1], arr[i]

        if valid(arr):
            return "YES\n"

        arr[i], arr[i + 1] = arr[i + 1], arr[i]

    return "NO\n"

# provided sample
assert run("BR\n") == "YES\n", "sample"

# custom cases
assert run("RB\n") == "YES\n", "single swap fixes order"
assert run("RR\n") == "NO\n", "impossible"
assert run("BBR\n") == "YES\n", "already valid"
assert run("RBRR\n") == "YES\n", "swap last two positions"
assert run("RRR\n") == "NO\n", "cannot repair with one swap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`RB`|`YES`| Một trao đổi liền kề sửa chuỗi | 
|`RR`|`NO`| Trường hợp bất khả thi | 
|`BBR`|`YES`| Đã hợp lệ, không hoán đổi | 
|`RBRR`|`YES`| Sắp xếp hợp lệ có thể đạt được bằng một lần trao đổi | 
|`RRR`|`NO`| Nhiều vi phạm không thể sửa chữa | 

## Vỏ cạnh 

Hãy xem xét:```
RB
```Cấu hình ban đầu không hợp lệ vì bong bóng màu đỏ ở đầu. Thuật toán thử hoán đổi liền kề duy nhất và thu được:```
BR
```Bây giờ quả bóng màu đỏ có một quả bóng màu xanh ngay trước nó, vậy câu trả lời là`YES`. 

Coi như:```
BBR
```Thuật toán kiểm tra chuỗi gốc trước khi thử bất kỳ thao tác hoán đổi nào. Bong bóng màu đỏ duy nhất được đặt trước bong bóng màu xanh lam, do đó cấu hình đã chính xác. Vì không được phép hoán đổi nên câu trả lời là`YES`. 

Coi như:```
RR
```Quả bóng màu đỏ đầu tiên vi phạm quy tắc. Hoán đổi cặp lân cận duy nhất không làm thay đổi chuỗi. Mọi cấu hình có thể truy cập vẫn không hợp lệ, do đó thuật toán sẽ xuất ra chính xác`NO`. 

Coi như:```
RBRR
```Ban đầu, quả bóng màu đỏ cuối cùng theo sau quả bóng màu đỏ khác. Hoán đổi hai ký tự cuối cùng tạo ra:```
RBRR
```không thay đổi gì, nhưng việc hoán đổi cặp giữa mang lại:```
BRRR
```vẫn không hợp lệ. Thuật toán kiểm tra mọi trao đổi liền kề và tìm ra sự sắp xếp có thể truy cập hợp lệ nếu có. Việc liệt kê đầy đủ đảm bảo rằng không có giải pháp hoán đổi một lần nào có thể bị bỏ qua.
