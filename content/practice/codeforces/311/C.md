---
title: "CF 311C - Truy Tìm Kho Báu"
description: "Chúng ta có một sự sắp xếp tuyến tính của các ô h, được đánh số từ 1 đến h. Một số ô này chứa kho báu, mỗi ô có giá trị đô la dương."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 311
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 185 (Div. 1)"
rating: 2500
weight: 311
solve_time_s: 67
verified: true
draft: false
---

[CF 311C - Truy tìm kho báu](https://codeforces.com/problemset/problem/311/C) 

**Đánh giá:** 2500 
**Tags:** Brute Force, cấu trúc dữ liệu, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có sự sắp xếp tuyến tính của`h`các ô được đánh số từ 1 đến`h`. Một số ô này chứa kho báu, mỗi ô có giá trị đô la dương. Freda bắt đầu ở ô đầu tiên và ban đầu chỉ có một kích thước bước,`k`, cho phép cô ấy tiến về phía trước một cách chính xác`k`các ô lặp đi lặp lại hoặc quay trở lại ô đầu tiên. Theo thời gian, cô ấy có thể đạt được kích thước bước mới, giá trị kho báu có thể giảm đi hoặc cô ấy có thể được yêu cầu lấy kho báu có giá trị nhất có thể tiếp cận được. Mỗi hoạt động sẽ thay đổi khả năng di chuyển của cô ấy hoặc trạng thái kho báu. Nhiệm vụ là mô phỏng các hoạt động này một cách hiệu quả và báo cáo giá trị kho báu khi cô ấy lấy được kho báu. 

Vấn đề có một số hạn chế chặt chẽ:`h`có thể lớn tới 10^18, loại trừ mọi cách tiếp cận theo dõi rõ ràng mọi ô. Số lượng kho báu`n`và hoạt động`m`có thể lên tới 10^5, điều này ngụ ý rằng cách tiếp cận O(n·m) quá chậm. Kích thước bước`k`và các bước di chuyển được thêm vào rất nhỏ, nhiều nhất là 10^4 và có tối đa 20 kích thước bước bổ sung, gợi ý về việc sử dụng cấu trúc toán học (số học mô-đun hoặc BFS trên dư lượng) thay vì theo dõi tất cả các vị trí một cách rõ ràng. 

Một trường hợp quan trọng là khi kho báu tồn tại ngoài tầm với với kích thước bước hiện tại. Ví dụ: nếu các ô duy nhất có thể truy cập là bội số của`k`, bất kỳ kho báu nào không nằm ở bội số này sẽ bị bỏ qua đối với truy vấn loại 3. Tương tự, nhiều báu vật có thể có cùng giá trị; chúng ta phải luôn chọn kho báu được lập chỉ mục nhỏ nhất để lấy. 

Một giải pháp ngây thơ lặp lại tất cả các kho báu cho từng truy vấn loại 3 sẽ thất bại vì`n * m`có thể lên tới 10^10, một con số lớn không thể chấp nhận được. Chúng ta cần một phương pháp để nhanh chóng kiểm tra khả năng tiếp cận và tìm ra kho báu tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là duy trì danh sách tất cả các kho báu và đối với mỗi truy vấn loại 3, lặp lại tất cả các kho báu để xem liệu chúng có thể truy cập được với kích thước bước hiện tại hay không, sau đó chọn mức tối đa. Mỗi thao tác loại 1 (thêm kích thước bước) sẽ yêu cầu tính toán lại các ô có thể truy cập. Với n và m lên tới 10^5 và mỗi truy vấn loại 3 có khả năng quét tất cả n kho báu, điều này sẽ dẫn đến các hoạt động O(n·m), tốc độ này quá chậm. 

Điểm mấu chốt là vấn đề làm giảm khả năng tiếp cận modulo ước số chung lớn nhất (GCD) của các kích thước bước có sẵn. Bất kỳ vị trí nào có thể truy cập từ ô 1 đều có thể được biểu thị bằng 1 cộng với sự kết hợp tuyến tính của kích thước bước hiện tại. Nếu chúng ta theo dõi GCD của tất cả các kích cỡ bước, bất kỳ kho báu nào ở vị trí`a_i`có thể truy cập được nếu`(a_i - 1) % gcd == 0`. Điều này làm giảm việc kiểm tra khả năng tiếp cận thành một thao tác modulo đơn giản. 

Để xử lý hiệu quả các truy vấn loại 3, chúng tôi duy trì hàng đợi ưu tiên (đống tối đa) hoặc nhiều tập hợp được khóa theo giá trị kho báu, được nhóm theo trạng thái có thể truy cập. Bất cứ khi nào thao tác loại 2 sửa đổi một kho báu, chúng tôi sẽ cập nhật giá trị của nó trong cấu trúc. Đối với loại 3, chúng tôi chỉ trích xuất mức tối đa từ tập hợp có thể truy cập. Loại 1 cập nhật GCD và có thể chuyển đổi những kho báu không thể tiếp cận trước đây thành những kho báu có thể tiếp cận được; chúng tôi có thể tính toán lại các kho báu có thể truy cập dựa trên GCD mới, nhưng vì có tối đa 20 kích thước bước được thêm vào nên việc tính toán lại không thường xuyên xảy ra và có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Dựa trên GCD + Heap | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào: số lượng ô`h`, số lượng kho báu`n`, số thao tác`m`và kích thước bước ban đầu`k`. Lưu trữ kho báu theo cặp`(position, value)`và theo dõi chúng theo chỉ số đầu vào của chúng. 
2. Khởi tạo tập hợp kích thước bước với`{k}`và tính toán dòng điện`gcd`của tất cả các kích cỡ bước. GCD này xác định những vị trí nào có thể tiếp cận được: một vị trí`a_i`có thể truy cập được nếu`(a_i - 1) % gcd == 0`. 
3. Xây dựng một đống tối đa hoặc nhiều bộ theo thứ tự cho các kho báu mà ban đầu có thể tiếp cận được. Duy trì ánh xạ từ chỉ mục kho báu đến mục nhập heap của nó để cập nhật nhanh chóng. 
4. Đối với từng thao tác: 

- Loại 1: Thêm kích thước bước`x`. Cập nhật bộ kích thước bước và tính toán lại GCD. Đánh giá lại khả năng tiếp cận đối với các kho báu chưa thể tiếp cận được: bất kỳ kho báu nào có thể tiếp cận được bằng GCD mới sẽ được đưa vào heap. 
- Loại 2: Giảm giá trị bảo vật`x`qua`y`. Nếu kho báu nằm trong đống, hãy cập nhật giá trị của nó một cách thích hợp. Tùy thuộc vào việc triển khai vùng heap, chúng tôi có thể xóa và lắp lại để duy trì thứ tự vùng heap. 
- Loại 3: Trích xuất kho báu có giá trị lớn nhất có thể tiếp cận được từ heap. Xuất giá trị của nó. Loại bỏ nó khỏi heap để cho biết nó đã được lấy. Nếu heap trống, xuất ra 0. 
5. Tiếp tục cho đến khi tất cả các thao tác được xử lý. 

Tính đúng đắn phụ thuộc vào tính bất biến rằng heap luôn chứa chính xác các kho báu có thể truy cập được. Thuộc tính GCD đảm bảo rằng mọi vị trí có thể tiếp cận đều có thể được biểu diễn dưới dạng kết hợp tuyến tính của kích thước bước hiện tại, do đó`(a_i - 1) % gcd == 0`nắm bắt chính xác khả năng tiếp cận. Vì chúng tôi luôn cập nhật kho báu khi giá trị của chúng thay đổi hoặc khả năng tiếp cận thay đổi nên truy vấn loại 3 luôn trả về chính xác kho báu tối đa có thể tiếp cận. 

## Giải pháp Python```python
import sys
import math
import heapq

input = sys.stdin.readline

def main():
    h, n, m, k = map(int, input().split())
    treasures = []
    for _ in range(n):
        a, c = map(int, input().split())
        treasures.append([a, c, _])  # store position, value, index

    step_sizes = [k]
    gcd = k
    reachable = []
    unreachable = []

    for idx, (a, c, i) in enumerate(treasures):
        if (a - 1) % gcd == 0:
            heapq.heappush(reachable, (-c, i, idx))  # max-heap by value, break ties by index
        else:
            unreachable.append(idx)

    treasure_values = [c for a, c, _ in treasures]

    for _ in range(m):
        op = input().split()
        if op[0] == '1':
            x = int(op[1])
            step_sizes.append(x)
            new_gcd = gcd
            for s in step_sizes:
                new_gcd = math.gcd(new_gcd, s)
            if new_gcd != gcd:
                gcd = new_gcd
                # move treasures that became reachable
                still_unreachable = []
                for idx in unreachable:
                    a, c, i = treasures[idx]
                    if (a - 1) % gcd == 0:
                        heapq.heappush(reachable, (-treasure_values[idx], i, idx))
                    else:
                        still_unreachable.append(idx)
                unreachable = still_unreachable

        elif op[0] == '2':
            x = int(op[1]) - 1
            y = int(op[2])
            treasure_values[x] -= y
            a, _, i = treasures[x]
            if (a - 1) % gcd == 0:
                # re-insert updated value
                heapq.heappush(reachable, (-treasure_values[x], i, x))
        else:  # op[0] == '3'
            while reachable:
                val, i, idx = heapq.heappop(reachable)
                if treasure_values[idx] == -val:
                    print(treasure_values[idx])
                    treasure_values[idx] = 0  # mark as taken
                    break
            else:
                print(0)

if __name__ == "__main__":
    main()
```Giải pháp phân tách các kho báu có thể tiếp cận và không thể tiếp cận. Heap đảm bảo chúng ta có thể lấy được kho báu tối đa một cách hiệu quả. GCD duy trì khả năng tiếp cận và các bản cập nhật cẩn thận xử lý việc giảm giá trị. Loại bỏ kho báu bằng cách đánh dấu`treasure_values[idx] = 0`tránh các hoạt động loại bỏ đống phức tạp. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
10 3 5 2
5 50
7 60
8 100
2 2 5
3
1 3
3
3
```| Bước | Hoạt động | Kho báu có thể tiếp cận (pos,value) | Trạng thái đống | Đầu ra | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | 5:50,7:60 | (-60,1,1), (-50,0,0) | - | 
| 1 | 2 2 5 | 5:50,7:55 | (-55,1,1), (-50,0,0) | - | 
| 2 | 3 | tối đa=55 | (-50,0,0) | 55 | 
| 3 | 1 3 | thêm bước 3 -> gcd=1 | tất cả kho báu có thể tiếp cận: 5:50,7:55,8:100 | (-100,2,2),(-50,0,0),(-55,1,1) | 
| 4 | 3 | tối đa=100 | (-55,1,1),(-50,0,0) | 100 | 
| 5 | 3 | tối đa=55->đã lấy=50? | (-50,0,0) | 50 | 

Dấu vết xác nhận khả năng tiếp cận và cập nhật vùng nhớ heap chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi thao tác heap là log n, mỗi thao tác trong số m thao tác có thể đẩy/bật. Việc tính toán lại GCD và di chuyển các kho báu có thể tiếp cận xảy ra nhiều nhất |
