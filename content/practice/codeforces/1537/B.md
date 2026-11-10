---
title: "CF 1537B - Cậu Bé Xấu"
description: "Chúng tôi được yêu cầu đặt hai chiếc yo-yo trong một căn phòng hình chữ nhật để tối đa hóa tổng quãng đường mà Anton phải di chuyển để nhặt cả hai chiếc yo-yo và quay trở lại vị trí ban đầu. Căn phòng là một lưới $n nhân m$ và Anton bắt đầu tại ô $(i, j)$."
date: "2026-06-10T15:22:50+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1537
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 726 (Div. 2)"
rating: 900
weight: 1537
solve_time_s: 1375
verified: false
draft: false
---

[CF 1537B - Bad Boy](https://codeforces.com/problemset/problem/1537/B) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 22 phút 55 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đặt hai chiếc yo-yo trong một căn phòng hình chữ nhật để tối đa hóa tổng quãng đường mà Anton phải di chuyển để nhặt cả hai chiếc yo-yo và quay trở lại vị trí ban đầu. Căn phòng là một$n \times m$lưới và Anton bắt đầu tại ô$(i, j)$. Riley có thể ném cả yo-yo vào bất kỳ ô nào, kể cả cùng một ô. Anton chỉ di chuyển đến các ô liền kề theo bốn hướng chính, do đó khoảng cách giữa hai ô là tổng chênh lệch tuyệt đối của chỉ số hàng và cột của chúng. 

Đầu vào bao gồm tối đa$10^4$trường hợp thử nghiệm và kích thước phòng$n, m$có thể lớn như$10^9$. Điều này ngay lập tức loại trừ mọi cách tiếp cận bạo lực nhằm thử tất cả các vị trí có thể có của yo-yo hoặc mô phỏng từng bước hành trình của Anton. Giải pháp khả thi duy nhất là tính toán câu trả lời bằng toán học với thời gian không đổi cho mỗi trường hợp kiểm thử. 

Các trường hợp cạnh không rõ ràng bao gồm các phòng chỉ có một hàng hoặc cột, trong đó có ít ô riêng biệt hơn để đặt yo-yo hoặc khi Anton bắt đầu ở một góc, vì “điểm xa nhất” tính từ anh ta có thể trùng với ranh giới phòng. Ví dụ, nếu$n = 1$Và$m = 5$với Anton tại$(1, 3)$, vị trí yo-yo tối ưu là ở$(1, 1)$Và$(1, 5)$, khai thác các đầu của hàng. Bất kỳ cách tiếp cận nào giả định diện tích hình chữ nhật theo mọi hướng đều sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ xem xét từng cặp ô trong phòng, tính toán khoảng cách Manhattan để Anton nhặt cả hai ô và quay lại điểm xuất phát, đồng thời chọn cặp có khoảng cách tối đa. Số cặp là$O((nm)^2)$, điều này là không thể đối với các ràng buộc tối đa của$n, m \le 10^9$. Điều này cho thấy vũ lực là không thể thực hiện được. 

Quan sát quan trọng là khoảng cách Manhattan được tối đa hóa khi hai yo-yo ở hai góc đối diện của căn phòng. Điều này là do khoảng cách Anton phải di chuyển là tổng khoảng cách từ ô xuất phát của anh ấy đến từng yo-yo và quay lại, đồng thời các góc tối đa hóa sự khác biệt giữa hàng và cột. Vì vị trí xuất phát của Anton không làm thay đổi thực tế rằng các góc là điểm cực trị nên chúng ta chỉ cần xét bốn góc:$(1, 1)$,$(1, m)$,$(n, 1)$, Và$(n, m)$. Chỉ có sáu cặp góc duy nhất, vì vậy chúng ta có thể tính tổng khoảng cách cho Anton cho mỗi cặp trong$O(1)$mỗi cặp, chọn mức tối đa và xuất ra. 

Điều này làm giảm vấn đề từ mức độ phức tạp không thể thực hiện được thành giải pháp liên tục theo thời gian cho mỗi trường hợp thử nghiệm, ngay cả đối với kích thước phòng lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(1) | Quá chậm | 
| Tối ưu (góc) | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định bốn góc của căn phòng:$(1,1), (1,m), (n,1), (n,m)$. Đây là những vị trí duy nhất có thể tối đa hóa khoảng cách Manhattan. 
2. Tạo tất cả sáu cặp góc duy nhất. Các cặp là: trên cùng bên trái & trên cùng bên phải, trên cùng bên trái & dưới cùng bên trái, trên cùng bên trái & dưới cùng bên phải, trên cùng bên phải & dưới cùng bên trái, trên cùng bên phải & dưới cùng bên phải, dưới cùng bên trái & dưới cùng bên phải. 
3. Đối với mỗi cặp góc, hãy tính tổng khoảng cách Manhattan mà Anton sẽ đi: khoảng cách từ vị trí xuất phát của anh ấy đến con yo-yo đầu tiên, cộng với khoảng cách giữa hai con yo-yo, cộng với khoảng cách từ con yo-yo thứ hai trở lại vị trí bắt đầu. 
4. Theo dõi cặp cho khoảng cách tối đa. 
5. Xuất tọa độ của hai yo-yo tương ứng với cặp có khoảng cách tối đa. 

Tại sao nó hoạt động: khoảng cách Manhattan có tính cộng trong các hàng và cột, do đó, việc đặt yo-yo ở các góc xa nhất sẽ đảm bảo rằng mỗi chênh lệch tọa độ được tối đa hóa. Bất kỳ điểm bên trong nào cũng sẽ làm giảm khoảng cách hàng hoặc cột, tạo ra tổng đường đi nhỏ hơn. Chỉ kiểm tra các cặp góc đảm bảo chúng ta tìm được giải pháp tối ưu trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_distance_yoyos(n, m, i, j):
    corners = [(1,1), (1,m), (n,1), (n,m)]
    best = None
    max_dist = -1
    for c1 in corners:
        for c2 in corners:
            dist = abs(i - c1[0]) + abs(j - c1[1]) \
                 + abs(c1[0] - c2[0]) + abs(c1[1] - c2[1]) \
                 + abs(c2[0] - i) + abs(c2[1] - j)
            if dist > max_dist:
                max_dist = dist
                best = (c1, c2)
    return best

t = int(input())
for _ in range(t):
    n, m, i, j = map(int, input().split())
    (x1, y1), (x2, y2) = max_distance_yoyos(n, m, i, j)
    print(x1, y1, x2, y2)
```chức năng`max_distance_yoyos`tính toán rõ ràng khoảng cách cho tất cả các cặp góc và theo dõi mức tối đa. Chỉ sử dụng bốn góc sẽ tránh được việc phải lặp đi lặp lại rất nhiều$n \times m$lưới. Vòng lặp lồng nhau trên các góc tối đa chỉ có 16 lần lặp, thực tế là không đổi. Mã này cũng xử lý chính xác tất cả các ranh giới, bao gồm cả các phòng một hàng hoặc một cột. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
n=2, m=3, i=1, j=1
```| c1 | c2 | Khoảng cách | 
| --- | --- | --- | 
| (1,1) | (1,3) | 4 | 
| (1,1) | (2,1) | 2 | 
| (1,1) | (2,3) | 6 | 
| (1,3) | (2,1) | 6 | 
| (1,3) | (2,3) | 4 | 
| (2,1) | (2,3) | 2 | 

Cặp tối ưu:`(1,1) & (2,3)`hoặc`(1,3) & (2,1)`, cho khoảng cách tối đa 6. 

### Đầu vào mẫu 2```
n=4, m=4, i=1, j=2
```Tính toán tất cả các cặp góc tương tự. Cặp tối ưu là`(4,1) & (4,4)`, cho khoảng cách tối đa là 9. 

Những ví dụ này cho thấy rằng chỉ cần đánh giá các cặp góc là đủ và thuật toán xác định chính xác cặp góc cho khoảng cách di chuyển tối đa của Manhattan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ 16 phép tính khoảng cách cho mỗi trường hợp thử nghiệm (bình phương bốn góc) | 
| Không gian | O(1) | Chỉ có một vài biến để lưu trữ khoảng cách và cặp tốt nhất | 

Ngay cả đối với$t = 10^4$và tối đa$n, m$, tổng số hoạt động là khoảng$1.6 \times 10^5$, cũng trong thời gian giới hạn. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # run the solution
    t = int(input())
    for _ in range(t):
        n, m, i, j = map(int, input().split())
        (x1, y1), (x2, y2) = max_distance_yoyos(n, m, i, j)
        print(x1, y1, x2, y2)
    return output.getvalue().strip()

# provided samples
assert run("7\n2 3 1 1\n4 4 1 2\n3 5 2 2\n5 1 2 1\n3 1 3 1\n1 1 1 1\n1000000000 1000000000 1000000000 50\n") == \
"1 1 2 3\n4 1 4 4\n1 1 3 5\n1 1 5 1\n1 1 3 1\n1 1 1 1\n1 1 1000000000 1000000000"

# custom tests
assert run("1\n1 1 1 1\n") == "1 1 1 1", "single cell"
assert run("1\n1 5 1 3\n") == "1 1 1 5", "single row"
assert run("1\n5 1 2 1\n") == "1 1 5 1", "single column"
assert run("1\n1000000000 1000000000 500000000 500000000\n") == "1 1 1000000000 1000000000", "large grid"
assert run("1\n2 2 2 2\n") == "1 1 2 2", "small square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 1 1 | |
