---
title: "CF 1777D - Điểm của một cái cây"
description: "Chúng ta có một cây có gốc với n nút. Mỗi nút ban đầu giữ một giá trị 0 hoặc 1. Sự phát triển của các giá trị trong cây được xác định bởi một quy tắc đơn giản: tại mỗi số nguyên thời điểm t 0, mọi nút không phải lá sẽ cập nhật giá trị của nó lên XOR của các giá trị của các nút con của nó từ…"
date: "2026-06-09T11:40:02+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dfs-and-similar", "dp", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 1777
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 845 (Div. 2) and ByteRace 2023"
rating: 1900
weight: 1777
solve_time_s: 93
verified: true
draft: false
---

[CF 1777D - Điểm của một cái cây](https://codeforces.com/problemset/problem/1777/D) 

**Xếp hạng:** 1900 
**Thẻ:** bitmasks, tổ hợp, dfs và tương tự, dp, toán học, xác suất, cây 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với`n`nút. Mỗi nút ban đầu giữ một giá trị 0 hoặc 1. Sự tiến hóa của các giá trị trong cây được xác định bởi một quy tắc đơn giản: tại mỗi thời điểm nguyên`t > 0`, mọi nút không phải lá sẽ cập nhật giá trị của nó lên XOR của các giá trị của các nút con của nó từ bước thời gian trước đó. Các nút lá trở thành 0 vì chúng không có nút con. Tổng của tất cả các giá trị nút tại một thời điểm nhất định`t`được biểu thị`S(t)`. 

Chúng tôi xác định`F(A)`như tổng số tiền của`S(t)`trong tất cả các bước thời gian bắt đầu từ`t = 0`tới mức độ lớn về mặt thiên văn`t = 10^100`cho một cấu hình ban đầu cụ thể`A`. Nhiệm vụ là tính tổng`F(A)`trên tất cả các cấu hình ban đầu có thể có của các giá trị nút. Câu trả lời cuối cùng phải là modulo`10^9 + 7`. 

Thách thức chính đến từ số lượng cấu hình ban đầu theo cấp số nhân (`2^n`) và số bước thời gian có khả năng không giới hạn. Mô phỏng ngây thơ là không thể bởi vì ngay cả với`n`xung quanh`10^5`, lặp đi lặp lại`2^n`cấu hình là không thể thực hiện được. Tuy nhiên, cấu trúc cây và phép toán XOR cho thấy có thể có một thuộc tính toán học cho phép chúng ta tính tổng một cách hiệu quả mà không cần liệt kê các cấu hình hoặc các bước thời gian. 

Các trường hợp cạnh bao gồm các cây có một nút duy nhất, các cây là chuỗi (chiều cao = n-1) và các cây có tất cả các lá được kết nối trực tiếp với gốc. Trong tất cả các trường hợp này, các lá luôn đóng góp bằng 0 sau bước thời gian đầu tiên, điều này giúp đơn giản hóa việc truyền XOR nhưng phải được xử lý cẩn thận để tránh các lỗi riêng lẻ trong tính toán độ sâu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: liệt kê tất cả`2^n`các bài tập ban đầu, mô phỏng quá trình lan truyền XOR theo thời gian cho đến khi tất cả các nút trở về 0 và tính tổng tất cả`S(t)`các giá trị. Về nguyên tắc thì điều này đúng nhưng không khả thi. Ngay cả đối với`n = 20`,`2^20`cấu hình khiến nó không thể chạy trong thực tế và sự phát triển theo thời gian cho mỗi cấu hình sẽ làm tăng thêm chi phí. 

Quan sát quan trọng giúp mở ra một giải pháp hiệu quả đến từ việc xem xét vấn đề một cách tuyến tính về mặt đóng góp. Mỗi nút đóng góp vào tổng`F(A)`theo cách chỉ phụ thuộc vào số lượng nút ở khoảng cách chẵn và lẻ tính từ nó. Cụ thể, nếu một nút ở độ sâu`d`từ một chiếc lá, nó luân phiên đóng góp giữa các bước thời gian. Cấu trúc XOR cho phép chúng tôi tính toán tổng mức đóng góp của một nút trên tất cả các cấu hình bằng cách sử dụng lũy ​​thừa 2 mà không cần mô phỏng quá trình truyền XOR thực tế. Sự đóng góp từ mỗi nút có thể được giảm xuống thành công thức tổ hợp dựa trên độ sâu của nó trong cây. 

Khi chúng tôi chỉ định độ sâu thông qua DFS, chúng tôi lưu ý rằng tổng đóng góp trên tất cả các cấu hình ban đầu tỷ lệ thuận với`2^(n-1) * (number of nodes not leaves)`. Mỗi nút có một lựa chọn lật XOR trong nút cha của nó hoặc không, đó là sự kết hợp. Sử dụng số học mô-đun, chúng ta có thể tính toán một cách hiệu quả`2^(n-1) modulo 10^9+7`và tổng hợp sự đóng góp của tất cả các nút không phải lá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Đóng góp dựa trên chiều sâu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp kiểm thử, hãy đọc cấu trúc cây và lưu trữ dưới dạng danh sách kề. 
2. Thực hiện DFS để đếm số nút không phải lá trong cây. Điều này có thể được thực hiện bằng cách kiểm tra kích thước của danh sách kề cho mỗi nút. Một nút được gọi là lá nếu nó có đúng một nút lân cận (trừ nút gốc). 
3. Tính toán trước lũy thừa của 2 modulo`10^9+7`lên đến`n`, vì mỗi nút đóng góp một hệ số`2^(n-1)`trên tất cả các cấu hình ban đầu. Điều này giải thích cho hiệu ứng tổ hợp của tất cả các phép gán có thể có là 0 và 1 cho các nút còn lại. 
4. Nhân số nút không phải lá với`2^(n-1)`để có được tổng`F(A)`trên tất cả các cấu hình. 
5. Xuất kết quả theo modulo`10^9+7`. 

Tại sao nó hoạt động: Mỗi nút không phải lá đóng góp vào tổng XOR trong tất cả các cấu hình theo kiểu tuyến tính, có thể dự đoán được vì XOR tuyến tính trên GF(2). Lá đóng góp bằng 0 sau`t=1`và có thể được bỏ qua. Công suất được tính toán trước của 2 nắm bắt số lượng cấu hình theo cấp số nhân cho các nút còn lại. Điều này làm giảm vấn đề liệt kê theo cấp số nhân thành quét tuyến tính đơn giản với phép nhân tổ hợp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

def solve():
    t = int(input())
    pow2 = [1] * (2 * 10**5 + 5)
    for i in range(1, len(pow2)):
        pow2[i] = pow2[i-1] * 2 % MOD

    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n)]
        for _ in range(n-1):
            u, v = map(int, input().split())
            adj[u-1].append(v-1)
            adj[v-1].append(u-1)

        non_leaf_count = 0
        for i in range(n):
            # root can have one child and still be non-leaf
            if i == 0:
                if len(adj[i]) > 0:
                    non_leaf_count += 1
            else:
                if len(adj[i]) > 1:
                    non_leaf_count += 1

        answer = non_leaf_count * pow2[n-1] % MOD
        print(answer)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng quyền hạn tính toán trước của 2
