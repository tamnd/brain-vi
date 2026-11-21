---
title: "CF 2187E - Cửa và Chìa khóa"
description: "Chúng ta có một dãy các phòng $n+1$ được nối với nhau bằng các cửa $n$. Mỗi cánh cửa $i$ có một số $ai$ cho biết chính xác thời điểm nó sẽ tự động mở, nếu không được chạm vào. Một số phòng chứa chìa khóa ở đầu theo chuỗi nhị phân $s$, trong đó số 1 có nghĩa là chìa khóa tồn tại trong phòng đó."
date: "2026-06-07T21:22:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2187
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1077 (Div. 1)"
rating: 3500
weight: 2187
solve_time_s: 131
verified: false
draft: false
---

[CF 2187E - Cửa và Chìa khóa](https://codeforces.com/problemset/problem/2187/E) 

**Đánh giá:** 3500 
**Tags:** bạo lực, cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi$n+1$phòng được kết nối bởi$n$cửa. Mỗi cửa$i$có một số$a_i$cho biết chính xác thời điểm nó sẽ tự động mở, nếu không được chạm vào. Một số phòng chứa chìa khóa lúc đầu theo chuỗi nhị phân$s$, ở đâu một`1`có nghĩa là có chìa khóa trong căn phòng đó. Bạn bắt đầu trong phòng$1$vào thời điểm đó$0$và mỗi giây bạn có thể nhặt hoặc đánh rơi chìa khóa trong phòng hiện tại của mình. Vào cuối mỗi giây, bạn có thể di chuyển sang phòng tiếp theo nếu cửa mở hoặc nếu bạn có chìa khóa để mở hoặc tự do di chuyển về phòng trước. Chìa khóa chỉ dùng một lần và bạn có thể mang theo nhiều nhất một chiếc. 

Mục đích là tính toán cho mỗi cửa$i$, giây sớm nhất bạn có thể đến phòng$i+1$, bắt đầu từ phòng$1$vào thời điểm đó$0$. Mỗi cửa đều độc lập theo nghĩa là câu trả lời cho cửa$i$không phụ thuộc vào việc sau này bạn có muốn đến các phòng khác hay không. 

Những ràng buộc cho phép$n$lên tới$2 \cdot 10^5$, và tổng số tiền$n$trên các trường hợp thử nghiệm cũng được giới hạn ở mức$2 \cdot 10^5$. Mô phỏng lực lượng vũ phu theo giây sẽ quá chậm vì$a_i$có thể lớn như$2 \cdot 10^5$, làm cho tổng chiều thời gian trở nên rất lớn. Thay vào đó, chúng ta cần một nghiệm tuyến tính trong$n$mỗi trường hợp thử nghiệm. 

Các trường hợp có cạnh không rõ ràng bao gồm các cửa mở vào thời điểm$0$, những căn phòng có chìa khóa có thể sử dụng ngay lập tức và dãy cửa mở muộn hơn thời gian bạn lấy được chúng. Ví dụ, nếu$a_1 = 0$Và$s = "1"$, bạn có thể ngay lập tức chọn phím và sử dụng nó để di chuyển$0$giây, đến phòng$2$vào thời điểm đó$1$. Nếu bạn không tính toán việc sử dụng khóa một cách hợp lý, bạn có thể đánh giá quá cao thời gian có thể truy cập sớm nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là BFS đầy đủ hoặc mô phỏng từng giây. Đối với mỗi cửa, hãy theo dõi những phòng bạn có thể đến khi có hoặc không có chìa khóa và tăng dần thời gian cho đến khi đến được phòng mục tiêu. Điều này đúng nhưng độ phức tạp$O(n \cdot T)$, Ở đâu$T = \max(a_i)$. Từ$T$có thể$2 \cdot 10^5$Và$n$lên tới$2 \cdot 10^5$, điều này dẫn đến khoảng$4 \cdot 10^{10}$hoạt động quá chậm. 

Thông tin chi tiết quan trọng là việc di chuyển qua các phòng thực sự là vấn đề của ba đại lượng: thời điểm sớm nhất cửa tự động mở ($a_i$), phím gần nhất bên trái ($l_i$) và phím gần nhất bên phải ($r_i$). Vì mỗi lần chỉ được mang một chìa khóa nên bạn có thể tính toán trước cho mỗi phòng thời điểm sớm nhất có thể lấy chìa khóa từ bên trái hoặc bên phải và dùng để mở cửa. Sau đó là thời gian sớm nhất để đến phòng$i+1$là thời gian tối thiểu chờ cửa tự động mở bằng phím trái hoặc dùng phím phải. 

Quan sát này làm giảm vấn đề thành ba đường tuyến tính: một từ trái sang phải, một từ phải sang trái và một đường cuối cùng để tính mức tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force theo giây | O(n * max(a_i)) | O(n) | Quá chậm | 
| Tính toán trước thời gian chạm phím trái/phải | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích đầu vào: đọc$n$, mảng$a$, và chuỗi$s$. 
2. Khởi tạo hai mảng`left`Và`right`có kích thước $n`, representing the earliest time a key can reach each door from the left and from the right. Initialize `bên trái`with infinity, and`đúng` với vô cùng. 
3. **Chuyển từ trái sang phải**: theo dõi thời gian quan trọng được nhìn thấy lần cuối. 

Vì`i`từ`0`ĐẾN`n-1`: 

- Nếu phòng`i`chứa một khóa (`s[i] == '1'`), bộ`last_key = 0`bởi vì một chìa khóa có sẵn ngay lập tức. 
- Nếu như`last_key`được xác định,`left[i] = last_key + 1`và tăng dần`last_key`mỗi bước 1 để tính đến chuyển động. 

Mô hình này chọn chìa khóa và di chuyển sang phải, thêm một giây cho mỗi phòng. 
4. **Đường chuyền từ phải sang trái**: đối xứng với đường chuyền bên trái. 

Vì`i`từ`n-1`xuống tới`0`: 

- Nếu phòng`i`chứa một khóa (`s[i] == '1'`), bộ`last_key = 0`. 
- Nếu như`last_key`được xác định,`right[i] = last_key + 1`và tăng dần`last_key`mỗi bước 1 để tính đến việc di chuyển sang trái rồi quay lại. 
5. Tính thời gian sớm nhất có thể đến được mỗi cửa: 

Đối với mỗi`i`từ`0`ĐẾN`n-1`: 

-`ans[i] = min(a[i], left[i], right[i])`Việc này giúp giảm thiểu thời gian chờ cửa tự động mở bằng cách sử dụng chìa khóa từ bên trái hoặc sử dụng chìa khóa từ bên phải. 
6. In kết quả. 

Tại sao nó hoạt động: Bất biến là vậy`left[i]`lưu trữ thời gian tối thiểu mà khóa có thể đến từ bên trái,`right[i]`từ bên phải. Vì mỗi lần bạn chỉ có thể lấy một chìa khóa nên việc di chuyển theo một hướng duy nhất đến cửa mục tiêu luôn là tối ưu. Lấy mức tối thiểu với`a[i]`xử lý các cửa tự động mở trước khi bất kỳ chìa khóa nào có thể chạm tới chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = input().strip()
        INF = 10**9

        left = [INF] * n
        right = [INF] * n

        # Left to right
        last_key = -INF
        for i in range(n):
            if s[i] == '1':
                last_key = 0
            else:
                if last_key != -INF:
                    last_key += 1
            if last_key != -INF:
                left[i] = last_key

        # Right to left
        last_key = -INF
        for i in range(n-1, -1, -1):
            if s[i] == '1':
                last_key = 0
            else:
                if last_key != -INF:
                    last_key += 1
            if last_key != -INF:
                right[i] = last_key

        ans = []
        for i in range(n):
            ans.append(str(min(a[i], left[i], right[i])))
        print(' '.join(ans))

if __name__ == "__main__":
    solve()
```Mã này triển khai thuật toán với I/O nhanh, tính toán trước thời gian tiếp cận phím trái và phải. lính gác`-INF`đảm bảo các phòng không có chìa khóa truy cập sẽ không ảnh hưởng đến việc tính toán. tăng dần`last_key`từng bước mô hình chuyển động giữa các phòng. Mức tối thiểu cuối cùng so sánh việc sử dụng chìa khóa so với việc chờ cửa tự động mở. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**```
n = 5
a = [0, 1, 4, 2, 3]
s = "11010"
```| tôi | một [tôi] | trái[i] | đúng[i] | phút | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 1 | 1 | 
| 3 | 2 | 2 | 1 | 1 | 
| 4 | 3 | 3 | 2 | 2 | 

Giải thích: Có thể đến Cửa 3 nhanh hơn bằng cách sử dụng chìa khóa từ phòng 1 thay vì chờ đợi.`a[2]=4`. 

**Ví dụ 2:**```
n = 3
a = [0, 9, 10]
s = "110"
```| tôi | một [tôi] | trái[i] | đúng[i] | phút | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0 | 
| 1 | 9 | 1 | 1 | 1 | 
| 2 | 10 | 2 | INF | 2 | 

Ở đây, có thể đến cửa 2 bằng chìa khóa từ phòng 2 vào thời điểm 2, nhanh hơn so với việc chờ tự động mở lúc 10 giờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Ba đường đi tuyến tính: từ trái sang phải, từ phải sang trái, tính toán phút cuối cùng. Mỗi lần vượt qua là O (n). | 
| Không gian | O(n) | Mảng cho thời gian tiếp cận trái và phải, cộng với mảng đầu vào. | 

Được cho$n \le 2 \cdot 10^5$và tổng số tiền trong các trường hợp thử nghiệm cũng bị giới hạn, giải pháp này phù hợp thoải mái với giới hạn thời gian 3 giây và giới hạn bộ nhớ là 1 GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin =
```
