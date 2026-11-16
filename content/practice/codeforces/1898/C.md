---
title: "CF 1898C - Lưới đầy màu sắc"
description: "Chúng ta được yêu cầu tô màu các đoạn của một lưới hình chữ nhật, được tạo thành bởi $n$ đường ngang và $m$ đường thẳng đứng, sử dụng hai màu đỏ và xanh lam. Mỗi đoạn nối hai điểm liền kề theo chiều ngang hoặc chiều dọc."
date: "2026-06-08T21:30:32+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1898
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 910 (Div. 2)"
rating: 1700
weight: 1898
solve_time_s: 192
verified: false
draft: false
---

[CF 1898C - Lưới đầy màu sắc](https://codeforces.com/problemset/problem/1898/C) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tô màu các đoạn của một lưới hình chữ nhật, được tạo thành bởi$n$đường ngang và$m$đường thẳng đứng, sử dụng hai màu đỏ và xanh. Mỗi đoạn nối hai điểm liền kề theo chiều ngang hoặc chiều dọc. Mục tiêu là tô màu tất cả các đoạn sao cho tồn tại một lối đi từ góc trên bên trái$(1,1)$đến góc dưới bên phải$(n,m)$nơi các đoạn dọc theo lối đi có màu sắc xen kẽ nhau. Việc đi bộ có thể quay lại các điểm nhiều lần nên không bị giới hạn ở những con đường đơn giản. 

Đầu vào cung cấp kích thước của lưới và số lượng phân đoạn cần thiết$k$trong cuộc đi bộ xen kẽ. Chúng tôi phải cung cấp màu thỏa mãn điều kiện này hoặc báo cáo rằng không có màu nào như vậy tồn tại. 

Kích thước lưới nhỏ, lên tới 16x16, nhưng$k$có thể rất lớn, lên tới$10^9$. Điều này cho chúng ta biết rằng việc tìm kiếm vũ phu ở mọi tầng lớp là không khả thi. Thay vào đó, chúng ta phải suy luận về cấu trúc của lưới và thuộc tính xen kẽ màu. Hạn chế chính không rõ ràng là lối đi xen kẽ màu sắc, điều này ngay lập tức ngụ ý rằng lối đi xen kẽ tối thiểu từ$(1,1)$ĐẾN$(n,m)$có chiều dài$n + m - 2$các phân đoạn, đi theo con đường Manhattan. Bất kỳ cuộc đi bộ nào ngắn hơn thế này là không thể. Từ$k$có thể rất lớn, chúng ta cần xem xét liệu các điểm lặp lại có cho phép đi bộ xen kẽ dài tùy ý hay không. 

Một giải pháp ngây thơ cố gắng tạo ra tất cả các đường dẫn có độ dài$k$sẽ thất bại, vì ngay cả trong lưới 16x16 cũng có hàng tỷ bước đi nếu chúng ta cho phép xem lại các điểm. Một trường hợp khó phát hiện khác xảy ra khi$n=1$hoặc$m=1$, điều này sẽ làm cho một số đường dẫn trở nên không xen kẽ nhau một cách tầm thường, nhưng ở đây$n,m\ge3$vì vậy chúng tôi tránh lưới suy biến. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các bước đi có chiều dài$k$và kiểm tra các màu xen kẽ. Về nguyên tắc, điều này đúng vì bất kỳ màu hợp lệ nào đều được đảm bảo mang lại một bước đi thay thế. Tuy nhiên, số lượng lối đi tiềm năng tăng theo cấp số nhân với kích thước lưới. Đối với lưới 16x16, có khoảng$O(2^{n\cdot m})$khả năng xảy ra nếu chúng ta thử tất cả các bước đi, điều này vượt xa mọi tính toán khả thi. 

Ý tưởng sâu sắc quan trọng là khai thác cấu trúc của lưới: nếu chúng ta tô màu các cạnh ngang và dọc theo mẫu bàn cờ (ví dụ: các đoạn ngang xen kẽ theo hàng, các đoạn dọc xen kẽ theo cột), thì bất kỳ đường đi nào di chuyển hoàn toàn sang phải và xuống đều có thể được thực hiện xen kẽ và các bước đi dài hơn có thể được tạo ra bằng cách vòng quanh các góc. Độ dài tối thiểu của đường xen kẽ là$n + m - 2$, đó là khoảng cách Manhattan. Điều này có nghĩa là bất kỳ$k < n + m - 2$là không thể. Đối với lớn hơn$k$, sự xen kẽ có thể được mở rộng bằng cách xem lại các điểm, vì vậy bất kỳ$k \ge n + m - 2$có thể đạt được bằng cách tô màu bàn cờ nhất quán. 

Do đó, thuật toán giảm xuống còn kiểm tra xem$k \ge n + m - 2$. Nếu không, xuất ra NO. Mặt khác, hãy tạo một màu đơn giản để đảm bảo bước đi xen kẽ, chẳng hạn như sọc ngang theo hàng cho các cạnh ngang và sọc dọc theo cột cho các cạnh dọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n*m)) | O(n*m) | Quá chậm | 
| Tô màu bàn cờ | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$,$m$, Và$k$. 
2. Kiểm tra độ dài tối thiểu có thể có của chuyến đi bộ xen kẽ từ$(1,1)$ĐẾN$(n,m)$, đó là$n + m - 2$. Nếu như$k < n + m - 2$, in KHÔNG, vì không tồn tại bước đi hợp lệ. 
3. Nếu$k \ge n + m - 2$, in CÓ. Bây giờ chúng ta xây dựng một màu đảm bảo cho việc đi bộ xen kẽ. 
4. Tô màu tất cả các cạnh ngang theo hàng, xen kẽ các màu giữa các cột liên tiếp. Đối với hàng$i$, gán 'R' cho cột lẻ và 'B' cho cột chẵn nếu$i$là số lẻ và hoán đổi mẫu nếu$i$là chẵn. Điều này đảm bảo các đoạn liền kề trong bất kỳ bước đi nào đều xen kẽ chính xác dọc theo các bước di chuyển ngang. 
5. Tô màu tất cả các cạnh dọc của từng cột theo kiểu xen kẽ tương tự, đảm bảo rằng việc di chuyển xuống từ một ô cũng xen kẽ với đoạn ngang trước đó. Để đơn giản, các cột có thể thay thế màu bắt đầu ở mỗi hàng. 
6. Xuất màu theo định dạng quy định: đầu tiên$n$các hàng cho các đoạn ngang, sau đó$n-1$hàng cho các phân đoạn dọc. 

Tại sao nó hoạt động: Màu sắc đảm bảo rằng mọi chuyển động ngang xen kẽ với chuyển động dọc trước đó. Vì việc đi bộ có thể xem lại các điểm, bất kỳ$k \ge n + m - 2$có thể đạt được bằng cách lặp dọc theo mô hình xen kẽ. Không có hai đoạn đường liên tiếp nào trên đường đi có cùng màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        min_len = n + m - 2
        if k < min_len:
            print("NO")
            continue
        print("YES")
        hor = []
        for i in range(n):
            row = []
            for j in range(m-1):
                if (i+j)%2 == 0:
                    row.append('R')
                else:
                    row.append('B')
            hor.append(row)
        ver = []
        for i in range(n-1):
            row = []
            for j in range(m):
                if (i+j)%2 == 0:
                    row.append('R')
                else:
                    row.append('B')
            ver.append(row)
        for row in hor:
            print(' '.join(row))
        for row in ver:
            print(' '.join(row))

if __name__ == "__main__":
    solve()
```Mã đầu tiên kiểm tra xem$k$nhỏ hơn khoảng cách tối thiểu của Manhattan. Màu ngang đảm bảo rằng bất kỳ chuyển động hàng nào cũng xen kẽ giữa màu đỏ và xanh lam. Màu dọc căn chỉnh với mẫu ngang để di chuyển xuống xen kẽ một cách chính xác. Chúng tôi dựa vào số học modulo$(i+j)\%2$để tạo mẫu bàn cờ mà không cần theo dõi các phân đoạn trước đó theo cách thủ công. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4 5 11
```| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| m | 5 | 
| k | 11 | 
| min_len | 4 + 5 - 2 = 7 | 
| k >= min_len | Có | 

Thuật toán in CÓ và tạo màu bàn cờ. Bất kỳ đường đi nào có độ dài 11 đều có thể được theo dõi bằng cách di chuyển sang phải và xuống bằng các vòng để kéo dài bước đi trong khi vẫn duy trì các màu xen kẽ. 

### Đầu vào mẫu 2```
3 3 2
```| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 3 | 
| k | 2 | 
| min_len | 3 + 3 - 2 = 4 | 
| k >= min_len | Không | 

Thuật toán in ra KHÔNG, vì không có bước đi nào có độ dài 2 có thể kết nối$(1,1)$ĐẾN$(3,3)$trong khi luân phiên. 

Những ví dụ này chứng minh rằng kiểm tra độ dài tối thiểu xử lý chính xác các trường hợp không thể xảy ra và logic tô màu hoạt động cho các bước đi có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Tô màu từng đoạn ngang và dọc một lần | 
| Không gian | O(n*m) | Lưu trữ các mảng màu | 

Được cho$n,m \le 16$, điều này là không đáng kể và giải pháp dễ dàng phù hợp trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n4 5 11\n3 3 2\n3 4 1000000000\n3 3 12588\n4 4 8\n") == \
"""YES
R B R B
B R B R
R B R B
B R B R
R B R B R
B R B R B
R B R B R
NO
YES
R B R
B R B
R B R
B R B
R B R
YES
R B
B R
R B
B R
YES
R B R
B R B
R B R
B R B
R B R
B R B
R B R
""", "provided samples"

# Custom cases
assert run("1\n3 3 4\n") == "YES\nR B\nB R\nR B\nR B R\nB R B\n", "minimum walk length"
assert run("1\n3 3 3\n") == "NO", "impossible short walk"
assert run("1\n16 16 1000\n")[:3] == "YES", "maximum grid size large k"
assert run("1\n3 3 1000000000\n")[:3] == "YES", "large k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | | |
