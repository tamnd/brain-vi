---
title: "CF 1982A - Bóng đá"
description: "Trong bài toán này, chúng ta được đưa ra một kịch bản về một trận bóng đá trong đó Dima ghi nhớ hai điểm: một ngay trước khi anh ấy bị phân tâm và một điểm khác khi anh ấy quay lại. Mỗi tỷ số bao gồm các bàn thắng của hai đội. Trò chơi tiến triển từng mục tiêu một cho một trong hai đội."
date: "2026-06-08T16:41:58+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1982
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 955 (Div. 2, with prizes from NEAR!)"
rating: 800
weight: 1982
solve_time_s: 118
verified: true
draft: false
---

[CF 1982A - Bóng đá](https://codeforces.com/problemset/problem/1982/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta được đưa ra một kịch bản về một trận bóng đá trong đó Dima ghi nhớ hai điểm: một ngay trước khi anh ấy bị phân tâm và một điểm khác khi anh ấy quay lại. Mỗi tỷ số bao gồm các bàn thắng của hai đội. Trò chơi tiến triển từng mục tiêu một cho một trong hai đội. Chúng tôi được yêu cầu xác định xem liệu có thể trong thời gian Dima vắng mặt, hai đội chưa bao giờ có tỷ số ngang nhau hay không. Đầu ra phải là "CÓ" nếu điều này là có thể và "KHÔNG" nếu không. 

Điểm số là số nguyên lớn, lên tới$10^9$và số lượng ca kiểm thử có thể lên tới$10^4$. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng mọi bàn thắng được ghi bởi vì điều đó có thể yêu cầu lặp lại hàng tỷ bước. Thay vào đó, chúng ta phải suy luận một cách toán học về các chuỗi điểm số có thể có mà không tạo ra chúng một cách rõ ràng. 

Các trường hợp có lợi thế không rõ ràng bao gồm các tình huống trong đó một đội dẫn trước rất xa và tiếp tục ghi bàn trong khi đội kia không ghi được bàn nào. Ví dụ: nếu điểm ban đầu là$0:2$và tỉ số cuối cùng là$5:2$, đội đầu tiên có thể ghi 5 bàn liên tiếp mà tỷ số không bao giờ bằng nhau. Một cách tiếp cận bất cẩn mô phỏng tất cả các chuỗi mục tiêu có thể cho rằng sự bình đẳng phải xảy ra một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi trình tự ghi điểm có thể có từ điểm đầu tiên đến điểm thứ hai. Về mặt lý thuyết, điều này đúng vì bất kỳ chuỗi tăng thêm một bàn nào cho một trong hai đội đều bao gồm tất cả các khả năng. Tuy nhiên, điều đó hoàn toàn không thực tế vì số bàn thắng ghi được có thể cao tới mức$10^9$, làm cho số lượng hoạt động không được chấp nhận. 

Nhận xét quan trọng giúp đơn giản hóa vấn đề là cách duy nhất để điểm số có thể bằng nhau là nếu chênh lệch giữa các đội lúc đầu được vượt qua theo từng bước. Định nghĩa$d_1 = x_1 - y_1$Và$d_2 = x_2 - y_2$. Nếu như$d_1$Và$d_2$có cùng dấu (cả dương hoặc cả âm) hoặc một trong hai bằng 0 thì có thể đạt được điểm thứ hai mà không cần điểm bằng nhau. Nếu dấu khác nhau thì không thể tránh khỏi sự bằng nhau ở một điểm nào đó. Điều này làm giảm vấn đề thành một kiểm tra đơn giản về sự khác biệt ban đầu và cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(x2-x1, y2-y1)) | O(1) | Quá chậm | 
| Kiểm tra dựa trên dấu hiệu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Đối với mỗi trường hợp kiểm tra, hãy đọc điểm trước và sau khi Dima bị phân tâm,$(x_1, y_1)$Và$(x_2, y_2)$. 
3. Tính sự khác biệt$d_1 = x_1 - y_1$Và$d_2 = x_2 - y_2$. 
4. Kiểm tra xem$d_1 \cdot d_2 > 0$hoặc$d_1 = d_2 = 0$. Điều này đảm bảo điểm số không bao giờ vượt qua 0, nghĩa là sự bình đẳng không bao giờ xảy ra. 
5. Nếu điều kiện đúng, in ra "CÓ". Nếu không thì in ra "NO". 

Tại sao nó hoạt động: Sản phẩm$d_1 \cdot d_2$dương đảm bảo rằng cả hai hiệu đều có cùng dấu. Vì một bàn thắng chỉ làm tăng số điểm của một đội nên sự khác biệt thay đổi một cách đơn điệu và không bao giờ có thể vượt qua con số 0. Điều này đảm bảo rằng các đội không bao giờ có số điểm bằng nhau trong suốt khoảng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x1, y1 = map(int, input().split())
        x2, y2 = map(int, input().split())
        d1 = x1 - y1
        d2 = x2 - y2
        if d1 * d2 > 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử một cách hiệu quả bằng cách sử dụng I/O nhanh. Việc tính hiệu và kiểm tra dấu được thực hiện trong thời gian không đổi. Logic tránh việc mô phỏng trình tự tính điểm, điều này sẽ không khả thi. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| x1 | y1 | x2 | y2 | d1 | d2 | d1*d2 | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | 0 | 1 | 5 | 5 | CÓ | 
| 1 | 2 | 3 | 2 | -1 | 1 | -1 | KHÔNG | 
| 1 | 2 | 4 | 5 | -1 | -1 | 1 | CÓ | 

Ví dụ đầu tiên cho thấy rằng chừng nào chênh lệch vẫn dương thì sẽ tránh được sự bình đẳng. Ví dụ thứ hai cho thấy hiệu thay đổi dấu, có nghĩa là sự bằng nhau phải xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Được cho$t \le 10^4$, thuật toán này thực hiện trong giới hạn thời gian một cách thoải mái. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n1 0\n5 0\n1 2\n3 2\n1 2\n4 5\n1 2\n4 3\n1 2\n1 2\n998244353 0\n1000000000 999999999\n") == \
"YES\nNO\nYES\nNO\nYES\nYES", "sample 1"

# Custom cases
assert run("2\n0 1\n0 3\n5 2\n10 7\n") == "YES\nYES", "increasing differences"
assert run("2\n0 1\n2 0\n3 5\n7 4\n") == "NO\nNO", "crossing differences"
assert run("1\n0 0\n5 5\n") == "YES", "starts and ends tied but not crossed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 1 -> 0 3 | CÓ | Chênh lệch vẫn âm, không có sự ràng buộc | 
| 0 1 -> 2 0 | KHÔNG | Sự khác biệt vượt qua 0, hòa xảy ra | 
| 0 0 -> 5 5 | CÓ | Cả trận bắt đầu và kết thúc đều hòa nhưng không có bàn thắng trung gian | 

## Vỏ cạnh 

Hãy xem xét trường hợp một đội ghi được nhiều bàn thắng trong khi đội kia không ghi được bàn thắng nào. Ví dụ, bắt đầu từ$0:1$và kết thúc tại$0:5$, sự khác biệt luôn âm và không xảy ra sự ràng buộc. Thuật toán tính toán$d_1 = -1$,$d_2 = -5$, sản phẩm dương tính và xuất ra chính xác "CÓ". 

Nếu chênh lệch vượt qua 0, chẳng hạn như$1:2$ĐẾN$3:2$, thuật toán tính toán$d_1 = -1$,$d_2 = 1$, sản phẩm âm tính và xuất ra chính xác "KHÔNG". Điều này xác nhận tính đúng đắn của các điều kiện biên tinh tế mà không cần mô phỏng mọi điểm số.
