---
title: "CF 1244A - Bút mực và bút chì"
description: "Ngày mai Polycarp có hai loại công việc: viết bài giảng và vẽ trong giờ học thực hành. Đối với các bài giảng, anh ấy phải sử dụng bút và mỗi cây bút có thể được sử dụng cho một số lượng bài giảng giới hạn trước khi bút bị khô."
date: "2026-06-15T21:22:15+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1244
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 592 (Div. 2)"
rating: 800
weight: 1244
solve_time_s: 153
verified: false
draft: false
---

[CF 1244A - Bút mực và Bút chì](https://codeforces.com/problemset/problem/1244/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ngày mai Polycarp có hai loại công việc: viết bài giảng và vẽ trong giờ học thực hành. Đối với các bài giảng, anh ấy phải sử dụng bút và mỗi cây bút có thể được sử dụng cho một số lượng bài giảng giới hạn trước khi bút bị khô. Đối với các lớp học thực hành, anh ấy phải sử dụng bút chì và mỗi cây bút chì có thể được sử dụng lại trong một số lớp học cố định trước khi nó không còn sử dụng được nữa. 

Nhiệm vụ là chọn số lượng bút và bút chì để có thể đọc hết tất cả các bài giảng và lớp học thực hành, đồng thời nhét mọi thứ vào trong một hộp bút chì có thể đựng được nhiều nhất.$k$tổng số mặt hàng. Nếu không thể vừa đáp ứng được yêu cầu về khối lượng công việc vừa đáp ứng được năng lực thì chúng tôi phải báo cáo lỗi. 

Mỗi test case đưa ra số lượng bài giảng$a$, lớp thực hành$b$, dung lượng bút$c$, dung tích bút chì$d$và tổng giới hạn lưu trữ$k$. Chúng tôi xuất ra bất kỳ cặp hợp lệ nào$(x, y)$Ở đâu$x$bút che tất cả các bài giảng,$y$bút chì bao gồm tất cả các lớp thực hành, và$x + y \le k$. 

Hạn chế chính là bút mực và bút chì có cách sử dụng độc lập nhưng có giới hạn dung lượng dùng chung. Sự kết hợp đó chính là nguyên nhân làm cho vấn đề trở nên không tầm thường. 

Giới hạn đầu vào nhỏ, tất cả các giá trị tối đa là 100 và tối đa 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất cứ điều gì nặng hơn thời gian không đổi cho mỗi trường hợp thử nghiệm. Thậm chí một$O(k^2)$Về mặt lý thuyết, cách tiếp cận này là tốt, nhưng không cần thiết. 

Trường hợp phức tạp xuất hiện khi một trong các yêu cầu buộc phải có số lượng lớn mục trong khi dung lượng nhỏ. Ví dụ, nếu$a = 10, c = 1$, thì cần ít nhất 10 cây bút. Nếu như$k < 10$, câu trả lời là không thể ngay cả khi bút chì không còn phù hợp nữa. Một cách tiếp cận ngây thơ chỉ kiểm tra tính khả thi tổng thể mà không tôn trọng mức tối thiểu của mỗi loại sẽ thất bại ở đây. 

Một vấn đề khác nảy sinh nếu người ta cố gắng giảm thiểu tổng số mặt hàng một cách tham lam mà không tính đến tính linh hoạt. Vì bất kỳ cặp hợp lệ nào đều được chấp nhận nên việc giảm thiểu là không cần thiết và có thể dẫn đến suy nghĩ quá mức hoặc chiến lược cân bằng không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử tất cả số lượng bút có thể$x$từ 0 đến$k$, tính xem họ có thể giảng được bao nhiêu bài giảng, rồi kiểm tra xem có tồn tại số bút chì tương ứng không$y$thỏa mãn cả các ràng buộc về vùng phủ sóng và giới hạn dung lượng. Đối với mỗi$x$, tính khả thi của tính toán là thời gian không đổi, do đó độ phức tạp tổng thể là$O(k)$mỗi trường hợp thử nghiệm. Điều này đã hoạt động trong điều kiện hạn chế, nhưng nó phức tạp hơn mức cần thiết. 

Điều quan trọng là số lượng bút tối thiểu cần có là cố định: mỗi bút che phủ$c$bài giảng, vì vậy chúng tôi cần ít nhất$\lceil a / c \rceil$bút. Tương tự, chúng ta cần ít nhất$\lceil b / d \rceil$bút chì. Nếu chúng ta lấy chính xác số lượng yêu cầu tối thiểu này thì câu hỏi duy nhất còn lại là liệu chúng có vừa với hộp bút chì hay không. Nếu không, việc tăng một trong hai loại chỉ làm cho tình hình trở nên tồi tệ hơn về năng lực, vì việc tăng các hạng mục không bao giờ giúp ích cho tính khả thi theo giới hạn trên cố định. 

Điều này giúp giảm vấn đề xuống còn kiểm tra tính khả thi trực tiếp, sau đó là đầu ra của cấu hình yêu cầu tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k)$mỗi bài kiểm tra |$O(1)$| Được chấp nhận nhưng không cần thiết | 
| Tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng bút tối thiểu cần thiết là$x = \lceil a / c \rceil$. Điều này đảm bảo tất cả các bài giảng đều được trình bày đầy đủ vì mỗi cây bút hỗ trợ tối đa$c$bài giảng. 
2. Tính số bút chì tối thiểu cần dùng là$y = \lceil b / d \rceil$. Điều này đảm bảo tất cả các lớp thực hành được bao phủ bởi vì mỗi cây bút chì hỗ trợ tối đa$d$các lớp học. 
3. Kiểm tra xem$x + y \le k$. Điều này đảm bảo ràng buộc về trường hợp bút chì được thỏa mãn. 
4. Nếu điều kiện không thành công, xuất ra$-1$bởi vì ngay cả lựa chọn hiệu quả nhất cũng đã vượt quá khả năng và bất kỳ lựa chọn thay thế nào cũng chỉ làm tăng mức sử dụng. 
5. Ngược lại, xuất ra$x$Và$y$. Bất kỳ mục bổ sung nào đều không cần thiết và vấn đề cho phép bất kỳ cấu hình hợp lệ nào, vì vậy cấu trúc tối thiểu này là đủ. 

### Tại sao nó hoạt động 

Công trình sử dụng số lượng hạng mục của từng loại ít nhất có thể mà vẫn đáp ứng yêu cầu khối lượng công việc. Bất kỳ giải pháp hợp lệ nào cũng phải sử dụng ít nhất nhiều bút mực và bút chì này một cách riêng lẻ, vì việc giảm một trong hai sẽ không bao gồm được tất cả các nhiệm vụ. Vì vậy, nếu ngay cả cấu hình tối thiểu này cũng vượt quá dung lượng thì không có sự sắp xếp lại nào có thể khắc phục được. Nếu nó phù hợp, nó tự động là một giải pháp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b, c, d, k = map(int, input().split())

    pens = (a + c - 1) // c
    pencils = (b + d - 1) // d

    if pens + pencils > k:
        print(-1)
    else:
        print(pens, pencils)
```Việc tính toán sử dụng phép chia trần thông qua số học số nguyên, tránh các phép tính dấu phẩy động. Mỗi trường hợp thử nghiệm được xử lý độc lập. Điểm quyết định duy nhất là kiểm tra tính khả thi đối với$k$, phản ánh trực tiếp hạn chế lưu trữ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 5 4 5 8
```| Bước | tính toán bút | tính toán bút chì | tổng hợp | quyết định | 
| --- | --- | --- | --- | --- | 
| tính toán | (7+3)//4 = 2 | (5+4)//5 = 1 | 3 | 3 8 8 | 

Đầu ra là:```
2 1
```Điều này chứng tỏ trường hợp cả hai yêu cầu đều nhỏ và phù hợp với khả năng của bạn. 

### Ví dụ 2 

đầu vào:```
20 53 45 26 4
```| Bước | tính toán bút | tính toán bút chì | tổng hợp | quyết định | 
| --- | --- | --- | --- | --- | 
| tính toán | (20+44)//45 = 1 | (53+25)//26 = 3 | 4 | 4 4 4 | 

Đầu ra là:```
1 3
```Điều này cho thấy trường hợp ranh giới dung lượng chặt chẽ trong đó giải pháp khớp chính xác với giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm chỉ yêu cầu một vài phép tính số học | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài biến | 

Các ràng buộc đủ nhỏ đến mức ngay cả số học lặp đi lặp lại cũng không đáng kể. Giải pháp chạy trong thời gian không đổi cho mỗi trường hợp thử nghiệm, dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        a, b, c, d, k = map(int, input().split())
        pens = (a + c - 1) // c
        pencils = (b + d - 1) // d
        if pens + pencils > k:
            out.append("-1")
        else:
            out.append(f"{pens} {pencils}")
    return "\n".join(out)

# provided samples
assert solve("""3
7 5 4 5 8
7 5 4 5 2
20 53 45 26 4
""") == """2 1
-1
1 3"""

# minimum case
assert solve("""1
1 1 1 1 2
""") == "1 1"

# tight impossible
assert solve("""1
10 10 5 5 3
""") == "-1"

# exact fit
assert solve("""1
4 6 2 3 3
""") in {"2 2", "2 2".strip()}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 2 | 1 1 | trường hợp hợp lệ không tầm thường nhỏ nhất | 
| 10 10 5 5 3 | -1 | không thể do năng lực | 
| 4 6 2 3 3 | 2 2 | ranh giới đóng gói chặt chẽ | 

## Vỏ cạnh 

Khi nào$a$không chia hết cho$c$, cần phải chia trần. Ví dụ,$a = 5, c = 2$cho$x = 3$. Sử dụng phép chia số nguyên mà không điều chỉnh sẽ cho kết quả 2 không chính xác, không bao gồm tất cả các bài giảng. 

Khi năng lực bằng đúng hạng mục yêu cầu thì giải pháp phải chấp nhận. Ví dụ, nếu$x + y = k$, nó vẫn hợp lệ và việc từ chối đẳng thức sẽ loại bỏ các câu trả lời đúng một cách không chính xác. 

Khi một trong hai$a = 0$hoặc$b = 0$, công thức vẫn hoạt động chính xác, tạo ra 0 bút hoặc bút chì như mong đợi vì không có mục nào cần thiết cho danh mục đó.
