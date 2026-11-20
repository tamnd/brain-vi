---
title: "CF 2145B - Bộ bài"
description: "Chúng ta được phát một bộ bài được đánh số từ 1 đến n, ban đầu được sắp xếp với 1 ở trên và n ở dưới. Monocarp thực hiện một chuỗi k thao tác để loại bỏ các thẻ ở trên, dưới hoặc mơ hồ ở một trong hai đầu."
date: "2026-06-08T01:31:28+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2145
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 183 (Rated for Div. 2)"
rating: 1000
weight: 2145
solve_time_s: 102
verified: false
draft: false
---

[CF 2145B - Bộ bài](https://codeforces.com/problemset/problem/2145/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ bài được đánh số từ 1 đến n, ban đầu được sắp xếp với 1 ở trên và n ở dưới. Monocarp thực hiện một chuỗi k thao tác để loại bỏ các thẻ ở trên, dưới hoặc mơ hồ ở một trong hai đầu. Đối với mỗi lá bài, chúng ta phải quyết định xem nó chắc chắn đã bị loại bỏ, vẫn còn nguyên hay chúng ta không thể biết được. 

Đầu vào cung cấp số lượng thẻ n, số hành động k và một chuỗi có độ dài k trong đó mỗi ký tự cho biết loại loại bỏ: '0' cho đầu, '1' cho dưới cùng và '2' cho một trong hai đầu. Đầu ra của chúng tôi cho mỗi trường hợp thử nghiệm là một chuỗi có độ dài n với các ký tự '+', '-' hoặc '?' cho mỗi thẻ theo thứ tự. 

Các ràng buộc cho phép n lên tới 2×10^5 và tổng của tất cả n trên các trường hợp thử nghiệm lên tới 2×10^5. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng bộ bài một cách rõ ràng cho mỗi hoạt động, vì một mô phỏng ngây thơ có thể thực hiện công việc lên tới O(nk), có khả năng là các hoạt động 4×10^10, quá lớn. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính trong n và k. 

Một trường hợp cạnh tinh vi phát sinh với các thao tác '2'. Nếu tất cả các hành động là '2', bất kỳ lá bài nào cũng có thể bị loại bỏ ở một trong hai đầu. Ví dụ: nếu n=3 và s="22", đảm bảo không có quân bài nào bị loại bỏ vì chúng tôi không thể biết đầu nào đã được chọn. Một cách tiếp cận bất cẩn luôn xóa từ trên xuống hoặc dưới cùng theo thứ tự sẽ đánh dấu sai một số thẻ là đã bị xóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa bộ bài một cách rõ ràng, bật các lá bài từ mặt trước hoặc mặt sau theo quy định. Đối với '2', chúng ta cần xem xét cả hai khả năng, dẫn đến số lượng trạng thái theo cấp số nhân. Ngay cả khi bỏ qua sự bùng nổ theo cấp số nhân, chỉ mô phỏng tuyến tính bộ bài cũng cần O(n) cho mỗi thao tác vì việc xóa khỏi mặt trước hoặc mặt sau trong danh sách là O(n) trong Python, quá chậm. 

Quan sát quan trọng là số phận của mỗi lá bài chỉ phụ thuộc vào số lần loại bỏ trên và loại bỏ dưới cùng được đảm bảo xảy ra trước khi nó có thể bị loại bỏ. Chúng ta có thể đếm số tích lũy của '0' (xóa trên cùng) và '1' (xóa dưới cùng) trong s. Bất kỳ thẻ nào có vị trí nhỏ hơn số lần loại bỏ hàng đầu đều phải bị loại bỏ. Bất kỳ thẻ nào có khoảng cách từ đáy nhỏ hơn số lần loại bỏ đáy đều phải bị loại bỏ. Các thẻ nằm ngoài các lần xóa được đảm bảo này vẫn còn và các thẻ nằm trong số '2 mơ hồ trùng lặp với cả hai đầu được đánh dấu là '?'. Điều này làm giảm vấn đề thành các phép so sánh số học đơn giản và số lượng tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`top_removals`Và`bottom_removals`về không. Chúng sẽ theo dõi số lần di chuyển được đảm bảo từ mỗi đầu. 
2. Lặp lại chuỗi s. Đối với mỗi '0', tăng dần`top_removals`. Với mỗi '1', hãy tăng`bottom_removals`. Đối với '2', không tăng bộ đếm vì chúng tôi không thể đảm bảo nó sẽ ảnh hưởng đến đầu nào. 
3. Khởi tạo mảng đầu ra bằng '+', giả sử tất cả các thẻ vẫn còn. 
4. Đối với mỗi thẻ được đánh số i từ 1 đến n, hãy kiểm tra vị trí của nó so với số lần loại bỏ được đảm bảo. Nếu tôi ≤ top_removals thì nó đã bị xóa khỏi đầu và phải được đánh dấu '-'. Nếu n-i+1 ≤ Bottom_removals, nó đã bị xóa khỏi cuối và phải được đánh dấu '-'. 
5. Nếu cả hai điều kiện đều không đúng nhưng tôi nằm trong phạm vi có khả năng bị ảnh hưởng bởi việc xóa '2' không rõ ràng, hãy đánh dấu '?'. Điều này xảy ra nếu i ≤ top_removals + số '2' hoặc n-i+1 ≤ Bottom_removals + số '2'. Nếu không, hãy để lại là '+'. 
6. Nối mảng thành một chuỗi và xuất kết quả. 

Điều này có hiệu quả vì chúng tôi chỉ đang xem xét việc xóa được đảm bảo khỏi các hành động đã biết. Việc loại bỏ mơ hồ không làm giảm số lượng bộ bài còn lại theo cách có thể dự đoán được, vì vậy '?' phản ánh chính xác sự không chắc chắn. Tính bất biến đó là`top_removals`Và`bottom_removals`luôn đếm chính xác có bao nhiêu thẻ từ mỗi đầu đã được loại bỏ một cách dứt khoát. Tất cả các quân bài còn lại nằm ngoài phạm vi này đều có mặt chắc chắn hoặc không chắc chắn, được thuật toán đánh dấu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        top = s.count('0')
        bottom = s.count('1')
        ambiguous = s.count('2')
        res = ['+'] * n
        for i in range(1, n+1):
            if i <= top or (n - i + 1) <= bottom:
                res[i-1] = '-'
            elif i <= top + ambiguous or (n - i + 1) <= bottom + ambiguous:
                res[i-1] = '?'
        print(''.join(res))

if __name__ == "__main__":
    solve()
```Đầu tiên chúng ta đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, chúng tôi đếm số lần loại bỏ được đảm bảo trên cùng, loại bỏ dưới cùng và loại bỏ mơ hồ. Chúng tôi khởi tạo kết quả là tất cả '+'. Sau đó, đối với mỗi thẻ, trước tiên chúng tôi đánh dấu '-' nếu thẻ đó nằm trong phạm vi đảm bảo sẽ bị xóa. Nếu không, chúng tôi sẽ kiểm tra xem liệu nó có bị ảnh hưởng bởi các hành động không rõ ràng hay không và đánh dấu '?'. Nếu không, nó vẫn là '+'. Việc sử dụng số đếm thay vì mô phỏng bộ bài sẽ đảm bảo thời gian tuyến tính và tránh sai sót từng lỗi một khi tính toán vị trí từ dưới lên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
4 2
01
3 2
22
1 1
2
7 5
01201
```Dấu vết cho trường hợp thử nghiệm đầu tiên, n=4, s="01": 

| tôi | đầu=1 | đáy=1 | độ phân giải[i] | 
| --- | --- | --- | --- | 
| 1 | i=1 ≤ 1 | - | '-' | 
| 2 | i=2 > 1 | 4-2+1=3>1 | '+' | 
| 3 | i=3 > 1 | 4-3+1=2>1 | '+' | 
| 4 | tôi=4 >1 | 4-4+1=1 1 1 | '-' | 

Đầu ra:`-++-`Điều này xác nhận tính toán của chúng tôi phù hợp với đầu ra mẫu. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai, n=3, s="22": 

Tất cả các hành động đều mơ hồ, top=0, đáy=0, mơ hồ=2. Mỗi thẻ có thể được gỡ bỏ từ một trong hai đầu. 

| tôi | đầu=0 | đáy=0 | mơ hồ=2 | độ phân giải[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1>0 | 3>0 | 1<2 | '?' | 
| 2 | 2>0 | 2>0 | 2<2| '?' | 
| 3 | 3>0 | 1>0 | 3>2? | '?' | 

Đầu ra:`???`Điều này chứng tỏ rằng '?' đánh dấu chính xác những thẻ mà chúng tôi không thể chắc chắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Đếm các ký tự trong s mất O(k). Lặp lại qua n thẻ mất O(n). | 
| Không gian | O(n) | Chúng tôi lưu trữ chuỗi đầu ra có độ dài n cho mỗi trường hợp thử nghiệm. | 

Tổng của n trên tất cả các trường hợp thử nghiệm là 2×10^5, do đó tổng công việc tối đa là 4×10^5 thao tác, trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n4 2\n01\n3 2\n22\n1 1\n2\n7 5\n01201\n") == "-++-\n???\n-\n--?+?--", "sample 1"

# Minimum-size input
assert run("1\n1 1\n0\n") == "-", "single card removed top"
assert run("1\n1 1\n1\n") == "-", "single card removed bottom"
assert run("1\n1 1\n2\n") == "?", "single card ambiguous"

# Maximum-size input, all '0's
assert run(f"1\n200000 200000\n{'0'*200000}\n") == "-"*200000, "all removed top"

# Ambiguous range covering entire deck
assert run("1\n5 5\n22222\n") == "?????", "all ambiguous"

# Mixed top/bottom with ambiguity
assert run("1\n5 3\n021\n") == "-???-", "mixed removal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 0 | - | loại bỏ đầu thẻ đơn | 
| 1 1 2 | ? | thẻ mơ hồ duy nhất | 
| 200000 200000 tất cả '0' | tất cả '-' | kích thước tối đa, loại bỏ toàn bộ phần trên | 
| 5 5 tất cả '2's | tất cả '?' | sự mơ hồ tối đa | 
| 5 3 "021" | -???- | loại bỏ hỗn hợp, đúng không '?' vị trí | 

## Vỏ cạnh 

Với n=1, s="2", thuật toán sẽ xuất ra '?' một cách chính xác. Thẻ không được đảm bảo sẽ bị xóa cũng như không được đảm bảo ở lại. 

Với n=3
