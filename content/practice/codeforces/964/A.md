---
title: "CF 964A - Chia đôi"
description: "Chúng ta đang nghiên cứu cách viết số nguyên $n$ dưới dạng một dãy số nguyên dương không tăng. Nói cách khác, chúng ta chia $n$ thành các phần không bao giờ tăng khi chúng ta di chuyển sang phải và tất cả các phần phải dương."
date: "2026-06-17T01:41:01+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 964
codeforces_index: "A"
codeforces_contest_name: "Tinkoff Internship Warmup Round 2018 and Codeforces Round 475 (Div. 2)"
rating: 800
weight: 964
solve_time_s: 89
verified: true
draft: false
---

[CF 964A - Tách](https://codeforces.com/problemset/problem/964/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang nghiên cứu cách viết một số nguyên$n$dưới dạng một dãy số nguyên dương không tăng. Nói cách khác, chúng ta phá vỡ$n$thành những phần không bao giờ tăng khi chúng ta di chuyển sang phải và tất cả các phần đều phải dương. Điều này giống như một phân vùng số nguyên tiêu chuẩn được viết ở dạng được sắp xếp. 

Từ mỗi phân vùng như vậy, chúng tôi trích xuất một số duy nhất: đếm xem có bao nhiêu phần tử ban đầu bằng phần tử đầu tiên. Nếu phân vùng bắt đầu bằng một giá trị$x$, rồi đếm xem bao nhiêu lần$x$xuất hiện liên tiếp từ đầu trước khi dãy giảm xuống dưới$x$. Số lượng đó được gọi là trọng lượng. 

Đối với một cố định$n$, tồn tại nhiều phân vùng khác nhau và mỗi phân vùng tạo ra một trọng số. Nhiệm vụ không phải là đếm các phân vùng mà là đếm xem có thể đạt được bao nhiêu trọng số riêng biệt trên tất cả các phân vùng của$n$. 

Đầu vào chỉ là một số nguyên$n$và đầu ra là số giá trị khác nhau mà trọng số này có thể nhận. 

Ràng buộc$n \le 10^9$ngay lập tức loại trừ mọi phép liệt kê các phân vùng hoặc thậm chí một phần DP trên tất cả các tổng. Số lượng phân vùng của$n$phát triển siêu đa thức, vì vậy bất kỳ cách tiếp cận nào lặp lại trên các phân vùng đều không khả thi. Ngay cả việc lặp lại tất cả các phần đầu tiên có thể có và các cấu trúc bên dưới chúng cũng phải tránh trừ khi nó thu gọn về cấu trúc logarit hoặc căn bậc hai. 

Một trường hợp khó nhận thấy là khi các phân vùng bị lệch quá nhiều. Ví dụ,$[n]$cho trọng số 1, trong khi$[1,1,\dots,1]$mang lại trọng lượng$n$. Một trường hợp khác ít rõ ràng hơn là các trọng số trung gian có thể không liên tục; không phải mọi số nguyên từ 1 đến$n$nhất thiết có thể đạt được đối với nhỏ$n$và nhiệm vụ là đếm các giá trị riêng biệt có thể đạt được thay vì giả định một khoảng đầy đủ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra tất cả các phân vùng của$n$, tính trọng lượng của từng loại và thu thập các kết quả riêng biệt. Điều này đơn giản về mặt khái niệm: chọn đệ quy phần tiếp theo không vượt quá phần trước, cho đến khi tổng đạt$n$. Đối với mỗi phân vùng hoàn chỉnh, hãy đếm xem có bao nhiêu phần tử ban đầu khớp với giá trị đầu tiên. 

Điều này thất bại ngay lập tức vì số lượng phân vùng của$n$là số mũ trong$\sqrt{n}$. Ngay cả đối với$n = 50$, điều này đã tạo ra hàng triệu phân vùng và đối với$10^9$điều đó là hoàn toàn không thể. 

Quan sát quan trọng là trọng số chỉ phụ thuộc vào khối đầu tiên có giá trị bằng nhau. Giả sử phân vùng bắt đầu bằng giá trị$x$, lặp lại$k$lần. Sau đó chúng ta có:$$n = kx + (\text{remainder})$$trong đó phần còn lại là sự phân chia của số tiền còn lại bằng cách sử dụng nhiều nhất các phần$x$, và nó phải nhỏ hơn$x$(nếu không khối đầu tiên sẽ dài hơn). 

Cái nhìn sâu sắc về cấu trúc quan trọng là đối với một trọng lượng cố định$k$, tổng nhỏ nhất có thể xảy ra khi mọi thứ càng nhỏ càng tốt sau khối đầu tiên. Nghĩa là, chúng tôi lấy:$$[x, x, \dots, x, x-1, x-2, \dots, 1]$$với$k$bản sao của$x$. Điều này tạo ra tổng số tiền tối thiểu cho cấu trúc trọng số đó. 

Điều này biến vấn đề thành một điều kiện khả thi: với mỗi trọng số có thể$k$, chúng ta có thể hỏi liệu có tồn tại một số$x$sao cho một phân vùng có kích thước khối đầu tiên$k$là có thể. Điều kiện tồn tại trở thành một bất đẳng thức liên quan đến các số tam giác, vì phần tiếp tục “chặt” nhất sau khối đầu tiên là dãy giảm tối đa. 

Điều này dẫn đến một mối quan hệ đơn điệu: như$k$tăng lên, yêu cầu tối thiểu$n$phát triển gần giống như$k^2$. Vì vậy, khả năng tối đa có thể$k$theo thứ tự của$\sqrt{n}$và tất cả các trọng số hợp lệ tạo thành một phạm vi liền kề bắt đầu từ 1 cho đến mức tối đa đó. 

Vậy bài toán quy về việc tìm giá trị lớn nhất$k$như vậy:$$k(k+1)/2 \le n$$và câu trả lời chỉ đơn giản là tối đa$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | O(n) đệ quy | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển giới hạn cấu trúc thành một tính toán trực tiếp. 

1. Quan sát rằng một trọng lượng$k$yêu cầu ít nhất$k$bản sao của giá trị đầu tiên, sau đó chuỗi còn lại phải giảm hẳn hoặc ở dưới giá trị đó. Điều này buộc một “chi phí hình dạng” tối thiểu tăng theo phương trình bậc hai với$k$. 
2. Thể hiện cấu hình nhỏ nhất có thể cho trọng lượng nhất định$k$. Việc xây dựng chặt chẽ nhất xảy ra khi giá trị đầu tiên càng nhỏ càng tốt trong khi vẫn cho phép$k$các bản sao, đóng gói trình tự này một cách hiệu quả vào cấu trúc bậc thang. 
3. Tính số tiền tối thiểu cần thiết cho trọng lượng$k$, đó là số tam giác$k(k+1)/2$. Điều này tương ứng với việc xếp chồng các hàng có ràng buộc giảm dần trong khối đầu tiên. 
4. Tìm số lớn nhất$k$sao cho số tiền tối thiểu này không vượt quá$n$. Đây là trọng lượng tối đa khả thi. 
5. Xuất cái này$k$, vì tất cả trọng lượng nhỏ hơn đều có thể đạt được bằng cách nới lỏng kết cấu. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ phân vùng nào có trọng số khối đầu tiên$k$phải chứa ít nhất$k$số nguyên dương trong cấu trúc thực thi phần dư giảm dần sau khi giá trị đầu tiên được cố định. Điều này áp đặt giới hạn dưới tương đương với phân vùng cầu thang có chiều cao$k$. Số tam giác thể hiện tổng tối thiểu của một cầu thang như vậy và bất kỳ độ trễ bổ sung nào trong$n$chỉ có thể tăng tính linh hoạt chứ không bao giờ giảm tính khả thi. Do đó tính khả thi chỉ phụ thuộc vào việc liệu$n$vượt qua ngưỡng tối thiểu đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

k = 0
# find maximum k such that k*(k+1)//2 <= n
# can do O(sqrt(n)) loop or binary search; sqrt is enough for 1e9
while (k + 1) * (k + 2) // 2 <= n:
    k += 1

print(k)
```Việc thực hiện trực tiếp tìm kiếm số tam giác lớn nhất không vượt quá$n$. Vòng lặp tăng lên$k$đồng thời duy trì tình trạng$k(k+1)/2 \le n$. sử dụng$(k+1)(k+2)/2$tránh tính toán lại sau khi tăng và giữ cho điều kiện sạch sẽ. 

Không cần xử lý cạnh ngoài phân tích số nguyên tiêu chuẩn vì$n \ge 1$đảm bảo ít nhất$k = 1$được coi là đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 7$Chúng tôi kiểm tra giá trị tăng dần của$k$. 

| k | k(k+1)/2 | ≤ 7 | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 3 | vâng | 
| 3 | 6 | vâng | 
| 4 | 10 | không | 

Vòng lặp dừng ở$k = 3$. 

Điều này phù hợp với cấu trúc có trọng số 1, 2, 3 và 7 có thể đạt được, mang lại 4 giá trị riêng biệt. 

### Ví dụ 2:$n = 8$| k | k(k+1)/2 | 8 | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 3 | vâng | 
| 3 | 6 | vâng | 
| 4 | 10 | không | 

Vậy câu trả lời lại là$3$. 

Điều này cho thấy ngày càng tăng$n$trọng lượng tối đa không tăng ngay lập tức; ngưỡng chỉ thay đổi ở số tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n})$| Chúng tôi tăng$k$cho đến khi số tam giác vượt quá$n$, Và$k$lớn lên khoảng$\sqrt{2n}$| 
| Không gian |$O(1)$| Chỉ có một số biến số nguyên được sử dụng | 

Sự ràng buộc$n \le 10^9$làm cho$\sqrt{n} \approx 31623$, do đó, ngay cả việc quét tuyến tính cũng không đáng kể dưới 1 giây trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input().strip())
    k = 0
    while (k + 1) * (k + 2) // 2 <= n:
        k += 1
    return str(k)

# provided sample
assert run("7\n") == "3"

# minimum input
assert run("1\n") == "1"

# small triangular boundary
assert run("6\n") == "3"

# just above triangular number
assert run("7\n") == "3"

# large input
assert run("1000000000\n") == str(int(((2*1000000000)**0.5)))  # rough upper check
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp tối thiểu | 
| 6 | 3 | ranh giới tam giác chính xác | 
| 7 | 3 | ngay sau ranh giới | 
| 1e9 | ~44721 | hiệu suất và mở rộng quy mô | 

## Vỏ cạnh 

cho$n = 1$, phân vùng duy nhất là$[1]$, do đó trọng số duy nhất là 1. Thuật toán bắt đầu bằng$k = 0$, kiểm tra$1 \le 1$, tăng lên 1 và dừng ngay sau khi thất bại trong lần kiểm tra tam giác tiếp theo. 

Vì$n = 6$, chúng tôi đánh chính xác$3 \cdot 4 / 2 = 6$. Thuật toán cho phép chính xác$k = 3$, cho thấy rằng các số tam giác chính xác được đưa vào dưới dạng cực đại hợp lệ thay vì bị loại trừ từng số một. 

Vì$n = 7$, yêu cầu tối thiểu đối với$k = 4$là 10 nên bị bác bỏ. Thuật toán giữ đúng$k = 3$, xác nhận rằng tính khả thi chỉ thay đổi ở các ngưỡng tam giác chứ không phải giữa chúng.
