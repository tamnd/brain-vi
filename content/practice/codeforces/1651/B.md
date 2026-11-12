---
title: "CF 1651B - Chứng minh anh ta sai"
description: "Chúng ta cần xây dựng một mảng có độ dài n sao cho việc áp dụng thao tác đã cho vào bất kỳ cặp vị trí nào cũng không bao giờ làm giảm tổng của mảng. Giả sử chúng ta chọn hai giá trị x và y."
date: "2026-06-10T03:48:49+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1651
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 124 (Rated for Div. 2)"
rating: 800
weight: 1651
solve_time_s: 130
verified: true
draft: false
---

[CF 1651B - Chứng minh anh ta sai](https://codeforces.com/problemset/problem/1651/B) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng có độ dài`n`sao cho việc áp dụng thao tác đã cho cho bất kỳ cặp vị trí nào không bao giờ làm giảm tổng của mảng. 

Giả sử chúng ta chọn hai giá trị`x`Và`y`. Hoạt động thay thế cả hai số bằng`|x-y|`. 

Trước khi phẫu thuật, hai vị trí này góp phần$$x+y$$đến tổng số tiền. 

Sau khi phẫu thuật, họ góp phần$$2|x-y|.$$Để tổng không bao giờ giảm thì ta cần$$2|x-y| \ge x+y$$cho từng cặp phần tử trong mảng. 

Đầu vào chứa một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi chỉ được cung cấp độ dài mảng mong muốn`n`. Chúng ta phải xây dựng một mảng hợp lệ có các phần tử nằm giữa`1`Và`10^9`hoặc chứng minh rằng không tồn tại mảng như vậy. 

Những hạn chế là rất nhỏ. Có tối đa 100 trường hợp thử nghiệm và`n`tối đa là 1000. Bất kỳ công trình nào chạy trong thời gian tuyến tính cho mỗi trường hợp thử nghiệm là quá đủ. Thử thách thực sự là khám phá ra mô hình toán học làm cho mỗi cặp thỏa mãn bất đẳng thức cần thiết trong khi vẫn giữ tất cả các giá trị trong phạm vi cho phép. 

Một vài trường hợp đáng chú ý. 

Coi như`n = 2`. Mảng`[1, 2]`là hợp lệ bởi vì$$2|2-1| = 2,\qquad 1+2 = 3.$$Trên thực tế điều này làm giảm số tiền, vì vậy nó không hợp lệ. Một giải pháp bất cẩn có thể cho rằng bất kỳ chuỗi tăng dần nào cũng có tác dụng. Chúng ta cần khoảng cách lớn hơn nhiều giữa các giá trị liên tiếp. 

Coi như`n = 3`và mảng`[1, 2, 4]`. Đối với cặp`(2,4)`,$$2|4-2| = 4 < 6.$$Tổng giảm nên lũy thừa của hai là không đủ. 

Xem xét lớn`n`. Ngay cả khi chúng ta tìm thấy một dãy tăng nhanh thì tất cả các giá trị tối đa phải được duy trì`10^9`. Một công trình phát triển quá nhanh có thể vượt quá giới hạn trước khi đạt chiều dài`n`, làm cho một số giá trị trở thành bất hợp pháp. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là suy nghĩ trực tiếp về điều kiện của mỗi cặp. Cho một mảng, chúng ta có thể kiểm tra tất cả các cặp`(i,j)`và xác minh xem$$2|a_i-a_j| \ge a_i+a_j.$$Đối với một mảng có chiều dài`n`, điều này đòi hỏi phải kiểm tra`O(n^2)`cặp. 

Vấn đề thực sự không phải là xác minh mà là xây dựng. Chúng ta cần một họ mảng tự động thỏa mãn bất đẳng thức cho mỗi cặp. 

Giả sử hai phần tử thỏa mãn`x < y`. Sau đó$$|x-y| = y-x.$$Thay vào bất đẳng thức sẽ cho$$2(y-x)\ge x+y.$$Sắp xếp lại,$$y \ge 3x.$$Đây là quan sát quan trọng. Mỗi cặp phải thỏa mãn điều kiện và đối với một mảng được sắp xếp, chỉ cần mỗi phần tử lớn hơn ít nhất gấp ba lần phần tử nhỏ là đủ. 

Một công trình xây dựng tự nhiên là$$1,\ 3,\ 9,\ 27,\dots$$trong đó mỗi phần tử gấp ba lần phần tử trước đó. 

Đối với bất kỳ cặp nào có chỉ số`i < j`,$$a_j = 3^{j-i}a_i \ge 3a_i,$$vì vậy điều kiện dẫn xuất được giữ tự động. 

Bây giờ chúng ta chỉ cần kiểm tra xem có bao nhiêu giá trị như vậy phù hợp với`10^9`.$$3^{19}=1162261467>10^9,$$trong khi$$3^{18}=387420489<10^9.$$Bắt đầu từ`1`, chúng ta có thể sử dụng tối đa 19 số:$$3^0,3^1,\dots,3^{18}.$$Do đó một mảng hợp lệ chỉ tồn tại khi`n \le 19`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xác minh Brute Force của một ứng cử viên | O(n²) | O(1) | Không phải là công trình | 
| Quyền hạn 3 công trình | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`. 
2. Nếu`n > 19`, in`NO`. 

Không có công trình ba quyền hợp lệ nào phù hợp với giới hạn`10^9`và giải pháp chính thức dựa trên thực tế là không thể xây dựng hợp lệ lớn hơn trong giới hạn cho phép. 
3. Nếu không thì in`YES`. 
4. Tạo chuỗi$$1,3,9,27,\dots$$bằng cách nhân liên tục giá trị hiện tại với`3`. 
5. Xuất đầu tiên`n`các giá trị. 

Lý do công trình này hoạt động là vì mọi phần tử sau ít nhất gấp ba lần mọi phần tử trước đó. Đó chính xác là điều kiện thu được từ bất đẳng thức điều chỉnh xem tổng có giảm hay không. 

### Tại sao nó hoạt động 

Lấy hai phần tử bất kỳ của mảng được xây dựng và giả sử`x < y`. 

Bởi vì mảng là lũy thừa của ba nên mỗi phần tử lớn hơn ít nhất gấp ba lần phần tử nhỏ hơn:$$y \ge 3x.$$Sắp xếp lại mang lại$$y-x \ge \frac{x+y}{2}.$$Nhân hai,$$2(y-x)\ge x+y.$$Từ`y>x`, chúng tôi có`|x-y|=y-x`, Vì thế$$2|x-y|\ge x+y.$$Sự đóng góp của hai vị trí đó sau khi hoạt động ít nhất là sự đóng góp của họ trước khi hoạt động. Vì tất cả các vị trí khác không thay đổi nên tổng số tiền không bao giờ giảm. 

Giá trị sinh ra lớn nhất là`3^{18}=387420489`, thỏa mãn ràng buộc`10^9`. Công suất tiếp theo vượt quá giới hạn nên độ dài 19 là mức tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())

    if n > 19:
        print("NO")
        continue

    print("YES")

    cur = 1
    ans = []

    for _ in range(n):
        ans.append(cur)
        cur *= 3

    print(*ans)
```Chương trình theo sát việc xây dựng trực tiếp. 

Quyết định đầu tiên là liệu`n`vượt quá 19. Nếu có, chúng tôi sẽ in ngay`NO`. 

Để có độ dài khả thi, chúng tôi xây dựng trình tự lặp đi lặp lại. Bắt đầu từ`1`, mỗi giá trị mới thu được bằng cách nhân giá trị trước đó với`3`. Điều này tránh mọi lo ngại về lỗi số học hoặc lũy thừa dấu phẩy động. 

Tất cả số học dễ dàng phù hợp với số nguyên Python. Ngay cả giá trị được tạo lớn nhất cũng thấp hơn`10^9`, do đó không có mối lo ngại tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
```| Bước | Giá trị hiện tại | Mảng được tạo | 
| --- | --- | --- | 
| Bắt đầu | 1 | [] | 
| Thêm phần tử đầu tiên | 1 | [1] | 
| Nhân với 3 | 3 | [1] | 
| Thêm phần tử thứ hai | 3 | [1, 3] | 
| Nhân với 3 | 9 | [1, 3] | 
| Thêm phần tử thứ ba | 9 | [1, 3, 9] | 

Đầu ra:```
YES
1 3 9
```Kiểm tra các cặp:$$(1,3),\quad (1,9),\quad (3,9)$$Mỗi giá trị lớn hơn ít nhất gấp ba lần giá trị nhỏ hơn, do đó tổng không bao giờ giảm. 

### Ví dụ 2 

đầu vào:```
1
20
```| Bước | n | Quyết định | 
| --- | --- | --- | 
| Đọc test case | 20 | So sánh với 19 | 
| Kiểm tra giới hạn | 20 > 19 | Không thể | 
| Đầu ra | - | KHÔNG | 

Đầu ra:```
NO
```Dấu vết này thể hiện sự hạn chế về giá trị. Một số hạng thứ hai mươi sẽ vượt quá`10^9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tạo và in`n`số | 
| Không gian | O(n) | Lưu trữ mảng đã xây dựng | 

Từ`n ≤ 1000`và tồn tại nhiều nhất 100 trường hợp thử nghiệm, độ phức tạp này thấp hơn nhiều so với giới hạn sẵn có. Cấu trúc được chấp nhận thực tế không bao giờ tạo ra nhiều hơn 19 giá trị. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        if n > 19:
            out.append("NO")
            continue

        out.append("YES")

        cur = 1
        arr = []
        for _ in range(n):
            arr.append(str(cur))
            cur *= 3

        out.append(" ".join(arr))

    return "\n".join(out) + "\n"

# sample-style tests
assert run("1\n2\n") == "YES\n1 3\n"

# minimum valid size
assert run("1\n2\n") == "YES\n1 3\n"

# largest valid size
out = run("1\n19\n")
assert out.startswith("YES\n")

# first impossible size
assert run("1\n20\n") == "NO\n"

# multiple test cases
assert run("3\n2\n19\n20\n").startswith("YES\n1 3\nYES\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n2\n`|`YES`với hai giá trị | Độ dài tối thiểu cho phép | 
|`1\n19\n`|`YES`| Mảng xây dựng lớn nhất | 
|`1\n20\n`|`NO`| Ranh giới nơi giá trị vượt quá`10^9`| 
|`3\n2\n19\n20\n`| Câu trả lời hỗn hợp | Xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
```Các đầu ra thuật toán:```
YES
1 3
```Đối với cặp duy nhất,$$2|3-1| = 4,$$trong khi đóng góp ban đầu là$$1+3 = 4.$$Tổng không thay đổi, điều này được cho phép. 

Coi như:```
1
19
```Mảng được tạo kết thúc bằng$$3^{18}=387420489.$$Mọi phần tử đều nằm trong phạm vi cho phép. Đối với bất kỳ cặp nào, giá trị lớn hơn ít nhất gấp ba lần giá trị nhỏ hơn, do đó bất đẳng thức yêu cầu giữ nguyên. 

Coi như:```
1
20
```Sức mạnh tiếp theo sẽ là$$3^{19}=1162261467,$$vượt quá`10^9`. Thuật toán in chính xác`NO`thay vì tạo ra một mảng không hợp lệ. 

Hãy xem xét một cặp nằm sâu bên trong chuỗi, chẳng hạn như`81`Và`2187`. 

Từ$$2187 = 27 \cdot 81,$$chúng tôi có$$2187 \ge 3 \cdot 81.$$Như vậy$$2(2187-81)=4212,$$Và$$2187+81=2268.$$Đóng góp mới lớn hơn đóng góp cũ khẳng định tính bất biến được sử dụng trong chứng minh.
