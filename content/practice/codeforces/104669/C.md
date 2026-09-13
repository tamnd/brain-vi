---
title: "CF 104669C - Hoán vị tối đa"
description: "Chúng ta được cấp một số $n$ và chúng ta phải đưa ra một hoán vị của các số nguyên từ $1$ đến $n$. Đối với mỗi vị trí $i$, chúng ta tính toán một giá trị được hình thành bằng cách nhân chỉ số và giá trị được đặt ở đó, cụ thể là $i cdot pi$."
date: "2026-06-29T09:40:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104669
codeforces_index: "C"
codeforces_contest_name: "Turtle Codes"
rating: 0
weight: 104669
solve_time_s: 105
verified: false
draft: false
---

[CF 104669C - Hoán vị tối đa](https://codeforces.com/problemset/problem/104669/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số duy nhất$n$và chúng ta phải xuất ra một hoán vị của các số nguyên từ$1$ĐẾN$n$. Đối với mỗi vị trí$i$, chúng tôi tính toán một giá trị được hình thành bằng cách nhân chỉ số và giá trị được đặt ở đó, cụ thể là$i \cdot p_i$. Mục đích là sắp xếp hoán vị sao cho càng nhiều sản phẩm trong số này càng khác biệt càng tốt. 

Vì có$n$vị trí, chúng ta không bao giờ có thể có nhiều hơn$n$sản phẩm riêng biệt. Câu hỏi thực sự là liệu chúng ta có thể luôn đạt đến giới hạn trên này hay không hay va chạm là không thể tránh khỏi. 

Một cách hữu ích để nghĩ về trường hợp thất bại là khi hai vị trí sản xuất ra cùng một sản phẩm. Ví dụ, nếu$i \cdot p_i = j \cdot p_j$, thì việc xây dựng đang “lãng phí” một giá trị khác biệt có thể có. Một cách tiếp cận ngây thơ có thể thử hoán vị ngẫu nhiên hoặc hoán đổi tham lam, nhưng những cách đó có thể vô tình tạo ra các sản phẩm lặp lại mà không có hình mẫu rõ ràng, đặc biệt vì các sản phẩm phụ thuộc vào cả chỉ số và giá trị. 

Ràng buộc$n \le 2 \cdot 10^5$cho thấy chúng tôi không thể mô phỏng hoặc tìm kiếm các hoán vị. Bất kỳ cách tiếp cận nào liên quan đến việc kiểm tra nhiều ứng viên hoặc quay lại các bài tập sẽ là quá chậm. Chúng ta cần một cách xây dựng trực tiếp đảm bảo số lượng sản phẩm riêng biệt tối đa trong thời gian tuyến tính. 

Các trường hợp cạnh đáng chú ý là các giá trị nhỏ của$n$, đặc biệt$n = 1$, trong đó câu trả lời là tầm thường và rất lớn$n$, trong đó bất kỳ suy luận bậc hai nào về va chạm theo cặp là không thể. Một cạm bẫy tinh vi là giả định rằng hầu hết các hoán vị tự nhiên tạo ra các sản phẩm riêng biệt, điều này là sai, vì các phép gán đối xứng như$p_i = n+1-i$thường tạo ra các giá trị lặp đi lặp lại. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra một hoán vị và đếm xem có bao nhiêu giá trị riêng biệt xuất hiện trong mảng$i \cdot p_i$, cố gắng cải thiện nó bằng cách hoán đổi các phần tử khi tìm thấy xung đột. Điều này đòi hỏi phải tính toán lại các sản phẩm nhiều lần và kiểm tra xung đột trên nhiều cấu hình. Trong trường hợp xấu nhất, mỗi lần điều chỉnh có thể yêu cầu quét$O(n)$giá trị, và làm điều này nhiều lần dẫn đến ít nhất$O(n^2)$hành vi quá chậm đối với$n = 2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta thực sự không bắt buộc phải tối đa hóa bất cứ điều gì thông qua sự tương tác giữa các vị trí. Chúng ta chỉ cần đảm bảo rằng tất cả$n$sản phẩm là khác biệt, đó là mức tối đa về mặt lý thuyết. Vì vậy, vấn đề giảm xuống còn việc xây dựng một hoán vị sao cho ánh xạ$i \mapsto i \cdot p_i$là tiêm chích. 

Một cách đơn giản để đảm bảo tính tiêm vào là loại bỏ tất cả sự đối xứng về cấu trúc trong chuỗi sản phẩm. Cách xây dựng trực tiếp nhất là giữ cho hoán vị không thay đổi, thiết lập$p_i = i$. Trong trường hợp này, mọi sản phẩm đều trở thành$i^2$, và vì hàm$i^2$đang tăng nghiêm ngặt trên các số nguyên dương, tất cả các giá trị sẽ tự động được phân biệt. 

Điều này loại bỏ mọi nhu cầu ghép nối các đối số, kết hợp tham lam hoặc xử lý xung đột. Cuộc đấu tranh bạo lực xuất phát từ việc cố gắng quản lý các tương tác giữa các chỉ số khác nhau, nhưng hoán vị danh tính đã tách rời hoàn toàn các tương tác này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trao đổi Brute Force để khắc phục va chạm |$O(n^2)$|$O(n)$| Quá chậm | 
| hoán vị danh tính$p_i = i$|$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hoán vị độ dài$n$trong đó giá trị ở vị trí$i$là$i$. Điều này có nghĩa là chúng tôi chỉ xuất ra các số từ$1$ĐẾN$n$theo thứ tự. Động lực là làm cho mỗi sản phẩm phụ thuộc vào một biến duy nhất thay vì tương tác theo cặp. 
2. Tính toán sản phẩm theo khái niệm$i \cdot p_i = i \cdot i$. Chúng ta thực sự không cần phải lưu trữ chúng, nhưng bước này làm rõ rằng mỗi vị trí sẽ ánh xạ tới một hình vuông hoàn hảo. 
3. Xuất trực tiếp hoán vị đã xây dựng. 

### Tại sao nó hoạt động 

Trình tự được xây dựng tạo ra sản phẩm$1^2, 2^2, 3^2, \dots, n^2$. Kể từ khi chức năng$i^2$đang tăng nghiêm ngặt đối với số nguyên$i \ge 1$, không có hai chỉ số nào có thể tạo ra cùng một sản phẩm. Điều này đảm bảo rằng tất cả$n$sản phẩm khác biệt, đạt được giá trị tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    print(*range(1, n + 1))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc in hoán vị danh tính. Chi tiết triển khai chính là không yêu cầu mảng hoặc tính toán bổ sung. Định dạng đầu ra đáp ứng trực tiếp yêu cầu hoán vị. 

Không có vấn đề riêng lẻ nào vì Python`range(1, n+1)`tự nhiên tạo ra chính xác các chỉ số cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Công dụng xây dựng$p_i = i$. 

| tôi | p_i | tôi *p_i | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 4 | 
| 3 | 3 | 9 | 
| 4 | 4 | 16 | 

Tất cả các giá trị đều khác biệt, vì vậy đầu ra là:```
1 2 3 4
```Điều này chứng tỏ rằng ngay cả cấu trúc đơn giản nhất cũng đã đạt được mức tối đa. 

### Ví dụ 2 

đầu vào:```
5
```| tôi | p_i | tôi *p_i | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 4 | 
| 3 | 3 | 9 | 
| 4 | 4 | 16 | 
| 5 | 5 | 25 | 

Đầu ra:```
1 2 3 4 5
```Điều này xác nhận rằng mô hình có tỷ lệ mà không tạo ra bất kỳ xung đột nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chúng tôi xuất ra một hoán vị tuyến tính duy nhất | 
| Không gian |$O(1)$| Không có bộ nhớ phụ ngoài đầu ra | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$và đầu ra tuyến tính dễ dàng đủ nhanh. Không có quá trình xử lý trước hoặc tính toán nào ngoài việc in số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    return " ".join(map(str, range(1, n + 1)))

# provided samples (identity is also valid output)
assert run("4\n") == "1 2 3 4"
assert run("5\n") == "1 2 3 4 5"
assert run("7\n") == "1 2 3 4 5 6 7"

# custom cases
assert run("1\n") == "1"
assert run("2\n") == "1 2"
assert run("6\n") == "1 2 3 4 5 6"
assert run("10\n") == "1 2 3 4 5 6 7 8 9 10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp tối thiểu | 
| 2 | 1 2 | hoán vị không tầm thường nhỏ nhất | 
| 6 | 1 2 3 4 5 6 | kiểm tra tính chính xác của kích thước trung bình | 
| 10 | 1 2 3 4 5 6 7 8 9 10 | tính đúng đắn chung | 

## Vỏ cạnh 

cho$n = 1$, việc xây dựng tạo ra một hoán vị phần tử đơn$[1]$và bộ sản phẩm chỉ chứa$1$, khác biệt một cách tầm thường. 

Vì$n = 2$, đầu ra$[1, 2]$sản xuất sản phẩm$1$Và$4$, khác nhau, xác nhận rằng ngay cả trường hợp không tầm thường nhỏ nhất cũng hoạt động mà không cần điều chỉnh. 

Đối với lớn$n$, công trình vẫn ổn định vì mỗi sản phẩm chỉ phụ thuộc vào chỉ số của nó nên không có khả năng xảy ra các va chạm tiềm ẩn phát sinh từ sự tương tác giữa các vị trí.
