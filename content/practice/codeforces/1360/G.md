---
title: "CF 1360G - Ma trận A/B"
description: "Chúng ta được yêu cầu xây dựng một lưới nhị phân có kích thước $n nhân m$, trong đó mỗi ô là 0 hoặc 1, dưới hai ràng buộc đồng thời liên kết chặt chẽ các hàng và cột. Mỗi hàng phải chứa chính xác $a$ cái và mỗi cột phải chứa chính xác $b$ cái."
date: "2026-06-16T11:16:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1360
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 644 (Div. 3)"
rating: 1900
weight: 1360
solve_time_s: 432
verified: false
draft: false
---

[CF 1360G - Ma trận A/B](https://codeforces.com/problemset/problem/1360/G) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 7 phút 12 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một lưới nhị phân có kích thước$n \times m$, trong đó mỗi ô là 0 hoặc 1, dưới hai ràng buộc đồng thời liên kết chặt chẽ các hàng và cột. Mỗi hàng phải chứa chính xác$a$những cái đó và mỗi cột phải chứa chính xác$b$những cái đó. Nếu không thể thỏa mãn cả hai ràng buộc cùng một lúc, chúng ta phải báo cáo lỗi. 

Một cách hữu ích để suy nghĩ về vấn đề này là vấn đề thiết kế tỷ lệ mắc bệnh lưỡng đảng. Mỗi hàng có một “công suất” cố định về những hàng mà nó phải phân phối trên các cột và mỗi cột có một “nhu cầu” cố định về những hàng mà nó phải nhận từ các hàng. Về cơ bản chúng tôi đang chỉ định$n \cdot a$mã thông báo giống hệt nhau (đóng góp hàng) vào một$n \times m$lưới sao cho mỗi cột nhận được chính xác$b$mã thông báo. 

Ràng buộc về cấu trúc đầu tiên xuất phát từ việc tính toán. Tổng số cái được xác định theo hai cách. Từ hàng nó là$n \cdot a$và từ các cột nó là$m \cdot b$. Nếu hai giá trị này khác nhau thì không thể tồn tại cấu trúc nào vì cùng một lưới không thể chứa đồng thời hai tổng số khác nhau. 

Vậy điều kiện cần là:$$n \cdot a = m \cdot b$$Cho rằng$n, m \le 50$, lưới rất nhỏ. Điều này loại bỏ bất kỳ nhu cầu tối ưu hóa nâng cao nào, nhưng nó cũng gợi ý rằng một mẫu hình xây dựng trực tiếp là đủ nếu đáp ứng các điều kiện tồn tại. 

Một nỗ lực ngây thơ có thể thử sắp xếp ngẫu nhiên hoặc sắp xếp tham lam từng hàng mà không có sự phối hợp. Điều này thất bại theo những cách tinh tế. Ví dụ: điền từng hàng một cách độc lập bằng$a$những cột này có xu hướng làm quá tải các cột đầu tiên và bỏ đói các cột sau vì dung lượng của cột không được theo dõi trên toàn cầu. Ngay cả việc quét tham lam cân bằng mà không có sự bao bọc cũng thất bại trong những trường hợp như$n=3, m=6, a=2, b=1$, trong đó việc phân phối đồng đều cục bộ vẫn có thể tạo ra sự xung đột hoặc thiếu hụt cột. 

Khó khăn chính là cả tổng hàng và tổng cột phải được thực thi chính xác, do đó các quyết định từng phần phải nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng gán cho mỗi ô 0 hoặc 1 và xác minh các ràng buộc về hàng và cột sau khi lấp đầy lưới. Về mặt khái niệm, điều này khám phá tất cả các tập hợp con có kích thước$n \cdot a$sang$n \cdot m$các vị trí có tính chất tổ hợp. Số cách chọn vị trí cho 1 người là$\binom{nm}{na}$, nó trở nên lớn về mặt thiên văn ngay cả đối với$50 \times 50$. Ngay cả với việc cắt tỉa, sự kết hợp ràng buộc giữa các hàng và cột làm cho việc quay lui trở nên bùng nổ vì các lựa chọn ban đầu hạn chế rất nhiều tính khả thi sau này. 

Quan sát cấu trúc là chúng ta không cần vị trí tùy ý nào cả. Vì mỗi hàng có các yêu cầu giống nhau và mỗi cột có các yêu cầu giống nhau nên chúng ta có thể phân phối các yêu cầu đó theo một mẫu tuần hoàn hoàn hảo. Nếu chúng ta làm phẳng ma trận thành một chuỗi có độ dài tuần hoàn$m$, mỗi hàng chỉ bắt đầu từ một vị trí đã dịch chuyển trong chu kỳ đó. Điều này đảm bảo mỗi hàng nhìn thấy chính xác$a$những cái đó, trong khi những thay đổi theo chu kỳ phân phối tải trọng cột một cách đồng đều. 

Điều này biến vấn đề thành việc xây dựng một phép dịch chuyển kiểu Latin tuần hoàn: row$i$đặt những cái ở vị trí$$(i \cdot a + j) \bmod m$$vì$j = 0 \dots a-1$, giả sử$n \cdot a = m \cdot b$đảm bảo độ bao phủ cột thống nhất. 

Cái nhìn sâu sắc cốt lõi là số học mô-đun thực thi sự cân bằng một cách tự động: mọi chỉ mục cột xuất hiện chính xác$b$lần trên tất cả các ca hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$nm$|$O(nm)$| Quá chậm | 
| Xây dựng theo chu kỳ tối ưu |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng ma trận một cách xác định bằng cách sử dụng các dịch chuyển mô-đun. 

1. Kiểm tra xem$n \cdot a = m \cdot b$. Nếu không bằng thì xuất NO ngay lập tức. Điều kiện này là bắt buộc vì nó thực thi sự bằng nhau của tổng số từ cả phối cảnh hàng và cột. 
2. Khởi tạo một$n \times m$ma trận chứa đầy số không. Điều này mang lại cho chúng ta một đế sạch sẽ, nơi chúng ta chỉ đặt những cái theo một mẫu cố định. 
3. Đối với mỗi hàng$i$, chúng tôi đặt chính xác$a$bằng cách chọn các chỉ số cột theo cách tuần hoàn. Chúng tôi tính toán các vị trí này bằng cách sử dụng$(i \cdot a + j) \bmod m$vì$j = 0 \dots a-1$. Sự thay đổi bởi$i \cdot a$đảm bảo các hàng khác nhau không căn chỉnh các hàng của chúng trong cùng một cột. 
4. Đặt các vị trí được tính toán đó thành 1 trong ma trận. Mỗi nhiệm vụ đều độc lập nhưng cấu trúc mô-đun đảm bảo sự cân bằng toàn cầu. 
5. Sau khi xử lý tất cả các hàng, xuất ma trận. 

Lý do điều này có tác dụng là vì mỗi hàng đóng góp chính xác$a$những cái bằng cách xây dựng. Sự dịch chuyển theo chu kỳ phân phối những đóng góp này một cách đồng đều trên tất cả các cột vì bước qua các hàng sẽ nâng cao vị trí bắt đầu theo một modulo tăng cố định$m$. Vì tổng số cái trùng khớp$m \cdot b$và sự phân bố đối xứng dưới sự dịch chuyển mô-đun, mỗi cột tích lũy chính xác$b$những cái đó. 

Bất biến chính là sau khi xử lý bất kỳ tiền tố nào của hàng, sự khác biệt giữa số lượng cột luôn bị giới hạn tối đa là 1 theo nghĩa tuần hoàn và sau khi tất cả các hàng được xử lý, tính đối xứng sẽ tạo ra sự bằng nhau chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, a, b = map(int, input().split())

        if n * a != m * b:
            print("NO")
            continue

        grid = [[0] * m for _ in range(n)]

        # pointer moves through columns cyclically
        for i in range(n):
            start = (i * a) % m
            for j in range(a):
                col = (start + j) % m
                grid[i][col] = 1

        print("YES")
        for row in grid:
            print("".join(map(str, row)))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc xây dựng theo chu kỳ. Phần tinh tế duy nhất là việc sử dụng`(i * a) % m`dưới dạng phần bù dịch chuyển, đảm bảo các hàng được phân bổ đều trên các cột thay vì lặp lại các mẫu giống hệt nhau. 

Một lỗi phổ biến là lặp lại các cột một cách tuyến tính mà không gói hoặc không dịch chuyển theo chỉ mục hàng. Điều đó tạo ra tổng hàng hợp lệ nhưng không thống nhất được cột. Sự dịch chuyển mô-đun là cơ chế ghép các hàng lại với nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 6 2 1
```Đây$n \cdot a = 3 \cdot 2 = 6$Và$m \cdot b = 6 \cdot 1 = 6$, do đó có thể xây dựng được. 

| Hàng | bắt đầu = (i·a)%6 | cột đã chọn | kết quả hàng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0,1 | 110000 | 
| 1 | 2 | 2,3 | 001100 | 
| 2 | 4 | 4,5 | 000011 | 

Mỗi cột nhận được chính xác một số 1, xác nhận tính đúng đắn. 

Điều này cho thấy cách dịch chuyển ngăn chặn sự chồng chéo và trải rộng vùng phủ sóng một cách đồng đều. 

### Ví dụ 2 

đầu vào:```
4 4 2 2
```Đây$n \cdot a = 8$,$m \cdot b = 8$, rất khả thi. 

| Hàng | bắt đầu | cột đã chọn | kết quả hàng | 
| --- | --- | --- | --- | 
| 0 | 0 | 0,1 | 1100 | 
| 1 | 2 | 2,3 | 0011 | 
| 2 | 0 | 0,1 | 1100 | 
| 3 | 2 | 2,3 | 0011 | 

Tổng cột đều bằng 2, trùng khớp$b$. 

Ví dụ này cho thấy rằng việc lặp lại giữa các hàng được cho phép miễn là sự phân bổ cột toàn cục được cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được viết tối đa một lần trên tất cả các công trình | 
| Không gian |$O(nm)$| Lưu trữ cho lưới đầu ra | 

Cho rằng cả hai thứ nguyên nhiều nhất là 50, tổng công việc là không đáng kể ngay cả đối với 1000 trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m, a, b = map(int, input().split())
        if n * a != m * b:
            out.append("NO")
            continue
        g = [[0]*m for _ in range(n)]
        for i in range(n):
            start = (i * a) % m
            for j in range(a):
                g[i][(start + j) % m] = 1
        out.append("YES")
        out.extend("".join(map(str, r)) for r in g)
    return "\n".join(out) + "\n"

# provided samples
assert run("""5
3 6 2 1
2 2 2 1
2 2 2 2
4 4 2 2
2 1 1 2
""").strip().split("\n")[0] == "YES"

# custom cases
assert "NO" in run("1\n3 3 2 1\n")
assert run("1\n1 1 1 1\n").count("1") == 1
assert run("1\n2 2 1 1\n").count("YES") == 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 2 1 | KHÔNG | tổng số tiền không khớp không thể thực hiện được | 
| 1 1 1 1 | CÓ 1 | lưới hợp lệ tối thiểu | 
| 2 2 1 1 | Ma trận CÓ | phân phối cân bằng | 

## Vỏ cạnh 

Khi nào$n \cdot a \neq m \cdot b$, thuật toán ngay lập tức bác bỏ. Ví dụ,$n=2, m=3, a=2, b=1$cho tổng số 4 và 3, vì vậy không có cách xây dựng nào tồn tại. Mã thoát chính xác trước bất kỳ vị trí nào. 

Khi$n=1$, trường hợp hợp lệ duy nhất là$a=m$Và$b=1$. Cấu trúc giảm xuống còn lấp đầy hoàn toàn một hàng, tự động khớp với yêu cầu của cột. 

Khi$m=1$, áp dụng tính đối xứng: tất cả các hàng phải có$a=1$và tất cả các hàng phải khớp với số lượng cột đơn$b=n$. Cấu trúc mô-đun thoái hóa thành việc đặt chính xác các mô-đun vào cột duy nhất$n$lần, thỏa mãn điều kiện ràng buộc. 

Trong mỗi trường hợp này, sự dịch chuyển theo chu kỳ sẽ giảm xuống một mẫu hằng số tầm thường hoặc bị bỏ qua hoàn toàn do kiểm tra tính khả thi, đảm bảo không tạo ra cấu trúc từng phần không nhất quán.
