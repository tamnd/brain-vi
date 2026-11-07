---
title: "CF 1363C - Trò Chơi Trên Lá"
description: "Trò chơi được chơi trên một cái cây, là một đồ thị không có chu kỳ, được kết nối và vô hướng. Mỗi nút được đánh số từ $1$ đến $n$ và một nút $x$ là đặc biệt. Hai người chơi lần lượt loại bỏ các nút lá, trong đó một lá là một nút chỉ có một nút lân cận, cùng với cạnh kết nối của nó."
date: "2026-06-11T12:32:11+07:00"
tags: ["codeforces", "competitive-programming", "games", "trees"]
categories: ["algorithms"]
codeforces_contest: 1363
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 646 (Div. 2)"
rating: 1600
weight: 1363
solve_time_s: 372
verified: true
draft: false
---

[CF 1363C - Game On Leaves](https://codeforces.com/problemset/problem/1363/C) 

**Đánh giá:** 1600 
**Tags:** trò chơi, cây cối 
**Thời gian giải:** 6 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi được chơi trên một cái cây, là một đồ thị không có chu kỳ, được kết nối và vô hướng. Mỗi nút được đánh số từ$1$ĐẾN$n$và một nút$x$là đặc biệt. Hai người chơi lần lượt loại bỏ các nút lá, trong đó một lá là một nút chỉ có một nút lân cận, cùng với cạnh kết nối của nó. Người chơi loại bỏ nút đặc biệt$x$thắng. Ayush di chuyển trước và nhiệm vụ là xác định người chơi nào sẽ giành chiến thắng trong lối chơi tối ưu. 

Đầu vào mô tả cây như$n$nút và$n-1$các cạnh, theo sau là một số trường hợp thử nghiệm$t$. Đầu ra là một chuỗi xác định người chiến thắng cho mỗi trường hợp thử nghiệm. Số lượng nút$n$nhiều nhất là$1000$, do đó, bất kỳ thuật toán bậc hai nào trong$n$hoặc tốt hơn sẽ chạy hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó nút đặc biệt$x$đã là một chiếc lá rồi, vì khi đó Ayush có thể loại bỏ nó ở lượt đầu tiên và ngay lập tức giành chiến thắng. Một trường hợp tế nhị khác phát sinh khi$x$có đúng một nút lân cận và tổng số nút là nhỏ. Nếu giải pháp bất cẩn cho rằng tất cả các nút đều có bậc ít nhất là 2 hoặc thay phiên nhau một cách mù quáng mà không kiểm tra cấu trúc xung quanh$x$, nó sẽ cho kết quả sai. 

Ví dụ, hãy xem xét một cái cây có$n=3$,$x=1$và các cạnh$1-2$Và$1-3$. Nút$2$Và$3$là những chiếc lá. Ayush chỉ có thể loại bỏ một trong số này, sau đó$1$trở thành một chiếc lá và Ashish có thể loại bỏ nó, chiến thắng trong trò chơi. Điều này minh họa rằng ngay cả với một cái cây nhỏ, cấu trúc cục bộ xung quanh$x$quyết định kết quả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trò chơi một cách rõ ràng. Ở mỗi lượt, chúng tôi sẽ liệt kê tất cả các lá hiện tại, tạo ra cây kết quả từ mỗi lần di chuyển có thể và tính toán đệ quy người chiến thắng cho lượt tiếp theo. Phương pháp này đúng vì nó áp dụng trực tiếp các quy tắc và khám phá tất cả các trạng thái trò chơi, nhưng nó chậm theo cấp số nhân.$n$vì số lượng cây con tăng lên nhanh chóng và$n$có thể lên tới 1000, khiến phương pháp này không khả thi. 

Cách tiếp cận tối ưu sử dụng cái nhìn sâu sắc rằng trò chơi hoàn toàn được xác định bởi cấp độ của nút đặc biệt$x$và tính chẵn lẻ của tổng số nút. Nếu như$x$có bằng cấp$1$hoặc$0$, đó là lá hoặc bị cô lập nên Ayush có thể loại bỏ nó ngay lập tức và giành chiến thắng. Nếu không, trò chơi sẽ trở nên tương đương với việc loại bỏ các$n-1$các nút, với nước đi cuối cùng sẽ xác định người chiến thắng. Cụ thể, nếu$n$chẵn, Ayush sẽ loại bỏ chiếc lá cuối cùng và giành chiến thắng; nếu như$n$lẻ, Ashish sẽ loại bỏ chiếc lá cuối cùng và giành chiến thắng. Quan sát này làm giảm vấn đề để kiểm tra mức độ của$x$và sự ngang bằng của$n$, tránh mọi mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc số lượng nút$n$và nút đặc biệt$x$. 
3. Xây dựng danh sách kề cho cây từ$n-1$các cạnh. 
4. Tính cấp độ của nút đặc biệt$x$. 
5. Nếu mức độ$x$là$1$hoặc$0$, Ayush thắng ngay vì có thể loại bỏ nó ngay ở nước đi đầu tiên. 
6. Ngược lại, hãy kiểm tra tính chẵn lẻ của$n$. Nếu như$n$chẵn, Ayush thắng; nếu như$n$thật kỳ lạ, Ashish thắng. Lý do là mỗi nước đi sẽ loại bỏ đúng một nút và người chơi đi khi tổng số nút còn lại là$1$thắng. 

Tại sao nó hoạt động: Điều bất biến chính là người chiến thắng được xác định bởi ai buộc phải loại bỏ nút cuối cùng. Nếu như$x$ban đầu là một chiếc lá, Ayush ngay lập tức thắng. Mặt khác, mỗi lần di chuyển sẽ làm giảm số nút đi một, do đó tính chẵn lẻ của$n$liên quan đến bước đi đầu tiên sẽ xác định ai sẽ loại bỏ nút cuối cùng. Thuật toán tận dụng cấu trúc cây mà không cần mô phỏng rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)
        if len(adj[x]) <= 1:
            print("Ayush")
        else:
            if n % 2 == 0:
                print("Ayush")
            else:
                print("Ashish")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả và xây dựng danh sách kề cho cây. Kiểm tra`len(adj[x])`đưa ra mức độ của nút đặc biệt, trực tiếp xác định kịch bản thắng ngay lập tức. Kiểm tra tính chẵn lẻ trên`n`tính đến sự luân phiên của các bước di chuyển khi$x$không phải là một chiếc lá. Xử lý nhiều trường hợp thử nghiệm trong một vòng lặp đảm bảo tính chính xác trên các đầu vào khác nhau. Một điểm tinh tế đang sử dụng`n+1`để lân cận khớp với các chỉ số nút dựa trên 1. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
1
3 1
2 1
3 1
```| Bước | n | x | độ(x) | n % 2 | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 3 | 1 | 2 | 1 | Ashish | 

Ayush không thể loại bỏ nút 1 ngay lập tức, vì vậy chúng tôi kiểm tra tính chẵn lẻ. n=3 là số lẻ nên người chơi thứ hai Ashish thắng. 

**Đầu vào tùy chỉnh 1**```
1
4 2
1 2
2 3
3 4
```| Bước | n | x | độ(x) | n % 2 | Người chiến thắng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 4 | 2 | 2 | 0 | Ayush | 

Bậc của x là 2 nên nước đi đầu tiên không loại bỏ được x. Tổng số nút n=4 là số chẵn nên Ayush thắng. 

Những dấu vết này xác nhận rằng cả trường hợp lá ngay lập tức và kịch bản dựa trên chẵn lẻ đều được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đọc các cạnh của cây và xây dựng danh sách kề cho mỗi trường hợp thử nghiệm mất O(n) thời gian. | 
| Không gian | O(n) | Danh sách kề sử dụng không gian O(n) và một vài số nguyên cho mỗi trường hợp thử nghiệm là không đáng kể. | 

Thuật toán dễ dàng xử lý n 1000 và t 10 trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("1\n3 1\n2 1\n3 1\n") == "Ashish", "sample 1"

# Custom cases
assert run("1\n4 2\n1 2\n2 3\n3 4\n") == "Ayush", "even n, x not leaf"
assert run("1\n1 1\n") == "Ayush", "single node, x is leaf"
assert run("1\n2 2\n1 2\n") == "Ayush", "x is leaf initially"
assert run("1\n5 3\n1 2\n2 3\n3 4\n4 5\n") == "Ashish", "odd n, x internal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2, cạnh 1-2,2-3,3-4 | Ayush | Kịch bản chẵn n, x không lá | 
| 1 nút x=1 | Ayush | Trường hợp cạnh nút đơn | 
| 2 nút x=2, cạnh 1-2 | Ayush | x ban đầu là lá | 
| 5 nút x=3, đường dẫn | Ashish | Lẻ n, x nút nội | 

## Vỏ cạnh 

Nếu cây chỉ có một nút thì nút đó tự động là một lá. Thuật toán xử lý việc này bằng cách kiểm tra`len(adj[x]) <= 1`, báo cáo chính xác Ayush là người chiến thắng. 

Trong trường hợp x ban đầu là lá nhưng n > 1, Ayush thắng ngay và danh sách kề hiển thị chính xác`len(adj[x]) == 1`, kích hoạt nhánh thích hợp. 

Đối với các chuỗi dài hoặc cây có x là nút trung tâm có bậc >1, tính chẵn lẻ của n sẽ quyết định kết quả. Việc xây dựng danh sách kề với chỉ mục dựa trên 1 đảm bảo tính toán độ chính xác và kiểm tra modulo xác định chính xác người chơi sẽ loại bỏ nút cuối cùng. Các trường hợp đặc biệt này xác nhận rằng cả logic dựa trên cấu trúc và tính chẵn lẻ đều được triển khai chính xác.
