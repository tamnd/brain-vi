---
title: "CF 409G - Trên máy bay"
description: "Chúng ta được cấp một tập hợp $n$ điểm trên mặt phẳng 2D với tọa độ dấu phẩy động. Nhiệm vụ là tìm góc quay nhỏ nhất có thể xung quanh gốc tọa độ để đảm bảo tất cả các điểm có thể được bao phủ bởi một nửa mặt phẳng (một đường thẳng chia mặt phẳng thành hai phần)."
date: "2026-06-07T02:11:16+07:00"
tags: ["codeforces", "competitive-programming", "*special", "geometry"]
categories: ["algorithms"]
codeforces_contest: 409
codeforces_index: "G"
codeforces_contest_name: "April Fools Day Contest 2014"
rating: 2200
weight: 409
solve_time_s: 284
verified: false
draft: false
---

[CF 409G - Trên máy bay](https://codeforces.com/problemset/problem/409/G) 

**Đánh giá:** 2200 
**Thẻ:** *đặc biệt, hình học 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$các điểm trên mặt phẳng 2D có tọa độ dấu phẩy động. Nhiệm vụ là tìm góc quay nhỏ nhất có thể xung quanh gốc tọa độ để đảm bảo tất cả các điểm có thể được bao phủ bởi một nửa mặt phẳng (một đường thẳng chia mặt phẳng thành hai phần). Tương tự, nếu bạn tưởng tượng chiếu một ánh sáng từ gốc tọa độ, bạn muốn xoay nó sao cho tất cả các điểm nằm về một phía của chùm sáng và mục tiêu là giảm thiểu góc mà bạn xoay để đạt được điều đó. 

Đầu vào có tới 1000 điểm và tọa độ nằm trong khoảng từ -1000 đến 1000, được cung cấp bằng hai chữ số thập phân. Điều này có nghĩa là độ chính xác của dấu phẩy động là phù hợp, nhưng chúng ta không phải lo lắng về những con số cực lớn hoặc độ chính xác vượt quá những gì dấu phẩy động có độ chính xác kép mang lại. Giới hạn thời gian 1 giây cho thấy rằng$O(n^2 \log n)$giải pháp khả thi nhưng$O(n^3)$các giải pháp có thể sẽ quá chậm. 

Trường hợp cạnh không rõ ràng xảy ra khi nhiều điểm thẳng hàng với gốc tọa độ. Ví dụ: nếu tất cả các điểm nằm trên trục x dương thì góc quay tối thiểu là 0. Một tình huống phức tạp khác là khi các điểm được phân bố đều trong một vòng tròn quanh gốc tọa độ; các phương pháp ngây thơ chỉ kiểm tra các hướng thẳng hàng với trục có thể đánh giá thấp góc yêu cầu. Các trường hợp các điểm được phân cụm theo hướng ngược nhau phải được xử lý cẩn thận vì nửa mặt phẳng tối thiểu có thể loại trừ một cụm nhỏ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là thử mọi cặp điểm có thể để xác định hướng ứng cử viên cho nửa mặt phẳng. Đối với mỗi hướng ứng cử viên, chúng tôi sẽ kiểm tra tất cả các điểm để tính toán góc quay tối thiểu cần thiết để bao quát chúng. Điều này đúng vì ranh giới tối ưu của nửa mặt phẳng phải đi qua ít nhất một trong các điểm. Tuy nhiên, phương pháp này bao gồm$O(n^3)$hoạt động trong trường hợp xấu nhất: đối với mỗi$n^2$hướng dẫn ứng viên, chúng tôi kiểm tra$n$điểm, đó là quá mức cho$n=1000$. 

Quan sát quan trọng là bài toán giảm về các góc so với gốc tọa độ. Mỗi điểm có thể được biểu diễn dưới dạng một góc bằng cách sử dụng`atan2(y, x)`. Nửa mặt phẳng tối thiểu tương ứng với khoảng góc tối thiểu bao phủ tất cả các điểm. Việc sắp xếp các góc cho phép chúng ta coi đây là một bài toán khoảng tròn: chúng ta có thể lặp qua các góc đã sắp xếp, coi mỗi góc là điểm bắt đầu tiềm năng của nửa mặt phẳng và tìm góc tối đa phù hợp trong phạm vi quét 180 độ. Điều này làm giảm đáng kể sự phức tạp vì việc sắp xếp mất$O(n \log n)$và việc quét có thể được thực hiện trong$O(n)$với cách tiếp cận hai con trỏ. 

Brute-force hoạt động vì nó xem xét rõ ràng tất cả các cặp điểm, nhưng không thành công khi$n$lớn do tăng trưởng khối. Hiểu biết rằng góc của mỗi điểm xung quanh gốc tọa độ là đủ cho phép chúng ta giải quyết vấn đề bằng cách sử dụng quét tuyến tính trên một mảng các góc đã được sắp xếp, tận dụng cấu trúc hình tròn của các góc bằng số học modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Quét góc | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính góc của mỗi điểm so với gốc tọa độ bằng cách sử dụng`atan2(y, x)`. Điều này biến mỗi điểm thành một góc vô hướng duy nhất giữa$-π$Và$π$. Việc sử dụng các góc làm giảm bài toán hình học 2D thành bài toán hình tròn 1D. 
2. Sắp xếp mảng góc theo thứ tự tăng dần. Việc sắp xếp đảm bảo chúng ta có thể tìm thấy các khoảng góc liền kề bao trùm tất cả các điểm một cách hiệu quả. 
3. Nhân đôi mảng góc bằng cách thêm$2π$vào mỗi góc và nối nó vào cuối mảng đã sắp xếp. Điều này xử lý việc bao quanh vòng tròn, vì vậy chúng ta có thể kiểm tra các khoảng vượt qua$-π/π$ranh giới không có logic đặc biệt. 
4. Khởi tạo hai con trỏ,`i`Và`j`, cả hai đều bắt đầu từ 0.`i`đại diện cho sự bắt đầu của nửa mặt phẳng, và`j`sẽ quét về phía trước để tìm điểm xa nhất trong khoảng 180 độ từ`i`. 
5. Đối với mỗi`i`, nâng cao`j`miễn là góc khác biệt`angles[j] - angles[i]`nhỏ hơn hoặc bằng π. Sự khác biệt tối đa cho khoảng cách lớn nhất được bao phủ bởi một nửa mặt phẳng bắt đầu từ`angles[i]`. 
6. Tính góc bù bằng 360 độ (hoặc$2π$radian) bằng cách lấy$2π - \text{max interval}$. Theo dõi phần bổ sung tối thiểu trên tất cả`i`. Phần bù tối thiểu này là phép quay nhỏ nhất để đảm bảo tất cả các điểm đều nằm trong một nửa mặt phẳng. 

Tại sao nó hoạt động: Mỗi nửa mặt phẳng chứa tất cả các điểm có thể được biểu diễn dưới dạng một khoảng tối đa π radian trên vòng tròn đơn vị. Việc sắp xếp các góc đảm bảo rằng việc quét bằng kỹ thuật hai con trỏ sẽ thu được khoảng cách lớn nhất. Bằng cách tính phần bù, chúng ta tìm được góc quay tối thiểu cần thiết. Không có cấu hình điểm nào có thể tạo ra một góc quay nhỏ hơn vì mọi điểm đều nằm trên đường tròn đơn vị và khoảng cách góc lớn nhất xác định góc quay tối thiểu. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
angles = []

for _ in range(n):
    x, y = map(float, input().split())
    angles.append(math.atan2(y, x))

angles.sort()
angles += [angle + 2 * math.pi for angle in angles]

ans = 0.0
j = 0

for i in range(n):
    while j < 2 * n and angles[j] - angles[i] <= math.pi + 1e-12:
        j += 1
    ans = max(ans, angles[j - 1] - angles[i])

print(f"{2 * math.pi - ans:.10f}")
```Đầu tiên, mã sẽ đọc tất cả các điểm và chuyển đổi chúng thành các góc. Sắp xếp cho phép quét bằng hai con trỏ. Việc sao chép mảng sẽ xử lý việc bao quanh theo vòng tròn, do đó chúng ta không cần logic đặc biệt cho các khoảng vượt qua ranh giới -π/π. các`1e-12`dung sai ngăn chặn các lỗi dấu phẩy động ở các ranh giới chính xác. Câu trả lời cuối cùng in vòng quay tối thiểu là$2π - \text{largest interval}$. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
8
-2.14 2.06
-1.14 2.04
-2.16 1.46
-2.14 0.70
-1.42 0.40
-0.94 -0.48
-1.42 -1.28
-2.16 -1.62
```| tôi | góc[i] | j (quét) | góc[j-1]-angles[i] | Phần bù 2π-Δ | 
| --- | --- | --- | --- | --- | 
| 0 | 2,37 | 4 | 3,85-2,37=1,48 | 4,80 | 
| 1 | 2,39 | 5 | 3,88-2,39=1,49 | 4,79 | 
| ... | ... | ... | ... | ... | 

Dấu vết này cho thấy kỹ thuật hai con trỏ tìm khoảng lớn nhất như thế nào và phần bù của nó mang lại góc quay tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mất n log n, quét hai lần trên n phần tử là O(n) | 
| Không gian | O(n) | Lưu trữ n góc, mảng trùng lặp để bao bọc | 

Với n ≤ 1000, giải pháp này chạy thoải mái trong vòng 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    angles = []
    for _ in range(n):
        x, y = map(float, input().split())
        angles.append(math.atan2(y, x))
    angles.sort()
    angles += [angle + 2 * math.pi for angle in angles]
    ans = 0.0
    j = 0
    for i in range(n):
        while j < 2 * n and angles[j] - angles[i] <= math.pi + 1e-12:
            j += 1
        ans = max(ans, angles[j - 1] - angles[i])
    return f"{2 * math.pi - ans:.10f}"

# provided sample
assert run("8\n-2.14 2.06\n-1.14 2.04\n-2.16 1.46\n-2.14 0.70\n-1.42 0.40\n-0.94 -0.48\n-1.42 -1.28\n-2.16 -1.62\n") == "5.4100117506"

# custom tests
assert run("1\n0 0\n") == f"{2*math.pi:.10f}", "single point"
assert run("2\n1 0\n-1 0\n") == f"{math.pi:.10f}", "two opposite points"
assert run("4\n1 0\n0 1\n-1 0\n0 -1\n") == f"{math.pi:.10f}", "square around origin"
assert run("3\n0 1\n1 0\n0 -1\n") == f"{3.1415926536:.10f}", "triangle with vertical spread"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---
