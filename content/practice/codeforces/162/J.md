---
title: "CF 162J - Chân Đế"
description: "Chúng ta được cung cấp một chuỗi chỉ bao gồm các dấu ngoặc tròn mở và đóng. Nhiệm vụ là xác định xem chuỗi này có cân bằng hay không, nghĩa là nó có thể biểu thị sự sắp xếp chính xác của dấu ngoặc đơn trong biểu thức toán học."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "J"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 64
verified: true
draft: false
---

[CF 162J - Dấu ngoặc](https://codeforces.com/problemset/problem/162/J) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chỉ bao gồm các dấu ngoặc tròn mở và đóng. Nhiệm vụ là xác định xem chuỗi này có cân bằng hay không, nghĩa là nó có thể biểu thị sự sắp xếp chính xác của dấu ngoặc đơn trong biểu thức toán học. Nói cách khác, mỗi dấu ngoặc mở phải có một dấu ngoặc đóng phù hợp và các dấu ngoặc phải được lồng vào nhau đúng cách. Ví dụ: "(())()" được cân bằng vì mỗi "(" được đóng theo đúng thứ tự, trong khi "(()" thì không, vì một dấu ngoặc mở vẫn không khớp và ")(" không phải vì dấu ngoặc đóng xuất hiện trước bất kỳ dấu ngoặc mở phù hợp nào. 

Độ dài chuỗi đầu vào được đảm bảo nằm trong khoảng từ 1 đến 100 ký tự. Giới hạn trên nhỏ này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp thời gian tuyến tính hoặc tốt hơn đều được chấp nhận. Các giải pháp bậc hai hoặc phức tạp hơn cũng sẽ chạy đủ nhanh do giới hạn thấp, nhưng ưu tiên tính đơn giản và rõ ràng. Các trường hợp cạnh cần lưu ý bao gồm các chuỗi bắt đầu bằng dấu ngoặc đóng, các chuỗi kết thúc bằng dấu ngoặc mở, các chuỗi có dấu ngoặc không khớp ở giữa và các chuỗi bao gồm hoàn toàn một loại dấu ngoặc. Ví dụ: chuỗi ")" sẽ trả về "NO" vì dấu ngoặc đóng không thể xuất hiện nếu không có dấu ngoặc mở trước đó. Tương tự, "(((" sẽ trả về "KHÔNG" vì các dấu ngoặc mở bổ sung vẫn không khớp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng kiểm tra mọi chuỗi con có thể để xem liệu nó có tạo thành một chuỗi dấu ngoặc hợp lệ hay không. Điều này liên quan đến việc ghép nối đệ quy hoặc lặp đi lặp lại từng dấu ngoặc mở với dấu ngoặc đóng và xác minh tất cả các chuỗi lồng nhau. Mặc dù đúng nhưng cách tiếp cận này có độ phức tạp theo thời gian theo cấp số nhân, O(2^n) trong trường hợp xấu nhất và không cần thiết do có các ràng buộc. Rõ ràng nó trở nên quá mức cần thiết ngay cả đối với một chuỗi có độ dài từ 20 trở lên, khi số lượng séc tăng lên một cách bùng nổ. 

Cách tiếp cận tối ưu tận dụng cơ chế đếm đơn giản sử dụng một số nguyên duy nhất làm bộ đếm số dư. Chúng ta lặp qua chuỗi từ trái sang phải, tăng bộ đếm cho mỗi dấu ngoặc mở và giảm nó cho mỗi dấu ngoặc đóng. Nếu bộ đếm chuyển sang giá trị âm, điều đó cho biết rằng dấu ngoặc đóng xuất hiện mà không có dấu ngoặc mở phù hợp, do đó chuỗi ngay lập tức không hợp lệ. Sau khi xử lý toàn bộ chuỗi, chuỗi chỉ được cân bằng nếu bộ đếm bằng 0, nghĩa là tất cả các dấu ngoặc mở đều khớp với dấu ngoặc đóng. Cách tiếp cận này hoạt động trong thời gian O(n) và không gian O(1) vì chúng ta chỉ cần một số nguyên duy nhất để duy trì sự cân bằng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến đếm về 0. Bộ đếm này sẽ theo dõi số lượng thực của các dấu ngoặc mở chưa khớp khi chúng tôi quét chuỗi. 
2. Lặp lại từng ký tự trong chuỗi. Nếu ký tự là dấu ngoặc mở "(", tăng bộ đếm vì chúng ta có thêm một dấu ngoặc mở không khớp. Nếu ký tự là dấu ngoặc đóng ")", hãy giảm bộ đếm vì một dấu ngoặc mở không khớp đã khớp. 
3. Nếu tại bất kỳ thời điểm nào trong quá trình lặp, bộ đếm trở nên âm, ngay lập tức in "NO" và kết thúc. Điều này chỉ ra rằng dấu ngoặc đóng xảy ra trước dấu ngoặc mở tương ứng, làm cho chuỗi không cân bằng. 
4. Sau khi duyệt toàn bộ chuỗi, hãy kiểm tra bộ đếm. Nếu nó chính xác bằng 0, hãy in "CÓ" vì mọi dấu ngoặc mở đều có dấu ngoặc đóng tương ứng. Nếu không, hãy in "KHÔNG" vì một số dấu ngoặc mở vẫn chưa khớp.

Tại sao nó hoạt động: bộ đếm duy trì một bất biến đại diện cho số lượng dấu ngoặc mở hiện chưa khớp. Giá trị âm báo hiệu trạng thái không hợp lệ và giá trị 0 ở cuối xác nhận rằng mọi dấu ngoặc đều được khớp và lồng đúng cách. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
balance = 0

for char in s:
    if char == '(':
        balance += 1
    else:  # char == ')'
        balance -= 1
    if balance < 0:
        print("NO")
        sys.exit()

print("YES" if balance == 0 else "NO")
```Giải pháp đọc đầu vào và loại bỏ các dòng mới ở cuối. Bộ đếm số dư bắt đầu từ số 0. Với mỗi ký tự, chúng ta điều chỉnh bộ đếm theo loại dấu ngoặc. Nếu số dư âm tại bất kỳ điểm nào, chúng tôi ngay lập tức in "KHÔNG" và kết thúc, phản ánh rằng chuỗi không thể cân bằng. Sau vòng lặp, việc kiểm tra xem số dư có bằng 0 hay không để đảm bảo rằng tất cả các dấu ngoặc đều khớp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1: "(()(()))()" 

| Bước | Char | Số dư | Hành động | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | Tăng cho '(' | 
| 2 | ( | 2 | Tăng cho '(' | 
| 3 | ) | 1 | Giảm cho ')' | 
| 4 | ( | 2 | Tăng cho '(' | 
| 5 | ( | 3 | Tăng cho '(' | 
| 6 | ) | 2 | Giảm cho ')' | 
| 7 | ) | 1 | Giảm cho ')' | 
| 8 | ) | 0 | Giảm cho ')' | 
| 9 | ( | 1 | Tăng cho '(' | 
| 10 | ) | 0 | Giảm cho ')' | 

Số dư kết thúc ở mức 0, do đó đầu ra là "CÓ". Điều này xác nhận thuật toán theo dõi chính xác các cặp lồng nhau và kết quả khớp. 

Đầu vào mẫu 2: "(()" 

| Bước | Char | Số dư | Hành động | 
| --- | --- | --- | --- | 
| 1 | ( | 1 | Tăng cho '(' | 
| 2 | ( | 2 | Tăng cho '(' | 
| 3 | ) | 1 | Giảm cho ')' | 

Số dư kết thúc ở mức 1, biểu thị một '(' chưa khớp. Đầu ra là "KHÔNG", xác định chính xác chuỗi không cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng ta lặp qua từng ký tự một lần. | 
| Không gian | O(1) | Chỉ có một bộ đếm số nguyên duy nhất được sử dụng. | 

Cho n ≤ 100, giải pháp O(n) chạy thoải mái trong giới hạn 3 giây và không gian O(1) đảm bảo hiệu quả bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        s = input().strip()
        balance = 0
        for char in s:
            if char == '(':
                balance += 1
            else:
                balance -= 1
            if balance < 0:
                print("NO")
                return
        print("YES" if balance == 0 else "NO")
    return out.getvalue().strip()

# Provided sample
assert run("(()(()))()") == "YES", "sample 1"

# Custom cases
assert run("(") == "NO", "single opening"
assert run(")") == "NO", "single closing"
assert run("()()()") == "YES", "repeated balanced pairs"
assert run("((()))") == "YES", "nested balanced"
assert run("((())") == "NO", "unmatched opening inside"
assert run("())(") == "NO", "early closing unmatched"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "(" | KHÔNG | Mở màn duy nhất chưa từng có | 
| ")" | KHÔNG | Chốt đơn chưa từng có | 
| "()()()" | CÓ | Nhiều cặp cân bằng | 
| "((()))" | CÓ | Dấu ngoặc cân bằng lồng nhau | 
| "((())" | KHÔNG | Độ mở chưa từng có bên trong | 
| "())(" | KHÔNG | Đóng trước khi mở | 

## Vỏ cạnh 

Đối với chuỗi bắt đầu bằng dấu ngoặc đóng, ví dụ: ")", số dư ngay lập tức trở thành âm ở bước 1, kích hoạt "KHÔNG". Đối với một chuỗi kết thúc bằng các phần mở không khớp như "(((", số dư cuối cùng là dương, dẫn đến "KHÔNG". Trong một chuỗi được lồng hoàn hảo như "((()))", bộ đếm tăng và giảm một cách chính xác, kết thúc bằng 0, xác nhận "CÓ". Thuật toán xử lý chính xác tất cả các trường hợp có kết thúc sớm và kiểm tra số dư cuối cùng.
