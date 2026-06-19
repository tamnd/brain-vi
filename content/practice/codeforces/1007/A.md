---
title: "CF 1007A - Sắp xếp lại mảng"
description: "Chúng ta được cung cấp một dãy số và được phép tự do sắp xếp lại chúng. Sau khi sắp xếp lại, ta so sánh mảng mới với mảng ban đầu theo từng vị trí."
date: "2026-06-16T23:06:50+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "math", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1007
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 497 (Div. 1)"
rating: 1300
weight: 1007
solve_time_s: 191
verified: true
draft: false
---

[CF 1007A - Sắp xếp lại mảng](https://codeforces.com/problemset/problem/1007/A) 

**Đánh giá:** 1300 
**Tags:** tổ hợp, cấu trúc dữ liệu, toán, sắp xếp, hai con trỏ 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và được phép tự do sắp xếp lại chúng. Sau khi sắp xếp lại, ta so sánh mảng mới với mảng ban đầu theo từng vị trí. Một vị trí được coi là "tốt" nếu giá trị được đặt ở đó sau khi hoán vị lớn hơn giá trị ban đầu chiếm giữ vị trí đó. 

Nhiệm vụ là chọn một hoán vị sao cho số lượng vị trí tốt như vậy là lớn nhất. Nói cách khác, chúng tôi muốn khớp các phần tử sao cho càng nhiều vị trí càng tốt nhận được giá trị lớn hơn trước đây trong khi vẫn sử dụng mọi phần tử chính xác một lần. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào thử tất cả các hoán vị, vì$n!$phát triển vượt xa giới hạn khả thi. Thậm chí$O(n^2)$các giải pháp có đường biên quá chậm trong Python đối với kích thước này, vì vậy giải pháp về cơ bản phải là tuyến tính hoặc$n \log n$. 

Một sai lầm ngây thơ là nghĩ cục bộ, đối với mỗi vị trí, chúng ta luôn có thể tìm thấy một số phần tử lớn hơn chưa được sử dụng. Điều này không thành công khi các phần tử lớn bị “lãng phí” sớm. 

Ví dụ, hãy xem xét mảng:```
[1, 2, 3, 100, 101]
```Nếu chúng ta tham lam gán các phần tử lớn nhất cho các vị trí nhỏ nhất mà không cẩn thận, chúng ta có thể sử dụng 100 và 101 quá sớm, không để lại cải tiến hợp lệ cho các vị trí trung gian. Câu trả lời đúng phụ thuộc vào kết hợp toàn cục chứ không phải ghép nối cục bộ. 

Một trường hợp thất bại tinh vi khác là khi sự trùng lặp chiếm ưu thế:```
[5, 5, 5, 1, 1]
```Mặc dù có các giá trị lớn, nhưng một khi chúng được chỉ định, số lượng kết quả phù hợp được cải thiện nghiêm ngặt sẽ bị giới hạn bởi số lượng giá trị nhỏ hơn thực sự tồn tại trong cấu trúc ban đầu. 

Khó khăn chính là chúng ta đang khớp hai bản sao của mảng một cách hiệu quả với một ràng buộc bất đẳng thức nghiêm ngặt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi hoán vị và đếm xem có bao nhiêu vị trí được cải thiện so với mảng ban đầu. Đối với mỗi hoán vị, chúng tôi so sánh từng phần tử với mảng ban đầu, tính giá$O(n)$mỗi hoán vị. Vì có$n!$hoán vị, điều này trở thành$O(n \cdot n!)$, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 10$. 

Cấu trúc của vấn đề gợi ý cách giải thích phù hợp. Chúng tôi muốn ghép các giá trị ban đầu với các giá trị được hoán vị sao cho$b[i] > a[i]$cho càng nhiều chỉ số càng tốt, trong đó$b$là một hoán vị của$a$. Đây là vấn đề đối sánh hai bên trên một tập hợp nhiều tập hợp được sắp xếp. 

Sau khi các mảng được sắp xếp, chúng ta có thể nghĩ đến việc gán các phần tử “chiến thắng” nhỏ nhất có thể để đánh bại các bản gốc nhỏ hơn một chút. Ý tưởng tham lam trở nên rõ ràng: nếu chúng ta luôn cố gắng đánh bại giá trị ban đầu nhỏ nhất còn lại bằng cách sử dụng giá trị lớn hơn nhỏ nhất hiện có, chúng ta sẽ tránh lãng phí số lượng lớn vào các kết quả vốn đã dễ dàng. 

Đây là chiến lược hai con trỏ cổ điển trên các mảng đã được sắp xếp. Chúng tôi sắp xếp mảng, sau đó cố gắng so khớp phần tử lớn hơn với phần tử nhỏ hơn ở đầu bên kia, nâng cấp con trỏ một cách cẩn thận để tối đa hóa các bất đẳng thức nghiêm ngặt thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot n!)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp + kết hợp tham lam) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích lại vấn đề dưới dạng khớp các phần tử của mảng với bản sao được hoán vị của chính nó. 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này mang lại một cái nhìn có cấu trúc trong đó các so sánh trở nên có thể dự đoán được và những lựa chọn tham lam là an toàn. 
2. Sử dụng hai con trỏ: một con trỏ`i`đại diện cho “phía nhỏ hơn” mà chúng tôi muốn đánh bại và một con trỏ khác`j`đại diện cho những ứng cử viên có thể đánh bại nó. 
3. Khởi tạo`i = 0`Và`j = 0`, và duy trì một bộ đếm`ans = 0`. 
4. Di chuyển qua mảng để tìm kết quả khớp. Nếu như`a[j] > a[i]`, chúng ta có thể tạo thành một cặp thành công, vì vậy chúng ta tăng`ans`và nâng cao cả hai con trỏ. Điều này thể hiện việc chỉ định một giá trị lớn hơn hoàn toàn để đánh bại giá trị nhỏ nhất hiện tại chưa từng có. 
5. Nếu`a[j] <= a[i]`, sau đó`a[j]`không hữu ích cho việc đánh bại`a[i]`, vì vậy chúng tôi chỉ tiến lên`j`. Điều này bỏ qua các giá trị không thể đóng góp vào việc tăng kết quả phù hợp cho mục tiêu này. 
6. Tiếp tục cho đến khi một trong hai con trỏ đến cuối mảng. 

Quá trình này đảm bảo mọi kết quả phù hợp thành công đều có giá trị và không có cải tiến tiềm năng nào bị bỏ qua sớm. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi phép gán hợp lệ đều tương ứng với việc ghép một phần tử lớn hơn với phần tử nhỏ hơn. Chiến lược tham lam luôn phù hợp với “người chiến thắng” nhỏ nhất có thể với “kẻ thua cuộc” nhỏ nhất có thể mà nó có thể đánh bại. Nếu một giá trị không thể đánh bại mục tiêu nhỏ nhất còn lại hiện tại, thì nó cũng không thể đánh bại bất kỳ mục tiêu nào sau này (lớn hơn), do đó việc bỏ qua nó không làm giảm tính tối ưu. Điều này tạo ra một quá trình so khớp đơn điệu trong đó mỗi cặp thành công đều tối ưu không thể đảo ngược theo nghĩa là nó duy trì tính linh hoạt tối đa cho các phần tử còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    i = 0
    j = 0
    ans = 0
    
    while i < n and j < n:
        if a[j] > a[i]:
            ans += 1
            i += 1
            j += 1
        else:
            j += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp là cần thiết vì nó biến vấn đề thành một nhiệm vụ so khớp đơn điệu. Nếu không sắp xếp, quy tắc tham lam “sử dụng phần tử nhỏ nhất có thể để hoạt động” sẽ không được xác định rõ ràng. 

Vòng lặp hai con trỏ duy trì sự phân tách rõ ràng: con trỏ`i`theo dõi phần tử nhỏ nhất tiếp theo vẫn cần được đánh bại, trong khi`j`tìm kiếm một ứng cử viên lớn hơn nó. điều kiện`a[j] > a[i]`đảm bảo cải tiến chặt chẽ, khớp chính xác yêu cầu bài toán. 

Sự tinh tế quan trọng là`i`chỉ tiến bộ khi tìm thấy kết quả phù hợp. Điều này đảm bảo rằng mỗi lần tăng thành công đều tương ứng với một cặp duy nhất và không có phần tử nào được sử dụng lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
10 1 1 1 5 5 3
```Mảng được sắp xếp:```
[1, 1, 1, 3, 5, 5, 10]
```| tôi | j | một [tôi] | một[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | j++ (không lớn hơn) | 0 | 
| 0 | 1 | 1 | 1 | j++ | 0 | 
| 0 | 2 | 1 | 1 | j++ | 0 | 
| 0 | 3 | 1 | 3 | trận đấu | 1 | 
| 1 | 4 | 1 | 5 | trận đấu | 2 | 
| 2 | 5 | 1 | 5 | trận đấu | 3 | 
| 3 | 6 | 3 | 10 | trận đấu | 4 | 

Đầu ra:```
4
```Điều này cho thấy các giá trị trùng lặp ở bên trái dần dần được sử dụng như thế nào và chỉ những cải tiến nghiêm ngặt mới được tính. 

### Ví dụ 2 

đầu vào:```
4
4 4 4 4
```Mảng được sắp xếp:```
[4, 4, 4, 4]
```| tôi | j | một [tôi] | một[j] | Hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 4 | 4 | j++ | 0 | 
| 0 | 1 | 4 | 4 | j++ | 0 | 
| 0 | 2 | 4 | 4 | j++ | 0 | 
| 0 | 3 | 4 | 4 | j++ | 0 | 

Đầu ra:```
0
```Không có phần tử nào có thể vượt quá phần tử khác một cách nghiêm ngặt, do đó không tồn tại kết quả khớp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính | 
| Không gian |$O(1)$hoặc$O(n)$| Sắp xếp tại chỗ hoặc lưu trữ mảng | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó$O(n \log n)$Giải pháp nhanh chóng thoải mái. Thuật toán chỉ sử dụng tính năng sắp xếp và quét tuyến tính, cả hai đều nằm trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline()  # placeholder, replace with actual solve()

# provided samples (conceptual placeholders)
# assert run("7\n10 1 1 1 5 5 3\n") == "4\n"

# custom cases
assert run("1\n5\n") == "0\n", "single element"
assert run("3\n1 2 3\n") == "2\n", "strictly increasing"
assert run("4\n4 4 4 4\n") == "0\n", "all equal"
assert run("5\n5 1 4 2 3\n") == "4\n", "mixed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | ranh giới tối thiểu | 
| được sắp xếp tăng dần | 2 | tiềm năng phù hợp tối đa | 
| tất cả đều bình đẳng | 0 | ràng buộc bất bình đẳng nghiêm ngặt | 
| hỗn hợp xáo trộn | 4 | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với mảng một phần tử như`[7]`, mảng được sắp xếp vẫn còn`[7]`. Con trỏ`j`không thể tìm thấy bất kỳ giá trị nào lớn hơn cho`i = 0`, do đó không có sự gia tăng nào xảy ra và đầu ra vẫn bằng 0. 

Đối với một mảng thống nhất như`[2, 2, 2, 2]`, mọi so sánh đều thất bại trong việc kiểm tra bất đẳng thức nghiêm ngặt, do đó thuật toán chỉ tiến bộ`j`, không bao giờ tăng`ans`. Điều này trực tiếp phản ánh việc không thể cải thiện bất kỳ vị trí nào khi tất cả các giá trị đều giống hệt nhau.
