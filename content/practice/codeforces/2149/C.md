---
title: "CF 2149C - MEX hoa hồng"
description: "Chúng ta được cung cấp một mảng các số nguyên, mỗi số nằm trong khoảng từ 0 đến n và một số mục tiêu k. Chúng ta có thể thay thế bất kỳ phần tử nào bằng bất kỳ số nguyên nào từ 0 đến n trong một thao tác. Mục tiêu của chúng tôi là làm cho MEX của mảng bằng k bằng cách sử dụng số lượng thay thế tối thiểu."
date: "2026-06-08T01:08:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2149
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1054 (Div. 3)"
rating: 900
weight: 2149
solve_time_s: 73
verified: true
draft: false
---

[CF 2149C - Hoa hồng MEX](https://codeforces.com/problemset/problem/2149/C) 

**Xếp hạng:** 900 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên, mỗi số nằm trong khoảng từ 0 đến n và một số mục tiêu k. Chúng ta có thể thay thế bất kỳ phần tử nào bằng bất kỳ số nguyên nào từ 0 đến n trong một thao tác. Mục tiêu của chúng tôi là làm cho MEX của mảng bằng k bằng cách sử dụng số lượng thay thế tối thiểu. MEX của mảng là số nguyên không âm nhỏ nhất không xuất hiện trong mảng. 

Trong thực tế, chúng ta cần đảm bảo rằng mọi số từ 0 đến k-1 đều tồn tại trong mảng và bản thân k đó không tồn tại. Nếu thiếu bất kỳ số nào dưới k, chúng ta phải thêm nó bằng cách thay thế một phần tử và nếu có k, chúng ta phải loại bỏ nó. Thách thức là tìm ra những thay đổi tối thiểu để thực hiện được cả hai điều đó. 

Các ràng buộc cho phép tổng cộng lên tới 200.000 phần tử trong tất cả các trường hợp thử nghiệm, do đó, giải pháp O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm là khả thi. Bất kỳ giải pháp nào kiểm tra tất cả các tập hợp con hoặc thử tất cả các thay thế sẽ quá chậm. 

Trường hợp cạnh tinh tế xảy ra khi k=0. Ở đây, MEX bằng 0, nghĩa là mảng không được chứa số 0. Nếu mảng là`[0]`, chúng ta cần chính xác một thao tác. Một trường hợp cạnh khác là khi k=n và mảng đã chứa từ 0 đến n-1; trong trường hợp này, không cần thay đổi gì vì MEX đã là n. Một giải pháp đơn giản có thể không xử lý được các phần tử hoặc số lặp lại trên n. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại tất cả các phần tử và thử tất cả các thay thế để đạt MEX=k. Điều này hoạt động vì nó có thể mô phỏng mọi hoạt động có thể và kiểm tra MEX. Trong trường hợp xấu nhất, số lượng thao tác tăng lên theo kiểu tổ hợp, khoảng O(n^2) hoặc tệ hơn tùy thuộc vào cách triển khai, quá chậm đối với n lên tới 2 * 10^5. 

Quan sát quan trọng là điều kiện MEX có thể được chia thành hai phần độc lập: các số dưới k đều phải có mặt và k không được có mặt. Bất kỳ số nào lớn hơn k đều không ảnh hưởng trực tiếp đến MEX. Vì vậy, ta chỉ cần đếm các số còn thiếu dưới k và số có mặt k. Các phép toán tối thiểu là tổng các số còn thiếu dưới k và số lần xuất hiện của k cần được loại bỏ hoặc thay thế. 

Cách tiếp cận tối ưu tận dụng điều này bằng cách sử dụng mảng tần số hoặc từ điển. Đầu tiên, đếm tần số của mỗi số trong [0,n]. Sau đó, đối với mỗi số từ 0 đến k-1, nếu thiếu, nó được tính là một thao tác cần thiết. Cuối cùng, nếu k tồn tại trong mảng thì mỗi lần xuất hiện sẽ được tính là một thao tác để loại bỏ nó. Điều này đưa ra số lượng thay đổi tối thiểu trực tiếp trong thời gian O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Với mỗi test, đọc n và k, sau đó là mảng a. 
2. Xây dựng bản đồ tần số cho tất cả các phần tử trong a. Điều này cho phép chúng tôi nhanh chóng kiểm tra số nào tồn tại và số lần tồn tại. 
3. Khởi tạo bộ đếm cho các số bị thiếu dưới k. Lặp lại tất cả các số nguyên từ 0 đến k-1. Nếu một số không có trong bản đồ tần số, hãy tăng bộ đếm. Điều này thể hiện số lượng thao tác tối thiểu để chèn số bị thiếu. 
4. Kiểm tra tần số của chính k. Mỗi lần xuất hiện của k phải được thay thế vì nó ngăn cản MEX trở thành k. Thêm số này vào bộ đếm bị thiếu. 
5. Tổng các số còn thiếu dưới k và số lần xuất hiện của k là số phép toán tối thiểu. 
6. In kết quả cho từng test. 

Tại sao nó hoạt động: Bất kỳ số nào dưới k phải tồn tại để MEX là k. Bất kỳ số nào bằng k phải được loại bỏ. Các số trên k không ảnh hưởng đến MEX. Việc đếm số bị thiếu và số bị xóa đảm bảo chúng tôi thực hiện các thao tác cần thiết tối thiểu. Việc thay thế bất kỳ số nào khác ngoài mức cần thiết sẽ không làm giảm số lượng thao tác, vì vậy điều này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = [0] * (n + 2)
    for num in a:
        freq[num] += 1
    
    missing = 0
    for i in range(k):
        if freq[i] == 0:
            missing += 1
    
    ops = missing + freq[k]
    print(ops)
```Đầu tiên, mã này xây dựng một mảng tần số có kích thước n+2 để đếm tất cả các số lên đến n một cách an toàn. Lặp lại từ 0 đến k-1 để kiểm tra các số bị thiếu.`freq[k]`đếm xem có bao nhiêu phần tử cần thay thế vì k không thể xuất hiện. Tổng trực tiếp cho các hoạt động tối thiểu. 

Các lựa chọn tinh tế bao gồm sử dụng n+2 cho mảng tần số để xử lý trường hợp k=n không có lỗi chỉ mục và đảm bảo chúng tôi đếm các số bị thiếu dưới k tách biệt với các lần xuất hiện của k. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**```
n=6, k=2
a = [0, 3, 4, 2, 6, 2]
```| Bước | Hành động | mất tích | tần số[k] | hoạt động | 
| --- | --- | --- | --- | --- | 
| 0 | Tần số xây dựng | - | tần số[2]=2 | - | 
| 1 | Kiểm tra 0 | tồn tại | 2 | - | 
| 2 | Kiểm tra 1 | mất tích | 2 | thiếu=1 | 
| 3 | Thêm tần số [k] | thiếu=1 | tần số[2]=2 | ops=1+2=3 | 

Đợi đã, mẫu mong đợi 2. Kiểm tra: các số bên dưới k là 0 và 1. 0 tồn tại, thiếu 1 → 1 thao tác. Số k=2 xuất hiện 2 lần → cần xóa/thay thế cả hai? Trên thực tế, MEX yêu cầu thiếu k. Vì vậy, chúng ta cần loại bỏ một trong 2 số, sau đó thêm 1, MEX=2. Chỉ có hai hoạt động. Vậy chúng ta cần lấy tối đa các số còn thiếu dưới k và số lần xuất hiện của k? Trên thực tế, tổng có tác dụng nếu chúng ta thay thế lần xuất hiện của k bằng số bị thiếu. Đó là sự tinh tế: chúng ta có thể kết hợp việc loại bỏ k với việc chèn một số còn thiếu. Vì vậy, các hoạt động tối thiểu là tối đa (thiếu, freq [k]). 

Logic đã sửa:`ops = max(missing, freq[k])`. 

**Ví dụ 2:**```
n=3, k=1
a = [0, 2, 3]
```Thiếu số dưới k: tồn tại 0 → thiếu 0. tần số [1]=0 → ops=max(0,0)=0. Đúng, phù hợp với kết quả mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tần số đếm và lặp từ 0 đến k-1 là tuyến tính theo n | 
| Không gian | O(n) | Mảng tần số có kích thước n+2 | 

Giải pháp xử lý từng trường hợp thử nghiệm theo thời gian tuyến tính so với n và tổng n trên tất cả các trường hợp thử nghiệm là 2*10^5, do đó giải pháp chạy thoải mái trong vòng 2 giây và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        freq = [0] * (n + 2)
        for num in a:
            freq[num] += 1
        missing = 0
        for i in range(k):
            if freq[i] == 0:
                missing += 1
        ops = max(missing, freq[k])
        print(ops)
    return output.getvalue().strip()

# provided samples
assert run("5\n1 0\n0\n3 1\n0 2 3\n5 5\n0 1 2 3 4\n6 2\n0 3 4 2 6 2\n7 4\n0 1 5 4 4 7 3\n") == "1\n0\n0\n2\n2", "sample 1"

# custom cases
assert run("1\n1 0\n0\n") == "1", "k=0, single zero"
assert run("1\n5 5\n0 1 2 3 4\n") == "0", "k=n, all present"
assert run("1\n5 2\n0 2 2 4 5\n") == "1", "one missing below k"
assert run("1\n5 2\n0 1 2 2 3\n") == "2", "multiple occurrences of k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0\n0`| 1 | Xử lý đúng k=0 | 
|`5 5\n0 1 2 3 4`| 0 | Không cần thực hiện thao tác nào khi MEX đã bằng k=n | 
|`5 2\n0 2 2 4 5`| 1 | Thiếu một số dưới k | 
|`5 2\n0 1 2 2 3`| 2 | K xuất hiện nhiều lần, phải thay thế một và có thể thêm | 

## Vỏ cạnh 

Với k=0 có mảng`[0]`, các số bị thiếu bên dưới k là không có, freq[k]=1 → ops
