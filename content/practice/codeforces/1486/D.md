---
title: "CF 1486D - Trung bình tối đa"
description: "Chúng ta được yêu cầu tìm giá trị trung vị lớn nhất có thể có trong số tất cả các mảng con liền kề của một mảng nhất định có độ dài ít nhất là $k$."
date: "2026-06-10T23:10:14+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1486
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 703 (Div. 2)"
rating: 2100
weight: 1486
solve_time_s: 186
verified: false
draft: false
---

[CF 1486D - Trung bình tối đa](https://codeforces.com/problemset/problem/1486/D) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm giá trị trung vị lớn nhất có thể có trong số tất cả các mảng con liền kề của một mảng nhất định có độ dài ít nhất$k$. Nói cách khác, từ mảng đầu vào$a$chiều dài$n$, chúng ta phải xét mọi đoạn có độ dài$k$hoặc lâu hơn, hãy tính giá trị trung bình của phân đoạn đó và trả về mức tối đa của các giá trị trung vị này. Trung vị của một mảng có kích thước$m$là phần tử ở vị trí$\lfloor (m + 1) / 2 \rfloor$nếu mảng được sắp xếp. 

Những ràng buộc chỉ ra rằng$n$có thể lớn như$2 \cdot 10^5$. Một giải pháp đơn giản để kiểm tra tất cả các mảng con một cách rõ ràng sẽ yêu cầu khoảng$O(n^2)$hoạt động, có thể lên đến$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Điều này vượt xa giới hạn thời gian 2 giây, vì vậy chúng ta cần một cách tiếp cận gần như tuyến tính hoặc$O(n \log n)$nhiều nhất. Các phần tử mảng được giới hạn bởi$1$ĐẾN$n$, gợi ý về khả năng xử lý dựa trên giá trị thay vì sắp xếp đầy đủ các mảng con lặp đi lặp lại. 

Các trường hợp biên không rõ ràng bao gồm các mảng con có nhiều phần tử bằng nhau hoặc khi trung vị lớn nhất xuất hiện trong một mảng con dài hơn$k$. Ví dụ, đối với$n = 5$,$k = 3$, Và$a = [1, 2, 2, 2, 1]$, mảng con$[2, 2, 2]$có trung vị 2 và các mảng con dài hơn như$[1, 2, 2, 2]$cũng có trung vị 2. Các cách tiếp cận bất cẩn chỉ chọn trung vị từ các mảng con có độ dài k đầu tiên có thể bỏ lỡ mức tối đa thực sự. 

Một trường hợp tế nhị khác xảy ra khi tất cả các phần tử đều bằng nhau. Bất kỳ mảng con nào cũng sẽ có cùng một trung vị, vì vậy trung vị tối đa gần như là phần tử đó. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp lại trên tất cả các mảng con có độ dài ít nhất$k$, sắp xếp từng mảng con và lấy giá trị trung vị. Cách tiếp cận này đúng vì nó tính toán trung vị của tất cả các mảng con theo đúng nghĩa đen, nhưng độ phức tạp trong trường hợp xấu nhất là$O(n^3 \log n)$nếu chúng ta tính đến tất cả các kích thước và cách sắp xếp của mảng con, điều này là không khả thi đối với$n$lên đến$2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là nhận ra rằng chúng ta không cần thứ tự sắp xếp chính xác để biết liệu một ứng cử viên có trung bình hay không$m$là có thể. Đối với một mảng con có độ dài$l \ge k$ít nhất phải có trung vị$m$, hơn một nửa số phần tử của nó phải là$\ge m$. Điều này tương đương với việc đánh dấu các phần tử là +1 nếu$\ge m$và -1 nếu ngược lại, sau đó kiểm tra xem có tồn tại một mảng con có độ dài ít nhất không$k$có tổng tích lũy là dương. Điều này biến bài toán thành một biến thể của bài toán tổng mảng con tối đa. 

Với phép chuyển đổi này, chúng ta có thể thực hiện tìm kiếm nhị phân trên các trung vị ứng cử viên. Vì các phần tử được giới hạn giữa 1 và$n$, có nhiều nhất$n$ứng viên riêng biệt. Đối với mỗi trung vị ứng cử viên, chúng tôi sử dụng tổng tiền tố và cửa sổ trượt để kiểm tra hiệu quả xem một mảng con có độ dài ít nhất$k$tồn tại với các phần tử đủ lớn để làm cho trung vị ít nhất là ứng cử viên đó. Điều này mang lại một cái nhìn tổng thể$O(n \log n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3 \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tìm kiếm nhị phân trên các giá trị trung bình ứng cử viên trong khoảng từ 1 đến$n$. Mỗi ứng viên$m$đại diện cho mức trung bình mà chúng tôi đang thử nghiệm. 
2. Đối với một ứng viên nhất định$m$, biến đổi mảng$a$thành một mảng$b$Ở đâu$b[i] = 1$nếu như$a[i] \ge m$, Và$-1$nếu không thì. Điều này cho phép chúng tôi theo dõi xem một mảng con có đủ phần tử lớn hay không. 
3. Tính tổng tiền tố$s[i] = b[0] + b[1] + \dots + b[i-1]$. Sự khác biệt tổng tiền tố$s[r] - s[l]$đưa ra tổng số phần tử lớn trừ đi phần tử nhỏ trong mảng con$a[l..r-1]$. 
4. Để kiểm tra xem một mảng con có độ dài ít nhất$k$tồn tại với trung vị ít nhất$m$, lặp đi lặp lại$r = k$ĐẾN$n$và duy trì tổng tiền tố tối thiểu$min\_prefix = \min(s[0..r-k])$. Nếu như$s[r] - min\_prefix > 0$, thì mảng con kết thúc tại$r-1$có nhiều +1 hơn -1, do đó trung vị ít nhất là$m$. 
5. Nếu tồn tại một mảng con như vậy thì trung vị dự kiến ​​là khả thi; nếu không thì không. Điều chỉnh giới hạn tìm kiếm nhị phân tương ứng cho đến khi tìm thấy trung vị khả thi lớn nhất. 

Tại sao nó hoạt động: Điều bất biến được duy trì là bất kỳ mảng con nào có hơn một nửa số phần tử của nó$\ge m$tương ứng với sự khác biệt dương trong tổng tiền tố được chuyển đổi. Tìm kiếm nhị phân đảm bảo rằng giá trị tối đa$m$được tìm thấy và kiểm tra tổng tiền tố đảm bảo tính chính xác cho các mảng con có độ dài ít nhất$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_median(n, k, a):
    low, high = 1, n
    ans = 1
    while low <= high:
        mid = (low + high) // 2
        b = [1 if x >= mid else -1 for x in a]
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i+1] = prefix[i] + b[i]
        min_prefix = 0
        found = False
        for r in range(k, n+1):
            min_prefix = min(min_prefix, prefix[r-k])
            if prefix[r] - min_prefix > 0:
                found = True
                break
        if found:
            ans = mid
            low = mid + 1
        else:
            high = mid - 1
    return ans

if __name__ == "__main__":
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    print(max_median(n, k, a))
```Đầu tiên, mã thực hiện tìm kiếm nhị phân trên các giá trị trung bình có thể có. Đối với mỗi ứng cử viên, nó biến đổi mảng thành các giá trị +1/-1 và tính tổng tiền tố. Một cửa sổ trượt có kích thước tối thiểu$k$đảm bảo chúng tôi kiểm tra hiệu quả xem có tồn tại một mảng con hợp lệ hay không. Đang cập nhật`min_prefix`là tinh tế; nó phải luôn biểu thị tổng tiền tố tối thiểu từ đầu đến`r-k`để nắm bắt tất cả các mảng con có độ dài ít nhất$k$. 

## Ví dụ đã hoạt động 

Mẫu 1:$n = 5, k = 3, a = [1,2,3,2,1]$| Ứng viên trung vị | mảng b | tổng tiền tố | tiền tố tối thiểu | Thành lập? | 
| --- | --- | --- | --- | --- | 
| 3 | [-1,-1,1,-1,-1] | [0,-1,-2,-1,-2,-3] | 0, -1, -2 | Đúng | 
| 2 | [-1,1,1,1,-1] | [0,-1,0,1,2,1] | 0 | Tìm thấy tại r=4 | 

Điều này chứng tỏ rằng 2 là khả thi còn 3 thì không. 

Mẫu 2:$n=4, k=2, a=[3,1,4,2]$| Ứng viên trung vị | mảng b | tổng tiền tố | tiền tố tối thiểu | Thành lập? | 
| --- | --- | --- | --- | --- | 
| 3 | [1,-1,1,-1] | [0,1,0,1,0] | 0 | Tìm thấy tại r=3 | 
| 4 | [-1,-1,1,-1] | [0,-1,-2,-1,-2] | 0 | Không | 

Trung bình tối đa khả thi là 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tìm kiếm nhị phân trên n trung vị ứng viên, mỗi trung vị yêu cầu tính tổng tiền tố O(n) | 
| Không gian | O(n) | Mảng tổng tiền tố và mảng được chuyển đổi | 

Với n lên đến$2 \cdot 10^5$,$O(n \log n) \approx 4 \cdot 10^6$hoạt động, vừa vặn thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    return str(max_median(n, k, a))

# Provided samples
assert run("5 3\n1 2 3 2 1\n") == "2", "sample 1"
assert run("4 2\n3 1 4 2\n") == "3", "sample 2"

# Custom cases
assert run("1 1\n7\n") == "7", "single element"
assert run("5 5\n2 2
```
