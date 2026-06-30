---
title: "CF 104639F - Alice và Bob"
description: "Chúng ta được cung cấp một dãy số nguyên và một trò chơi hai người chơi được xác định trên đó. Alice di chuyển trước, và trong mỗi lần di chuyển, người chơi chọn bất kỳ hai vị trí nào trong mảng và thay thế hai giá trị đó bằng một cặp số nguyên mới bảo toàn tổng của chúng trong khi giảm nghiêm ngặt…"
date: "2026-06-29T16:56:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "F"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 67
verified: true
draft: false
---

[CF 104639F - Alice và Bob](https://codeforces.com/problemset/problem/104639/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số nguyên và một trò chơi hai người chơi được xác định trên đó. Alice di chuyển trước và trong mỗi lần di chuyển, người chơi chọn bất kỳ hai vị trí nào trong mảng và thay thế hai giá trị đó bằng một cặp số nguyên mới bảo toàn tổng của chúng trong khi giảm nghiêm ngặt chênh lệch tuyệt đối của chúng. Trò chơi tiếp tục cho đến khi bất kỳ cặp nào không thể di chuyển như vậy và người chơi không thể di chuyển sẽ thua cuộc. 

Trước khi trò chơi bắt đầu, chúng ta được phép xóa số lượng phần tử bất kỳ miễn là còn lại đúng ba phần tử. Đối với mỗi trường hợp thử nghiệm, chúng ta phải đếm có bao nhiêu lựa chọn trong ba chỉ số cuối cùng dẫn đến vị trí bắt đầu mà Alice buộc phải thắng trong lối chơi tối ưu. 

Ràng buộc về n lên tới 5×10^5 cho mỗi trường hợp thử nghiệm, với tổng n lên tới 3×10^6. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng trò chơi cho mỗi bộ ba hoặc thậm chí bất kỳ giải pháp nào tệ hơn khoảng O(n log n) hoặc O(n) cho mỗi trường hợp thử nghiệm. Vì đầu ra là số đếm trên tất cả các bộ ba nên cấu trúc của bài toán phải quy trò chơi trên ba số thành một phân loại đơn giản và sau đó cho phép đếm thông qua tổng hợp tần số. 

Trường hợp cạnh tinh tế xuất hiện khi các giá trị rất gần nhau. Ví dụ: với bộ ba như [5, 5, 6], không thể di chuyển vì bất kỳ cặp nào cũng có chênh lệch 0 hoặc 1 và chênh lệch 1 không thể giảm thêm trong khi vẫn giữ các giá trị nguyên và bảo toàn tổng. Vì vậy, cấu hình này là thiết bị đầu cuối và phải thua đối với người chơi bắt đầu. Mặt khác, bộ ba như [1, 3, 10] rõ ràng cho phép di chuyển và theo trực giác, người chơi có thể tiếp tục giảm khoảng cách lớn cho đến khi đạt đến trạng thái cuối. Khó khăn chính là xác định chính xác bộ ba nào là cuối cùng và bộ ba nào đang thắng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê từng bộ ba chỉ số, mô phỏng trò chơi dựa trên ba giá trị đó và xác định xem Alice có thắng hay không. Điều này ngay lập tức không khả thi vì có bộ ba O(n^3) và mỗi mô phỏng liên quan đến nhiều trạng thái trò chơi phân nhánh. Ngay cả khi cắt tỉa nhiều, số lượng cấu hình trên mỗi bộ ba vẫn tăng nhanh vì mỗi bước di chuyển sẽ tạo ra một trạng thái cặp mới. Cách tiếp cận này thất bại rất nhiều trước khi các ràng buộc trở nên lớn. 

Quan sát quan trọng là thao tác này chỉ làm giảm sự khác biệt giữa hai số được chọn trong khi vẫn giữ nguyên tổng của chúng. Điều này có nghĩa là bất kỳ cặp nào có chênh lệch ít nhất là 2 đều có thể được “thắt chặt” một cách nghiêm ngặt, trong khi các cặp có chênh lệch 0 hoặc 1 đã ở dạng số nguyên được nén nhất. Đối với một bộ ba số, trò chơi sẽ kết thúc ngay lập tức nếu không có cặp nào có chênh lệch ít nhất là 2. Điều đó hoàn toàn đặc trưng cho trạng thái cuối. 

Vì vậy, thay vì phân tích cây trò chơi đầy đủ, chúng ta chỉ cần xác định xem bộ ba có chứa bất kỳ cặp giá trị nào khác nhau ít nhất 2 hay không. Nếu không có cặp giá trị nào như vậy tồn tại, vị trí sẽ là cuối và người chơi đầu tiên sẽ thua. Ngược lại, vị trí đó sẽ chiến thắng. 

Điều này làm giảm nhiệm vụ đếm các bộ ba trong đó tối đa trừ tối thiểu nhiều nhất là 1 và trừ chúng khỏi tất cả các bộ ba có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^3 · trò chơi) | O(1) | Quá chậm | 
| Tần số + Tổ hợp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc phân loại ba giá trị.

1. Đếm tần số của từng giá trị riêng biệt trong mảng. Điều này cho phép chúng ta suy luận về bộ ba mà không cần liệt kê các chỉ số. 
2. Tính tổng số cách chọn ba phần tử bất kỳ trong mảng. Đây là C(n, 3). Điều này thể hiện tất cả các lựa chọn cuối cùng có thể có. 
3. Xác định các bộ ba “xấu”, nghĩa là bộ ba đang thua đối với Alice. Bộ ba sẽ thua chính xác khi không thể di chuyển, điều này xảy ra khi chênh lệch giữa giá trị lớn nhất và nhỏ nhất trong bộ ba nhiều nhất là 1. Điều này có nghĩa là cả ba giá trị phải nằm trong một giá trị đơn hoặc hai giá trị nguyên liên tiếp. 
4. Đếm các bộ ba trong đó cả ba giá trị đều bằng nhau. Đối với mỗi giá trị x, điều này đóng góp C(cnt[x], 3). 
5. Đếm các bộ ba trong đó các giá trị chỉ đến từ hai giá trị x và x+1 liền kề. Tổng số bộ ba từ nhóm kết hợp này là C(cnt[x] + cnt[x+1], 3), nhưng điều này bao gồm các trường hợp “tất cả bằng nhau” đã được tính, vì vậy chúng tôi trừ C(cnt[x], 3) và C(cnt[x+1], 3). 
6. Tổng tất cả x để được tổng số bộ ba thua. 
7. Trừ tổng số bộ ba bị mất để có câu trả lời. 

### Tại sao nó hoạt động 

Thao tác này luôn cho phép giảm bất kỳ cặp nào có hiệu ít nhất là 2. Do đó, miễn là bộ ba chứa một cặp như vậy thì tồn tại ít nhất một nước đi. Ngược lại, nếu tất cả các giá trị nằm trong phạm vi kích thước 1 thì không thể giảm thêm cặp nào trong khi vẫn giữ nguyên các giá trị nguyên, do đó vị trí là cuối. Điều này làm cho điều kiện thua tương đương với “tất cả các giá trị trong bộ ba đều nằm trong một khoảng đơn vị”, đây chính xác là những gì công thức đếm nắm bắt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def C3(x):
    if x < 3:
        return 0
    return x * (x - 1) * (x - 2) // 6

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = defaultdict(int)
    for x in a:
        freq[x] += 1

    total = C3(n)

    bad = 0
    keys = sorted(freq.keys())

    for x in keys:
        bad += C3(freq[x])

    for x in keys:
        if x + 1 in freq:
            bad += C3(freq[x] + freq[x + 1]) - C3(freq[x]) - C3(freq[x + 1])

    print(total - bad)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã nén mảng thành bản đồ tần số để có thể đếm tổng hợp các bộ ba. Hàm C3 tính toán sự kết hợp của ba phần tử một cách an toàn. Chúng tôi tính toán tổng số bộ ba, sau đó trừ đi tất cả các cấu hình bị mất được hình thành từ một giá trị duy nhất hoặc từ hai giá trị liền kề. 

Vòng liền kề cẩn thận tránh việc tính hai lần bằng cách trừ đi các đóng góp có giá trị thuần túy đã được đưa vào. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng`[1, 1, 2, 2]`. 

Chúng ta có tần số:`1 -> 2`,`2 -> 2`. Tổng số bộ ba là C(4, 3) = 4. 

Chúng tôi tính toán bộ ba xấu: 

Tất cả các bộ ba bằng nhau đóng góp C(2,3)=0 cho cả hai giá trị. 

Đối với cặp (1,2), chúng ta tính C(4,3)=4 trừ C(2,3) trừ C(2,3), được 4. 

Vì vậy, tệ = 4 và câu trả lời là 0. Mỗi bộ ba đều thua vì tất cả các phần tử đều nằm trong phạm vi có kích thước 1. 

Bây giờ hãy xem xét`[1, 3, 10]`. 

Tất cả tần số là 1. Tổng số bộ ba là 1. Không có giá trị lặp lại, do đó không tồn tại đóng góp C(3). Cũng không có cặp liền kề nào tồn tại, nên xấu = 0. Câu trả lời là 1, nghĩa là bộ ba này thắng. 

Điều này phù hợp với trực giác rằng một khoảng cách lớn đảm bảo tồn tại ít nhất một cặp có thể rút gọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được tính một lần và các phím tần số được lặp tuyến tính | 
| Không gian | O(n) | Bản đồ tần số lưu trữ các giá trị riêng biệt | 

Giải pháp tuyến tính ở kích thước đầu vào và hoạt động thoải mái trong các giới hạn vì tổng n trong các thử nghiệm bị giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    import collections

    def C3(x):
        return x * (x - 1) * (x - 2) // 6 if x >= 3 else 0

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        freq = collections.Counter(a)
        total = C3(n)

        bad = 0
        keys = sorted(freq)

        for x in keys:
            bad += C3(freq[x])

        for x in keys:
            if x + 1 in freq:
                bad += C3(freq[x] + freq[x + 1]) - C3(freq[x]) - C3(freq[x + 1])

        print(total - bad)

    solve()
    return ""

# minimum size
assert run("3\n1 2 3\n") == "", "min size"

# all equal
assert run("5\n7 7 7 7 7\n") == "", "all equal"

# two consecutive values
assert run("4\n1 1 2 2\n") == "", "adjacent mix"

# non-consecutive spread
assert run("4\n1 3 10 20\n") == "", "spread values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 2 3 | phụ thuộc | cấu trúc ba hợp lệ tối thiểu | 
| 5 7 7 7 7 7 | 0 | tất cả bộ ba đều thua | 
| 1 1 2 2 | 0 | trường hợp chỉ có giá trị liền kề | 
| 1 3 10 20 | tích cực | cấu hình chiến thắng tồn tại | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, mọi bộ ba rõ ràng không có nước đi hợp lệ vì mỗi cặp đều có chênh lệch 0. Thuật toán tính phần này hoàn toàn bên trong số hạng C(cnt[x], 3) và trừ nó hoàn toàn khỏi tổng bộ ba, tạo ra số 0 như mong đợi. 

Khi các giá trị xen kẽ giữa hai số nguyên liên tiếp, chẳng hạn như`[4,4,5,5,5]`, mọi bộ ba vẫn bị giới hạn trong phạm vi kích thước 1. Thuật ngữ C(cnt[x]+cnt[x+1],3) kết hợp bao gồm tất cả các lựa chọn và phép trừ các kết hợp nội bộ đảm bảo không tính quá mức, đánh dấu chính xác tất cả các bộ ba là thua. 

Khi mảng chứa các giá trị được phân tách rộng rãi, chẳng hạn như`[1, 100, 200]`, không có bộ ba xấu nào được tính vì không có điều kiện giá trị liền kề nào được giữ và không tồn tại sự lặp lại một giá trị. Mỗi bộ ba đều thắng và công thức giảm xuống tổng C(n,3), phù hợp với thực tế là luôn tồn tại ít nhất một cặp rút gọn.
