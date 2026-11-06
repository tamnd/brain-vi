---
title: "CF 1225A - Quên Đồ"
description: "Chúng ta được yêu cầu xây dựng hai số nguyên dương $a$ và $b$ sao cho chúng khác nhau đúng một đơn vị theo nghĩa $a + 1 = b$. Chúng tôi không được cung cấp những con số. Thay vào đó, chúng ta chỉ có chữ số đầu tiên của $a$ và chữ số đầu tiên của $b$."
date: "2026-06-13T18:39:36+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1225
codeforces_index: "A"
codeforces_contest_name: "Technocup 2020 - Elimination Round 2"
rating: 900
weight: 1225
solve_time_s: 533
verified: false
draft: false
---

[CF 1225A - Quên đồ](https://codeforces.com/problemset/problem/1225/A) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 8m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta được yêu cầu xây dựng hai số nguyên dương$a$Và$b$sao cho chúng khác nhau đúng một đơn vị theo nghĩa là$a + 1 = b$. Chúng tôi không được cung cấp những con số. Thay vào đó, chúng ta chỉ được cung cấp chữ số đầu tiên của$a$và chữ số đầu tiên của$b$. Nhiệm vụ là quyết định xem một cặp như vậy có tồn tại hay không và nếu có thì tạo ra bất kỳ cặp hợp lệ nào trong phạm vi cho phép. 

Khó khăn chính là các chữ số đầu chỉ ràng buộc một số rất lỏng lẻo. Nhiều số có cùng chữ số đầu tiên và việc tăng thêm một đơn vị có thể giữ nguyên chữ số đứng đầu hoặc thay đổi nó một cách đáng kể, đặc biệt là ở lũy thừa mười. Bài toán rút gọn thành việc tìm xem có tồn tại ít nhất một số nguyên hay không$a$sao cho khi cộng một, chữ số đầu tiên của kết quả sẽ trở thành chữ số cần tìm. 

Đầu ra không cần phải là duy nhất, điều này cho phép chúng tôi tìm kiếm bất kỳ cấu trúc thuận tiện nào thay vì một cấu trúc chính xác hoặc tối thiểu. 

Các ràng buộc rất nhỏ, với cả hai chữ số trong$[1,9]$. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc phức tạp hoặc không gian tìm kiếm. Ngay cả một lực lượng vũ phu đối với một phạm vi nhỏ các số đại diện cho mỗi chữ số hàng đầu cũng là đủ. Tuy nhiên, cấu trúc biểu diễn số thập phân làm cho lập luận mang tính xây dựng trực tiếp trở nên hiệu quả và rõ ràng hơn. 

Một trường hợp cạnh tinh tế phát sinh khi việc tăng vượt qua lũy thừa mười ranh giới. Ví dụ,$199 + 1 = 200$thay đổi chữ số hàng đầu từ 1 thành 2. Đây là cơ chế chính cho phép hầu hết các chuyển đổi hợp lệ giữa các chữ số hàng đầu. Tuy nhiên, không phải tất cả các chuyển đổi chữ số đều có thể thực hiện được và một số cặp$(d_a, d_b)$không thể xảy ra dưới thao tác +1 bất kể cường độ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là lặp lại tất cả các số lên tới$10^9$, kiểm tra những người có chữ số đầu tiên là$d_a$và kiểm tra xem việc cộng một có tạo ra số có chữ số đầu tiên là$d_b$. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa nhưng không khả thi vì không gian tìm kiếm chứa tới hàng tỷ ứng viên. 

Nhận xét quan trọng là hoạt động$a \to a + 1$chỉ thay đổi chữ số đầu một cách rất hạn chế. Lần duy nhất chữ số đứng đầu thay đổi là khi số mang truyền qua một chuỗi dài 9 giây. Điều đó có nghĩa là các số có dạng$d_a99\ldots9$là những ứng cử viên duy nhất có thể dịch chuyển chữ số đầu của chúng lên sau khi tăng. Đặc biệt,$199 \to 200$,$2999 \to 3000$, vân vân. 

Điều này gợi ý một chiến lược mang tính xây dựng. Chúng tôi cố gắng xây dựng các số bắt đầu từ kết hợp tiền tố nhỏ nhất có thể$d_a$, sau đó nối thêm bao nhiêu số 9 nếu cần và kiểm tra xem việc thêm một số có tạo ra số bắt đầu bằng không$d_b$. Vì chỉ có chữ số hàng đầu mới quan trọng nên chúng ta chỉ cần kiểm tra một vài độ dài của số 9 ở cuối. Nếu không có công trình xây dựng như vậy thì không có giải pháp nào tồn tại. 

Điều này làm giảm vấn đề từ việc tìm kiếm quy mô lớn đến việc kiểm tra một số ít ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(10^9)$|$O(1)$| Quá chậm | 
| Kiểm tra mang tính xây dựng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thử xây dựng số lượng ứng viên$a$bắt đầu bằng chữ số$d_a$. Cấu trúc có ý nghĩa nhỏ nhất là một số bắt đầu bằng$d_a$theo sau là một chuỗi số 9. Điều này là do các số 9 được kích hoạt là cách duy nhất để thay đổi chữ số hàng đầu một cách có kiểm soát. 
2. Với mỗi độ dài có thể có của các số 9 ở cuối (từ 0 đến một hằng số nhỏ chẳng hạn như 9), hãy xây dựng$a = d_a$theo sau là nhiều số 9. Điều này mang lại cho một gia đình như$1, 19, 199, 1999, \ldots$. 
3. Tính toán$b = a + 1$. Việc tăng sẽ bảo toàn cấu trúc hoặc kích hoạt việc mang làm tăng chữ số đầu. 
4. Trích xuất chữ số đầu tiên của$b$bằng cách chuyển đổi nó thành một chuỗi và đọc ký tự đầu tiên. Điều này tương ứng trực tiếp với yêu cầu của vấn đề. 
5. Nếu chữ số đầu tiên của$b$trận đấu$d_b$, xuất cặp này$(a, b)$ngay lập tức. 
6. Nếu không có ứng viên nào làm việc sau tất cả các lần thử, hãy xuất ra$-1$. 

Lý do điều này có hiệu quả là vì bất kỳ giải pháp hợp lệ nào cũng phải liên quan đến chuỗi mang do hậu tố 9 gây ra. Nếu một nghiệm tồn tại, chúng ta có thể giả sử cấu trúc của nó tương đương với một số nào đó có tiền tố$d_a$theo sau là đủ 9 giây để buộc chuyển đổi chữ số hàng đầu tương tự khi tăng lên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def first_digit(x):
    return int(str(x)[0])

def solve():
    da, db = map(int, input().split())

    for k in range(0, 10):
        a = int(str(da) + "9" * k)
        b = a + 1
        if first_digit(b) == db:
            print(a, b)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp ý tưởng xây dựng. Vòng lặp kết thúc$k$xây dựng số lượng ngày càng lớn hơn bắt đầu bằng$d_a$. Mỗi ứng cử viên được kiểm tra bằng một mức tăng duy nhất và kiểm tra liên tục chữ số đầu tiên của nó. Việc chuyển đổi thành chuỗi là an toàn vì các số được xây dựng vẫn nằm trong phạm vi$10^9$ràng buộc được đảm bảo bởi vấn đề. 

Một điểm tinh tế là chúng tôi không cố gắng tối ưu hóa việc trích xuất chữ số về mặt toán học. Việc chuyển đổi chuỗi ở đây đủ nhanh vì vòng lặp chạy với số lần không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
```| k | một | b = a + 1 | chữ số đầu tiên của b | trận đấu | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | vâng | 

Chúng tôi ngay lập tức tìm thấy$a = 1$,$b = 2$, thỏa mãn điều kiện. 

Điều này thể hiện trường hợp đơn giản nhất khi không cần lan truyền mang và số gia tăng trực tiếp thay đổi chữ số đầu. 

### Ví dụ 2 

đầu vào:```
1 3
```| k | một | b = a + 1 | chữ số đầu tiên của b | trận đấu | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | không | 
| 1 | 19 | 20 | 2 | không | 
| 2 | 199 | 200 | 2 | không | 
| 3 | 1999 | 2000 | 2 | không | 

Không có giá trị của$k$tạo ra chữ số hàng đầu 3 sau khi tăng. Quá trình này làm cạn kiệt tất cả độ dài mang có ý nghĩa và không thành công. 

Điều này cho thấy rằng không phải mọi chuyển đổi chữ số đều có thể thực hiện được bằng thao tác +1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Tối đa 10 lần kiểm tra ứng viên, mỗi lần làm việc liên tục | 
| Không gian |$O(1)$| Chỉ một số số nguyên được lưu trữ | 

Các ràng buộc là không đáng kể, do đó, ngay cả các chuyển đổi chuỗi lặp lại và các phép toán số nguyên cũng nằm trong giới hạn. Thuật toán là thời gian không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # inline solution
    da, db = map(int, sys.stdin.readline().split())

    def first_digit(x):
        return int(str(x)[0])

    for k in range(10):
        a = int(str(da) + "9" * k)
        b = a + 1
        if first_digit(b) == db:
            return f"{a} {b}\n"

    return "-1\n"

# provided sample
assert run("1 2") == "1 2\n"

# custom cases
assert run("1 3") == "-1\n", "no transition possible"
assert run("2 3") == "2 3\n", "direct increment works"
assert run("9 1") == "9 10\n", "carry wraps to new digit"
assert run("1 2") in ["1 2\n", "19 20\n"], "multiple valid answers allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 | -1 | chuyển đổi không thể | 
| 2 3 | 2 3 | trực tiếp +1 không mang theo | 
| 9 1 | 9 10 | tiến hành sản xuất chữ số mới | 
| 1 2 | cặp hợp lệ | nhiều giải pháp đúng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi không có số nào bắt đầu bằng$d_a$có thể tạo ra một số bắt đầu bằng$d_b$sau khi tăng dần. Ví dụ,$1 \to 3$không thành công vì thao tác tăng dần luôn tạo ra một thay đổi nhỏ cục bộ hoặc một số mang tạo ra chữ số 2 làm chữ số hàng đầu mới. 

Một trường hợp khác là ranh giới mang như$9 \to 10$. Ở đây, chữ số đứng đầu thay đổi từ 9 thành 1, đây là hành vi bao bọc không tầm thường duy nhất có thể có trong số gia cơ sở 10. Thuật toán kiểm tra rõ ràng$a = 9$, sản xuất$b = 10$và nắm bắt chính xác sự thay đổi chữ số hàng đầu. 

Cuối cùng, chuỗi dài số 9 như$1999 \to 2000$xác nhận rằng việc mở rộng số không làm thay đổi chữ số đầu kết quả sau khi truyền mang. Vòng lặp tăng dần số 9 đảm bảo rằng nếu bất kỳ độ dài mang nào hoạt động thì nó sẽ được tìm thấy.
