---
title: "CF 2074B - Mặt thứ ba"
description: "Chúng ta bắt đầu với một dãy số nguyên dương. Trong khi vẫn còn nhiều hơn một số, chúng tôi chọn hai giá trị, thay thế chúng bằng một số nguyên dương mới $x$ và yêu cầu ba độ dài tạo thành một tam giác không suy biến."
date: "2026-06-08T06:38:42+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2074
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1009 (Div. 3)"
rating: 800
weight: 2074
solve_time_s: 91
verified: true
draft: false
---

[CF 2074B - Bên thứ ba](https://codeforces.com/problemset/problem/2074/B) 

**Đánh giá:** 800 
**Tags:** hình học, tham lam, toán học 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên dương. Trong khi vẫn còn nhiều hơn một số, chúng tôi chọn hai giá trị, thay thế chúng bằng một số nguyên dương mới$x$và yêu cầu ba độ dài tạo thành một tam giác không suy biến. 

Đối với hai bên$a$Và$b$, bất đẳng thức tam giác cho chúng ta biết rằng cạnh thứ ba phải thỏa mãn$$|a-b| < x < a+b.$$Từ$x$phải là số nguyên nên lựa chọn lớn nhất có thể luôn là$$x = a+b-1.$$Quá trình tiếp tục cho đến khi chỉ còn lại một số. Mục tiêu của chúng tôi là tối đa hóa con số cuối cùng đó. 

Các ràng buộc đủ lớn nên chúng ta cần một giải pháp rất đơn giản. Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, có nghĩa là một$O(n)$hoặc$O(n \log n)$thuật toán cho mỗi trường hợp thử nghiệm đủ nhanh. Bất kỳ cách tiếp cận nào cố gắng khám phá các thứ tự hợp nhất khác nhau sẽ trở nên không khả thi vì số lượng các chuỗi hoạt động có thể tăng theo cấp số nhân. 

Thách thức chính là nhận ra rằng thứ tự hợp nhất chính xác không thành vấn đề một khi chúng ta hiểu được giá trị bị mất ở mỗi hoạt động là bao nhiêu. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Hãy xem xét một yếu tố duy nhất:```
1
10
```Không có thao tác nào được thực hiện nên câu trả lời đơn giản là`10`. Một giải pháp áp dụng một cách mù quáng công thức liên quan đến số lần hợp nhất có thể vô tình sửa đổi giá trị. 

Hãy xem xét hai yếu tố:```
2
5 7
```Sự thay thế hợp lệ lớn nhất là$5+7-1=11$, vậy câu trả lời là`11`. Việc giải thích bất cẩn về bất đẳng thức tam giác có thể cho phép$x=12$, nhưng điều đó sẽ tạo ra một tam giác suy biến. 

Hãy xem xét tất cả những cái:```
3
1 1 1
```Hợp nhất đầu tiên mang lại$1+1-1=1$. Hợp nhất một lần nữa mang lại$1+1-1=1$. Câu trả lời là`1`. Ví dụ này cho thấy rằng mỗi lần hợp nhất nhất thiết phải mất chính xác một đơn vị từ tổng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi cặp có thể ở mỗi bước và mọi giá trị hợp lệ của$x$. Vì mỗi thao tác thay đổi trạng thái của mảng nên số lượng các chuỗi có thể sẽ tăng lên theo tổ hợp. Ngay cả đối với nhỏ$n$, cây tìm kiếm trở nên khổng lồ. Cách tiếp cận như vậy là hoàn toàn không thực tế. 

Để tìm một mẫu, hãy tập trung vào điều gì xảy ra với tổng mảng. 

Giả sử chúng ta hợp nhất các giá trị$a$Và$b$. Để tối đa hóa câu trả lời cuối cùng, chúng ta nên chọn rõ ràng sự thay thế hợp lệ lớn nhất:$$x=a+b-1.$$Trước khi hoạt động, hai yếu tố đó góp phần$a+b$đến tổng số tiền. 

Sau khi phẫu thuật, họ góp phần$a+b-1$. 

Tổng số tiền giảm đi đúng một. 

Quan sát này là toàn bộ vấn đề. 

Mỗi thao tác sẽ loại bỏ chính xác một phần tử khỏi mảng, do đó làm giảm kích thước của mảng$n$để kích thước$1$yêu cầu chính xác$n-1$hợp nhất. 

Vì mỗi lần hợp nhất sẽ giảm tổng đi đúng một, nên giá trị cuối cùng phải là$$\left(\sum a_i\right) - (n-1).$$Không có gì khác quan trọng. Thứ tự hợp nhất không liên quan vì mỗi phép hợp nhất tối ưu sẽ mất đi một đơn vị và luôn có chính xác$n-1$hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính tổng của nó$S$. 
2. Hãy để$n$là số phần tử. 
3. Quan sát rằng mọi sự hợp nhất tối ưu đều thay thế$a$Và$b$với$a+b-1$. 
4. Mỗi lần hợp nhất như vậy sẽ giảm tổng số tiền đi đúng một. 
5. Giảm$n$các phần tử cho một phần tử yêu cầu chính xác$n-1$hợp nhất. 
6. Sau khi hợp nhất tất cả, giá trị cuối cùng bằng$$S-(n-1).$$1. Xuất giá trị này. 

### Tại sao nó hoạt động 

Sự thay thế lớn nhất có thể cho một cặp$(a,b)$là$a+b-1$, vì cạnh thứ ba của một tam giác không suy biến phải nhỏ hơn$a+b$. Bất kỳ lựa chọn nào nhỏ hơn sẽ chỉ làm giảm khả năng trong tương lai, vì vậy mọi chiến lược tối ưu đều chọn$a+b-1$. 

Mỗi lần hợp nhất tối ưu sẽ giảm tổng mảng đi đúng một. Vì số lần hợp nhất được cố định tại$n-1$, tổng mức giảm cũng được cố định tại$n-1$. Mảng cuối cùng chứa một giá trị duy nhất có giá trị bằng tổng ban đầu trừ đi tổng mức giảm này. Không có thứ tự hợp nhất nào có thể tạo ra một kết quả khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append(str(sum(a) - (n - 1)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo quan sát toán học. 

Tổng của mảng được tính một lần. Vì mỗi lần hợp nhất sẽ giảm tổng đi một và chính xác$n-1$sự hợp nhất xảy ra, chúng tôi trừ đi$n-1$từ tổng số. 

Không có điều kiện biên phức tạp. Khi$n=1$, công thức trở thành$$\text{sum}(a)-0,$$trả về chính xác giá trị ban đầu. Số nguyên Python dễ dàng xử lý tổng tối đa có thể, vì vậy việc tràn không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
998 244 353
```Số tiền ban đầu:$$998+244+353=1595.$$Số lần hợp nhất:$$3-1=2.$$| Số lượng | Giá trị | 
| --- | --- | 
| Tổng của mảng | 1595 | 
| Số lần hợp nhất | 2 | 
| Tổng số giảm | 2 | 
| Câu trả lời cuối cùng | 1593 | 

Câu trả lời là:```
1593
```Điều này chứng tỏ rằng chỉ có tổng số tiền và số lần hợp nhất mới quan trọng. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 5
```| Số lượng | Giá trị | 
| --- | --- | 
| Tổng của mảng | 15 | 
| Số lần hợp nhất | 4 | 
| Tổng số giảm | 4 | 
| Câu trả lời cuối cùng | 11 | 

Câu trả lời là:```
11
```Ví dụ này cho thấy rằng ngay cả với nhiều thứ tự hợp nhất có thể, mọi chuỗi tối ưu đều kết thúc bằng cùng một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Việc quét tuyến tính của từng mảng được thực hiện dễ dàng trong giới hạn thời gian và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans.append(str(sum(a) - (n - 1)))

    return "\n".join(ans)

# provided samples
assert run(
"""4
1
10
3
998 244 353
5
1 2 3 4 5
9
9 9 8 2 4 4 3 5 3
"""
) == """10
1593
11
39""", "sample 1"

# minimum size
assert run(
"""1
1
7
"""
) == "7", "single element"

# two elements
assert run(
"""1
2
5 7
"""
) == "11", "single merge"

# all equal values
assert run(
"""1
4
1 1 1 1
"""
) == "1", "all ones"

# larger values
assert run(
"""1
5
1000 1000 1000 1000 1000
"""
) == "4996", "large equal values"

# boundary style case
assert run(
"""1
3
1 1000 1
"""
) == "1000", "sum minus n-1"
```Trình trợ giúp tái tạo giải pháp cuộc thi và cho phép xác minh trực tiếp bằng các xác nhận. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Không có sự hợp nhất nào được thực hiện | 
|`5 7`|`11`| Xử lý đúng một lần hợp nhất | 
|`1 1 1 1`|`1`| Liên tiếp thua lỗ đúng một | 
|`1000 1000 1000 1000 1000`|`4996`| Giá trị lớn và độ chính xác số học | 
|`1 1000 1`|`1000`| Công thức hoạt động bất kể phân phối giá trị | 

## Vỏ cạnh 

Hãy xem xét mảng nhỏ nhất có thể:```
1
10
```Thuật toán tính toán:$$10-(1-1)=10.$$Không có sự hợp nhất nào xảy ra, khớp chính xác với quy trình thực. 

Hãy xem xét hai yếu tố:```
2
5 7
```Thuật toán tính toán:$$12-1=11.$$Một mô phỏng trực tiếp cũng cho$5+7-1=11$. Điều này khẳng định rằng bất đẳng thức tam giác nghiêm ngặt được xử lý chính xác. 

Hãy xem xét tất cả những cái:```
3
1 1 1
```Thuật toán tính toán:$$3-2=1.$$Một dấu vết thủ công mang lại:$$(1,1)\rightarrow 1,$$sau đó$$(1,1)\rightarrow 1.$$Giá trị cuối cùng lại là`1`. Điều này chứng tỏ tính bất biến rằng mỗi lần hợp nhất sẽ giảm tổng số tiền đi đúng một. 

Xem xét các giá trị rất không đồng đều:```
3
1 1000 1
```Thuật toán tính toán:$$1002-2=1000.$$Một trình tự tối ưu là:$$1+1-1=1,$$rời đi`[1000, 1]`. 

Sau đó:$$1000+1-1=1000.$$Giá trị cuối cùng là`1000`, khớp chính xác với công thức. Điều này xác nhận rằng kết quả chỉ phụ thuộc vào tổng số và số lần hợp nhất chứ không phụ thuộc vào thứ tự hợp nhất.
