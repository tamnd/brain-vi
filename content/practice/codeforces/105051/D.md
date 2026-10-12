---
title: "CF 105051D - \u041f\u0438\u0440\u0430\u043c\u0438\u0434\u0430"
description: "Mỗi viên gạch đầu vào là một hình chữ nhật có kích thước cố định $wi nhân hi$. Từ bất kỳ viên gạch nào, bạn được phép “thu nhỏ” nó thành đúng một hình chữ nhật mới $a nhân b$, miễn là cả hai cạnh không tăng, nghĩa là $a le wi$ và $b le hi$."
date: "2026-06-28T01:01:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105051
codeforces_index: "D"
codeforces_contest_name: "2023-2024 \u0424\u0438\u043d\u0430\u043b \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb"
rating: 0
weight: 105051
solve_time_s: 58
verified: true
draft: false
---

[CF 105051D - \u041f\u0438\u0440\u0430\u043c\u0438\u0434\u0430](https://codeforces.com/problemset/problem/105051/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi viên gạch đầu vào là một hình chữ nhật có kích thước cố định$w_i \times h_i$. Từ bất kỳ viên gạch nào, bạn được phép “thu nhỏ” nó thành đúng một hình chữ nhật mới$a \times b$, miễn là cả hai bên không tăng, nghĩa là$a \le w_i$Và$b \le h_i$. Không có phép biến đổi nào khác được phép, vì vậy mỗi viên gạch ban đầu chỉ có thể tạo ra một hình chữ nhật nhỏ hơn có thể sử dụng được. 

Một kim tự tháp hợp lệ là một chồng chính xác$k$gạch, nơi$t$-lớp thứ từ trên xuống phải có kích thước hình vuông$(2t-1) \times (2t-1)$. Vậy lớp 1 là$1 \times 1$, lớp 2 là$3 \times 3$, lớp 3 là$5 \times 5$, vân vân. Mỗi lớp phải được hỗ trợ bằng cách chọn một trong các viên gạch đã cho và thu nhỏ nó nếu cần để phù hợp với kích thước hình vuông yêu cầu. 

Nhiệm vụ là tính chiều cao tối đa có thể$k$, nghĩa là số lượng lớn nhất trong số các lớp hình vuông bắt buộc này có thể được khớp bằng cách sử dụng các viên gạch có sẵn, mỗi viên gạch được sử dụng nhiều nhất một lần. 

Những ràng buộc đẩy chúng ta tới một$O(n \log n)$hoặc$O(n)$giải pháp. Với$n \le 10^5$, bất kỳ cách tiếp cận nào thử tất cả các phép gán giữa các khối và các lớp hoặc cố gắng so khớp thông qua tìm kiếm hai bên sẽ quá chậm nếu vượt quá khoảng$10^8$hoạt động. Chiến lược dựa trên sắp xếp hoặc tham lam là những ứng cử viên tự nhiên. 

Một vấn đề tế nhị là mỗi viên gạch chỉ có thể được sử dụng một lần và việc thu nhỏ là không đối xứng: một viên gạch$(w, h)$có thể sản xuất bất kỳ$(a, b)$với cả hai tọa độ đều bị giới hạn, nhưng nó không thể “xoay” hoặc phân chia một phần dung lượng giữa nhiều lớp. Một trường hợp khác là một viên gạch đủ lớn cho lớp cao hơn sẽ tự động hoạt động cho tất cả các lớp nhỏ hơn, do đó, việc phân bổ quá mức các viên gạch lớn cho các yêu cầu nhỏ có thể cản trở tính khả thi của các lớp lớn hơn. 

Ví dụ: nếu chúng ta cần các lớp$1 \times 1$Và$3 \times 3$, và có một$5 \times 5$gạch và một$3 \times 3$gạch, gán$5 \times 5$gạch để$1 \times 1$đúng cục bộ nhưng có thể cản trở việc phân công khả thi duy nhất cho$3 \times 3$nếu không có viên gạch nào phù hợp với nó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử gán các viên gạch cho các cấp độ kim tự tháp. Đối với mỗi cấp độ$t$, chúng tôi kiểm tra những viên gạch nào có thể được giảm xuống$(2t-1) \times (2t-1)$, sau đó tìm kiếm bài tập phù hợp ở tất cả các cấp độ. Điều này trở thành vấn đề đối sánh hai bên giữa các cấp độ và các viên gạch, trong đó tồn tại một cạnh nếu viên gạch có thể đáp ứng một cấp độ. Một kết quả phù hợp tối đa sẽ đưa ra câu trả lời. 

Điều này đúng, nhưng biểu đồ có tới$10^5$các nút ở một bên và có khả năng$10^5$mặt khác, với tối đa$10^{10}$các cạnh ẩn trong trường hợp xấu nhất nếu có nhiều viên gạch lớn. Ngay cả khi các cạnh được tạo nhanh chóng, việc chạy khớp hoặc luồng vẫn quá chậm. 

Quan sát chính là tính đơn điệu. Yêu cầu đối với các lớp tăng nghiêm ngặt: lớp$t+1$yêu cầu một hình vuông lớn hơn lớp$t$. Trong khi đó, mỗi viên gạch có một “công suất” được xác định bằng độ lớn của hình vuông mà nó có thể hỗ trợ, tức là$\min(w_i, h_i)$. Nếu chúng ta sắp xếp các viên gạch theo khả năng này, về cơ bản chúng ta đang cố gắng ghép các lớp được yêu cầu nhỏ nhất với những viên gạch đủ nhỏ nhất, một cách tham lam. 

Điều này chuyển đổi vấn đề thành: các giá trị bắt buộc đã cho$1, 3, 5, \dots$và năng lực sẵn có$c_i = \min(w_i, h_i)$, hãy tìm tiền tố dài nhất của các yêu cầu có thể được đáp ứng bằng cách gán cho mỗi yêu cầu một dung lượng riêng biệt có giá trị ít nhất bằng giá trị đó. Đây là một kết hợp tham lam tiêu chuẩn trên các mảng được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp / dòng chảy vũ lực |$O(n^3)$hoặc tệ hơn |$O(n^2)$| Quá chậm | 
| Sắp xếp tham lam + kết hợp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng viên gạch$(w_i, h_i)$thành một giá trị duy nhất$c_i = \min(w_i, h_i)$. Đây là chiều dài cạnh hình vuông lớn nhất mà viên gạch có thể đỡ một cách an toàn sau khi co lại. 
2. Sắp xếp tất cả năng lực$c_i$theo thứ tự không giảm để có thể đáp ứng các yêu cầu nhỏ trước. 
3. Khởi tạo con trỏ$i = 0$trên những viên gạch và một quầy$t = 1$, đại diện cho độ dài cạnh hình vuông cần thiết hiện tại$2t - 1$. 
4. Trong khi chúng tôi vẫn còn gạch và có thể đáp ứng yêu cầu hiện tại: 

1. Di chuyển$i$chuyển tiếp cho đến khi chúng tôi tìm thấy một viên gạch với$c_i \ge 2t - 1$. 
2. Nếu không có viên gạch đó thì dừng lại; chúng tôi không thể xây dựng các lớp tiếp theo. 
3. Gán viên gạch này vào cấp độ$t$và tăng cả hai$i$Và$t$. 
5. Đầu ra$t - 1$, đó là chiều cao tối đa có thể đạt được. 

Sự lựa chọn tham lam là luôn sử dụng viên gạch nhỏ nhất có thể đáp ứng được yêu cầu hiện tại. Điều này bảo tồn những viên gạch lớn hơn cho những yêu cầu lớn hơn trong tương lai và khó đáp ứng hơn. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, chúng tôi đều đáp ứng yêu cầu chưa được đáp ứng nhỏ nhất hiện tại$2t-1$với công suất nhỏ nhất có thể đáp ứng được. Thay vào đó, nếu chúng tôi sử dụng khối gạch lớn hơn mức cần thiết thì chúng tôi sẽ chỉ giảm tính linh hoạt cho các bước sau vì tất cả các yêu cầu trong tương lai đều lớn hơn. Bất kỳ phép gán tối ưu nào cũng có thể được chuyển đổi thành một phép gán tôn trọng thứ tự này mà không làm giảm số lớp phù hợp, bằng cách hoán đổi các phép gán bất cứ khi nào một viên gạch lớn hơn mức cần thiết được sử dụng cho lớp trước đó trong khi vẫn tồn tại một viên gạch khả thi nhỏ hơn. 

Lập luận trao đổi này đảm bảo rằng việc kết hợp tham lam không bao giờ cản trở một giải pháp khả thi mà một chiến lược tối ưu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    caps = []
    for _ in range(n):
        w, h = map(int, input().split())
        caps.append(min(w, h))

    caps.sort()

    t = 1
    i = 0
    ans = 0

    while i < n:
        need = 2 * t - 1

        while i < n and caps[i] < need:
            i += 1

        if i == n:
            break

        ans += 1
        i += 1
        t += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp nén từng viên gạch thành một giá trị có thể sử dụng duy nhất, giúp tránh phải xử lý trực tiếp các ràng buộc hai chiều. Việc sắp xếp đảm bảo chúng ta luôn xem xét các viên gạch yếu hơn trước tiên. Vòng lặp bên trong bỏ qua các viên gạch không sử dụng được và mỗi viên gạch được tiêu thụ tối đa một lần, do đó con trỏ chỉ di chuyển về phía trước. 

Một lỗi phổ biến là cố gắng ghép từ những viên gạch lớn nhất trở xuống, điều này có thể gây lãng phí công suất lớn cho các lớp nhỏ và làm giảm tổng chiều cao có thể đạt được. Một điều nữa là quên rằng mỗi viên gạch được sử dụng một lần, do đó khả năng sử dụng lại trên nhiều lớp là không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
3 3
5 5
```Các lớp bắt buộc là$1, 3, 5$. 

| Bước | Bắt buộc | Con trỏ tôi | Mũ[i] | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | chỉ định 1x1 | 1 | 
| 2 | 3 | 1 | 3 | phân công 3x3 | 2 | 
| 3 | 5 | 2 | 5 | phân công 5x5 | 3 | 

Điều này cho thấy sự liên kết hoàn hảo khi năng lực đã đáp ứng chính xác yêu cầu. 

### Ví dụ 2 

đầu vào:```
4
2 2
6 6
4 4
1 1
```Dung lượng: [2, 6, 4, 1] → sắp xếp: [1, 2, 4, 6] 

| Bước | Bắt buộc | Con trỏ tôi | Mũ[i] | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | sử dụng 1 | 1 | 
| 2 | 3 | 1 | 2 | bỏ qua 2 | - | 
| 2 | 3 | 2 | 4 | sử dụng 4 | 2 | 
| 3 | 5 | 3 | 6 | sử dụng 6 | 3 | 

Điều này thể hiện việc bỏ qua những viên gạch không đủ trong khi vẫn bảo quản những viên gạch lớn hơn cho các yêu cầu sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, quét tuyến tính sau đó | 
| Không gian |$O(n)$| khả năng lưu trữ | 

Thuật toán phù hợp thoải mái trong giới hạn cho$n = 10^5$, kể từ khi sắp xếp$10^5$số nguyên và một lần truyền tuyến tính duy nhất có hiệu quả trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# NOTE: placeholder; assumes solve() is imported or included

def test():
    import subprocess, textwrap, sys

    def exec_run(inp):
        from subprocess import Popen, PIPE
        p = Popen([sys.executable, "main.py"], stdin=PIPE, stdout=PIPE, stderr=PIPE, text=True)
        out, _ = p.communicate(inp)
        return out.strip()

    # minimal
    assert exec_run("1\n1 1\n") == "1"

    # sample-like increasing
    assert exec_run("3\n1 1\n3 3\n5 5\n") == "3"

    # all too small except first
    assert exec_run("3\n1 1\n1 1\n1 1\n") == "1"

    # mixed sizes
    assert exec_run("4\n2 2\n6 6\n4 4\n1 1\n") == "3"

    # large but insufficient chain
    assert exec_run("2\n10 10\n100 100\n") == "2"

test()
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 viên gạch | 1 | trường hợp tối thiểu | 
| tăng nghiêm ngặt | 3 | đầy tham lam tiến triển | 
| tất cả đều nhỏ | 1 | chấm dứt sớm | 
| kích cỡ hỗn hợp | 3 | bỏ qua logic | 
| dây chuyền lớn | 2 | kết hợp đơn điệu đúng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi nhiều viên gạch quá nhỏ so với yêu cầu ban đầu. Đối với đầu vào:```
5
1 1
1 1
10 10
10 10
10 10
```Yêu cầu là$1, 3, 5, \dots$. Thuật toán tiêu thụ đầu tiên$1 \times 1$, sau đó bỏ qua phần tiếp theo$1 \times 1$và sử dụng đúng các viên gạch lớn hơn cho các lớp sau. Vòng lặp bỏ qua đảm bảo chúng ta không gán nhầm một viên gạch nhỏ cho một yêu cầu lớn. 

Một trường hợp khác là khi những viên gạch lớn xuất hiện sớm. Vì:```
3
10 10
3 3
1 1
```Sắp xếp tạo ra [1, 3, 10]. Thuật toán sử dụng 1 cho cấp 1, 3 cho cấp 2, 10 cho cấp 3. Mặc dù một viên gạch lớn xuất hiện đầu tiên trong đầu vào, việc sắp xếp sẽ ngăn ngừa lãng phí sớm, duy trì tính khả thi cho các lớp cao hơn.
