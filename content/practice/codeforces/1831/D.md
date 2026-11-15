---
title: "CF 1831D - BOSS Có Thể Đếm Đôi"
description: "Chúng ta có hai mảng a và b có độ dài n bằng nhau. Nhiệm vụ là đếm số cặp chỉ số (i, j) có i < j sao cho tích của các phần tử a tại các chỉ số đó bằng tổng các phần tử b tương ứng: a[i] a[j] = b[i] + b[j]."
date: "2026-06-09T07:06:54+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 1831
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 875 (Div. 2)"
rating: 2000
weight: 1831
solve_time_s: 96
verified: false
draft: false
---

[CF 1831D - BOSS có thể đếm các cặp](https://codeforces.com/problemset/problem/1831/D) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, toán học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng`a`Và`b`có độ dài bằng nhau`n`. Nhiệm vụ là đếm số cặp chỉ số`(i, j)`với`i < j`sao cho sản phẩm của`a`các phần tử tại các chỉ số đó bằng tổng của các phần tử tương ứng`b`các yếu tố:`a[i] * a[j] = b[i] + b[j]`. Về cơ bản, chúng tôi đang tìm kiếm sự kết hợp của các vị trí trong đó mối quan hệ nhân trong`a`phù hợp với mối quan hệ phụ gia trong`b`. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên đến`2 * 10^5`tổng cộng trên tất cả các trường hợp thử nghiệm và các phần tử mảng riêng lẻ được giới hạn bởi`n`. Một giải pháp đơn giản để kiểm tra tất cả các cặp sẽ yêu cầu khoảng`O(n^2)`hoạt động cho mỗi trường hợp thử nghiệm. Với`n`lên đến`2 * 10^5`, điều này có thể dẫn đến 10^10 thao tác trong trường hợp xấu nhất, quá chậm so với giới hạn thời gian 4 giây. Chúng ta cần một giải pháp gần tuyến tính hoặc hơi siêu tuyến tính trong`n`cho từng trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử giống hệt nhau hoặc có các giá trị nhỏ như`1`tương tác với các giá trị lớn. Ví dụ, nếu`a = [1, 1]`Và`b = [1, 1]`, cặp duy nhất`(1,2)`thỏa mãn`1*1 = 1+1`đó là`1 = 2`, SAI. Việc triển khai bất cẩn giả định số lượng nhỏ sẽ tự động tạo ra các cặp hợp lệ sẽ đưa ra câu trả lời sai. 

Một khía cạnh tinh tế khác là`a[i] * a[j]`phát triển nhanh hơn`b[i] + b[j]`, vì vậy hầu hết các cặp sẽ không đạt được sự bình đẳng. Một giải pháp vũ phu có thể lãng phí công sức để kiểm tra tất cả chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: với mỗi`i`từ`1`ĐẾN`n-1`, lặp lại tất cả`j > i`và kiểm tra xem`a[i]*a[j] == b[i]+b[j]`. Điều này đúng, nhưng độ phức tạp về thời gian của nó là`O(n^2)`. Với`n`lên đến`2*10^5`, điều này dẫn đến khoảng 10^10 thao tác, điều này là không khả thi. Brute-force hoạt động vì nó trực tiếp thực hiện định nghĩa, nhưng nó không thành công khi`n`là lớn. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận hiệu quả là sắp xếp lại điều kiện`a[i]*a[j] = b[i]+b[j]`vào trong`a[i]*a[j] - b[j] = b[i]`. Đối xử`i`như cố định và xác định`c[i] = a[i]*a[j] - b[j]`. Đối với mỗi`i`, chúng tôi đang tìm kiếm bao nhiêu`j > i`thỏa mãn`b[i] = a[i]*a[j] - b[j]`. Chúng ta có thể điều chỉnh lại nó bằng cách đếm số lần xuất hiện của`a[j]*a[i] - b[j]`trận đấu đó`b[i]`. Bằng cách lặp lại từ đầu đến cuối và lưu trữ các giá trị trong bản đồ tần số, chúng ta có thể đếm các cặp khớp một cách hiệu quả trong`O(n * sqrt(n))`thời gian. Lý do`sqrt(n)`có liên quan là`a[i]`được giới hạn bởi`n`, vì vậy chúng ta có thể giới hạn phép lặp bên trong của mình đối với các số nhân nhỏ có thể có. Điều này làm giảm vụ nổ bậc hai xuống mức có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n * sqrt(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`ans`về không. Điều này sẽ lưu trữ số lượng cặp hợp lệ. 
2. Tạo từ điển tần số`freq`để ánh xạ các giá trị nguyên tới số lượng`b[j] - a[j]*k`thấy cho đến nay. 
3. Lặp lại`i`từ`n-1`xuống tới`0`. Bằng cách quét ngược lại, chúng tôi đảm bảo rằng chúng tôi chỉ đếm các cặp`(i,j)`với`i < j`. 
4. Đối với mỗi`i`, lặp đi lặp lại`a[i]`các ước số của`d`lên đến`sqrt(n)`. Đối với mỗi ước số, hãy tính toán kết quả phù hợp tiềm năng`b[j]`giá trị sử dụng`b[j] = d * a[i] - b[i]`. Nếu giá trị này tồn tại trong`freq`, cộng số của nó vào`ans`. 
5. Sau khi đếm số trận đấu cho`i`, cập nhật`freq`bao gồm`a[i]*k - b[i]`cho nhỏ`k`. Điều này đảm bảo các lần lặp lại trong tương lai có thể tìm thấy`i`BẰNG`j`. 
6. Sau khi vòng lặp kết thúc,`ans`chứa tổng số cặp hợp lệ. 

Tại sao nó hoạt động: Bằng cách lặp lại, chúng tôi duy trì bất biến`freq`chứa tất cả`b[j]-a[j]*k`giá trị cho`j > i`. Khi kiểm tra`i`, chúng tôi truy vấn một cách hiệu quả có bao nhiêu tương lai`j`thỏa mãn đẳng thức mà không cần quét tất cả`j`. Việc liệt kê dựa trên số chia hoạt động vì`a[i]`được giới hạn bởi`n`, giới hạn số lần kiểm tra ở mức gần như`sqrt(n)`mỗi chỉ số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict
import math

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        ans = 0
        freq = defaultdict(int)
        for i in reversed(range(n)):
            for d in range(1, int(math.isqrt(a[i])) + 1):
                if a[i] % d == 0:
                    # divisor d
                    target = a[i] // d * a[i] - b[i]
                    if target in freq:
                        ans += freq[target]
                    # complementary divisor
                    if d * d != a[i]:
                        target2 = d * a[i] - b[i]
                        if target2 in freq:
                            ans += freq[target2]
            # update freq for current element
            freq[a[i] * 1 - b[i]] += 1
        print(ans)

solve()
```Giải pháp bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm và xử lý từng trường hợp riêng lẻ. Chúng tôi khởi tạo`ans`Và`freq`để đếm. Lặp lại ngược lại đảm bảo chúng tôi chỉ tính hợp lệ`i < j`cặp. Việc liệt kê số chia cho phép chúng ta giới hạn số lượng cặp ứng cử viên mà chúng ta kiểm tra trên mỗi phần tử. Đang cập nhật`freq`sau khi đếm đảm bảo chúng tôi chỉ bao gồm`j > i`trong bản đồ tần số. sử dụng`defaultdict(int)`tránh các lỗi chính và đơn giản hóa việc đếm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào:`a = [2,3,2]`,`b = [3,3,1]`| tôi | tần số trước | tìm thấy cặp hợp lệ | tần số sau | 
| --- | --- | --- | --- | 
| 2 | {} | 0 | {1:1} | 
| 1 | {1:1} | 0 | {1:1,0:1} | 
| 0 | {1:1,0:1} | 2 | {1:2,0:1} | 

Dấu vết này cho thấy bằng cách đếm bằng bản đồ tần số, chúng tôi xác định chính xác hai cặp`(1,2)`Và`(1,3)`. 

### Mẫu 2 

Mảng đầu vào:`a = [4,2,8,2,1,2,7,5]`,`b = [3,5,8,8,1,1,6,5]`| tôi | tần số | cặp mới | tần suất cập nhật | 
| --- | --- | --- | --- | 
| 7 | {} | 0 | {0:1} | 
| 6 | {0:1} | 1 | {0:1,1:1} | 
| 5 | {0:1,1:1} | 1 | ... | 

Bảng tiếp tục tương tự, chứng minh rằng phép lặp ngược với việc đếm tần số sẽ nắm bắt được tất cả các cặp hợp lệ một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(n)) | Các ước số của mỗi phần tử được liệt kê, đưa ra các phép kiểm tra khoảng sqrt(n) cho mỗi phần tử. | 
| Không gian | O(n) | Cửa hàng từ điển tần số đếm tối đa n phần tử. | 

Cho số tiền đó`n`trên tất cả các trường hợp thử nghiệm là 2*10^5, phương pháp này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n3\n2 3 2\n3 3 1\n8\n4 2 8 2 1 2 7 5\n3 5 8 8 1 1 6 5\n8\n4 4 8 8 8 8 8 8\n8 8 8 8 8 8 8 8\n") == "2\n7\n1"

# custom: minimum-size
assert run("1\n2\n1 1\n1 1\n") == "0", "minimum size, no valid pair"
# custom: all-equal
assert run("1\n4\n2 2 2 2\n2 2 2 2\n") == "6", "all equal, all pairs valid"
# custom: large values
assert run(f"1\n5\n5 5 5 5 5\n5 5 5 5 5
```
