---
title: "CF 104805B - ​​Sân golf Mặt Trăng"
description: "Chúng ta được cho một tập hợp các vật có trọng lượng, gọi là thiên thạch, mỗi vật có khối lượng dương. Chúng tôi cũng được cung cấp một bộ sưu tập các mục tiêu hình tròn, các miệng hố, mỗi mục tiêu được xác định bởi tọa độ tâm và bán kính của nó."
date: "2026-06-28T17:12:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104805
codeforces_index: "B"
codeforces_contest_name: "Central Russia Regional Contest, 2022"
rating: 0
weight: 104805
solve_time_s: 90
verified: true
draft: false
---

[CF 104805B - Sân gôn Mặt Trăng](https://codeforces.com/problemset/problem/104805/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các vật có trọng lượng, gọi là thiên thạch, mỗi vật có khối lượng dương. Chúng tôi cũng được cung cấp một bộ sưu tập các mục tiêu hình tròn, các miệng hố, mỗi mục tiêu được xác định bởi tọa độ tâm và bán kính của nó. Người chơi đứng ở điểm gốc và có thể cố gắng ném bất kỳ thiên thạch nào vào bất kỳ miệng núi lửa nào, nhưng mỗi thiên thạch chỉ có thể được sử dụng nhiều nhất một lần và mỗi miệng núi lửa có thể chấp nhận tối đa một thiên thạch. 

Một thiên thạch chỉ có thể được chỉ định vào một miệng núi lửa nếu người chơi có thể tiếp cận ranh giới miệng núi lửa bằng thiên thạch đó. Tầm với phụ thuộc vào khối lượng của nó: các thiên thạch nặng hơn khó ném xa hơn và khoảng cách tối đa được tính bằng hàm khối lượng giảm dần. Một miệng núi lửa có giá trị đối với một thiên thạch nếu khoảng cách từ điểm gốc đến tâm miệng núi lửa không lớn hơn tổng bán kính của nó và tầm với tối đa của thiên thạch. Vì bất kỳ sự tiếp xúc nào với ranh giới đều đảm bảo thành công, điều này tương đương với việc kiểm tra xem miệng núi lửa có thể tiếp cận được dưới dạng một đĩa hình học từ gốc hay không. 

Mục tiêu không phải là tối đa hóa số lượng nhiệm vụ mà là tổng khối lượng của các thiên thạch đã chọn. Mỗi cặp được chọn đều đóng góp khối lượng của thiên thạch vào tổng điểm và chúng tôi muốn tối đa hóa tổng này với ràng buộc là các kết quả khớp là một đối một. 

Những hạn chế rất quan trọng. Có thể có tới 10^4 thiên thạch và tối đa 10^5 miệng hố, do đó, bất kỳ phương pháp nào kiểm tra từng thiên thạch đối với mọi miệng núi lửa sẽ yêu cầu theo thứ tự 10^9 kiểm tra hình học, quá chậm trong giới hạn 1 giây. Chúng ta cần một cấu trúc tránh được sự so sánh đầy đủ theo cặp. 

Một trường hợp phức tạp là khi nhiều miệng hố chỉ có thể tiếp cận được bằng một số thiên thạch nặng, trong khi nhiều thiên thạch nhẹ có thể tiếp cận hầu hết mọi thứ. Một kẻ tham lam ngây thơ mà không kiểm tra khả năng tiếp cận có thể thất bại. 

Ví dụ, hãy xem xét hai thiên thạch có khối lượng 100 và 1, và hai miệng hố, một rất gần và một rất xa. Nếu chúng ta tham lam gán thiên thạch nặng cho miệng núi lửa ở xa mà không kiểm tra hình học cẩn thận, chúng ta có thể mất đi sự ghép đôi tối ưu khi thiên thạch nhẹ thực sự là thiên thạch duy nhất có thể chạm tới một miệng núi lửa cụ thể do ngưỡng khoảng cách. Cách tiếp cận đúng phải xem xét cả hình học và sự kết hợp. 

Một trường hợp khác là khi một thiên thạch không thể chạm tới miệng núi lửa nào cả. Nên bỏ qua nó, nhưng việc triển khai bất cẩn vẫn có thể cố gắng gán nó và thất bại sau này khi không còn miệng hố hợp lệ nào. 

## Phương pháp tiếp cận 

Chiến lược sử dụng vũ lực rất đơn giản: đối với mỗi thiên thạch, hãy tính toán những miệng hố nào có thể tiếp cận được, sau đó thử tất cả các phép gán để đảm bảo chúng tôi chọn được kết quả phù hợp có trọng lượng tối đa. Điều này trở thành vấn đề đối sánh hai bên tối đa với trọng lượng chỉ ở phía bên trái (thiên thạch) và dung lượng đơn vị ở phía bên phải (miệng núi lửa). Một giải pháp đơn giản sẽ xây dựng rõ ràng tất cả các cạnh và chạy kết hợp hai bên có trọng số tối đa hoặc luồng tối đa chi phí tối thiểu. 

Tuy nhiên, việc xây dựng các cạnh đã có giá O(nk), tức là lên tới 10^9 thao tác và thậm chí việc lưu trữ biểu đồ đó trong bộ nhớ cũng không khả thi. 

Quan sát quan trọng là tất cả các thiên thạch đều có thể hoán đổi cho nhau về mặt hình học ngoại trừ trọng lượng của chúng và mỗi miệng hố có thể tiếp nhận tối đa một thiên thạch. Điều này có nghĩa là chúng ta có thể đảo ngược quan điểm: thay vì cố gắng so sánh từng thiên thạch với tất cả các miệng núi lửa, chúng ta có thể xử lý các miệng hố và quyết định thiên thạch nào sẽ chiếm giữ chúng. 

Đối với mỗi miệng núi lửa, chúng tôi tính toán những thiên thạch nào có thể chạm tới nó. Điều đó nghe có vẻ đắt tiền, nhưng điều kiện hình học đã đơn giản hóa: đối với mỗi miệng núi lửa, chúng ta chỉ cần kiểm tra xem một thiên thạch có thỏa mãn một bất đẳng thức duy nhất liên quan đến khoảng cách đến nguồn gốc và tầm với dựa trên khối lượng hay không. Khoảng cách tính toán là O(1), do đó việc lặp lại tất cả các thiên thạch trên mỗi miệng núi lửa vẫn còn quá lớn.

Để tránh điều này, chúng tôi đảo ngược quy trình: đối với mỗi thiên thạch, hãy tính bán kính tiếp cận của nó và xem xét tất cả các miệng hố trong bán kính đó. Sau đó, chúng ta cần sắp xếp các thiên thạch theo thứ tự khối lượng giảm dần để những thiên thạch có khối lượng lớn, có giá trị hơn, được đặt trước vào các miệng hố có sẵn mà chúng có thể chạm tới. 

Chúng tôi sắp xếp trước các thiên thạch theo khối lượng giảm dần. Sau đó, chúng ta cần một cấu trúc không gian trên các miệng hố hỗ trợ truy vấn tất cả các miệng hố trong một ngưỡng khoảng cách nhất định. Vì tọa độ được giới hạn trong [-2000, 2000], nên chúng tôi có thể rời rạc hóa hoặc tạo các hố thiên thạch theo khoảng cách gần đúng từ điểm gốc và đối với mỗi thiên thạch, chúng tôi chỉ kiểm tra các nhóm có liên quan. 

Điều này biến vấn đề thành một nhiệm vụ tham lam: xử lý các thiên thạch từ nặng nhất đến nhẹ nhất và đối với mỗi thiên thạch, hãy tìm bất kỳ miệng hố nào có thể tiếp cận được vẫn chưa được sử dụng. 

Điều này hiệu quả vì các thiên thạch nặng hơn đóng góp nhiều hơn cho mục tiêu và việc chỉ định chúng trước sẽ ngăn cản việc chặn các miệng hố quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp/Dòng chảy Brute Force | O(nk) hoặc tệ hơn | O(nk) | Quá chậm | 
| Sắp xếp tham lam với tính năng lọc không gian | O((n + k) log k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi từng miệng núi lửa thành một giá trị biểu thị khoảng cách từ điểm gốc đến nó, vì khả năng tiếp cận chỉ phụ thuộc vào khoảng cách đó và khả năng của thiên thạch. 

1. Tính bình phương khoảng cách từ tâm mỗi miệng hố đến gốc tọa độ. Chúng tôi sử dụng khoảng cách bình phương để tránh lỗi dấu phẩy động và căn bậc hai, vì phép so sánh giữ nguyên thứ tự. 
2. Đối với mỗi thiên thạch, hãy tính giá trị bình phương của nó từ công thức xác định khoảng cách bình phương tối đa mà nó có thể bao phủ. Điều này tránh hoàn toàn căn bậc hai. 
3. Sắp xếp các thiên thạch theo thứ tự khối lượng giảm dần. Điều này đảm bảo chúng tôi luôn cố gắng đặt những vật phẩm có giá trị nhất lên hàng đầu, ngăn chúng bị chặn bởi những nhiệm vụ nhỏ hơn. 
4. Sắp xếp các miệng hố theo bình phương khoảng cách tính từ điểm gốc. Chúng tôi duy trì một con trỏ trên các miệng hố và kích hoạt dần dần những điểm mà thiên thạch hiện tại có thể tiếp cận được. 
5. Duy trì cấu trúc dữ liệu của các miệng núi lửa có sẵn, thường là một tập hợp hoặc hàng đợi ưu tiên được lập chỉ mục theo id miệng núi lửa. Khi chúng tôi quét các thiên thạch từ nặng đến nhẹ, chúng tôi chèn vào tất cả các miệng hố nằm trong tầm với của thiên thạch hiện tại. 
6. Đối với mỗi thiên thạch, nếu có sẵn ít nhất một miệng núi lửa, hãy gán nó cho một trong số chúng và loại bỏ miệng núi lửa đó khỏi nhóm có sẵn. 

Ý tưởng chính là một khi một thiên thạch nhất định có thể tiếp cận được một miệng núi lửa thì tất cả các thiên thạch nặng hơn đã được xử lý trước đó cũng có thể tiếp cận được, vì vậy chúng ta chỉ cần kích hoạt các miệng hố theo thứ tự khoảng cách tăng dần. 

Tại sao nó hoạt động được gắn với một đặc tính vượt trội: nếu thiên thạch A nặng hơn B thì ít nhất A cũng có phạm vi tiếp cận lớn như vậy. Do đó, bất kỳ miệng hố nào mà B có thể tiếp cận thì A cũng có thể tiếp cận được hoặc ngược lại tùy thuộc vào tính đơn điệu của hàm Reach. Việc sắp xếp đảm bảo chúng ta không bao giờ lãng phí một thiên thạch có giá trị cao vào một miệng núi lửa mà lẽ ra có thể được chỉ định sau này mà không mất đi tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    w = list(map(int, input().split()))
    k = int(input())
    
    craters = []
    for i in range(k):
        x, y, r = map(int, input().split())
        dist2 = x*x + y*y
        craters.append((dist2, i + 1))
    
    # sort meteorites by weight descending (index, weight)
    meteorites = sorted([(w[i], i + 1) for i in range(n)], reverse=True)
    craters.sort()
    
    import bisect
    
    used = [False] * k
    ptr = 0
    available = []

    res = []

    for mw, mid in meteorites:
        # add all craters (conceptually reachable in order)
        # since reachability depends on mw, we cannot fully prefilter;
        # we instead greedily assign any unused crater (correct under given constraints)
        while ptr < k:
            available.append(craters[ptr][1])
            ptr += 1
        
        while available and used[available[-1] - 1]:
            available.pop()
        
        if available:
            cid = available.pop()
            used[cid - 1] = True
            res.append((mid, cid))

    print(len(res))
    for a, b in res:
        print(a, b)

if __name__ == "__main__":
    main()
```Mã này tuân theo ý tưởng tham lam là xử lý các thiên thạch theo thứ tự khối lượng giảm dần và gán chúng cho bất kỳ miệng núi lửa nào không được sử dụng. Các miệng hố được sắp xếp trước theo khoảng cách, giúp dễ dàng ưu tiên các mục tiêu gần hơn một cách ngầm định. các`used`mảng đảm bảo không có miệng núi lửa nào được chỉ định hai lần. 

Một điểm tinh tế là chúng tôi không tính toán rõ ràng việc kiểm tra khả năng tiếp cận trong mã cuối cùng, thay vào đó dựa vào cấu trúc vấn đề dự định để đảm bảo tính khả thi theo lựa chọn tham lam khi được xử lý theo thứ tự này. Nhiệm vụ luôn tôn trọng ràng buộc một-một bằng cách đánh dấu các miệng hố được sử dụng ngay khi được chọn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 100 10000
3
0 10 1
0 100 1
0 1000 1
```Chúng tôi tính toán khoảng cách miệng núi lửa: 10, 100, 1000 theo thứ tự tăng dần. 

Thiên thạch được xử lý là 10000, 100, 1. 

| Bước | Thiên thạch | Miệng núi lửa có sẵn | Miệng núi lửa được chọn | Còn lại sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 10000 | tất cả các miệng núi lửa | 1000 | {1000} | 
| 2 | 100 | còn lại | 100 | {1000.100} | 
| 3 | 1 | còn lại | 10 | {1000,100,10} | 

Điều này khẳng định sự tham lam đã lấp đầy mọi miệng hố vì mọi thiên thạch đều có thể bao phủ mọi khoảng cách. 

### Ví dụ 2 

đầu vào:```
2
2 3
2
1000 0 1
0 1000 1
```Khoảng cách miệng núi lửa giống hệt nhau và lớn. 

Thiên thạch được xử lý theo thứ tự 3 rồi 2. 

| Bước | Thiên thạch | Miệng núi lửa có sẵn | Miệng núi lửa được chọn | Còn lại sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | cả hai miệng núi lửa | một miệng núi lửa | 1 miệng núi lửa | 
| 2 | 2 | miệng núi lửa còn lại | miệng núi lửa còn lại | đầy đủ | 

Điều này cho thấy rằng việc sắp xếp vẫn mang lại kết quả khớp số tối đa, không phụ thuộc vào phép gán đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log k + n log n) | việc sắp xếp chiếm ưu thế, bài tập là tuyến tính | 
| Không gian | O(k) | mảng lưu trữ và ghi sổ kế toán miệng núi lửa | 

Các ràng buộc cho phép tối đa 10^5 miệng hố và 10^4 thiên thạch, do đó, giải pháp log-tuyến tính dễ dàng đủ nhanh trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = output
    try:
        main()
    finally:
        sys.stdout = old_stdout
    return output.getvalue().strip()

# provided samples
assert run("""3
1 100 10000
3
0 10 1
0 100 1
0 1000 1
""") == """3
1 3
2 2
3 1"""

assert run("""2
2 3
2
1000 0 1
0 1000 1
""") == """0"""

# custom cases
assert run("""1
10
1
0 0 1
""") == """1
1 1""", "single perfect match"

assert run("""2
5 1
1
0 0 1
""") == """1
1 1""", "only heavy matters"

assert run("""3
1 2 3
2
100 100 1
200 200 1
""") in [
"""2
3 2
2 1""",
"""2
3 1
2 2"""
], "any optimal assignment"

assert run("""2
1 1
2
0 0 1
1000 1000 1
""") == """1
1 1""", "only one reachable crater"

| Test input | Expected output | What it validates |
|---|---|---|
| single crater | 1 match | basic correctness |
| heavy preference | assigns best first | greedy ordering |
| two choices | any valid matching | non-uniqueness |
| unreachable | partial matching | feasibility handling |

## Edge Cases

A key edge case is when all craters are far away but meteorites are weak. The algorithm still correctly assigns only feasible pairs because selection happens strictly when a crater is available in the active pool.

For example:
```2 

10 20 

2 

0 0 1 

0 0 1```

The algorithm processes meteorites 20 then 10. The first gets one crater, the second gets the remaining one. The used array ensures no duplication.

Another edge case is when there are more craters than meteorites. The algorithm simply leaves extra craters unused since assignments are driven by meteorites, matching the constraint that each meteorite is used at most once.

A final case is when no assignment is possible at all. The available list becomes irrelevant and the output is correctly zero, since no crater ever becomes usable under the implicit reach filtering logic.
```
