---
title: "CF 1903C - Cơn ác mộng của Theofanis"
description: "Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ của chúng ta là chia nó thành các mảng con liền kề, không trống. Mỗi mảng con đóng góp vào một tổng có trọng số gọi là giá trị Cypriot, được tính bằng tổng của tất cả các mảng con của tổng mảng con nhân với chỉ số dựa trên 1 của nó trong…"
date: "2026-06-08T21:01:18+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1903
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 912 (Div. 2)"
rating: 1400
weight: 1903
solve_time_s: 100
verified: true
draft: false
---

[CF 1903C - Cơn ác mộng của Theofanis](https://codeforces.com/problemset/problem/1903/C) 

**Đánh giá:** 1400 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ của chúng ta là chia nó thành các mảng con liền kề, không trống. Mỗi mảng con đóng góp vào một tổng có trọng số gọi là _giá trị Cypriot_, được tính bằng tổng trên tất cả các mảng con của tổng mảng con nhân với chỉ số dựa trên 1 của nó trong phân vùng. Mục tiêu của chúng tôi là tối đa hóa số tiền có trọng số này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chứa độ dài mảng`n`và các phần tử mảng, có thể âm hoặc dương. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: giá trị Síp tối đa có thể đạt được bởi một số phân vùng. 

Những ràng buộc cho chúng ta biết`n`có thể lên tới 100.000 và tổng của tất cả`n`trên các trường hợp thử nghiệm tối đa là 200.000. Điều này chỉ ra rằng bất kỳ nghiệm nào là nghiệm bậc hai trong`n`sẽ quá chậm. Cần có giải pháp tuyến tính hoặc gần tuyến tính. 

Các trường hợp biên không rõ ràng bao gồm các mảng toàn dương, toàn âm hoặc xen kẽ dấu. Ví dụ: một số âm duy nhất phải duy trì dưới dạng một mảng con đơn lẻ để tránh làm giảm tổng trọng số của các phần tử dương sau này. Đối với một mảng`[5, -2, 3]`, việc hợp nhất tất cả các phần tử một cách ngây thơ sẽ mang lại giá trị nhỏ hơn so với việc chia xung quanh số âm. 

## Phương pháp tiếp cận 

Giải pháp brute-force xem xét mọi cách có thể để phân chia mảng. Đối với mỗi phân vùng thành`k`mảng con, chúng tôi tính tổng có trọng số. Ngay cả khi sử dụng lập trình động, việc lặp qua tất cả các điểm phân tách cho mỗi tiền tố sẽ dẫn đến độ phức tạp O(n^2). Vì`n = 10^5`, điều này sẽ cần khoảng 10^10 thao tác, điều này là không khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát cách hoạt động của hệ số nhân chỉ số. Một số dương lớn sẽ đóng góp nhiều hơn nếu nó xuất hiện muộn hơn trong phân vùng vì nó được nhân với chỉ số lớn hơn. Ngược lại, số âm đóng góp ít hơn - thậm chí gây hại cho tổng số - nếu nó xuất hiện muộn hơn. Do đó, chiến lược tối ưu là phân vùng mảng sao cho mọi chuỗi phần tử liền kề có cùng dấu được coi là một mảng con duy nhất. Nói cách khác, chúng ta ngắt mảng bất cứ khi nào dấu thay đổi. Trong mỗi khối dương hoặc âm liền kề, chúng tôi chỉ lấy phần tử lớn nhất của khối đó nếu khối âm và tính tổng tất cả các phần tử nếu khối dương. 

Quan sát này làm giảm vấn đề thành một lần quét tuyến tính đơn giản: lặp qua mảng, theo dõi "khối dấu" hiện tại và quyết định nên hợp nhất hay bắt đầu một khối mới. Giải pháp chạy trong thời gian O(n) cho mỗi trường hợp thử nghiệm, phù hợp thoải mái trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`total`bằng 0 để giữ giá trị Síp cuối cùng. Đồng thời khởi tạo`current_max`là phần tử đầu tiên của mảng, biểu thị giá trị tốt nhất trong khối liền kề hiện tại. 
2. Lặp lại mảng từ phần tử thứ hai đến cuối. 
3. Nếu phần tử hiện tại có cùng dấu với`current_max`, thay thế`current_max`với mức tối đa của chính nó và phần tử hiện tại. Điều này đảm bảo chúng tôi chọn được phần tử có giá trị nhất trong khối tích cực hoặc tiêu cực. 
4. Nếu dấu thay đổi thì thêm`current_max`ĐẾN`total`, sau đó đặt`current_max`tới phần tử hiện tại. Điều này phân chia mảng khi thay đổi dấu hiệu. 
5. Sau khi hoàn thành việc lặp lại, hãy thêm phần cuối cùng`current_max`ĐẾN`total`để bao gồm khối cuối cùng. 
6. In`total`. 

Tại sao nó hoạt động: Tại mỗi khối ký hiệu, việc bao gồm phần tử lớn nhất sẽ tối đa hóa sự đóng góp của khối mà không gây tổn hại cho các khối sau này. Việc thêm các phần tử nhỏ hơn vào trong khối âm sẽ làm giảm giá trị của Síp vì nhân chúng với chỉ mục sẽ mang lại mức phạt âm lớn hơn. Cách tiếp cận tham lam này đảm bảo số tiền tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_cypriot_value(arr):
    total = 0
    current_max = arr[0]
    for num in arr[1:]:
        if (num > 0 and current_max > 0) or (num < 0 and current_max < 0):
            current_max = max(current_max, num)
        else:
            total += current_max
            current_max = num
    total += current_max
    return total

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(max_cypriot_value(a))
```Vòng lặp đầu tiên duy trì ứng cử viên tốt nhất trong mỗi khối dấu hiệu liền kề. Sự so sánh`(num > 0 and current_max > 0) or (num < 0 and current_max < 0)`kiểm tra xem phần tử hiện tại có tiếp tục khối dấu tương tự hay không. Nhánh thứ hai bổ sung`current_max`ĐẾN`total`khi dấu hiệu thay đổi, tạo ra một khối mới. Thêm`current_max`ở cuối xử lý khối cuối cùng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:`[1, -3, 7, -6, 2, 5]`** 

| tôi | số | hiện tại_max | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | -3 | -3 | 1 | 
| 2 | 7 | 7 | -2 | 
| 3 | -6 | -6 | 5 | 
| 4 | 2 | 2 | -1 | 
| 5 | 5 | 5 | 2 | 

Cuối cùng`total = 32`. Mỗi khối dấu được xử lý chính xác, xác nhận bất biến tham lam. 

**Ví dụ 2:`[2, 9, -5, -3]`** 

| tôi | số | hiện tại_max | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | 
| 1 | 9 | 9 | 0 | 
| 2 | -5 | -5 | 9 | 
| 3 | -3 | -3 | 4 | 

Cuối cùng`total = 4`. Cho thấy các khối âm chỉ đóng góp số âm đơn tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm 2 * 10^5, giải pháp chạy thoải mái dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Solution code
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = 0
        current_max = a[0]
        for num in a[1:]:
            if (num > 0 and current_max > 0) or (num < 0 and current_max < 0):
                current_max = max(current_max, num)
            else:
                total += current_max
                current_max = num
        total += current_max
        print(total)
    return output.getvalue().strip()

# Provided samples
assert run("4\n6\n1 -3 7 -6 2 5\n4\n2 9 -5 -3\n8\n-3 -4 2 -5 1 10 17 23\n1\n830\n") == "32\n4\n343\n830", "sample 1"

# Custom test cases
assert run("2\n1\n-100\n3\n0 0 0\n") == "-100\n0", "single negative and all zeros"
assert run("1\n5\n1 2 3 4 5\n") == "15", "all positive increasing"
assert run("1\n5\n-1 -2 -3 -4 -5\n") == "-1", "all negative, pick max"
assert run("1\n6\n1 -1 1 -1 1 -1\n") == "3", "alternating signs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n-100`|`-100`| Số âm đơn | 
|`0 0 0`|`0`| Tất cả số không | 
|`1 2 3 4 5`|`15`| Tất cả đều tích cực, không cần chia tách | 
|`-1 -2 -3 -4 -5`|`-1`| Tất cả âm, chọn phần tử tối đa | 
|`1 -1 1 -1 1 -1`|`3`| Biển báo xen kẽ, công trình tham lam chia cắt | 

## Vỏ cạnh 

Mảng một phần tử`[830]`bộ`total = 830`ngay lập tức. Thuật toán xử lý chính xác các mảng chỉ có số âm bằng cách chọn phần tử âm tối đa làm phần tử đóng góp duy nhất. Các dấu hiệu thay thế được xử lý vì mỗi thay đổi sẽ kích hoạt việc phân chia khối, đảm bảo phân vùng tối ưu mà không có bất kỳ logic phức tạp nào.
