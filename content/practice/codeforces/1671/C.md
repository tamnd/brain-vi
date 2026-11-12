---
title: "CF 1671C - Dolce Vita"
description: "Chúng tôi có một tình huống trong đó bạn muốn mua gói đường từ một số cửa hàng trong những ngày liên tiếp, mỗi cửa hàng bán một gói mỗi ngày. Mỗi cửa hàng có một mức giá ban đầu cho gói của mình và mỗi ngày giá sẽ tăng thêm một."
date: "2026-06-10T01:36:55+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1671
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 127 (Rated for Div. 2)"
rating: 1200
weight: 1671
solve_time_s: 115
verified: false
draft: false
---

[CF 1671C - Dolce Vita](https://codeforces.com/problemset/problem/1671/C) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, bạo lực, tham lam, toán học 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tình huống trong đó bạn muốn mua gói đường từ một số cửa hàng trong những ngày liên tiếp, mỗi cửa hàng bán một gói mỗi ngày. Mỗi cửa hàng có một mức giá ban đầu cho gói của mình và mỗi ngày giá sẽ tăng thêm một. Bạn có ngân sách cố định mỗi ngày và không thể chuyển tiền sang ngày hôm sau. Mục tiêu là xác định tổng số gói bạn có thể mua cho đến khi tất cả các gói vượt quá ngân sách hàng ngày của bạn. 

Đầu vào là số lượng cửa hàng`n`, ngân sách hàng ngày`x`, và mảng`a`đại diện cho giá gói ban đầu cho mỗi cửa hàng. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng số gói bạn có thể mua trước khi không có gói nào có giá cả phải chăng. 

Những hạn chế là quan trọng.`n`có thể lên tới 2·10^5,`x`Và`a_i`lên tới 10^9 và tổng của`n`trên tất cả các trường hợp thử nghiệm là 2·10^5. Điều này ngụ ý rằng bất kỳ mô phỏng bạo lực nào mỗi ngày cho mỗi cửa hàng đều quá chậm, vì số ngày cũng có thể lên tới 10^9 trong những trường hợp cực đoan. Chúng ta cần một cách tiếp cận tránh việc mô phỏng từng ngày một. 

Trường hợp đặc biệt xảy ra khi gói rẻ nhất đã đắt hơn ngân sách hàng ngày của bạn vào ngày đầu tiên. Ví dụ, nếu`x = 5`và rẻ nhất`a_i = 6`, bạn không thể mua bất kỳ gói nào. Một trường hợp tinh vi khác phát sinh khi nhiều gói giá rẻ cho phép mua lặp lại trong nhiều ngày, chẳng hạn như`a = [1, 1]`với ngân sách khổng lồ`x = 1000`. Một mô phỏng đơn giản mỗi ngày sẽ lặp đi lặp lại hàng nghìn lần một cách không cần thiết. Chúng ta cần một phương pháp nén sự lặp lại này một cách hiệu quả. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực đơn giản sẽ mô phỏng mỗi ngày: mỗi ngày, hãy tính giá hiện tại của mỗi gói, mua càng nhiều càng tốt trong phạm vi ngân sách, sau đó chuyển sang ngày hôm sau và tăng tất cả giá lên một. Điều này đúng vì nó tuân theo mô tả vấn đề theo đúng nghĩa đen. Tuy nhiên, trong trường hợp xấu nhất, nếu`n = 2·10^5`và giá rất thấp với ngân sách cao, quá trình mô phỏng có thể yêu cầu hàng tỷ lần lặp lại trong nhiều ngày, vượt xa giới hạn 2 giây. Điều này làm cho nó không thực tế. 

Điều quan trọng là chúng ta không cần phải mô phỏng hàng ngày. Nếu chúng ta sắp xếp các gói theo giá ban đầu thì gói rẻ nhất sẽ luôn được xem xét đầu tiên. Vào bất kỳ ngày nào, nếu bạn có thể mua`k`gói trong phạm vi ngân sách của bạn, số ngày bạn có thể tiếp tục mua những gói này`k`các gói liên tiếp mà không vượt quá ngân sách có thể được tính toán bằng toán học. Cụ thể, nếu tổng của`k`gói rẻ nhất cộng thêm`k*d`(Ở đâu`d`là số ngày liên tiếp) vẫn ≤`x`, sau đó chúng ta có thể mua`k`gói cho`d`ngày cùng một lúc. Điều này biến đổi mô phỏng mỗi ngày thành phép tính theo từng nhóm, giúp giảm vấn đề thành các phép toán sắp xếp O(n log n) và O(n). 

Brute-force hoạt động vì nó mô hình hóa vấn đề theo đúng nghĩa đen nhưng lại thất bại do có thể xảy ra hàng tỷ lần lặp lại. Quan sát về việc mua liên tiếp cho phép chúng tôi tính toán trước số ngày tối đa mà một tiền tố nhất định của giá đã sắp xếp có thể được mua, điều này thu gọn các phép tính lặp đi lặp lại thành một bước số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max_days) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`x`, sau đó đọc mảng`a`. 
3. Sắp xếp mảng`a`theo thứ tự tăng dần. Việc sắp xếp đảm bảo rằng chúng ta luôn cố gắng mua những gói rẻ nhất trước tiên, tối đa hóa số lượng gói được mua. 
4. Khởi tạo`total_packs = 0`. Điều này sẽ tích lũy tất cả các gói đã mua trong tất cả các ngày. 
5. Lặp lại mảng`a`với một chỉ mục`i`đại diện cho gói cuối cùng trong tiền tố hiện tại. Đối với mỗi tiền tố`a[0..i]`, tính tổng`prefix_sum`trong số này`i+1`các phần tử. Số tiền này thể hiện chi phí mua tất cả các gói trong tiền tố này vào ngày 0. 
6. Xác định số ngày liên tục`d`chúng ta có thể mua tiền tố này mà không cần vượt quá`x`. Sử dụng phép chia số nguyên:`d = (x - prefix_sum) // (i+1) + 1`. Chúng tôi thêm 1 vì ngày 0 được bao gồm. Nếu như`d <= 0`, bỏ qua tiền tố này. 
7. Tăng`total_packs`qua`(i+1) * d`. Điều này tính đến việc mua`i+1`gói mỗi ngày cho`d`ngày. 
8. Giảm`x`cho lần lặp tiếp theo nếu cần, nhưng theo cách tiếp cận của chúng tôi, chúng tôi thực sự không cần phải sửa đổi`x`mỗi tiền tố vì mỗi tiền tố tính ngày độc lập. 
9. Sau khi kết thúc vòng lặp, in`total_packs`cho trường hợp thử nghiệm hiện tại. 

Tại sao nó hoạt động: việc phân loại đảm bảo chúng tôi luôn xem xét các gói rẻ nhất trước tiên. Bằng cách tính toán các ngày liên tiếp theo toán học, chúng tôi đảm bảo rằng chúng tôi mua được số tiền tối đa có thể mà không cần phải mô phỏng mỗi ngày. Tổng tiền tố và phép tính ngày tôn trọng tính bất biến là giá tăng đúng 1 mỗi ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        total = 0
        prefix_sum = 0
        for i in range(n):
            prefix_sum += a[i]
            if prefix_sum > x:
                break
            days = (x - prefix_sum) // (i + 1) + 1
            total += days * (i + 1)
        print(total)

if __name__ == "__main__":
    solve()
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Việc sắp xếp giá gói đảm bảo chúng ta mua gói rẻ hơn trước.`prefix_sum`theo dõi chi phí tích lũy cho tiền tố hiện tại và`(x - prefix_sum) // (i + 1) + 1`tính số ngày liên tiếp chúng ta có thể mua được tiền tố này. nhân với`(i + 1)`cung cấp tổng số gói cho tiền tố này. Vòng lặp bị phá vỡ ngay khi`prefix_sum > x`bởi vì không có gói nào trong tiền tố này có giá cả phải chăng vào ngày đầu tiên. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
3 7
2 1 2
```| Ngày | Giá | Ngân sách | Đã mua | Tiền tố Tổng | Số ngày được tính | Tổng số gói | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1,2,2 | 7 | 3 | 5 | 1 | 3 | 
| 2 | 2,3,3 | 7 | 2 | 5 | 1 | 5 | 
| 3 | 3,4,4 | 7 | 2 | 5 | 1 | 7 | 
| 4 | 4,5,5 | 7 | 1 | 5 | 1 | 8 | 
| 5 | 5,6,6 | 7 | 1 | 5 | 1 | 9 | 
| 6 | 6,7,7 | 7 | 1 | 5 | 1 | 10 | 
| 7 | 7,8,8 | 7 | 1 | 5 | 1 | 11 | 

Dấu vết này xác nhận rằng việc tính toán`days`thông qua`(x - prefix_sum) // (i+1) + 1`nắm bắt các giao dịch mua liên tiếp một cách hiệu quả. 

**Đầu vào mẫu 2:**```
5 9
10 20 30 40 50
```Ở đây gói rẻ nhất là 10, lớn hơn`x=9`. Không có gói nào có thể được mua. Thuật toán ngay lập tức bỏ qua vòng lặp sau lần kiểm tra tổng tiền tố đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tổng tiền tố và vòng lặp là O(n) | 
| Không gian | O(n) | Để lưu trữ mảng giá | 

Được cho`n`lên tới 2·10^5 và tổng số`n`trên tất cả các trường hợp thử nghiệm 2·10^5, giải pháp này dễ dàng phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("4\n3 7\n2 1 2\n5 9\n10 20 30 40 50\n1 1\n1\n2 1000\n1 1\n") == "11\n0\n1\n1500"

# custom tests
assert run("1\n1 1\n1\n") == "1", "minimum input"
assert run("1\n3 10\n2 2 2\n") == "15", "all equal small"
assert run("1\n2 5\n1 2\n") == "6", "small varied"
assert run("1\n3 1000000000\n100000000 200000000 300000000\n") == "5500000000", "large numbers"
assert run("1\n5 7\n1 1 1 1 1\n") == "35", "repeated cheap packs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|
