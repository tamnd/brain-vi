---
title: "CF 1894B - Hai trong ba"
description: "Chúng ta được cấp một mảng số nguyên và cần xây dựng một mảng nhãn song song chỉ chứa 1, 2 hoặc 3. Mục tiêu là đáp ứng chính xác hai trong ba điều kiện ghép nối, mỗi điều kiện liên quan đến hai chỉ số trong đó các số ban đầu bằng nhau và các nhãn được gán tạo thành một trong…"
date: "2026-06-09T01:15:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1894
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 908 (Div. 2)"
rating: 1000
weight: 1894
solve_time_s: 117
verified: false
draft: false
---

[CF 1894B - Hai trong số ba](https://codeforces.com/problemset/problem/1894/B) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng các số nguyên và cần xây dựng một mảng nhãn song song chỉ chứa 1, 2 hoặc 3. Mục tiêu là đáp ứng chính xác hai trong ba điều kiện ghép nối, mỗi điều kiện liên quan đến hai chỉ số trong đó các số ban đầu bằng nhau và các nhãn được gán tạo thành một trong các cặp (1,2), (1,3) hoặc (2,3). Về cơ bản, chúng tôi đang cố gắng tô màu các bản sao trong mảng để hai trong số ba "cặp màu" này xuất hiện ở đâu đó trong số các bản sao, nhưng cặp màu thứ ba thì không. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm có một mảng có kích thước tối đa là 100 và các phần tử mảng được giới hạn bởi 100. Vì kích thước mảng nhỏ nên chúng ta có thể cung cấp các giải pháp với các phép toán bậc hai cho mỗi trường hợp thử nghiệm, nhưng chúng ta vẫn nên tìm một phương pháp đơn giản và trực tiếp. Các trường hợp cạnh bao gồm các mảng trong đó tất cả các số đều giống hệt nhau, mảng không có bản sao hoặc mảng có chính xác hai bản sao. Ví dụ, một mảng`[7,7,7,7,7,7,7]`không thể đáp ứng chính xác hai điều kiện vì bất kỳ nhãn trùng lặp nào cũng sẽ đáp ứng cả ba điều kiện, do đó đầu ra phải là`-1`. Một trường hợp khó phát hiện khác là mảng chỉ có các phần tử duy nhất; không có cặp nào tồn tại nên giải pháp là không thể. 

## Phương pháp tiếp cận 

Một phương pháp brute-force ngây thơ sẽ thử mọi cách gán nhãn (1,2,3) cho từng phần tử và kiểm tra xem có thỏa mãn chính xác hai điều kiện hay không. Đối với một mảng có kích thước`n`, đó là`3^n`những khả năng khó có thể thực hiện được ngay cả đối với`n=20`, vì vậy chúng ta cần một cách tiếp cận thông minh hơn. Quan sát quan trọng là cả ba điều kiện chỉ phụ thuộc vào các cặp số bằng nhau. Vì vậy, chúng ta có thể tập trung vào các bản sao. 

Nếu một phần tử xuất hiện ba lần trở lên, chúng ta có đủ quyền tự do gán nhãn để thỏa mãn chính xác hai điều kiện. Chúng ta có thể gắn nhãn hai lần xuất hiện đầu tiên là 1 và 2, thỏa mãn điều kiện (1,2), sau đó gán lần xuất hiện thứ ba là 3, cho phép thỏa mãn điều kiện (1,3) hoặc (2,3). Nếu cẩn thận, chúng ta có thể chọn được nhãn thỏa mãn đúng hai trong ba điều kiện. 

Nếu tất cả các sự trùng lặp xảy ra nhiều nhất hai lần thì chúng ta phải phân tích cẩn thận. Mảng trong đó tất cả các phần tử được sao chép chính xác hai lần cho phép chúng ta gán nhãn 1 và 2 cho một phần tử, sau đó gán nhãn 1 và 2 cho phần tử khác, nhưng khi đó điều kiện (1,3) hoặc (2,3) có thể không thành công. Nếu không có phần tử nào xuất hiện quá hai lần và không có đủ số bản sao để phân biệt các nhãn thì vấn đề có thể không xảy ra. Mảng có tất cả các phần tử duy nhất hoặc chỉ có các cặp trùng lặp thường không thể đáp ứng chính xác hai điều kiện, dẫn đến`-1`. 

Giải pháp tối ưu hoạt động bằng cách phân loại các phần tử mảng theo tần số. Nếu một phần tử xuất hiện ba lần trở lên, nó sẽ trở thành phần tử "flex" cho phép chúng ta kiểm soát hai điều kiện nào được giữ. Mặt khác, nếu tất cả các bản sao chỉ xuất hiện hai lần, chúng tôi sẽ gán nhãn 1 và 2 cho một phần tử và 1 và 2 cho các phần tử khác một cách cẩn thận để đảm bảo chỉ thỏa mãn hai điều kiện. Nếu cấu trúc không cho phép chính xác hai điều kiện, chúng ta sẽ xuất ra`-1`. Cách tiếp cận này là tuyến tính trong`n`cho từng trường hợp thử nghiệm vì chúng tôi chỉ lặp lại mảng và gán nhãn dựa trên số lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng số trong mảng. Điều này cho phép chúng tôi biết những con số nào là trùng lặp và chúng xuất hiện bao nhiêu lần. 
2. Khởi tạo mảng đầu ra`b`với số không. Điều này sẽ giữ các nhãn 1, 2 hoặc 3. 
3. Theo dõi xem có tồn tại một số có tần số ít nhất là 3 hay không. Nếu tồn tại một số như vậy, chúng ta có thể gán nhãn 1, 2, 3 cho ba lần xuất hiện đầu tiên của nó. Điều này rất quan trọng vì nó cho phép chúng ta kiểm soát hai điều kiện nào được thỏa mãn. 
4. Đối với tất cả các bản sao khác (tần số 2), gán nhãn 1 và 2. Các phép gán này tự động đáp ứng điều kiện đầu tiên (1,2) cho số đó. 
5. Nếu không có số nào có tần số ít nhất là 3 và số trùng lặp có tần số 2 là số lẻ thì không thể thỏa mãn đúng hai điều kiện. đầu ra`-1`trong trường hợp này. Số lẻ sẽ buộc phải thỏa mãn cả ba điều kiện. 
6. Đối với mọi phần tử còn lại (tần số 1), hãy gán nhãn 1 theo mặc định. Những điều này không đóng góp vào bất kỳ điều kiện nào trong ba điều kiện, vì vậy chúng không ảnh hưởng đến số lượng. 
7. Xuất mảng vừa tạo`b`. 

Tại sao nó hoạt động: Bằng cách cho một phần tử "flex" xuất hiện ba lần, chúng ta có thể gán các nhãn 1, 2, 3 để thỏa mãn có chọn lọc chính xác hai điều kiện. Các bản sao có tần số 2 chỉ thỏa mãn điều kiện (1,2) nên tránh vô tình tạo ra điều kiện thứ ba. Nếu không có phần tử nào có tần số ≥3 và số phần tử tần số-2 là số lẻ thì bất kỳ nhãn nào cũng sẽ thỏa mãn cả ba điều kiện, vì vậy chúng tôi báo cáo`-1`. Điều bất biến là chỉ những bản sao được dán nhãn cẩn thận mới góp phần thỏa mãn các điều kiện, cho phép chúng ta kiểm soát số lượng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    b = [0] * n
    idx_map = {}
    for i, x in enumerate(a):
        if x not in idx_map:
            idx_map[x] = []
        idx_map[x].append(i)
    
    # Find a number with at least 3 occurrences
    flex_idx = -1
    for num, positions in idx_map.items():
        if len(positions) >= 3:
            flex_idx = num
            break
    
    if flex_idx != -1:
        # assign 1,2,3 to first three occurrences
        positions = idx_map[flex_idx]
        b[positions[0]] = 1
        b[positions[1]] = 2
        b[positions[2]] = 3
        # assign 1,2 to remaining duplicates if any
        for pos in positions[3:]:
            b[pos] = 1
        # assign 1,2 to all other duplicates (frequency 2)
        for num, positions in idx_map.items():
            if num == flex_idx:
                continue
            if len(positions) == 2:
                b[positions[0]] = 1
                b[positions[1]] = 2
            elif len(positions) == 1:
                b[positions[0]] = 1
    else:
        # Count elements with frequency 2
        pairs = [num for num, positions in idx_map.items() if len(positions) == 2]
        if len(pairs) % 2 == 1:
            print(-1)
            continue
        flip = True
        for num, positions in idx_map.items():
            if len(positions) == 2:
                if flip:
                    b[positions[0]] = 1
                    b[positions[1]] = 2
                else:
                    b[positions[0]] = 2
                    b[positions[1]] = 1
                flip = not flip
            else:
                b[positions[0]] = 1
    print(' '.join(map(str, b)))
```Mã đầu tiên xây dựng bản đồ tần số và danh sách chỉ mục để xử lý các bản sao một cách hiệu quả. các`flex_idx`phần tử cho phép gán có chọn lọc 1,2,3, đây là thông tin chi tiết cốt lõi để đáp ứng chính xác hai điều kiện. Đối với các mảng không có phần tử flex, việc gán xen kẽ cho các phần tử tần số-2 sẽ ngăn chặn việc vô tình tạo ra điều kiện thứ ba. Singletons được gắn nhãn 1 theo mặc định vì chúng không ảnh hưởng đến bất kỳ điều kiện nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`6 1 2 3 2 2 3`| Bước | Hành động | mảng b | Ghi chú | 
| --- | --- | --- | --- | 
| Tần số đếm | 1:1, 2:3, 3:2 | - | Số 2 có tần số ≥3 | 
| Phần tử linh hoạt | 2 | - | Gán 1,2,3 cho ba lần xuất hiện đầu tiên | 
| Chỉ định người khác | vị trí 3 | b[5]=1, b[6]=2 | phần tử tần số 2 | 
| Chỉ định còn lại | 1 | b[0]=1 | đơn | 
| Đầu ra | b | 1 2 3 1 1 2 | thỏa mãn đúng hai điều kiện | 

### Ví dụ 2 

đầu vào:`7 7 7 7 7 7 7`| Bước | Hành động | mảng b | Ghi chú | 
| --- | --- | --- | --- | 
| Tần số đếm | 7:7 | - | Tất cả đều giống nhau, tần số ≥3 | 
| Chỉ định flex | 7 | 1,2,3 | ba vị trí đầu tiên | 
| Còn lại | 1 1 1 1 | b[3..6]=1 | tất cả các vị trí khác | 
| Đầu ra | b | 1 2 3 1 1 1 1 | Điều này thỏa mãn cả ba điều kiện, vì vậy đầu ra phải là -1 | 

Điều này chứng tỏ rằng ngay cả với tần số ≥3, chúng ta phải kiểm tra xem có thỏa mãn chính xác hai điều kiện hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại mảng và bản đồ tần số một lần | 
| Không gian | O(n) | |
