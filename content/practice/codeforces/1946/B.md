---
title: "CF 1946B - Tổng tối đa"
description: "Chúng ta bắt đầu với một mảng số nguyên. Mỗi thao tác cho phép chúng ta chọn bất kỳ mảng con liền kề nào, bao gồm cả mảng con trống, tính tổng của nó và chèn tổng đó dưới dạng một phần tử mới vào bất kỳ đâu trong mảng. Quan sát quan trọng là việc chèn một giá trị sẽ thay đổi tổng của mảng."
date: "2026-06-07T17:50:36+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1946
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 936 (Div. 2)"
rating: 1100
weight: 1946
solve_time_s: 74
verified: true
draft: false
---

[CF 1946B - Tổng tối đa](https://codeforces.com/problemset/problem/1946/B) 

**Đánh giá:** 1100 
**Tags:** dp, tham lam, toán học 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng số nguyên. Mỗi thao tác cho phép chúng ta chọn bất kỳ mảng con liền kề nào, bao gồm cả mảng con trống, tính tổng của nó và chèn tổng đó dưới dạng một phần tử mới vào bất kỳ đâu trong mảng. 

Quan sát quan trọng là việc chèn một giá trị sẽ thay đổi tổng của mảng. Nếu mảng con được chọn có tổng`x`, sau đó sau khi chèn`x`, tổng mảng tăng chính xác`x`. 

Nhiệm vụ là thực hiện chính xác`k`hoạt động và tối đa hóa tổng mảng cuối cùng. Vì câu trả lời có thể trở nên rất lớn nên chúng tôi xuất nó theo modulo`10^9 + 7`. 

Các ràng buộc đủ lớn để chúng ta cần một giải pháp gần như tuyến tính. Trên tất cả các trường hợp thử nghiệm, tổng giá trị của`n + k`nhiều nhất là`2·10^5`, loại trừ bất kỳ cách tiếp cận nào mô phỏng các mảng con lặp đi lặp lại hoặc khám phá các chuỗi hoạt động. Chúng ta cần thứ gì đó xung quanh`O(n)`hoặc`O(n log n)`mỗi trường hợp thử nghiệm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một mảng mà mọi mảng con đều có tổng không dương:```
n = 2, k = 2
a = [-4, -7]
```Sự lựa chọn tốt nhất là mảng con trống, có tổng bằng`0`. Bất kỳ mảng con nào không trống sẽ làm giảm câu trả lời cuối cùng. Kết quả đúng là`-11`, trở thành`999999996`modulo`10^9+7`. 

Hãy xem xét một mảng có mảng con tối đa dương:```
n = 3, k = 2
a = [2, 2, 8]
```Tổng mảng con tối đa là`12`. Sau khi chèn`12`, phần tử mới đó có thể trở thành một phần của mảng con thậm chí còn lớn hơn. Một giải pháp bổ sung`12`hai lần sẽ có được`36`, nhưng quá trình tối ưu tạo ra`12 + 24`, đưa ra tổng cuối cùng là`48`. 

Một trường hợp tinh tế khác là khi tổng mảng là âm nhưng tồn tại một mảng con dương:```
n = 3, k = 1
a = [100, -200, 150]
```Tổng số tiền là`50`, nhưng thao tác tốt nhất sẽ chèn tổng mảng con tối đa`150`, mang lại`200`. Chỉ nhìn vào tổng số tiền sẽ bỏ lỡ nguồn lợi nhuận thực sự. 

## Phương pháp tiếp cận 

Quan điểm brute-force là coi mỗi thao tác là một sự lựa chọn trong số tất cả các mảng con. có`O(n²)`mảng con ở mỗi bước và độ dài mảng tăng lên sau mỗi lần chèn. Ngay cả đối với một hoạt động, việc này cũng tốn kém và có thể lên tới`2·10^5`hoạt động đó trở nên hoàn toàn không khả thi. 

The brute-force idea is correct because every operation contributes exactly the chosen subarray sum to the global total. The difficulty is determining which sequence of choices maximizes the accumulated gain.

 The breakthrough comes from understanding what happens after we insert a positive sum.

 Suppose the maximum subarray sum of the original array is`M`. 

Nếu như`M ≤ 0`, không có mảng con nào không trống đáng được lấy. Vì mảng con trống được cho phép nên mọi thao tác đều có thể đóng góp`0`. Câu trả lời đơn giản là tổng mảng ban đầu. 

Bây giờ giả sử`M > 0`. 

Trong thao tác đầu tiên, chúng ta chèn một giá trị bằng`M`. Vì giá trị đó hiện có trong mảng nên chúng ta có thể chọn một mảng con chứa cả phân đoạn có tổng tối đa ban đầu và phân đoạn mới được chèn vào.`M`. Tổng của nó trở thành`2M`. 

Sau khi chèn`2M`, chúng ta có thể lặp lại ý tưởng tương tự và có được`4M`, sau đó`8M`, vân vân. 

Trình tự lợi nhuận là```
M, 2M, 4M, ..., 2^(k-1)M
```tổng đóng góp của họ là```
M(2^k - 1)
```Không có chiến lược nào có thể làm tốt hơn, bởi vì mọi giá trị được chèn cuối cùng đều bắt nguồn từ tổng mảng con tối đa có sẵn cho đến nay và chiến lược nhân đôi này luôn mang lại mức tăng trưởng lớn nhất có thể. 

Do đó, bài toán rút gọn thành hai đại lượng:```
S = sum of all array elements
M = maximum subarray sum, but at least 0
```Câu trả lời cuối cùng là```
S + M(2^k - 1)
```modulo tính toán`10^9 + 7`. 

Tổng mảng con tối đa thu được bằng thuật toán Kadane trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / tệ hơn O(k·n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng mảng`S`. 
2. Chạy thuật toán Kadane để tìm tổng mảng con lớn nhất. 
3. Thay thế giá trị đó bằng`max(0, maximum_subarray_sum)`. 

Nếu mọi mảng con đều âm, chúng ta luôn có thể chọn mảng con trống và thu được`0`. 
4. Tính toán`pow2 = 2^k mod MOD`. 
5. Tính số tiền đóng góp thêm:```
M(2^k - 1)
```modulo`MOD`. 
6. Quy đổi số tiền ban đầu`S`sang dạng modulo. 

Từ`S`có thể âm, hãy sử dụng phép toán modulo của Python. 
7. Cộng hai phần theo modulo`MOD`và xuất kết quả. 

### Tại sao nó hoạt động 

hãy để`M`là tổng mảng con không âm tối đa hiện có thể đạt được. 

Sau khi chèn một giá trị bằng`M`, giá trị được chèn đó có thể được đặt liền kề với phân đoạn tạo ra`M`. Tổng mảng con tối đa có thể đạt được tiếp theo sẽ trở thành`2M`. Lặp lại lập luận này cho thấy mức tăng tối ưu tạo thành chuỗi```
M, 2M, 4M, ..., 2^(k-1)M.
```Tổng của chúng chính xác là`M(2^k-1)`. 

Bất kỳ giá trị được chèn nào đều bắt nguồn từ một số tổng của mảng con. Không có mảng con nào có thể vượt quá tổng số mảng con tối đa có thể đạt được hiện tại, do đó, không có hoạt động nào có thể đạt được nhiều hơn mức tăng gấp đôi của chiến lược ở bước đó. Vì chiến lược luôn đạt được mức tăng tối đa có thể nên nó là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        total = sum(a)

        best = 0
        cur = 0

        for x in a:
            cur = max(0, cur + x)
            best = max(best, cur)

        extra = best * ((pow(2, k, MOD) - 1) % MOD)
        ans = (total % MOD + extra) % MOD

        print(ans)

solve()
```Biến`total`lưu trữ tổng mảng ban đầu. 

Thuật toán của Kadane được thực hiện bằng cách sử dụng`cur`Và`best`. biểu hiện```
cur = max(0, cur + x)
```duy trì tổng mảng con tối đa kết thúc ở vị trí hiện tại, đồng thời cho phép mảng con trống. Kết quả là,`best`tự động trở thành`max(0, maximum_subarray_sum)`. 

giá trị`2^k`có thể rất lớn nên cần phải có lũy thừa mô-đun. Tích hợp sẵn của Python`pow(2, k, MOD)`tính toán nó trong`O(log k)`thời gian. 

Một sai lầm phổ biến là quên điều đó`total`có thể là tiêu cực. sử dụng```
total % MOD
```chuyển đổi nó thành biểu diễn mô-đun chính xác trước khi thêm phần đóng góp bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 3
a = [2, 2, 8]
```Dấu vết của Kadane: 

| Yếu tố | cur | tốt nhất | 
| --- | --- | --- | 
| 2 | 2 | 2 | 
| 2 | 4 | 4 | 
| 8 | 12 | 12 | 

Bây giờ: 

| Biến | Giá trị | 
| --- | --- | 
| S | 12 | 
| M | 12 | 
| 2^k - 1 | 7 | 
| Thêm | 84 | 
| Trả lời | 96 | 

Lợi ích là`12`,`24`, Và`48`, tổng của nó là`84`. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 2
a = [-4, -7]
```Dấu vết của Kadane: 

| Yếu tố | cur | tốt nhất | 
| --- | --- | --- | 
| -4 | 0 | 0 | 
| -7 | 0 | 0 | 

Bây giờ: 

| Biến | Giá trị | 
| --- | --- | 
| S | -11 | 
| M | 0 | 
| Thêm | 0 | 
| Trả lời | 999999996 | 

Điều này chứng tỏ tại sao việc cho phép mảng con trống là điều cần thiết. Mọi lựa chọn không trống rỗng
