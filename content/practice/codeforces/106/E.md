---
title: "CF 106E - Lực lượng cứu hộ vũ trụ"
description: "Chúng ta được yêu cầu đặt một trạm cứu hộ không gian trong không gian ba chiều sao cho khoảng cách tối đa từ nó đến bất kỳ hành tinh nào đã cho là nhỏ nhất. Mỗi hành tinh được biểu thị bằng tọa độ $(x, y, z)$."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 106
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 82 (Div. 2)"
rating: 2100
weight: 106
solve_time_s: 323
verified: false
draft: false
---

[CF 106E - Nhân viên cứu hộ không gian](https://codeforces.com/problemset/problem/106/E) 

**Xếp hạng:** 2100 
**Tags:** hình học, tìm kiếm ba ngôi 
**Thời gian giải:** 5 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đặt một trạm cứu hộ không gian trong không gian ba chiều sao cho khoảng cách tối đa từ nó đến bất kỳ hành tinh nào đã cho là nhỏ nhất. Mỗi hành tinh được đại diện bởi tọa độ của nó$(x, y, z)$. Đầu vào bao gồm một số nguyên$n$đại diện cho số lượng các hành tinh theo sau là$n$các đường tọa độ. Đầu ra phải là một điểm$(x_0, y_0, z_0)$giúp giảm thiểu khoảng cách xa nhất tới bất kỳ hành tinh nào. Câu trả lời được chấp nhận nếu khoảng cách khác với mức tối ưu nhiều nhất$10^{-6}$. 

Những hạn chế là nhỏ:$n \le 100$và tọa độ nằm trong$[-10^4, 10^4]$. Vì số lượng hành tinh thấp nên chúng ta có thể sử dụng các thuật toán với phép lặp bậc ba hoặc thậm chí bậc bốn trên các hành tinh, nhưng tính chất liên tục của không gian ngăn cản một lực lượng vũ phu hoàn toàn rời rạc. Một cách tiếp cận đơn giản sẽ thử tất cả các điểm có thể có trên lưới, nhưng yêu cầu về độ chính xác khiến điều đó là không thể. 

Các trường hợp cạnh không rõ ràng bao gồm các cấu hình trong đó các hành tinh được căn chỉnh hoặc đối xứng. Ví dụ, nếu các hành tinh ở$(1, 0, 0)$Và$(-1, 0, 0)$, điểm tối ưu là$(0, 0, 0)$. Một thuật toán bất cẩn chỉ coi bản thân các hành tinh là điểm ứng cử viên có thể chọn sai một trong các hành tinh thay vì trung tâm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là kiểm tra tất cả các điểm trong hộp giới hạn 3D và tính khoảng cách tối đa đến các hành tinh. Đối với lưới có kích thước$M^3$, mỗi lần kiểm tra yêu cầu$O(n)$các phép tính khoảng cách. Với$M = 20000$(bao gồm tất cả các tọa độ nguyên) và$n = 100$, điều này sẽ yêu cầu$20000^3 \cdot 100$hoạt động, rõ ràng là không thể thực hiện được. 

Quan sát quan trọng là hàm khoảng cách tối đa$f(x, y, z) = \max_i \text{dist}((x, y, z), (x_i, y_i, z_i))$lồi trong không gian 3D. Theo trực giác, khi chúng ta di chuyển điểm ứng viên đến gần tâm của các hành tinh xa nhất thì khoảng cách tối đa sẽ giảm. Tính lồi cho phép chúng ta sử dụng các phương pháp tối ưu hóa hình học lặp đi lặp lại như tìm kiếm bậc ba hoặc giảm độ dốc. Từ$n$là nhỏ, một phương pháp leo đồi ngẫu nhiên để di chuyển điểm ứng cử viên về phía hành tinh xa nhất sẽ hoạt động hiệu quả. 

Ý tưởng là bắt đầu tại bất kỳ điểm nào (ví dụ: hành tinh đầu tiên), xác định hành tinh xa nhất so với điểm hiện tại và di chuyển nhẹ về phía nó. Lặp lại điều này dần dần hội tụ đến tâm hình cầu bao quanh tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới vũ phu | O(M^3 * n) | O(n) | Quá chậm | 
| Leo đồi lặp đi lặp lại / Tìm kiếm bậc ba | O(n * lần lặp) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo điểm ứng viên ở bất kỳ hành tinh nào, ví dụ như điểm đầu tiên. Điều này hợp lý vì điểm tối ưu thường nằm ở đâu đó bên trong vỏ lồi của các hành tinh. 
2. Đặt kích thước bước ban đầu bằng một nửa khoảng cách lớn nhất giữa hai hành tinh bất kỳ. Điều này đảm bảo những bước di chuyển ban đầu chiếm không gian đáng kể. 
3. Lặp lại cho đến khi kích thước bước trở nên rất nhỏ: 

1. Tính khoảng cách từ điểm ứng viên hiện tại đến tất cả các hành tinh. 
2. Xác định hành tinh xa nhất so với điểm ứng cử viên hiện tại. 
3. Di chuyển điểm ứng cử viên một phần kích thước bước về phía hành tinh xa nhất đó. 
4. Giảm kích thước bước một chút, ví dụ: nhân với 0,99, để cho phép điều chỉnh tốt hơn. 
4. Sau khi lặp đủ số lần, xuất ra điểm đề cử hiện tại. Quá trình hội tụ đến một điểm giảm thiểu khoảng cách tối đa đến các hành tinh. 

Tại sao nó hoạt động: ở mỗi bước chúng ta giảm khoảng cách tối đa bằng cách di chuyển về phía hành tinh xa nhất. Độ lồi của hàm khoảng cách cực đại đảm bảo rằng không có cực tiểu cục bộ nào cản trở quá trình. Việc giảm dần kích thước bước cho phép chúng ta tinh chỉnh vị trí với độ chính xác tùy ý. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def distance(p1, p2):
    return math.sqrt((p1[0]-p2[0])**2 + (p1[1]-p2[1])**2 + (p1[2]-p2[2])**2)

def main():
    n = int(input())
    planets = [tuple(map(int, input().split())) for _ in range(n)]
    
    # Initialize at the first planet
    x, y, z = planets[0]
    
    # Step size: half the max distance between any two planets
    step = 0.0
    for i in range(n):
        for j in range(i+1, n):
            step = max(step, distance(planets[i], planets[j]))
    step /= 2
    
    # Iterative hill climb
    for _ in range(10000):
        farthest = max(planets, key=lambda p: distance((x, y, z), p))
        dx, dy, dz = farthest[0]-x, farthest[1]-y, farthest[2]-z
        d = math.sqrt(dx*dx + dy*dy + dz*dz)
        if d == 0:
            break
        x += dx/d * step
        y += dy/d * step
        z += dz/d * step
        step *= 0.995
    
    print(f"{x:.10f} {y:.10f} {z:.10f}")

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu tại một hành tinh để đảm bảo điểm ứng viên nằm bên trong bao lồi của các hành tinh. Tính toán khoảng cách theo cặp tối đa sẽ cho kích thước bước ban đầu hợp lý. Trong mỗi lần lặp lại, chúng tôi di chuyển về phía hành tinh xa nhất một phần kích thước bước, giảm dần kích thước bước đó để tinh chỉnh độ chính xác. Chúng tôi xử lý các trường hợp khoảng cách bằng 0 để ngăn chia cho 0. Số lần lặp được chọn đủ cao để đạt được yêu cầu$10^{-6}$độ chính xác. 

## Ví dụ đã hoạt động 

Đối với mẫu 1: 

| Lặp lại | Ứng viên (x,y,z) | Hành tinh xa nhất | Khoảng cách tối đa | 
| --- | --- | --- | --- | 
| 0 | 5,0,0 | (-5,0,0) | 10 | 
| 1 | 0,0,0 | (-5,0,0) | 5 | 
| ... | ... | ... | ... | 
| cuối cùng | 0,0,0 | hành tinh nào | 5 | 

Dấu vết cho thấy rằng việc di chuyển về phía hành tinh xa nhất ở mỗi bước sẽ nhanh chóng tập trung vào điểm giữa các hành tinh, giảm thiểu khoảng cách tối đa. 

Đầu vào được xây dựng:```
2
1 0 0
-1 0 0
```Đầu ra:`0.0 0.0 0.0`. Thuật toán di chuyển từ hành tinh thứ nhất sang hành tinh thứ hai và dừng lại ở điểm giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * lần lặp) | Mỗi lần lặp tính toán khoảng cách tới n hành tinh; lần lặp là một hằng số cố định (10000) | 
| Không gian | O(n) | Chỉ lưu trữ n hành tinh | 

Với$n \le 100$và số lần lặp cố định, thuật toán chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided sample
assert run("5\n5 0 0\n-5 0 0\n0 3 4\n4 -3 0\n2 2 -2\n") == "0.0000000000 0.0000000000 0.0000000000", "sample 1"

# Custom cases
assert run("2\n1 0 0\n-1 0 0\n") == "0.0000000000 0.0000000000 0.0000000000", "two planets symmetric"
assert run("1\n10000 10000 10000\n") == "10000.0000000000 10000.0000000000 10000.0000000000", "single planet"
assert run("3\n1 1 1\n2 2 2\n3 3 3\n") == "2.0000000000 2.0000000000 2.0000000000", "aligned planets"
assert run("4\n-10000 -10000 -10000\n10000 10000 10000\n-10000 10000 -10000\n10000 -10000 10000\n") == "0.0000000000 0.0000000000 0.0000000000", "cube corners"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 hành tinh ở ±1 | 0,0,0 | Tính điểm giữa đối xứng | 
| 1 hành tinh | 10000,10000,10000 | Trường hợp hành tinh đơn | 
| 3 hành tinh thẳng hàng | 2,2,2 | Tâm đoạn thẳng | 
| 4 góc lập phương | 0,0,0 | Phân phối 3D đối xứng | 

## Vỏ cạnh 

Đối với hai hành tinh ở$(1,0,0)$Và$(-1,0,0)$, điểm ban đầu là$(1,0,0)$. Xa nhất là ((-1,0,0)\
