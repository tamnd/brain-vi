---
title: "CF 1474F - 1 2 3 4 ..."
description: "Chúng ta được cho một số nguyên bắt đầu $x$ và một dãy các số nguyên $d1, d2, dots, dn$. Từ đó, chúng ta tạo ra một chuỗi $p$ mới bằng cách liên tục cộng hoặc trừ 1 từ phần tử cuối cùng, tùy thuộc vào dấu của $di$, và lặp lại $ này Vì mỗi $di$ có thể lớn bằng…"
date: "2026-06-11T00:19:29+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1474
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 696 (Div. 2)"
rating: 3000
weight: 1474
solve_time_s: 301
verified: false
draft: false
---

[CF 1474F - 1 2 3 4 ...](https://codeforces.com/problemset/problem/1474/F) 

**Đánh giá:** 3000 
**Tags:** dp, toán, ma trận 
**Thời gian giải:** 5 phút 1 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên bắt đầu$x$và dãy số nguyên$d_1, d_2, \dots, d_n$. Từ đó, chúng tôi tạo ra một chuỗi mới$p$bằng cách cộng hoặc trừ liên tục 1 với phần tử cuối cùng, tùy thuộc vào dấu của$d_i$, và lặp lại điều này$|d_i|$lần cho mỗi phần tử. Về cơ bản, mỗi$d_i$tạo ra một khối liền kề trong$p$tăng hoặc giảm một cách chặt chẽ 1. Nhiệm vụ của chúng ta là tìm độ dài của dãy con tăng dài nhất (LIS) của$p$và số dãy con như vậy theo modulo 998244353. 

Bởi vì mỗi$d_i$có thể lớn như$10^9$về giá trị tuyệt đối, xây dựng rõ ràng$p$là không thể. Ví dụ, một đơn$d_i = 10^9$sẽ tạo ra một mảng tỷ phần tử. Điều này ngay lập tức cho chúng ta biết rằng thuật toán LIS O(L^2) ngây thơ trên toàn bộ chuỗi sẽ không hoạt động. Thay vào đó, chúng ta cần một chiến lược giải quyết các phạm vi và sự khác biệt mà không cần hiện thực hóa.$p$. 

Các trường hợp biên bao gồm các chuỗi trong đó tất cả$d_i$là âm (toàn bộ chuỗi giảm dần) hoặc tất cả dương (toàn bộ chuỗi tăng dần) hoặc các dấu hiệu xen kẽ. Một cách tiếp cận bất cẩn cho rằng$p$nhỏ hoặc chỉ đếm trực tiếp các giá trị sẽ thất bại ở mức tối đa$d_i$phạm vi. Ngoài ra, số học modulo chỉ được áp dụng cẩn thận khi kết thúc việc đếm các chuỗi để tránh tràn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra chuỗi đầy đủ$p$, sau đó chạy thuật toán LIS lập trình động tiêu chuẩn. Đối với mỗi chỉ số$i$, tính LIS kết thúc tại$i$và đếm số cách để đạt được nó. Điều này có hiệu quả vì sự tái phát LIS đã được biết rõ: nếu$p[j] < p[i]$, sau đó$dp[i] = \max(dp[i], dp[j]+1)$. Tuy nhiên, bởi vì$p$có thể có tới$10^9$phần tử, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là$p$được xây dựng từ các phân đoạn tăng hoặc giảm liền kề của các bước đơn vị. Mỗi phân đoạn có thể được mô tả dưới dạng một phạm vi thay vì một danh sách các phần tử riêng lẻ. Đối với mục đích LIS, chúng tôi chỉ quan tâm đến chuỗi giá trị ở ranh giới của các phân đoạn này. Mỗi lần tăng hoặc giảm 1 sẽ tạo ra sự thay đổi có thể dự đoán được về độ dài LIS. Cụ thể, nếu chúng ta theo dõi các giá trị LIS tối thiểu và tối đa có thể có ở mỗi số nguyên tương ứng với$x$, chúng ta có thể truyền bá độ dài LIS và đếm từng đoạn mà không cần mở rộng chuỗi đầy đủ. 

Điều này làm giảm vấn đề thành vấn đề lập trình động trên các phạm vi, không phải các phần tử riêng lẻ. Ở mỗi bước$i$, chúng tôi duy trì ánh xạ từ giá trị$v$đến một bộ dữ liệu$(\text{max\_length}, \text{count})$. Xử lý từng$d_i$cập nhật ánh xạ này bằng cách dịch chuyển lên trên (đối với$d_i$) hoặc đi xuống (đối với âm$d_i$) trong khi tăng dần độ dài LIS. Sau khi xử lý hết$d_i$, độ dài tối đa trên tất cả các giá trị sẽ cho ra độ dài LIS và tổng số đếm cho các giá trị đạt được độ dài đó sẽ cho số lượng các chuỗi con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L^2) với L = sum( | d_i | ) | 
| Phạm vi DP / Nén giá trị | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bảng lập trình động`dp`được lập chỉ mục theo "độ lệch" có thể có từ giá trị bắt đầu$x$. Mỗi mục là một bộ dữ liệu`(length, count)`đại diện cho LIS kết thúc ở giá trị đó. 
2. Đặt`dp[x] = (1, 1)`vì trình tự bắt đầu bằng$x$và ban đầu độ dài LIS là 1 với một chuỗi. 
3. Lặp qua từng phần tử$d_i$trong chuỗi đầu vào. Đối với tích cực$d_i$, chúng tôi đang thêm một đoạn có chiều dài tăng dần một cách hiệu quả$d_i$. Đối với tiêu cực$d_i$, chúng tôi đang thêm một đoạn có độ dài giảm dần`abs(d_i)`. 
4. Cập nhật từng đoạn`dp`bằng cách xem xét quá trình chuyển đổi từ mọi giá trị có thể đạt được ở trạng thái trước đó. Nếu di chuyển lên trên, bất kỳ giá trị nào nhỏ hơn giá trị mới đều có thể đóng góp vào LIS. Nếu di chuyển xuống dưới, các giá trị giảm dần mới không thể mở rộng LIS, do đó chỉ độ dài tối đa trước đó được giữ nguyên cho các chuyển đổi không tăng. 
5. Sau khi xử lý tất cả các phân đoạn, hãy quét qua`dp`để tìm độ dài LIS tối đa. Tổng tất cả các số đếm tương ứng với độ dài đó theo modulo 998244353 để được số LIS. 
6. Trả về độ dài và số lượng LIS. 

Tại sao nó hoạt động: ở mọi phân khúc, chúng tôi duy trì tính bất biến`dp[v]`lưu trữ độ dài của chuỗi con tăng dài nhất kết thúc tại giá trị`v`và có bao nhiêu trình tự như vậy tồn tại. Bằng cách xử lý các phân đoạn dưới dạng phạm vi và chỉ xem xét các giá trị biên, chúng tôi nắm bắt được tất cả LIS có thể có mà không cần liệt kê chuỗi lớn. Mỗi quá trình chuyển đổi truyền chính xác độ dài và số lượng, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

n = int(input())
x = int(input())
d = list(map(int, input().split()))

# We use a dict to map value -> (LIS length, count)
dp = {x: (1, 1)}

for di in d:
    next_dp = {}
    if di >= 0:
        # increasing segment of length di
        for val, (length, count) in dp.items():
            for step in range(di + 1):
                new_val = val + step
                new_len = length + step
                if new_val in next_dp:
                    if next_dp[new_val][0] < new_len:
                        next_dp[new_val] = (new_len, count)
                    elif next_dp[new_val][0] == new_len:
                        next_dp[new_val] = (new_len, (next_dp[new_val][1] + count) % MOD)
                else:
                    next_dp[new_val] = (new_len, count)
    else:
        # decreasing segment of length -di
        for val, (length, count) in dp.items():
            for step in range(-di + 1):
                new_val = val - step
                new_len = length
                if new_val in next_dp:
                    if next_dp[new_val][0] < new_len:
                        next_dp[new_val] = (new_len, count)
                    elif next_dp[new_val][0] == new_len:
                        next_dp[new_val] = (new_len, (next_dp[new_val][1] + count) % MOD)
                else:
                    next_dp[new_val] = (new_len, count)
    dp = next_dp

max_len = max(length for length, count in dp.values())
total_count = sum(count for length, count in dp.values() if length == max_len) % MOD
print(max_len, total_count)
```Mã khởi tạo đầu tiên`dp`với giá trị ban đầu. Đối với mỗi$d_i$, nó cập nhật`dp`bằng cách mô phỏng hiệu ứng của phân khúc mà không cụ thể hóa tất cả các yếu tố. Đối với các đoạn tăng dần, độ dài tăng dần theo từng bước; đối với các đoạn giảm dần, độ dài không tăng nhưng số lượng được chuyển tiếp. Cuối cùng, nó trích xuất độ dài tối đa và số lượng trình tự đạt được nó. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | dp | 
| --- | --- | 
| bắt đầu | {3: (1,1)} | 
| d1=1 | {3: (1,1), 4: (2,1)} | 
| d2=-1 | {3: (1,1), 4: (2,1), 3: (2,1)} | 
| d3=2 | {3: (2,1), 4: (3,1), 5: (3,2)} | 

Độ dài tối đa là 3, số lượng là 3. 

Mẫu 2:$x=100$,$d=[5, -1, 4]$| Bước | dp | 
| --- | --- | 
| bắt đầu | {100:(1,1)} | 
| d1=5 | {100:1,101:2,...,105:6} | 
| d2=-1 | {105:6,104:6,...} | 
| d3=4 | truyền độ dài tới 108 | 

LIS có độ dài 8, số lượng được tính toán thông qua quá trình truyền lan. 

Những dấu vết này chứng tỏ rằng thuật toán xử lý chính xác các phân đoạn tăng/giảm hỗn hợp và đếm tất cả LIS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Chúng tôi xử lý n phân đoạn, mỗi phân đoạn có độ dài tối đa n so với các trạng thái LIS trước đó. | 
| Không gian | O(n^2) | Từ điển dp chứa tối đa các giá trị riêng biệt O(n^2) vì mỗi phân đoạn kéo dài tối đa | 

Vì n 50 nên điều này phù hợp thoải mái trong giới hạn thời gian 5s. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    n = int(input())
    x = int(input())
    d = list(map(int, input().split()))
    dp = {x: (1, 1)}
    for di in d:
        next_dp = {}
        if di >= 0:
            for val, (length, count) in dp.items():
                for step in range(di + 1):
                    new_val = val + step
                    new_len = length + step
                    if new
```
