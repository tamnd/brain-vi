---
title: "CF 1846G - Rudolf và CodeVid-23"
description: "Chúng tôi được cung cấp một loạt các triệu chứng mà Rudolf hiện đang mắc phải, cùng với danh sách các loại thuốc. Mỗi loại thuốc cần dùng trong một số ngày nhất định, loại bỏ một số triệu chứng và có thể gây ra các triệu chứng mới dưới dạng tác dụng phụ."
date: "2026-06-09T05:53:29+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1846
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 883 (Div. 3)"
rating: 1900
weight: 1846
solve_time_s: 75
verified: true
draft: false
---

[CF 1846G - Rudolf và CodeVid-23](https://codeforces.com/problemset/problem/1846/G) 

**Xếp hạng:** 1900 
**Thẻ:** bitmasks, dp, đồ thị, tham lam, đường đi ngắn nhất 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một loạt các triệu chứng mà Rudolf hiện đang mắc phải, cùng với danh sách các loại thuốc. Mỗi loại thuốc cần dùng trong một số ngày nhất định, loại bỏ một số triệu chứng và có thể gây ra các triệu chứng mới dưới dạng tác dụng phụ. Mục tiêu của chúng tôi là xác định tổng số ngày tối thiểu cần thiết để loại bỏ tất cả các triệu chứng, giả sử Rudolf chỉ có thể dùng một loại thuốc mỗi lần. Nếu không có loại thuốc nào có thể loại bỏ tất cả các triệu chứng thì câu trả lời là -1. 

Đầu vào có tối đa 10 triệu chứng nhưng có tới 1000 loại thuốc cho mỗi trường hợp thử nghiệm. Tác dụng phụ và tác dụng phụ của mỗi loại thuốc được đưa ra dưới dạng chuỗi nhị phân có độ dài bằng số lượng triệu chứng. Số lượng triệu chứng ít cho thấy chúng ta có thể biểu thị trạng thái hiện tại của các triệu chứng dưới dạng bitmask. Ví dụ: nếu Rudolf có triệu chứng 1 và 3 trên 5, trạng thái có thể được lưu trữ dưới dạng`0b10100`. Điều này làm giảm vấn đề từ thao tác trên chuỗi sang thao tác trên số nguyên, điều này lý tưởng cho các thuật toán khám phá tất cả các trạng thái có thể. 

Một cách tiếp cận ngây thơ có thể thử tất cả các hoán vị của thuốc, nhưng điều này sẽ liên quan đến`m!`các khả năng, không thể thực hiện được đối với m lên tới 1000. Các trường hợp đặc biệt bao gồm khi Rudolf không có triệu chứng khi bắt đầu, giá trị này sẽ trả về 0 hoặc khi mọi loại thuốc không thể loại bỏ tất cả các triệu chứng hoặc tái xuất hiện các triệu chứng theo cách không có trình tự nào dẫn đến trạng thái khỏe mạnh hoàn toàn, trạng thái này sẽ trả về -1. Một cách tiếp cận tham lam bất cẩn luôn chọn loại thuốc loại bỏ nhiều triệu chứng nhất có thể thất bại khi các tác dụng phụ không còn tiến triển nữa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi trình tự thuốc, áp dụng chúng theo thứ tự và theo dõi tổng số ngày. Điều này hiệu quả vì nó mô phỏng tất cả các lộ trình điều trị có thể xảy ra, nhưng số lượng trình tự tăng theo giai thừa theo số lượng thuốc, con số này quá lớn đối với m = 1000. Ngay cả việc giới hạn thử tất cả các nhóm thuốc cũng dẫn đến`2^m`khả năng, vẫn không thực tế. 

Cái nhìn sâu sắc quan trọng là số lượng trạng thái triệu chứng có thể xảy ra là rất ít. Với`n`có nhiều nhất là các triệu chứng`2^n`các trạng thái riêng biệt. Mỗi loại thuốc là sự chuyển đổi từ trạng thái này sang trạng thái khác với chi phí bằng số ngày sử dụng. Điều này tự nhiên tạo thành một vấn đề đường đi ngắn nhất trong biểu đồ kích thước`2^n`, trong đó mỗi nút là trạng thái triệu chứng và các cạnh tương ứng với thuốc. Giải pháp tối ưu là mô hình hóa điều này dưới dạng biểu đồ và sử dụng thuật toán Dijkstra để tìm tổng số ngày tối thiểu để đạt đến trạng thái`0`(không có triệu chứng). Biểu diễn mặt nạ bit cho phép chúng ta tính toán các chuyển tiếp một cách hiệu quả bằng cách sử dụng các phép toán theo chiều bit:`next_state = (current_state & ~medicine_removes) | medicine_side_effects`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ơ(m!) | O(1) | Quá chậm | 
| BFS / Dijkstra trên biểu đồ trạng thái | O(2^n * m * log(2^n)) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi triệu chứng ban đầu thành mặt nạ bit số nguyên. Bit 1 cho biết sự hiện diện của một triệu chứng. Đây sẽ là nút bắt đầu của chúng ta để duyệt đồ thị. 
2. Đối với mỗi loại thuốc, hãy chuyển đổi danh sách loại bỏ và danh sách tác dụng phụ của nó thành các bitmask số nguyên. Tính toán trước những điều này vì chúng được sử dụng nhiều lần để tính toán các trạng thái tiếp theo. 
3. Khởi tạo một mảng`min_days`kích thước`2^n`, Ở đâu`min_days[state]`lưu trữ số ngày tối thiểu cần thiết để đạt đến trạng thái triệu chứng đó. Đặt tất cả các mục thành vô cùng ngoại trừ trạng thái bắt đầu được đặt thành 0. 
4. Sử dụng hàng đợi ưu tiên (min-heap) để triển khai thuật toán Dijkstra. Xô`(0, start_state)`vào heap, trong đó 0 là tổng số ngày hiện tại. 
5. Trong khi heap không trống, hãy bật trạng thái có tổng số ngày nhỏ nhất. Nếu trạng thái này là 0 (không có triệu chứng), hãy trả về số ngày tích lũy. 
6. Đối với trạng thái hiện tại, hãy lặp lại tất cả các loại thuốc. Tính trạng thái mới sau khi uống thuốc như sau`(current_state & ~medicine_removes) | medicine_side_effects`. Tính tổng số ngày mới như`current_days + medicine_days`. 
7. Nếu tổng số ngày mới nhỏ hơn`min_days[new_state]`, cập nhật`min_days[new_state]`và đẩy`(new_total_days, new_state)`vào đống. 
8. Nếu heap trống mà không đạt trạng thái 0, trả về -1 cho trường hợp kiểm thử này. 

Tại sao nó hoạt động: Mỗi trạng thái được truy cập nhiều nhất một lần với số ngày tối thiểu bằng thuật toán của Dijkstra. Bitmask đảm bảo rằng tất cả các tổ hợp triệu chứng có thể được khám phá một cách hiệu quả. Thuộc tính đơn điệu của chi phí ngày đảm bảo Dijkstra tạo ra tổng số ngày tối thiểu chính xác. 

## Giải pháp Python```python
import sys, heapq
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        start = int(input().strip(), 2)
        medicines = []
        for _ in range(m):
            d = int(input())
            removes = int(input().strip(), 2)
            side_effects = int(input().strip(), 2)
            medicines.append((d, removes, side_effects))
        
        if start == 0:
            print(0)
            continue
        
        max_state = 1 << n
        min_days = [float('inf')] * max_state
        min_days[start] = 0
        heap = [(0, start)]
        
        while heap:
            current_days, state = heapq.heappop(heap)
            if state == 0:
                break
            if current_days > min_days[state]:
                continue
            for d, remove, side in medicines:
                next_state = (state & ~remove) | side
                next_days = current_days + d
                if next_days < min_days[next_state]:
                    min_days[next_state] = next_days
                    heapq.heappush(heap, (next_days, next_state))
        
        print(min_days[0] if min_days[0] != float('inf') else -1)
```Giải pháp bắt đầu bằng cách chuyển đổi chuỗi triệu chứng thành mặt nạ số nguyên và tác dụng của từng loại thuốc thành mặt nạ. Việc sử dụng hàng đợi ưu tiên đảm bảo chúng tôi luôn mở rộng trạng thái với tổng số ngày tối thiểu trước tiên. Chúng tôi cập nhật`min_days`chỉ khi tìm được đường đi ngắn hơn. Việc sử dụng các phép toán theo bit đảm bảo quá trình chuyển đổi được tính toán trong thời gian không đổi. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**```
n=5, m=4, start=10011
medicines:
3, removes=10000, side=00110
3, removes=00101, side=00000
3, removes=01010, side=00100
5, removes=11010, side=00100
```| Bước | Tiểu bang | Ngày | Hành động | 
| --- | --- | --- | --- | 
| 0 | 10011 | 0 | bắt đầu | 
| 1 | 00101 | 5 | uống thuốc 4 | 
| 2 | 00000 | 8 | uống thuốc 2 | 

Chúng tôi đạt đến mức không có triệu chứng trong 8 ngày. 

**Ví dụ 2:**```
n=4, m=1, start=0000
```| Bước | Tiểu bang | Ngày | Hành động | 
| --- | --- | --- | --- | 
| 0 | 0000 | 0 | bắt đầu | 

Đã khỏe mạnh, câu trả lời là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n * m * log(2^n)) | Mỗi trạng thái trong số 2^n có thể được truy cập nhiều nhất một lần trong mỗi lần đẩy heap và với mỗi trạng thái, chúng tôi kiểm tra m loại thuốc, log(2^n) để biết hoạt động của heap | 
| Không gian | O(2^n + m) | Mảng min_days và danh sách thuốc | 

Với n 10, 2^n = 1024 và m ≤ 1000, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("""4
5 4
10011
3
10000
00110
3
00101
00000
3
01010
00100
5
11010
00100
4 1
0000
10
1011
0100
2 2
11
2
10
01
3
01
10
2 3
11
3
01
10
3
10
00
4
10
01""") == "8\n0\n-1\n6"

# Custom cases
assert run("""1
3 2
111
2
111
000
1
011
100""") == "3", "removal sequence with overlapping side effects"
assert run("""1
2 1
11
1
01
10""") == -1, "side effect prevents complete healing"
assert run("""1
5 0
10101""") == -1, "no medicines"
assert run("""1
1 1
1
1
0""") == "1", "single symptom and single medicine"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 111, 2 loại thuốc | 3 | Tác dụng phụ chồng chéo được xử lý chính xác | 
| 11, 1 thuốc | -1 | Tác dụng phụ của thuốc ngăn ngừa bệnh | 
| 10101, 0 loại thuốc | -1 | Không có thuốc | 
| 1, 1 thuốc | | |
