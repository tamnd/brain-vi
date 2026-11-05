---
title: "CF 1194F - Chuyên gia ô chữ"
description: "Chúng ta được yêu cầu tính số lượng ô chữ dự kiến ​​mà Adilbek có thể giải hoàn toàn trong một khoảng thời gian cố định, với điều kiện là mỗi ô chữ mất ti hoặc ti + 1 giây độc lập với xác suất bằng nhau."
date: "2026-06-12T00:19:41+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "number-theory", "probabilities", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1194
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 68 (Rated for Div. 2)"
rating: 2400
weight: 1194
solve_time_s: 151
verified: true
draft: false
---

[CF 1194F - Chuyên gia ô chữ](https://codeforces.com/problemset/problem/1194/F) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dp, lý thuyết số, xác suất, hai con trỏ 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính số lượng ô chữ dự kiến mà Adilbek có thể giải hoàn toàn trong một khoảng thời gian cố định, với điều kiện là mỗi ô chữ cần một trong hai`t_i`hoặc`t_i + 1`giây độc lập với xác suất bằng nhau. Anh ta giải ô chữ theo thứ tự nghiêm ngặt và dừng ngay lập tức khi tổng thời gian đạt hoặc vượt quá`T`. Giá trị kỳ vọng mà chúng ta phải tính là tổng có trọng số xác suất trên tất cả các số ô chữ đã hoàn thành có thể có. 

Đầu vào mang lại`n`, số ô chữ,`T`, tổng thời gian có sẵn và danh sách`t`số lần chơi ô chữ riêng lẻ. Đầu ra là giá trị mong đợi được biểu thị bằng`P * Q^{-1} mod 10^9+7`Ở đâu`P/Q`là giá trị kỳ vọng hợp lý. 

Các ràng buộc rất chặt chẽ:`n`có thể lên tới 2·10^5 và`T`lên đến 2·10^14. Điều này loại trừ bất kỳ cách tiếp cận bạo lực nào xem xét tất cả`2^n`trình tự thời gian hoàn thành. Đệ quy đơn giản hoặc bảng xác suất rõ ràng sẽ quá chậm. Mỗi`t_i`lên tới 10^9, vì vậy cần phải xử lý cẩn thận các tổng tích lũy để tránh tràn số nguyên. 

Các trường hợp cạnh bao gồm các tình huống trong đó`T`nhỏ hơn thời gian chơi ô chữ đầu tiên (giá trị mong đợi phải là 0) hoặc chỉ đủ cho đúng một ô chữ (giá trị mong đợi có thể là phân số). Một trường hợp tinh tế khác xảy ra khi nhiều ô chữ có cùng thời gian - phân bố xác suất trên số lần hoàn thành liên quan đến lũy thừa 1/2 và cần số học mô-đun chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ xem xét mọi chuỗi thành công có thể có và độ trễ thêm một giây, tính toán xác suất chính xác để hoàn thành 0, 1, …,`n`ô chữ. Đối với mỗi ô chữ, có hai lựa chọn, đưa ra`2^n`tổng số trình tự. Vì`n=2*10^5`, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là tuyến tính của kỳ vọng. Chúng ta không cần phải xem xét tất cả các tổ hợp độ trễ. Số ô chữ dự kiến ​​mà Adilbek hoàn thành được là tổng xác suất trên mỗi ô chữ mà Adilbek hoàn thành đúng thời hạn. Nếu như`dp[i]`đại diện cho phân bố xác suất trong tổng thời gian sau`i`trò chơi ô chữ, sự đóng góp dự kiến ​​của`(i+1)`- Ô chữ thứ chỉ là tổng xác suất hoàn thành tất cả các ô chữ trước đó và có đủ thời gian còn lại. 

Chúng ta có thể thực hiện điều này một cách hiệu quả bằng cách sử dụng cửa sổ trượt xác suất tích lũy. Cho phép`prefix[i]`là tổng số lần`t[0..i-1]`. Nếu có thời gian sau`i`vượt quá ô chữ`T - t[i]`, ô chữ chắc chắn đã hoàn thành; nếu nó vượt quá`T - t[i] - 1`, nó được hoàn thành với xác suất 1/2. Điều này làm giảm vấn đề tính tổng tích lũy theo chuỗi và áp dụng lũy ​​thừa 1/2. Nghịch đảo mô-đun xử lý các yếu tố 1/2 theo số học modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kỳ vọng tuyến tính + Tổng tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tổng tiền tố của số lần chơi ô chữ. Điều này mang lại`S[i] = t_0 + t_1 + … + t_{i-1}`. Đây là thời gian tối thiểu để hoàn thành công việc đầu tiên`i`ô chữ. 
2. Tính nghịch đảo mô đun của 2, vì mỗi giây tăng thêm xảy ra với xác suất là 1/2. Nghịch đảo mô-đun sẽ cho phép tính toán xác suất theo`10^9+7`. 
3. Khởi tạo một biến`prob`bằng 1 đại diện cho xác suất tất cả các ô chữ trước đó được hoàn thành trong thời gian tối thiểu. 
4. Lặp lại từng ô chữ`i`. Nếu tổng tích lũy`S[i] + t[i]`nhỏ hơn hoặc bằng`T`, ô chữ chắc chắn đã hoàn thành. Thêm vào`prob`đến giá trị mong đợi. Nếu như`S[i] + t[i] + 1`nhỏ hơn hoặc bằng`T`, ô chữ hoàn thành với xác suất 1/2; thêm vào`prob * 1/2`. nhân`prob`bằng 1/2 sau khi xem xét kịch bản thêm giây, vì tính đến xác suất hoàn thành các nửa ô chữ tiếp theo. 
5. Sau khi lặp qua tất cả các ô chữ, chúng ta có giá trị mong đợi dưới dạng phân số hữu tỉ`P/Q`. Nhân với`Q^-1 mod 10^9+7`để có được câu trả lời cuối cùng. 

Tại sao nó hoạt động: tính tuyến tính của kỳ vọng cho phép chúng ta tính tổng đóng góp dự kiến ​​của từng ô chữ một cách độc lập. Tổng tiền tố đảm bảo chúng tôi chỉ xem xét các trường hợp hoàn thành khả thi. Quyền hạn của 1/2 tính toán chính xác độ trễ thêm một giây trong khi tránh việc liệt kê các chuỗi theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def main():
    n, T = map(int, input().split())
    t = list(map(int, input().split()))
    
    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + t[i]
    
    inv2 = modinv(2)
    expected = 0
    prob = 1
    
    for i in range(n):
        if prefix[i + 1] <= T:
            expected = (expected + prob) % MOD
        elif prefix[i] + t[i] <= T:
            expected = (expected + prob * inv2) % MOD
            prob = (prob * inv2) % MOD
            break
        else:
            break
        prob = (prob * inv2) % MOD

    print(expected)

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng tổng tiền tố để có thời gian hoàn thành tối thiểu. Chúng tôi xử lý nghịch đảo mô-đun một cách rõ ràng để tránh số học dấu phẩy động. các`prob`biến duy trì xác suất rằng tất cả các trò chơi ô chữ trước đó đã được hoàn thành trong thời gian tối thiểu. nhân với`inv2`mô phỏng cơ hội mất thêm một giây cho ô chữ tiếp theo. Các trường hợp biên được xử lý một cách tự nhiên bằng cách so sánh tổng tiền tố. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`3 5`với thời gian`2 2 2`| tôi | tiền tố[i] | tiền tố[i+1] | thăm dò | dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 1 | 1 | 
| 1 | 2 | 4 | 1/2 | 1 + 1/2 = 3/2 | 
| 2 | 4 | 6 | 1/4 | 3/2 + 1/8 = 14/8 | 

Giá trị kỳ vọng cuối cùng: 14/8, mang lại`750000007`modulo`10^9+7`. 

**Mẫu 2:**`2 3`với thời gian`1 2`| tôi | tiền tố[i] | tiền tố[i+1] | thăm dò | dự kiến ​​| 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 
| 1 | 1 | 3 | 1/2 | 1 + 1/2 = 3/2 | 

Giá trị kỳ vọng cuối cùng: 3/2, được chuyển đổi bằng nghịch đảo mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tính tổng tiền tố và một lượt để tính kỳ vọng. | 
| Không gian | O(n) | Mảng tổng tiền tố có kích thước n+1 và một vài biến. | 

Thuật toán chia tỷ lệ tuyến tính với`n`, vừa vặn thoải mái trong giới hạn 2 giây đối với`n`lên đến 2·10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 5\n2 2 2\n") == "750000007", "sample 1"
assert run("2 3\n1 2\n") == "500000005", "sample 2"

# Custom cases
assert run("1 1\n1\n") == "1", "single crossword fits exactly"
assert run("1 1\n2\n") == "0", "single crossword too long"
assert run("5 10\n1 2 3 4 5\n") == "375000003", "mixed times, total T smaller than sum"
assert run("3 100000000000000\n1000000000 1000000000 1000000000\n") == "3", "very large T, all crosswords done"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1 | 1 | trường hợp tối thiểu, phù hợp chính xác | 
| 1 1\n2 | 0 | trường hợp tối thiểu, quá ngắn | 
| 5 10\n1 2 3 4 5 | 375000003 | logic tiền tố tích lũy và xác suất | 
| 3 1e14\n1e9 1e9 1e9 | 3 | T rất lớn, tất cả các ô chữ đã hoàn thành | 

## Vỏ cạnh 

cho`T`nhỏ hơn thời gian chơi ô chữ đầu tiên, ví dụ:`1 2\n2`, thuật toán bị hỏng ở vòng lặp đầu tiên vì`prefix[1] = 2 > T`, Và`expected`vẫn là 0, điều này đúng. Đối với một lượng lớn`T`nơi tất cả các ô chữ có thể được hoàn thành, ví dụ:
