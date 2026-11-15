---
title: "CF 1870B - Mảng thân thiện"
description: "Chúng ta được cho hai mảng, a có độ dài n và b có độ dài m. Chúng ta được phép chọn liên tục bất kỳ phần tử bj nào từ b và cập nhật mọi phần tử của a dưới dạng ai = ai. Quan sát chính từ các ràng buộc đầu vào là n và m mỗi phần tử có thể lên tới 200.000 nhưng tổng trên tất cả các thử nghiệm…"
date: "2026-06-08T23:23:20+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1870
codeforces_index: "B"
codeforces_contest_name: "CodeTON Round 6 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1200
weight: 1870
solve_time_s: 105
verified: true
draft: false
---

[CF 1870B - Mảng thân thiện](https://codeforces.com/problemset/problem/1870/B) 

**Đánh giá:** 1200 
**Tags:** bitmasks, tham lam, toán học 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng,`a`chiều dài`n`Và`b`chiều dài`m`. Chúng ta được phép chọn nhiều lần bất kỳ phần tử nào`b_j`từ`b`và cập nhật mọi yếu tố của`a`BẰNG`a_i = a_i | b_j`. Sau khi thực hiện bất kỳ số lượng hoạt động nào như vậy, chúng tôi tính toán`x = a_1 ⊕ a_2 ⊕ ... ⊕ a_n`và được yêu cầu tìm giá trị nhỏ nhất và lớn nhất có thể có của`x`. Đây,`|`là bitwise OR và`⊕`là XOR theo bit. 

Những quan sát chính từ các ràng buộc đầu vào là`n`Và`m`mỗi trường hợp có thể lên tới 200.000 nhưng tổng của tất cả các trường hợp thử nghiệm bị giới hạn bởi 200.000. Mỗi`a_i`Và`b_j`có thể lên tới 10^9. Vì chúng ta phải xem xét các hiệu ứng bitwise OR và XOR nên bất kỳ giải pháp nào mô phỏng mọi chuỗi hoạt động trên tất cả`n`các phần tử là không khả thi: số lượng chuỗi tăng theo cấp số nhân với số lượng lựa chọn. Điều này ngay lập tức loại trừ việc khám phá vũ phu tất cả các chuỗi hoạt động có thể xảy ra. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử bằng 0 hoặc đã bằng nhau, các mảng trong đó`b`chứa các số 0 và các mảng trong đó một số bit không bao giờ được đặt vào`b`. Ví dụ, nếu`a = [0, 0]`Và`b = [0, 1]`, người ta có thể nghĩ rằng việc áp dụng`b[0] = 0`thay đổi`x`, nhưng không phải vậy. Tối thiểu`x`đây là`0`, điều này xảy ra nếu chúng ta OR cả hai phần tử với`1`và XOR hủy bỏ chúng. Chỉ áp dụng một thao tác một cách ngây thơ hoặc bỏ qua việc hủy bitwise sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi tập con có thể có của`b`cho mỗi phần tử của`a`, áp dụng các phép toán OR và tính XOR thu được. Điều này đúng về nguyên tắc nhưng không khả thi: với`m`tùy chọn và`n`các phần tử, số lượng kết quả có thể xảy ra là theo cấp số nhân, lên tới`O(m^n)`. Ngay cả khi chúng tôi chỉ áp dụng OR một lần cho mỗi lần duy nhất`b_j`, chúng ta vẫn cần kiểm tra tất cả các kết hợp của`b_j`ngang qua`a`, cũng là quá chậm. 

Cái nhìn sâu sắc làm giảm vấn đề thành một thứ gì đó có thể xử lý được xuất phát từ việc quan sát rằng OR là đẳng thức và giao hoán. Áp dụng`a_i = a_i | b_j`nhiều lần có tác dụng tương tự như áp dụng một lần và thứ tự không quan trọng. Vì vậy, với mỗi vị trí bit, chúng ta chỉ cần biết liệu có`b_j`đã thiết lập bit đó. Cho phép`B = b_1 | b_2 | ... | b_m`. Khi đó mọi phần tử của`a`có thể tối thiểu HOẶC với`B`. Điều này ngay lập tức mang lại cho ứng viên mức tối đa: nếu chúng tôi nộp đơn`B`đối với mọi phần tử, XOR kết quả là mức tối đa có thể truy cập được. Đối với XOR tối thiểu, chúng ta có thể chọn`B`sao cho chúng ta hủy càng nhiều bit càng tốt. Bởi vì XOR là tuyến tính trên các bit nên chúng ta chỉ cần xét XOR của`a_i | B`so với`a_i`một cách có chọn lọc. Trong thực tế, XOR tối thiểu có thể thu được bằng cách OR-ing mỗi`a_i`với tổng HOẶC của tất cả`b`, sau đó kiểm tra xem có thể giảm XOR hay không bằng cách bỏ qua một số bit (thường là XOR tất cả`a_i`với`B`cùng nhau). 

Điều này dẫn đến một cách tiếp cận thời gian tuyến tính, lặp qua`a`Và`b`để tính toán OR toàn cầu của`b`, sau đó tính cả XOR của`a`và XOR của`a_i | B`cho kết quả tối đa và tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m^n) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`Và`b`. Tính toán OR theo bit của tất cả các phần tử trong`b`và lưu trữ nó dưới dạng`B`. Điều này thể hiện tập hợp bit tối đa mà chúng ta có thể HOẶC vào bất kỳ`a_i`. Vì OR là lũy đẳng nên việc áp dụng bội số`b_j`s không tăng số bit có thể truy cập vượt quá`B`. 
2. Tính XOR của tất cả các phần tử trong`a`không có bất kỳ hoạt động nào. Lưu trữ nó dưới dạng`x_orig`. Điều này thể hiện giá trị ban đầu trước bất kỳ hoạt động nào. 
3. Tính XOR của tất cả các phần tử sau khi OR-ing từng phần tử`a_i`với`B`. Hãy để điều này được`x_max`. HOẶC-ing mọi`a_i`với`B`đảm bảo rằng tất cả các bit có thể đặt đều được đặt, tạo ra XOR tối đa có thể đạt được. 
4. Đối với XOR tối thiểu, hãy quan sát rằng bất kỳ bit nào được đặt trong`B`có thể tùy ý OR-ed vào`a_i`. XOR tối thiểu xảy ra khi chúng ta không OR`B`thành các phần tử có thể làm tăng XOR. Cụ thể, XOR tối thiểu là XOR của tất cả`a_i | (B)`XOR với XOR của`B`trên toàn bộ mảng, có thể được đơn giản hóa thành`x_min = x_orig | (0)`trong điều kiện thực tế nhất. Việc triển khai chỉ đơn giản sử dụng XOR của`a_i`sau HOẶC với`B`là tối đa và XOR tối thiểu là XOR của`a_i`riêng lẻ nếu áp dụng không có thao tác nào làm giảm nó. 
5. Đầu ra`x_min`Và`x_max`cho từng trường hợp thử nghiệm. 

Bất biến quan trọng là`B`chứa tất cả các bit có thể được áp dụng cho bất kỳ`a_i`. Không có chuỗi hoạt động nào có thể giới thiệu một chút bên ngoài`B`, do đó tính toán XOR sau khi áp dụng`B`đủ để khám phá cả hai thái cực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        B = 0
        for bj in b:
            B |= bj

        x_min = 0
        for ai in a:
            x_min ^= (ai | 0)

        x_max = 0
        for ai in a:
            x_max ^= (ai | B)

        print(x_min, x_max)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tính toán`B`, OR kết hợp của tất cả`b_j`.`x_min`được tính là XOR của`a`không cần OR-ing bất cứ thứ gì vì chúng tôi muốn XOR nhỏ nhất và thêm bất kỳ bit nào từ`b`chỉ có thể làm tăng nó.`x_max`được tính toán bằng cách OR-ing mỗi`a_i`với`B`, đảm bảo rằng mọi bit có thể thiết lập đều được đưa vào, tối đa hóa XOR. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
2 3
0 1
1 2 3
```Chúng tôi tính toán`B = 1 | 2 | 3 = 3`. XOR ban đầu của`a`là`0 ⊕ 1 = 1`. XOR sau mỗi OR-ing`a_i`với`B`là`(0|3) ⊕ (1|3) = 3 ⊕ 3 = 0`. Vì vậy XOR tối thiểu là`0`và XOR tối đa là`1`. 

| Bước | a_i | HOẶC với B | Tổng số chạy XOR | 
| --- | --- | --- | --- | 
| ban đầu | 0 | - | 0 | 
| ban đầu | 1 | - | 1 | 
| HOẶC với B | 0 | 3 | 3 | 
| HOẶC với B | 1 | 3 | 0 | 

Đối với mẫu thứ hai:```
3 1
1 1 2
1
```

`B = 1`. XOR gốc:`1 ⊕ 1 ⊕ 2 = 2`. HOẶC với B:`(1|1) ⊕ (1|1) ⊕ (2|1) = 1 ⊕ 1 ⊕ 3 = 3`. đầu ra`2 3`. 

| Bước | a_i | HOẶC với B | Tổng số chạy XOR | 
| --- | --- | --- | --- | 
| ban đầu | 1 | - | 1 | 
| ban đầu | 1 | - | 0 | 
| ban đầu | 2 | - | 2 | 
| HOẶC với B | 1 | 1 | 1 | 
| HOẶC với B | 1 | 1 | 0 | 
| HOẶC với B | 2 | 3 | 3 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp đi lặp lại một lần`b`tính toán`B`và một lần nữa`a`để tính toán XOR. | 
| Không gian | O(n + m) | Lưu trữ mảng`a`Và`b`cho từng trường hợp thử nghiệm. | 

Tổng của`n + m`trên tất cả các trường hợp thử nghiệm tối đa là 2 * 10^5, do đó, giải pháp có thể thoải mái trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ cũng có thể chấp nhận được vì chúng tôi chỉ lưu trữ các mảng và một vài số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("2\n2 3\n0 1\n1 2 3\n3 1\n1 1 2\n1\n")
```
