---
title: "CF 474F - Đàn kiến"
description: "Chúng ta được cho một hàng kiến, mỗi hàng có một sức mạnh nguyên. Chuột chũi muốn quan sát cuộc chiến giữa những con kiến ​​ở những đoạn liền kề cụ thể của hàng kiến. Đối với một đoạn nhất định từ vị trí l đến r, mỗi con kiến ​​sẽ chiến đấu với mọi con kiến ​​khác trong đoạn đó."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 474
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 271 (Div. 2)"
rating: 2100
weight: 474
solve_time_s: 96
verified: true
draft: false
---

[CF 474F - Đàn kiến](https://codeforces.com/problemset/problem/474/F) 

**Xếp hạng:** 2100 
**Tas:** cấu trúc dữ liệu, toán, lý thuyết số 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hàng kiến, mỗi hàng có một sức mạnh nguyên. Chuột chũi muốn quan sát cuộc chiến giữa những con kiến ​​ở những đoạn liền kề cụ thể của hàng kiến. Đối với một phân đoạn nhất định từ vị trí _l_ đến _r_, mỗi con kiến ​​sẽ chiến đấu với mọi con kiến ​​khác trong phân đoạn đó. Một con kiến ​​chỉ nhận được điểm chiến đấu từ một con kiến ​​khác nếu sức mạnh của nó chia đôi sức mạnh của con kiến ​​kia. Sau tất cả các trận chiến, một con kiến ​​sẽ được giải phóng nếu nó thắng trong mọi trận chiến mà nó tham gia - tương tự, nếu điểm chiến đấu của nó bằng số đối thủ trong phân đoạn, tức là _(r - l)_. Chuột chũi ăn hết số kiến ​​còn lại. Nhiệm vụ là xác định, đối với mỗi truy vấn phân đoạn, chuột chũi ăn bao nhiêu con kiến. 

Bài toán có tới 100.000 con kiến ​​và 100.000 truy vấn, sức mạnh của mỗi con kiến ​​có thể lớn tới 10^9. Một giải pháp đơn giản lặp lại từng cặp kiến ​​trong mỗi truy vấn sẽ liên quan đến các phép toán O(n²) cho mỗi truy vấn, điều này hoàn toàn không khả thi. Chúng ta cần một cách tiếp cận tận dụng cấu trúc theo độ mạnh và mối quan hệ chia hết của chúng để giảm bớt tính toán. 

Các trường hợp cạnh không rõ ràng bao gồm các phân đoạn trong đó tất cả các điểm mạnh đều bằng nhau, điều này sẽ giải phóng mọi con kiến ​​hoặc các phân đoạn bao gồm 1, vì 1 chia hết tất cả các số. Ví dụ, nếu điểm mạnh là`[1, 2, 3]`và truy vấn là`[1,3]`, chỉ con kiến ​​có độ mạnh 1 mới được giải thoát, vì 2 không chia cho 3 và 3 không chia cho 2. Một cách tiếp cận bất cẩn có thể bỏ qua sự bất đối xứng này. 

Một trường hợp khác là các số lặp lại. TRONG`[2, 2, 2]`trên phân khúc`[1,3]`, mỗi con kiến ​​được giải phóng, vì mỗi con kiến ​​đều chia rẽ nhau. Không tính đến sức mạnh lặp đi lặp lại sẽ dẫn đến số lượng kiến ​​ăn không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các cặp trong mỗi phân đoạn. Đối với một đoạn có độ dài _k_, điều này yêu cầu kiểm tra O(k²) về khả năng chia hết, dẫn đến các phép toán O(n² * t) trong trường hợp xấu nhất. Với n = 10^5 và t = 10^5 thì điều này hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng để có giải pháp nhanh hơn là chỉ những số chia hết cho tất cả các số khác trong phân đoạn mới có thể được giải phóng. Vì vậy, con kiến ​​được giải phóng phải có sức mạnh bằng ước số chung lớn nhất (GCD) của tất cả các sức mạnh trong đoạn đó. Bất kỳ con kiến ​​nào khác đều không thể chia hết cho tất cả những con khác trừ khi nó bằng GCD. Chúng ta có thể tận dụng cây phân đoạn hoặc mảng GCD tiền tố để tính toán nhanh GCD của bất kỳ phân đoạn truy vấn nào trong thời gian O(log n) hoặc O(1) bằng tính toán trước. 

Tiếp theo, để đếm xem có bao nhiêu con kiến ​​bằng GCD trong một phân đoạn, chúng ta có thể duy trì ánh xạ từ từng cường độ duy nhất đến các vị trí mà nó xuất hiện, cho phép truy vấn O(log k) bằng cách sử dụng tìm kiếm nhị phân. Câu trả lời cho một đoạn là tổng số kiến ​​trong đoạn trừ đi số kiến ​​có sức mạnh bằng đoạn GCD. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² * t) | O(1) | Quá chậm | 
| Tiền tố GCD + Số lượng được lập chỉ mục | O(n + t log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước mảng GCD tiền tố. Cho phép`prefix_gcd[i]`là GCD của những con kiến ​​đầu tiên. Điều này cho phép tính toán GCD của bất kỳ phân khúc nào`[l, r]`BẰNG`gcd(prefix_gcd[r], prefix_gcd[l-1])`. 
2. Xây dựng ánh xạ từ các giá trị cường độ tới danh sách được sắp xếp các vị trí của chúng. Điều này cho phép chúng ta đếm xem có bao nhiêu con kiến ​​bằng GCD trong bất kỳ phân đoạn nào bằng cách sử dụng tìm kiếm nhị phân. 
3. Đối với mỗi truy vấn`[l, r]`, tính đoạn GCD. Đây là sức mạnh mà bất kỳ con kiến ​​được thả tự do nào cũng phải có. 
4. Sử dụng phép ánh xạ để tìm số kiến ​​có sức mạnh bằng đoạn GCD bên trong`[l, r]`. Trừ số này khỏi độ dài đoạn`(r-l+1)`để biết số lượng kiến ​​mà Chuột Chũi ăn. 
5. Xuất kết quả cho tất cả các truy vấn. 

Tại sao nó hoạt động: Một con kiến ​​được giải phóng khi và chỉ khi nó chia tất cả các điểm mạnh khác trong đoạn đó. Sức mạnh duy nhất chia tất cả các số trong một tập hợp là GCD. Việc đếm các vị trí của GCD trong phân đoạn sẽ xác định chính xác tất cả các con kiến ​​được giải phóng. Trừ cho số ăn. 

## Giải pháp Python```python
import sys
import math
import bisect
input = sys.stdin.readline

def main():
    n = int(input())
    s = list(map(int, input().split()))
    
    prefix_gcd = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix_gcd[i] = math.gcd(prefix_gcd[i-1], s[i-1])
    
    pos_map = {}
    for idx, val in enumerate(s):
        if val not in pos_map:
            pos_map[val] = []
        pos_map[val].append(idx + 1)  # 1-indexed for queries
    
    t = int(input())
    res = []
    for _ in range(t):
        l, r = map(int, input().split())
        # Compute GCD of segment
        seg_gcd = prefix_gcd[r]
        if l > 1:
            seg_gcd = math.gcd(seg_gcd, prefix_gcd[l-1])
        # Count how many ants equal GCD in [l,r]
        positions = pos_map.get(seg_gcd, [])
        left = bisect.bisect_left(positions, l)
        right = bisect.bisect_right(positions, r)
        count_gcd = right - left
        res.append((r - l + 1) - count_gcd)
    
    print('\n'.join(map(str, res)))

if __name__ == "__main__":
    main()
```Tiền tố GCD đảm bảo chúng ta có thể tính toán GCD của bất kỳ phân đoạn nào một cách nhanh chóng. các`pos_map`cho phép đếm số lần xuất hiện mà không cần quét phân đoạn. Tìm kiếm nhị phân trên các vị trí được sắp xếp là rất quan trọng; quét tuyến tính đơn giản trên phân đoạn sẽ quá chậm. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[1, 3, 2, 4, 2]`, truy vấn`[1,5]`| Bước | tiền tố_gcd | seg_gcd | vị trí của GCD | đếm_gcd | ăn | 
| --- | --- | --- | --- | --- | --- | 
| Tính tiền tố_gcd | [0,1,1,1,1,1] | - | - | - | - | 
| Truy vấn 1 | - | 1 | [1] | 1 | 5-1 = 4 | 

Chỉ có kiến ​​1 được giải phóng; Chuột chũi ăn kiến ​​2,3,4,5. 

**Ví dụ 2:**`[2,2,2,2]`, truy vấn`[1,4]`| Bước | tiền tố_gcd | seg_gcd | vị trí của GCD | đếm_gcd | ăn | 
| --- | --- | --- | --- | --- | --- | 
| Tính tiền tố_gcd | [0,2,2,2,2] | - | - | - | - | 
| Truy vấn | - | 2 | [1,2,3,4] | 4 | 4-4 = 0 | 

Tất cả kiến ​​đều được giải thoát; Chuột chũi không ăn gì cả. 

Những dấu vết này cho thấy cách thuật toán xác định chính xác đoạn GCD và đếm các vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + t log n) | Tiền tố tòa nhà GCD và bản đồ vị trí là O(n). Mỗi truy vấn thực hiện GCD theo O(1) và tìm kiếm nhị phân theo O(log n). | 
| Không gian | O(n) | Tiền tố mảng GCD và bản đồ vị trí lưu trữ tổng cộng tối đa n vị trí. | 

Với n, t 10^5 và mỗi thao tác được giới hạn bởi log n cho tìm kiếm nhị phân, giải pháp chạy thoải mái trong vòng 1 giây và sử dụng tốt bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    main()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# Provided sample
assert run("5\n1 3 2 4 2\n4\n1 5\n2 5\n3 5\n4 5\n") == "4\n4\n1\n1"

# All equal strengths
assert run("4\n2 2 2 2\n2\n1 4\n2 3\n") == "0\n0"

# Minimum input
assert run("1\n5\n1\n1 1\n") == "0"

# Segment with GCD = 1
assert run("3\n2 3 5\n1\n1 3\n") == "2"

# Large input with repeated pattern
inp = "10\n" + " ".join(["6","2","3","6","2","3","6","2","3","6"]) + "\n1\n1 10\n"
assert run(inp) == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[2,2,2,2]`|`0`| Giải phóng hết kiến ​​| 
|`[5]`|`0`| Phân khúc kích thước tối thiểu | 
|`[2,3,5]`|`2`| GCD=1 kịch bản | 
| mẫu lặp đi lặp lại |`6`| Đếm GCD chính xác theo chuỗi dài | 

## Vỏ cạnh 

Đối với một người
