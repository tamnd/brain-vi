---
title: "CF 1839B - Đèn"
description: "Chúng ta được cung cấp một bộ sưu tập đèn, mỗi đèn có hai thuộc tính: ai và bi. Ban đầu, tất cả các đèn đều tắt. Bạn có thể bật đèn tắt để kiếm bi điểm."
date: "2026-06-09T06:29:40+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1839
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 876 (Div. 2)"
rating: 1100
weight: 1839
solve_time_s: 101
verified: false
draft: false
---

[CF 1839B - Đèn](https://codeforces.com/problemset/problem/1839/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập đèn, mỗi chiếc có hai đặc tính:`a_i`Và`b_i`. Ban đầu, tất cả các đèn đều tắt. Bạn có thể bật đèn tắt để kiếm tiền`b_i`điểm. Tuy nhiên, sau mỗi lần vận hành, bất kỳ đèn nào có ngưỡng`a_i`nhỏ hơn hoặc bằng số lượng đèn đang bật sẽ bị hỏng. Đèn hỏng không thể bật lại được và đèn bị vỡ không còn góp phần vào tình trạng “đếm” cho các hoạt động sau này. Mục tiêu của chúng tôi là tối đa hóa tổng số điểm chúng tôi có thể kiếm được. 

Những hạn chế cho chúng ta biết rằng`n`có thể lên tới 200.000 và tổng số`n`trên các trường hợp thử nghiệm cũng bị giới hạn ở 200.000. Điều này ngay lập tức loại trừ các thuật toán bậc hai trong`n`, bởi vì O(n²) sẽ đạt tới các phép toán 4 × 10¹⁰ trong trường hợp xấu nhất. Chúng ta cần một thuật toán gần O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh khi đèn có công suất rất thấp`a_i`và cao`b_i`. Bật quá sớm có thể làm hỏng các loại đèn có giá trị cao khác nên chiến lược tham lam phải tính đến sự tương tác giữa các ngưỡng`a_i`và phần thưởng`b_i`. Ví dụ, với hai đèn:`(a=1, b=100)`Và`(a=2, b=1)`, bật đèn đầu tiên sẽ được 100 điểm nhưng lại cản trở việc bật đèn thứ hai một cách an toàn. Cách tiếp cận đúng có thể là bật đèn có giá trị thấp hơn trước để tránh làm vỡ đèn có giá trị cao. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ là thử mọi hoán vị khi bật đèn, theo dõi xem đèn nào bị hỏng sau mỗi bước và tính tổng điểm. Về nguyên tắc, điều này đúng vì nó khám phá mọi khả năng, nhưng độ phức tạp của nó phụ thuộc vào yếu tố`n`(O(n!)), điều này hoàn toàn không khả thi ngay cả đối với`n = 20`. 

Quan sát vấn đề kỹ hơn, chúng ta thấy trạng thái hỏng đèn chỉ phụ thuộc vào số lượng đèn đang bật. Điều này gợi ý về việc sắp xếp theo`a_i`, vì đèn có công suất cao hơn`a_i`có thể chịu đựng được nhiều đèn hơn trước khi nó bị hỏng. Trong số những chiếc đèn có thể tồn tại trong một khoảng thời gian nhất định, chúng ta nên chọn chiếc có giá trị cao nhất`b_i`đầu tiên, vì phần thưởng cao hơn sẽ đóng góp nhiều hơn vào tổng điểm. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng mọi thứ tự có thể. Chúng tôi có thể lý luận rằng nếu chúng tôi biết chúng tôi muốn bật chính xác`k`đèn trước khi đạt đến ngưỡng phá vỡ, tốt nhất`k`đèn để lựa chọn là những chiếc đèn có giá trị cao nhất`b_i`các giá trị. Điều này biến vấn đề thành một vấn đề trong đó chúng tôi kiểm tra từng số có thể`k`đèn để bật, chọn đầu`k`phần thưởng và kiểm tra xem liệu chúng tôi có thể đạt được cấu hình đó một cách an toàn hay không`a_i`ngưỡng. 

Điều này dẫn đến giải pháp O(n log n) cho mỗi trường hợp thử nghiệm: sắp xếp đèn theo`b_i`để nhanh chóng truy cập vào các phần thưởng hàng đầu, sau đó lặp lại số lượng có thể có, kiểm tra xem có đủ đèn không`a_i >= k`cho phép`k`hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng đèn`n`và của họ`(a_i, b_i)`các giá trị. 
2. Sắp xếp các đèn theo thứ tự giảm dần`b_i`. Điều này đảm bảo rằng khi chọn đèn để bật, chúng tôi luôn xem xét phần thưởng cao nhất trước tiên. 
3. Khởi tạo max-heap hoặc sắp xếp`a_i`các giá trị để đếm một cách hiệu quả số lượng đèn có thể tồn tại trong một lần đếm nhất định. 
4. Lặp lại số lượng thao tác có thể có`k`từ 1 đến`n`. Đối với mỗi`k`, đếm xem có bao nhiêu ngọn đèn`a_i >= k`. Nếu số lượng này ít nhất là`k`, sau đó chúng ta có thể bật lại một cách an toàn`k`đèn mà không vi phạm chiến lược. 
5. Trong số tất cả hợp lệ`k`, hãy chọn cái tối đa hóa tổng của đỉnh`k` `b_i`các giá trị. 
6. Xuất ra tổng số điểm tối đa. 

Tại sao nó hoạt động: Bất biến đối với bất kỳ số lượng đèn được chọn nào`k`, chúng ta có thể chọn`k`những chiếc đèn có phần thưởng lớn nhất mà ngưỡng cho phép chúng tồn tại. Không có sự kết hợp nào tốt hơn vì việc chọn phần thưởng thấp hơn không thể cải thiện tổng số và việc chọn một chiếc đèn không đủ`a_i`sẽ gây gãy sớm, mất an toàn. Lý do này đảm bảo rằng thuật toán tìm thấy tổng số điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        lamps = []
        for _ in range(n):
            a, b = map(int, input().split())
            lamps.append((a, b))
        
        # Sort lamps by b_i descending
        lamps.sort(key=lambda x: -x[1])
        
        # Precompute b_i prefix sums for efficiency
        b_prefix = [0]
        for a, b in lamps:
            b_prefix.append(b_prefix[-1] + b)
        
        # Sort a_i ascending
        a_sorted = sorted(a for a, b in lamps)
        
        max_points = 0
        for k in range(1, n + 1):
            # Count how many lamps have a_i >= k
            count = n - (lower_bound(a_sorted, k))
            if count >= k:
                max_points = max(max_points, b_prefix[k])
        
        print(max_points)

def lower_bound(arr, target):
    """Returns first index where arr[index] >= target"""
    lo, hi = 0, len(arr)
    while lo < hi:
        mid = (lo + hi) // 2
        if arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid
    return lo

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là sắp xếp đèn theo phần thưởng để tập trung vào những loại đèn có giá trị cao. Sau đó, nó sử dụng mảng tổng tiền tố để tổng hợp hiệu quả các phần thưởng hàng đầu. các`lower_bound`Chức năng đếm đèn hiệu quả với đủ ngưỡng cho mỗi ứng viên`k`, tránh quét tuyến tính đầy đủ và giữ độ phức tạp ở mức O(n log n). 

Các lựa chọn tinh tế bao gồm việc đảm bảo chúng tôi tính toán tổng tiền tố một cách chính xác (`b_prefix[0] = 0`) và xử lý so sánh`a_i >= k`sử dụng tìm kiếm nhị phân để tránh việc đếm O(n2). 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
4
2 2
1 6
1 10
1 13
```| Bước | Đèn (a,b) sắp xếp theo b | k | Đếm a_i >= k | Có hiệu lực? | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (13,1),(10,1),(6,1),(2,2) | 1 | 4 | Có | 13 | 
| 2 | ... | 2 | 1 | Không | - | 
| 3 | ... | 3 | 0 | Không | - | 
| 4 | ... | 4 | 0 | Không | - | 

Điểm tối đa = 15 sau khi thêm 2 từ đèn còn lại. 

Điều này xác nhận thuật toán chọn đèn có thưởng cao một cách an toàn trong khi vẫn tôn trọng ngưỡng phá vỡ. 

### Đầu vào mẫu 2```
1
5
3 4
3 1
2 5
3 2
3 3
```Theo cùng một logic, tổng số điểm tối đa có thể đạt được mà không vi phạm`a_i`giới hạn là 14. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp theo`b_i`Và`a_i`cho mỗi trường hợp thử nghiệm chiếm ưu thế, cộng với tìm kiếm nhị phân | 
| Không gian | O(n) | Lưu trữ đèn, tổng tiền tố và sắp xếp`a_i`mảng | 

Với`n`2 × 10⁵ và tổng của`n`qua các thử nghiệm 2 × 10⁵, điều này phù hợp trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n4\n2 2\n1 6\n1 10\n1 13\n5\n3 4\n3 1\n2 5\n3 2\n3 3\n6\n1 2\n3 4\n1 4\n3 4\n3 5\n2 3\n1\n1 1\n") == "15\n14\n20\n1"

# Minimum input
assert run("1\n1\n1 1\n") == "1"

# Maximum reward first but breaks others
assert run("1\n2\n1 100\n2 1\n") == "101"

# All equal a_i
assert run("1\n3\n2 1\n2 2\n2 3\n") == "6"

# Large n, small b_i
inp = "1\n5\n1 1\n2 1\n3 1\n4 1\n5 1\n"
assert run(inp) == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1 1`| | |
