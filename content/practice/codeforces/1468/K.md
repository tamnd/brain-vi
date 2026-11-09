---
title: "CF 1468K - Người máy"
description: "Chúng ta có một robot di chuyển trên lưới 2D vô hạn, bắt đầu từ điểm gốc (0, 0). Nó nhận được một chuỗi các lệnh di chuyển: 'L' cho trái, 'R' cho phải, 'U' cho lên và 'D' cho xuống."
date: "2026-06-11T01:30:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1468
codeforces_index: "K"
codeforces_contest_name: "2020-2021 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules)"
rating: 1600
weight: 1468
solve_time_s: 92
verified: true
draft: false
---

[CF 1468K - Người máy](https://codeforces.com/problemset/problem/1468/K) 

**Đánh giá:** 1600 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một robot di chuyển trên lưới 2D vô hạn, bắt đầu từ điểm gốc (0, 0). Nó nhận được một chuỗi các lệnh di chuyển: 'L' cho trái, 'R' cho phải, 'U' cho lên và 'D' cho xuống. Không có bất kỳ trở ngại nào, việc thực hiện tất cả các lệnh sẽ di chuyển robot đến một vị trí cuối cùng nào đó, vị trí này được đảm bảo không phải là điểm gốc. Nhiệm vụ của chúng ta là đặt một chướng ngại vật duy nhất lên một ô (không phải điểm gốc) để robot, tuân theo các lệnh tương tự, sẽ quay trở lại (0, 0). Nếu không có trở ngại nào tồn tại, chúng ta trả về (0, 0). 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi chuỗi lệnh có độ dài lên tới 5000 và tổng độ dài trong tất cả các trường hợp thử nghiệm nhiều nhất là 5000. Ràng buộc này rất quan trọng: nó cho chúng ta biết rằng chúng ta có thể cung cấp một thuật toán có độ phức tạp về thời gian gần như tuyến tính trong tổng số lệnh. Một giải pháp mạnh mẽ mô phỏng robot đối với mọi chướng ngại vật có thể có trên đường đi có thể trở thành phương trình bậc hai, điều này vẫn có thể chấp nhận được ở đây vì mỗi chuỗi đều nhỏ nhưng chúng ta có thể làm tốt hơn. 

Các trường hợp khó khăn có thể gặp phải một giải pháp đơn giản bao gồm các trình tự rất ngắn như "L" hoặc "R", trong đó chướng ngại vật phải cách chính xác một bước và các trình tự trong đó rô-bốt di chuyển theo vòng lặp hoặc quay lại cùng một ô vì có thể tồn tại nhiều chướng ngại vật ứng cử viên. Ngoài ra, chướng ngại vật chặn chuyển động chỉ nên ngăn chặn chuyển động đó chứ không dịch chuyển robot hoặc ảnh hưởng đến các chuyển động trong tương lai ngoài bước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: robot sẽ ghé thăm mỗi ô dọc theo đường đi của nó, mô phỏng việc đặt chướng ngại vật ở đó và sau đó thực hiện lại toàn bộ chuỗi lệnh. Nếu robot kết thúc ở điểm gốc với chướng ngại vật đó, chúng tôi đã tìm ra giải pháp. Điều này hoạt động vì đường dẫn của robot ngắn (tối đa 5000 lệnh) và số lượng ô được truy cập nhiều nhất là như nhau. Số lượng thao tác trong trường hợp xấu nhất sẽ là O(n²), trong đó n là độ dài của chuỗi lệnh, vì đối với mỗi ô trong số n ô ứng cử viên, chúng tôi mô phỏng n bước di chuyển. Với tổng độ dài của tất cả các trường hợp thử nghiệm ≤ 5000, điều này là đủ nhanh. 

Thông tin chi tiết quan trọng cần tối ưu hóa là chúng ta chỉ cần kiểm tra các ô mà robot thực sự truy cập. Các ô chưa bao giờ được truy cập không thể ảnh hưởng đến robot. Chúng ta có thể mô phỏng đường đi một lần và lưu trữ chuỗi vị trí. Sau đó, đối với mỗi vị trí đã ghé thăm, chúng tôi mô phỏng lại đường đi của robot, bỏ qua bước di chuyển sẽ bước vào chướng ngại vật dự kiến ​​đó. Nếu sau tất cả các bước di chuyển, robot quay trở lại điểm gốc thì chúng ta có giải pháp. Điều này làm giảm việc kiểm tra không cần thiết và đơn giản về mặt khái niệm trong khi vẫn tuyến tính trong thực tế đối với các ràng buộc đầu vào của chúng tôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận do hạn chế đầu vào nhỏ | 
| Tối ưu | O(n²) | O(n) | Được chấp nhận, lý do đơn giản hơn và tránh được các ô không thể | 

## Hướng dẫn thuật toán

1. Bắt đầu bằng cách đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, khởi tạo robot tại (0, 0) và một danh sách trống để ghi lại đường đi của tất cả các vị trí mà robot ghé thăm. 
2. Mô phỏng chuyển động của robot bằng lệnh. Đối với mỗi lần di chuyển, hãy cập nhật tọa độ hiện tại và thêm vị trí mới vào danh sách đường dẫn. 
3. Đối với mỗi vị trí trong danh sách đường đi, hãy coi đó là ứng cử viên để đặt chướng ngại vật. Bỏ qua gốc tọa độ vì chúng ta không thể đặt chướng ngại vật ở đó. 
4. Simulate the robot’s movement again for the entire command string, but whenever a move would result in entering the candidate obstacle cell, the robot stays in place instead of moving.
 5. Sau khi hoàn thành việc mô phỏng chướng ngại vật dự kiến ​​này, hãy kiểm tra tọa độ cuối cùng. Nếu robot kết thúc ở điểm gốc, hãy in chướng ngại vật này làm giải pháp và ngừng kiểm tra các ứng cử viên tiếp theo cho trường hợp thử nghiệm này. 
6. Nếu không có chướng ngại vật nào dẫn đến việc quay trở lại điểm gốc, thì xuất ra (0, 0) để cho biết không tồn tại giải pháp nào. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng chướng ngại vật chỉ ảnh hưởng đến một bước tại một thời điểm. Because we consider every cell the robot actually visits, we are guaranteed to find a cell that prevents the robot from reaching its original endpoint. Nếu việc chặn một ô khiến robot quay về (0, 0) thì thỏa mãn bài toán. Chúng tôi không bao giờ xem xét các ô không liên quan, vì vậy mô phỏng chỉ kiểm tra các ứng cử viên hợp lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    commands = input().strip()
    x, y = 0, 0
    path = []
    
    for c in commands:
        if c == 'L':
            x -= 1
        elif c == 'R':
            x += 1
        elif c == 'U':
            y += 1
        elif c == 'D':
            y -= 1
        path.append((x, y))
    
    found = False
    for ox, oy in path:
        if ox == 0 and oy == 0:
            continue
        rx, ry = 0, 0
        for c in commands:
            nx, ny = rx, ry
            if c == 'L':
                nx -= 1
            elif c == 'R':
                nx += 1
            elif c == 'U':
                ny += 1
            elif c == 'D':
                ny -= 1
            if nx == ox and ny == oy:
                continue
            rx, ry = nx, ny
        if rx == 0 and ry == 0:
            print(ox, oy)
            found = True
            break
    if not found:
        print(0, 0)
```Vòng lặp đầu tiên tính toán đường đi của robot. Chúng tôi lưu trữ từng ô được truy cập trong`path`để tránh xem xét các vị trí chướng ngại vật không thể. Trong vòng lặp thứ hai, chúng tôi mô phỏng lại, chỉ điều chỉnh bước di chuyển nếu nó chạm vào chướng ngại vật dự kiến. Lần kiểm tra cuối cùng đảm bảo robot quay về điểm xuất phát. Chúng tôi phá vỡ ngay sau khi tìm ra giải pháp, vì chỉ cần một giải pháp. 

## Ví dụ đã hoạt động 

**Ví dụ 1: "L"** 

| Bước | Lệnh | x | y | Đường dẫn | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | [] | 
| 1 | L | -1 | 0 | [(-1,0)] | 

Chướng ngại vật ứng cử viên (-1,0) ngăn cản việc di chuyển sang trái. Mô phỏng lại, robot giữ nguyên ở điểm (0,0) và kết thúc ở điểm gốc. Đầu ra:`-1 0`. 

**Ví dụ 2: "RUUDL"** 

| Bước | Lệnh | x | y | Đường dẫn | 
| --- | --- | --- | --- | --- | 
| 0 | bắt đầu | 0 | 0 | [] | 
| 1 | R | 1 | 0 | [(1,0)] | 
| 2 | Bạn | 1 | 1 | [(1,0),(1,1)] | 
| 3 | Bạn | 1 | 2 | [(1,0),(1,1),(1,2)] | 
| 4 | D | 1 | 1 | [(1,0),(1,1),(1,2),(1,1)] | 
| 5 | L | 0 | 1 | [(1,0),(1,1),(1,2),(1,1),(0,1)] | 

Chướng ngại vật ứng viên (1,2) ngăn cản việc bước vào (1,2). Mô phỏng lại, tọa độ cuối cùng là (0,0). Đầu ra:`1 2`. 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác chướng ngại vật buộc robot quay trở lại điểm xuất phát. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi ô được truy cập (tối đa n), chúng tôi mô phỏng toàn bộ chuỗi lệnh (độ dài n). Trường hợp xấu nhất n2 cho mỗi trường hợp thử nghiệm. Tổng của tất cả các lệnh ≤ 5000, do đó tổng số 25.000.000 thao tác. | 
| Không gian | O(n) | Chúng tôi lưu trữ đường dẫn của tất cả các ô đã truy cập, tối đa bằng độ dài của chuỗi lệnh. | 

Thuật toán phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste solution here
    t = int(input())
    for _ in range(t):
        commands = input().strip()
        x, y = 0, 0
        path = []
        for c in commands:
            if c == 'L':
                x -= 1
            elif c == 'R':
                x += 1
            elif c == 'U':
                y += 1
            elif c == 'D':
                y -= 1
            path.append((x, y))
        found = False
        for ox, oy in path:
            if ox == 0 and oy == 0:
                continue
            rx, ry = 0, 0
            for c in commands:
                nx, ny = rx, ry
                if c == 'L':
                    nx -= 1
                elif c == 'R':
                    nx += 1
                elif c == 'U':
                    ny += 1
                elif c == 'D':
                    ny -= 1
                if nx == ox and ny == oy:
                    continue
                rx, ry = nx, ny
            if rx == 0 and ry == 0:
                print(ox, oy)
                found = True
                break
        if not found:
            print(0, 0)
    return output.getvalue().strip()
```
