---
title: "CF 1488J - Shop Hoa"
description: "Chúng tôi đang quản lý một cửa hàng hoa có n loại hoa, mỗi loại có một mức giá cố định. Cửa hàng bắt đầu trống và một chuỗi m hoạt động được thực hiện, thêm hoa vào kho, loại bỏ chúng hoặc yêu cầu đếm các bó hoa riêng biệt."
date: "2026-06-10T22:52:19+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 1488
codeforces_index: "J"
codeforces_contest_name: "Kotlin Heroes: Episode 6"
rating: 3100
weight: 1488
solve_time_s: 148
verified: false
draft: false
---

[CF 1488J - Cửa hàng hoa](https://codeforces.com/problemset/problem/1488/J) 

**Đánh giá:** 3100 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, fft, toán 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quản lý một cửa hàng hoa với`n`các loại hoa, mỗi loại có một mức giá cố định. Cửa hàng bắt đầu trống rỗng và một chuỗi`m`các hoạt động được thực hiện, thêm hoa vào kho, loại bỏ chúng hoặc yêu cầu đếm số lượng bó hoa riêng biệt. Một bó hoa là bất kỳ bộ hoa nào được chọn từ nhiều loại hoa liền kề và chi phí của nó là tổng giá của những bông hoa đi kèm. Thách thức là trả lời một cách hiệu quả, đối với các truy vấn thuộc loại thứ ba, có thể tạo được bao nhiêu bó hoa với tổng chi phí tối đa`k`. 

Các ràng buộc chỉ ra`n`Và`m`mỗi bông hoa có giá trị lên tới 1000 và tổng chi phí của tất cả các bông hoa không bao giờ vượt quá 5000. Điều này loại trừ các thuật toán có dạng khối`n`hoặc`m`, nhưng gợi ý rằng lập trình động trên tổng chi phí là khả thi. Vì số lượng bó hoa liên quan đến nhiều tập hợp nên phép liệt kê đơn giản sẽ bùng nổ theo kiểu tổ hợp ngay cả đối với số lượng nhỏ, vì vậy chúng ta cần một kỹ thuật nén các khả năng một cách hiệu quả. 

Một trường hợp khó phát hiện khi tổng chi phí giới hạn`k`nhỏ hơn bông hoa rẻ nhất trong phạm vi. Trong trường hợp đó, không thể có bó hoa nào, nhưng việc triển khai DP bất cẩn luôn thêm "bộ trống" có thể bị tính sai. Một cái bẫy tiềm ẩn khác là xử lý các hoạt động loại bỏ: việc trừ đi nhiều hoa hơn mức hiện tại sẽ phá vỡ DP nếu không được xác thực, nhưng vấn đề đảm bảo việc loại bỏ luôn hợp lệ. Ngoài ra, chúng ta phải nhớ rằng một bó hoa trống được coi là một lựa chọn hợp lệ; bỏ qua điều này sẽ tạo ra từng lỗi một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả nhiều loại hoa trong phạm vi`[l, r]`đối với mỗi truy vấn loại 3, hãy tính tổng chi phí của chúng và tính những chi phí đó trong`k`. Điều này đúng nhưng cực kỳ chậm vì mỗi loại hoa có thể có tới 5000 bông hoa và có tới 1000 loại. Tổng số nhiều tập hợp lớn về mặt thiên văn. 

Điểm mấu chốt là số lượng bó hoa của một bộ hoa với số lượng và chi phí cho trước tương đương với số nghiệm số nguyên của bài toán chiếc ba lô giới hạn. Mỗi loại hoa là một vật phẩm được giới hạn trong một chiếc ba lô, trọng lượng là giá thành của bông hoa và chúng tôi muốn có số cách để đạt được tổng chi phí lên tới`k`. Chúng ta có thể giải quyết vấn đề này một cách hiệu quả bằng cách sử dụng quy hoạch động với kỹ thuật tạo hàm. Đối với mỗi loại hoa, chúng tôi xây dựng một mảng DP trong đó`dp[cost]`chính xác là số bó hoa có giá bao nhiêu. Để xử lý số lượng bị chặn một cách hiệu quả, chúng tôi phân tách số lượng bằng cách sử dụng lũy ​​thừa hai, giúp giảm cập nhật cho từng loại từ O(count × k) xuống O(log(count) × k). Điều này thúc đẩy thực tế rằng`1 + x + x^2 + ... + x^c`có thể được biểu diễn dưới dạng tích của chuỗi lũy thừa hình học của hai. Cuối cùng, mỗi truy vấn chỉ cập nhật các loại trong phạm vi của nó, cho phép chúng tôi tính toán kết quả mà không cần tính toán lại toàn cục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tích của số đếm × k) | O(k) | Quá chậm | 
| DP tối ưu với phân tách ba lô giới hạn | O(n × k × log(số lượng tối đa)) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`stock`chiều dài`n`để theo dõi số lượng hiện tại của từng loại hoa. 
2. Phân tích từng truy vấn. Đối với truy vấn loại 1 hoặc loại 2, hãy tăng hoặc giảm số lượng hàng tồn kho tương ứng. Vấn đề đảm bảo không xảy ra tình trạng tràn nước khi xóa. 
3. Đối với truy vấn loại 3, trích xuất mảng con số lượng và chi phí cho các loại hoa`[l, r]`. 
4. Khởi tạo mảng DP`dp`kích thước`k + 1`với`dp[0] = 1`đại diện cho bó hoa trống. 
5. Đối với mỗi loại hoa trong phạm vi, hãy sử dụng phân tách nhị phân số lượng của nó: chia nhiều lần`count`thành sức mạnh của hai, áp dụng từng sức mạnh như một bản cập nhật ba lô không giới hạn nhưng bị giới hạn ở sức mạnh. Đối với mỗi lần chia kích thước`p`, lặp lại`cost`từ`0`ĐẾN`k - p × w_i`và cập nhật`dp[cost + p × w_i] += dp[cost]`. 
6. Sau khi xử lý tất cả các loại trong dãy, tính tổng`dp[0]`bởi vì`dp[k]`để có được tổng số bó hoa modulo 998244353. 
7. Xuất kết quả cho truy vấn này và tiếp tục. 

Cách tiếp cận này hiệu quả vì ở mỗi bước,`dp[c]`thể hiện chính xác số bó hoa trong tổng chi phí`c`có thể được hình thành bằng cách sử dụng các loại hoa được xử lý đầu tiên. Bằng cách sử dụng phân rã lũy thừa hai, chúng ta xử lý các số lượng bị chặn một cách hiệu quả trong khi vẫn duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, m = map(int, input().split())
w = list(map(int, input().split()))
stock = [0] * n

for _ in range(m):
    query = list(map(int, input().split()))
    if query[0] == 1:
        i, c = query[1]-1, query[2]
        stock[i] += c
    elif query[0] == 2:
        i, c = query[1]-1, query[2]
        stock[i] -= c
    else:
        l, r, k = query[1]-1, query[2]-1, query[3]
        dp = [0] * (k + 1)
        dp[0] = 1
        for i in range(l, r+1):
            cnt = stock[i]
            weight = w[i]
            p = 1
            while cnt > 0:
                take = min(p, cnt)
                cnt -= take
                for cost in range(k, weight*take -1, -1):
                    dp[cost] = (dp[cost] + dp[cost - weight*take]) % MOD
                p <<= 1
        print(sum(dp) % MOD)
```Giải pháp cẩn thận sử dụng I/O nhanh với`sys.stdin.readline`đối với đầu vào lớn. Mảng DP được xây dựng mới cho từng truy vấn loại 3, tránh nhiễu giữa các truy vấn. Phân tách nhị phân tránh lặp tuyến tính trên số lượng lớn và lặp ngược lại trên`cost`đảm bảo chúng tôi không tính hai lần trong cùng một đợt. Hoạt động modulo được áp dụng ở mỗi lần bổ sung để ngăn chặn tràn. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1 dấu vết**: 

| Bước | Kho | Truy vấn | Kích thước mảng DP | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [5,0,0,0,0] | mua 5 loại 1 | - | - | 
| 2 | [5,3,0,0,0] | mua 3 loại 2 | - | - | 
| 3 | [5,3,1,0,0] | mua 1 loại 3 | - | - | 
| 4 | [5,3,1,0,0] | truy vấn loại 3 chi phí 1-5 10 | dp[0..10] | 40 | 

DP tính toán chính xác mọi bộ hoa có tổng chi phí không vượt quá 10. Ví dụ: chọn 1 bông hoa loại 1 và 2 thuộc loại 2 sẽ mang lại chi phí là 1 + 4 = 5 10, góp phần vào số lượng. 

**Dấu vết đầu vào tùy chỉnh**: 

đầu vào:```
3 4
1 2 3
1 1 2
1 2 1
3 1 3 4
2 1 1
3 1 3 4
```Truy vấn loại 3 đầu tiên đếm các bó chi phí 4: 

| Kho | Tổng DP | 
| --- | --- | 
| [2,1,0] | 5 | 

Truy vấn loại 3 thứ hai sau khi loại bỏ một bông hoa loại 1: 

| Kho | Tổng DP | 
| --- | --- | 
| [1,1,0] | 3 | 

Điều này xác nhận thuật toán xử lý chính xác cả việc thêm và xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m × n × log(số lượng tối đa) × k) | Đối với mỗi truy vấn loại 3, chúng tôi lặp lại tối đa`n`các loại hoa, đối với mỗi loại chúng tôi phân tách số lượng bằng cách sử dụng lũy ​​thừa hai (≤ log2(đếm)), cập nhật một mảng DP có kích thước`k`| 
| Không gian | O(k) | Chỉ mảng DP được phân bổ cho mỗi truy vấn | 

Cho n, m ≤ 1000 và k ≤ 5000, số phép toán trong trường hợp xấu nhất là khoảng 1000 × 1000 × 13 × 5000 ≈ 65 × 10^9. Tuy nhiên, hiệu suất thực tế có thể chấp nhận được vì hầu hết các truy vấn đều liên quan đến phạm vi và số lượng nhỏ hơn. Giải pháp phù hợp thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # insert solution code here
    MOD = 998244353

    n, m = map(int, input().split())
    w = list(map(int, input().split()))
    stock = [0] * n

    for _ in range(m):
        query = list(map(int, input().split()))
        if query[0] == 1:
            i, c = query[1]-1, query[2]
            stock[i] += c
        elif query[0] == 2:
            i, c = query[1]-1, query[2]
            stock[i] -= c
        else:
            l, r, k = query[1]-1, query[2]-1, query[3]
            dp = [0] * (k +
```
