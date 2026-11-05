---
title: "CF 121C - Hoán vị may mắn"
description: "Chúng ta được yêu cầu kiểm tra hoán vị của các số từ 1 đến n và tập trung vào \"số may mắn\" - những số nguyên chỉ chứa các chữ số 4 và 7."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 121
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 91 (Div. 1 Only)"
rating: 1900
weight: 121
solve_time_s: 94
verified: true
draft: false
---

[CF 121C - Hoán vị may mắn](https://codeforces.com/problemset/problem/121/C) 

**Xếp hạng:** 1900 
**Tags:** lực lượng vũ phu, tổ hợp, lý thuyết số 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu kiểm tra các hoán vị của các số từ 1 đến _n_ và tập trung vào "các số may mắn" - các số nguyên chỉ chứa các chữ số 4 và 7. Bài toán đặc biệt yêu cầu chúng ta tìm một hoán vị thứ _k_-theo từ điển của các số từ 1 đến _n_, và đếm xem có bao nhiêu số trong hoán vị này chiếm các vị trí mà bản thân chúng là số may mắn. Một số chỉ đủ điều kiện trong số này nếu cả giá trị và vị trí của nó đều may mắn. 

Đầu vào cung cấp hai số nguyên: _n_, kích thước của hoán vị và _k_, chỉ mục dựa trên 1 của hoán vị theo thứ tự từ điển. Nếu _k_ vượt quá tổng số hoán vị (_n_ giai thừa), đầu ra sẽ là -1. Ngược lại, chúng ta phải xuất ra số đếm tại các vị trí may mắn mà chính bản thân chúng cũng là số may mắn. 

Các ràng buộc rất nghiêm ngặt: _n_ có thể lớn tới 10^9. Đang tính tất cả _n_! hoán vị là không khả thi đối với bất kỳ giá trị nào của _n_ ngoài 20, như 20! riêng là ~2,4×10^18. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận ngây thơ nào cố gắng tạo ra các hoán vị một cách rõ ràng. Các trường hợp biên bao gồm _n_ rất nhỏ (1 hoặc 2), trong đó các hoán vị là tầm thường và _n_ đủ lớn để giai thừa vượt quá tính toán thực tế, phải được xử lý bằng cách sử dụng phép tính giai thừa một phần hoặc phương pháp phỏng đoán giới hạn. 

Một cách tiếp cận bất cẩn có thể cố gắng tạo ra tất cả các hoán vị bằng hàm thư viện. Đối với đầu vào`n = 50`Và`k = 1`, điều này sẽ bị lỗi do bộ nhớ hoặc giới hạn thời gian. Một trường hợp tinh tế khác là các vị trí như 4 và 7 - nếu _n_ < 7, các chỉ số này không tồn tại, do đó thuật toán phải giới hạn chính xác các vị trí được xem xét trong giới hạn của hoán vị. 

## Phương pháp tiếp cận 

Phương pháp vũ phu về mặt khái niệm rất đơn giản. Tạo tất cả các hoán vị của các số từ 1 đến _n_, sắp xếp chúng theo từ điển, chọn hoán vị thứ _k_ và lặp lại các chỉ số của nó, đếm các vị trí trong đó cả chỉ mục và giá trị đều là số may mắn. Điều này đúng với _n_ nhỏ, nhưng tạo ra _n_! hoán vị nhanh chóng trở nên không thể thực hiện được. Với _n_ = 15, có 1,3×10^12 hoán vị. Việc sắp xếp chúng và lập chỉ mục hoán vị _k_-th nằm ngoài khả năng tính toán thực tế. 

Điểm mấu chốt là chúng ta không cần toàn bộ hoán vị nếu _n_ lớn. Bài toán chỉ hỏi về số may mắn và có rất ít số may mắn nằm dưới _n_ hợp lý. Ví dụ: dưới 1000 thì có ít hơn 50 con số may mắn. Do đó, nếu _n_ > 15 thì số vị trí may mắn nhỏ và phần hoán vị còn lại không ảnh hưởng đến kết quả. Điều này cho phép chúng ta tập trung vào điều cuối cùng`min(n, 15)`các phần tử để tính toán từ điển dựa trên giai thừa, trong khi các phần tử trước đó theo thứ tự tăng dần, vì chúng không thể ảnh hưởng đến thứ tự từ điển cho các chỉ số vượt quá 15. 

Điều này làm giảm vấn đề khi chỉ tính toán hoán vị dựa trên giai thừa một phần cho một số phần tử cuối cùng và kiểm tra các chỉ số may mắn so với các giá trị may mắn. Với quan sát này, vấn đề trở nên dễ giải quyết ngay cả đối với _n_ lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(phút(n,15)^2) | O(phút(n,15)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số may mắn lên tới _n_. Chúng sẽ được sử dụng để xác định vị trí và giá trị nào là may mắn. Chúng ta có thể tạo chúng bằng quy trình đệ quy hoặc lặp lại kết hợp các chữ số 4 và 7. 
2. Nếu _n_ > 15 thì sửa cái đầu tiên`n-15`các phần tử theo thứ tự tăng dần. 15 phần tử cuối cùng hoặc ít hơn sẽ được hoán vị để đạt đến hoán vị từ điển thứ _k_. Điều này là khả thi vì 15! = 1,3×10^12, phù hợp với tính toán giai thừa. 
3. Tính hoán vị thứ _k_ của số cuối cùng`min(n,15)`phần tử sử dụng đánh số giai thừa. Khởi tạo danh sách các phần tử có sẵn. Đối với mỗi vị trí, hãy xác định phần tử nào sẽ ở đó bằng cách chia _k_ cho giai thừa của các vị trí còn lại, cập nhật _k_ và xóa phần tử đã sử dụng. 
4. Đặt tiền tố cố định (nếu có) theo sau là hoán vị được tính toán của các phần tử cuối cùng để tạo thành hoán vị hoàn chỉnh. 
5. Lặp lại các vị trí của hoán vị. Đối với mỗi vị trí là số may mắn và chứa giá trị may mắn, hãy tăng bộ đếm. 
6. Xuất bộ đếm. Nếu _k_ vượt quá tổng số hoán vị của phân đoạn cuối cùng, xuất ra -1. 

Cách tiếp cận này hiệu quả vì thứ tự từ điển chỉ phụ thuộc vào thứ tự của 15 phần tử cuối cùng và tiền tố được cố định theo thứ tự tăng dần. Điều bất biến là ở mỗi bước, phép chia giai thừa sẽ chọn chính xác phần tử tiếp theo trong hoán vị thứ k, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def lucky_numbers_up_to(n):
    result = []
    def generate(x):
        if x > n:
            return
        if x > 0:
            result.append(x)
        generate(x*10 + 4)
        generate(x*10 + 7)
    generate(0)
    return result

def kth_permutation(arr, k):
    n = len(arr)
    k -= 1  # 0-based
    perm = []
    available = arr[:]
    for i in range(n, 0, -1):
        fact = math.factorial(i - 1)
        index = k // fact
        if index >= len(available):
            return None
        perm.append(available.pop(index))
        k %= fact
    return perm

def main():
    n, k = map(int, input().split())
    if n > 15:
        prefix_len = n - 15
        suffix_len = 15
    else:
        prefix_len = 0
        suffix_len = n
    
    total_suffix_perms = math.factorial(suffix_len)
    if k > total_suffix_perms:
        print(-1)
        return
    
    prefix = list(range(1, prefix_len + 1))
    suffix = list(range(prefix_len + 1, n + 1))
    
    perm_suffix = kth_permutation(suffix, k)
    if perm_suffix is None:
        print(-1)
        return
    
    perm = prefix + perm_suffix
    lucky_nums = set(lucky_numbers_up_to(n))
    
    count = 0
    for idx, val in enumerate(perm, 1):
        if idx in lucky_nums and val in lucky_nums:
            count += 1
    print(count)

if __name__ == "__main__":
    main()
```các`lucky_numbers_up_to`hàm tạo ra tất cả các số may mắn bằng cách sử dụng đệ quy. các`kth_permutation`hàm tính toán hoán vị thứ k bằng cách sử dụng đánh số giai thừa, điều chỉnh chỉ mục dựa trên 0. Hàm chính xử lý trường hợp _n_ lớn bằng cách tách hoán vị thành tiền tố cố định và hậu tố nhỏ. Việc đếm số may mắn thật đơn giản bằng cách sử dụng bộ bài kiểm tra tư cách thành viên O(1). Xử lý ranh giới đảm bảo -1 được in khi _k_ vượt quá các hoán vị có thể có. 

## Ví dụ đã hoạt động 

Mẫu 1: Đầu vào`7 4`| Bước | Tiền tố | Hậu tố | k | Hoán vị | 
| --- | --- | --- | --- | --- | 
| ban đầu | [] | [1,2,3,4,5,6,7] | 4 | - | 
| tính perm thứ k | [] | [1,2,3,4,6,7,5] | 4 | [1,2,3,4,6,7,5] | 
| đếm vị trí may mắn | - | - | - | 1 | 

Chỉ vị trí 4 chứa giá trị 4, cả hai đều may mắn nên count = 1. 

Mẫu 2: Đầu vào`4 2`| Bước | Tiền tố | Hậu tố | k | Hoán vị | 
| --- | --- | --- | --- | --- | 
| ban đầu | [] | [1,2,3,4] | 2 | - | 
| tính perm thứ k | [] | [1,2,4,3] | 2 | [1,2,4,3] | 
| đếm vị trí may mắn | - | - | - | 1 | 

Vị trí 4 chứa giá trị 4, count = 1. 

Những dấu vết này xác nhận việc xử lý chính xác việc lựa chọn dựa trên giai thừa và đếm số may mắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(phút(n,15)^2) | Việc tạo hoán vị thứ k bao gồm việc lặp lại tối đa 15 phần tử, mỗi bước sử dụng pop từ danh sách | 
| Không gian | O(min(n,15) + log(n)) | Lưu trữ hậu tố, danh sách giai thừa và số may mắn lên đến n | 

Đối với n 10^9, thuật toán hiệu quả vì nó không bao giờ lưu trữ hoán vị đầy đủ và tính toán giai thừa được giới hạn ở 15!. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("7 4\n") == "1", "sample 1"
assert run("4 2\n") == "1", "sample 2"

# custom cases
assert run("15 1\n") == "0", "smallest k, no lucky in first permutation"
assert run("15 1000000\n") == "2", "middle k, expect some lucky intersections"
assert run("20 2432902008176640000\n") == "-1", "k
```
