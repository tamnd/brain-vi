---
title: "CF 105472H - Đi bộ đường dài nóng bỏng"
description: "Chúng ta được cung cấp một chuỗi nhiệt độ hàng ngày trong một kỳ nghỉ có độ dài $n$. Chúng ta phải chọn một dãy liên tục gồm đúng ba ngày liên tiếp: ngày đầu tiên là đi bộ lên hồ, ngày thứ hai là nghỉ ngơi (bỏ qua vì lý do nhiệt), và ngày thứ ba là đi bộ về."
date: "2026-06-23T18:05:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105472
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2019)"
rating: 0
weight: 105472
solve_time_s: 46
verified: true
draft: false
---

[CF 105472H - Đi bộ đường dài nóng bỏng](https://codeforces.com/problemset/problem/105472/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhiệt độ hàng ngày trong một kỳ nghỉ dài$n$. Chúng ta phải chọn một dãy liên tục gồm đúng ba ngày liên tiếp: ngày đầu tiên là đi bộ lên hồ, ngày thứ hai là nghỉ ngơi (bỏ qua vì lý do nhiệt), và ngày thứ ba là đi bộ về. Chỉ có hai ngày đi bộ đường dài là quan trọng và sự khó chịu của chuyến đi được xác định là nhiệt độ tối đa trong hai ngày đi bộ đường dài đó. 

Vì vậy, với mỗi ngày bắt đầu hợp lệ$d$, chúng ta xét cặp nhiệt độ$(t_d, t_{d+2})$và chúng tôi muốn giảm thiểu$\max(t_d, t_{d+2})$. Đầu ra là ngày bắt đầu tốt nhất và giá trị tối thiểu tương ứng của mức tối đa này. Nếu nhiều ngày bắt đầu đạt được giá trị tối thiểu như nhau thì chúng ta chọn chỉ số ngày bắt đầu nhỏ nhất. 

Ràng buộc$3 \le n \le 50$ngay lập tức cho chúng ta biết rằng ngay cả một nghiệm bậc hai hoặc bậc ba cũng có thể chạy dễ dàng trong giới hạn. Quét trực tiếp tất cả các ngày bắt đầu có thể là đủ. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa. 

Một trường hợp góc cạnh tinh tế đến từ việc bẻ dây buộc. Nếu hai vị trí xuất phát tạo ra nhiệt độ đi bộ đường dài tối đa như nhau thì chúng ta phải chọn vị trí sớm nhất. Ví dụ, nếu nhiệt độ$[10, 1, 10, 1]$, sau đó bắt đầu từ ngày 1 sẽ đạt giá trị tối đa$10$, và bắt đầu từ ngày thứ 2 cho giá trị tối đa$10$cũng vậy, nhưng chúng ta phải xuất ngày 1. 

Một trường hợp khác là khi giải pháp tốt nhất xuất hiện ở cuối mảng. Vì chúng ta truy cập$d+2$, ngày bắt đầu hợp lệ cuối cùng là$n-2$. Một lỗi phổ biến là lặp đi lặp lại đến$n-1$, sẽ lập chỉ mục ngoài giới hạn hoặc bỏ qua các ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực cố gắng mỗi ngày bắt đầu có thể$d$từ 1 đến$n-2$. Đối với mỗi$d$, nó tính toán nhiệt độ tối đa của hai ngày đi bộ đường dài$t_d$Và$t_{d+2}$và theo dõi giá trị tối thiểu được thấy cho đến nay. Điều này đúng vì mỗi chuyến đi hợp lệ được coi là đúng một lần. 

Chi phí của phương pháp này là$O(n)$, vì mỗi ứng viên được đánh giá theo thời gian không đổi và có$n-2$ứng viên. Ngay cả khi chúng ta mở rộng bài toán để kiểm tra các mẫu phức tạp hơn, phép liệt kê cửa sổ trượt này vẫn rẻ. 

Không có sự tối ưu hóa có ý nghĩa nào ngoài điều này vì mỗi ngày bắt đầu đều độc lập. Cấu trúc của vấn đề đã ở mức tối thiểu: một cửa sổ có kích thước cố định với sự tổng hợp đơn giản trên hai điểm cuối. Quan sát quan trọng là ngày giữa không liên quan nên chúng ta không bao giờ cần xem xét nó trong tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

Trên thực tế, giải pháp “tối ưu” chỉ là quét mạnh mẽ với sổ sách kế toán rõ ràng. 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các vị trí xuất phát hợp lệ và tính toán chi phí cho mỗi chuyến đi của ứng viên. 

1. Khởi tạo các biến để lưu trữ câu trả lời tốt nhất: giá trị lớn cho nhiệt độ tối thiểu và phần giữ chỗ cho ngày bắt đầu tốt nhất. Chúng tôi sử dụng một giá trị ban đầu rất lớn để bất kỳ nhiệt độ thực nào cũng sẽ cải thiện nó. 
2. Lặp lại tất cả các ngày bắt đầu$d$từ 0 đến$n-3$trong lập chỉ mục dựa trên số không. Điều này đảm bảo cả$d$Và$d+2$là những chỉ số hợp lệ. 
3. Đối với mỗi$d$, tính giá trị$v = \max(t[d], t[d+2])$. Đây là nhiệt độ tồi tệ nhất trong ngày đi bộ đường dài cho chuyến đi đó. 
4. Nếu$v$hoàn toàn nhỏ hơn giá trị tốt nhất được thấy cho đến nay, hãy cập nhật cả giá trị tốt nhất và ngày bắt đầu tốt nhất để$d$. Chúng tôi cũng ngầm xử lý tình trạng ràng buộc bằng cách chỉ cập nhật cải tiến nghiêm ngặt, giúp bảo toàn chỉ số sớm nhất. 
5. Sau khi quét xong, xuất ra ngày bắt đầu tốt nhất được chuyển đổi về chỉ mục một lần, cùng với giá trị tốt nhất. 

### Tại sao nó hoạt động 

Mỗi chuyến đi hợp lệ tương ứng với đúng một chỉ số$d$, và chi phí của chuyến đi đó chỉ phụ thuộc vào hai yếu tố cố định$t[d]$Và$t[d+2]$. Vì chúng tôi đánh giá tất cả các cặp như vậy chính xác một lần và luôn giữ lại giá trị tối thiểu đối với tất cả các ứng viên nên câu trả lời được lưu trữ là tối ưu toàn cục. Tính chính xác của việc ràng buộc xuất phát từ việc không bao giờ ghi đè một giá trị bằng nhau khi lần xuất hiện đầu tiên đã được ghi lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
t = list(map(int, input().split()))

best_val = float('inf')
best_d = 0

for d in range(n - 2):
    val = max(t[d], t[d + 2])
    if val < best_val:
        best_val = val
        best_d = d

print(best_d + 1, best_val)
```Vòng lặp lõi trực tiếp thực hiện việc liệt kê tất cả các ngày bắt đầu hợp lệ. Việc sử dụng lập chỉ mục dựa trên số 0 giúp đơn giản hóa việc xử lý ranh giới, vì chúng ta tự nhiên dừng lại ở$n-3$. điều kiện`val < best_val`thực thi cả việc giảm thiểu và phá vỡ ràng buộc chính xác, bởi vì các chỉ số trước đó không bao giờ được thay thế bằng các chỉ số bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
23 27 31 28 30
```Chúng tôi đánh giá tất cả các lần khởi đầu hợp lệ: 

| d (dựa trên 0) | t[d] | t[d+2] | tối đa | 
| --- | --- | --- | --- | 
| 0 | 23 | 31 | 31 | 
| 1 | 27 | 28 | 28 | 
| 2 | 31 | 30 | 31 | 

Giá trị tốt nhất là 28 tại$d = 1$, vì vậy đầu ra là:```
2 28
```Điều này cho thấy việc bỏ qua ngày giữa cho phép ghép đôi tốt hơn như thế nào mà chỉ xét riêng mức cực tiểu cục bộ thì không rõ ràng. 

### Ví dụ 2 

đầu vào:```
4
30 20 20 30
```| d | t[d] | t[d+2] | tối đa | 
| --- | --- | --- | --- | 
| 0 | 30 | 20 | 30 | 
| 1 | 20 | 30 | 30 | 

Cả hai lựa chọn đều có cùng giá trị 30, nhưng chúng tôi chọn chỉ số bắt đầu nhỏ nhất$d=0$, sản xuất:```
1 30
```Điều này khẳng định quy tắc ràng buộc được xử lý hoàn toàn bằng điều kiện cải tiến nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Chúng tôi đánh giá mỗi ngày bắt đầu hợp lệ một lần, tính toán giá trị tối đa theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Với$n \le 50$, giá trị này thấp hơn nhiều so với bất kỳ giới hạn thực tế nào và thậm chí đối với các giá trị lớn hơn nhiều$n$, quá trình quét tuyến tính vẫn tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO as _StringIO

    backup = _sys.stdin
    _sys.stdin = _StringIO(inp)

    n = int(input())
    t = list(map(int, input().split()))

    best_val = float('inf')
    best_d = 0

    for d in range(n - 2):
        val = max(t[d], t[d + 2])
        if val < best_val:
            best_val = val
            best_d = d

    _sys.stdin = backup
    return f"{best_d + 1} {best_val}"

assert run("5\n23 27 31 28 30\n") == "2 28"
assert run("4\n30 20 20 30\n") == "1 30"

# minimum size
assert run("3\n10 1 10\n") == "1 10"

# all equal
assert run("5\n5 5 5 5 5\n") == "1 5"

# decreasing
assert run("5\n40 30 20 10 0\n") == "3 20"

# increasing
assert run("5\n0 10 20 30 40\n") == "1 20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Luân phiên 3 ngày | 1 10 | độ dài hợp lệ tối thiểu | 
| tất cả đều bình đẳng | 1 5 | sự ổn định liên kết | 
| dãy giảm dần | 3 20 | ranh giới gần cuối tốt nhất | 
| trình tự tăng dần | 1 20 | tốt nhất ở ranh giới bắt đầu | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu$n = 3$, có đúng một chuyến đi có thể xảy ra. Thuật toán chỉ đánh giá$d = 0$, tính toán$\max(t[0], t[2])$. Không có sự mơ hồ và vòng lặp thực hiện chính xác một lần. 

Đối với các mảng không đổi như$[5,5,5,5,5]$, mọi ứng cử viên đều tạo ra giá trị như nhau. Bởi vì chúng tôi chỉ cập nhật về sự cải tiến nghiêm ngặt nên chỉ số đầu tiên vẫn được chọn, đảm bảo sự liên kết chính xác mà không có bất kỳ sự so sánh rõ ràng nào về các chỉ số. 

Đối với các trường hợp nặng về biên như dãy tăng nghiêm ngặt, giải pháp tối ưu luôn là$d = 0$, vì các vị trí sau này kết hợp với các giá trị thậm chí còn lớn hơn tại$d+2$. Quá trình quét sẽ bảo tồn điều này một cách tự nhiên mà không cần xử lý đặc biệt. 

Đối với các chuỗi giảm nghiêm ngặt, lựa chọn tốt nhất sẽ chuyển về phía cuối nơi cả hai điểm cuối đều nhỏ nhất. Vì vòng lặp bao gồm$d = n-3$, ứng cử viên cuối cùng luôn được xem xét, ngăn chặn các lỗi sai sót.
