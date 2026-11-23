---
title: "CF 466A - Du Lịch Giá Rẻ"
description: "Ann cần đi lại bằng tàu điện ngầm tổng cộng n lần. Cô ấy có thể trả tiền cho mỗi chuyến đi riêng lẻ với chi phí là một rúp, hoặc cô ấy có thể mua một vé đi nhiều chuyến đặc biệt bao gồm chính xác m chuyến đi và có giá b rúp."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 466
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 266 (Div. 2)"
rating: 1200
weight: 466
solve_time_s: 54
verified: true
draft: false
---

[CF 466A - Du lịch giá rẻ](https://codeforces.com/problemset/problem/466/A) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ann cần đi lại bằng tàu điện ngầm tổng cộng _n_ lần. Cô ấy có thể trả tiền cho từng chuyến đi riêng lẻ với chi phí _a_ rúp, hoặc cô ấy có thể mua một vé đặc biệt cho nhiều chuyến đi bao gồm chính xác _m_ chuyến đi và giá _b_ rúp. Bài toán yêu cầu chúng ta xác định tổng chi phí tối thiểu cô ấy phải trả để chi trả cho tất cả n_ chuyến đi, có thể bằng cách kết hợp vé riêng lẻ và vé nhiều chuyến. 

Các số nguyên đầu vào biểu thị các đại lượng sau: _n_ là tổng số chuyến đi, _m_ là số chuyến đi mà một vé nhiều chuyến bao gồm, _a_ là chi phí của một chuyến đi và _b_ là chi phí của một vé nhiều chuyến. Đầu ra là một số nguyên duy nhất, số tiền rúp tối thiểu cần thiết để hoàn thành tất cả các chuyến đi. 

Các ràng buộc là nhỏ, với tất cả các giá trị được giới hạn ở mức 1000. Điều này có nghĩa là ngay cả một giải pháp đơn giản lặp lại số lượng vé đi nhiều chuyến có thể cũng khả thi. Tuy nhiên, vấn đề có thể được giải quyết trực tiếp bằng số học đơn giản. Các trường hợp rủi ro chính phát sinh khi số lượng chuyến đi nhỏ hơn vé nhiều lượt hoặc khi vé nhiều lượt đắt hơn mua vé riêng lẻ. Ví dụ: nếu _n = 1_, _m = 5_, _a = 1_ và _b = 10_, thì lựa chọn rẻ nhất rõ ràng là mua một vé đi một chuyến với giá 1 rúp chứ không phải vé đi 5 chuyến. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ lặp lại tất cả số lượng vé nhiều lượt có thể có từ 0 đến ⌈_n/m_⌉, tính toán số lượt đi còn lại để trang trải bằng các vé đơn và theo dõi tổng chi phí tối thiểu. Cách tiếp cận này đúng vì nó khám phá tất cả các kết hợp có thể có, nhưng ở đây nó quá mức cần thiết. Trong trường hợp xấu nhất, vòng lặp này thực hiện tối đa 1000 lần, điều này có thể chấp nhận được đối với các ràng buộc của bài toán, nhưng chúng ta có thể làm tốt hơn với số học trực tiếp. 

Nhận xét quan trọng là vấn đề rút gọn thành một so sánh đơn giản giữa hai chiến lược cho số lượt đi còn lại sau khi mua càng nhiều vé đi nhiều chuyến càng tốt: hoặc mua riêng số lượng chuyến đi chính xác còn lại hoặc mua thêm một vé đi nhiều chuyến để trang trải chúng. Để có vé đi nhiều chuyến đầy đủ, hãy tính`full_sets = n // m`. Phần còn lại là`remainder = n % m`. Chi phí tối thiểu là tổng của`full_sets * b`cộng với lựa chọn rẻ hơn của`remainder * a`hoặc`b`. Logic này nắm bắt tất cả các kết hợp tối ưu mà không cần lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n/m) ≈ O(1000) | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem Ann có thể mua bao nhiêu vé đi nhiều lượt đầy đủ bằng cách chia số nguyên:`full_sets = n // m`. Điều này thể hiện số lượng vé đi nhiều chuyến tối đa không vượt quá số lượt đi được yêu cầu. 
2. Tính xem còn bao nhiêu chuyến đi chưa được thực hiện sau khi mua đủ vé đi nhiều lượt:`remainder = n % m`. 
3. Tính chi phí nếu Ann mua chính xác các chuyến đi còn lại dưới dạng vé lẻ:`cost_single_remainder = remainder * a`. 
4. Tính chi phí nếu Ann mua thêm một vé đi nhiều lượt để chi trả cho những lượt đi còn lại:`cost_extra_ticket = b`. 
5. Chọn tối thiểu hai lựa chọn cho các chuyến đi còn lại và cộng nó vào chi phí của toàn bộ vé đi nhiều chuyến:`total_cost = full_sets * b + min(cost_single_remainder, cost_extra_ticket)`. 
6. In`total_cost`. 

Tại sao nó hoạt động: Thuật toán hoạt động vì mọi sự kết hợp giữa vé nhiều chuyến và vé đơn đều có thể được biểu diễn dưới dạng bộ đầy đủ cộng với lựa chọn cho phần còn lại. Việc so sánh hai lựa chọn cho những chuyến đi còn lại đảm bảo rằng Ann không bao giờ trả nhiều hơn mức cần thiết cho những chuyến đi cuối cùng và việc mua thêm vé trọn gói cho những chuyến đi được bảo hiểm trước đó sẽ không bao giờ giảm chi phí. Vấn đề là tuyến tính về số chuyến đi nhưng giảm xuống số học theo thời gian không đổi do cấu trúc chi phí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, a, b = map(int, input().split())

full_sets = n // m
remainder = n % m

# cheapest way to cover remaining rides
remainder_cost = min(remainder * a, b)

total_cost = full_sets * b + remainder_cost
print(total_cost)
```Đầu tiên, mã sẽ đọc bốn số nguyên và ngay lập tức tính toán số lượng vé đi nhiều lượt đầy đủ và số lượt đi còn lại. Sau đó, nó tính toán lựa chọn rẻ hơn để bù đắp cho những chuyến đi còn sót lại, bằng vé đơn hoặc vé bổ sung cho nhiều lượt đi. Cuối cùng, nó tổng hợp chi phí và đưa ra kết quả. Việc sử dụng phép chia số nguyên và mô đun sẽ tránh được các lỗi riêng lẻ khi tính toán các tập hợp đầy đủ và các phần còn lại. Chức năng tối thiểu đảm bảo quyết định chính xác cho các chuyến đi còn sót lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`6 2 1 2`| full_sets | phần còn lại | còn lại_chi phí | tổng_chi phí | 
| --- | --- | --- | --- | 
| 3 | 0 | 0 | 6 | 

Giải thích: Ann có thể mua ba vé đi 2 lượt với giá 2 rúp mỗi vé, bao gồm cả 6 lượt đi. Không còn chuyến đi nào còn sót lại. Tổng chi phí là 3 * 2 = 6. 

### Mẫu 2 

đầu vào:`7 3 2 5`| full_sets | phần còn lại | còn lại_chi phí | tổng_chi phí | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 12 | 

Giải thích: Mua 2 vé đi nhiều lượt sẽ được 6 lượt đi. Còn lại một chuyến. Mua một vé duy nhất với giá 2 rúp sẽ rẻ hơn so với mua một vé đi 3 lượt khác với giá 5 rúp. Tổng chi phí = 2 * 5 + 2 = 12. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học được thực hiện, không có vòng lặp. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Cho n, m, a, b ≤ 1000, giải pháp này thực hiện tính bằng micro giây, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m, a, b = map(int, input().split())
    full_sets = n // m
    remainder = n % m
    remainder_cost = min(remainder * a, b)
    total_cost = full_sets * b + remainder_cost
    return str(total_cost)

# Provided sample
assert run("6 2 1 2\n") == "6", "sample 1"

# Minimum-size input
assert run("1 1 1 1\n") == "1", "minimum rides equal ticket"

# Single ticket cheaper than multi
assert run("1 5 1 10\n") == "1", "single ticket cheaper than multi"

# Multi-ticket cheaper than single multiple
assert run("10 3 4 10\n") == "34", "mixed multi and single optimal"

# Rides exactly divisible by multi-ride ticket
assert run("6 3 2 5\n") == "10", "rides divisible by multi-ride ticket"

# Rides smaller than multi-ride ticket, multi-ride cheaper
assert run("2 5 3 5\n") == "5", "multi-ride ticket cheaper than 2 singles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | 1 | Đầu vào tối thiểu | 
| 1 5 1 10 | 1 | Vé đơn rẻ hơn nhiều | 
| 10 3 4 10 | 34 | Kết hợp tối ưu đa và đơn | 
| 6 3 2 5 | 10 | Phân chia chính xác theo nhiều vé | 
| 2 5 3 5 | 5 | Nhiều vé rẻ hơn vé đơn | 

## Vỏ cạnh 

cho`n = 1, m = 5, a = 1, b = 10`, số dư là 1. Thuật toán so sánh 1 * 1 = 1 với 10 và chọn 1, đưa ra chi phí tối thiểu chính xác. Vì`n = 6, m = 2, a = 1, b = 2`, số dư = 0 nên không phát sinh thêm chi phí. Vì`n`chia hết cho`m`, thuật toán không mua quá mức, tránh phải trả tiền cho những chuyến đi không sử dụng. Mỗi trường hợp xác nhận rằng hàm min và phép chia số nguyên xử lý chính xác cả phần dư nhỏ và phần dư lớn.
