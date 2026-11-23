---
title: "CF 474B - Giun"
description: "Chúng tôi được phát cho một vài đống giun được sắp xếp theo thứ tự. Giun được đánh số liên tục trên tất cả các cọc. Nếu kích thước đống là: thì đống thứ nhất chứa giun được đánh số từ 1 đến 2, đống thứ hai chứa giun được đánh số từ 3 đến 9, và đống thứ ba chứa giun…"
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 474
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 271 (Div. 2)"
rating: 1200
weight: 474
solve_time_s: 96
verified: true
draft: false
---

[CF 474B - Giun](https://codeforces.com/problemset/problem/474/B) 

**Đánh giá:** 1200 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát cho một vài đống giun được sắp xếp theo thứ tự. Giun được đánh số liên tục trên tất cả các cọc. 

Nếu kích thước cọc là:```
2 7 3
```sau đó đống thứ nhất chứa những con giun được đánh số từ 1 đến 2, đống thứ hai chứa những con giun được đánh số từ 3 đến 9, và đống thứ ba chứa những con giun được đánh số từ 10 đến 12. 

Đối với mỗi truy vấn, chúng ta được cấp một nhãn sâu và phải xác định đống nào chứa sâu đó. 

Thách thức chính là cả số lượng cọc và số lượng truy vấn có thể lớn bằng$10^5$. Một giải pháp quét qua tất cả các chồng cho mỗi truy vấn sẽ có hiệu suất lên tới$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian 1 giây cho phép. Chúng ta cần một cái gì đó gần gũi hơn$O(n \log n)$hoặc$O((n+m)\log n)$. 

Một chi tiết tinh tế là nhãn sâu thường nằm chính xác trên ranh giới cọc. Coi như:```
2
2 3
2
2 3
```Phạm vi phủ sóng của cọc:```
Pile 1: [1, 2]
Pile 2: [3, 5]
```Các câu trả lời đúng là:```
1
2
```Việc triển khai tìm kiếm nhị phân bất cẩn có thể đặt sâu 2 vào chồng 2 không chính xác nếu nó tìm kiếm tổng tiền tố đầu tiên lớn hơn truy vấn thay vì lớn hơn hoặc bằng nó. 

Một nguồn sai lầm phổ biến khác là con sâu đầu tiên của cọc.```
3
2 2 2
1
3
```Các phạm vi là:```
Pile 1: [1, 2]
Pile 2: [3, 4]
Pile 3: [5, 6]
```Worm 3 thuộc cọc 2. Bất kỳ sai sót nào trong việc xây dựng dãy sẽ cho ra kết quả sai. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là xây dựng lại phạm vi được bao phủ bởi mỗi cột và sau đó, đối với mỗi truy vấn, hãy quét từng cột một cho đến khi tìm thấy phạm vi chính xác. 

Ví dụ: sau khi xây dựng:```
Pile 1: [1, 2]
Pile 2: [3, 9]
Pile 3: [10, 12]
...
```chúng ta có thể kiểm tra từng khoảng thời gian một cách tuần tự. Điều này hiệu quả vì mỗi con sâu thuộc về chính xác một đống. Vấn đề là hiệu suất. Với$10^5$cọc và$10^5$truy vấn, chúng tôi có thể thực hiện$10^{10}$kiểm tra định kỳ. 

Cấu trúc đánh số cho chúng ta một lựa chọn tốt hơn rất nhiều. Nếu tính tổng tiền tố của kích thước cọc, chúng ta thu được:```
a = [2, 7, 3, 4, 9]

prefix = [2, 9, 12, 16, 25]
```Mỗi tổng tiền tố đại diện cho nhãn sâu lớn nhất có trong đống đó. 

Một con sâu thuộc đống đầu tiên có nhãn kết thúc ít nhất là số con sâu. Ví dụ sâu 11 thuộc cọc 3 vì:```
2  < 11
9  < 11
12 >= 11
```Mảng tổng tiền tố được sắp xếp theo thứ tự tăng dần, giúp áp dụng tìm kiếm nhị phân. Thay vì quét tất cả các cọc, chúng tôi tìm thấy tổng tiền tố đầu tiên lớn hơn hoặc bằng truy vấn trong$O(\log n)$thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng cọc và kích thước của chúng. 
2. Xây dựng một mảng tổng tiền tố trong đó`prefix[i]`bằng tổng số giun trong đống`1..i+1`. 

Điều này chuyển đổi kích thước cọc thành vị trí kết thúc cọc. 
3. Đối với mỗi nhãn sâu truy vấn`q`, thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố. 
4. Tìm vị trí đầu tiên có tổng tiền tố lớn hơn hoặc bằng`q`. 

Điều này xác định đống đầu tiên có nhãn kết thúc đạt hoặc vượt qua sâu được truy vấn. 
5. Xuất chỉ số cọc bằng cách đánh số trên cơ sở 1. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố phân chia tất cả các nhãn sâu thành các phạm vi liên tiếp. 

Nếu như:```
prefix[i-1] < q <= prefix[i]
```sau đó tất cả các nhãn lên đến`prefix[i-1]`thuộc về đống trước đó, trong khi đống`i+1`chứa nhãn bắt đầu tại`prefix[i-1] + 1`và kết thúc tại`prefix[i]`. Tìm tổng tiền tố đầu tiên ít nhất là`q`do đó xác định duy nhất cọc đúng. Tìm kiếm nhị phân chỉ đơn giản là xác định ranh giới này một cách hiệu quả. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())
    piles = list(map(int, input().split()))

    prefix = []
    total = 0

    for x in piles:
        total += x
        prefix.append(total)

    m = int(input())
    queries = list(map(int, input().split()))

    ans = []

    for q in queries:
        pile = bisect_left(prefix, q) + 1
        ans.append(str(pile))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng mảng tổng tiền tố. Mỗi phần tử lưu trữ nhãn sâu lớn nhất thuộc về đống đó. 

Hoạt động quan trọng là:```
bisect_left(prefix, q)
```

`bisect_left`trả về vị trí đầu tiên có giá trị lớn hơn hoặc bằng`q`. Điều đó phù hợp với điều kiện toán học cần thiết cho vấn đề này. 

các`+1`chuyển đổi vị trí mảng dựa trên 0 thành đánh số cọc dựa trên một theo yêu cầu của câu lệnh. 

Sử dụng tìm kiếm nhị phân tích hợp sẵn của Python sẽ tránh được nhiều lỗi về ranh giới. Đặc biệt, nó xử lý chính xác các truy vấn tương đương với điểm cuối của đống, đây là bẫy sai lệch phổ biến nhất trong vấn đề này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 7 3 4 9
3
1 25 11
```Tổng tiền tố:```
[2, 9, 12, 16, 25]
```| Truy vấn | Tiền tố đầu tiên ≥ Truy vấn | Vị trí mảng | Cọc | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 1 | 
| 25 | 25 | 4 | 5 | 
| 11 | 12 | 2 | 3 | 

Đầu ra:```
1
5
3
```Ví dụ này thể hiện sự quan sát trung tâm. Chúng ta không bao giờ cần biết chính xác khoảng thời gian của mỗi cọc. Tổng tiền tố đầu tiên đạt đến nhãn được truy vấn đã xác định được câu trả lời. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
4
2 3 4 5
```Tổng tiền tố:```
[2, 4, 6]
```| Truy vấn | Tiền tố đầu tiên ≥ Truy vấn | Vị trí mảng | Cọc | 
| --- | --- | --- | --- | 
| 2 | 2 | 0 | 1 | 
| 3 | 4 | 1 | 2 | 
| 4 | 4 | 1 | 2 | 
| 5 | 6 | 2 | 3 | 

Đầu ra:```
1
2
2
3
```Dấu vết này thực hiện các giá trị biên. Truy vấn 2 và 4 hoàn toàn bằng điểm cuối cọc và`bisect_left`vẫn trả về đúng cọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) | Xây dựng tổng tiền tố lấy O(n), mỗi truy vấn sử dụng tìm kiếm nhị phân trong O(log n) | 
| Không gian | O(n) | Mảng tổng tiền tố lưu trữ một giá trị trên mỗi cọc | 

Với$n,m \le 10^5$, giải pháp thực hiện gần đúng$10^5$các phép toán tổng tiền tố và$10^5$tìm kiếm nhị phân. Điều này dễ dàng phù hợp trong thời hạn và lưu trữ$10^5$số nguyên nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    piles = list(map(int, input().split()))

    prefix = []
    total = 0

    for x in piles:
        total += x
        prefix.append(total)

    m = int(input())
    queries = list(map(int, input().split()))

    ans = []
    for q in queries:
        ans.append(str(bisect_left(prefix, q) + 1))

    return "\n".join(ans)

# provided sample
assert run(
"""5
2 7 3 4 9
3
1 25 11
"""
) == "1\n5\n3", "sample 1"

# minimum size
assert run(
"""1
1
1
1
"""
) == "1", "single pile single worm"

# all piles equal
assert run(
"""4
5 5 5 5
4
5 10 15 20
"""
) == "1\n2\n3\n4", "boundary endpoints"

# off-by-one at pile starts
assert run(
"""3
2 2 2
3
3 5 6
"""
) == "2\n3\n3", "pile beginnings"

# large boundary transitions
assert run(
"""2
1000 1000
4
1 1000 1001 2000
"""
) == "1\n1\n2\n2", "range transitions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đống đơn chứa một con sâu | 1 | Hạn chế tối thiểu | 
| Bốn cọc bằng nhau với các truy vấn điểm cuối | 1,2,3,4 | Xử lý ranh giới chính xác | 
| Truy vấn lúc bắt đầu cọc | 2,3,3 | Độ chính xác từng cái một | 
| Hai đống lớn với các truy vấn chuyển tiếp | 1,1,2,2 | Chuyển đổi giữa các cọc liền kề | 

## Vỏ cạnh 

Hãy xem xét một truy vấn chính xác bằng điểm cuối của đống.```
2
2 3
1
2
```Tổng tiền tố:```
[2, 5]
```Tìm kiếm nhị phân cho`2`trả về vị trí`0`bởi vì`prefix[0] = 2`. Câu trả lời là đống`1`, điều đó đúng. Tìm kiếm tổng tiền tố đầu tiên lớn hơn truy vấn sẽ trả về không chính xác`2`. 

Hãy xem xét một truy vấn là con sâu đầu tiên của một đống mới.```
2
2 3
1
3
```Tổng tiền tố:```
[2, 5]
```Tìm kiếm nhị phân cho`3`trả về vị trí`1`bởi vì`2 < 3 <= 5`. Câu trả lời trở thành đống`2`. Điều này xác nhận rằng phần đầu cọc được xử lý chính xác mà không cần lưu trữ rõ ràng phần bắt đầu theo khoảng thời gian. 

Hãy xem xét đầu vào nhỏ nhất có thể.```
1
1
1
1
```Tổng tiền tố:```
[1]
```Tìm kiếm nhị phân cho`1`trả về vị trí`0`, đưa cọc`1`. Thuật toán hoạt động ngay cả khi chỉ tồn tại một đống và một con sâu. 

Hãy xem xét con sâu cuối cùng trong toàn bộ bộ sưu tập.```
3
2 2 2
1
6
```Tổng tiền tố:```
[2, 4, 6]
```Tìm kiếm nhị phân tìm thấy tổng tiền tố đầu tiên ít nhất`6`, đó là phần tử cuối cùng. Câu trả lời là đống`3`, xác nhận rằng ranh giới trên của sơ đồ đánh số được xử lý chính xác.
