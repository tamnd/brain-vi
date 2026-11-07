---
title: "CF 1362B - Johnny và sở thích của anh ấy"
description: "Chúng ta được cho một tập hợp các số nguyên phân biệt. Johnny chọn một số nguyên dương k và thay thế mọi giá trị s trong tập hợp bằng s XOR k. Việc chuyển đổi được áp dụng đồng thời cho mọi phần tử."
date: "2026-06-11T12:34:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1362
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 647 (Div. 2) - Thanks, Algo Muse!"
rating: 1200
weight: 1362
solve_time_s: 122
verified: true
draft: false
---

[CF 1362B - Johnny và sở thích của anh ấy](https://codeforces.com/problemset/problem/1362/B) 

**Đánh giá:** 1200 
**Tags:** bitmask, vũ lực 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên phân biệt. Johnny chọn một số nguyên dương`k`và thay thế mọi giá trị`s`trong bộ với`s XOR k`. 

Việc chuyển đổi được áp dụng đồng thời cho mọi phần tử. Sau khi chuyển đổi, chúng ta muốn tập kết quả giống hệt tập ban đầu. Vì chúng ta đang xử lý các tập hợp nên thứ tự của các phần tử không quan trọng. 

Nhiệm vụ của chúng ta là tìm số nguyên dương nhỏ nhất`k`vì cái gì$$\{s \oplus k \mid s \in S\} = S$$hoặc báo cáo rằng không tồn tại số nguyên dương như vậy. 

Những hạn chế nhỏ một cách bất thường. Mọi số trong tập hợp đều nhỏ hơn`1024`, có nghĩa là nó sử dụng tối đa 10 bit. Số lượng phần tử trong một ca kiểm thử nhiều nhất là`1024`và tổng số phần tử trong tất cả các trường hợp thử nghiệm cũng nhiều nhất là`1024`. 

Những giới hạn đó ngay lập tức gợi ý rằng việc thử nhiều giá trị ứng viên của`k`có thể hoàn toàn khả thi. Vì tất cả các giá trị đều nằm trong phạm vi`[0, 1023]`, bất kỳ kết quả XOR nào được tạo ra từ hai số như vậy cũng nằm trong phạm vi đó. Việc tìm kiếm trên tất cả các mặt nạ 10 bit có thể chỉ tốn khoảng một nghìn lần lặp. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một tập hợp một phần tử:```
1
1
5
```Bất kỳ tích cực`k`thay đổi`5`thành một giá trị khác, do đó tập hợp không thể thay đổi. Câu trả lời đúng là:```
-1
```Một giải pháp bất cẩn có thể cho rằng mọi tập hợp đều có một số đối xứng và trả về một giá trị không chính xác. 

Một trường hợp quan trọng khác là khi đáp án lớn:```
1
2
0 1023
```sử dụng`k = 1023`hoán đổi hai yếu tố:```
0 XOR 1023 = 1023
1023 XOR 1023 = 0
```Bộ này không thay đổi nên đáp án là:```
1023
```Một giải pháp chỉ kiểm tra mặt nạ xuất hiện giữa các phần tử sẽ bỏ lỡ điều này. 

Một trường hợp tế nhị hơn là:```
1
3
1 2 3
```Thử các mặt nạ khác nhau không bao giờ tạo ra cùng một bộ. Câu trả lời đúng là:```
-1
```Thật hấp dẫn khi chỉ tìm kiếm các mối quan hệ theo cặp, nhưng toàn bộ tập hợp được chuyển đổi phải khớp chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là kiểm tra mọi số nguyên dương có thể`k`. 

Với mỗi ứng cử viên, XOR mọi phần tử của tập hợp với`k`, xây dựng tập đã biến đổi và so sánh nó với tập ban đầu. Nếu chúng khớp nhau, chúng tôi đã tìm thấy câu trả lời hợp lệ. 

Cách tiếp cận bạo lực này là chính xác vì nó thực sự kiểm tra điều kiện từ câu lệnh. Câu hỏi đặt ra là liệu nó có đủ nhanh hay không. 

Quan sát chính đến từ phạm vi giá trị. Mọi phần tử đều nhỏ hơn`1024`, vì vậy tất cả các mặt nạ XOR có liên quan cũng nhỏ hơn`1024`. Không có lý do để kiểm tra bất cứ điều gì ngoài phạm vi`[1, 1023]`. 

Đối với mỗi mặt nạ ứng cử viên, chúng tôi xử lý nhiều nhất`n ≤ 1024`các phần tử. Công việc trong trường hợp xấu nhất trên mỗi trường hợp thử nghiệm là:$$1023 \times 1024 \approx 10^6$$hoạt động. 

Cho rằng tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm chỉ`1024`, điều này thoải mái trong giới hạn. 

Quan sát cho thấy tất cả các số đều có giá trị 10 bit biến cái thường trông giống như một không gian tìm kiếm khổng lồ thành một không gian tìm kiếm nhỏ bé. Thay vì tìm kiếm trên các số nguyên tùy ý, chúng ta chỉ cần kiểm tra 1023 mặt nạ có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các số nguyên | Không giới hạn | O(n) | Không thể | 
| Liệt kê tất cả các mặt nạ 1..1023 | O(1024·n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bộ số. 
2. Lưu trữ các phần tử ban đầu trong tập băm để kiểm tra tính thành viên và đẳng thức của O(1). 
3. Lặp lại qua từng mặt nạ ứng cử viên`k`từ`1`ĐẾN`1023`. 
4. Đối với mặt nạ hiện tại, tính toán tất cả các giá trị`x XOR k`cho mọi phần tử`x`trong bộ phim. 
5. Xây dựng một bộ mới chứa các giá trị được chuyển đổi đó. 
6. So sánh tập đã biến đổi với tập ban đầu. 

Nếu chúng giống hệt nhau thì áp dụng`k`bảo toàn tập hợp một cách chính xác. 
7. Vì các mặt nạ được kiểm tra theo thứ tự tăng dần nên mặt nạ hợp lệ đầu tiên sẽ tự động là câu trả lời hợp lệ nhỏ nhất. Xuất nó ngay lập tức. 
8. Nếu không có mặt nạ nào hoạt động sau khi kiểm tra tất cả các giá trị từ`1`ĐẾN`1023`, đầu ra`-1`. 

### Tại sao nó hoạt động 

Đối với bất kỳ câu trả lời hợp lệ,`k`phải là số 10 bit vì tất cả các phần tử tập hợp đều nằm trong`[0,1023]`. Thuật toán sẽ kiểm tra mọi mặt nạ tích cực trong toàn bộ phạm vi đó. 

Đối với mặt nạ cố định, tập được chuyển đổi chính xác là tập được mô tả trong câu lệnh bài toán. So sánh nó với tập hợp ban đầu sẽ trực tiếp xác minh xem điều kiện có đúng hay không. 

Vì mọi mặt nạ ứng cử viên có thể đều được kiểm tra và mặt nạ hợp lệ đầu tiên được trả về nên thuật toán không thể bỏ sót một giải pháp và không thể trả về một giải pháp lớn hơn khi tồn tại một giải pháp nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        original = set(arr)

        answer = -1

        for k in range(1, 1024):
            transformed = {x ^ k for x in arr}

            if transformed == original:
                answer = k
                break

        answers.append(str(answer))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo thuật toán gần như theo đúng nghĩa đen. 

Các số ban đầu được lưu trữ trong Python`set`, cho phép so sánh bình đẳng hiệu quả. Hai bộ bằng nhau khi và chỉ khi chúng chứa chính xác các phần tử giống nhau, phù hợp với định nghĩa bài toán. 

Đối với mỗi mặt nạ ứng viên, việc hiểu tập hợp sẽ tạo ra các giá trị được chuyển đổi. Vì đầu vào đã bao gồm các giá trị riêng biệt nên kết quả được chuyển đổi cũng vẫn là một tập hợp các giá trị riêng biệt. So sánh trực tiếp hai tập hợp để kiểm tra xem phép biến đổi có làm tập hợp đó không thay đổi hay không. 

Vòng lặp trên mặt nạ bắt đầu tại`1`, không`0`. Câu lệnh yêu cầu một số nguyên dương`k`, Vì thế`0`không được phép mặc dù nó sẽ luôn bảo toàn tập hợp. 

Việc tìm kiếm dừng ngay sau khi tìm thấy mặt nạ hợp lệ vì mặt nạ được xử lý theo thứ tự tăng dần. Điều này đảm bảo câu trả lời hợp lệ nhỏ nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
4
1 0 2 3
```Bộ gốc:```
{0, 1, 2, 3}
```| k | Bộ Chuyển Đổi | Trận đấu? | 
| --- | --- | --- | 
| 1 | {1,0,3,2} = {0,1,2,3} | Có | 

Mặt nạ đầu tiên đã hoạt động nên câu trả lời là:```
1
```Ví dụ này chứng tỏ rằng XOR có thể đơn giản hoán vị các phần tử bên trong tập hợp mà không thay đổi chính tập hợp đó. 

### Ví dụ 2 

đầu vào:```
1
3
1 2 3
```| k | Bộ Chuyển Đổi | Trận đấu? | 
| --- | --- | --- | 
| 1 | {0,3,2} | Không | 
| 2 | {3,0,1} | Không | 
| 3 | {2,1,0} | Không | 
| ... | ... | ... | 
| 1023 | {1022,1021,1020} | Không | 

Không có mặt nạ ứng cử viên nào tái tạo bộ gốc. 

Đầu ra:```
-1
```Ví dụ này cho thấy rằng không phải mọi tập hợp đều có tính đối xứng XOR cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1024·n) | Kiểm tra 1023 mặt nạ, mỗi mặt nạ xử lý n phần tử | 
| Không gian | O(n) | Lưu trữ bộ gốc và bộ chuyển đổi | 

Từ`n ≤ 1024`và tổng số phần tử trong tất cả các trường hợp thử nghiệm nhiều nhất là`1024`, công việc trong trường hợp xấu nhất chỉ là khoảng một triệu phép tính XOR. Điều này dễ dàng phù hợp với giới hạn thời gian 1 giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        s = set(arr)

        cur = -1
        for k in range(1, 1024):
            if {x ^ k for x in arr} == s:
                cur = k
                break

        ans.append(str(cur))

    return "\n".join(ans) + "\n"

# provided sample
assert run(
"""6
4
1 0 2 3
6
10 7 14 8 3 12
2
0 2
3
1 2 3
6
1 4 6 10 11 12
2
0 1023
"""
) == """1
4
2
-1
-1
1023
"""

# minimum size
assert run(
"""1
1
0
"""
) == """-1
""", "single element"

# simple swap
assert run(
"""1
2
0 2
"""
) == """2
""", "two elements exchanged by xor 2"

# full 2-bit space
assert run(
"""1
4
0 1 2 3
"""
) == """1
""", "smallest valid mask"

# boundary values
assert run(
"""1
2
0 1023
"""
) == """1023
""", "largest possible answer"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`{0}`|`-1`| Tập hợp một phần tử không thể không thay đổi trong XOR dương | 
|`{0,2}`|`2`| Hoán đổi hai phần tử đơn giản | 
|`{0,1,2,3}`|`1`| Có nhiều mặt nạ hợp lệ, mặt nạ nhỏ nhất phải được trả lại | 
|`{0,1023}`|`1023`| Giá trị biên và mặt nạ lớn nhất có thể | 

## Vỏ cạnh 

Tập hợp một phần tử không có câu trả lời hợp lệ:```
1
1
5
```Thuật toán kiểm tra mọi mặt nạ từ`1`ĐẾN`1023`. Mỗi phép biến đổi tạo ra một tập hợp đơn lẻ khác nhau:```
{5 XOR k}
```điều không bao giờ có thể bằng`{5}`bởi vì`k > 0`. Việc tìm kiếm kết thúc và kết quả đầu ra:```
-1
```Đối với trường hợp ranh giới```
1
2
0 1023
```thuật toán cuối cùng đạt đến`k = 1023`và tính toán:```
0 XOR 1023 = 1023
1023 XOR 1023 = 0
```Tập đã biến đổi trở thành`{0,1023}`, phù hợp với bản gốc. Vì không có mặt nạ nhỏ hơn nào hoạt động nên kết quả là:```
1023
```Đối với một tập hợp không có đối xứng,```
1
3
1 2 3
```mỗi mặt nạ ứng cử viên sẽ tạo ra một bộ khác nhau. Kiểm tra đẳng thức mỗi lần đều thất bại, do đó thuật toán trả về chính xác:```
-1
```So sánh tập hợp trực tiếp ngăn chặn các kết quả dương tính giả có thể phát sinh từ việc chỉ kiểm tra mối quan hệ một phần giữa các phần tử.
