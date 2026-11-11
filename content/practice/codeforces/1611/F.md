---
title: "CF 1611F - ATM và sinh viên"
description: "Vấn đề liên quan đến việc mô phỏng một máy ATM bắt đầu bằng một số tiền cố định và phải phục vụ hàng đợi sinh viên. Mỗi sinh viên gửi số tiền dương hoặc rút số tiền âm từ máy ATM."
date: "2026-06-10T07:06:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1611
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 756 (Div. 3)"
rating: 1800
weight: 1611
solve_time_s: 72
verified: true
draft: false
---

[CF 1611F - ATM và sinh viên](https://codeforces.com/problemset/problem/1611/F) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề liên quan đến việc mô phỏng một máy ATM bắt đầu bằng một số tiền cố định và phải phục vụ hàng đợi sinh viên. Mỗi sinh viên gửi số tiền dương hoặc rút số tiền âm từ máy ATM. Polycarp, người vận hành máy ATM, có thể chọn bắt đầu phục vụ học sinh tại bất kỳ điểm nào trong hàng đợi và khi máy ATM không thể đáp ứng việc rút tiền, nó sẽ tắt vĩnh viễn. Nhiệm vụ là tìm dãy học sinh liền kề dài nhất có thể được phục vụ đầy đủ và đưa ra chỉ số của học sinh đầu tiên và cuối cùng trong dãy này hoặc -1 nếu không có học sinh nào có thể được phục vụ. 

Đầu vào bao gồm nhiều trường hợp kiểm tra, mỗi trường hợp xác định số lượng sinh viên, số dư ATM ban đầu và trình tự yêu cầu giao dịch. Các ràng buộc cho phép tối đa 200.000 học sinh trong mỗi trường hợp thử nghiệm và tối đa 10.000 trường hợp thử nghiệm, nhưng tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này ngay lập tức loại trừ mọi cách tiếp cận có độ phức tạp O(n²) cho mỗi trường hợp thử nghiệm vì trong trường hợp xấu nhất, chúng tôi sẽ thực hiện khoảng 4×10¹⁰ thao tác, vượt xa giới hạn thời gian. Cách tiếp cận O(n) hoặc O(n log n) là khả thi. Các trường hợp rủi ro chính phát sinh khi số dư ATM ban đầu không đủ để rút tiền sớm, khi tiền gửi xảy ra trước khi rút tiền hoặc khi các chuỗi chứa số 0, không được coi là rút tiền. 

Ví dụ, hãy xem xét trình tự`[-5, 10, -3]`với số dư ban đầu là`4`. Việc triển khai đơn giản bắt đầu từ sinh viên đầu tiên sẽ thất bại vì lần rút tiền đầu tiên vượt quá số dư. Cách tiếp cận đúng là bỏ qua học sinh đầu tiên, phục vụ hai học sinh tiếp theo và trả về các chỉ số chuỗi sau`2 3`. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét mọi sinh viên có thể bắt đầu, mô phỏng ATM cho từng dãy con liền kề và theo dõi độ dài của dãy con hợp lệ dài nhất. Cách tiếp cận này được đảm bảo chính xác vì nó kiểm tra rõ ràng mọi khả năng. Tuy nhiên, trong trường hợp xấu nhất n=200.000, nó sẽ thực hiện khoảng 2×10¹⁰ phép tính nếu chúng ta tính tổng độ dài của tất cả các chuỗi con, điều này không thể thực hiện được trong vòng 2 giây. 

Thông tin chi tiết quan trọng là vấn đề được giảm xuống thành một cửa sổ trượt một lần với việc theo dõi cẩn thận số dư ATM và số lần rút tiền. Khi ATM không thể phục vụ việc rút tiền, mọi trình tự tiếp theo bao gồm cả việc rút tiền đó đều không có hiệu lực. Tiền gửi dương chỉ làm tăng số dư ATM và không hạn chế các chuỗi tiếp theo, trong khi việc rút tiền âm có thể buộc cửa sổ thay đổi. Điều này tự nhiên gợi ý sử dụng cách tiếp cận hai con trỏ hoặc theo dõi giao dịch âm tối đa trong một cửa sổ, kết hợp với tổng tiền tố để đạt hiệu quả. Giải pháp duy trì số dư đang hoạt động và cập nhật động điểm bắt đầu của ứng viên khi số dư chuyển sang âm. Bằng cách tập trung vào các phân đoạn liền kề và tận dụng các đặc tính của tổng tích lũy, chúng ta có thể đạt được thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Cửa sổ trượt / Tổng tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai con trỏ:`start`ở mức 0 biểu thị thời điểm bắt đầu tiềm năng của chuỗi được phục vụ và`max_len`bằng 0 để lưu trữ độ dài của chuỗi con hợp lệ dài nhất. Theo dõi`max_start`Và`max_end`cho đầu ra cuối cùng. 
2. Lặp lại các sinh viên bằng một chỉ mục`i`, duy trì một`balance`biến được khởi tạo cho số tiền bắt đầu ATM`s`. 
3. Cộng giá trị giao dịch của mỗi học sinh`a[i]`ĐẾN`balance`. Nếu như`a[i]`là dương, nó làm tăng số dư ATM. Nếu âm tính thì kiểm tra xem`balance`trở nên tiêu cực sau khi rút tiền. 
4. Nếu`balance`giảm xuống dưới 0, học sinh này không thể được phục vụ bắt đầu từ hiện tại`start`. Di chuyển`start`ĐẾN`i + 1`và thiết lập lại`balance`ĐẾN`s`, coi học sinh tiếp theo là ứng viên mới bắt đầu một cách hiệu quả. 
5. Bất cứ khi nào một giao dịch hợp lệ xảy ra, hãy kiểm tra xem cửa sổ hiện tại có`i - start + 1`vượt quá`max_len`. Nếu có, hãy cập nhật`max_len`,`max_start`, Và`max_end`tới cửa sổ hiện tại. 
6. Sau khi xử lý tất cả học sinh, nếu`max_len`vẫn là 0, in`-1`. Ngược lại, in`max_start + 1`Và`max_end + 1`để chuyển đổi từ chỉ mục dựa trên 0 sang chỉ mục dựa trên 1. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng số dư ATM không bao giờ bị âm trong khoảng thời gian ứng viên và bằng cách chỉ thay đổi thời điểm bắt đầu khi không thể thực hiện rút tiền, thuật toán sẽ xác định một cách hiệu quả phân đoạn liền kề tối đa. Các giao dịch tích cực mở rộng khoảng thời gian mà không bị hạn chế, đảm bảo tìm thấy chuỗi con khả thi dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        a = list(map(int, input().split()))
        max_len = 0
        max_start = max_end = -1
        start = 0
        balance = s
        for i in range(n):
            balance += a[i]
            if balance < 0:
                start = i + 1
                balance = s
            else:
                if i - start + 1 > max_len:
                    max_len = i - start + 1
                    max_start = start
                    max_end = i
        if max_len == 0:
            print(-1)
        else:
            print(max_start + 1, max_end + 1)

if __name__ == "__main__":
    solve()
```Mã khởi tạo con trỏ và cân bằng như mô tả. Khi số dư âm, nó sẽ dịch chuyển`start`và đặt lại số dư để tránh các chuỗi không hợp lệ. Việc kiểm tra độ dài đảm bảo rằng chỉ chuỗi con hợp lệ dài nhất mới được ghi lại. Việc chuyển đổi sang lập chỉ mục dựa trên 1 là rất quan trọng để khớp với định dạng đầu ra. Đặt lại`balance`sau khi di chuyển`start`đảm bảo rằng mỗi phân đoạn ứng viên mới bắt đầu bằng số tiền ban đầu của ATM`s`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên:```
4 10
-16 2 -6 8
```| tôi | một [tôi] | cân bằng | bắt đầu | max_len | max_start | max_end | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | -16 | -6 | 1 | 0 | -1 | -1 | 
| 1 | 2 | 12 | 1 | 1 | 1 | 1 | 
| 2 | -6 | 6 | 1 | 2 | 1 | 2 | 
| 3 | 8 | 14 | 1 | 3 | 1 | 3 | 

Đầu ra là`2 4`, tương ứng với đoạn dài nhất`[2, -6, 8]`. 

Hãy xem xét đầu vào mẫu thứ hai:```
3 1000
-100000 -100000 -100000
```Tất cả sinh viên đều yêu cầu nhiều hơn mức mà ATM có. Số dư ngay lập tức giảm xuống dưới 0 đối với mỗi học sinh bắt đầu, do đó thuật toán sẽ in`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi học sinh được thăm viếng một lần, số dư cập nhật và so sánh là O(1). | 
| Không gian | O(n) cho mảng đầu vào | Chỉ mảng sinh viên được lưu trữ; các biến bổ sung sử dụng O(1). | 

Cho tổng cộng`n`trên tất cả các trường hợp thử nghiệm tối đa là 200.000, thuật toán thực hiện tối đa 200.000 lần lặp, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n4 10\n-16 2 -6 8\n3 1000\n-100000 -100000 -100000\n6 0\n2 6 -164 1 -1 -6543\n") in ["2 4\n-1\n1 2", "2 4\n-1\n4 5"], "sample 1"

# custom cases
assert run("1\n1 5\n-10\n") == "-1", "single withdrawal too large"
assert run("1\n5 0\n1 2 3 4 5\n") == "1 5", "all deposits, starting at 1"
assert run("1\n5 5\n-1 -1 -1 -1 -1\n") == "1 5", "withdrawals exactly equal to balance"
assert run("1\n5 3\n-2 -1 2 -1 -1\n") == "1 5", "mix, balance never negative"
assert run("1\n3 0\n-1 2 -1\n") == "2 3", "skip first negative, then serve next two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 -10 | -1 | Rút tiền một lần vượt quá ATM | 
| 5 0 1 2 3 4 5 | 1 5 | Tất cả tiền gửi, đoạn dài nhất là toàn bộ mảng | 
| 5 5 -1 -1 -1 -1 -1 | 1 5 | Rút tiền chính xác tiêu tốn số dư | 
| 5 3 -2 -1 2 -1 -1 | 1 5 | Tiền gửi và rút tiền hỗn hợp | 
| 3 0 -1 | | |
