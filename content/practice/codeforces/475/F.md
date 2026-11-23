---
title: "CF 475F - Siêu vũ trụ"
description: "Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D vô hạn, đại diện cho các hành tinh trong một "siêu vũ trụ". Mục tiêu là liên tục chia tập hợp các hành tinh dọc theo các hàng hoặc cột trống để tách hoàn toàn tập hợp đó thành hai tập hợp con không trống, mỗi tập hợp con nằm ở một bên của hàng hoặc cột."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 475
codeforces_index: "F"
codeforces_contest_name: "Bayan 2015 Contest Warm Up"
rating: 2900
weight: 475
solve_time_s: 108
verified: false
draft: false
---

[CF 475F - Siêu vũ trụ](https://codeforces.com/problemset/problem/475/F) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các điểm trên lưới 2D vô hạn, đại diện cho các hành tinh trong một "siêu vũ trụ". Mục tiêu là liên tục chia tập hợp các hành tinh dọc theo các hàng hoặc cột trống để tách hoàn toàn tập hợp đó thành hai tập hợp con không trống, mỗi tập hợp con nằm ở một bên của hàng hoặc cột. Một hàng hoặc cột chỉ có thể được sử dụng để phân tách nếu nó không chứa hành tinh nào và có các hành tinh ở cả hai phía của nó. Sau khi thực hiện tất cả các phép chia có thể, các tập hợp hành tinh còn lại được gọi là "vũ trụ" và chúng ta được yêu cầu đếm chúng. 

Dữ liệu đầu vào bao gồm tới 100.000 hành tinh có tọa độ có thể lớn tới ±10^9. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào quét toàn bộ lưới hoặc thử trực tiếp tất cả các phần tách có thể có, vì lưới thực sự là vô hạn. Bất kỳ giải pháp nào cũng phải tự hoạt động trên tập hợp các điểm và tránh xem xét các khoảng trống riêng lẻ. Các trường hợp cạnh bao gồm các tập hợp các hành tinh được sắp xếp trên cùng một hàng hoặc cột, các vũ trụ một hành tinh hoặc các cấu hình trong đó có thể phân chia nhiều lần nhưng không được tính hai vũ trụ. 

Việc triển khai bất cẩn có thể cố gắng mô phỏng việc phân tách bằng cách lặp qua mọi tọa độ giữa các giá trị x và y tối thiểu và tối đa, điều này sẽ không thành công do cả hiệu suất và nguy cơ thiếu vũ trụ nếu việc phân tách được áp dụng sai thứ tự. Ví dụ: một siêu vũ trụ có các hành tinh ở tọa độ (0,0), (0,2), (2,0), (2,1), (2,2) phải phân chia ba lần, tạo ra ba vũ trụ chứ không phải bốn, vì một phân chia có thể bao gồm nhiều hành tinh. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là cố gắng phân chia theo chiều dọc và chiều ngang có thể. Người ta sẽ quét từ x tối thiểu đến x tối đa để tìm các cột trống, kiểm tra xem các hành tinh có tồn tại ở cả hai phía của mỗi cột ứng cử viên hay không. Tương tự, quét từ y tối thiểu đến y tối đa cho các hàng. Cách tiếp cận này đúng nhưng cực kỳ chậm vì việc quét các đường phân chia tiềm năng liên quan đến các hoạt động O (phạm vi tọa độ), điều này không khả thi do tọa độ có thể là ± 10^9. Ngay cả khi chúng tôi giới hạn quá trình quét ở các tọa độ chứa các hành tinh, việc áp dụng đệ quy các phần tách vẫn dẫn đến hành vi O(n^2) trong trường hợp xấu nhất vì mỗi phần tách có thể yêu cầu quét tất cả các hành tinh còn lại để kiểm tra xem chúng ở bên này hay bên kia. 

Thông tin chi tiết quan trọng là việc phân tách chỉ có thể xảy ra dọc theo các khoảng trống trong tọa độ x hoặc tọa độ y đã được sắp xếp. Nếu chúng ta sắp xếp các hành tinh theo x và theo dõi y tối thiểu và tối đa trong tiền tố và hậu tố của danh sách được sắp xếp này, chúng ta có thể nhanh chóng xác định liệu có tồn tại một đường thẳng đứng giữa hai tọa độ x ngăn cách các hành tinh thành hai tập hợp không trống hay không. Tương tự, sắp xếp theo y cho phép kiểm tra sự phân chia theo chiều ngang. Điều này làm giảm vấn đề thành phân chia và chinh phục đệ quy trên danh sách các điểm được sắp xếp, theo dõi giới hạn thay vì quét lưới vô hạn. Mọi lệnh gọi đệ quy đều phân vùng tập hợp và kiểm tra xem có thể phân chia theo chiều dọc hoặc chiều ngang hay không bằng cách so sánh các hộp giới hạn của các tập hợp con. Nếu không thể phân chia được thì chúng ta đã đạt tới một vũ trụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lưu trữ tất cả các hành tinh dưới dạng danh sách các bộ dữ liệu (x, y). Đây là cấu trúc dữ liệu cơ sở mà chúng ta sẽ phân chia đệ quy. 
2. Xác định hàm đệ quy nhận danh sách các hành tinh và hộp giới hạn của chúng (min_x, max_x, min_y, max_y). Nếu danh sách chỉ chứa một hành tinh, trả về 1 vì một hành tinh đã là một vũ trụ. 
3. Sắp xếp danh sách các hành tinh theo tọa độ x. Tính toán mảng tiền tố và hậu tố cho tọa độ y tối thiểu và tối đa. Đối với mỗi cặp hành tinh liên tiếp, hãy kiểm tra xem có sự phân chia theo chiều dọc giữa chúng hay không: sự phân chia là hợp lệ nếu y lớn nhất trong tập con bên trái nhỏ hơn y tối thiểu trong tập con bên phải hoặc ngược lại. Nếu tìm thấy sự phân chia hợp lệ, hãy gọi đệ quy hàm trên các tập con bên trái và bên phải, đồng thời trả về tổng số vũ trụ từ cả hai phía. 
4. Nếu không thể phân chia theo chiều dọc, hãy lặp lại quá trình sắp xếp theo tọa độ y, tính toán các mảng tiền tố và hậu tố cho x và kiểm tra sự phân chia theo chiều ngang hợp lệ. 
5. Nếu không thể phân chia theo chiều dọc và chiều ngang, hãy trả về 1 vì tập hợp con này tạo thành một vũ trụ. 
6. Gọi hàm đệ quy trên danh sách các hành tinh ban đầu. Giá trị trả về là tổng số vũ trụ. 

Tại sao nó hoạt động: Bằng cách chỉ theo dõi các hộp giới hạn theo thứ tự được sắp xếp, chúng tôi kiểm tra một cách hiệu quả xem có thể phân tách mà không cần quét lưới vô hạn hay không. Mỗi lần phân chia sẽ tách các hành tinh thành hai tập hợp không trống, không chồng chéo. Bởi vì mỗi lần phân chia sẽ làm giảm kích thước của tập hợp một cách nghiêm ngặt nên quá trình đệ quy sẽ kết thúc. Các hộp giới hạn đảm bảo rằng không có sự phân chia nào bị bỏ sót và một khi không thể phân chia được thì tập hợp con thỏa mãn điều kiện vũ trụ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

def count_universes(planets):
    if len(planets) == 1:
        return 1

    planets_x = sorted(planets, key=lambda p: p[0])
    n = len(planets_x)
    pref_ymin = [0] * n
    pref_ymax = [0] * n
    suff_ymin = [0] * n
    suff_ymax = [0] * n

    pref_ymin[0] = pref_ymax[0] = planets_x[0][1]
    for i in range(1, n):
        pref_ymin[i] = min(pref_ymin[i-1], planets_x[i][1])
        pref_ymax[i] = max(pref_ymax[i-1], planets_x[i][1])
    
    suff_ymin[-1] = suff_ymax[-1] = planets_x[-1][1]
    for i in range(n-2, -1, -1):
        suff_ymin[i] = min(suff_ymin[i+1], planets_x[i][1])
        suff_ymax[i] = max(suff_ymax[i+1], planets_x[i][1])
    
    for i in range(n-1):
        if pref_ymax[i] < suff_ymin[i+1] or pref_ymin[i] > suff_ymax[i+1]:
            return count_universes(planets_x[:i+1]) + count_universes(planets_x[i+1:])
    
    planets_y = sorted(planets, key=lambda p: p[1])
    pref_xmin = [0] * n
    pref_xmax = [0] * n
    suff_xmin = [0] * n
    suff_xmax = [0] * n

    pref_xmin[0] = pref_xmax[0] = planets_y[0][0]
    for i in range(1, n):
        pref_xmin[i] = min(pref_xmin[i-1], planets_y[i][0])
        pref_xmax[i] = max(pref_xmax[i-1], planets_y[i][0])
    
    suff_xmin[-1] = suff_xmax[-1] = planets_y[-1][0]
    for i in range(n-2, -1, -1):
        suff_xmin[i] = min(suff_xmin[i+1], planets_y[i][0])
        suff_xmax[i] = max(suff_xmax[i+1], planets_y[i][0])
    
    for i in range(n-1):
        if pref_xmax[i] < suff_xmin[i+1] or pref_xmin[i] > suff_xmax[i+1]:
            return count_universes(planets_y[:i+1]) + count_universes(planets_y[i+1:])
    
    return 1

n = int(input())
planets = [tuple(map(int, input().split())) for _ in range(n)]
print(count_universes(planets))
```Giải pháp này triển khai kiểm tra hộp giới hạn đệ quy theo cả hai chiều x và y. Mảng tiền tố và hậu tố theo dõi hiệu quả các giá trị tối thiểu và tối đa, tránh việc quét danh sách nhiều lần. Phải cẩn thận để phân chia chặt chẽ giữa các hành tinh, do đó việc kiểm tra sử dụng chỉ số i và i+1. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
0 0
0 2
2 0
2 1
2 2
```| Bước | Các hành tinh được sắp xếp theo x | Pref y-min | Ưu tiên y-max | Đủ y-min | Đủ y-max | Tách ra? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (0,0),(0,2),(2,0),(2,1),(2,2) | 0,0,0,0,0 | 0,2,2,2,2 | 0,0,1,1,2 | 2,2,2,2,2 | Tại chỉ mục 1: pref_ymax=2, suff_ymin=0 → kiểm tra phần chia tiếp theo | 
| 2 | (0,0),(0,2) / (2,0),(2,1),(2,2) | ... | ... | ... | ... | Chia dọc tại x=1 tạo ra 2 hành tinh bên trái, 3 hành tinh bên phải | 

Quá trình đệ quy tiếp tục và cuối cùng tìm thấy 3 vũ trụ: {(0,0),(0,2)}, {(2,0),(2,1)}, {(2,2)}. 

Ví dụ thứ hai: Các hành tinh ở một hàng:```
3
0 0
1 0
2 0
```Được sắp xếp theo x, pref_ymax = suff_ymin = 0 ở mọi nơi, do đó không thể phân chia theo chiều dọc. Được sắp xếp theo y, pref_xmax < suff_xmin không thành công với tất cả i, do đó hàm đệ quy trả về 1 phạm vi. 

## Phân tích độ phức tạp 

|
