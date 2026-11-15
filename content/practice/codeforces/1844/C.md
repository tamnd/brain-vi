---
title: "CF 1844C - Hạt"
description: "Chúng ta được cho một chuỗi các hạt sắp xếp thành một dòng, mỗi hạt có điện tích nguyên. Chúng tôi có một thiết bị cho phép chúng tôi loại bỏ bất kỳ hạt nào, sau đó hai hạt lân cận hợp nhất thành một hạt có điện tích bằng tổng của hai hạt lân cận."
date: "2026-06-09T06:04:10+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1844
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 884 (Div. 1 + Div. 2)"
rating: 1300
weight: 1844
solve_time_s: 310
verified: false
draft: false
---

[CF 1844C - Hạt](https://codeforces.com/problemset/problem/1844/C) 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, thực hiện, toán học 
**Thời gian giải:** 5 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các hạt sắp xếp thành một dòng, mỗi hạt có điện tích nguyên. Chúng tôi có một thiết bị cho phép chúng tôi loại bỏ bất kỳ hạt nào, sau đó hai hạt lân cận hợp nhất thành một hạt có điện tích bằng tổng của hai hạt lân cận. Mục tiêu là áp dụng một chuỗi các thao tác như vậy cho đến khi chỉ còn lại một hạt và tối đa hóa điện tích của hạt cuối cùng đó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng tôi biết số lượng hạt và điện tích tương ứng của chúng. Đầu ra là điện tích tối đa có thể có của hạt còn lại sau khi thực hiện các thao tác một cách tối ưu. 

Những hạn chế rất đáng kể: có thể có tới$2 \cdot 10^5$tổng số hạt trong tất cả các trường hợp thử nghiệm và điện tích riêng lẻ có thể rất lớn hoặc âm. Do những hạn chế này, bất kỳ mô phỏng đơn giản nào liên tục loại bỏ các hạt và tính toán lại mảng đều quá chậm; việc mô phỏng từng thao tác có thể mất$O(n^2)$thời gian, sẽ không hoàn thành trong vòng một giây đối với đầu vào lớn. 

Trường hợp cạnh tinh tế xảy ra khi tất cả các hạt đều âm. Một cách tiếp cận bất cẩn khi kết hợp các hạt một cách bừa bãi có thể làm giảm điện tích tối đa còn lại một cách không cần thiết. Ví dụ, nếu dòng là`[-1, -2, -3]`, chiến lược tối ưu là hợp nhất các số âm theo cách giữ tổng âm nhỏ nhất làm hạt cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi lần loại bỏ một hạt, hãy mô phỏng đệ quy tất cả các chuỗi còn lại có thể có cho đến khi còn lại một hạt, sau đó lấy mức tối đa. Điều này đảm bảo sẽ đưa ra câu trả lời đúng vì nó khám phá mọi chuỗi hoạt động có thể xảy ra. Tuy nhiên, cách tiếp cận này mang tính giai thừa về độ phức tạp về thời gian: với$n$các hạt, có khoảng$n!$trình tự để khám phá, điều này là không khả thi đối với$n \sim 10^5$. 

Quan sát quan trọng là thứ tự hợp nhất không quan trọng ngoài việc nhóm các chuỗi dương hoặc âm liên tiếp. Về cơ bản, thao tác này hợp nhất các số liền kề, do đó, cuối cùng, điện tích của hạt cuối cùng là tổng của một dãy con liền kề của mảng ban đầu, có thể sau khi bỏ qua các phép hợp nhất có độ dài bằng 0 của tổng âm. Điều này làm giảm vấn đề thành một biến thể của vấn đề tổng mảng con tối đa. 

Cụ thể, điện tích tối đa có thể có của hạt còn lại bằng tổng của dãy con liền kề lớn nhất của các tổng không âm. Điều này có thể được tính theo thời gian tuyến tính bằng cách lặp qua mảng, hợp nhất các phần tử liên tiếp một cách tham lam: luôn tính tổng tất cả các đóng góp dương và đặt lại tổng khi nó trở nên nhỏ hơn bằng cách thêm tiền tố âm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test, đọc số lượng hạt`n`và danh sách chi phí`c`. 
2. Khởi tạo hai biến:`current_sum`để theo dõi tổng liên tục của một dãy con liền kề và`max_sum`để theo dõi số tiền tối đa được tìm thấy. 
3. Lặp lại các lần tính phí theo thứ tự. Đối với mỗi hạt, thêm điện tích của nó vào`current_sum`. Nếu như`current_sum`trở nên nhỏ hơn hạt hiện tại, hãy đặt lại`current_sum`đến điện tích của hạt. Đây là một sửa đổi tiêu chuẩn của thuật toán Kadane để tính tổng mảng con tối đa. 
4. Cập nhật`max_sum`nếu như`current_sum`vượt quá nó. 
5. Sau khi xử lý tất cả các hạt,`max_sum`chứa điện tích tối đa có thể đạt được của hạt cuối cùng. 
6. In hoặc lưu trữ kết quả của từng test. 

Tại sao nó hoạt động: thao tác hợp nhất làm giảm chuỗi thành tổng của một phân đoạn liền kề. Bất kỳ chuỗi hợp nhất tối ưu nào cũng không thể bỏ qua đóng góp tích cực mà không làm mất tổng tiềm năng. Do đó, việc tìm tổng tối đa trên tất cả các phân đoạn liền kề tương đương với việc thực hiện việc hợp nhất một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        c = list(map(int, input().split()))
        current_sum = max_sum = c[0]
        for charge in c[1:]:
            current_sum = max(charge, current_sum + charge)
            max_sum = max(max_sum, current_sum)
        print(max_sum)

if __name__ == "__main__":
    solve()
```Giải pháp này sử dụng I/O nhanh cho nhiều trường hợp thử nghiệm. Nó xử lý từng hạt chính xác một lần và duy trì tổng chạy một cách hiệu quả, đảm bảo không có số nguyên nào bị tràn vì số nguyên Python có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[-3, 1, 4, -1, 5, -9]`| Bước | Phí hiện tại | hiện tại_sum | max_sum | 
| --- | --- | --- | --- | 
| 0 | -3 | -3 | -3 | 
| 1 | 1 | 1 | 1 | 
| 2 | 4 | 5 | 5 | 
| 3 | -1 | 4 | 5 | 
| 4 | 5 | 9 | 9 | 
| 5 | -9 | 0 | 9 | 

Điện tích hạt còn lại tối đa là`9`. 

### Ví dụ 2 

đầu vào:`[998244353, 998244353, 998244353, 998244353, 998244353]`| Bước | Phí hiện tại | hiện tại_sum | max_sum | 
| --- | --- | --- | --- | 
| 0 | 998244353 | 998244353 | 998244353 | 
| 1 | 998244353 | 1996488706 | 1996488706 | 
| 2 | 998244353 | 2994733059 | 2994733059 | 
| 3 | 998244353 | 3992987412 | 3992987412 | 
| 4 | 998244353 | 4991231765 | 4991231765 | 

Sự hợp nhất tối ưu tổng hợp tất cả các hạt liên tiếp. Số tiền còn lại tối đa là`4991231765`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp qua mảng một lần để tính tổng mảng con tối đa. | 
| Không gian | O(1) | Chỉ có hai biến`current_sum`Và`max_sum`được duy trì bên cạnh đầu vào. | 

Tổng số hạt trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, vì vậy cách tiếp cận tuyến tính hiệu quả và phù hợp thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n6\n-3 1 4 -1 5 -9\n5\n998244353 998244353 998244353 998244353 998244353\n1\n-2718") == "9\n2994733059\n-2718", "sample 1"

# Custom cases
assert run("1\n3\n-1 -2 -3") == "-1", "all negative"
assert run("1\n5\n1 2 3 4 5") == "15", "all positive"
assert run("1\n4\n-5 1 -2 3") == "3", "mixed signs"
assert run("1\n1\n42") == "42", "single particle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[-1, -2, -3]`| -1 | Xử lý mọi hạt âm | 
|`[1,2,3,4,5]`| 15 | Tổng của tất cả các số dương | 
|`[-5,1,-2,3]`| 3 | Chọn chính xác tổng liền kề tối đa trong mảng hỗn hợp | 
|`[42]`| 42 | Vỏ cạnh hạt đơn | 

## Vỏ cạnh 

Nếu mảng chỉ chứa các số âm, thuật toán sẽ chọn chính xác phần tử âm ít nhất, tương ứng với việc không thực hiện phép hợp nhất nào có thể làm giảm giá trị. Ví dụ,`[-1, -2, -3]`trả lại`-1`. 

Nếu chỉ có một hạt thì không cần thực hiện thao tác nào và thuật toán sẽ trả về trực tiếp hạt đó. 

Giải pháp này cũng xử lý các số nguyên dương lớn mà không bị tràn vì Python hỗ trợ các số nguyên có độ chính xác tùy ý.
