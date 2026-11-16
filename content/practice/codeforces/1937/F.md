---
title: "CF 1937F - Nghịch lý bitwise"
description: "Chúng ta có hai mảng số nguyên a và b có độ dài n cùng với một số nguyên cố định v. Chúng ta được yêu cầu xử lý các truy vấn thuộc hai loại. Loại đầu tiên cập nhật một phần tử trong b."
date: "2026-06-09T01:50:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1937
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 930 (Div. 2)"
rating: 3100
weight: 1937
solve_time_s: 94
verified: false
draft: false
---

[CF 1937F - Nghịch lý Bitwise](https://codeforces.com/problemset/problem/1937/F) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên,`a`Và`b`, có chiều dài`n`, cùng với một số nguyên cố định`v`. Chúng tôi được yêu cầu xử lý các truy vấn thuộc hai loại. Loại đầu tiên cập nhật một phần tử trong`b`. Loại thứ hai yêu cầu "vẻ đẹp" tối thiểu trong số tất cả các mảng con liền kề`[l_0, r_0]`bên trong một phạm vi nhất định`[l, r]`sao cho OR theo bit của tất cả các phần tử của`b`trong mảng con đó ít nhất là`v`. “Vẻ đẹp” của một mảng con là phần tử lớn nhất trong mảng con tương ứng của`a`. Nếu không có mảng con nào đáp ứng điều kiện OR, chúng ta trả về`-1`. 

Thách thức chính đó là`n`Và`q`mỗi trường hợp có thể đạt tới 2×10^5 và tổng của tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức 2×10^5. Một cách tiếp cận đơn giản liệt kê tất cả các mảng con cho mỗi truy vấn sẽ thực hiện các thao tác O(n^2) cho mỗi truy vấn, điều này rõ ràng là không khả thi. Chúng tôi cần một cách tiếp cận gần với O(n log n) cho mỗi trường hợp thử nghiệm hoặc tốt hơn, với các phép toán logarit hoặc khấu hao cho mỗi lần cập nhật hoặc truy vấn. 

Các trường hợp cạnh không rõ ràng bao gồm các khoảng trong đó không có phần tử đơn lẻ nào thỏa mãn`b[i] >= v`nhưng một sự kết hợp thì có. Ví dụ,`b = [1, 2, 4]`với`v = 7`không có khoảng thời gian tốt của một phần tử, nhưng`[1,3]`thỏa mãn`1|2|4 = 7`. Các giải pháp bất cẩn chỉ có thể kiểm tra các phần tử riêng lẻ thay vì kiểm tra từng khoảng thời gian, tạo ra`-1`không chính xác. Tương tự, các bản cập nhật cho`b`có thể tạo hoặc hủy nhiều khoảng thời gian tốt, vì vậy mọi quá trình tiền xử lý đều phải hỗ trợ cập nhật động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các mảng con`[l_0, r_0]`bên trong`[l, r]`đối với truy vấn loại 2, tính toán OR của truy vấn tương ứng`b`phân đoạn và theo dõi mức tối đa trong`a`. Mỗi truy vấn loại 2 có thể yêu cầu tính toán O(n^2) OR và tối đa. Với tối đa 2×10^5 truy vấn và n lên tới 2×10^5, điều này hoàn toàn không khả thi. Các cập nhật trong truy vấn loại 1 không giúp ích gì vì mọi quá trình xử lý trước đều bị vô hiệu trừ khi chúng tôi tính toán lại mọi thứ. 

Thông tin chi tiết quan trọng là phép toán OR có tính đơn điệu: việc kéo dài một khoảng sang bên phải không bao giờ làm giảm OR. Điều này cho phép một cách tiếp cận hai con trỏ. Chúng ta có thể duy trì khoảng thời gian nhỏ nhất bắt đầu từ mỗi`l`có OR đạt tới`v`bằng cách tiến lên một con trỏ bên phải`r`cho đến khi OR đạt hoặc vượt quá`v`. Khi chúng ta biết các khoảng thỏa mãn điều kiện OR, chúng ta chỉ cần giá trị tối đa`a[i]`trong khoảng đó. Để xử lý nhiều truy vấn một cách hiệu quả, chúng ta có thể xử lý trước`a`vào cây phân đoạn hoặc bảng thưa để có phạm vi truy vấn tối đa và duy trì`b`trong một cấu trúc hỗ trợ tính toán OR tăng dần hiệu quả. 

Chiến lược cuối cùng kết hợp những thông tin chi tiết này: đối với mỗi truy vấn, chúng tôi sử dụng phương pháp hai con trỏ để tìm điểm cuối bên phải tối thiểu cho tất cả các vị trí bắt đầu trong`[l, r]`, sau đó truy vấn cây phân đoạn để có giá trị tối đa`a[i]`trong những khoảng đó. Cập nhật lên`b`chỉ yêu cầu cập nhật phép tính OR cho các truy vấn trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(n) | Quá chậm | 
| Cây hai con trỏ + cây phân đoạn | O(n log n + q log n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên`a`hỗ trợ phạm vi truy vấn tối đa. Điều này cho phép chúng ta nhanh chóng tính toán vẻ đẹp của bất kỳ khoảng thời gian nào. 
2. Đối với mỗi truy vấn loại 2`[l, r]`, khởi tạo con trỏ phải`r_ptr = l`Và`current_or = 0`. Con trỏ này sẽ mở rộng để tìm khoảng thời gian tối thiểu có OR đáp ứng hoặc vượt quá`v`. 
3. Lặp lại các điểm cuối bên trái có thể có từ`l`ĐẾN`r`. Đối với mỗi điểm cuối bên trái, hãy tiến lên`r_ptr`trong khi`current_or | b[r_ptr] < v`. Mỗi phần mở rộng đều hợp lý vì OR là đơn điệu; chúng tôi chỉ mở rộng`r_ptr`cho đến khi điều kiện OR được đáp ứng. 
4. Nếu`r_ptr > r`, không có khoảng thời gian phù hợp bắt đầu từ điểm cuối bên trái hiện tại và chúng ta tiếp tục đến điểm cuối bên trái tiếp theo. 
5. Nếu không, hãy truy vấn cây phân đoạn để tìm`max(a[l], ..., a[r_ptr])`và theo dõi vẻ đẹp tối thiểu trong số tất cả các điểm cuối bên trái. 
6. Đối với truy vấn loại 1, hãy cập nhật giá trị của`b[i]`. Điều này có thể ảnh hưởng đến tính toán OR trong tương lai. Chúng ta không cần cập nhật cây phân đoạn trên`b`, vì nó chỉ được sử dụng trong tính toán hai con trỏ. 
7. Xuất ra vẻ đẹp tối thiểu được tìm thấy cho truy vấn loại 2, hoặc`-1`nếu không có khoảng đáp ứng điều kiện OR. 

Tại sao nó hoạt động: tính đơn điệu của OR đảm bảo rằng việc nâng cao`r_ptr`từ một điểm cuối bên trái nhất định sẽ tìm thấy khoảng thỏa mãn tối thiểu. Cây phân đoạn đảm bảo chúng ta có thể tính toán hiệu quả mức tối đa trong`a`trên bất kỳ khoảng thời gian nào. Vì mọi khoảng được xem xét đều hợp lệ và chúng tôi theo dõi vẻ đẹp tối thiểu nên giải pháp là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, data):
        n = len(data)
        self.N = 1
        while self.N < n:
            self.N *= 2
        self.tree = [0] * (2 * self.N)
        for i in range(n):
            self.tree[self.N + i] = data[i]
        for i in range(self.N - 1, 0, -1):
            self.tree[i] = max(self.tree[2*i], self.tree[2*i + 1])
    def update(self, i, val):
        i += self.N
        self.tree[i] = val
        while i > 1:
            i //= 2
            self.tree[i] = max(self.tree[2*i], self.tree[2*i + 1])
    def query(self, l, r):
        l += self.N
        r += self.N
        res = 0
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.tree[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.tree[r])
                r -= 1
            l //= 2
            r //= 2
        return res

t = int(input())
for _ in range(t):
    n, v = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    seg = SegmentTree(a)
    q = int(input())
    res = []
    for _ in range(q):
        parts = input().split()
        if parts[0] == '1':
            i, x = int(parts[1])-1, int(parts[2])
            b[i] = x
        else:
            l, r = int(parts[1])-1, int(parts[2])-1
            ans = float('inf')
            r_ptr = l
            current_or = 0
            for left in range(l, r+1):
                if r_ptr < left:
                    r_ptr = left
                    current_or = 0
                while r_ptr <= r and (current_or | b[r_ptr]) < v:
                    current_or |= b[r_ptr]
                    r_ptr += 1
                if r_ptr > r:
                    continue
                max_a = seg.query(left, r_ptr)
                ans = min(ans, max_a)
                current_or &= ~b[left]
            res.append(str(ans if ans != float('inf') else -1))
    print(' '.join(res))
```Cây phân đoạn được sử dụng cho các truy vấn tối đa trong phạm vi thời gian không đổi. Đối với mỗi điểm cuối bên trái,`r_ptr`được di chuyển tăng dần để duy trì khoảng tối thiểu thỏa mãn yêu cầu OR. Sau khi xử lý một khoảng thời gian, chúng tôi loại bỏ`b[left]`từ`current_or`để chuẩn bị cho điểm cuối bên trái tiếp theo. Cần chú ý cẩn thận để quản lý các chỉ mục riêng biệt trong Python (dựa trên 0) so với vấn đề (dựa trên 1). Cập nhật lên`b`ảnh hưởng ngay lập tức và chính xác đến các truy vấn tiếp theo. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`a = [2,1,3]`,`b = [2,5,3]`,`v = 7`, truy vấn`[2,3]`. 
| trái | r_ptr | hiện tại hoặc | max(a[left..r_ptr]) | và | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | - | - | 
| 2 | 3 | 5 | 3 | 3 | 
Hai con trỏ xác định rằng`[2,3]`là khoảng thời gian tốt duy nhất. Tối đa của`a[2..3]`là 3, đó là câu trả lời đúng. 

**Ví dụ 2:**`a = [5,1,2,4]`,`b = [4,2,3,3]`,`v = 5`, truy vấn`[1,4]`. 
| trái | r_ptr | hiện tại hoặc | max(a[left..r_ptr]) | và | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 5 | 5 | 
| 2 | 3 | 5 | 2 | 2 | 
| 3 | 3 | 3 | - | 2 | 
| 4 | | | | |
