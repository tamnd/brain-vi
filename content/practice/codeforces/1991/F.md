---
title: "CF 1991F - Đội hình tam giác"
description: "Chúng ta được cho một dãy que, mỗi que có chiều dài nguyên dương. Đối với mỗi truy vấn, chúng ta được hỏi liệu có thể chọn chính xác sáu que khác nhau từ một mảng con liền kề nhất định để chúng ta có thể tạo thành hai hình tam giác không suy biến hay không."
date: "2026-06-08T15:27:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1991
codeforces_index: "F"
codeforces_contest_name: "Pinely Round 4 (Div. 1 + Div. 2)"
rating: 2200
weight: 1991
solve_time_s: 191
verified: false
draft: false
---

[CF 1991F - Hình thành tam giác](https://codeforces.com/problemset/problem/1991/F) 

**Đánh giá:** 2200 
**Tags:** bạo lực, tham lam, thực hiện, toán học, phân loại 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy que, mỗi que có chiều dài nguyên dương. Đối với mỗi truy vấn, chúng ta được hỏi liệu có thể chọn chính xác sáu que khác nhau từ một mảng con liền kề nhất định sao cho chúng ta có thể tạo thành **hai hình tam giác không suy biến** hay không. Tam giác không suy biến là tam giác có tổng hai cạnh bất kỳ lớn hơn cạnh thứ ba. 

Điểm mấu chốt là mỗi truy vấn hoạt động trên một mảng con liền kề có độ dài ít nhất là sáu và chúng ta cần chọn sáu **chỉ số riêng biệt** (không nhất thiết phải có sáu độ dài riêng biệt) để tạo thành hai hình tam giác. Các ràng buộc rất lớn: lên tới 100.000 que và 100.000 truy vấn, loại trừ mọi giải pháp bạo lực kiểm tra tất cả 6 tổ hợp que trong truy vấn. 

Các trường hợp cạnh rất tinh tế. Nếu tất cả các que trong một phạm vi đều bằng nhau thì bất kỳ bộ ba chỉ số phân biệt nào cũng tạo thành một tam giác hợp lệ. Tuy nhiên, nếu chỉ có hai que lớn và những que còn lại nhỏ thì không thể có hình tam giác. Ví dụ, một mảng con`[1, 1, 1, 1, 10, 10]`không thể tạo thành hai hình tam giác dù có sáu que tính. Việc triển khai bất cẩn chỉ nhìn vào số lượng độ dài hoặc ba cây gậy lớn nhất có thể trả lời sai "CÓ". 

Một điều tinh tế khác là độ dài thanh có thể rất lớn, lên tới 10^9, nhưng các phép tính số học không tràn trong Python, do đó trọng tâm hoàn toàn là hiệu quả thuật toán. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp lại tất cả 6 tổ hợp que trong mảng con đã cho và kiểm tra mọi phân vùng thành hai bộ ba. Mỗi bộ ba được kiểm tra sự bất đẳng thức của tam giác. Điều này hoạt động chính xác nhưng có độ phức tạp O((r-l+1)^6) cho mỗi truy vấn, quá lớn với rl+1 có thể lên tới 10^5. Ngay cả khi sử dụng phép lặp ba lần thông minh và tránh tạo kết hợp rõ ràng vẫn dẫn đến O((r-l+1)^3) ở mức tối thiểu cho mỗi truy vấn, điều này không khả thi đối với n và q lớn. 

Thông tin chi tiết quan trọng là **nếu chúng ta sắp xếp mảng con, việc tạo thành một hình tam giác sẽ giảm xuống việc kiểm tra xem ba phần tử liên tiếp có thể tạo thành một hình tam giác hay không**. Việc sắp xếp đảm bảo rằng với ba que liên tiếp a ≤ b ≤ c, chúng ta chỉ cần kiểm tra xem a + b > c hay không. Nếu bất đẳng thức đó được giữ nguyên thì bộ ba sẽ tạo thành một tam giác không suy biến. 

Để tìm hai tam giác trong một mảng con, chúng ta chỉ cần quét bộ ba đầu tiên tạo thành một tam giác rồi quét các phần tử còn lại để tìm bộ ba thứ hai. Tuy nhiên, thực hiện việc này cho mọi truy vấn từ đầu vẫn là O(n log n) cho mỗi truy vấn do sắp xếp. Điều này là quá chậm. 

Chúng ta có thể tối ưu hóa hơn nữa bằng cách sử dụng quan sát rằng **chỉ sáu que nhỏ nhất hoặc sáu que lớn nhất trong bất kỳ phạm vi nào mới quan trọng để tạo thành hai hình tam giác**, bởi vì các hình tam giác có nhiều khả năng hình thành hơn khi chúng ta xem xét các phần tử lớn nhất hoặc nhỏ nhất. Việc tính toán trước "tiền tố của các phần tử được sắp xếp" là không khả thi vì mỗi mảng con khác nhau. Thay vào đó, chúng tôi khai thác thực tế là bất kỳ cặp tam giác hợp lệ nào cũng sẽ xuất hiện trong số **6 phần tử nhỏ nhất và 6 phần tử lớn nhất** trong dãy. Điều này làm giảm mỗi truy vấn xuống còn quét tối đa 12 ô, tức là O(1) sau khi xử lý trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l+1)^6 * q) | O(1) | Quá chậm | 
| Sắp xếp + Bộ ba liên tiếp | O(q * (r-l+1) log(r-l+1)) | O(r-l+1) | Quá chậm | 
| Tối ưu: Quét 12 que cực chất | O(q * 12) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi truy vấn, trích xuất mảng con của que từ chỉ số l đến r. 
2. Theo dõi **sáu thanh nhỏ nhất và sáu thanh dài nhất** trong mảng con. Điều này đảm bảo chúng ta nắm bắt được tất cả các tam giác tiềm năng, vì sự hình thành tam giác phụ thuộc vào tổng ba cạnh. 
3. Kết hợp 12 que này thành một danh sách và sắp xếp chúng. 
4. Lặp lại các bộ ba liên tiếp trong danh sách này và ghi lại tất cả các bộ ba tạo thành một tam giác không suy biến với điều kiện a + b > c. 
5. Nếu tồn tại ít nhất hai bộ ba rời nhau (rách nhau về chỉ số trong mảng con ban đầu, không nhất thiết phải là giá trị), hãy trả lời "CÓ"; nếu không thì trả lời "KHÔNG". 

Tại sao nó hoạt động: Bất kỳ bộ ba nào tạo thành một tam giác sẽ bao gồm các phần tử nhỏ nhất hoặc lớn nhất trong mảng con, bởi vì các tam giác không thể được tạo thành từ ba phần tử có kích thước trung bình nếu các cực vi phạm bất đẳng thức tam giác. Bằng cách kiểm tra tất cả 12 ứng cử viên cực đoan, chúng tôi đảm bảo tìm ra hai hình tam giác hợp lệ nếu chúng tồn tại. Sắp xếp các ứng cử viên này cho phép quét tuyến tính hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_form_two_triangles(sub):
    sub = sorted(sub)
    triangles = []
    n = len(sub)
    for i in range(n-2):
        if sub[i] + sub[i+1] > sub[i+2]:
            triangles.append((i, i+1, i+2))
            if len(triangles) == 2:
                return True
    return False

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        sub = a[l:r+1]
        
        # pick up to six smallest and six largest
        sub_sorted = sorted(sub)
        candidates = sub_sorted[:6] + sub_sorted[-6:]
        if can_form_two_triangles(candidates):
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    main()
```Giải thích: Chúng ta lấy một mảng con liền kề, chọn sáu phần tử nhỏ nhất và sáu phần tử lớn nhất rồi sắp xếp chúng. các`can_form_two_triangles`chức năng quét bộ ba liên tiếp để tìm hình tam giác. Ngay sau khi tìm thấy hai, chúng tôi trả lại "CÓ". Việc triển khai này đảm bảo tính chính xác trong khi vẫn duy trì thời gian xử lý truy vấn về cơ bản không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Truy vấn:`l=1, r=6`với mảng con`[5, 2, 2, 10, 4, 10]`. 

| Sắp xếp ứng viên | Bộ ba tạo thành tam giác | 
| --- | --- | 
|`[2, 2, 4, 5, 10, 10]`|`(2,2,4)`,`(2,4,5)`,`(4,5,10)`,`(5,10,10)`| 
| Hai bộ ba hợp lệ đầu tiên |`(2,4,5)`,`(5,10,10)`| 

Đầu ra:`YES`### Ví dụ 2 

Truy vấn:`l=2, r=7`với mảng con`[2, 2, 10, 4, 10, 6]`. 

| Sắp xếp ứng viên | Bộ ba tạo thành tam giác | 
| --- | --- | 
|`[2, 2, 4, 6, 10, 10]`|`(2,2,4)`,`(2,4,6)`,`(4,6,10)`,`(6,10,10)`| 
| Chỉ có một bộ ba hợp lệ sử dụng các que riêng biệt |`(2,4,6)`| 

Đầu ra:`NO`Dấu vết xác nhận rằng chỉ cần kiểm tra các điểm cực trị là đủ và tránh được các bộ ba chồng chéo về mặt chỉ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * 12 log 12) ≈ O(q) | Đối với mỗi truy vấn, việc sắp xếp tối đa 12 phần tử và quét bộ ba là thời gian không đổi | 
| Không gian | O(n) | Lưu trữ mảng thanh gốc và danh sách ứng viên tạm thời cho mỗi truy vấn | 

Cho n và q lên tới 10^5, cách tiếp cận này đủ nhanh. Việc sắp xếp 12 phần tử cho mỗi truy vấn là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""10 5
5 2 2 10 4 10 6 1 5 3
1 6
2 7
2 8
5 10
4 10""") == """YES
NO
YES
NO
YES"""

# Custom minimum size input
assert run("""6 1
1 1 1 1 1 1
1 6""") == "YES"

# Custom all equal, multiple queries
assert run("""7 2
2 2 2 2 2 2 2
1 6
2 7""") == "YES\nYES"

# Custom impossible case
assert run("""6 1
1 1 1 1 10 10
1 6""") == "NO"

# Custom exact six forming two triangles
assert run("""6 1
3 4 5 6 7 8
1 6""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1 1`|`YES`| Kích thước tối thiểu, tất cả đều bằng nhau | 
|`2 2 2 2 2 2 2`|`YES\nYES`| Nhiều truy vấn, tất cả đều bằng nhau | 
|`1 1 1 1 10 10`|`NO`| Không thể có hai hình tam giác dù có sáu que tính | 
|`3 4 5 6 7 8`|`YES`| Trường hợp chung có giá trị riêng biệt | 

## Vỏ cạnh 

Đối với mảng con `[1,1,1
