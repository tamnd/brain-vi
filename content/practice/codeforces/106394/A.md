---
title: "CF 106394A - Sushi"
description: "Vấn đề là về một hàng miếng sushi trong đó mỗi miếng đều thuộc một trong hai loại. Phân đoạn bữa ăn hợp lệ là khối liền kề trong đó nửa đầu chỉ chứa một loại sushi và nửa sau chỉ chứa loại còn lại, với cả hai nửa có cùng kích thước."
date: "2026-06-25T10:09:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106394
codeforces_index: "A"
codeforces_contest_name: "RUCP x WiCS Mini-Contest"
rating: 0
weight: 106394
solve_time_s: 49
verified: true
draft: false
---

[CF 106394A - Sushi](https://codeforces.com/problemset/problem/106394/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề là về một hàng miếng sushi trong đó mỗi miếng đều thuộc một trong hai loại. Phân đoạn bữa ăn hợp lệ là khối liền kề trong đó nửa đầu chỉ chứa một loại sushi và nửa sau chỉ chứa loại còn lại, với cả hai nửa có cùng kích thước. Nhiệm vụ là tìm độ dài tối đa có thể có của đoạn đó. 

Đầu vào mô tả số lượng miếng sushi và loại từng miếng từ trái sang phải. Đầu ra là độ dài của đoạn liền kề hợp lệ dài nhất. 

Hạn chế chính là số lượng miếng sushi có thể lớn, lên tới khoảng$10^5$. Một giải pháp kiểm tra mọi phân đoạn có thể sẽ yêu cầu phép tính bậc hai, có nghĩa là đại khái$10^{10}$hoạt động trong trường hợp xấu nhất và vượt xa giới hạn thời gian thông thường cho phép. Chúng ta cần tìm cách xử lý hàng theo thời gian tuyến tính. 

Những trường hợp phức tạp đến từ những đoạn nhìn có vẻ cân đối nhưng thực chất lại không hợp lệ. Ví dụ:```
6
1 2 1 2 1 2
```Câu trả lời là:```
2
```Một cách tiếp cận bất cẩn có thể thấy rằng có số lượng bằng nhau của cả hai loại trong toàn bộ mảng và chọn một phân đoạn dài hơn, nhưng hai nửa của phân đoạn được chọn, mỗi nửa phải bao gồm một loại duy nhất. Sushi xen kẽ không đáp ứng quy tắc. 

Một trường hợp cạnh khác là khi đoạn dài nhất được hình thành chính xác ở ranh giới giữa hai nhóm:```
9
2 2 1 1 1 2 2 2 2
```Câu trả lời là:```
6
```Phân đoạn hợp lệ là`2 2 1 1 1 2`hoặc`1 1 1 2 2 2`. Lời giải phải xem xét mọi cặp nhóm lân cận có cùng loại chứ không chỉ nhóm lớn nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là tạo ra mọi phân đoạn liên tục có thể. Đối với mỗi đoạn, chúng tôi kiểm tra xem nó có hai nửa có chiều dài bằng nhau hay không và mỗi nửa chỉ chứa một loại sushi hay không. Điều này đúng vì mọi câu trả lời có thể đều được xem xét. Vấn đề là có$O(n^2)$các phân đoạn có thể và việc kiểm tra chúng có thể bổ sung thêm một yếu tố khác của$n$, dẫn đến một$O(n^3)$giải pháp trong việc thực hiện tồi tệ nhất. Ngay cả khi thông tin tiền tố làm giảm tính xác thực, số lượng phân đoạn ứng viên vẫn quá lớn. 

Quan sát quan trọng là phân đoạn hợp lệ phải nằm chính xác trên ranh giới nơi loại sushi thay đổi. Hãy tưởng tượng việc nén mảng thành các nhóm liên tiếp có giá trị bằng nhau. Một phân đoạn chỉ có thể lấy một số hậu tố của một nhóm và tiền tố có cùng kích thước của nhóm tiếp theo. Độ dài tối đa sử dụng hai nhóm lân cận gấp đôi kích thước nhóm nhỏ hơn. 

Ví dụ: nếu các nhóm là:```
2 2 2 1 1 1 1
```hai nhóm có độ dài 3 và 4. Chúng ta có thể lấy ba`2`s và ba`1`s, cho một đoạn có độ dài 6. Lấy bốn phần của nhóm thứ hai là không thể vì không đủ`2`S. 

Toàn bộ vấn đề trở thành việc tìm giá trị lớn nhất trong số tất cả các cặp nhóm liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc tệ hơn | O(1) | Quá chậm | 
| Nén nhóm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng từ trái sang phải và đếm chiều dài của mỗi khối liên tiếp của các loại sushi bằng nhau. 
2. Lưu trữ các độ dài khối này theo thứ tự. Một khối thể hiện sự đóng góp lớn nhất có thể có từ một loại sushi trước khi loại này thay đổi. 
3. Với mỗi cặp khối lân cận, hãy tính`2 * min(left_block, right_block)`. Đây là đoạn hợp lệ dài nhất vượt qua ranh giới này vì cả hai nửa phải có cùng độ dài. 
4. Giữ giá trị tối đa trong số tất cả các ranh giới và in nó. 

Lý do chúng tôi chỉ so sánh các khối lân cận là vì một phân đoạn hợp lệ phải chứa chính xác một chuyển đổi giữa các loại. Nếu có từ hai phần chuyển tiếp trở lên thì một trong hai nửa sẽ chứa các loại sushi hỗn hợp. 

Tại sao nó hoạt động: 

Mỗi phân đoạn hợp lệ có nửa bên trái của một loại và nửa bên phải của loại kia. Hai nửa gặp nhau tại một điểm mà kiểu thay đổi, do đó chúng thuộc về hai nhóm liền kề trong biểu diễn nén. Độ dài đoạn bị giới hạn bởi nhóm nhỏ hơn trong hai nhóm đó vì cả hai nửa đều cần kích thước bằng nhau. Kiểm tra tất cả các nhóm liền kề sẽ kiểm tra mọi phân đoạn hợp lệ có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    groups = []
    count = 1

    for i in range(1, n):
        if a[i] == a[i - 1]:
            count += 1
        else:
            groups.append(count)
            count = 1

    groups.append(count)

    ans = 0

    for i in range(len(groups) - 1):
        ans = max(ans, 2 * min(groups[i], groups[i + 1]))

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng một mảng vì toàn bộ thuật toán chỉ cần thứ tự các loại sushi. các`groups`list lưu trữ độ dài của các giá trị bằng nhau liên tiếp. 

Vòng lặp đầu tiên xây dựng biểu diễn nén. Bất cứ khi nào loại sushi hiện tại khác với loại trước đó, nhóm hiện tại sẽ kết thúc và độ dài của nó được lưu lại. 

Vòng lặp cuối cùng kiểm tra từng ranh giới có thể có giữa hai nhóm. Nhân độ dài nhóm nhỏ hơn với 2 sẽ được đoạn cân bằng lớn nhất có thể vượt qua ranh giới đó. 

Không có vấn đề tràn trong Python vì số nguyên có độ chính xác tùy ý. Việc xử lý ranh giới cũng đơn giản vì vòng lặp chỉ chạy khi có hàng xóm bên phải tồn tại. 

## Ví dụ đã hoạt động 

Mẫu 1:```
7
2 2 2 1 1 2 2
```| Vị trí | Độ dài nhóm hiện tại | Nhóm | Trả lời | 
| --- | --- | --- | --- | 
| 1 đến 3 | 3 | [3] | 0 | 
| 4 đến 5 | 2 | [3, 2] | 4 | 
| 6 đến 7 | 2 | [3, 2, 2] | 4 | 

Ranh giới tốt nhất là giữa hai nhóm đầu tiên. Chúng ta có thể lấy hai`2`s và hai`1`S. 

Mẫu 2:```
9
2 2 1 1 1 2 2 2 2
```| Vị trí | Độ dài nhóm hiện tại | Nhóm | Trả lời | 
| --- | --- | --- | --- | 
| 1 đến 2 | 2 | [2] | 0 | 
| 3 đến 5 | 3 | [2, 3] | 4 | 
| 6 đến 9 | 4 | [2, 3, 4] | 6 | 

Ranh giới ở giữa cho một đoạn có độ dài 6 vì các nhóm có độ dài 3 và 4 có thể đóng góp ba phần mỗi nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi miếng sushi được ghé thăm một lần trong khi tạo nhóm. | 
| Không gian | O(n) | Danh sách nhóm nén chứa tối đa một mục nhập cho mỗi nhóm. | 

Độ phức tạp tuyến tính phù hợp với kích thước đầu vào vì thuật toán tránh tạo ra các mảng con ứng cử viên và chỉ xử lý hàng một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    groups = []
    cnt = 1

    for i in range(1, n):
        if a[i] == a[i - 1]:
            cnt += 1
        else:
            groups.append(cnt)
            cnt = 1

    groups.append(cnt)

    ans = 0
    for i in range(len(groups) - 1):
        ans = max(ans, 2 * min(groups[i], groups[i + 1]))

    return str(ans) + "\n"

assert run("7\n2 2 2 1 1 2 2\n") == "4\n", "sample 1"
assert run("9\n2 2 1 1 1 2 2 2 2\n") == "6\n", "sample 2"

assert run("2\n1 2\n") == "2\n", "minimum size"
assert run("5\n1 1 1 1 1\n") == "0\n", "all equal values"
assert run("6\n1 2 1 2 1 2\n") == "2\n", "alternating values"
assert run("8\n2 2 2 2 1 1 1 1\n") == "8\n", "large single boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`2`| Chuyển đổi hợp lệ nhỏ nhất có thể | 
|`5 / 1 1 1 1 1`|`0`| Không có ranh giới tồn tại | 
|`6 / 1 2 1 2 1 2`|`2`| Các giá trị thay thế không thể tạo thành một đoạn dài | 
|`8 / 2 2 2 2 1 1 1 1`|`8`| Toàn bộ mảng là một phân đoạn hợp lệ | 

## Vỏ cạnh 

Đối với một mảng trong đó tất cả các loại sushi đều giống hệt nhau, không có điểm nào mà loại đó thay đổi, do đó không có phân đoạn hợp lệ nào có thể tồn tại. 

Ví dụ:```
5
1 1 1 1 1
```Thuật toán tạo ra một nhóm:```
[5]
```Không có nhóm liền kề nào để so sánh nên đáp án vẫn là`0`. 

Đối với các loại sushi xen kẽ:```
6
1 2 1 2 1 2
```Các nhóm là:```
[1, 1, 1, 1, 1, 1]
```Mọi ranh giới chỉ có thể tạo một đoạn có độ dài`2 * min(1, 1)`, vậy giá trị lớn nhất là`2`. Điều này ngăn chặn việc xử lý không chính xác số lượng bằng nhau của cả hai loại là đủ. 

Đối với trường hợp có một ranh giới lớn:```
8
2 2 2 2 1 1 1 1
```Các nhóm là:```
[4, 4]
```Thuật toán so sánh chúng và nhận được:```
2 * min(4, 4) = 8
```đó là toàn bộ mảng, vì hai nửa đều được làm từ một loại sushi duy nhất.
