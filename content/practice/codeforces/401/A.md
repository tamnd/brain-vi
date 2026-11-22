---
title: "CF 401A - Vanya và Thẻ"
description: "Vanya đã tìm thấy một tập hợp con các quân bài của mình, mỗi quân bài được gắn nhãn một số nguyên nằm giữa $-x$ và $x$. Anh ta muốn tổng số điểm của tất cả các quân bài của mình bằng 0. Chúng ta cần xác định số lượng quân bài bổ sung tối thiểu mà anh ta phải tìm để đạt được tổng bằng 0 đó."
date: "2026-06-07T01:14:54+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 401
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 235 (Div. 2)"
rating: 800
weight: 401
solve_time_s: 255
verified: true
draft: false
---

[CF 401A - Vanya và Thẻ](https://codeforces.com/problemset/problem/401/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 4 phút 15 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vanya đã tìm thấy một tập hợp con các quân bài của mình, mỗi quân bài được đánh dấu một số nguyên nằm giữa$-x$Và$x$. Anh ta muốn tổng số điểm của tất cả các quân bài của mình bằng 0. Chúng ta cần xác định số lượng quân bài bổ sung tối thiểu mà anh ta phải tìm để đạt được tổng bằng 0 đó. 

Đầu vào cho hai số,$n$Và$x$, đại diện cho số lượng quân bài anh ta đã tìm thấy và giá trị tuyệt đối tối đa được phép trên một quân bài. Sau đó một danh sách$n$số nguyên mô tả giá trị của các thẻ tìm thấy. Đầu ra là một số nguyên duy nhất: số lượng thẻ mới tối thiểu cần thiết để cân bằng tổng về 0. 

Các hạn chế là nhỏ:$n \le 1000$Và$x \le 1000$. Điều này cho phép các thuật toán chạy trong thời gian tuyến tính tương ứng với$n$. Tổng giá trị tuyệt đối lớn nhất mà Vanya có thể có từ các thẻ tìm được là$1000 \times 1000 = 10^6$, được xử lý an toàn với các loại số nguyên tiêu chuẩn. 

Một trường hợp tinh vi là khi tổng số thẻ tìm được đã chia hết cho$x$hoặc nhỏ hơn$x$. Ví dụ: nếu Vanya tìm thấy một thẻ có giá trị 7 và$x = 3$, anh ta không thể cân bằng nó bằng một thẻ duy nhất, vì vậy anh ta phải sử dụng nhiều thẻ. Các giải pháp bất cẩn có thể cho rằng số tối thiểu luôn là 1 nếu tổng khác 0, điều này sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là thử tất cả các kết hợp của các thẻ bổ sung giữa$-x$Và$x$cho đến khi tổng số tiền bằng không. Về nguyên tắc, điều này đúng vì chúng ta luôn có thể chọn đủ quân bài để bù cho bất kỳ số tiền nào, nhưng điều đó không thực tế. Ví dụ: nếu tổng là 1000 và$x = 1$, chúng ta sẽ cần khám phá các chuỗi lên tới 1000 thẻ, có tính chất bùng nổ về mặt tổ hợp. 

Thông tin chi tiết quan trọng cần lưu ý rằng điều duy nhất quan trọng là tổng tuyệt đối của các thẻ tìm thấy. Chúng tôi có thẻ vô hạn có giá trị bất kỳ từ$-x$ĐẾN$x$, do đó bài toán rút gọn thành: cho một tổng mục tiêu$S$, tối đa bao nhiêu lá bài có giá trị tuyệt đối$x$cần thiết để đạt được$S$? Số lượng thẻ tối thiểu chỉ đơn giản là mức trần của$|S| / x$. Điều này hiệu quả vì chúng ta luôn có thể chọn thẻ lớn nhất được phép liên tục theo hướng ngược lại với tổng cho đến khi chúng ta đạt đến số 0. 

Điều này chuyển đổi vấn đề từ tìm kiếm tổ hợp thành một phép tính số học đơn giản, chạy trong$O(n)$để tính tổng của đầu vào và$O(1)$để tính số thẻ cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các trình tự) | Hàm mũ | O(?) | Quá chậm | 
| Tối ưu (chia tổng & trần) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$Và$x$. Chúng đại diện cho số lượng thẻ được tìm thấy và giá trị thẻ tối đa. Chúng tôi sẽ cần$x$để xác định mức điều chỉnh lớn nhất cho một thẻ mà chúng tôi có thể thực hiện. 
2. Đọc mảng$n$số nguyên đại diện cho các thẻ tìm thấy. Tính tổng của chúng$S$. Mục tiêu bây giờ là giảm số tiền này xuống bằng không. 
3. Lấy giá trị tuyệt đối của$S$, vì hướng điều chỉnh không phù hợp; chúng ta có thể thêm thẻ tích cực hoặc tiêu cực tùy theo dấu của$S$. 
4. Chia$|S|$qua$x$sử dụng phép chia số nguyên để đếm xem cần bao nhiêu thẻ kích thước đầy đủ. Nếu như$|S|$không chia hết cho$x$, chúng ta cần thêm một thẻ nữa để trang trải phần còn lại. 
5. Xuất ra tổng số lá bài đã tính. 

Tại sao nó hoạt động: bằng cách liên tục lấy các thẻ có giá trị tuyệt đối tối đa được phép, chúng tôi giảm thiểu số lượng thẻ cần thiết. Điều bất biến là sau mỗi lần thêm thẻ, số tiền còn lại để sửa sẽ giảm tối đa$x$. Việc sử dụng vách ngăn trần đảm bảo rằng bất kỳ phần nào còn lại nhỏ hơn$x$được tính là một thẻ phụ. Điều này đảm bảo số lượng tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, x = map(int, input().split())
cards = list(map(int, input().split()))

total = sum(cards)
needed_cards = (abs(total) + x - 1) // x  # ceiling division
print(needed_cards)
```Đầu tiên chúng ta đọc dữ liệu đầu vào và tính tổng của các thẻ tìm được. biểu hiện`(abs(total) + x - 1) // x`tính toán trần`abs(total) / x`hiệu quả mà không cần hoạt động dấu phẩy động. Điều này tránh các lỗi xảy ra với phép chia số nguyên đơn giản khi tổng không chia hết cho$x$. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 2
-1 1 2
```| Bước | tổng cộng | cơ bụng (tổng cộng) | thẻ cần thiết | 
| --- | --- | --- | --- | 
| ban đầu | 2 | 2 | (2 + 2 - 1)//2 = 1 | 

Giải thích: Tổng số quân bài tìm được là 2. Giá trị quân bài tối đa là 2 nên một quân bài có -2 là đủ. 

**Mẫu 2** 

đầu vào:```
3 3
1 1 1
```| Bước | tổng cộng | cơ bụng (tổng cộng) | thẻ cần thiết | 
| --- | --- | --- | --- | 
| ban đầu | 3 | 3 | (3 + 3 - 1)//3 = 1 | 

Ở đây, tổng chia hết cho giá trị thẻ tối đa nên chỉ cần một thẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính tổng n thẻ một lần. | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách n số nguyên. | 

Đối với các ràng buộc đã cho, n ≤ 1000, giải pháp này chạy thoải mái trong giới hạn 1 giây. Việc sử dụng bộ nhớ cũng ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, x = map(int, input().split())
    cards = list(map(int, input().split()))
    total = sum(cards)
    return str((abs(total) + x - 1) // x)

# provided samples
assert run("3 2\n-1 1 2\n") == "1", "sample 1"
assert run("3 3\n1 1 1\n") == "1", "sample 2"

# custom cases
assert run("1 1\n7\n") == "7", "single card, large sum"
assert run("4 5\n5 5 5 5\n") == "4", "sum divisible by x"
assert run("2 10\n-3 4\n") == "1", "sum small, single card covers"
assert run("5 1\n1 -1 1 -1 1\n") == "1", "alternating sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n7 | 7 | Thẻ đơn không bằng 0, cần nhiều thẻ | 
| 4 5\n5 5 5 5 | 4 | Tổng chia hết cho thẻ tối đa | 
| 2 10\n-3 4 | 1 | Tổng nhỏ với x lớn hơn số dư | 
| 5 1\n1 -1 1 -1 1 | 1 | Giá trị thay thế, xử lý phần còn lại | 

## Vỏ cạnh 

Nếu tổng số quân bài bằng 0, thuật toán ngay lập tức trả về 0 vì không cần thẻ mới. Ví dụ, đầu vào`2 5\n3 -3`cho`total = 0`, Vì thế`(abs(0) + 5 - 1) // 5 = 0`. 

Nếu tổng tuyệt đối nhỏ hơn`x`, thuật toán sẽ đưa ra chính xác 1, bao gồm trường hợp chỉ cần một thẻ duy nhất. Đối với đầu vào`1 5\n3`,`abs(3)/5`nhỏ hơn 1 nên phép chia trần cho ra 1. 

Trong tất cả các trường hợp khác, việc chia trần đảm bảo chúng tôi luôn chọn số lượng thẻ tối thiểu cần thiết mà không vượt quá số tiền yêu cầu. 

Cách tiếp cận này là trực tiếp, xử lý tất cả các trường hợp khó khăn và chia tỷ lệ một cách tầm thường theo giới hạn đầu vào.
