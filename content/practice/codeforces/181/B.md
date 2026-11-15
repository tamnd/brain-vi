---
title: "CF 181B - Số lượng bộ ba"
description: "Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng 2D. Nhiệm vụ là đếm xem có bao nhiêu bộ ba điểm không có thứ tự tạo thành một cấu hình trong đó một điểm nằm chính xác tại trung điểm của đoạn được tạo bởi hai điểm còn lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 181
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2012 - Round 2 (Unofficial Div. 2 Edition)"
rating: 1300
weight: 181
solve_time_s: 94
verified: true
draft: false
---

[CF 181B - Số lượng sinh ba](https://codeforces.com/problemset/problem/181/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, vũ lực 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm phân biệt trên mặt phẳng 2D. Nhiệm vụ là đếm xem có bao nhiêu bộ ba điểm không có thứ tự tạo thành một cấu hình trong đó một điểm nằm chính xác tại trung điểm của đoạn được tạo bởi hai điểm còn lại. 

Nếu chúng ta chọn ba điểm$A$,$B$, Và$C$, sau đó$B$là trung điểm của đoạn$AC$khi:$B = \left(\frac{x_A + x_C}{2}, \frac{y_A + y_C}{2}\right)$Vì các bộ ba không có thứ tự nên cặp$(A,B,C)$cũng giống như$(C,B,A)$. Chúng tôi chỉ muốn đếm mỗi cấu hình hình học hợp lệ một lần. 

Ràng buộc$n \le 3000$là chìa khóa của vấn đề. Một giải pháp bậc ba để kiểm tra trực tiếp từng bộ ba sẽ cần khoảng$3000^3 = 27$tỷ tờ séc, vượt xa giới hạn thời gian 2 giây. Các giải pháp bậc hai, khoảng 9 triệu phép tính, hoàn toàn hợp lý trong Python. 

Tọa độ nhỏ, giữa$-1000$Và$1000$, nhưng chỉ điều đó là không đủ để xây dựng một giải pháp dựa trên lưới nhỏ. Quan sát quan trọng mang tính hình học: nếu$B$là trung điểm của$AC$, thì tọa độ của$A$Và$C$quyết tâm$B$độc đáo. 

Một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Hãy xem xét đầu vào này:```
3
0 0
1 1
2 2
```Câu trả lời là:```
1
```Điểm$(1,1)$là trung điểm của$(0,0)$Và$(2,2)$. 

Bây giờ hãy xem xét:```
3
0 0
1 0
2 1
```Câu trả lời là:```
0
```Việc triển khai đơn giản chỉ kiểm tra xem giá trị trung bình có phải là số nguyên hay không có thể đếm sai giá trị này. Trung điểm của$(0,0)$Và$(2,1)$là$(1,0.5)$, không tồn tại giữa các điểm. 

Một vấn đề tế nhị khác là tính hai lần. Giả sử chúng ta có:```
3
0 0
1 1
2 2
```Nếu chúng ta lặp lại các cặp có thứ tự$(A,C)$, thì cả hai$(0,0)\rightarrow(2,2)$Và$(2,2)\rightarrow(0,0)$tạo ra cùng một điểm giữa. Một giải pháp bất cẩn sẽ trả về 2 thay vì 1. 

Một trường hợp quan trọng hơn là khi tọa độ trung điểm là phân số:```
4
0 0
1 1
1 0
2 1
```Trung điểm của$(0,0)$Và$(1,1)$là$(0.5,0.5)$, không hợp lệ. Trung điểm của$(1,0)$Và$(2,1)$là$(1.5,0.5)$, cũng không hợp lệ. Câu trả lời đúng là 0. Bất kỳ cách tiếp cận nào sử dụng số học dấu phẩy động đều có rủi ro về độ chính xác và độ phức tạp không cần thiết. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là vũ lực. Chúng ta có thể lặp qua từng ba điểm phân biệt và kiểm tra xem một trong số chúng có phải là trung điểm của hai điểm còn lại hay không. 

Đối với mỗi bộ ba$(A,B,C)$, chúng tôi xác minh:$2x_B = x_A + x_C \quad \text{and} \quad 2y_B = y_A + y_C$Điều này hoạt động vì tọa độ điểm giữa chính xác là mức trung bình của các điểm cuối. Cách tiếp cận này đúng, nhưng thời gian chạy thật tai hại. có$\binom{3000}{3}$gấp ba lần, tức là khoảng 4,5 tỷ kết hợp. Ngay cả một hệ số hằng số nhỏ cũng không thể cứu được điều này. 

Nút thắt đến từ việc chọn bộ ba một cách rõ ràng. Điều kiện trung điểm thực sự chỉ phụ thuộc vào một cặp điểm cuối. Một lần$A$Và$C$cố định thì điểm giữa được xác định ngay lập tức. 

Sự quan sát đó thay đổi hoàn toàn vấn đề. 

Thay vì chọn ba điểm, chúng ta chỉ chọn hai điểm$A$Và$C$. Trung điểm của chúng là:$\left(\frac{x_A+x_C}{2}, \frac{y_A+y_C}{2}\right)$Nếu điểm giữa đó tồn tại trong số các điểm đã cho thì chúng ta đã tìm được một bộ ba hợp lệ. 

Chúng ta có thể lưu trữ tất cả các điểm bên trong một tập hợp băm cho$O(1)$kiểm tra thành viên. Sau đó, chúng tôi lặp lại từng cặp điểm không có thứ tự. 

Đối với mỗi cặp: 

1. Kiểm tra xem tổng$x_A+x_C$Và$y_A+y_C$đều chẵn. 
2. Tính tọa độ trung điểm. 
3. Kiểm tra xem điểm giữa đó có tồn tại trong tập hợp hay không. 

Điều này làm giảm độ phức tạp$O(n^2)$, dễ dàng phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm và lưu chúng vào danh sách. 
2. Chèn mọi điểm vào tập băm. 

Bộ này cho phép tra cứu theo thời gian liên tục khi kiểm tra xem điểm giữa có tồn tại hay không. 
3. Khởi tạo câu trả lời thành 0. 
4. Lặp lại tất cả các cặp điểm không có thứ tự$(i,j)$Ở đâu$i < j$. 

Việc sử dụng các cặp không có thứ tự sẽ ngăn cản việc đếm hai lần. Cặp đôi$(A,C)$nên được xử lý chính xác một lần. 
5. Với cặp hiện tại, hãy tính:$s_x = x_i + x_j, \quad s_y = y_i + y_j$6. Nếu một trong hai$s_x$hoặc$s_y$kỳ quặc, bỏ qua cặp này. 

Điểm tọa độ nguyên không thể có điểm giữa có tọa độ phân số. 
7. Ngược lại tính điểm giữa:$m_x = \frac{s_x}{2}, \quad m_y = \frac{s_y}{2}$8. Kiểm tra xem$(m_x,m_y)$tồn tại trong tập hợp điểm. 

Nếu nó tồn tại, hãy tăng câu trả lời lên 1. 
9. In câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Với mỗi bộ ba hợp lệ, có chính xác một cặp điểm cuối không có thứ tự$(A,C)$. Điểm giữa của chúng được xác định duy nhất và thuật toán kiểm tra xem điểm giữa đó có tồn tại trong đầu vào hay không. 

Mỗi bộ ba hợp lệ được tính một lần vì phép lặp cặp sử dụng$i < j$. Thứ tự ngược lại$(C,A)$không bao giờ được xem xét lại. 

Không thể đếm bộ ba không hợp lệ vì điều kiện trung điểm được kiểm tra chính xác bằng số học số nguyên:$2m_x = x_A + x_C, \quad 2m_y = y_A + y_C$Điểm giữa chỉ được chấp nhận nếu nó khớp với điểm đầu vào thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    points = []
    point_set = set()
    
    for _ in range(n):
        x, y = map(int, input().split())
        points.append((x, y))
        point_set.add((x, y))
    
    ans = 0
    
    for i in range(n):
        x1, y1 = points[i]
        
        for j in range(i + 1, n):
            x2, y2 = points[j]
            
            sx = x1 + x2
            sy = y1 + y2
            
            if sx % 2 != 0 or sy % 2 != 0:
                continue
            
            mx = sx // 2
            my = sy // 2
            
            if (mx, my) in point_set:
                ans += 1
    
    print(ans)

solve()
```Phần đầu tiên đọc tất cả các điểm vào cả danh sách và tập hợp. Danh sách duy trì thứ tự lặp lại, trong khi tập hợp cung cấp khả năng kiểm tra tồn tại điểm giữa nhanh chóng. 

Các vòng lặp lồng nhau liệt kê từng cặp không có thứ tự chính xác một lần. sử dụng`j`từ`i + 1`tránh xử lý trùng lặp và đảm bảo tính chính xác. 

Việc kiểm tra tính chẵn lẻ là rất quan trọng:```
if sx % 2 != 0 or sy % 2 != 0:
    continue
```Nếu không có nó, phép chia số nguyên sẽ âm thầm làm tròn xuống và tạo ra các điểm giữa sai. 

Điểm giữa chỉ được tính bằng số học số nguyên. Số học dấu phẩy động là không cần thiết và sẽ gây ra những lo ngại về độ chính xác có thể tránh được. 

Kiểm tra tư cách thành viên cuối cùng:```
if (mx, my) in point_set:
```là cốt lõi hình học của giải pháp. Nếu điểm giữa tồn tại trong số các điểm đầu vào thì ba điểm đó tạo thành một bộ ba hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1
2 2
3 3
```| Cặp | Tổng tọa độ | Điểm giữa | Tồn tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| (1,1) và (2,2) | (3,3) | phân số | Không | 0 | 
| (1,1) và (3,3) | (4,4) | (2,2) | Có | 1 | 
| (2,2) và (3,3) | (5,5) | phân số | Không | 1 | 

Đầu ra:```
1
```Dấu vết này cho thấy tại sao tính chẵn lẻ lại quan trọng. Chỉ cặp thứ hai tạo ra tọa độ điểm giữa nguyên. 

### Ví dụ 2 

đầu vào:```
5
0 0
2 0
1 0
0 2
2 2
```| Cặp | Tổng tọa độ | Điểm giữa | Tồn tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| (0,0) và (2,0) | (2,0) | (1,0) | Có | 1 | 
| (0,0) và (1,0) | (1,0) | phân số | Không | 1 | 
| (0,0) và (0,2) | (0,2) | (0,1) | Không | 1 | 
| (0,0) và (2,2) | (2,2) | (1,1) | Không | 1 | 
| (2,0) và (0,2) | (2,2) | (1,1) | Không | 1 | 
| (0,2) và (2,2) | (2,4) | (1,2) | Không | 1 | 

Đầu ra:```
1
```Ví dụ này chứng tỏ rằng chỉ tọa độ điểm giữa là số nguyên là không đủ. Điểm giữa cũng phải tồn tại trong số các điểm đã cho. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi cặp điểm không có thứ tự được kiểm tra một lần | 
| Không gian |$O(n)$| Bộ băm lưu trữ tất cả các điểm | 

Với$n = 3000$, thuật toán thực hiện khoảng 4,5 triệu lượt kiểm tra cặp, có thể dễ dàng quản lý bằng Python trong thời gian giới hạn. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())

    points = []
    point_set = set()

    for _ in range(n):
        x, y = map(int, input().split())
        points.append((x, y))
        point_set.add((x, y))

    ans = 0

    for i in range(n):
        x1, y1 = points[i]

        for j in range(i + 1, n):
            x2, y2 = points[j]

            sx = x1 + x2
            sy = y1 + y2

            if sx % 2 != 0 or sy % 2 != 0:
                continue

            mx = sx // 2
            my = sy // 2

            if (mx, my) in point_set:
                ans += 1

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""3
1 1
2 2
3 3
""") == "1", "sample 1"

# minimum size, no valid midpoint
assert run(
"""3
0 0
1 0
0 1
""") == "0", "minimum invalid case"

# one valid midpoint
assert run(
"""3
0 0
2 0
1 0
""") == "1", "simple midpoint"

# multiple valid triplets
assert run(
"""5
0 0
2 0
1 0
0 2
2 2
""") == "1", "single valid triplet among many pairs"

# negative coordinates
assert run(
"""5
-2 -2
0 0
2 2
-2 2
2 -2
""") == "1", "handles negative coordinates"

# catches double counting bugs
assert run(
"""3
-1 -1
0 0
1 1
""") == "1", "unordered counting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba điểm không liên quan | 0 | Cấu hình tối thiểu không hợp lệ | 
| Trường hợp trung điểm ngang | 1 | Phát hiện điểm giữa cơ bản | 
| Hỗn hợp các cặp hợp lệ và không hợp lệ | 1 | Logic lọc đúng | 
| Tọa độ âm | 1 | Xử lý dấu tọa độ | 
| Điểm chéo đối xứng | 1 | Ngăn chặn việc đếm hai lần | 

## Vỏ cạnh 

Xét trường hợp trung điểm phân số:```
3
0 0
1 1
2 1
```Thuật toán kiểm tra cặp$(0,0)$Và$(1,1)$. Tổng tọa độ của chúng là$(1,1)$, cả hai đều kỳ lạ. Vì điểm giữa sẽ là$(0.5,0.5)$, cặp này bị bỏ qua ngay lập tức. 

Tiếp theo nó kiểm tra$(0,0)$Và$(2,1)$. Tổng số tiền là$(2,1)$. Tổng tọa độ y là số lẻ nên cặp này cũng bị bỏ qua. 

Cặp cuối cùng cũng tạo ra tọa độ điểm giữa phân số. Câu trả lời vẫn là 0. 

Bây giờ hãy xem xét kịch bản đếm kép:```
3
0 0
1 1
2 2
```Bộ ba hợp lệ duy nhất sử dụng điểm cuối$(0,0)$Và$(2,2)$. Thuật toán xử lý cặp này một lần vì nó chỉ xem xét các chỉ số có$i < j$. Cặp ngược lại không bao giờ xuất hiện nên câu trả lời chính xác là 1 thay vì 2. 

Cuối cùng, hãy xem xét trường hợp tọa độ điểm giữa là số nguyên nhưng thiếu điểm giữa:```
3
0 0
2 2
5 5
```Cặp đôi$(0,0)$Và$(2,2)$tạo ra điểm giữa$(1,1)$. Việc kiểm tra tính chẵn lẻ đã thành công nhưng việc tra cứu tập hợp không thành công vì$(1,1)$không có trong đầu vào. Thuật toán loại bỏ chính xác cặp này và đưa ra 0.
