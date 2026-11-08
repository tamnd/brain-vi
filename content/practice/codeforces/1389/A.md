---
title: "CF 1389A - Sự cố LCM"
description: "Chúng ta được cho nhiều dãy số nguyên độc lập. Đối với mỗi phạm vi $[l, r]$, chúng ta cần chọn hai số nguyên khác nhau $x$ và $y$ trong khoảng này sao cho bội số chung nhỏ nhất của chúng cũng nằm trong cùng một khoảng."
date: "2026-06-16T14:51:59+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1389
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 92 (Rated for Div. 2)"
rating: 800
weight: 1389
solve_time_s: 328
verified: false
draft: false
---

[CF 1389A - Sự cố LCM](https://codeforces.com/problemset/problem/1389/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 5 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho nhiều dãy số nguyên độc lập. Đối với mỗi phạm vi$[l, r]$, ta cần chọn hai số nguyên khác nhau$x$Và$y$trong khoảng này sao cho bội chung nhỏ nhất của chúng cũng nằm trong cùng khoảng. 

Vì vậy, nhiệm vụ không phải là tối ưu hóa bất cứ thứ gì như tổng hay hiệu, mà là tìm một cặp “ổn định” trong phép toán LCM: cả hai đầu vào đều nằm trong phạm vi và LCM của chúng không thoát khỏi phạm vi đó. 

Đầu ra là một cặp như vậy hoặc một tuyên bố rằng không có cặp hợp lệ nào tồn tại. Chúng tôi có thể tự do in bất kỳ giải pháp hợp lệ nào nếu có nhiều giải pháp. 

Những ràng buộc là cực kỳ quan trọng ở đây. Chúng tôi có thể có tới$10^4$các trường hợp thử nghiệm và mỗi điểm cuối của phạm vi có thể lớn bằng$10^9$. Điều này loại trừ bất kỳ giải pháp nào thử tất cả các cặp hoặc thậm chí tất cả các ứng cử viên trong một phạm vi. Ngay cả việc lặp qua một khoảng lớn cho mỗi trường hợp kiểm thử cũng không thể thực hiện được khi phạm vi trường hợp xấu nhất trải rộng trên toàn bộ$10^9$tỉ lệ. 

Kiểm tra cặp ngây thơ sẽ kiểm tra tất cả$(x, y)$TRONG$[l, r]$, tính toán LCM mỗi lần. Đó là bậc hai về kích thước của khoảng, điều này ngay lập tức không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi khoảng quá chặt. Ví dụ, nếu$r = l + 1$, chúng ta chỉ có một cặp có thể$(l, l+1)$. Đôi khi điều này hoạt động, đôi khi nó thất bại. Ví dụ, trong$[88, 89]$, cặp duy nhất là 88 và 89, và vì chúng nguyên tố cùng nhau nên LCM của chúng là$88 \cdot 89$, nằm ngoài phạm vi, vì vậy câu trả lời phải là$-1, -1$. 

Một trường hợp cạnh khác là khi khoảng nhỏ nhưng chứa các số có quan hệ chia số, chẳng hạn như$[2, 4]$. Ở đây đang chọn$x=2, y=4$hoạt động vì$\mathrm{LCM}(2,4)=4$, nằm trong khoảng. 

Khó khăn cốt lõi là quyết định khi nào một cặp “đóng theo LCM” như vậy tồn tại mà không liệt kê các ứng cử viên. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cặp$(x, y)$trong phạm vi$[l, r]$và kiểm tra xem$\mathrm{LCM}(x, y)$nằm trong cùng một phạm vi. Điều này đúng vì nó trực tiếp tuân theo điều kiện trong bài toán. Tuy nhiên, nếu khoảng có kích thước$k$, điều này đòi hỏi$O(k^2)$kiểm tra và mỗi kiểm tra đều liên quan đến việc tính toán LCM. Trong trường hợp xấu nhất$k$có thể lớn nên không thể thực hiện được phương pháp này. 

Quan sát quan trọng là chúng ta thực sự không cần phải tìm kiếm bên trong khoảng. Cấu trúc của bài toán gợi ý rõ ràng rằng cặp ứng cử viên hữu ích duy nhất là một cái gì đó cực kỳ đơn giản. Nếu chúng ta nhìn vào những ví dụ thành công như$(2,4)$hoặc$(6,7)$, chúng tôi nhận thấy một mẫu: các số liên tiếp hoặc một số là bội số của số kia. 

Nếu chúng ta thử cặp$(l, 2l)$, nó trở nên thú vị. LCM của$l$Và$2l$luôn luôn là$2l$, từ$2l$đã là bội số của$l$. Điều này ngay lập tức đảm bảo rằng LCM bằng phần tử lớn hơn. Vì vậy điều kiện rút gọn thành việc kiểm tra xem$2l \le r$. 

Điều này thu gọn toàn bộ vấn đề thành một nỗ lực xây dựng duy nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l)^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc các giá trị$l$Và$r$. Chúng xác định phạm vi được phép cho cả số và cho LCM. 
2. Thử xây dựng một cặp ứng cử viên bằng số nhỏ nhất trong dãy. Chúng tôi thiết lập$x = l$bởi vì bất kỳ giải pháp hợp lệ nào cũng có thể được chuyển xuống dưới mà không làm mất tính khả thi khi có thể và bắt đầu từ mức tối thiểu sẽ mang lại cơ hội tốt nhất để giữ LCM trong khoảng. 
3. Đặt$y = 2l$. Lựa chọn này đảm bảo một cấu trúc LCM rõ ràng, vì một số chia hết cho số kia. Chúng tôi tránh tính toán gcd hoặc LCM một cách rõ ràng. 
4. Kiểm tra xem$y \le r$. Nếu điều này đúng thì cả hai số đều nằm trong phạm vi và LCM của chúng chính xác$y$, cũng nằm trong phạm vi. 
5. Nếu điều kiện không thành công, xuất ra$-1, -1$, vì không có cấu trúc nào thuộc loại này có thể vừa trong khoảng. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là bất kỳ cặp hợp lệ nào cũng phải có một số chia cho số kia hoặc tạo ra LCM vượt quá kích thước khoảng. Việc xây dựng$(l, 2l)$là mối quan hệ bội số không tầm thường tối thiểu có thể có. Nếu ngay cả mức nhân đôi nhỏ nhất này cũng vượt quá$r$, sau đó bất kỳ cặp nào khác liên quan đến$l$hoặc giá trị lớn hơn sẽ chỉ tạo ra LCM lớn hơn nữa. Do đó, không có cặp hợp lệ nào có thể tồn tại. Ngược lại, khi$2l \le r$, cặp$(l, 2l)$luôn có giá trị vì đảm bảo tính chia hết$\mathrm{LCM}(l, 2l)=2l$, nằm trong giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
out = []

for _ in range(t):
    l, r = map(int, input().split())
    
    if 2 * l <= r:
        out.append(f"{l} {2*l}")
    else:
        out.append("-1 -1")

print("\n".join(out))
```Giải pháp được thúc đẩy hoàn toàn bởi sự quan sát rằng ứng cử viên có ý nghĩa duy nhất là$(l, 2l)$. Mã này tránh mọi tính toán của gcd hoặc LCM vì cấu trúc chia hết được thể hiện rõ ràng bằng cách xây dựng. 

Điểm tinh tế duy nhất là đảm bảo phép nhân số nguyên không vượt quá giới hạn điển hình, nhưng Python xử lý các số nguyên lớn một cách an toàn. Séc$2*l \le r$là logic quyết định đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
l = 1, r = 1337
```Chúng tôi thử nghiệm việc xây dựng$x = 1$,$y = 2$. 

| Bước | x | y | Tình trạng | 
| --- | --- | --- | --- | 
| Xây dựng | 1 | 2 | - | 
| Kiểm tra | 1 | 2 | 2 1337 | 

Vì điều kiện được giữ, nên chúng ta xuất ra$1, 2$. LCM là 2, nằm trong phạm vi. 

Điều này cho thấy trường hợp khoảng đủ lớn để nhân đôi phần tử nhỏ nhất vẫn hợp lệ. 

### Ví dụ 2 

đầu vào:```
l = 88, r = 89
```Chúng tôi xây dựng$x = 88$,$y = 176$. 

| Bước | x | y | Tình trạng | 
| --- | --- | --- | --- | 
| Xây dựng | 88 | 176 | - | 
| Kiểm tra | 88 | 176 | 176 89 thất bại | 

Vì điều kiện không thành công nên chúng ta xuất ra$-1, -1$. Bất kỳ cặp nào khác đều không thể thực hiện được vì các cặp duy nhất có sẵn không tồn tại hoặc tạo ra LCM nằm ngoài phạm vi. 

Điều này chứng tỏ trường hợp thất bại trong khoảng thời gian chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học không đổi | 
| Không gian |$O(1)$| Chỉ một số biến được lưu trữ bất kể kích thước đầu vào | 

Giải pháp dễ dàng xử lý lên đến$10^4$trường hợp thử nghiệm vì nó không thực hiện lặp lại trong phạm vi$[l, r]$. Mọi trường hợp thử nghiệm đều được giải quyết bằng một lần kiểm tra số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        l, r = map(int, input().split())
        if 2 * l <= r:
            res.append(f"{l} {2*l}")
        else:
            res.append("-1 -1")
    return "\n".join(res)

# provided samples
assert run("""4
1 1337
13 69
2 4
88 89
""") == """1 2
13 26
2 4
-1 -1"""

# minimum size interval
assert run("""1
1 2
""") == "1 2"

# tight impossible interval
assert run("""1
5 6
""") == "-1 -1"

# large exact boundary
assert run("""1
10 20
""") == "10 20"

# large range where l is big
assert run("""1
1000000000 1000000000
""") == "-1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 2 | trường hợp hợp lệ nhỏ nhất | 
| 5 6 | -1 -1 | không có sự phù hợp nhân đôi hợp lệ | 
| 10 20 | 10 20 | trường hợp thành công tầm trung | 
| 1e9 1e9 | -1 -1 | trường hợp tràn ranh giới | 

## Vỏ cạnh 

Khi khoảng chỉ chứa hai số liên tiếp như$[88, 89]$, công trình tạo ra$y = 2l = 176$, ngay lập tức vượt quá phạm vi. Thuật toán bác bỏ chính xác trường hợp này vì điều kiện$2l \le r$thất bại, vì vậy nó xuất ra$-1, -1$. Bất kỳ cặp thay thế nào cũng không thể xảy ra vì không có số nguyên thứ hai tồn tại trong phạm vi có thể tạo thành LCM giới hạn với 88. 

Khi khoảng thời gian chính xác$[l, l+1]$, cặp duy nhất có thể bị ép buộc và thuật toán ngầm kiểm tra cặp đó thông qua quy tắc nhân đôi. Từ$2l > l+1$cho tất cả$l \ge 1$, mọi khoảng như vậy đều bị từ chối một cách chính xác trừ khi nó suy biến thành một cấu trúc chia hết đặc biệt như$[2,4]$, Ở đâu$2l = 4 \le r$, tạo ra một giải pháp hợp lệ.
