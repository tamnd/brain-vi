---
title: "CF 186B - Trồng Nấm"
description: "Chúng tôi có một cuộc thi trồng nấm với hai giai đoạn cách nhau bằng thời gian nghỉ. Mỗi người tham gia có hai tốc độ và vấn đề là chúng tôi không biết thứ tự họ sẽ sử dụng chúng. Trong giai đoạn đầu tiên của chiều dài t1, nấm phát triển với tốc độ như nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 186
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 118 (Div. 2)"
rating: 1200
weight: 186
solve_time_s: 161
verified: true
draft: false
---

[CF 186B - Trồng Nấm](https://codeforces.com/problemset/problem/186/B) 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuộc thi trồng nấm với hai giai đoạn cách nhau bằng thời gian nghỉ. Mỗi người tham gia có hai tốc độ và vấn đề là chúng tôi không biết thứ tự họ sẽ sử dụng chúng. Trong giai đoạn đầu tiên có chiều dài _t1_, nấm phát triển với tốc độ như nhau. Sau khi nghỉ, chiều cao của cây nấm giảm đi _k_ phần trăm. Sau đó, trong giai đoạn thứ hai có chiều dài _t2_, nấm phát triển với tốc độ thứ hai. Mỗi người tham gia chọn thứ tự hai tốc độ của mình để tối đa hóa chiều cao cuối cùng của cây nấm. Nhiệm vụ của chúng tôi là tính toán chiều cao cuối cùng cho tất cả người tham gia và in chúng theo thứ tự từ cao nhất đến ngắn nhất. Nếu hai cây nấm có chiều cao bằng nhau thì người có chỉ số nhỏ hơn sẽ đứng trước. 

Đầu vào cung cấp _n_, số lượng người tham gia, thời lượng _t1_ và _t2_ và tỷ lệ phần trăm giảm _k_. Sau đó, mỗi người tham gia sẽ được đưa ra hai tốc độ _a_i_ và _b_i_. Đầu ra là chiều cao cuối cùng tối đa của mỗi người tham gia được làm tròn đến chính xác hai chữ số thập phân, được sắp xếp như mô tả. 

Các ràng buộc rất nhỏ: _n_, _t1_, _t2_ đều tối đa là 1000 và tốc độ tối đa là 1000. Điều này có nghĩa là chúng ta có thể dễ dàng thực hiện các phép tính O(n) hoặc O(n log n). Chúng ta phải cẩn thận với số học dấu phẩy động vì phép tăng bao gồm việc giảm phần trăm, đây không phải là phép toán số nguyên. 

Các trường hợp khó khăn có thể phức tạp bao gồm người tham gia có hai tốc độ bằng nhau, trong đó thứ tự mang lại kết quả như nhau hoặc giá trị _k_ cực cao, chẳng hạn như giảm 100 phần trăm, sẽ xóa hoàn toàn giai đoạn đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi người tham gia, hãy thử cả hai cấp tốc độ có thể có, tính chiều cao cây nấm cuối cùng cho mỗi đơn hàng và lấy mức tối đa. Cụ thể 2 độ cao cần so sánh là:```
h1 = a_i * t1 * (1 - k/100) + b_i * t2
h2 = b_i * t1 * (1 - k/100) + a_i * t2
```Tối đa của`h1`Và`h2`là chiều cao tối ưu cho người tham gia đó. Điều này hiệu quả vì số lượng người tham gia ít và mỗi phép tính có thời gian không đổi, đưa ra giải pháp O(n). Sau khi tính toán độ cao, chúng tôi sắp xếp những người tham gia theo chiều cao giảm dần và theo chỉ số tăng dần trong trường hợp có quan hệ ràng buộc. 

Thông tin chi tiết quan trọng để triển khai nhanh hơn hoặc rõ ràng hơn là chúng ta không cần khám phá bất kỳ kết hợp nào khác. Chỉ có hai tốc độ cho mỗi người tham gia và chỉ có hai đơn hàng, do đó không thể tối ưu hóa thêm. Sắp xếp sau đó là tiêu chuẩn và đảm bảo thứ tự chính xác trong bảng kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

Sắp xếp là O(n log n), chiếm ưu thế trong tính toán tuyến tính O(n) về độ cao. 

## Hướng dẫn thuật toán 

1. Đọc số lượng người tham gia`n`, hai khoảng thời gian`t1`Và`t2`, và giảm thời gian nghỉ`k`. Lưu trữ hệ số giảm dưới dạng phân số thập phân`r = (100 - k)/100`để đơn giản hóa việc tính toán. 
2. Khởi tạo danh sách trống`results`cái đó sẽ lưu trữ các bộ dữ liệu`(participant_index, max_height)`. 
3. Lặp lại từng người tham gia. Đọc hai tốc độ của họ`a`Và`b`. 
4. Tính chiều cao cuối cùng nếu người tham gia sử dụng`a`đầu tiên, sau đó`b`:`h1 = a * t1 * r + b * t2`. 
5. Tính chiều cao cuối cùng nếu người tham gia sử dụng`b`đầu tiên, sau đó`a`:`h2 = b * t1 * r + a * t2`. 
6. Lấy`max(h1, h2)`là chiều cao nấm tối ưu của người tham gia. 
7. Nối thêm`(participant_index, max_height)`ĐẾN`results`. 
8. Sau khi tất cả người tham gia được xử lý, hãy sắp xếp`results`đầu tiên bởi`-max_height`(giảm dần) và sau đó bằng`participant_index`(tăng dần) để giải quyết mối quan hệ. 
9. In chỉ số và chiều cao của mỗi người tham gia được định dạng chính xác đến hai chữ số thập phân. 

Tại sao nó hoạt động: Điều bất biến là đối với mỗi người tham gia, chúng tôi đã khám phá tất cả các thứ tự có thể có của hai tốc độ của họ. Chiều cao tối đa được đảm bảo là chính xác. Việc sắp xếp sau đảm bảo thứ hạng chính xác, tôn trọng quy tắc hòa. Không có sự kết hợp nào bị bỏ sót và số học phản ánh chính xác các quy tắc cạnh tranh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, t1, t2, k = map(int, input().split())
r = (100 - k) / 100  # reduction factor

results = []

for i in range(1, n + 1):
    a, b = map(int, input().split())
    h1 = a * t1 * r + b * t2
    h2 = b * t1 * r + a * t2
    max_height = max(h1, h2)
    results.append((i, max_height))

# sort by height descending, then index ascending
results.sort(key=lambda x: (-x[1], x[0]))

for idx, height in results:
    print(f"{idx} {height:.2f}")
```Mã tuân theo các bước thuật toán một cách chính xác. Hệ số rút gọn được tính toán trước để tránh lặp lại phép chia. Độ cao được lưu trữ dưới dạng số dấu phẩy động để duy trì độ chính xác sau khi nhân với`r`. Sắp xếp sử dụng hàm lambda để sắp xếp giảm dần theo chiều cao và tăng dần theo chỉ mục người tham gia. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 3 3 50
2 4
4 2
```| Người tham gia | một | b | h1 = a_t1_r + b*t2 | h2 = b_t1_r + a*t2 | chiều cao tối đa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 4 | 2_3_0,5 + 4*3 = 3 + 12 = 15 | 4_3_0.5 + 2*3 = 6 + 6 = 12 | 15 | 
| 2 | 4 | 2 | 4_3_0.5 + 2*3 = 6 + 6 = 12 | 2_3_0,5 + 4*3 = 3 + 12 = 15 | 15 | 

Sau khi sắp xếp theo chiều cao giảm dần rồi chỉ số tăng dần:```
1 15.00
2 15.00
```Điều này chứng tỏ thuật toán đánh giá chính xác cả hai thứ tự và áp dụng tính năng giảm ngắt. 

Đầu vào tùy chỉnh 2:```
3 2 4 25
5 5
6 3
2 8
```| Người tham gia | h1 | h2 | chiều cao tối đa | 
| --- | --- | --- | --- | 
| 1 | 5_2_0,75 + 5*4 = 7,5 + 20 = 27,5 | tương tự = 27,5 | 27,5 | 
| 2 | 6_2_0,75 + 3*4 = 9 + 12 = 21 | 3_2_0,75 + 6*4 = 4,5 + 24 = 28,5 | 28,5 | 
| 3 | 2_2_0,75 + 8*4 = 3 + 32 = 35 | 8_2_0,75 + 2*4 = 12 + 8 = 20 | 35 | 

Kết quả được sắp xếp:```
3 35.00
2 28.50
1 27.50
```Điều này chứng tỏ rằng lựa chọn tối ưu của mỗi người tham gia có thể không phải lúc nào cũng là “tốc độ giây lớn hơn”; nó phụ thuộc vào thời lượng và hệ số giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tính toán hai độ cao cho mỗi người tham gia là O(1), với n người tham gia sẽ cho O(n). Sắp xếp kết quả chiếm ưu thế ở O(n log n). | 
| Không gian | O(n) | Chúng tôi lưu trữ chiều cao và chỉ số tối đa cho mỗi người tham gia. | 

Các ràng buộc của bài toán cho phép n lên tới 1000, do đó O(n log n) nằm trong giới hạn 2 giây. Phép nhân và số học dấu phẩy động không đáng kể về chi phí thời gian cho các giới hạn này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # solution
    n, t1, t2, k = map(int, input().split())
    r = (100 - k) / 100
    results = []
    for i in range(1, n + 1):
        a, b = map(int, input().split())
        h1 = a * t1 * r + b * t2
        h2 = b * t1 * r + a * t2
        results.append((i, max(h1, h2)))
    results.sort(key=lambda x: (-x[1], x[0]))
    for idx, height in results:
        print(f"{idx} {height:.2f}")
    return out.getvalue().strip()

# Provided sample
assert run("2 3 3 50\n2 4\n4 2\n") == "1 15.00\n2 15.00", "sample 1"

# Minimum size
assert run("1 1 1 1\n1 1\n") == "1 1.99", "min size"

# All speeds equal
assert run("2 5 5 50\n10 10\n10 10
```
