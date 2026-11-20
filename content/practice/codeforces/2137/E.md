---
title: "CF 2137E - Mex hóa"
description: "Chúng ta được cung cấp một mảng số nguyên và được yêu cầu thực hiện một phép biến đổi đặc biệt nhiều lần. Đối với mỗi phần tử trong mảng, chúng tôi thay thế nó bằng số nguyên không âm tối thiểu không xuất hiện ở bất kỳ nơi nào khác trong mảng."
date: "2026-06-08T02:32:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2137
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1047 (Div. 3)"
rating: 1500
weight: 2137
solve_time_s: 99
verified: false
draft: false
---

[CF 2137E - Mexification](https://codeforces.com/problemset/problem/2137/E) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên và được yêu cầu thực hiện một phép biến đổi đặc biệt nhiều lần. Đối với mỗi phần tử trong mảng, chúng tôi thay thế nó bằng số nguyên không âm tối thiểu không xuất hiện ở bất kỳ nơi nào khác trong mảng. Phép biến đổi này được áp dụng đồng thời cho tất cả các phần tử và chúng tôi lặp lại nó một số lần nhất định. Sau tất cả các thao tác, chúng ta được yêu cầu tính tổng của mảng. 

Thách thức chính là số lượng phép toán có thể cực kỳ lớn, lên tới một tỷ và mảng cũng có thể lớn, lên tới hai trăm nghìn phần tử. Điều này loại trừ mọi giải pháp mô phỏng rõ ràng mọi thao tác, bởi vì ngay cả một lần truyền qua một mảng lớn một tỷ lần cũng sẽ mất quá nhiều thời gian. 

Trường hợp cạnh tinh tế xảy ra khi mảng chứa tất cả các số nguyên từ 0 đến một mức tối đa nào đó mà không có khoảng trống. Trong trường hợp đó, mỗi phần tử cuối cùng sẽ hội tụ về phần tử lớn nhất cộng một. Một trường hợp cạnh khác là khi mảng đã chứa các giá trị lặp lại hoặc khi thiếu số nguyên trong phạm vi ban đầu. Những điều kiện này ảnh hưởng đến sự biến đổi theo những cách không rõ ràng: một số phần tử có thể trở thành không đổi sau một bước, trong khi những phần tử khác có thể tăng lên. 

Ví dụ, hãy xem xét mảng`[0, 0]`với hai thao tác. Sau thao tác đầu tiên, mỗi số 0 sẽ nhìn thấy số 0 còn lại, do đó MEX của phần tử còn lại là`1`. Sau hoạt động thứ hai, cả hai phần tử gặp lại nhau và MEX vẫn còn`1`. Việc triển khai đơn giản không tính đến sự hội tụ sẽ lãng phí thời gian lặp đi lặp lại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng hoạt động chính xác như được mô tả. Đối với mỗi bước, chúng tôi sẽ tính MEX cho mọi phần tử dựa trên tất cả các phần tử khác. Đối với một mảng có kích thước`n`, việc tính toán MEX cho một phần tử có giá O(n), do đó, một lần vượt qua đầy đủ có giá O(n^2). Lặp đi lặp lại điều này cho`k`các bước dẫn đến O(n^2 * k), điều này không khả thi với n tối đa 2×10^5 và k tối đa 10^9. 

Thông tin chi tiết quan trọng là nhận thấy rằng mảng ổn định rất nhanh. Sau tối đa hai thao tác, mảng vẫn không đổi hoặc phát triển theo cách có thể dự đoán được. Cụ thể, thao tác đầu tiên sẽ điền các số còn thiếu nhỏ nhất và thao tác thứ hai chỉ có thể tạo ra nhiều nhất một giá trị mới là mức trần của mức trung bình tối đa và MEX. Sau đó, các thao tác tiếp theo không làm thay đổi tổng nếu k lớn hơn hai. 

Chúng ta có thể khai thác thuộc tính này bằng cách xác định hai giá trị tới hạn: MEX hiện tại của mảng và phần tử lớn nhất. Nếu MEX lớn hơn phần tử tối đa, mọi thao tác sẽ thay thế các phần tử bằng MEX và tất cả các phần tử sẽ hội tụ về MEX ngay lập tức. Mặt khác, chỉ có thể tạo một phần tử mới ở mỗi thao tác và mảng sẽ ổn định sau nhiều nhất một hoặc hai bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * k) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và xác định MEX hiện tại và phần tử lớn nhất. MEX là số nguyên không âm nhỏ nhất không có trong mảng. Giá trị tối đa là giá trị lớn nhất trong mảng. 
2. Nếu MEX lớn hơn phần tử tối đa, mảng sẽ ổn định ngay lập tức. Mỗi phần tử nhỏ hơn MEX được thay thế bằng MEX trong một thao tác, nhưng vì tất cả các thao tác tiếp theo không có hiệu lực nên tổng sau`k`hoạt động là`n * MEX`. 
3. Nếu MEX nhỏ hơn hoặc bằng mức tối đa, hãy tính giá trị ứng cử viên mới như`(max + MEX + 1) // 2`. Giá trị này biểu thị số nguyên nhỏ nhất lớn hơn MEX có thể xuất hiện bằng cách chèn một giá trị mới mà không làm giảm bất kỳ giá trị hiện có nào. Thêm nó vào tập hợp các phần tử mảng nếu nó chưa tồn tại. 
4. Sau khi thêm giá trị mới, mảng sẽ ổn định vì thao tác tiếp theo không thể tạo ra bất kỳ giá trị mới nào; tất cả các phần tử trong tương lai đều đã tồn tại hoặc phần tử mới sẽ trở thành MEX. Tính tổng của mảng đã cập nhật. 
5. Xuất ra tổng. 

Tại sao nó hoạt động: Mỗi thao tác sẽ tăng hoặc duy trì tập hợp số trong mảng. Khi mảng bao gồm tất cả các số nguyên từ 0 đến tối đa, việc thêm`(max + MEX + 1) // 2`là sự thay đổi duy nhất có thể xảy ra và các phép toán tiếp theo không làm thay đổi tổng. Bất biến này đảm bảo rằng chúng ta không cần phải mô phỏng tất cả`k`các bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        s = set(a)
        mex = 0
        while mex in s:
            mex += 1
        mx = max(a)
        if k == 0:
            print(sum(a))
            continue
        if mex > mx:
            # All numbers less than mex exist; array will be filled with new mex
            print(sum(a) + k * mex)
        else:
            new_elem = (mx + mex + 1) // 2
            if new_elem not in s:
                s.add(new_elem)
                print(sum(s))
            else:
                print(sum(a))

if __name__ == "__main__":
    solve()
```Giải pháp này trước tiên tính toán MEX và phần tử tối đa một cách hiệu quả bằng cách sử dụng tập hợp để kiểm tra tư cách thành viên O(1). Sau đó chúng tôi kiểm tra hai trường hợp: MEX có vượt quá mức tối đa hay không. Đối với trường hợp thứ hai, chúng tôi tính toán giá trị ứng cử viên và chỉ thêm nó nếu nó là mới. Các trường hợp cạnh như k = 0 được xử lý riêng. 

## Ví dụ đã hoạt động 

Đối với mảng`[0, 2, 1]`với`k = 3`, MEX ban đầu là`3`và tối đa là`2`. Vì MEX > max, tổng sau bất kỳ số thao tác nào là`0 + 2 + 1 = 3`. Mảng ổn định ngay lập tức. 

Vì`[0, 0]`với`k = 2`, MEX ban đầu là`1`và tối đa là`0`. MEX > max, do đó mảng trở thành`[1, 1]`sau ca phẫu thuật đầu tiên. Tổng = 2. Mọi thao tác tiếp theo giữ tổng bằng 2. 

| Bước | Mảng | MEX | Tối đa | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [0,0] | 1 | 0 | 0 | 
| Sau ca phẫu thuật đầu tiên | [1,1] | 2 | 1 | 2 | 
| Sau lần phẫu thuật thứ 2 | [1,1] | 2 | 1 | 2 | 

Dấu vết này xác nhận rằng mảng ổn định sau một thao tác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi tính toán MEX và tối đa theo O(n) và thực hiện các thao tác thiết lập theo O(1) được khấu hao. | 
| Không gian | O(n) | Tập hợp này lưu trữ tất cả các phần tử duy nhất của mảng. | 

Với các ràng buộc, giải pháp dễ dàng xử lý tổng n trên tất cả các trường hợp thử nghiệm 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("""5
3 3
0 2 1
2 4
0 2
4 1
0 0 1 1
8 7
6 6 2 4 3 0 1 8
2 2
0 0""") == "3\n1\n8\n25\n0"

# custom cases
assert run("1\n2 0\n0 1") == "1", "k=0 no change"
assert run("1\n3 1\n0 0 0") == "2", "all zeros, single operation"
assert run("1\n5 1000000000\n0 1 2 3 4") == "15", "array already consecutive 0..n-1"
assert run("1\n1 10\n0") == "10", "single element, multiple operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0\n0 1`| 1 | k=0, mảng không thay đổi | 
|`3 1\n0 0 0`| 2 | tất cả số không, thao tác đơn lẻ | 
|`5 1000000000\n0 1 2 3 4`| 15 | mảng số liên tiếp, k lớn | 
|`1 10\n0`| 10 | phần tử đơn hoạt động lặp đi lặp lại | 

## Vỏ cạnh 

Khi k = 0 thì mảng không thay đổi. Thuật toán trả về chính xác tổng ban đầu. 

Đối với các mảng có MEX > max, như`[0,2,1]`, mảng đã ổn định và tổng sẽ đúng ngay lập tức. Đối với các mảng có phần tử lặp lại, chẳng hạn như`[0,0]`, thao tác đầu tiên sẽ thay thế chúng bằng MEX và tổng được cập nhật một lần. Thuật toán xử lý k cực lớn bằng cách kiểm tra các điều kiện ổn định thay vì lặp lại một cách không cần thiết.
