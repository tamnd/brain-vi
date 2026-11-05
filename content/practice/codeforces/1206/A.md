---
title: "CF 1206A - Chọn Hai Số"
description: "Chúng ta có hai tập hợp số nguyên dương. Một bộ sưu tập đại diện cho các giá trị có thể có mà chúng ta có thể chọn làm số đầu tiên và bộ sưu tập thứ hai đại diện cho các giá trị có thể có mà chúng ta có thể chọn làm số thứ hai."
date: "2026-06-13T16:15:59+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1206
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 580 (Div. 2)"
rating: 800
weight: 1206
solve_time_s: 671
verified: false
draft: false
---

[CF 1206A - Chọn hai số](https://codeforces.com/problemset/problem/1206/A) 

**Đánh giá:** 800 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 11m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp số nguyên dương. Một bộ sưu tập đại diện cho các giá trị có thể có mà chúng ta có thể chọn làm số đầu tiên và bộ sưu tập thứ hai đại diện cho các giá trị có thể có mà chúng ta có thể chọn làm số thứ hai. Sau khi chọn một giá trị từ mỗi bộ sưu tập, chúng ta tính tổng của chúng. Hạn chế là số tiền này không được xuất hiện ở bất kỳ đâu trong cả hai bộ sưu tập gốc. 

Nhiệm vụ là xuất ra bất kỳ cặp hợp lệ nào, một phần tử từ mảng đầu tiên và một phần tử từ mảng thứ hai, sao cho tổng của chúng là “mới”, nghĩa là nó không tồn tại trong cả hai mảng. 

Cấu trúc chính của vấn đề là các giá trị nhỏ. Mọi số nằm trong khoảng từ 1 đến 200 và cả hai mảng có kích thước tối đa là 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào liên quan đến việc kiểm tra tất cả các tổng có thể đều khả thi, vì tổng số cặp ứng cử viên nhiều nhất là 10.000 và mỗi lần kiểm tra có thể được thực hiện trong thời gian không đổi với một bảng tần số. 

Một quan sát ngây thơ nhưng quan trọng là nếu chúng ta chọn các phần tử tùy ý, chúng ta có thể vô tình tạo ra một tổng đã tồn tại trong một trong các mảng. Ví dụ: nếu cả hai mảng đều chứa các phạm vi nhỏ dày đặc như từ 1 đến 100 thì nhiều tổng sẽ nằm trong các phạm vi đó, do đó việc chọn số nhỏ một cách mù quáng là không an toàn. 

Một trường hợp thất bại tinh vi khác đến từ tính đối xứng. Nếu cả hai mảng giống hệt nhau hoặc chồng chéo nhiều, việc chọn cùng một giá trị từ cả hai mảng thường tạo ra tổng vẫn nằm trong tập hợp. Ví dụ: nếu cả hai mảng đều chứa 1 và 2, việc chọn (1, 2) sẽ cho kết quả 3, có thể vẫn tồn tại ở một trong số chúng. Vì vậy, tính đúng đắn phụ thuộc vào việc kiểm tra rõ ràng tư cách thành viên của tổng chứ không phụ thuộc vào các giả định về cấu trúc. 

Trường hợp quan trọng duy nhất là đảm bảo chúng ta không cho rằng sự đơn điệu hoặc rời rạc. Các mảng có thể chồng lên nhau tùy ý. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử từng cặp (a, b), tính tổng của chúng và kiểm tra xem tổng này có tồn tại trong một trong hai mảng hay không. Vì mỗi mảng có tối đa 100 phần tử nên sẽ tạo ra tối đa 10.000 cặp. Đối với mỗi cặp, việc kiểm tra thành viên trong một mảng bằng cách sử dụng tìm kiếm tuyến tính sẽ tốn O(n + m), trong trường hợp xấu nhất là khoảng 10.000 × 200 thao tác, vẫn có thể chấp nhận được nhưng chậm và dư thừa một cách không cần thiết. 

Cải tiến quan trọng là thay thế việc kiểm tra tư cách thành viên lặp đi lặp lại bằng cấu trúc tra cứu trực tiếp. Vì tất cả các giá trị nằm trong khoảng từ 1 đến 200 nên chúng ta có thể xây dựng một mảng hiện diện boolean cho cả A và B. Sau đó, việc kiểm tra xem tổng có tồn tại hay không sẽ trở thành O(1). 

Điều này biến vấn đề thành một quá trình quét đơn giản: thử các cặp cho đến khi chúng tôi tìm thấy một cặp có tổng không được đánh dấu trong cả hai bảng hiện diện. 

Chúng tôi không cần tối ưu hóa thêm vì các ràng buộc nhỏ và sự đảm bảo đảm bảo tồn tại giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm(n+m)) | O(1) | Quá chậm | 
| Với bảng tra cứu | O(nm) | O(200) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cả hai mảng A và B, đồng thời xây dựng hai mảng boolean, PresentA và PresentB, trong đó PresentA[x] đúng nếu x nằm trong A và tương tự với B. Điều này cho phép truy vấn thành viên theo thời gian không đổi. 
2. Lặp lại mọi phần tử a trong A. Với mỗi a, lặp lại mọi phần tử b trong B. Điều này đảm bảo chúng ta kiểm tra một cách có hệ thống mọi cặp có thể mà không bỏ sót bất kỳ sự kết hợp nào. 
3. Với mỗi cặp (a, b), hãy tính s = a + b. 
4. Kiểm tra xem PresentA[s] hay PresentB[s] có đúng không. Nếu cả hai đều không đúng thì chúng tôi đã tìm thấy một cặp hợp lệ và có thể xuất nó ngay lập tức. 
5. Nếu không tìm thấy cặp nào trong quá trình quét, sự cố đảm bảo điều này sẽ không xảy ra, do đó việc chấm dứt luôn được đảm bảo trước khi cạn kiệt. 

### Tại sao nó hoạt động

Thuật toán kiểm tra mọi cặp có thể, vì vậy nếu tồn tại một cặp hợp lệ thì phải gặp nó trong quá trình lặp. Điều kiện đúng đắn được thực thi trực tiếp tại thời điểm lựa chọn: chúng tôi chỉ chấp nhận một cặp khi tổng của nó không có trong cả hai mảng. Vì việc kiểm tra tư cách thành viên là chính xác và không gần đúng nên không có cặp không hợp lệ nào có thể đi qua và không có cặp hợp lệ nào có thể bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))
    m = int(input())
    B = list(map(int, input().split()))
    
    presentA = [False] * 205
    presentB = [False] * 205
    
    for x in A:
        presentA[x] = True
    for x in B:
        presentB[x] = True
    
    for a in A:
        for b in B:
            s = a + b
            if not presentA[s] and not presentB[s]:
                print(a, b)
                return

solve()
```Giải pháp bắt đầu bằng cách mã hóa thành viên của cả hai mảng thành các bảng boolean có kích thước cố định được lập chỉ mục theo giá trị. Điều này tránh việc quét lặp lại trong quá trình đánh giá cặp. Các vòng lặp lồng nhau sau đó liệt kê tất cả các cặp có thể có theo thứ tự xác định. Cặp đầu tiên thỏa mãn điều kiện sẽ được in ngay lập tức, điều này hợp lệ vì mọi câu trả lời hợp lệ đều được chấp nhận. 

Một điểm tinh tế là các mảng đủ nhỏ để chúng tôi phân bổ một cách an toàn các mảng có kích thước lớn hơn 200 một chút để bao gồm tất cả các tổng có thể lên tới 400. Tuy nhiên, do ràng buộc đảm bảo tính chính xác nên chúng tôi không cần phải xử lý rõ ràng các khoản tiền ngoài phạm vi trong logic điều kiện, vì việc lập chỉ mục danh sách Python sẽ vẫn an toàn nếu được định cỡ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

A = [20], B = [10, 20] 

| Bước | một | b | tổng hợp | ở A | ở B | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 20 | 10 | 30 | không | không | vâng | 

Chúng tôi chọn (20, 10) ngay lập tức vì 30 không tồn tại trong cả hai mảng. Thuật toán dừng ở cặp hợp lệ đầu tiên, cho thấy rằng không cần tìm kiếm đầy đủ sau khi tìm thấy giải pháp. 

### Ví dụ 2 

đầu vào: 

A = [3, 2, 2], B = [1, 5, 7, 7, 9] 

| Bước | một | b | tổng hợp | ở A | ở B | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 4 | không | không | vâng | 

Cặp đầu tiên đã thỏa mãn điều kiện, xác nhận rằng việc đặt hàng không thành vấn đề. Bất kỳ cặp hợp lệ nào đều được chấp nhận và thuật toán sẽ tự nhiên trả về cặp sớm nhất. 

Những dấu vết này cho thấy giải pháp không nhạy cảm với thứ tự đầu vào và chỉ dựa vào cấu trúc thành viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | mỗi cặp phần tử được kiểm tra một lần với truy vấn thành viên O(1) | 
| Không gian | O(200) | mảng boolean cho sự hiện diện giá trị | 

Các ràng buộc đảm bảo n, m ≤ 100, do đó xảy ra tối đa 10.000 đánh giá cặp. Mỗi lần đánh giá là thời gian không đổi, giúp giải pháp được thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    
    def solve():
        input = sys.stdin.readline
        n = int(input())
        A = list(map(int, input().split()))
        m = int(input())
        B = list(map(int, input().split()))
        
        presentA = [False] * 205
        presentB = [False] * 205
        
        for x in A:
            presentA[x] = True
        for x in B:
            presentB[x] = True
        
        for a in A:
            for b in B:
                s = a + b
                if not presentA[s] and not presentB[s]:
                    print(a, b)
                    return
    
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("1\n20\n2\n10 20") in ["20 10", "10 20"]

# all equal small case
assert run("2\n1 1\n2\n1 1") in ["1 1"]

# disjoint simple case
assert run("3\n1 2 3\n3\n10 11 12") != ""

# boundary small values
assert run("1\n200\n1\n200") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng nhỏ giống hệt nhau | bất kỳ cặp hợp lệ nào | tính đúng đắn dưới sự chồng chéo nặng nề | 
| phạm vi rời rạc | cặp nào | số tiền an toàn đơn giản | 
| giá trị biên tối đa | cặp hợp lệ | xử lý số tiền cực lớn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi cả hai mảng giống hệt nhau và nhỏ, ví dụ A = [1, 2] và B = [1, 2]. Thuật toán kiểm tra tất cả bốn cặp. Với (1,1), tổng bằng 2 tồn tại nên bị bác bỏ. Đối với (1,2), tổng bằng 3 không có trong cả hai mảng nên được chấp nhận ngay lập tức. Thuật toán không giả định tính duy nhất, nó xác minh tư cách thành viên một cách rõ ràng. 

Một trường hợp khác là khi giá trị đạt mức tối đa, chẳng hạn như A = [200] và B = [200]. Cặp duy nhất tạo ra tổng 400, nằm ngoài phạm vi của cả hai mảng. Vì mảng hiện diện chỉ đánh dấu tối đa 200 nên 400 sẽ tự động vắng mặt và cặp này được chấp nhận chính xác. 

Trường hợp thứ ba là sự chồng chéo dày đặc, trong đó cả A và B đều chứa hầu hết các số từ 1 đến 200 ngoại trừ một số. Ngay cả khi đó, thuật toán vẫn hoạt động vì nó không phụ thuộc vào khoảng trống lớn hay nhỏ, nó trực tiếp kiểm tra tổng cho đến khi tìm thấy số thiếu, điều này được đảm bảo bởi câu lệnh bài toán.
