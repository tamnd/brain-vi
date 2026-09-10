---
title: "CF 104598E - Vịt AI"
description: "Chúng tôi đang làm việc trên một lưới từ $(1,1)$ đến $(N,M)$ và chuyển động bị hạn chế chỉ ở các bước di chuyển sang phải hoặc lên. Điều này có nghĩa là mọi đường đi hợp lệ đều đơn điệu: cả hai tọa độ đều tăng dọc theo đường đi. Trên hết, chúng ta được cấp $K$ các ô đặc biệt mà tất cả các ô này phải được đường dẫn truy cập."
date: "2026-06-30T04:32:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104598
codeforces_index: "E"
codeforces_contest_name: "GPL 2023 Advanced"
rating: 0
weight: 104598
solve_time_s: 142
verified: false
draft: false
---

[CF 104598E - Vịt AI](https://codeforces.com/problemset/problem/104598/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một mạng lưới từ$(1,1)$ĐẾN$(N,M)$và chuyển động bị hạn chế chỉ ở các bước di chuyển sang phải hoặc lên. Điều này có nghĩa là mọi đường đi hợp lệ đều đơn điệu: cả hai tọa độ đều tăng dọc theo đường đi. 

Trên hết, chúng tôi được cấp$K$các ô đặc biệt mà tất cả đều phải được truy cập bằng đường dẫn. Một đường dẫn chỉ hợp lệ nếu nó đi qua mọi ô bắt buộc ngoài việc bắt đầu và kết thúc ở các góc. 

Khó khăn chính là điểm yêu cầu không được sắp xếp theo thứ tự. Một số trong chúng có thể gây ra mâu thuẫn, bởi vì một đường đi đơn điệu không thể đi qua một điểm “trước” một điểm bắt buộc khác trong cả hai tọa độ. Ví dụ: nếu một điểm bắt buộc là$(2,5)$và cái khác là$(3,4)$, không có đường đi đơn điệu nào có thể đi qua cả hai chiều vì không cái nào lấn át cái kia ở cả hai chiều theo một thứ tự nhất quán. 

Các ràng buộc rất lớn: lên tới$10^5$trường hợp thử nghiệm, với tổng số$K$trong tất cả các bài kiểm tra cũng$10^5$và kích thước lưới lên tới$10^5$. Điều này loại trừ mọi thử nghiệm$O(K^2)$hoặc DP qua lưới. Giải pháp phải xử lý từng bài kiểm tra một cách đại khái$O(K \log K)$hoặc tốt hơn. 

Trường hợp cạnh khóa xuất hiện khi các điểm được yêu cầu không nhất quán với thứ tự đơn điệu. Ví dụ:```
1
3 3 2
2 3
3 2
```Không có đường dẫn hợp lệ tồn tại bởi vì$(2,3)$Và$(3,2)$không thể truy cập theo một trình tự đơn điệu. Bất kỳ phép tính ngây thơ nào nhân các đoạn đường dẫn độc lập mà không kiểm tra tính khả thi sẽ tạo ra câu trả lời tích cực không chính xác. 

Một trường hợp thất bại khác phát sinh khi có điểm trùng lặp. Mặc dù câu lệnh cho phép các tọa độ lặp lại, nhưng việc xử lý chúng một cách riêng biệt có thể đếm gấp đôi các đường dẫn trừ khi chúng được loại bỏ trùng lặp. 

## Phương pháp tiếp cận 

Không có ràng buộc, ý tưởng tự nhiên là thử tất cả các hoán vị của việc truy cập vào$K$điểm cần thiết theo một thứ tự nào đó. Đối với mỗi thứ tự, chúng tôi kiểm tra xem nó có đơn điệu hay không (sắp xếp theo cả hai tọa độ) rồi nhân hệ số nhị thức cho từng đoạn giữa các điểm liên tiếp. Điều này đúng vì giữa hai điểm cố định$(x_1,y_1)$Và$(x_2,y_2)$, số đường đi đơn điệu là$\binom{(x_2-x_1)+(y_2-y_1)}{x_2-x_1}$. 

Vấn đề là có$K!$những hoán vị không thể thực hiện được$K$lên đến$10^5$. Ngay cả việc hạn chế các đơn đặt hàng hợp lệ cũng không giúp ích gì trong trường hợp xấu nhất khi điểm được sắp xếp một phần. 

Điều quan trọng là mọi đường đi hợp lệ đều phải truy cập các điểm được yêu cầu theo thứ tự tăng dần của cả hai tọa độ cùng một lúc. Điều này có nghĩa là chúng ta có thể sắp xếp tất cả các điểm cần thiết theo$x$, và nếu hai điểm giống nhau$x$, qua$y$. Sau khi phân loại, chúng ta chỉ cần kiểm tra tính khả thi:$y$-tọa độ cũng phải không giảm. Nếu điều kiện này không thành công thì câu trả lời là 0. 

Sau khi được sắp xếp, bài toán sẽ trở thành một tích đơn giản của các bài toán con độc lập: đếm đường đi giữa các điểm liên tiếp trong chuỗi đã sắp xếp, nhân với nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(K! \cdot K)$|$O(K)$| Quá chậm | 
| Sắp xếp + sản ​​phẩm DP |$O(K \log K)$|$O(K)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn bài toán thành một chuỗi các đoạn đơn điệu. 

### bước 

1. Thu thập tất cả các điểm cần thiết và thêm điểm bắt đầu$(1,1)$và điểm cuối$(N,M)$. 

Những điều này xác định chuỗi bắt buộc đầy đủ của đường dẫn. 
2. Sắp xếp tất cả các điểm theo thứ tự tăng dần$x$, và nếu$x$bằng nhau, bằng cách tăng$y$. 

Điều này phản ánh thực tế là mọi đường đi lên/phải đều phải đi qua các điểm theo thứ tự này nếu có thể. 
3. Kiểm tra tính khả thi bằng cách quét danh sách đã sắp xếp và xác minh rằng$y$- tọa độ không giảm. 

Nếu xảy ra bất kỳ sự giảm nào, trả về 0 vì không có đường dẫn đơn điệu nào có thể đáp ứng cả hai yêu cầu. 
4. Với mọi cặp điểm liên tiếp$(x_i,y_i)$Và$(x_{i+1},y_{i+1})$, tính số đường đi đơn điệu giữa chúng:$$\binom{(x_{i+1}-x_i)+(y_{i+1}-y_i)}{x_{i+1}-x_i}$$5. Nhân tất cả số đoạn theo modulo$998244353$. 

Mỗi đoạn là độc lập vì khi đường đi đạt đến điểm bắt buộc, các lựa chọn còn lại chỉ phụ thuộc vào đoạn tiếp theo. 
6. Xuất sản phẩm cuối cùng. 

### Tại sao nó hoạt động 

Đường đi đơn điệu áp đặt một trật tự tổng thể phù hợp với cả hai tọa độ. Việc sắp xếp thực thi thứ tự ứng cử viên duy nhất có thể có khi truy cập các điểm bắt buộc. Kiểm tra tính khả thi đảm bảo trật tự này nhất quán ở cả hai khía cạnh. Sau khi được sửa, mọi đoạn trở thành một bài toán đường mạng độc lập và tính độc lập xuất phát từ thực tế là các lựa chọn trong các khoảng tọa độ rời rạc không tương tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def comb(n, k):
    if k < 0 or k > n:
        return 0
    k = min(k, n - k)
    res = 1
    for i in range(1, k + 1):
        res = res * (n - k + i) // i
    return res % MOD

def solve():
    T = int(input())
    for _ in range(T):
        N, M, K = map(int, input().split())
        pts = [(1, 1)]
        for _ in range(K):
            x, y = map(int, input().split())
            pts.append((x, y))
        pts.append((N, M))

        pts.sort()

        ok = True
        for i in range(len(pts) - 1):
            if pts[i][1] > pts[i + 1][1]:
                ok = False
                break

        if not ok:
            print(0)
            continue

        ans = 1
        for i in range(len(pts) - 1):
            x1, y1 = pts[i]
            x2, y2 = pts[i + 1]
            dx = x2 - x1
            dy = y2 - y1
            if dx < 0 or dy < 0:
                ok = False
                break
            ways = comb(dx + dy, dx)
            ans = ans * ways % MOD

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sẽ chèn các điểm cuối để tất cả các ràng buộc trở thành chuyển tiếp phân đoạn. Việc sắp xếp đảm bảo chúng tôi thực thi thứ tự truy cập duy nhất có thể phù hợp với chuyển động đơn điệu. Việc kiểm tra tính khả thi là cần thiết vì chỉ phân loại thôi không đảm bảo rằng đường dẫn tồn tại; cái$y$- Trình tự cũng phải đơn điệu. 

Hàm kết hợp tính toán trực tiếp số lượng đường đi trong mạng bằng cách sử dụng các hệ số nhị thức, biểu thị việc chọn vị trí của các nước đi bên phải trong tổng số nước đi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 3 1
2 2
```| Bước | Điểm | Khả thi | Phân khúc sản phẩm | 
| --- | --- | --- | --- | 
| Thêm điểm cuối | (1,1),(2,2),(3,3) | | | 
| Sắp xếp | không thay đổi | | | 
| Kiểm tra đơn hàng y | 1 2 3 | vâng | | 
| Phân đoạn | (1,1)->(2,2)->(3,3) | | | 
| Tính toán | C(2,1)=2, C(2,1)=2 | | 4 | 

Đáp án là 4, tương ứng với các lựa chọn độc lập trên mỗi đoạn. 

Điều này xác nhận rằng việc phân đoạn làm giảm việc tính toàn cục vào tổ hợp cục bộ. 

### Ví dụ 2 

đầu vào:```
1
3 3 2
2 3
3 2
```| Bước | Điểm | Khả thi | Phân khúc sản phẩm | 
| --- | --- | --- | --- | 
| Thêm điểm cuối | (1,1),(2,3),(3,2),(3,3) | | | 
| Sắp xếp | (1,1),(2,3),(3,2),(3,3) | | | 
| Kiểm tra đơn hàng y | 1 ≤ 3 2 | không | 0 | 

Vi phạm xuất hiện khi$y$giảm sau khi sắp xếp theo$x$, chứng tỏ không có đường đi đơn điệu nào có thể đi qua cả hai điểm cần tìm. 

Điều này chứng tỏ tại sao việc kiểm tra tính khả thi là cần thiết trước khi tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((N+K)\log(N+K))$| phân loại chiếm ưu thế trong mỗi bài kiểm tra | 
| Không gian |$O(K)$| lưu trữ điểm cần thiết | 

Vì tổng cộng$K$trên tất cả các trường hợp thử nghiệm là$10^5$, việc sắp xếp vẫn hiệu quả và mỗi bài kiểm tra được xử lý độc lập mà không cần tính toán lại. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def comb(n, k):
        if k < 0 or k > n:
            return 0
        k = min(k, n - k)
        res = 1
        for i in range(1, k + 1):
            res = res * (n - k + i) // i
        return res % MOD

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            N, M, K = map(int, input().split())
            pts = [(1, 1)]
            for _ in range(K):
                x, y = map(int, input().split())
                pts.append((x, y))
            pts.append((N, M))
            pts.sort()

            ok = True
            for i in range(len(pts) - 1):
                if pts[i][1] > pts[i + 1][1]:
                    ok = False
                    break

            if not ok:
                out.append("0")
                continue

            ans = 1
            for i in range(len(pts) - 1):
                x1, y1 = pts[i]
                x2, y2 = pts[i + 1]
                dx, dy = x2 - x1, y2 - y1
                ans = ans * comb(dx + dy, dx) % MOD

            out.append(str(ans))
        return "\n".join(out)

    return solve()

# sample cases
assert run("""1
3 5 0
""") == "15"

assert run("""1
3 5 1
2 3
""") == "9"

assert run("""1
3 5 2
2 3
3 4
""") == "6"

assert run("""1
3 3 2
2 3
3 2
""") == "0"

# minimal grid
assert run("""1
1 1 0
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới tối thiểu | 1 | tính đúng đắn của trường hợp cơ sở | 
| ràng buộc duy nhất | 15 | đếm mạng tiêu chuẩn | 
| hai điểm có thứ tự | 6 | phép nhân nhiều đoạn | 
| điểm mâu thuẫn | 0 | cắt tỉa khả thi | 

## Vỏ cạnh 

Khi điểm yêu cầu không tương thích với cách sắp xếp đơn điệu, hãy sắp xếp theo$x$bộc lộ sự giảm sút$y$, ngay lập tức buộc trả lời 0. Ví dụ$(2,5)$Và$(3,4)$tạo ra một chuỗi được sắp xếp với mức giảm dần$y$và thuật toán dừng lại trước khi nhân, loại bỏ chính xác các ràng buộc không thể thực hiện được. 

Khi$K=0$, chỉ còn lại các điểm cuối và giải pháp giảm xuống một hệ số nhị thức duy nhất$\binom{N+M-2}{N-1}$, được xử lý một cách tự nhiên bởi cơ cấu sản phẩm cùng phân khúc. 

Khi nhiều điểm trùng nhau, việc sắp xếp sẽ giữ chúng liền kề nhau và các đoạn có độ dài bằng 0 góp phần nhận dạng nhân, duy trì tính chính xác mà không cần viết hoa đặc biệt.
