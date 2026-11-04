---
title: "CF 1138A - Sushi cho 2 người"
description: "Chúng ta được cung cấp một hàng miếng sushi, trong đó mỗi miếng là loại 1 hoặc loại 2. Chúng ta muốn chọn một đoạn liền kề của hàng này. Một phân đoạn hợp lệ nếu nó bao gồm hai khối liên tiếp thuộc các loại sushi khác nhau và cả hai khối đều có cùng kích thước."
date: "2026-06-12T03:55:02+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1138
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 545 (Div. 2)"
rating: 900
weight: 1138
solve_time_s: 72
verified: true
draft: false
---

[CF 1138A - Sushi cho hai người](https://codeforces.com/problemset/problem/1138/A) 

**Xếp hạng:** 900 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một hàng miếng sushi, trong đó mỗi miếng là một loại`1`hoặc gõ`2`. Chúng tôi muốn chọn một đoạn liền kề của hàng này. 

Một phân đoạn hợp lệ nếu nó bao gồm hai khối liên tiếp thuộc các loại sushi khác nhau và cả hai khối đều có cùng kích thước. Ví dụ,`[2,2,2,1,1,1]`hợp lệ vì nó là ba`2`s theo sau là ba`1`S. Một phân đoạn như`[2,2,1,1]`cũng hợp lệ. Một phân đoạn như`[1,2,1,2]`không hợp lệ vì mỗi nửa chứa cả hai loại. 

Nhiệm vụ là tìm độ dài tối đa có thể có của đoạn liền kề hợp lệ như vậy. 

Độ dài mảng có thể lớn bằng`100000`, điều này ngay lập tức loại trừ các phương pháp đắt tiền để kiểm tra tất cả các mảng con. Có khoảng`n²`các mảng con có thể có, sẽ ở xung quanh`10^10`kiểm tra trong trường hợp xấu nhất. Thậm chí một`O(n²)`thuật toán quá chậm. Chúng ta cần một giải pháp xử lý mảng theo thời gian tuyến tính hoặc gần với nó. 

Phần khó khăn là phân đoạn được chọn không nhất thiết phải sử dụng toàn bộ các chuỗi có giá trị bằng nhau. Nó có thể chỉ sử dụng một phần của quá trình chạy. 

Coi như:```
1 1 1 1 2 2
```Các đường chạy có độ dài`4`Và`2`. Phân đoạn hợp lệ tốt nhất là:```
1 1 2 2
```với chiều dài`4`. Việc triển khai bất cẩn mà chỉ xem xét các lần chạy hoàn chỉnh sẽ trả về không chính xác`6`. 

Một trường hợp tế nhị khác là:```
1 2 1 2
```Mỗi lần chạy đều có độ dài`1`. 

Câu trả lời đúng là`2`, bởi vì phân khúc tốt nhất là`[1,2]`hoặc`[2,1]`. Không còn phân đoạn hợp lệ. Việc triển khai cố gắng hợp nhất nhiều lần chạy xen kẽ sẽ tạo ra kết quả không chính xác. 

Một trường hợp cạnh nữa là:```
1 1 1 2
```Độ dài chạy là`3`Và`1`. 

Câu trả lời là`2`, sử dụng`[1,2]`. Cuộc chạy lớn hơn không buộc chúng ta phải sử dụng tất cả các yếu tố của nó. Chúng ta chỉ cần số lượng bằng nhau từ hai lần chạy liền kề. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi mảng con có thể có và kiểm tra xem nó có bao gồm chính xác hai khối liên tiếp gồm các loại sushi khác nhau có kích thước bằng nhau hay không. Việc kiểm tra một mảng con có thể mất thời gian tuyến tính, gây ra độ phức tạp trong trường hợp xấu nhất`O(n³)`. Ngay cả khi tối ưu hóa, việc kiểm tra tất cả`O(n²)`mảng con là không thể cho`n = 100000`. 

Quan sát chính xuất phát từ việc xem mảng dưới dạng các chuỗi có giá trị bằng nhau. 

Ví dụ:```
2 2 2 1 1 2 2
```trở thành:```
[3, 2, 2]
```trong đó mỗi số đại diện cho độ dài của một khối liên tiếp. 

Bất kỳ câu trả lời hợp lệ nào cũng phải được hình thành từ hai lần chạy liền kề. Nếu hai đường chạy lân cận có độ dài`a`Và`b`, chúng ta có thể lấy:```
min(a, b)
```các yếu tố từ mỗi phía. 

Điều đó tạo ra một đoạn có độ dài hợp lệ:```
2 * min(a, b)
```Ví dụ, chạy`(3,2)`tạo ra một đoạn có chiều dài`4`, trong khi chạy`(5,7)`tạo ra một đoạn có chiều dài`10`. 

Vì mọi phân đoạn hợp lệ phải nằm trên chính xác một ranh giới giữa hai lần chạy lân cận, nên bài toán giảm xuống còn việc tìm giá trị lớn nhất của:```
2 * min(run[i], run[i+1])
```trên tất cả các cặp chạy liền kề. 

Việc xây dựng độ dài lần chạy cần một lần duyệt qua mảng và việc kiểm tra các lần chạy lân cận cần một lần nữa. Điều này đưa ra một giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng sushi từ trái sang phải và nén nó thành các đoạn dài. 

Ví dụ:```
2 2 2 1 1 2 2
```trở thành:```
[3, 2, 2]
```Mỗi số ghi lại kích thước của một khối sushi giống hệt nhau tối đa. 
2. Lặp lại qua từng cặp lần chạy lân cận. 

Nếu độ dài là`a`Và`b`, phân đoạn hợp lệ lớn nhất vượt qua ranh giới sử dụng của họ`min(a,b)`mảnh từ mỗi lần chạy. 
3. Tính toán`2 * min(a,b)`cho mỗi cặp liền kề. 

Đây là phân đoạn hợp lệ tối đa có thể được hình thành xung quanh ranh giới đó. 
4. Giữ giá trị lớn nhất được nhìn thấy. 
5. Xuất ra mức tối đa. 

### Tại sao nó hoạt động 

Mỗi phân đoạn hợp lệ chứa chính xác hai nhóm sushi liên tiếp, mỗi nhóm một loại. Một đoạn như vậy phải vượt qua một ranh giới giữa hai lần chạy liền kề trong biểu diễn nén. 

Giả sử độ dài chạy lân cận là`a`Và`b`. Để giữ số lượng cả hai loại sushi bằng nhau, chúng ta có thể sử dụng tối đa`min(a,b)`mảnh từ mỗi bên. Sử dụng nhiều hơn sẽ vượt quá kích thước của lần chạy nhỏ hơn và phá vỡ điều kiện bằng. 

Do đó, đoạn tốt nhất liên kết với ranh giới đó có độ dài`2 * min(a,b)`. Vì mọi phân đoạn hợp lệ đều tương ứng với một số cặp chuỗi liền kề nên việc lấy giá trị tối đa trên tất cả các lần chạy lân cận sẽ kiểm tra mọi câu trả lời có thể có và trả về giá trị tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    t = list(map(int, input().split()))

    runs = []
    cnt = 1

    for i in range(1, n):
        if t[i] == t[i - 1]:
            cnt += 1
        else:
            runs.append(cnt)
            cnt = 1

    runs.append(cnt)

    ans = 0
    for i in range(len(runs) - 1):
        ans = max(ans, 2 * min(runs[i], runs[i + 1]))

    print(ans)

solve()
```Phần đầu tiên của mã xây dựng mã hóa độ dài chạy của chuỗi sushi. Biến`cnt`theo dõi kích thước khối hiện tại. Bất cứ khi nào loại sushi thay đổi, lần chạy đã hoàn thành sẽ được lưu trữ và lần chạy mới sẽ bắt đầu. 

Sau khi quá trình quét kết thúc, lần chạy cuối cùng cũng phải được thêm vào. Quên bước này là một lỗi phổ biến vì lần chạy cuối cùng không bao giờ có sự thay đổi. 

Vòng lặp thứ hai kiểm tra từng cặp lần chạy lân cận. Với mỗi cặp, biểu thức`2 * min(runs[i], runs[i + 1])`đưa ra phân đoạn hợp lệ lớn nhất vượt qua ranh giới đó. 

Câu trả lời đơn giản là mức tối đa của các giá trị này. 

Không cần xử lý đặc biệt đối với các ranh giới của mảng ban đầu vì mọi phân đoạn hợp lệ được biểu thị bằng một số cặp lần chạy liền kề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
2 2 2 1 1 2 2
```Độ dài chạy:```
[3, 2, 2]
```| Chạy liền kề | phút(a,b) | Độ dài ứng viên | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| (3, 2) | 2 | 4 | 4 | 
| (2, 2) | 2 | 4 | 4 | 

Câu trả lời cuối cùng:```
4
```Ví dụ này cho thấy chỉ có các hoạt động lân cận mới quan trọng. Đoạn tối ưu có thể được hình thành xung quanh một trong hai ranh giới. 

### Ví dụ 2 

đầu vào:```
2
2 1
```Độ dài chạy:```
[1, 1]
```| Chạy liền kề | phút(a,b) | Độ dài ứng viên | Tốt nhất cho đến nay | 
| --- | --- | --- | --- | 
| (1, 1) | 1 | 2 | 2 | 

Câu trả lời cuối cùng:```
2
```Đây là đầu vào hợp lệ nhỏ nhất có thể. Toàn bộ mảng tạo thành câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt xây dựng chạy, một lượt kiểm tra các lượt chạy liền kề | 
| Không gian | O(n) | Trong trường hợp xấu nhất, mọi phần tử đều bắt đầu một lần chạy mới | 

Giải pháp chỉ thực hiện một lượng công việc không đổi trên mỗi phần tử mảng. Với`n ≤ 100000`, thời gian tuyến tính dễ dàng phù hợp trong giới hạn thời gian và mảng độ dài chạy yêu cầu nhiều nhất`100000`số nguyên, nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    t = list(map(int, input().split()))

    runs = []
    cnt = 1

    for i in range(1, n):
        if t[i] == t[i - 1]:
            cnt += 1
        else:
            runs.append(cnt)
            cnt = 1

    runs.append(cnt)

    ans = 0
    for i in range(len(runs) - 1):
        ans = max(ans, 2 * min(runs[i], runs[i + 1]))

    return str(ans) + "\n"

# provided sample
assert run("7\n2 2 2 1 1 2 2\n") == "4\n", "sample 1"

# minimum size
assert run("2\n1 2\n") == "2\n", "minimum size"

# unequal neighboring runs
assert run("6\n1 1 1 1 2 2\n") == "4\n", "take only part of larger run"

# alternating values
assert run("4\n1 2 1 2\n") == "2\n", "many runs of length one"

# larger balanced runs
assert run("8\n1 1 1 2 2 2 2 2\n") == "6\n", "min of adjacent runs"

# off-by-one around final run
assert run("7\n1 1 2 2 2 2 1\n") == "4\n", "last run handled correctly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`2`| Phiên bản hợp lệ tối thiểu | 
|`1 1 1 1 2 2`|`4`| Đường chạy lớn hơn phải được cắt bớt | 
|`1 2 1 2`|`2`| Chạy luân phiên | 
|`1 1 1 2 2 2 2 2`|`6`| Công dụng`2 * min(a,b)`| 
|`1 1 2 2 2 2 1`|`4`| Xử lý đúng lần chạy cuối cùng | 

## Vỏ cạnh 

Hãy xem xét:```
4
1 1 1 2
```Độ dài chạy là:```
[3, 1]
```Thuật toán tính toán:```
2 * min(3, 1) = 2
```và trả về`2`. Điều này tương ứng với phân đoạn hợp lệ`[1,2]`. Phần lớn hơn không thể đóng góp nhiều hơn một phần tử vì phần nhỏ hơn chỉ chứa một món sushi. 

Bây giờ hãy xem xét:```
4
1 2 1 2
```Độ dài chạy là:```
[1, 1, 1, 1]
```Các giá trị ứng cử viên là:```
2, 2, 2
```và câu trả lời là`2`. Thuật toán tránh được việc kết hợp nhiều hơn hai lần chạy liên tiếp một cách chính xác. 

Cuối cùng, hãy xem xét:```
8
1 1 1 1 2 2 2 2
```Độ dài chạy là:```
[4, 4]
```Câu trả lời là:```
2 * min(4, 4) = 8
```Toàn bộ mảng là hợp lệ và thuật toán trả về độ dài đầy đủ.
