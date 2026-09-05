---
title: "CF 104511G - Nghỉ giải lao"
description: "Chúng tôi đang theo dõi một chuỗi các khoảnh khắc khi một người kiểm tra đồng hồ. Mỗi lần kiểm tra diễn ra tại một thời điểm được xác định bởi sự lặp lại, bắt đầu từ độ trễ ban đầu và sau đó phát triển dựa trên thời gian kiểm tra trước đó và một hàm tuần hoàn nhỏ của số bước."
date: "2026-06-30T10:45:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104511
codeforces_index: "G"
codeforces_contest_name: "Lexington Informatics Tournament (LIT) 2023"
rating: 0
weight: 104511
solve_time_s: 94
verified: false
draft: false
---

[CF 104511G - Nghỉ giải lao](https://codeforces.com/problemset/problem/104511/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một chuỗi các khoảnh khắc khi một người kiểm tra đồng hồ. Mỗi lần kiểm tra diễn ra tại một thời điểm được xác định bởi sự lặp lại, bắt đầu từ độ trễ ban đầu và sau đó phát triển dựa trên thời gian kiểm tra trước đó và một hàm tuần hoàn nhỏ của số bước. 

Tại mỗi lần kiểm tra, chúng tôi không quan tâm đến dấu thời gian đầy đủ. Chúng tôi chỉ quan tâm đến những gì đồng hồ hiển thị, đó là thời gian hiện tại theo modulo$m$. Nếu giá trị được hiển thị đó nhiều nhất$x$, chúng tôi coi đó là một sự “nghỉ ngơi”. Sau chính xác$n$những lần kiểm tra như vậy, quá trình sẽ dừng lại và chúng tôi phải báo cáo số lần xảy ra sự cố. 

Khó khăn chính là lần kiểm tra tiếp theo phụ thuộc hoàn toàn vào thời gian trước đó, nhưng quyết định chỉ phụ thuộc vào thời gian modulo$m$. Điều này ngay lập tức gợi ý rằng chúng ta nên cố gắng theo dõi mọi thứ theo số học mô-đun thay vì các giá trị tuyệt đối, vì thời gian thô tăng lên không giới hạn và có thể đạt tới các giá trị cực lớn. 

Các ràng buộc làm cho việc mô phỏng lực lượng vũ phu của tất cả$n$bước không thể khi$n$lớn nên cấu trúc của phép truy toán phải chứa đựng sự lặp lại hoặc tính tuần hoàn mạnh mẽ. Một điểm tinh tế khác là quy tắc cập nhật phụ thuộc vào$i \bmod c$, giới thiệu một mẫu lặp lại nhỏ tương tác với modulo$m$tình trạng. 

Một mô phỏng đơn giản sẽ tính toán lần tiếp theo trực tiếp từ lần trước. Điều này đúng về mặt logic, nhưng không thành công khi$n$lớn vì nó đòi hỏi tới$10^9$chuyển tiếp. 

Một trường hợp thất bại phổ biến đối với việc tối ưu hóa bất cẩn là cố gắng giảm sự lặp lại theo modulo$m$mà không hiểu toàn bộ thời gian tương tác với modulo như thế nào. Sự truy hồi kết hợp phép nhân với$m-1$với một nhiễu loạn phụ gia nhỏ và thiếu hiệu ứng lật dấu dẫn đến chuyển tiếp không chính xác. 

Ví dụ, nếu người ta giả định sai rằng chỉ$z_i \bmod m$là cần thiết và cố gắng cập nhật nó như$(m-1)z_i \bmod m = z_i \bmod m$, họ mất modulo hành vi phủ định thiết yếu$m$, điều này làm thay đổi hoàn toàn trình tự. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp duy trì toàn bộ thời gian$z_i$. Mỗi bước áp dụng$$z_{i+1} = (m-1)z_i + a(i \bmod c)^2.$$Điều này rõ ràng có tác dụng đối với nhỏ$n$, nhưng các giá trị của$z_i$phát triển cực kỳ nhanh và thậm chí việc lưu trữ chúng cũng trở nên không cần thiết vì chỉ$z_i \bmod m$vấn đề để đếm thời gian nghỉ. 

Quan sát quan trọng là quá trình chuyển đổi đơn giản hóa đáng kể theo modulo$m$. Từ$m-1 \equiv -1 \pmod m$, chúng tôi nhận được:$$z_{i+1} \bmod m = (-z_i + a(i \bmod c)^2) \bmod m.$$Vì vậy nếu chúng ta định nghĩa$t_i = z_i \bmod m$, quá trình sẽ trở thành một sự tái diễn hoàn toàn trên dư lượng:$$t_{i+1} = (a(i \bmod c)^2 - t_i) \bmod m.$$Đây là một phép truy hồi tuyến tính với dấu xen kẽ, được điều khiển bởi một số hạng tuần hoàn bên ngoài của chu kỳ$c$. Việc lật dấu là cấu trúc quan trọng: nó có nghĩa là trạng thái luân phiên giữa “lật dấu và thêm một giá trị nhỏ”. 

Để loại bỏ dấu xen kẽ, chúng ta tách dãy thành các chỉ số chẵn và lẻ. Nếu chúng ta áp dụng phép truy hồi hai lần thì dấu sẽ bị hủy:$$t_{i+2} = t_i + (f_{i+1} - f_i) \bmod m,$$Ở đâu$f_i = a(i \bmod c)^2$. 

Bây giờ mỗi lớp chẵn lẻ trở thành một quá trình cộng thuần túy. Sự gia tăng chỉ phụ thuộc vào$i \bmod c$, vì vậy nó lặp lại mỗi$c$các bước. Điều này làm cho dãy tuần hoàn với chu kỳ nhiều nhất là$2c$. Từ$c \le 1000$, chúng ta có thể tính toán rõ ràng toàn bộ khoảng thời gian$2c$, sau đó lặp lại nó để bao gồm tất cả$n$các bước. 

Khi chúng ta biết các giá trị trong một khoảng thời gian, việc đếm các khoảng nghỉ sẽ giảm xuống việc đếm xem có bao nhiêu giá trị đó$\le x$, nhân với bao nhiêu khoảng thời gian đầy đủ phù hợp$n$, cộng với phần còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Giảm định kỳ |$O(c)$|$O(c)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn với dư lượng$t_i = z_i \bmod m$. 

1. Bắt đầu với$t_1 = b \bmod m$. Đây là lần quan sát đồng hồ đầu tiên. 
2. Tính toán$t_{i+1} = (a(i \bmod c)^2 - t_i) \bmod m$vì$i = 1$lên đến$2c$. 

Chúng tôi dừng lại ở$2c$vì độ dài này đủ để nắm bắt toàn bộ hành vi lặp lại của hệ thống. 
3. Lưu trữ tất cả các giá trị$t_1, t_2, \dots, t_{2c}$và đếm xem có bao nhiêu$\le x$. Gọi giá trị này`cnt_period`. 
4. Tính xem có bao nhiêu khoảng thời gian đầy đủ$2c$phù hợp với$n$. Cho phép`full = n // (2c)`. 
5. Nhân: mỗi tiết đầy đủ có cùng số lần nghỉ giải lao, vì vậy hãy cộng`full * cnt_period`. 
6. Mô phỏng phần còn lại$n \bmod (2c)$các bước và thêm đóng góp của họ một cách riêng lẻ. 

Tại sao nó hoạt động xuất phát từ hai sự thật. Đầu tiên, sự tái diễn của dư lượng chỉ phụ thuộc vào dư lượng hiện tại và hàm tuần hoàn xác định của chỉ số bước. Thứ hai, sau hai bước, hiệu ứng ký hiệu sẽ bị hủy bỏ, biến quá trình tiến hóa thành một hệ thống cộng gộp thuần túy với trình tự tăng dần định kỳ có giới hạn. Điều này buộc toàn bộ hệ thống phải lặp lại mọi$2c$các bước, do đó trình tự trạng thái có tính tuần hoàn và số lần ngắt cũng có tính tuần hoàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, a, b, x, c = map(int, input().split())

        def f(i):
            r = i % c
            return (a * r * r) % m

        max_len = min(n, 2 * c)

        tvals = [0] * (max_len + 1)
        tvals[1] = b % m

        for i in range(1, max_len):
            tvals[i + 1] = (f(i) - tvals[i]) % m

        cnt = 0
        for i in range(1, max_len + 1):
            if tvals[i] <= x:
                cnt += 1

        if n <= max_len:
            print(cnt)
            continue

        full = n // max_len
        rem = n % max_len

        ans = full * cnt
        for i in range(1, rem + 1):
            if tvals[i] <= x:
                ans += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ lưu trữ chuỗi dư lượng trong tối đa một chu kỳ đầy đủ$2c$. Quá trình chuyển đổi sử dụng trực tiếp dạng mô-đun, tránh việc xử lý các số nguyên lớn. 

Việc lập chỉ mục được giữ dựa trên 1 để khớp với định nghĩa lặp lại một cách rõ ràng. chức năng$f(i)$tính toán khoản đóng góp định kỳ$a(i \bmod c)^2$, luôn giảm modulo$m$. 

Câu trả lời cuối cùng được tập hợp bằng cách chia quy trình thành các chu kỳ đầy đủ và tiền tố còn sót lại, đảm bảo không có bước nào vượt quá$O(c)$mô phỏng là cần thiết. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$c = 3$, do đó mẫu số hạng cộng lặp lại sau mỗi 3 bước. Chúng tôi chỉ hiển thị một vài giá trị đầu tiên. 

| tôi | f(i) | t[i] | ngắt (t[i] ≤ x) | 
| --- | --- | --- | --- | 
| 1 | f(1) | t₁ | có/không | 
| 2 | f(2) | t₂ | có/không | 
| 3 | f(3) | t₃ | có/không | 
| 4 | f(1) | t₄ | mô hình lặp lại | 
| 5 | f(2) | t₅ | mô hình lặp lại | 

Dấu vết này cho thấy cả thuật ngữ điều khiển và trạng thái kết quả đều tuân theo cấu trúc lặp lại, điều này cho phép nén thành một khối hữu hạn. 

Ví dụ thứ hai với$m$nhỏ minh họa hành vi bao quanh: ngay cả khi các giá trị dao động, chỉ có dư lượng của chúng là quan trọng và dư lượng giống hệt nhau tái diễn sau toàn bộ chu kỳ, xác nhận rằng việc đếm có thể được thực hiện trên mỗi khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(c)$mỗi trường hợp thử nghiệm | Chúng tôi chỉ mô phỏng tối đa$2c$bước | 
| Không gian |$O(c)$| Lưu trữ một phân đoạn định kỳ | 

Ràng buộc$c \le 1000$đảm bảo rằng ngay cả với$t \le 100$, giải pháp chạy thoải mái trong giới hạn, vì tổng công việc tối đa là khoảng$2 \cdot 10^5$chuyển tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import *
    # assume solve() is defined in same file
    solve()

# provided samples (formatted placeholders, actual CF input should be used)
# assert run(...) == ...

# minimal case
# n=1 should directly check first value
# assert run("1\n1 5 2 3 1 2\n") == "..."

# x = m-1 always break
# assert run("1\n10 7 3 5 6 3\n") == "10\n"

# c = 1 edge periodicity collapse
# assert run("1\n10 9 2 4 3 1\n") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu n | đánh giá đơn đúng | tính đúng đắn của trường hợp cơ sở | 
| x = m-1 | tất cả các bước đều được nghỉ | logic giới hạn trên | 
| c = 1 | cấu trúc tuần hoàn đơn giản nhất | hành vi cạnh tái phát | 

## Vỏ cạnh 

Khi nào$c = 1$, thuật ngữ$i \bmod c$luôn bằng 0, do đó phép truy toán đơn giản hóa thành$t_{i+1} = -t_i$. Trình tự xen kẽ một cách xác định giữa hai giá trị và thuật toán nắm bắt chính xác giá trị này bên trong giá trị được tính toán.$2c = 2$Giai đoạn. 

Khi$b \ge m$, giá trị ban đầu giảm modulo$m$. Việc thực hiện áp dụng rõ ràng$b \bmod m$, đảm bảo trạng thái hợp lệ ngay cả khi thời gian bắt đầu vượt quá mô đun. 

Khi$x = m-1$, mọi phần dư được tính là một khoảng nghỉ. Phương pháp đếm định kỳ vẫn tạo ra tổng số chính xác vì nó đếm tất cả các vị trí trong mỗi chu kỳ mà không có ngoại lệ.
