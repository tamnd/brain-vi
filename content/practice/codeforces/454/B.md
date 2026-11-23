---
title: "CF 454B - Chú ngựa nhỏ và sắp xếp theo ca"
description: "Chúng ta có một chuỗi các số nguyên được sắp xếp thành một dòng và mục tiêu của chúng ta là sắp xếp chúng theo thứ tự không giảm bằng cách thực hiện liên tục một thao tác được phép duy nhất: di chuyển phần tử cuối cùng của chuỗi lên phía trước."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 454
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 259 (Div. 2)"
rating: 1200
weight: 454
solve_time_s: 67
verified: true
draft: false
---

[CF 454B - Chú ngựa nhỏ và sắp xếp theo ca](https://codeforces.com/problemset/problem/454/B) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các số nguyên được sắp xếp thành một dòng và mục tiêu của chúng ta là sắp xếp chúng theo thứ tự không giảm bằng cách thực hiện liên tục một thao tác được phép duy nhất: di chuyển phần tử cuối cùng của chuỗi lên phía trước. Về mặt khái niệm, chúng ta được phép "xoay" mảng sang phải một vị trí bất kỳ số lần nào và chúng ta muốn tìm số lần quay tối thiểu như vậy để tạo ra một chuỗi được sắp xếp. Nếu không có số lần quay nào có thể sắp xếp được dãy, chúng ta trả về -1. 

Đầu vào bao gồm độ dài của chuỗi`n`và chính trình tự đó, ở đâu`n`có thể lớn tới 100.000. Với giới hạn thời gian 1 giây, bất kỳ giải pháp nào có độ phức tạp kém hơn O(n log n) đều có rủi ro và các phương pháp tiếp cận O(n^2) đều bị loại trừ một cách hiệu quả. Bản thân các phần tử đều là số nguyên dương lên tới 100.000, nghĩa là chúng ta chỉ cần lo lắng về việc sắp xếp logic chứ không phải lo lắng về các kiểu dữ liệu đặc biệt. 

Các trường hợp biên bao gồm các chuỗi đã được sắp xếp, các chuỗi được sắp xếp theo thứ tự giảm dần nghiêm ngặt (yêu cầu xoay hoàn toàn), các chuỗi tồn tại nhiều bản sao và các chuỗi không có phép xoay nào có thể khiến chuỗi được sắp xếp. Ví dụ, đầu vào`[3, 1, 2]`không thể sắp xếp theo bất kỳ số vòng quay đơn nào, vì vậy kết quả đầu ra đúng là -1. Một cách tiếp cận bất cẩn chỉ kiểm tra xem mảng có được xoay theo chu kỳ một lần hay không sẽ trả về không chính xác một số phép quay ngay cả khi không thể sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi phép quay có thể từ 0 đến n-1 và kiểm tra xem chuỗi có được sắp xếp sau mỗi lần quay hay không. Đối với mỗi vòng quay, việc xác minh xem mảng có được sắp xếp hay không sẽ mất thời gian O(n). Vì có thể có n phép quay nên phương pháp này yêu cầu các phép toán O(n^2) trong trường hợp xấu nhất. Với n = 10^5, điều này dẫn đến 10^10 thao tác, vượt xa giới hạn có thể chấp nhận được, do đó, việc sử dụng vũ lực là không khả thi. 

Quan sát quan trọng là chỉ có thể thu được một chuỗi đã sắp xếp bằng một phép quay nếu chuỗi đó đã "gần như được sắp xếp" ngoại trừ một điểm giảm duy nhất trong đó chuỗi bao quanh. Nói cách khác, có thể có nhiều nhất một vị trí`i`Ở đâu`a[i] > a[i+1]`. Nếu có nhiều hơn một vị trí như vậy thì không có số lần quay nào có thể sắp xếp mảng. Nếu có đúng một vị trí như vậy thì số vòng quay cần thiết là`n - (i + 1)`bởi vì di chuyển tất cả các phần tử sau`i`ở phía trước sắp xếp trình tự theo thứ tự được sắp xếp. 

Thông tin chi tiết này giúp giảm vấn đề thành một lần quét tuyến tính duy nhất để đếm "số lần đảo ngược" trong đó một số lớn hơn số tiếp theo của nó và tính toán số học đơn giản về các phép quay nếu tồn tại chính xác một lần đảo ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`break_point_count`về 0 và một biến`break_index`đến -1. Chúng sẽ theo dõi số vị trí mà phần tử hiện tại lớn hơn phần tử tiếp theo. 
2. Lặp lại mảng từ chỉ số 0 đến n-2. Đối với mỗi chỉ số`i`, so sánh`a[i]`với`a[i+1]`. Nếu như`a[i] > a[i+1]`, tăng`break_point_count`và thiết lập`break_index = i`. 
3. Sau vòng lặp, so sánh phần tử cuối cùng`a[n-1]`với phần tử đầu tiên`a[0]`để kiểm tra xem có sự đảo ngược bao quanh không. Nếu như`a[n-1] > a[0]`, tăng`break_point_count`và thiết lập`break_index = n-1`. 
4. Nếu`break_point_count`bằng 0, mảng đã được sắp xếp và không cần xoay. 
5. Nếu`break_point_count`chính xác là một, số vòng quay tối thiểu cần thiết là`n - (break_index + 1)`. 
6. Nếu`break_point_count`lớn hơn một, việc sắp xếp mảng theo phép quay là không thể và chúng tôi xuất ra -1. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng một chuỗi được sắp xếp phải có nhiều nhất một điểm "ngắt" trong đó thứ tự bị vi phạm, bởi vì mỗi vòng quay chỉ có thể di chuyển đoạn đuôi lên phía trước. Bằng cách theo dõi điểm ngắt đơn này và tính toán độ lệch của nó, chúng ta có thể xác định chính xác số vòng quay cần thiết. Nhiều lần ngắt ngụ ý rằng không một phép quay nào có thể đưa tất cả các phần tử vào thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    break_point_count = 0
    break_index = -1
    
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            break_point_count += 1
            break_index = i
    
    if a[n - 1] > a[0]:
        break_point_count += 1
        break_index = n - 1
    
    if break_point_count == 0:
        print(0)
    elif break_point_count == 1:
        print(n - break_index - 1)
    else:
        print(-1)
```Đầu tiên, mã sẽ đọc trình tự và khởi tạo bộ đếm cho các điểm ngắt. Nó lặp qua mảng kiểm tra từng cặp liên tiếp và cặp bao quanh ở cuối. Nếu có chính xác một phép đảo ngược, nó sẽ tính toán các phép quay bằng cách sử dụng`n - break_index - 1`, căn chỉnh phần tử nhỏ nhất ở phía trước. Đảo ngược bằng không không cần phải quay. Không thể khắc phục nhiều lần đảo ngược bằng một kiểu xoay. 

## Ví dụ đã hoạt động 

**Mẫu 1:** Đầu vào`[2, 1]`| tôi | một [tôi] | a[i+1] | break_point_count | break_index | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 0 | 

Kiểm tra toàn diện`a[1] > a[0]`→ 1 > 2 → sai`break_point_count = 1`,`break_index = 0`, phép quay =`2 - 0 - 1 = 1`Điều này phù hợp với đầu ra dự kiến 1. 

**Ví dụ tùy chỉnh:** Đầu vào`[3, 4, 1, 2]`| tôi | một [tôi] | a[i+1] | break_point_count | break_index | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 4 | 0 | -1 | 
| 1 | 4 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 1 | 

Bao quanh`a[3] > a[0]`→ 2 > 3 → sai`break_point_count = 1`, phép quay =`4 - 1 - 1 = 2`Điều này xác nhận thuật toán tính toán số vòng quay chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính đơn qua mảng và một lần kiểm tra toàn diện | 
| Không gian | O(1) | Chỉ sử dụng một số biến số nguyên, không cần thêm dung lượng | 

Đối với n tối đa 10^5, giải pháp tuyến tính này thực thi thoải mái trong giới hạn thời gian 1 giây và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("2\n2 1\n") == "1", "sample 1"

# Already sorted
assert run("3\n1 2 3\n") == "0", "already sorted"

# Maximum rotations
assert run("3\n3 1 2\n") == "2", "wrap-around needed"

# Impossible to sort
assert run("4\n3 1 4 2\n") == "-1", "cannot sort"

# All equal values
assert run("5\n2 2 2 2 2\n") == "0", "all equal"

# Minimum size descending
assert run("2\n2 1\n") == "1", "min size descending"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 | Kích thước tối thiểu giảm dần | 
| 1 2 3 | 0 | Trình tự đã được sắp xếp | 
| 3 1 2 | 2 | Cần xoay để sắp xếp | 
| 3 1 4 2 | -1 | Không thể sắp xếp | 
| 2 2 2 2 2 | 0 | Tất cả các giá trị bằng nhau | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp như`[1, 2, 3]`, thuật toán không tìm thấy điểm dừng, tạo ra các phép quay bằng 0 và tránh mọi tính toán không cần thiết. Đối với một mảng có nhiều điểm giảm như`[3, 1, 4, 2]`, thuật toán đếm nhiều hơn một dấu ngắt, xuất ra chính xác -1 và chứng minh rằng tính bất biến của một điểm ngắt duy nhất là cần thiết cho việc sắp xếp dựa trên xoay vòng. Đối với các mảng có phần tử cuối cùng lớn hơn phần tử đầu tiên, chẳng hạn như`[3, 4, 1, 2]`, thuật toán sẽ kiểm tra tổng thể và tính toán các phép quay tối thiểu một cách chính xác. Mỗi trường hợp này xác nhận rằng thuật toán xử lý các ranh giới và điều kiện đặc biệt một cách nhất quán.
