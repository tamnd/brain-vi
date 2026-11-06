---
title: "CF 130H - Giá đỡ cân bằng"
description: "Chúng ta được yêu cầu xác định xem một chuỗi dấu ngoặc tròn có cân bằng hay không. Trình tự cân bằng là trình tự trong đó mọi dấu ngoặc mở «(» đều có dấu ngoặc đóng tương ứng «)» và các dấu ngoặc được lồng vào nhau đúng cách."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "H"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1600
weight: 130
solve_time_s: 76
verified: true
draft: false
---

[CF 130H - Dấu ngoặc cân bằng](https://codeforces.com/problemset/problem/130/H) 

**Đánh giá:** 1600 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một chuỗi dấu ngoặc tròn có cân bằng hay không. Trình tự cân bằng là trình tự trong đó mọi dấu ngoặc mở «(» đều có dấu ngoặc đóng tương ứng «)» và các dấu ngoặc được lồng vào nhau đúng cách. Nói cách khác, nếu bạn duyệt chuỗi từ trái sang phải, không bao giờ có nhiều dấu ngoặc đóng hơn dấu ngoặc mở và đến cuối, số dấu ngoặc mở và đóng phải khớp chính xác. 

Đầu vào là một chuỗi có độ dài từ 1 đến 100 ký tự, chỉ bao gồm «(» và «)». Đầu ra là một câu trả lời có/không đơn giản tùy thuộc vào việc trình tự có cân bằng hay không. 

Các ràng buộc ngụ ý rằng chúng ta không cần các thuật toán được tối ưu hóa cao. Ngay cả cách tiếp cận O(n²) cũng có thể chạy trong giới hạn 2 giây vì n ≤ 100. Tuy nhiên, cách tiếp cận tuyến tính vẫn được ưa chuộng hơn vì nó có quy mô rõ ràng và tránh tính toán không cần thiết. 

Các trường hợp cạnh phát sinh khi các chuỗi bắt đầu bằng dấu ngoặc đóng hoặc kết thúc bằng dấu ngoặc mở. Ví dụ: «)(» không cân bằng vì ký tự đầu tiên không thể khớp. Một trường hợp tinh vi khác là một chuỗi như «((()))» được lồng hoàn hảo nhưng yêu cầu theo dõi cẩn thận độ sâu của dấu ngoặc đơn. Chuỗi ký tự đơn «(» hoặc «)» không cân bằng vì chúng không có đối tác trùng khớp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng việc tạo ra tất cả các biểu thức hợp lệ có thể có bằng cách thêm các phần tử, nhưng điều này là không cần thiết. Một cách tiếp cận đơn giản hơn là kiểm tra mọi tiền tố của chuỗi để xem liệu nó có chứa nhiều dấu ngoặc đóng hơn dấu ngoặc mở hay không. Nếu có, trình tự sẽ ngay lập tức mất cân bằng. Sau đó, ở cuối, kiểm tra xem tổng số dấu ngoặc mở và đóng có khớp nhau không. Điều này đúng nhưng có thể được triển khai hiệu quả mà không cần mảng tiền tố rõ ràng. 

Cách tiếp cận tối ưu tận dụng bộ đếm đang chạy. Khởi tạo bộ đếm về 0. Duyệt chuỗi từ trái sang phải. Khi gặp dấu ngoặc mở, hãy tăng bộ đếm. Khi gặp dấu ngoặc đóng, hãy giảm bộ đếm. Nếu tại bất kỳ thời điểm nào bộ đếm trở thành số âm, điều đó cho biết rằng có nhiều dấu ngoặc đóng hơn dấu ngoặc mở, nghĩa là trình tự không cân bằng. Sau khi xử lý toàn bộ chuỗi, bộ đếm bằng 0 xác nhận chuỗi cân bằng; bất kỳ giá trị nào khác cho biết dấu ngoặc không khớp. Thông tin chi tiết là việc lồng ghép thích hợp sẽ đảm bảo bộ đếm không bao giờ giảm xuống dưới 0 và sự bằng nhau ở cuối đảm bảo mọi dấu ngoặc mở đều có dấu ngoặc đóng tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tiền tố) | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu (bộ đếm) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo biến đếm`balance`về 0. Bộ đếm này sẽ theo dõi số lượng thực của dấu ngoặc mở chưa khớp. 
2. Duyệt tuần tự từng ký tự của chuỗi. 
3. Nếu ký tự là dấu ngoặc mở «(», tăng dần`balance`bằng 1. 
4. Nếu ký tự là dấu ngoặc đóng «)», hãy giảm`balance`bằng 1. 
5. Ngay sau khi giảm, kiểm tra xem`balance`là tiêu cực. Nếu đúng, xuất «NO» và dừng. Số dư âm biểu thị khung đóng chưa khớp. 
6. Sau khi quá trình truyền tải hoàn tất, hãy kiểm tra xem`balance`bằng không. Nếu đúng như vậy, hãy xuất «CÓ» vì tất cả các dấu ngoặc mở đều có dấu ngoặc đóng phù hợp. Nếu không, xuất «KHÔNG» vì có dấu ngoặc mở không khớp. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước,`balance`đại diện cho số lượng dấu ngoặc mở hiện chưa khớp. Số dư âm biểu thị dấu ngoặc đóng không có dấu ngoặc mở tương ứng, vi phạm điều kiện cân bằng. Kết thúc bằng`balance`số 0 đảm bảo rằng mọi dấu ngoặc mở đều được ghép nối. Cách tiếp cận này phản ánh trực tiếp định nghĩa logic của chuỗi khung cân bằng và không thể tạo ra đầu ra không chính xác vì nó theo dõi rõ ràng thuộc tính lồng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
balance = 0
for ch in s:
    if ch == '(':
        balance += 1
    else:  # ch == ')'
        balance -= 1
    if balance < 0:
        print("NO")
        sys.exit(0)
print("YES" if balance == 0 else "NO")
```Mã đọc chuỗi đầu vào và xóa mọi dòng mới ở cuối. Chúng tôi duy trì một`balance`biến để theo dõi các dấu ngoặc mở chưa khớp. Mỗi dấu ngoặc mở sẽ tăng số dư, mỗi dấu ngoặc đóng sẽ giảm số dư. Việc kiểm tra số dư âm ngay lập tức đảm bảo chấm dứt sớm các chuỗi bắt đầu bằng dấu ngoặc đóng không khớp. Cuối cùng, việc kiểm tra số dư bằng 0 đảm bảo tất cả các lần mở đều khớp. Việc triển khai này tránh được từng lỗi một và xử lý chính xác các chuỗi có độ dài 1. 

## Ví dụ đã hoạt động 

Mẫu 1: «(()(()))()» 

| Nhân vật | cân bằng | Hành động | 
| --- | --- | --- | 
| ( | 1 | Tăng | 
| ( | 2 | Tăng | 
| ) | 1 | Giảm | 
| ( | 2 | Tăng | 
| ( | 3 | Tăng | 
| ) | 2 | Giảm | 
| ) | 1 | Giảm | 
| ) | 0 | Giảm | 
| ( | 1 | Tăng | 
| ) | 0 | Giảm | 

Không có lúc nào số dư trở nên âm và kết thúc ở mức 0, vì vậy đầu ra là «CÓ». Điều này xác nhận việc lồng và khớp chính xác. 

Mẫu 2: «)(» 

| Nhân vật | cân bằng | Hành động | 
| --- | --- | --- | 
| ) | -1 | Giảm và phát hiện số dư âm | 

Ngay lập tức in «KHÔNG» vì ký tự đầu tiên không thể khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một chuỗi, n ≤ 100 | 
| Không gian | O(1) | Chỉ có một biến đếm được duy trì | 

Với ràng buộc n ≤ 100, giải pháp tuyến tính này rất đơn giản để chạy trong giới hạn 2 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    balance = 0
    for ch in s:
        if ch == '(':
            balance += 1
        else:
            balance -= 1
        if balance < 0:
            return "NO"
    return "YES" if balance == 0 else "NO"

# Provided samples
assert run("(()(()))()\n") == "YES", "sample 1"

# Custom cases
assert run(")\n") == "NO", "single unmatched closing"
assert run("(\n") == "NO", "single unmatched opening"
assert run("()()()()()\n") == "YES", "repeated balanced pairs"
assert run("((())())(()())\n") == "YES", "nested balanced"
assert run("((())\n") == "NO", "missing closing"
assert run("())((\n") == "NO", "negative balance mid-sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ) | KHÔNG | Dấu ngoặc đóng duy nhất chưa từng có sẽ kích hoạt thoát sớm | 
| ( | KHÔNG | Đã xử lý khung mở duy nhất chưa từng có | 
| ()()()()() | CÓ | Các cặp đơn giản lặp đi lặp lại duy trì sự cân bằng | 
| ((())())(()()) | CÓ | Trình tự cân bằng lồng nhau hoạt động | 
| ((()) | KHÔNG | Đã phát hiện thiếu dấu ngoặc đóng | 
| ())(( | KHÔNG | Số dư được xử lý ở giữa chuỗi âm | 

## Vỏ cạnh 

Đối với đầu vào «)», số dư bắt đầu từ 0, ký tự đầu tiên giảm xuống -1, ngay lập tức trả về «KHÔNG». Đối với «((())», số dư tăng và giảm thông qua chuỗi, kết thúc ở 1, dẫn đến «KHÔNG». Những trường hợp này xác nhận rằng thuật toán xử lý chính xác các chuỗi bắt đầu bằng dấu ngoặc đóng không khớp, các chuỗi kết thúc bằng dấu ngoặc mở không khớp và các cấu trúc lồng nhau, luôn tạo ra kết quả đầu ra chính xác.
