---
title: "CF 978E - Hệ thống video xe buýt"
description: "Chúng ta được cung cấp một chuỗi các thay đổi được ghi lại về số lượng hành khách trên xe buýt. Mỗi giá trị cho chúng ta biết số lượng hành khách thay đổi như thế nào sau một điểm dừng, vì vậy nếu chúng ta biểu thị số lượng hành khách bắt đầu bằng $x$, thì sau mỗi điểm dừng, chúng ta liên tục cộng các delta đã cho và thu được…"
date: "2026-06-17T01:23:31+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 1400
weight: 978
solve_time_s: 71
verified: true
draft: false
---

[CF 978E - Hệ thống video xe buýt](https://codeforces.com/problemset/problem/978/E) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các thay đổi được ghi lại về số lượng hành khách trên xe buýt. Mỗi giá trị cho chúng ta biết số lượng hành khách thay đổi như thế nào sau khi dừng, vì vậy nếu chúng ta biểu thị số lượng hành khách bắt đầu bằng$x$, thì sau mỗi điểm dừng, chúng tôi liên tục thêm các vùng đồng bằng đã cho và thu được quy trình tính tổng tiền tố đang chạy. 

Xe buýt có một ràng buộc cứng rắn: tại mọi thời điểm, số lượng hành khách phải ở trong khoảng$0$Và$w$, bao gồm. Điều này không chỉ áp dụng sau mỗi điểm dừng được ghi mà còn ngay trước điểm dừng đầu tiên (trong đó số đếm là$x$). 

Nhiệm vụ không phải là xây dựng lại một kịch bản hợp lệ mà là đếm xem có bao nhiêu giá trị nguyên của số hành khách ban đầu$x$hợp lệ để tất cả các tổng tiền tố trung gian vẫn nằm trong phạm vi cho phép. 

Những hạn chế đã gợi ý rằng một$O(n)$hoặc$O(n \log n)$giải pháp là cần thiết. Với$n \le 1000$, thậm chí một$O(n^2)$phương pháp tiếp cận có thể đạt được, nhưng bất cứ điều gì cố gắng mô phỏng trực tiếp tất cả các giá trị bắt đầu có thể có trên một phạm vi rộng lên đến$10^9$là không thể. 

Một điểm tinh tế quan trọng là giá trị ban đầu$x$không bị giới hạn bởi đầu vào, chỉ ngầm định bởi tính khả thi. Một người đọc ngây thơ có thể thử lặp lại tất cả các giá trị bắt đầu có thể có từ$0$ĐẾN$w$, nhưng phạm vi khả thi thực tế có thể dịch chuyển ra ngoài phạm vi đó ở các trạng thái trung gian do tổng tiền tố âm. 

Một trường hợp thất bại phổ biến là do bỏ qua các ràng buộc trung gian. Ví dụ: nếu tổng tiền tố giảm xuống dưới 0, giá trị bắt đầu có vẻ hợp lệ trên toàn cầu vẫn có thể vi phạm các ràng buộc ở một số bước. Tương tự, nếu tổng tiền tố vượt quá một phạm vi nhất định, điều đó cũng làm mất hiệu lực thời điểm bắt đầu. Độ chính xác phụ thuộc vào việc theo dõi toàn bộ quỹ đạo tổng tiền tố chứ không chỉ giá trị cuối cùng của nó. 

## Phương pháp tiếp cận 

Quan sát cốt lõi là số lượng hành khách ở bước$i$có thể được viết là:$$x + p_i$$Ở đâu$p_i$là tổng tiền tố của những thay đổi được ghi lại lên đến$i$. 

Do đó mọi ràng buộc đều trở thành:$$0 \le x + p_i \le w$$Việc sắp xếp lại đưa ra một ràng buộc về khoảng thời gian trên$x$:$$-p_i \le x \le w - p_i$$Vì vậy, mỗi tổng tiền tố đóng góp một khoảng giá trị bắt đầu hợp lệ. Câu trả lời cuối cùng là giao điểm của tất cả các khoảng này trên tất cả$i$, bao gồm tổng tiền tố ẩn$p_0 = 0$. 

Ý tưởng bạo lực sẽ là kiểm tra mọi khả năng$x$từ$0$ĐẾN$w$và mô phỏng tất cả các bước, chi phí$O(wn)$, quá lớn vì$w$có thể$10^9$. Quan sát ở trên biến vấn đề thành việc duy trì giao điểm của các khoảng, giảm mọi thứ xuống chỉ còn theo dõi hai giá trị. 

Giao điểm của tất cả các ràng buộc bản thân nó là một khoảng duy nhất$[L, R]$, được cập nhật dần dần. Mỗi tổng tiền tố thu hẹp khoảng này. Nếu tại bất kỳ thời điểm nào khoảng trống thì không có giá trị bắt đầu hợp lệ nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nw)$|$O(1)$| Quá chậm | 
| Giao lộ khoảng cách |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$p$dưới dạng tổng tiền tố đang chạy của các thay đổi. 

1. Khởi tạo$p = 0$và đặt phạm vi hợp lệ cho$x$BẰNG$L = 0$,$R = w$. Điều này phản ánh thực tế là ban đầu xe buýt phải đáp ứng được hạn chế về năng lực. 
2. Đối với mỗi thay đổi được ghi lại$a_i$, cập nhật tổng tiền tố$p = p + a_i$. Điều này thể hiện sự dịch chuyển ròng từ trạng thái ban đầu sang bước$i$. 
3. Chuyển đổi điều kiện$0 \le x + p \le w$vào những hạn chế về$x$, tạo ra khoảng$[-p, w - p]$. 
4. Giao khoảng này với phạm vi hợp lệ hiện tại:$L = \max(L, -p)$,$R = \min(R, w - p)$. 

Điều này đảm bảo cho đến nay chúng tôi chỉ giữ lại các giá trị bắt đầu thỏa mãn tất cả các ràng buộc. 
5. Nếu tại bất kỳ thời điểm nào$L > R$, dừng sớm vì không tồn tại giá trị ban đầu hợp lệ. 
6. Sau khi xử lý tất cả các bước, câu trả lời là$R - L + 1$, vì tất cả các số nguyên trong khoảng cuối cùng đều là số lượng hành khách ban đầu hợp lệ. 

### Tại sao nó hoạt động 

Mỗi bước áp đặt một ràng buộc tuyến tính trên giá trị ban đầu$x$. Vì mọi ràng buộc đều có dạng$x \in [\ell_i, r_i]$, tập hợp lệ là giao điểm của các khoảng. Giao điểm của các khoảng có tính liên kết và có thể được duy trì tăng dần mà không làm mất thông tin. Thuật toán bảo toàn chính xác tập hợp tất cả các giá trị bắt đầu khả thi sau mỗi bước, do đó không có giải pháp hợp lệ nào bị loại bỏ và không có giải pháp không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, w = map(int, input().split())
    a = list(map(int, input().split()))

    prefix = 0
    L, R = 0, w

    for x in a:
        prefix += x

        L = max(L, -prefix)
        R = min(R, w - prefix)

        if L > R:
            print(0)
            return

    print(max(0, R - L + 1))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một tổng tiền tố đang chạy và cập nhật phạm vi khả thi ở mỗi bước. Giới hạn$L$Và$R$luôn đại diện cho giao điểm của tất cả các ràng buộc được thấy cho đến nay. Việc thoát sớm khi$L > R$tránh tính toán không cần thiết một khi tính khả thi bị phá vỡ. 

Một điểm tinh tế là đảm bảo rằng câu trả lời cuối cùng được đưa ra một cách chính xác: nếu các ràng buộc nhất quán,$R - L + 1$đếm tất cả các giá trị nguyên ban đầu. Nếu phát sinh sự mâu thuẫn thì hàm đã trả về 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
2 1 -3
```Chúng tôi theo dõi tổng tiền tố và khoảng thời gian. 

| Bước | a_i | tiền tố p | khoảng hợp lệ cho x [-p, w-p] | hiện tại [L, R] | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | [0, 5] | [0, 5] | 
| 1 | 2 | 2 | [-2, 3] | [0, 3] | 
| 2 | 1 | 3 | [-3, 2] | [0, 2] | 
| 3 | -3 | 0 | [0, 5] | [0, 2] | 

Câu trả lời cuối cùng là$2 - 0 + 1 = 3$. 

Dấu vết này cho thấy các ràng buộc thu hẹp dần khoảng thời gian bắt đầu khả thi mà không cần liệt kê bất kỳ giá trị ban đầu nào. 

### Ví dụ 2 

đầu vào:```
4 4
1 2 -1 -2
```| Bước | a_i | tiền tố p | khoảng [-p, w-p] | [L, R] | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | 0 | [0, 4] | [0, 4] | 
| 1 | 1 | 1 | [-1, 3] | [0, 3] | 
| 2 | 2 | 3 | [-3, 1] | [0, 1] | 
| 3 | -1 | 2 | [-2, 2] | [0, 1] | 
| 4 | -2 | 0 | [0, 4] | [0, 1] | 

Câu trả lời là$2$, tương ứng với giá trị ban đầu$0$Và$1$. 

Ví dụ này cho thấy rằng ngay cả khi tổng tiền tố cuối cùng trở về 0, các ràng buộc trung gian vẫn hạn chế các giá trị ban đầu hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi điểm dừng cập nhật tổng tiền tố và khoảng một lần | 
| Không gian |$O(1)$| Chỉ tổng tiền tố và hai giới hạn được lưu trữ | 

Quét tuyến tính trên tối đa 1000 phần tử là đủ nhanh và không cần cấu trúc dữ liệu bổ sung. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite

    n, w = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    prefix = 0
    L, R = 0, w

    for x in a:
        prefix += x
        L = max(L, -prefix)
        R = min(R, w - prefix)
        if L > R:
            return "0\n"

    return str(max(0, R - L + 1)) + "\n"

# provided sample
assert run("3 5\n2 1 -3\n") == "3\n"

# all zero changes
assert run("4 10\n0 0 0 0\n") == "11\n"

# immediate overflow
assert run("2 3\n5 -10\n") == "0\n"

# tight range
assert run("3 4\n1 2 -1\n") == "2\n"

# boundary case
assert run("1 0\n0\n") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả không thay đổi | 11 | phạm vi đầy đủ vẫn hợp lệ | 
| tràn ngay lập tức | 0 | phát hiện sớm tính không khả thi | 
| phạm vi chặt chẽ | 2 | khoảng thời gian thu hẹp chính xác | 
| trường hợp ranh giới w=0 | 1 | xử lý năng lực suy thoái | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các thay đổi đều bằng 0. Trong trường hợp đó, tổng tiền tố vẫn bằng 0 xuyên suốt, vì vậy mọi giá trị bắt đầu từ$0$ĐẾN$w$là hợp lệ. Thuật toán giữ$L=0$Và$R=w$không đổi, tạo ra$w+1$, khớp với số đếm chính xác. 

Một trường hợp đặc biệt khác là khi các ràng buộc trở nên bất khả thi ngay lập tức. Ví dụ, nếu$w = 3$và sự thay đổi đầu tiên là$+5$, thì tổng tiền tố đã nằm ngoài phạm vi khả thi đối với bất kỳ giá trị bắt đầu nào. Khoảng thời gian trở thành$[L, R] = [0, 3] \cap [-5, -2]$, trống và thuật toán đưa ra kết quả bằng 0 một cách chính xác mà không cần xử lý thêm. 

Trường hợp tinh tế cuối cùng là khi tiền tố trung gian tính tổng các ràng buộc lực không thể nhìn thấy được từ tổng cuối cùng. Ví dụ hoạt động thứ hai chứng minh điều này: mặc dù tổng thay đổi là nhỏ nhưng sự tích lũy trung gian sẽ hạn chế những lần khởi động hợp lệ. Phương pháp giao cắt khoảng thời gian tự động nắm bắt điều này vì nó đánh giá các ràng buộc ở mọi tiền tố, không chỉ ở cuối.
