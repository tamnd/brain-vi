---
title: "CF 911A - Mức tối thiểu gần nhất"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong số tất cả các giá trị trong mảng, có một giá trị nhỏ nhất và bài toán đảm bảo rằng giá trị tối thiểu này xuất hiện ít nhất hai lần. Nhiệm vụ của chúng ta là tìm khoảng cách nhỏ nhất giữa hai lần xuất hiện của giá trị nhỏ nhất đó."
date: "2026-06-12T10:19:32+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 911
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 35 (Rated for Div. 2)"
rating: 1100
weight: 911
solve_time_s: 210
verified: true
draft: false
---

[CF 911A - Mức tối thiểu gần nhất](https://codeforces.com/problemset/problem/911/A) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong số tất cả các giá trị trong mảng, có một giá trị nhỏ nhất và bài toán đảm bảo rằng giá trị tối thiểu này xuất hiện ít nhất hai lần. 

Nhiệm vụ của chúng ta là tìm khoảng cách nhỏ nhất giữa hai lần xuất hiện của giá trị nhỏ nhất đó. Khoảng cách giữa các vị trí chỉ đơn giản là sự khác biệt giữa các chỉ số của chúng. 

Ví dụ: trong mảng:```
[5, 2, 7, 2, 9]
```giá trị tối thiểu là`2`, xuất hiện ở chỉ số`1`Và`3`. Khoảng cách của họ là`3 - 1 = 2`, vậy câu trả lời là`2`. 

Kích thước mảng có thể lớn như`100000`. Với ràng buộc này, một thuật toán so sánh từng cặp vị trí sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong giới hạn thời gian 2 giây. Chúng ta cần một giải pháp xử lý mảng theo thời gian tuyến tính. 

Một vài trường hợp đáng chú ý. 

Hãy xem xét một mảng trong đó mọi phần tử đều có giá trị tối thiểu:```
4
1 1 1 1
```Mức tối thiểu xuất hiện tại các chỉ số`0, 1, 2, 3`. Cặp gần nhất liền kề nhau nên đáp án là`1`. Việc triển khai bất cẩn chỉ so sánh lần xuất hiện đầu tiên và lần xuất hiện cuối cùng sẽ trả về không chính xác`3`. 

Hãy xem xét trường hợp các giá trị tối thiểu được phân tách không đồng đều:```
5
2 8 2 3 2
```Mức tối thiểu xuất hiện tại các chỉ số`0, 2, 4`. Khoảng cách là`2`Và`2`, vậy câu trả lời là`2`. Chúng ta phải kiểm tra tất cả các lần xuất hiện liên tiếp chứ không chỉ một cặp. 

Một trường hợp quan trọng khác là đầu vào hợp lệ nhỏ nhất:```
2
3 3
```Mức tối thiểu xảy ra ở cả hai vị trí và câu trả lời là`1`. Những sai lầm ngẫu nhiên trong tính toán chỉ số thường xuất hiện trong trường hợp biên này. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là trước tiên hãy xác định vị trí tất cả các lần xuất hiện có giá trị nhỏ nhất, sau đó so sánh từng cặp lần xuất hiện đó và lấy khoảng cách nhỏ nhất. 

Điều này hiệu quả vì câu trả lời được xác định là khoảng cách tối thiểu giữa hai vị trí chứa giá trị tối thiểu. Nếu có`k`xảy ra, chúng ta có thể kiểm tra tất cả`k(k-1)/2`cặp và tính khoảng cách của chúng. 

Vấn đề xuất hiện khi nhiều phần tử bằng mức tối thiểu. Trong trường hợp xấu nhất, tất cả`100000`các phần tử là tối thiểu. Khi đó chúng ta sẽ cần khoảng:```
100000 × 99999 / 2 ≈ 5 × 10^9
```so sánh cặp, quá chậm. 

Quan sát quan trọng là sau khi chúng ta biết giá trị tối thiểu, chỉ có vị trí của nó là quan trọng. Giả sử mức tối thiểu xảy ra tại các vị trí:```
p1 < p2 < p3 < ... < pk
```Nếu chúng ta muốn khoảng cách nhỏ nhất giữa hai lần xuất hiện bất kỳ thì nó phải xảy ra giữa hai vị trí liên tiếp trong danh sách được sắp xếp này. 

Tại sao? Nếu chúng ta đảm nhận vị trí`pi`Và`pj`với`j > i + 1`, thì có ít nhất một lần xuất hiện giữa chúng. Khoảng cách`pj - pi`lớn hơn khoảng cách lân cận trong khoảng đó. Một cặp như vậy không bao giờ có thể tạo ra khoảng cách tối thiểu. 

Điều đó có nghĩa là chúng ta chỉ cần kiểm tra các lần xuất hiện liên tiếp của giá trị nhỏ nhất. 

Đầu tiên chúng ta có thể tìm phần tử nhỏ nhất của mảng. Sau đó, chúng tôi quét lại mảng, ghi nhớ vị trí gần đây nhất nơi mức tối thiểu xuất hiện. Mỗi lần chúng tôi tìm thấy một lần xuất hiện khác, chúng tôi tính toán khoảng cách đến lần xuất hiện trước đó và cập nhật câu trả lời. 

Điều này đưa ra một giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tìm giá trị nhỏ nhất trong mảng. 
3. Khởi tạo`last = -1`để thể hiện rằng chưa có sự xuất hiện của mức tối thiểu nào được nhìn thấy. 
4. Khởi tạo`answer`với giá trị rất lớn. 
5. Quét mảng từ trái sang phải. 
6. Bất cứ khi nào phần tử hiện tại bằng giá trị tối thiểu: 

1. Nếu`last`đã lưu trữ lần xuất hiện trước đó, hãy tính khoảng cách`current_index - last`. 
2. Cập nhật`answer`nếu khoảng cách này nhỏ hơn. 
3. Lưu chỉ mục hiện tại vào`last`. 
7. Sau khi quét xong, hãy in`answer`. 

Lý do điều này có tác dụng là do quá trình quét tự nhiên gặp phải số lần xuất hiện tối thiểu theo thứ tự chỉ mục tăng dần. Mỗi khoảng cách được tính toán chính xác là khoảng cách giữa hai lần xuất hiện liên tiếp của giá trị tối thiểu. 

### Tại sao nó hoạt động 

Đặt vị trí có giá trị nhỏ nhất là:```
p1 < p2 < p3 < ... < pk
```Thuật toán tính toán tất cả các khoảng cách:```
p2 - p1
p3 - p2
...
pk - p(k-1)
```Xem xét bất kỳ cặp không liên tiếp`pi`Và`pj`với`j > i + 1`. Khoảng cách của nó là:```
pj - pi
```Trong khoảng này có ít nhất một khoảng trống liên tiếp. Khoảng cách liên tiếp đó nhỏ hơn hoặc bằng`pj - pi`. Do đó, khoảng cách nhỏ nhất có thể có giữa hai lần xuất hiện phải xuất hiện giữa các lần xuất hiện liên tiếp. 

Vì thuật toán kiểm tra từng cặp liên tiếp chính xác một lần và lấy khoảng cách tối thiểu nên nó luôn trả về câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)

    last = -1
    ans = n

    for i in range(n):
        if a[i] == mn:
            if last != -1:
                ans = min(ans, i - last)
            last = i

    print(ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên là tìm giá trị nhỏ nhất của mảng. Chỉ những lần xuất hiện của giá trị này mới có thể đóng góp vào câu trả lời, vì vậy tất cả các phần tử khác có thể bị bỏ qua sau đó. 

Biến`last`lưu trữ chỉ mục gần đây nhất nơi xuất hiện giá trị tối thiểu. Khi tìm thấy một lần xuất hiện khác, khoảng cách giữa hai chỉ số chính xác là khoảng cách giữa các mức tối thiểu liên tiếp. 

Biến`ans`theo dõi khoảng cách nhỏ nhất được thấy cho đến nay. Nó được khởi tạo thành`n`, lớn hơn một cách an toàn so với bất kỳ câu trả lời hợp lệ nào vì khoảng cách tối đa giữa hai chỉ số trong một mảng có độ dài`n`là`n - 1`. 

Một lỗi phổ biến là cập nhật`last`trước khi tính khoảng cách. Làm như vậy sẽ so sánh một chỉ mục với chính nó và luôn cho kết quả bằng 0. Thứ tự đúng là tính khoảng cách trước, sau đó cập nhật`last`. 

Không có lo ngại về tràn vì chênh lệch chỉ số nhiều nhất`99999`, nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3 3
```Giá trị tối thiểu là`3`. 
| tôi | một [tôi] | cuối cùng trước | Khoảng cách | và sau | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | -1 | - | 2 | 0 | 
| 1 | 3 | 0 | 1 | 1 | 1 | 
Đầu ra:```
1
```Ví dụ này hiển thị đầu vào hợp lệ nhỏ nhất. Thuật toán phát hiện chính xác lần xuất hiện thứ hai và tính khoảng cách đến lần xuất hiện trước đó. 

### Ví dụ 2 

đầu vào:```
6
4 1 7 1 9 1
```Giá trị tối thiểu là`1`. 

| tôi | một [tôi] | cuối cùng trước | Khoảng cách | trả lời sau | cuối cùng sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | -1 | - | 6 | -1 | 
| 1 | 1 | -1 | - | 6 | 1 | 
| 2 | 7 | 1 | - | 6 | 1 | 
| 3 | 1 | 1 | 2 | 2 | 3 | 
| 4 | 9 | 3 | - | 2 | 3 | 
| 5 | 1 | 3 | 2 | 2 | 5 | 

Đầu ra:```
2
```Dấu vết này thể hiện sự bất biến cốt lõi. Tại mỗi lần xuất hiện tối thiểu,`last`chứa vị trí tối thiểu trước đó, vì vậy mỗi khoảng cách được tính toán là khoảng cách giữa các mức tối thiểu liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm mức tối thiểu và một lượt để tính số lần xuất hiện gần nhất | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Với`n ≤ 100000`, thời gian tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ không đổi, thấp hơn nhiều so với giới hạn khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    n = int(input())
    a = list(map(int, input().split()))

    mn = min(a)

    last = -1
    ans = n

    for i in range(n):
        if a[i] == mn:
            if last != -1:
                ans = min(ans, i - last)
            last = i

    return str(ans)

# provided sample
assert run("2\n3 3\n") == "1", "sample 1"

# minimum size input
assert run("2\n1 1\n") == "1", "minimum size"

# all values equal
assert run("4\n5 5 5 5\n") == "1", "all equal"

# uneven gaps between minimums
assert run("5\n2 8 2 3 2\n") == "2", "multiple minimum positions"

# nearest pair at the end
assert run("6\n1 7 8 9 1 1\n") == "1", "adjacent minimums at end"

# larger spacing
assert run("7\n4 1 5 6 7 1 8\n") == "4", "distance calculation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 1`|`1`| Đầu vào pháp lý nhỏ nhất | 
|`4 / 5 5 5 5`|`1`| Tất cả các yếu tố đều ở mức tối thiểu | 
|`5 / 2 8 2 3 2`|`2`| Nhiều lần xuất hiện tối thiểu | 
|`6 / 1 7 8 9 1 1`|`1`| Tối thiểu liền kề tạo ra câu trả lời | 
|`7 / 4 1 5 6 7 1 8`|`4`| Tính toán chênh lệch chỉ số đúng | 

## Vỏ cạnh 

### Tất cả các phần tử đều bằng nhau 

đầu vào:```
4
1 1 1 1
```Giá trị tối thiểu là`1`, xuất hiện ở chỉ số`0, 1, 2, 3`. 

Thuật toán tính khoảng cách:```
1 - 0 = 1
2 - 1 = 1
3 - 2 = 1
```Giá trị nhỏ nhất của các giá trị này là`1`, đó là câu trả lời đúng. Chỉ nhìn vào lần xuất hiện đầu tiên và cuối cùng sẽ tạo ra kết quả không chính xác`3`. 

### Giá trị tối thiểu có khoảng cách không đều 

đầu vào:```
5
2 8 2 3 2
```Giá trị tối thiểu là`2`, xuất hiện ở chỉ số`0, 2, 4`. 

Thuật toán tính toán:```
2 - 0 = 2
4 - 2 = 2
```Câu trả lời trở thành`2`. 

Điều này chứng tỏ tại sao việc kiểm tra các lần xuất hiện liên tiếp là đủ. Cặp không liên tiếp`(0, 4)`có khoảng cách`4`, lớn hơn và không thể tối ưu. 

### Đầu vào hợp lệ nhỏ nhất 

đầu vào:```
2
3 3
```Giá trị tối thiểu xuất hiện tại các chỉ số`0`Và`1`. 

Tập hợp lần xuất hiện đầu tiên`last = 0`. Lần xuất hiện thứ hai tạo ra:```
1 - 0 = 1
```Đầu ra của thuật toán`1`, đây là khoảng cách duy nhất có thể có trong mảng hai phần tử. Trường hợp này xác nhận rằng việc khởi tạo và xử lý ranh giới là chính xác.
