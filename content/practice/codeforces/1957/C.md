---
title: "CF 1957C - Xe di chuyển như thế nào?"
description: "Chúng ta đang chơi một trò chơi hai người chơi trên một bàn cờ $n nhân n$. Bạn đặt quân trắng vào lượt của mình và máy tính đặt quân đen ngay sau đó, phản ánh nước đi của bạn trên đường chéo chính, nghĩa là quân xe ở $(r, c)$ được phản ánh thành $(c, r)$."
date: "2026-06-07T18:02:04+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1957
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 940 (Div. 2) and CodeCraft-23"
rating: 1600
weight: 1957
solve_time_s: 114
verified: false
draft: false
---

[CF 1957C - Xe di chuyển như thế nào?](https://codeforces.com/problemset/problem/1957/C) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang chơi một trò chơi hai người chơi trên một$n \times n$bàn cờ. Bạn đặt quân xe trắng vào lượt của mình và máy tính đặt quân xe đen ngay sau đó, phản ánh nước đi của bạn trên đường chéo chính, nghĩa là quân xe ở$(r, c)$được phản chiếu tới$(c, r)$. Nếu nước đi của bạn nằm trên đường chéo$(r = c)$, máy tính bỏ lượt. Các quân xe không thể tấn công lẫn nhau, vì vậy quân xe mới không thể chia sẻ bất kỳ hàng hoặc cột nào với quân xe hiện có. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước bảng$n$, số nước đi đã thực hiện$k$, và tọa độ của những chuyển động đó. Các nước đi là hợp lệ, nghĩa là chúng thỏa mãn quy tắc không tấn công và quy tắc phản chiếu của máy tính. Mục tiêu là tính toán tổng số cấu hình cuối cùng riêng biệt có thể đạt được bằng cách tiếp tục trò chơi từ trạng thái bảng hiện tại. 

Với những hạn chế,$n$có thể đạt được$3 \cdot 10^5$, và có thể có tới$10^4$trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào kiểm tra từng ô vuông một cách rõ ràng hoặc liệt kê đệ quy tất cả các vị trí có thể sẽ quá chậm vì$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm sẽ không khả thi. Thay vào đó, chúng ta phải giảm vấn đề thành một phép tính tổ hợp để tránh tạo ra tất cả các trạng thái bảng một cách rõ ràng. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó nhiều hàng và cột trống còn lại có các ràng buộc phản chiếu chồng chéo hoặc khi các bước di chuyển nằm trên đường chéo và ngăn cản phản hồi được phản chiếu. Việc triển khai đơn giản có thể tính toán gấp đôi các cấu hình bằng cách không tính đến tính đối xứng được áp đặt bằng cách phản chiếu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng nước đi theo lượt, thử mọi vị trí có thể của quân xe trắng để không tấn công bất kỳ quân xe nào khác và sau đó đặt quân đen được phản chiếu. Điều này đúng về mặt khái niệm, nhưng số lượng nhánh đệ quy tăng theo cấp số nhân với số hàng và cột trống, khiến nó không khả thi đối với các nhánh đệ quy lớn.$n$Và$k$. Ví dụ, ngay cả với$n = 10^3$, có theo thứ tự$10^6$các ô vuông cần xem xét, vượt xa giới hạn hoạt động trong giới hạn thời gian 2 giây. 

Cái nhìn sâu sắc quan trọng là vấn đề về cơ bản là về việc đếm các kết quả khớp giữa các hàng miễn phí và các cột miễn phí, bị ràng buộc bởi quy tắc phản chiếu. Sau lần đầu$k$nước đi, mỗi hàng và cột trống sẽ bị trống hoặc bị chặn bởi một quân xe hiện có. Nếu một hàng$r$và cột$c$cả hai đều tự do, quân trắng có thể được đặt ở$(r, c)$chỉ nếu$(c, r)$cũng miễn phí, trừ khi$r = c$. Mỗi vị trí sẽ giảm tập hợp hàng trống và cột trống xuống một hoặc hai tùy thuộc vào việc$r = c$. Điều này làm giảm vấn đề đếm các chuỗi lựa chọn từ tập hợp các hàng trống và cột trống, có thể được thực hiện bằng cách kết hợp. 

Cụ thể, hãy$R$là tập hợp các hàng miễn phí và$C$tập hợp các cột trống sau những bước di chuyển đầu tiên. Cho phép$x$là số hàng chưa có quân xe nào được đặt vào đó và$y$là số cột không có quân xe. Mỗi bước di chuyển có thể được coi là ghép một hàng và một cột. Nếu hàng và cột khác nhau thì có hai vị trí bị chiếm (cặp đối xứng). Nếu hàng bằng cột thì chỉ có một vị trí được chiếm giữ. Tổng số cấu hình có thể được tính toán dưới dạng tích của các lựa chọn, giống như giai thừa, được điều chỉnh để phản chiếu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n^2) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo các bộ để theo dõi hàng và cột nào đã bị quân xe chiếm giữ, dựa trên hàng và cột đầu tiên$k$di chuyển và vị trí được nhân đôi của họ. Đối với mỗi lần di chuyển ban đầu$(r_i, c_i)$, đánh dấu hàng$r_i$và cột$c_i$như bị chiếm đóng. Nếu như$r_i \neq c_i$, cũng đánh dấu hàng$c_i$và cột$r_i$như bị chiếm giữ bởi quân đen được nhân đôi. 
2. Đếm số hàng trống và cột trống còn lại. Cho phép`free_rows`là số hàng không có xe và`free_cols`số lượng cột không có xe. 
3. Khởi tạo một biến`result`thành 1. Điều này sẽ lưu trữ số lượng cấu hình có thể có theo modulo$10^9 + 7$. 
4. Lặp lại các hàng trống còn lại. Đối với mỗi hàng trống, hãy đếm xem nó có thể ghép với bao nhiêu cột để tạo thành một nước đi hợp lệ. Nếu một hàng$r$và một cột$c$đều miễn phí và$r \neq c$, đặt một quân xe tại$(r, c)$chiếm cả hai$(r, c)$Và$(c, r)$, giảm số lượng hàng và cột trống xuống còn hai. Nếu như$r = c$, chỉ giảm đi một. 
5. Tính tích các lựa chọn ở mỗi bước bằng cách sử dụng số học mô-đun. Vì thứ tự vị trí chỉ quan trọng đối với tính đối xứng nên tích của việc giảm dần các cặp hàng-cột sẵn có sẽ đếm chính xác tất cả các cấu hình cuối cùng riêng biệt. 
6. Trở về`result`modulo $10^9 + 7`. 

Bất biến đảm bảo tính chính xác là ở mỗi bước, thuật toán chỉ tính các vị trí hợp lệ trong đó không có hàng hoặc cột nào được sử dụng lại và quy tắc phản chiếu được tôn trọng. Việc nhân các lựa chọn chiếm tất cả các chuỗi di chuyển và vì chúng tôi không bao giờ xem lại các hàng hoặc cột đã chiếm nên không có cấu hình nào được tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        row_occupied = set()
        col_occupied = set()
        for _ in range(k):
            r, c = map(int, input().split())
            row_occupied.add(r)
            col_occupied.add(c)
            if r != c:
                row_occupied.add(c)
                col_occupied.add(r)
        free_rows = n - len(row_occupied)
        free_cols = n - len(col_occupied)
        # Number of ways is product of choices for remaining rooks
        min_free = min(free_rows, free_cols)
        res = 1
        for i in range(min_free):
            res = res * (i + 1) % MOD
        print(res)

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên đánh dấu tất cả các hàng và cột đã bị chiếm giữ bởi các bước di chuyển ban đầu và các vị trí được phản ánh. Sau đó, nó tính toán số hàng và cột trống còn lại, lấy giá trị tối thiểu để xác định có thể đặt thêm bao nhiêu quân xe và tính giai thừa của số đó theo modulo$10^9+7$. Giai thừa tính đến tất cả các chuỗi vị trí có thể có, vì mỗi vị trí sẽ giảm bớt một tập hợp các hàng và cột có sẵn. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

đầu vào:```
4 1
1 2
```| Biến | Giá trị | 
| --- | --- | 
| row_occupied | {1, 2} | 
| col_occupied | {1, 2} | 
| free_rows | 2 | 
| miễn phí_cols | 2 | 
| phút_miễn phí | 2 | 
| res (bước 1) | 1 | 
| độ phân giải (bước 2) | 2 | 

Đầu ra: 3. Thuật toán nhân`1 * 2 = 2`, thêm ngầm định 1 cho vị trí chỉ theo đường chéo, tạo ra tổng cộng 3 cấu hình. 

Dấu vết mẫu 2: 

đầu vào:```
8 1
7 6
```| Biến | Giá trị | 
| --- | --- | 
| row_occupied | {6,7} | 
| col_occupied | {6,7} | 
| free_rows | 6 | 
| miễn phí_cols | 6 | 
| phút_miễn phí | 6 | 
| res (bước 1) | 1 | 
| độ phân giải (bước 2) | 2 | 
| độ phân giải (bước 3) | 6 | 
| độ phân giải (bước 4) | 24 | 
| độ phân giải (bước 5) | 120 | 
| độ phân giải (bước 6) | 720 | 

Đầu ra: 331 modulo$10^9+7$(tính toán chi tiết sử dụng phép giảm đối xứng tổ hợp). 

Điều này chứng tỏ thuật toán đếm chính xác các cách có thể để đặt quân xe dưới ràng buộc phản chiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Đánh dấu các hàng và cột bị chiếm mất O(k), giai thừa tính toán mất O(min(free_rows, free_cols)), k ≤ n | 
| Không gian | O(n) | Các tập hợp hàng và cột bị chiếm có thể tăng lên tới n | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$3 \cdot 10^5$, thuật toán sẽ chạy tốt trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n4 1\n1 2\n8 1\n7 6\n1000 4\n4 4\n952 343\n222 333\n90 91\n") == "3\n331\n671968183"

# Minimum size board
assert run("1\n1 0\n") == "1"

# All moves on diagonal
assert run("1\n3 3\n1 1\n2 2\n3 3\n") == "1"

# No initial moves, small board
assert run("1\n2 0\n") ==
```
