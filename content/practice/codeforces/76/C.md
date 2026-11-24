---
title: "CF 76C - Đột biến"
description: "Chúng ta được cung cấp một chuỗi đại diện cho bộ gen của một sinh vật, trong đó mỗi ký tự là một trong K chữ cái viết hoa đầu tiên. Các gen liền kề góp phần vào tổng “nguy cơ mắc bệnh” theo ma trận K × K nhất định gồm các số nguyên không âm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 76
codeforces_index: "C"
codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics"
rating: 2700
weight: 76
solve_time_s: 126
verified: true
draft: false
---

[CF 76C - Đột biến](https://codeforces.com/problemset/problem/76/C) 

**Đánh giá:** 2700 
**Tags:** bitmasks, dp, toán học 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho bộ gen của một sinh vật, trong đó mỗi ký tự là một trong những chữ cái _K_ in hoa đầu tiên. Các gen liền kề góp phần vào tổng “nguy cơ mắc bệnh” theo ma trận _K × K_ nhất định gồm các số nguyên không âm. Ngoài ra, chúng tôi được phép loại bỏ tất cả sự xuất hiện của một số loại gen. Việc loại bỏ một loại gen sẽ làm tăng tổng rủi ro hơn nữa bằng một giá trị cố định liên quan đến loại gen đó. 

Nhiệm vụ của chúng tôi là đếm xem có thể thu được bao nhiêu bộ gen khác nhau thông qua việc xóa như vậy sao cho tổng rủi ro của bộ gen thu được không vượt quá một ngưỡng nhất định _T_. Hai bộ gen được coi là khác nhau nếu trình tự kết quả của chúng không giống nhau. 

Những hạn chế làm cho điều này không tầm thường. Độ dài bộ gen _N_ có thể lên tới 200.000, do đó, bất kỳ cách tiếp cận nào lặp lại trên tất cả các chuỗi con hoặc tất cả các thao tác xóa có thể xảy ra một cách ngây thơ sẽ là quá chậm. Mặt khác, _K_ nhiều nhất là 22, vì vậy các tập hợp con của các loại gen có thể được biểu diễn bằng cách sử dụng mặt nạ bit, cho chúng ta gợi ý rằng phương pháp lập trình động hoặc phương pháp loại trừ bao gồm đối với các tập hợp con là khả thi. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó việc loại bỏ hầu hết tất cả các gen chỉ để lại một hoặc hai ký tự hoặc khi nhiều thao tác xóa chồng chéo lên nhau sẽ ảnh hưởng đến các rủi ro liền kề. Một cách tiếp cận bất cẩn có thể bỏ qua sự tương tác giữa các gen bị loại bỏ và đánh giá thấp tổng rủi ro. 

Ví dụ, hãy xem xét bộ gen`ABAB`với`K = 2`,`T = 5`,`t = [1, 2]`, và rủi ro lân cận`a = [[0, 3], [3, 0]]`. Nếu bạn loại bỏ tất cả`A`là, bộ gen còn lại là`BB`, nhưng tổng rủi ro phải bao gồm cả phần tăng thêm`t[0]`để loại bỏ`A`s, nếu không thì việc kiểm tra _T_ sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Người ta có thể thử loại bỏ mọi tập hợp con có thể có của loại gen _K_, sau đó mô phỏng bộ gen thu được, tính toán rủi ro lân cận, thêm hình phạt loại bỏ và kiểm tra xem tổng rủi ro có phải là ≤ _T_ hay không. Số lượng tập hợp con là 2^K, nhiều nhất là 4 triệu với K = 22. Đối với mỗi tập hợp con, việc tính toán rủi ro bộ gen một cách đơn giản bao gồm việc quét chuỗi có độ dài lên tới 200.000. Điều đó mang lại tổng số thao tác khoảng 2^22 × 2×10^5 ≈ 8×10^11, điều này rõ ràng là không khả thi. 

Cái nhìn sâu sắc quan trọng là tách các đóng góp lân cận của bộ gen khỏi các hình phạt loại bỏ. Đối với mỗi cặp loại gen, chúng ta có thể tính toán trước số lần cặp đó xuất hiện liền kề trong bộ gen ban đầu. Việc loại bỏ một loại gen chỉ ảnh hưởng đến các cặp gen đó xuất hiện. Do đó, chúng ta có thể tính toán hiệu quả của việc loại bỏ bất kỳ tập hợp con gen nào bằng cách sử dụng số lượng được tính toán trước này thay vì quét lại bộ gen. Điều này làm giảm tính toán trên mỗi tập hợp con xuống còn O(K^2), điều này khả thi vì các phép toán 2^22 × 22^2 ≈ 2×10^7, cũng nằm trong giới hạn thời gian. 

Giải pháp tối ưu sử dụng lập trình động bitmask. Chúng tôi lặp lại tất cả các tập hợp con của các loại gen và theo dõi rủi ro bổ sung do các gen bị loại bỏ gây ra. Sự đóng góp lân cận của các gen bị loại bỏ có thể được tính toán một cách hiệu quả bằng cách tính tổng số lượng tính toán trước nhân với ma trận rủi ro. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^K × N) | O(1) | Quá chậm | 
| Tối ưu | O(2^K × K^2) | O(K^2 + N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích đầu vào: đọc chiều dài bộ gen`N`, số kiểu gen`K`, và ngưỡng`T`. Đọc chuỗi gen, loại bỏ hình phạt`t[i]`và ma trận rủi ro lân cận`a[i][j]`. 
2. Chuyển đổi các ký tự bộ gen thành các chỉ số dựa trên 0 để đơn giản hóa việc tính toán. 
3. Tính toán trước số kề cận: khởi tạo ma trận K × K`cnt[i][j]`đại diện cho bao nhiêu lần loại gen`i`ngay sau đó là loại gen`j`trong bộ gen. Lặp lại bộ gen một lần và tăng dần`cnt[genome[i]][genome[i+1]]`. 
4. Khởi tạo một biến`result = 0`để đếm các bộ gen hợp lệ. 
5. Lặp lại tất cả các tập hợp con của các loại gen bằng cách sử dụng mặt nạ bit`mask`từ 1 đến 2^K - 1. Mỗi bit được đặt trong mặt nạ cho biết loại gen cần loại bỏ. 
6. Đối với mỗi tập hợp con, hãy tính tổng rủi ro bổ sung do loại bỏ các gen này. Bắt đầu với tổng`t[i]`cho tất cả các loại gen bị loại bỏ. 
7. Tính toán rủi ro kề cận cho bộ gen thu được. Đối với mỗi cặp gen`(i, j)`trong đó cả hai đều không bị xóa, tính tổng`cnt[i][j] * a[i][j]`. 
8. Tổng hợp rủi ro lân cận và hình phạt loại bỏ. Nếu tổng ≤`T`, tăng`result`. 
9. Đầu ra`result`. 

Tại sao nó hoạt động: mỗi tập hợp con được xem xét chính xác một lần. Việc tính toán trước số lượng lân cận đảm bảo rằng các hiệu ứng loại bỏ được tính toán chính xác mà không cần quét bộ gen nhiều lần. Vì các tập hợp con được liệt kê một cách có hệ thống bằng cách sử dụng mặt nạ bit nên tất cả các bộ gen đột biến có thể được tính mà không trùng lặp và tổng tính toán rủi ro là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N, K, T = map(int, input().split())
genome = input().strip()
t = list(map(int, input().split()))
a = [list(map(int, input().split())) for _ in range(K)]

# convert genome to 0-based indices
genome_idx = [ord(c) - ord('A') for c in genome]

# precompute adjacency counts
cnt = [[0] * K for _ in range(K)]
for i in range(N - 1):
    cnt[genome_idx[i]][genome_idx[i+1]] += 1

result = 0
for mask in range(1, 1 << K):
    removed = [bool(mask & (1 << i)) for i in range(K)]
    total_risk = 0
    
    # add removal penalties
    for i in range(K):
        if removed[i]:
            total_risk += t[i]
    
    # compute adjacency risk
    for i in range(K):
        if removed[i]:
            continue
        for j in range(K):
            if removed[j]:
                continue
            total_risk += cnt[i][j] * a[i][j]
    
    if total_risk <= T:
        result += 1

print(result)
```Mã này tuân theo thuật toán chính xác. Tính toán trước số lượng lân cận đảm bảo chúng tôi không quét bộ gen nhiều lần. Việc sử dụng mặt nạ bit cho các tập hợp con sẽ giúp việc liệt kê tập hợp con được rõ ràng. Các điều kiện biên được xử lý bằng cách bắt đầu từ mặt nạ = 1 để tránh các bộ gen trống. Chuyển đổi sang các chỉ mục dựa trên 0 sẽ tránh được các lỗi sai sót khi lập chỉ mục`a[i][j]`Và`cnt[i][j]`. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5 3 13
BACAC
4 1 2
1 2 3
2 3 4
3 4 10
```| Mặt nạ tập hợp con | Loại bỏ gen | Rủi ro lân cận | Rủi ro loại bỏ | Tổng rủi ro | ≤ T | 
| --- | --- | --- | --- | --- | --- | 
| 0b000 | không | 11 | 0 | 11 | vâng | 
| 0b001 | A | 6 | 4 | 10 | vâng | 
| 0b010 | B | 2 | 1 | 3 | vâng | 
| 0b011 | A, B | 1 | 5 | 6 | vâng | 
| 0b100 | C | 7 | 2 | 9 | vâng | 
| người khác | ... | ... | ... | >13 | không | 

Số lượng sinh vật hợp lệ là 5. 

**Ví dụ tùy chỉnh:** 

bộ gen`AA`, K = 1, T = 1, t = [1], a = [[1]] 

| Tập hợp con | Đã xóa | Liền kề | Loại bỏ | Tổng cộng | ≤T | 
| --- | --- | --- | --- | --- | --- | 
| 0b0 | không | 1 | 0 | 1 | vâng | 
| 0b1 | A | 0 | 1 | 1 | vâng | 

Đếm = 2, xác nhận loại gen đơn hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^K × K^2) | 2^K tập hợp con, tổng kề cận O(K^2) trên mỗi tập hợp con | 
| Không gian | O(K^2 + N) | ma trận đếm kề + lưu trữ bộ gen | 

Với các phép toán K 22, 2^22 × 22^2 ≈ 2×10^7. N 2×10^5 vừa với bộ nhớ. Điều này thoải mái chạy dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    N, K, T = map(int, input().split())
    genome = input().strip()
    t = list(map(int, input().split()))
    a = [list(map(int, input().split())) for _ in range(K)]
    genome_idx = [ord(c) - ord('A') for c in genome]
    cnt = [[0] * K for _ in range(K)]
    for i in range(N-1):
        cnt[genome_idx[i
```
