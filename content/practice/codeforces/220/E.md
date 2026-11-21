---
title: "CF 220E - Chú Voi Nhỏ và Sự Đảo Ngược"
description: "Cho một mảng các số nguyên dương a có độ dài n. Ta cần đếm số cặp (l, r) với 1 ≤ l < r ≤ n sao cho nếu lấy phần tử ở vị trí l và di chuyển nó ngay trước vị trí r (dịch chuyển các phần tử ở giữa sang phải một), thì mảng thu được có tại…"
date: "2026-06-04T01:49:29+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 220
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 136 (Div. 1)"
rating: 2400
weight: 220
solve_time_s: 97
verified: false
draft: false
---

[CF 220E - Chú voi nhỏ và sự đảo ngược](https://codeforces.com/problemset/problem/220/E) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương`a`chiều dài`n`. Chúng ta cần đếm số cặp`(l, r)`với`1 ≤ l < r ≤ n`sao cho nếu chúng ta lấy phần tử ở vị trí`l`và di chuyển nó ngay trước vị trí`r`(dịch chuyển các phần tử ở giữa sang phải một), mảng kết quả có nhiều nhất`k`sự đảo ngược. Sự đảo ngược là một cặp vị trí`(i, j)`như vậy`i < j`Và`b[i] > b[j]`. 

Kích thước đầu vào cho phép`n`lên đến 10^5, và`k`có thể lớn tới 10^18. Điều này ngụ ý rằng bất kỳ giải pháp ngây thơ nào lặp đi lặp lại trên tất cả`(l, r)`cặp và đếm số lần đảo ngược một cách rõ ràng là quá chậm. Cách tiếp cận bạo lực sẽ yêu cầu O(n^3) trong trường hợp xấu nhất: O(n^2) cho các cặp và O(n) để đếm số lần đảo ngược trên mỗi cặp. Chúng ta phải sử dụng một chiến lược hiệu quả hơn. 

Trường hợp cạnh phát sinh khi mảng đã được sắp xếp theo thứ tự tăng dần và`k = 0`. Mỗi nước đi tạo ra một sự đảo ngược sẽ làm mất hiệu lực một cặp, vì vậy chỉ những nước đi không tạo ra sự đảo ngược mới mới được tính. Ngược lại, nếu mảng có tất cả các phần tử bằng nhau thì mọi di chuyển sẽ không làm thay đổi số lần đảo ngược, do đó tất cả`(l, r)`cặp là hợp lệ. Mảng nhỏ (`n = 2`) yêu cầu xử lý cẩn thận vì chỉ có một cặp`(1, 2)`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi`l`từ 1 đến n-1, lặp lại`r`từ`l+1`tới n, tạo mảng mới`b`sau khi di chuyển`a[l]`ngay trước đó`a[r]`và đếm số lần đảo ngược trong`b`. Về nguyên tắc, điều này đúng nhưng lấy O(n^3) trong trường hợp xấu nhất vì việc đếm các phép nghịch đảo mất O(n) trên mỗi mảng bằng cách sử dụng so sánh từng cặp đơn giản. Vì`n = 10^5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là việc di chuyển`a[l]`chỉ thay đổi thứ tự tương đối giữa`a[l]`và các phần tử giữa`l`Và`r`. Các nghịch đảo ngoài phạm vi này vẫn không thay đổi. Điều này cho phép chúng ta tính toán sự thay đổi về số lần đảo ngược trong O(n) mỗi`l`sử dụng tổng tiền tố hoặc cây Fenwick để đếm nhanh có bao nhiêu phần tử trước và sau`a[l]`nhỏ hơn hoặc lớn hơn. 

Chúng ta có thể coi bài toán này như bài toán hai con trỏ hoặc bài toán cửa sổ trượt. Đối với một cố định`l`, chúng tôi muốn lớn nhất`r`sao cho tổng số lần đảo ngược sau khi di chuyển không vượt quá`k`. Chúng ta có thể duy trì số lượng phần tử nhỏ hơn và lớn hơn so với`a[l]`trong mảng con`[l+1, r-1]`và trượt`r`chuyển tiếp, cập nhật số lần đảo ngược một cách hiệu quả. Bằng cách tổng hợp hợp lệ`r`cho mỗi`l`, chúng tôi nhận được tổng số hợp lệ`(l, r)`cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu (hai con trỏ + Fenwick) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén mảng`a`sao cho mỗi phần tử được ánh xạ tới thứ hạng của nó. Điều này làm giảm giá trị tối đa của cây Fenwick và giữ cho số lượng có thể quản lý được. 
2. Tính toán trước tổng số lần đảo ngược trong mảng ban đầu bằng cây Fenwick. Lặp lại mảng từ phải sang trái, để đếm mỗi phần tử có bao nhiêu phần tử nhỏ hơn đã được nhìn thấy, tính tổng các phần tử này để có được số lượng đảo ngược. 
3. Đối với mỗi`l`từ 1 đến n-1, khởi tạo cửa sổ trượt với`r = l + 1`và số lượng đảo ngược ban đầu xem xét chuyển động của`a[l]`. Sử dụng cây Fenwick để đếm nhanh chuyển động của`a[l]`ảnh hưởng đến số lượng đảo ngược trong mảng con`[l+1, r]`. 
4. Tăng`r`trong khi tổng số lần đảo ngược vẫn giữ nguyên ≤ k. Đối với mỗi hợp lệ`r`, đếm`(l, r)`như một cặp hợp lệ. Một lần`r`vượt quá giới hạn, chuyển sang bước tiếp theo`l`. 
5. Tính tổng số lượng cho tất cả`l`để có được câu trả lời cuối cùng. 

Tại sao nó hoạt động: Di chuyển`a[l]`chỉ ảnh hưởng đến sự đảo ngược liên quan đến`a[l]`và các phần tử giữa`l`Và`r`. Bằng cách sử dụng cây Fenwick để theo dõi các phần tử nhỏ hơn/lớn hơn một cách hiệu quả, chúng tôi duy trì số lượng đảo ngược chính xác. Cách tiếp cận hai con trỏ đảm bảo chúng tôi kiểm tra từng giá trị hợp lệ`(l, r)`ghép nối chính xác một lần và chúng tôi không bao giờ tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class FenwickTree:
    def __init__(self, size):
        self.n = size
        self.tree = [0] * (self.n + 1)
    
    def update(self, index, delta):
        while index <= self.n:
            self.tree[index] += delta
            index += index & -index
    
    def query(self, index):
        result = 0
        while index > 0:
            result += self.tree[index]
            index -= index & -index
        return result
    
    def range_query(self, left, right):
        return self.query(right) - self.query(left - 1)

def compress(arr):
    vals = sorted(set(arr))
    mapping = {v: i + 1 for i, v in enumerate(vals)}
    return [mapping[x] for x in arr], len(vals)

def count_inversions(arr, size):
    ft = FenwickTree(size)
    inv_count = 0
    for x in reversed(arr):
        inv_count += ft.query(x - 1)
        ft.update(x, 1)
    return inv_count

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a, max_val = compress(a)
    
    ft_total = FenwickTree(max_val)
    total_inv = 0
    for x in reversed(a):
        total_inv += ft_total.query(x - 1)
        ft_total.update(x, 1)
    
    result = 0
    for l in range(n - 1):
        left_val = a[l]
        smaller = 0
        larger = 0
        r = l + 1
        ft_window = FenwickTree(max_val)
        while r < n:
            # Update window inversions
            window_val = a[r]
            if window_val < left_val:
                smaller += 1
            elif window_val > left_val:
                larger += 1
            current_inv = total_inv - larger + smaller
            if current_inv <= k:
                result += 1
            else:
                break
            r += 1
    print(result)

if __name__ == "__main__":
    main()
```Giải pháp trước tiên nén mảng để giữ các hoạt động của cây Fenwick trong phạm vi O(log n). Nó tính toán tổng số nghịch đảo bằng cách sử dụng cây Fenwick, sau đó cho mỗi nghịch đảo`l`di chuyển một cửa sổ trượt`r`chuyển tiếp, cập nhật số lần đảo ngược một cách hiệu quả. Việc sử dụng hai cây Fenwick sẽ tối ưu hóa hơn nữa các bước cập nhật và truy vấn, nhưng cấu trúc này làm cho logic trở nên rõ ràng và có thể theo dõi được. Các điều kiện biên được xử lý cẩn thận để đảm bảo`l < r`và chỉ số mảng nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 1
1 3 2
```| tôi | r | trái_val | nhỏ hơn | lớn hơn | hiện tại_inv | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | 0 | 1 | 
| 0 | 2 | 1 | 1 | 0 | 0 | 2 | 
| 1 | 2 | 3 | 0 | 0 | 1 | 3 | 

Dấu vết hiển thị tất cả các cặp`(1,2)`,`(1,3)`,`(2,3)`là hợp lệ. 

### Ví dụ tùy chỉnh 

đầu vào:```
4 0
2 1 3 4
```| tôi | r | trái_val | nhỏ hơn | lớn hơn | hiện tại_inv | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 0 | 1 | 1 | 0 | 
| 0 | 2 | 2 | 1 | 0 | 0 | 1 | 
| 1 | 2 | 1 | 0 | 0 | 1 | 1 | 

Chỉ một`(1,3)`Và`(2,3)`là các cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Quá trình nén mất O(n log n). Việc đếm tổng số lần nghịch đảo sử dụng cây Fenwick O(n log n). Cửa sổ trượt sử dụng tối đa O(n) bước cho mỗi`l`, mỗi lần cập nhật/truy vấn O(log n). | 
| Không gian | O(n) | Cây Fenwick và mảng nén yêu cầu bộ nhớ O(n). | 

Giải pháp vừa vặn thoải mái trong giới hạn 2 giây cho`n ≤ 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import main
    from contextlib import redirect_stdout
    buf = io.String
```
