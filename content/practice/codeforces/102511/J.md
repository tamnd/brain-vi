---
title: "CF 102511J - Golf thu nhỏ"
description: "Mỗi người chơi có một danh sách điểm, một điểm cho mỗi lỗ. Giá trị được ghi nhớ sau khi áp dụng giới hạn l chưa biết không phải là điểm x ban đầu mà là min(x, l). Đối với l cố định, mọi người chơi sẽ nhận được tổng điểm bằng cách thay thế tất cả điểm của lỗ lớn bằng l."
date: "2026-08-06T19:30:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102511
codeforces_index: "J"
codeforces_contest_name: "2019 ICPC World Finals"
rating: 0
weight: 102511
solve_time_s: 78
verified: true
draft: false
---

[CF 102511J - Sân gôn thu nhỏ](https://codeforces.com/problemset/problem/102511/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi có một danh sách điểm, một điểm cho mỗi lỗ. Giá trị được ghi nhớ sau khi áp dụng giới hạn chưa biết`l`không phải là điểm ban đầu`x`, Nhưng`min(x, l)`. Đối với một cố định`l`, mỗi người chơi sẽ nhận được tổng điểm bằng cách thay thế tất cả các điểm của lỗ lớn bằng`l`. Tổng số nhỏ hơn sẽ tốt hơn và thứ hạng của người chơi là số người chơi có tổng số điều chỉnh không lớn hơn của họ. 

Nhiệm vụ không phải là tìm một giới hạn cụ thể. Thay vào đó, chúng ta có thể chọn giá trị của`l`mang lại cho mỗi người chơi vị trí tốt nhất có thể và chúng tôi phải đưa ra thứ hạng tối thiểu có thể đạt được đó. 

Số lượng người chơi nhiều nhất là 500 và số lỗ nhiều nhất là 50. Không thể mô phỏng trực tiếp vượt quá giới hạn có thể vì điểm số có thể lớn bằng`10^9`. Hạn chế hữu ích là số lượng nhỏ giá trị điểm cho mỗi người chơi. Điểm điều chỉnh của người chơi chỉ thay đổi khi`l`đạt đến một trong các điểm lỗ ban đầu, do đó tổng số điểm thú vị bị giới hạn bởi`p * h`, không phải bởi kích thước của các giá trị điểm. 

Phần khó khăn là giới hạn tốt nhất cho một người chơi có thể nằm ở đâu đó giữa hai giá trị điểm. Một giải pháp chỉ kiểm tra các giới hạn xuất hiện trong đầu vào có thể bỏ lỡ mức tối ưu. Một sai lầm phổ biến khác là tìm ra giới hạn tốt nhất cho từng đối thủ. Giống nhau`l`phải cải thiện vị trí của người chơi trước tất cả các đối thủ cùng một lúc. 

Ví dụ:```
2 1
5
10
```Nếu chúng tôi chỉ kiểm tra các giá trị ban đầu, chúng tôi sẽ kiểm tra`l = 5`Và`l = 10`. Tại`l = 5`, cả hai người chơi đều có điểm 5 nên người chơi thứ nhất xếp thứ 2. Lúc`l = 10`, điểm số là 5 và 10, vì vậy người chơi đầu tiên có hạng 1. Câu trả lời là 1. Trường hợp này cho thấy tại sao bản thân giới hạn lại quan trọng chứ không chỉ khoảng cách giữa các giới hạn. 

Một trường hợp ranh giới khác là:```
2 2
100 100
1 1
```Người chơi đầu tiên không bao giờ có thể đánh bại người chơi thứ hai. Đối với mọi tích cực`l`, người chơi thứ hai có tổng điểm tối đa là 2 trong khi người chơi thứ nhất có tổng điểm ít nhất là 2 và không bao giờ cao hơn. Thứ hạng đúng là:```
2 1
```Việc thực hiện bất cẩn cho rằng mọi người chơi cuối cùng đều có thể trở nên tốt hơn khi`l`phát triển sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi giá trị có thể có của`l`, tính toán tất cả các điểm đã điều chỉnh, sắp xếp người chơi và ghi lại thứ hạng tốt nhất. Điều này đúng vì nó thực sự kiểm tra mọi điều chỉnh có thể có của trò chơi. Tuy nhiên,`l`có thể lớn như`10^9`, vì vậy điều này không thể hoạt động. 

Một cái nhìn tốt hơn là so sánh người chơi theo cặp. Sửa chữa một cầu thủ`i`. Nếu chúng ta biết khoảng thời gian của`l`Ở đâu`i`đánh bại người chơi khác`j`, thì thứ hạng của`i`tốt nhất là khi số lượng đối thủ bị đánh ở cùng một giới hạn càng nhiều càng tốt. 

Đối với hai người chơi, sự khác biệt về điểm số của họ là một hàm tuyến tính từng phần. Nơi duy nhất mà công thức thay đổi là điểm số lỗ của hai người chơi này. Giữa hai giá trị liên tiếp như vậy, mọi lỗ đều đã được giới hạn hoặc vẫn bằng`l`, do đó tổng số điểm là một hàm tuyến tính của`l`. Điều này có nghĩa là chúng tôi có thể quét qua các phạm vi quan trọng, xác định chính xác vị trí của người chơi`i`trở nên tốt hơn người chơi`j`và lưu trữ các khoảng đó. 

Sau khi thu thập tất cả các khoảng đối với tất cả các đối thủ, vấn đề còn lại là tìm số khoảng tối đa bao gồm cùng một giá trị nguyên của`l`. Đây là vấn đề về đường quét tiêu chuẩn sử dụng các sự kiện bắt đầu và kết thúc. 

Cách tiếp cận bạo lực có hiệu quả vì hàm tính điểm rất đơn giản đối với một giới hạn cố định nhưng không thành công vì có quá nhiều giới hạn có thể có. Quan sát thấy rằng các hàm điểm là tuyến tính từng phần làm giảm không gian tìm kiếm vô hạn thành một tập hợp các khoảng nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số giới hạn * p * h) | O(p) | Quá chậm | 
| Tối ưu | O(p²h log(ph)) | O(ph) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi người chơi`i`, giả sử chúng ta muốn biết thứ hạng tốt nhất của người chơi này. Chúng tôi sẽ đếm xem có bao nhiêu người chơi khác`i`có thể đánh bại một cách nghiêm túc một số giá trị chung của`l`. 
2. Đối với mọi đối thủ`j`, xây dựng các khoảng giới hạn mà người chơi`i`có tổng điểm điều chỉnh nhỏ hơn người chơi`j`. 
3. Để xây dựng các khoảng thời gian này, hãy thu thập tất cả điểm số lỗ của người chơi`i`Và`j`. Các giá trị này chia các số nguyên dương thành các phạm vi trong đó cả hai điểm được điều chỉnh đều là hàm tuyến tính. 
4. Trên mỗi phạm vi, hãy tính độ chênh lệch tuyến tính giữa hai người chơi. Vì sự khác biệt có dạng`a*l+b`, chúng ta có thể tìm thấy phần nguyên chính xác nơi nó âm. Những số nguyên đó là giới hạn trong đó`i`nhịp đập`j`. 
5. Thêm khoảng kết quả vào cấu trúc đường quét. Sự kiện bắt đầu sẽ làm tăng số lượng đối thủ bị đánh bại và sự kiện sau khi khoảng thời gian kết thúc sẽ giảm số lượng đối thủ đó. 
6. Sau khi xử lý tất cả đối thủ, số lần hoạt động đồng thời tối đa là số lượng người chơi tối đa bị đánh bại`i`. Thứ hạng tốt nhất là`p - maximum_beaten`. 

Tại sao nó hoạt động: 

Đối với bất kỳ đối thủ cố định nào, sự so sánh giữa hai người chơi chỉ thay đổi ở các giá trị điểm số hoặc khi hai hàm tuyến tính giao nhau. Việc xây dựng khoảng thời gian tìm thấy chính xác tất cả các giới hạn trong đó người chơi đầu tiên giỏi hơn. Sau đó, đường quét sẽ kiểm tra mọi giới hạn có thể được biểu thị bằng các khoảng đó và tìm ra số lượng đối thủ tối đa có thể bị đánh bại đồng thời. Vì thứ hạng chính xác là số người chơi không bị đánh bại cộng với chính người chơi đó nên giá trị được tính toán là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def adjusted_diff_coeff(a, b, x):
    m = 0
    c = 0
    for u, v in zip(a, b):
        if x < u:
            m += 1
        else:
            c += u
        if x < v:
            m -= 1
        else:
            c -= v
    return m, c

def intervals(a, b):
    pts = sorted(set(a + b))
    segs = []

    if pts[0] > 1:
        segs.append((1, pts[0] - 1))
    for x in pts:
        segs.append((x, x))
    for x, y in zip(pts, pts[1:]):
        if x + 1 <= y - 1:
            segs.append((x + 1, y - 1))
    segs.append((pts[-1] + 1, None))

    ans = []

    for l, r in segs:
        m, c = adjusted_diff_coeff(a, b, l)

        if r is None:
            if m == 0:
                if c < 0:
                    ans.append((l, None))
            elif m < 0:
                ans.append((l, None))
            continue

        if l == r:
            if m * l + c < 0:
                ans.append((l, l))
            continue

        if m == 0:
            if c < 0:
                ans.append((l, r))
        elif m > 0:
            # m*x+c < 0
            hi = (-c - 1) // m
            if hi >= l:
                ans.append((l, min(r, hi)))
        else:
            # m*x+c < 0, multiply by -1
            lo = c // (-m) + 1
            if lo <= r:
                ans.append((max(l, lo), r))

    return ans

def solve():
    p, h = map(int, input().split())
    scores = [list(map(int, input().split())) for _ in range(p)]

    res = []

    for i in range(p):
        events = []
        for j in range(p):
            if i == j:
                continue
            for l, r in intervals(scores[i], scores[j]):
                events.append((l, 1))
                if r is not None:
                    events.append((r + 1, -1))

        events.sort()
        cur = 0
        best = 0
        k = 0
        while k < len(events):
            pos = events[k][0]
            while k < len(events) and events[k][0] == pos:
                cur += events[k][1]
                k += 1
            best = max(best, cur)

        res.append(str(p - best))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Chức năng chính xử lý từng người chơi một vì thứ hạng của một người chơi không ảnh hưởng đến khoảng thời gian của người chơi khác. các`intervals`hàm xử lý một cặp người chơi và trả về tất cả các giới hạn trong đó người chơi đầu tiên thắng cuộc so sánh. 

Người trợ giúp`adjusted_diff_coeff`tính toán biểu diễn tuyến tính của chênh lệch điểm số trên một đoạn cố định. Một lỗ đóng góp một điểm gốc không đổi hoặc một bản sao của`l`, vì vậy toàn bộ sự khác biệt luôn có thể được viết là`m*l+c`. 

Mã khoảng cách tách ba trường hợp. Một điểm duy nhất kiểm tra một giới hạn chính xác. Một phân đoạn hữu hạn giải quyết bất đẳng thức tuyến tính. Đuôi vô hạn được xử lý riêng vì không có điểm cuối phía trên. Số nguyên Python tránh tràn ngay cả khi đạt điểm`10^9`. 

Việc quét sử dụng`r + 1`để kết thúc các sự kiện vì các khoảng chứa giới hạn số nguyên. Việc quên chuyển đổi này là một lỗi thường gặp. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 3
2 2 2
4 2 1
4 4 1
```Hãy xem xét người chơi 1. 

| Đối thủ | Giới hạn chiến thắng cho người chơi 1 | Bị đánh bại tối đa cho đến nay | 
| --- | --- | --- | 
| Người chơi 2 | giới hạn trong đó điểm 1 < điểm 2 | 1 | 
| Người chơi 3 | giới hạn trong đó điểm 1 < điểm 3 | 2 | 

Người chơi 1 có thể đánh bại cả hai đối thủ ở cùng một giới hạn, vì vậy thứ hạng tốt nhất là`3 - 2 = 1`. 

Đối với mẫu thứ hai, quy trình tương tự cho: 

| Người chơi | Đánh bại đối thủ tối đa | Thứ hạng tối thiểu | 
| --- | --- | --- | 
| 1 | 5 | 1 | 
| 2 | 4 | 2 | 
| 3 | 1 | 5 | 
| 4 | 1 | 5 | 
| 5 | 2 | 4 | 
| 6 | 3 | 3 | 

Dấu vết chứng tỏ rằng giới hạn đã chọn phải được chia sẻ giữa tất cả các phép so sánh. Đường quét tìm thấy chính xác những vùng chung đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(p²h log(ph)) | Mỗi cặp người chơi tạo ra các sự kiện O(h) và mỗi người chơi thực hiện quét tất cả các khoảng thời gian của đối thủ. | 
| Không gian | O(ph) | Chỉ các sự kiện của người chơi hiện tại và ma trận điểm mới được lưu trữ. | 

Với`p = 500`Và`h = 50`, số lượng so sánh cặp có thể quản lý được. Thuật toán không bao giờ phụ thuộc vào độ lớn của điểm số, mà chỉ phụ thuộc vào số lượng thay đổi điểm tồn tại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# sample 1
assert run("""3 3
2 2 2
4 2 1
4 4 1
""") == "1 2 2\n"

# sample 2
assert run("""6 4
3 1 2 2
4 3 2 2
6 6 3 2
7 3 4 3
3 4 2 4
2 3 3 5
""") == "1 2 5 5 4 3\n"

# minimum size
assert run("""2 1
5
10
""") == "1 2\n"

# all equal
assert run("""3 2
4 4
4 4
4 4
""") == "3 3 3\n"

# always losing
assert run("""2 2
100 100
1 1
""") == "2 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai người chơi, một lỗ |`1 2`| Một giới hạn hữu ích có thể nằm giữa các giá trị điểm. | 
| Người chơi bình đẳng |`3 3 3`| Mối quan hệ được tính chính xác trong thứ hạng. | 
| Người chơi thống trị |`2 1`| Một cầu thủ không thể trở nên giỏi hơn nếu không có khoảng thời gian hợp lệ. | 
| Mẫu | Kết quả đầu ra mẫu | Tính đúng đắn chung. | 

## Vỏ cạnh 

Trường hợp cạnh thứ nhất là khi giới hạn tốt nhất không phải là một trong những điểm ban đầu. Đối với đầu vào:```
2 1
5
10
```khoảng thời gian`(5,10)`thay đổi so sánh và thuật toán kiểm tra nó vì các hàm điểm là tuyến tính giữa các điểm dừng. 

Trường hợp thứ hai là người chơi không bao giờ có thể đánh bại người chơi khác. Vì:```
2 2
100 100
1 1
```hàm sai phân không bao giờ âm. Trình tạo khoảng thời gian không tạo ra khoảng thời gian chiến thắng, do đó, lượt quét không tính đối thủ nào bị đánh bại đối với người chơi đầu tiên và một đối thủ đối với người thứ hai. 

Trường hợp thứ ba có nhiều điểm giống nhau:```
3 1
7
7
7
```Mỗi cặp so sánh luôn bằng nhau nên không có người chơi nào có khoảng cách thắng. Số lượng đối thủ bị đánh bại tối đa là 0 và mọi hạng sẽ trở thành 3, phù hợp với định nghĩa rằng điểm bằng nhau sẽ có cùng hạng.
