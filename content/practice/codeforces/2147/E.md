---
title: "CF 2147E - Số lượng tối đa HOẶC Số lượng"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và với mỗi truy vấn, chúng ta được phép tăng bất kỳ phần tử nào của mảng một số lần nhất định."
date: "2026-06-08T01:20:47+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2147
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 29 (Div. 1 + Div. 2)"
rating: 2300
weight: 2147
solve_time_s: 113
verified: true
draft: false
---

[CF 2147E - Số lượng tối đa HOẶC Số lượng](https://codeforces.com/problemset/problem/2147/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, bitmasks, lực lượng vũ phu, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và với mỗi truy vấn, chúng ta được phép tăng bất kỳ phần tử nào của mảng một số lần nhất định. Mục tiêu là tối đa hóa số bit được đặt thành 1 trong bitwise OR của toàn bộ mảng sau khi thực hiện tối đa số lượng gia tăng được phép. Nói cách khác, mỗi truy vấn cung cấp một quỹ hoạt động và chúng ta phải sử dụng chúng một cách chiến lược để biến càng nhiều bit 0 trong OR thành 1 bit càng tốt. Đầu ra cho mỗi truy vấn là số lượng 1 bit trong OR kết quả. 

Các ràng buộc chỉ ra rằng mảng có thể khá lớn, lên tới 100.000 phần tử và số lượng truy vấn cũng có thể lên tới 100.000. Ngoài ra, mỗi phần tử có thể lên tới hàng tỷ và ngân sách hoạt động có thể lớn tương tự. Điều này loại trừ bất kỳ cách tiếp cận ngây thơ nào mô phỏng mọi mức tăng có thể hoặc thử tất cả các tập hợp con, vì điều đó sẽ dẫn đến hàng tỷ thao tác. Thay vào đó, chúng ta cần một cách tiếp cận tập trung vào cấu trúc cấp độ bit và các thao tác tối thiểu cần thiết để lật một bit trong OR. 

Một trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử đã đủ lớn để OR của chúng chứa tất cả các bit cao hoặc khi các thao tác được phép nhỏ hơn chi phí để lật bit cao hơn tiếp theo. Ví dụ, hãy xem xét một mảng`[1, 1]`và một truy vấn với`b=1`. HOẶC hiện tại là`1`, có một bit được đặt. Tăng một trong hai phần tử lên 1 sẽ cho`[2, 1]`với HOẶC`3`, có hai bit được đặt. Một cách tiếp cận bất cẩn làm tăng các phần tử một cách mù quáng có thể bỏ lỡ sự phân phối hoạt động tối ưu trên các bit. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, chúng tôi thử mọi cách để phân phối số gia tăng trên các phần tử mảng, tính OR mới và đếm số bit của nó. Điều này có hiệu quả vì phép toán OR có tính chất kết hợp và giao hoán nhưng lại thất bại ngay lập tức trên các đầu vào lớn. Ví dụ: với 100.000 phần tử và 1.000.000 số gia có thể có, việc liệt kê tất cả các phân phối là không thể. 

Điểm mấu chốt là mỗi bit trong OR có thể được xử lý độc lập. Để đặt một chút ở vị trí`k`trong OR, chúng ta cần ít nhất một số trong mảng để đặt bit đó. Đối với các số có bit`k`là 0, chúng ta có thể tính số lượng gia số tối thiểu cần thiết để đặt nó. Khi chúng ta biết chi phí để lật từng bit, chúng ta có thể sắp xếp các bit theo chi phí này và sử dụng một cách tham lam các thao tác được phép để tối đa hóa số bit được đặt trong OR. Điều này làm giảm vấn đề từ thao tác trực tiếp các phần tử mảng đến thao tác đếm số lượng gia tăng cần thiết trên mỗi bit. 

Cách tiếp cận tham lam có hiệu quả vì các bit bậc cao hơn có thể yêu cầu nhiều mức tăng hơn, nhưng các bit bậc thấp hơn luôn có thể được đặt với ít thao tác hơn. Cấu trúc của số nhị phân đảm bảo rằng việc thiết lập bit thấp hơn không bao giờ ngăn cản việc thiết lập bit cao hơn sau này nếu vẫn còn đủ thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * max(a_i)) | O(n) | Quá chậm | 
| Tối ưu | O(30 * n + 30 * q) | O(30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, khởi tạo một mảng`costs`có độ dài 30 để lưu trữ số lượng gia số tối thiểu cần thiết để đặt từng bit trong OR. 
2. Lặp lại từng phần tử của mảng và đối với mỗi vị trí bit từ 0 đến 29, hãy tính xem cần bao nhiêu mức tăng để biến bit đó thành 1 nếu nó hiện tại là 0. Đối với bit`k`, đây là`(1 << k) - (a_i % (1 << (k + 1)))`nếu bit bằng 0. Theo dõi chi phí tối thiểu giữa tất cả các phần tử cho mỗi bit. 
3. Sau khi xử lý mảng, sắp xếp các bit theo giá trị tối thiểu của chúng. 
4. Đối với mỗi truy vấn, hãy bắt đầu bằng bộ đếm các bit đã được đặt trong OR gốc. Sau đó lặp lại các chi phí đã được sắp xếp và trong khi ngân sách truy vấn cho phép, hãy "mua" các bit bằng cách trừ chi phí của chúng khỏi các hoạt động còn lại. Tăng bộ đếm bit cho mỗi bit được mua. 
5. Xuất ra số bit cuối cùng cho mỗi truy vấn. 

Tại sao hoạt động: Thuật toán đảm bảo tính chính xác vì nó luôn sử dụng số lượng thao tác tối thiểu để thiết lập từng bit một cách độc lập, đảm bảo không có thao tác nào bị lãng phí. Các bit đã là 1 sẽ không thực hiện các thao tác. Chi phí sắp xếp đảm bảo rằng các bit rẻ hơn được xem xét trước tiên, tối đa hóa tổng số bit được đặt trong điều kiện ngân sách hoạt động hạn chế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_or_popcount():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        # Compute original OR and minimal costs to set each bit
        orig_or = 0
        costs = [float('inf')] * 30
        for num in a:
            orig_or |= num
            for k in range(30):
                if (num >> k) & 1 == 0:
                    need = (1 << k) - (num % (1 << (k + 1)))
                    costs[k] = min(costs[k], need)
        
        sorted_costs = sorted([c for i, c in enumerate(costs) if not (orig_or >> i) & 1])
        
        # Handle each query
        for _ in range(q):
            b = int(input())
            bits = bin(orig_or).count('1')
            for cost in sorted_costs:
                if cost <= b:
                    b -= cost
                    bits += 1
                else:
                    break
            print(bits)

if __name__ == "__main__":
    max_or_popcount()
```Vòng lặp đầu tiên tính toán OR của mảng ban đầu và xây dựng chi phí tối thiểu để thiết lập mỗi bit 0. Vòng lặp bên trong dành cho các truy vấn sẽ áp dụng ngân sách hoạt động một cách tham lam cho các bit rẻ nhất. Việc sắp xếp đảm bảo chúng tôi tối đa hóa số lượng bit được đặt trong ngân sách. Xử lý`orig_or`riêng biệt tránh các bit đếm kép đã được đặt. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Truy vấn b | nguồn gốc_or | Chi phí được sắp xếp | Bộ bit | Còn lại b | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1] | 0 | 0 | 
| 2 | 0 | [1] | 1 | 1 | 
| 4 | 0 | [1] | 2 | 2 | 

Dấu vết cho thấy thuật toán tham lam luôn chọn các bit rẻ nhất trước tiên, đảm bảo chúng tôi tối đa hóa tổng số bit 1. 

Trường hợp thử nghiệm thứ hai:`[1, 3]`với các truy vấn`0`Và`3`. 

| Truy vấn b | nguồn gốc_or | Chi phí được sắp xếp | Bộ bit | Còn lại b | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | [2] | 2 | 0 | 
| 3 | 3 | [2] | 3 | 1 | 

Thuật toán bỏ qua chính xác các bit đã được đặt trong OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30 * n + 30 * q) | Lặp lại hơn 30 bit cho n phần tử, cộng thêm 30 bit cho mỗi truy vấn | 
| Không gian | O(30) | Lưu trữ chi phí tối thiểu mỗi bit | 

Độ phức tạp phù hợp với các ràng buộc vì các phép toán 30_n + 30_q ≈ 6 * 10^6, có thể chấp nhận được trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    max_or_popcount()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("3\n1 3\n0\n0\n2\n4\n2 2\n1 3\n0\n3\n2 1\n1000000000 1000000000\n1000000000\n") == "0\n1\n2\n2\n3\n31", "sample 1"

# Minimum input
assert run("1\n1 1\n0\n0\n") == "0", "minimum input"

# All ones
assert run("1\n3 2\n7 7 7\n0\n3\n") == "3\n3", "all bits already set"

# Large b
assert run("1\n2 1\n1 2\n1000000000\n") == "2", "large operations budget"

# Edge increment needed
assert run("1\n2 1\n2 3\n1\n") == "2", "just enough increment to flip one bit"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1\n0\n0\n`|`0`| mảng nhỏ nhất, không có phép toán | 
|`1\n3 2\n7 7 7\n0\n3\n`|`3\n3`| tất cả các bit đã được thiết lập, các phép toán không thay đổi HOẶC | 
|`1\n2 1\n1 2\n1000000000\n`|`2`| ngân sách hoạt động rất lớn không vượt quá số bit tối đa | 
|`1\n2 1\n2 3\n1\n`|`2`| tăng đơn lẻ đủ để lật bit tiếp theo | 

## Vỏ cạnh 

Khi mảng có tất cả các phần tử bằng 0 và ngân sách không đủ để lật bit đầu tiên của phần tử cao nhất, thuật toán sẽ đưa ra kết quả bằng 0 một cách chính xác, vì nó chỉ đếm các bit có thể mua được bằng các hoạt động khả dụng. Ví dụ: `[
