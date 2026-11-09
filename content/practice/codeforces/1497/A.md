---
title: "CF 1497A - Mexim hóa"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta muốn sắp xếp lại nó để tối đa hóa tổng giá trị MEX trên tất cả các tiền tố. Tiền tố là bất kỳ phân đoạn ban đầu nào của mảng. MEX của một tập hợp là số nguyên không âm nhỏ nhất không có trong tập hợp đó."
date: "2026-06-10T21:48:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1497
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 708 (Div. 2)"
rating: 800
weight: 1497
solve_time_s: 378
verified: false
draft: false
---

[CF 1497A - Meximization](https://codeforces.com/problemset/problem/1497/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 6 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và chúng ta muốn sắp xếp lại nó để tối đa hóa tổng giá trị **MEX** trên tất cả các tiền tố. Tiền tố là bất kỳ phân đoạn ban đầu nào của mảng. **MEX** của một tập hợp là số nguyên không âm nhỏ nhất không có trong tập hợp đó. Ví dụ: MEX của`{0, 1, 3}`là`2`, bởi vì`0`Và`1`có mặt nhưng`2`bị thiếu. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp kích thước mảng`n`(tối đa 100) và chính các phần tử mảng (tất cả nằm trong khoảng từ 0 đến 100). Kích thước nhỏ của`n`ngụ ý rằng một thuật toán có độ phức tạp O(n²) có thể chạy thoải mái dưới 1 giây, nhưng chúng ta nên hướng tới thứ gì đó đơn giản và trực tiếp hơn vì vấn đề có cấu trúc tham lam rõ ràng. 

Các trường hợp cạnh không rõ ràng bao gồm mảng bị thiếu`0`, mảng trong đó tất cả các phần tử đều bằng nhau hoặc mảng trong đó một số số xuất hiện nhiều lần. Ví dụ, nếu mảng là`[2, 2, 3]`, một cách tiếp cận đơn giản có thể thử bắt đầu với các phần tử lớn nhất để cực đại hóa MEX, nhưng điều này không thành công vì MEX rất nhạy cảm với việc thiếu các số nhỏ. Đầu ra đúng nên ưu tiên bắt đầu từ`0`nếu nó tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của mảng, tính MEX cho từng tiền tố và tính tổng chúng. Điều này hoạt động về nguyên tắc, nhưng số lượng hoán vị là`n!`phát nổ ngay cả đối với`n=10`, làm cho điều này hoàn toàn không thực tế. 

Quan sát quan trọng là MEX chỉ tăng khi chúng ta đưa vào số còn thiếu nhỏ nhất. Vì vậy, để tối đa hóa tổng số MEX tiền tố, trước tiên chúng ta nên bao gồm tất cả các số riêng biệt bắt đầu từ`0`trở lên, theo thứ tự. Once the first occurrence of each number`0,1,2,...`được đặt, MEX của tiền tố sẽ tăng dần. 

Sau lần xuất hiện đầu tiên, các phần tử còn lại có thể được thêm vào theo bất kỳ thứ tự nào vì chúng không còn làm tăng MEX nữa. Chiến lược tham lam đơn giản này tạo ra một giải pháp chính xác và hiệu quả vì MEX ở mỗi tiền tố được xác định hoàn toàn bởi tập hợp các số đã được đặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Tham lam do lần đầu xuất hiện | O(n + max(a)) | O(max(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần xuất hiện của từng số trong mảng. Chúng ta cần biết số nào hiện diện ít nhất một lần. 
2. Khởi tạo một mảng kết quả trống. 
3. Vòng lặp từ`0`trở lên và nối thêm lần xuất hiện đầu tiên của mỗi số tồn tại trong mảng. Đối với mỗi số được nối thêm, hãy giảm số lượng của nó. Điều này đảm bảo rằng tiền tố MEX tăng đều đặn thêm 1 ở mỗi bước. 
4. Một khi tất cả các số từ`0`đến mức hiện tại tối đa đã được xử lý một lần, nối các số còn lại theo thứ tự bất kỳ. Chúng không góp phần tăng MEX mà hoàn thiện mảng. 
5. Xuất mảng kết quả. 

**Tại sao nó hoạt động**: Ở mỗi bước, chúng tôi đảm bảo rằng MEX của tiền tố hiện tại được tối đa hóa bằng cách bao gồm số còn thiếu nhỏ nhất trước tiên. Khi tiền tố chứa tất cả các số từ`0`ĐẾN`k-1`, MEX trở thành`k`. Việc thêm các bản sao sau đó không thể tăng MEX nên đặt chúng ở cuối là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    count = [0] * 102  # numbers are 0..100
    for x in a:
        count[x] += 1
    
    result = []
    # step 1: include each number once to increase MEX
    for i in range(102):
        if count[i]:
            result.append(i)
            count[i] -= 1
    
    # step 2: append remaining numbers in any order
    for i in range(102):
        while count[i]:
            result.append(i)
            count[i] -= 1
    
    print(' '.join(map(str, result)))
```Vòng lặp đầu tiên đảm bảo rằng mỗi tiền tố tăng MEX một cách tối ưu. Vòng lặp thứ hai xử lý các số còn sót lại. Mảng`count`đủ lớn để bao gồm tất cả các số đầu vào và ngăn ngừa lỗi chỉ mục. Việc xử lý ranh giới là ngầm định vì tất cả các số đều 100. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

Mảng đầu vào:`[4, 2, 0, 1, 3, 3, 7]`| Bước | Hành động | Mảng kết quả | MEX của Tiền tố | 
| --- | --- | --- | --- | 
| 1 | Nối 0 | [0] | 1 | 
| 2 | Nối 1 | [0,1] | 2 | 
| 3 | Nối 2 | [0,1,2] | 3 | 
| 4 | Nối 3 | [0,1,2,3] | 4 | 
| 5 | Nối 4 | [0,1,2,3,4] | 5 | 
| 6 | Nối 7 | [0,1,2,3,4,7] | 5 | 
| 7 | Nối 3 còn lại | [0,1,2,3,4,7,3] | 5 | 

Trình tự này khớp với đầu ra mẫu và xác nhận logic tham lam. 

**Mẫu 2:** 

Mảng đầu vào:`[2, 2, 8, 6, 9]`| Bước | Hành động | Mảng kết quả | MEX của Tiền tố | 
| --- | --- | --- | --- | 
| 1 | Nối 0 (thiếu) | bỏ qua | MEX=0 | 
| 2 | Nối 1 (thiếu) | bỏ qua | MEX=0 | 
| 3 | Nối 2 | [2] | 0 | 
| 4 | Nối các số còn lại 2,6,8,9 | [2,6,8,9,2] | 1 | 

Ở đây MEX chỉ tăng khi cần thiết; bản sao được thêm vào cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + max(a)) | Đếm và lặp tới 101 số | 
| Không gian | O(max(a)) | Đếm mảng số 0..100 | 

Với n 100 và các số ≤100, thuật toán thực hiện tối đa 200 lần lặp cho mỗi trường hợp thử nghiệm, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        count = [0] * 102
        for x in a:
            count[x] += 1
        result = []
        for i in range(102):
            if count[i]:
                result.append(i)
                count[i] -= 1
        for i in range(102):
            while count[i]:
                result.append(i)
                count[i] -= 1
        print(' '.join(map(str, result)))
    return output.getvalue().strip()

# provided samples
assert run("3\n7\n4 2 0 1 3 3 7\n5\n2 2 8 6 9\n1\n0\n") == "0 1 2 3 4 7 3\n2 6 8 9 2\n0", "samples"

# custom cases
assert run("1\n5\n0 0 0 0 0\n") == "0 0 0 0 0", "all equal zeros"
assert run("1\n4\n1 1 1 1\n") == "1 1 1 1", "all equal nonzero, MEX=0"
assert run("1\n3\n0 2 1\n") == "0 1 2", "small array consecutive numbers"
assert run("1\n6\n0 1 2 4 5 6\n") == "0 1 2 4 5 6", "missing 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 0 0`|`0 0 0 0 0`| Bản sao của 0, MEX tăng trưởng một lần | 
|`1 1 1 1`|`1 1 1 1`| Thiếu 0, MEX ban đầu vẫn là 0 | 
|`0 2 1`|`0 1 2`| Số nhỏ liên tiếp | 
|`0 1 2 4 5 6`|`0 1 2 4 5 6`| Không liên tiếp, thiếu số 3 | 

## Vỏ cạnh 

Nếu`0`bị thiếu hoàn toàn, MEX của tiền tố đầu tiên là`0`và không có tiền tố nào có thể bắt đầu bằng MEX > 0. Ví dụ:`[1,1,2]`kết quả là`[1,2,1]`. Nếu tất cả các phần tử đều giống nhau, MEX chỉ tăng một lần (nếu có 0) và không đổi sau đó. Thuật toán xử lý những điều này một cách chính xác vì nó luôn kiểm tra lần xuất hiện đầu tiên bắt đầu từ`0`và nối thêm các phần tử còn lại sau đó.
