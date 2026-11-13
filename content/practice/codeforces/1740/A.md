---
title: "CF 1740A - Phân tích nhân tử N+M"
description: "Chúng ta có một số trường hợp thử nghiệm và mỗi trường hợp thử nghiệm bắt đầu bằng số nguyên tố $n$. Với mỗi $n$ như vậy, chúng ta phải chọn một số nguyên tố $m$ khác sao cho tổng $n + m$ không phải là số nguyên tố."
date: "2026-06-15T03:39:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1740
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 831 (Div. 1 + Div. 2)"
rating: 800
weight: 1740
solve_time_s: 482
verified: false
draft: false
---

[CF 1740A - Phân tích nhân tử N+M](https://codeforces.com/problemset/problem/1740/A) 

**Đánh giá:** 800 
**Tas:** giải thuật xây dựng, lý thuyết số 
**Thời gian giải:** 8m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số trường hợp thử nghiệm và mỗi trường hợp thử nghiệm bắt đầu bằng một số nguyên tố$n$. Đối với mỗi như vậy$n$, chúng ta phải chọn một số nguyên tố khác$m$sao cho tổng$n + m$không phải là nguyên tố. Bất kỳ số nguyên tố hợp lệ$m$trong phạm vi cho phép là có thể chấp nhận được và các trường hợp thử nghiệm khác nhau là độc lập. 

Nhiệm vụ cốt lõi không phải là tính toán tính nguyên tố nhiều lần một cách nặng nề mà là xây dựng một số nguyên tố đối tác hợp lệ cho mỗi số nguyên tố đầu vào. Ràng buộc đầu ra rất hào phóng:$m$có thể là bất kỳ số nguyên tố nào lên tới$10^5$và không có yêu cầu giảm thiểu hoặc tối ưu hóa nó ngoài giá trị hợp lệ. 

Ràng buộc$n \le 10^5$Và$t \le 10^4$gợi ý rằng các giải pháp về cơ bản phải có thời gian không đổi cho mỗi trường hợp thử nghiệm sau một số quá trình tiền xử lý. Bất kỳ điều gì liên quan đến việc kiểm tra tính nguyên thủy lặp đi lặp lại cho mỗi trường hợp kiểm thử vẫn chỉ ổn nếu việc kiểm tra được thực hiện$O(1)$, nhưng tìm kiếm lặp lại trên các số nguyên tố cho mỗi truy vấn sẽ quá chậm trong trường hợp xấu nhất. 

Một trường hợp khó phát hiện khi$n$là nhỏ. Nếu như$n = 2$, sau đó chọn các số nguyên tố nhỏ như$m = 2$hoặc$m = 3$ngay lập tức tạo ra số tiền$4$hoặc$5$, và chỉ một trong số đó là không nguyên tố. Một cách tiếp cận ngây thơ luôn chọn số nguyên tố đầu tiên trong danh sách có thể thất bại nếu nó giả định tất cả các tổng đều hoạt động giống nhau. Một trường hợp góc khác là khi$n$lớn và gần$10^5$, nơi các chiến lược xây dựng bất cẩn có thể vượt quá giới hạn cho$n + m$, mặc dù tính nguyên tố của tổng là mối quan tâm duy nhất chứ không phải độ lớn của nó. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi số nguyên tố$m$và kiểm tra xem$n + m$là nguyên tố. Đối với mỗi trường hợp thử nghiệm, điều này có thể quét tất cả các số nguyên tố lên tới$10^5$và với mỗi ứng viên hãy tính tính nguyên tố của$n + m$. Ngay cả khi tính nguyên tố được tính toán trước bằng sàng, chúng tôi vẫn sẽ kiểm tra tới khoảng 9.500 số nguyên tố cho mỗi trường hợp thử nghiệm, dẫn đến khoảng$10^8$kiểm tra trong trường hợp xấu nhất trên tất cả các bài kiểm tra, điều này là không cần thiết. 

Quan sát quan trọng là chúng ta không cần phải tìm kiếm. Chúng ta chỉ cần đảm bảo rằng ít nhất một lựa chọn đơn giản có hiệu quả. Cấu trúc số nguyên tố ngay lập tức đưa ra một lối tắt: nếu chúng ta chọn một số nguyên tố cố định nhỏ$m$, chúng ta chỉ cần kiểm tra xem$n + m$là tổng hợp. Từ$n$luôn là số nguyên tố nên ta có thể khai thác tính chẵn lẻ. Ngoại trừ$n = 2$, mọi số nguyên tố đều lẻ. Tổng của hai số lẻ là số chẵn và lớn hơn 2 nên là hợp số. Điều này có nghĩa là với mọi số nguyên tố lẻ$n$, đang chọn$m = 3$luôn tạo ra một tổng hợp. 

Trường hợp có vấn đề duy nhất là$n = 2$. Đây$n$là số chẵn, vì vậy$2 + 3 = 5$là số nguyên tố, thất bại. Chúng ta cần một số nguyên tố nhỏ cố định khác. Lựa chọn$m = 2$cho$2 + 2 = 4$, đó là hợp chất. Điều này giải quyết trường hợp ngoại lệ duy nhất. 

Vì vậy, giải pháp giảm xuống thành lựa chọn có điều kiện theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force trên các số nguyên tố |$O(t \cdot \pi(N))$|$O(N)$| Quá chậm | 
| Thi công cố định (2 hoặc 3) |$O(t)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số nguyên tố$n$. Chúng tôi coi nó hoàn toàn là một số nguyên; không cần phân tích nhân tử. 
2. Kiểm tra xem$n = 2$. Đây là số nguyên tố chẵn duy nhất và hoạt động khác với tất cả các số khác vì nó phá vỡ cấu trúc lẻ-cộng-lẻ. 
3. Nếu$n = 2$, đầu ra$m = 2$. Điều này đảm bảo$n + m = 4$, không phải là số nguyên tố. 
4. Ngược lại, xuất ra$m = 3$. Vì tất cả các số nguyên tố trừ 2 đều là số lẻ, điều này đảm bảo$n + 3$là số chẵn và lớn hơn 2, do đó là hợp số. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào đặc tính cấu trúc của số nguyên tố. Mọi số nguyên tố lớn hơn 2 đều là số lẻ. Khi cộng hai số lẻ thì kết quả là số chẵn. Số chẵn lớn hơn 2 không thể là số nguyên tố vì nó có ước số là 2. Vì vậy, với mọi số nguyên tố lẻ$n$, đang chọn$m = 3$lực lượng$n + m$để được tổng hợp. Ngoại lệ duy nhất là$n = 2$, trong đó tính chẵn lẻ bị phá vỡ và chúng tôi chọn rõ ràng một số nguyên tố khác để tránh tạo ra một tổng số nguyên tố khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n == 2:
            print(2)
        else:
            print(3)

if __name__ == "__main__":
    solve()
```Mã theo sau việc xây dựng trực tiếp. Điểm quyết định duy nhất là kiểm tra sự bằng nhau với 2. Mọi thứ khác đều là đầu ra không đổi. 

Một lỗi phổ biến là cố gắng “tìm kiếm” một giá trị hợp lệ$m$một cách linh hoạt, điều này là không cần thiết và có nguy cơ bị hết thời gian chờ. Một sai lầm nữa là quên điều đó$2 + 3 = 5$là số nguyên tố, điều này làm cho$m = 3$không hợp lệ cho$n = 2$. Nhánh rõ ràng xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
7
2
75619
```Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

| n | kiểm tra n=2 | đã chọn m | n + m | là n+m nguyên tố | 
| --- | --- | --- | --- | --- | 
| 7 | không | 3 | 10 | không | 
| 2 | vâng | 2 | 4 | không | 
| 75619 | không | 3 | 75622 | không | 

Bảng cho thấy chỉ cần một quy tắc có điều kiện duy nhất để xử lý tất cả các trường hợp. Đối với các số nguyên tố lẻ, tổng sẽ trở thành số chẵn và tự động không phải là số nguyên tố. 

### Ví dụ 2 

Hãy xem xét một bộ bổ sung nhỏ:```
4
2
3
5
11
```| n | kiểm tra n=2 | đã chọn m | n + m | là n+m nguyên tố | 
| --- | --- | --- | --- | --- | 
| 2 | vâng | 2 | 4 | không | 
| 3 | không | 3 | 6 | không | 
| 5 | không | 3 | 8 | không | 
| 11 | không | 3 | 14 | không | 

Điều này thể hiện hành vi thống nhất trên tất cả các số nguyên tố lẻ. Mỗi đầu ra cho$n \neq 2$thu gọn về cùng một cấu trúc, xác nhận rằng không cần tìm kiếm hoặc tính toán trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t)$| Một lần kiểm tra và in liên tục cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài biến | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì thậm chí$10^4$trường hợp thử nghiệm chỉ yêu cầu$10^4$so sánh số nguyên đơn giản và kết quả đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n == 2:
            out.append("2")
        else:
            out.append("3")
    return "\n".join(out) + "\n"

# provided samples
assert run("3\n7\n2\n75619\n") == "3\n2\n3\n"

# minimum input
assert run("1\n2\n") == "2\n"

# small odd primes
assert run("3\n3\n5\n7\n") == "3\n3\n3\n"

# mixed case
assert run("4\n2\n11\n2\n13\n") == "2\n3\n2\n3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 2 | 2 | trường hợp cạnh nhỏ nhất | 
| số nguyên tố lẻ nhỏ | 3 giây | tính đúng đắn của quy tắc chẵn lẻ | 
| xen kẽ 2 số nguyên tố lẻ | mẫu 2/3 | tính nhất quán trong nhiều trường hợp | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là$n = 2$. Nếu thuật toán xuất ra một cách mù quáng$m = 3$, nó tạo ra$5$, là số nguyên tố và do đó không hợp lệ. Nhánh rõ ràng đảm bảo xử lý chính xác. 

Ví dụ: với đầu vào:```
1
2
```thuật toán chọn$m = 2$, sản xuất$4$, đó là hợp chất. Logic tránh mọi sự phụ thuộc vào các giả định chẵn lẻ sẽ thất bại khi$n$là chẵn. 

Tất cả các số nguyên tố khác đều là số lẻ và việc xây dựng$m = 3$luôn buộc tổng số chẵn lớn hơn 2, đảm bảo tính chính xác mà không cần kiểm tra thêm.
