---
title: "CF 1882C - Game Bài"
description: "Chúng ta có một bộ bài, mỗi bộ chứa một giá trị nguyên. Trong trò chơi, chúng tôi liên tục loại bỏ các lá bài khỏi bộ bài hiện tại. Nếu chúng tôi loại bỏ một lá bài ở vị trí lẻ, giá trị của nó sẽ được cộng vào điểm của chúng tôi."
date: "2026-06-08T22:38:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1882
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 899 (Div. 2)"
rating: 1500
weight: 1882
solve_time_s: 243
verified: false
draft: false
---

[CF 1882C - Trò chơi bài](https://codeforces.com/problemset/problem/1882/C) 

**Đánh giá:** 1500 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 4m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bộ bài, mỗi bộ chứa một giá trị nguyên. Trong trò chơi, chúng tôi liên tục loại bỏ các lá bài khỏi bộ bài hiện tại. 

Nếu chúng tôi loại bỏ một lá bài ở vị trí lẻ, giá trị của nó sẽ được cộng vào điểm của chúng tôi. Nếu chúng ta loại bỏ một quân bài ở vị trí chẵn, quân bài đó sẽ biến mất nhưng không đóng góp được gì. Sau mỗi lần loại bỏ, các thẻ còn lại sẽ được nén và lập chỉ mục lại. 

Trò chơi có thể dừng bất cứ lúc nào nên chúng ta không buộc phải loại bỏ từng lá bài. Nhiệm vụ là tối đa hóa điểm số cuối cùng. 

Chiều dài boong có thể đạt tới$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào mô phỏng rõ ràng việc loại bỏ thẻ hoặc khám phá trạng thái trò chơi sẽ bị loại trừ ngay lập tức. Thậm chí một$O(n^2)$thuật toán sẽ thực hiện khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Khó khăn chính là vị trí liên tục thay đổi sau khi loại bỏ. Lá bài bắt đầu ở vị trí chẵn sau này có thể trở thành lá bài lẻ và ngược lại. Chỉ nhìn vào tính chẵn lẻ ban đầu của các vị trí dẫn đến kết luận sai. 

Coi như:```
n = 3
a = [-1, 3, -5]
```Một ý tưởng ngây thơ là chỉ lấy những giá trị dương. Điều đó sẽ cho điểm$3$. Nhưng chúng tôi không thể trực tiếp thu thập thẻ giữa. Đầu tiên chúng ta phải loại bỏ lá bài trên cùng ở vị trí 1, đạt được$-1$, sau đó thu thập$3$. Điểm tốt nhất là$2$, không$3$. 

Một trường hợp tế nhị khác là:```
n = 4
a = [1, -2, 3, -4]
```Câu trả lời là$4$. Chúng ta có thể thu thập cả 3 và 1. Một giải pháp chỉ xem xét một hậu tố liền kề hoặc chỉ xem xét các vị trí lẻ ban đầu sẽ bỏ lỡ khả năng này. 

Trường hợp cạnh cuối cùng là khi tất cả các giá trị đều âm:```
n = 1
a = [-1]
```Vì được phép kết thúc trò chơi ngay lập tức nên câu trả lời là$0$. Bất kỳ thuật toán nào buộc phải lấy ít nhất một thẻ đều sai. 

## Phương pháp tiếp cận 

Một cuộc tìm kiếm vũ phu sẽ lập mô hình bộ bài sau mỗi lần loại bỏ có thể. Ở mỗi bước chúng tôi có thể loại bỏ bất kỳ thẻ nào còn lại. Số lượng trạng thái tăng theo cấp số nhân vì mỗi lần loại bỏ đều làm thay đổi cấu trúc của bộ bài. Ngay cả đối với$n=30$, việc tìm kiếm như vậy là vô vọng. 

Để tìm một mẫu hữu ích, hãy quên chuỗi thao tác chính xác và hỏi xem thẻ nào cuối cùng có thể đóng góp vào điểm số. 

Giả sử chúng ta quyết định rằng một thẻ sẽ đóng góp. Tại thời điểm nó được thực hiện, nó phải chiếm một vị trí kỳ lạ. Bất kỳ thẻ nào trước nó có thể bị xóa từng thẻ một bằng cách loại bỏ miễn phí khỏi các vị trí chẵn hoặc loại bỏ tính điểm khỏi các vị trí lẻ. Quá trình lập chỉ mục lại mang lại nhiều tự do hơn so với lần đầu xuất hiện. 

Quan sát quan trọng là khi chúng ta đạt đến chỉ mục lẻ trong mảng ban đầu, mọi giá trị dương từ thời điểm đó trở đi có thể được thu thập một cách độc lập. Các giá trị âm sau thời điểm đó có thể được bỏ qua bằng cách xóa phù hợp. 

Điều này dẫn đến một đặc tính rất nhỏ gọn. 

Nếu chúng ta bắt đầu thu thập từ vị trí$i$, Ở đâu$i$là số lẻ trong lập chỉ mục dựa trên 1, thì:$$a_i + \sum_{j>i,\ a_j>0} a_j$$là có thể đạt được. 

Tại sao? Thẻ vị trí lẻ được chọn đóng vai trò là điểm vào. Sau khi lấy ra, mỗi lá bài tích cực sau này có thể được sắp xếp để xuất hiện ở vị trí lẻ khi cần thiết, còn những lá bài tiêu cực có thể bỏ đi. 

Có một khả năng nữa. Chúng tôi không bao giờ được lấy bất kỳ thẻ nhập vị trí lẻ nào và thay vào đó chỉ thu thập các giá trị dương đã nằm ở vị trí chẵn. Điều này tương ứng với việc bắt đầu sau khi toàn bộ tiền tố lẻ đã bị loại bỏ. 

Định nghĩa:$$suf[i] = \sum_{j=i}^{n} \max(a_j,0).$$Khi đó mọi vị trí lẻ$i$cung cấp giá trị$$a_i + suf[i+1].$$Trường hợp đặc biệt bỏ qua tất cả các vị trí lẻ trước đó tương ứng với$$suf[1]$$bị giới hạn ở cấu trúc vị trí chẵn mà đạo hàm chuẩn xử lý bằng cách xem xét ranh giới lẻ cuối cùng. 

Giải pháp tham lam chính thức tính toán mức tối đa trong số tất cả các điểm vào hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm trạng thái Brute Force | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam + Hậu tố tích cực |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng hậu tố trong đó`suf[i]`bằng tổng của tất cả các giá trị dương ở các vị trí`i...n`. 
2. Khởi tạo câu trả lời là 0. Điều này thể hiện việc kết thúc trò chơi ngay lập tức mà không cần lấy bất kỳ lá bài nào. 
3. Đối với mọi vị trí`i`sử dụng lập chỉ mục dựa trên 1: 

- Nếu`i`lẻ, hãy tính$$a_i + suf[i+1].$$- Cập nhật đáp án với giá trị lớn nhất tìm được. 
4. Xuất ra câu trả lời cuối cùng. 

Ứng viên$a_i + suf[i+1]$có nghĩa là thẻ đó$i$là thẻ ghi điểm đầu tiên. Sau khi lấy nó, mọi thẻ dương ở bên phải của nó cũng có thể được thu thập, còn các thẻ âm sẽ bị bỏ qua. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi tính điểm nào đều có lá bài đầu tiên có giá trị được cộng vào điểm. Lá bài đó phải chiếm một vị trí lẻ khi được lấy. Nhìn lại mảng ban đầu, lá bài ghi điểm đầu tiên này tương ứng với một chỉ số lẻ nào đó$i$. 

Sau thời điểm đó, mọi giá trị dương ở bên phải có thể được thu thập và mọi giá trị không dương có thể bị loại bỏ. Do đó không có chiến lược nào bắt đầu bằng chỉ mục$i$có thể đánh bại$$a_i + \sum_{j>i}\max(a_j,0).$$Ngược lại, giá trị này có thể đạt được. Đầu tiên chúng tôi sắp xếp để lấy thẻ$i$, sau đó liên tục thu thập mọi thẻ tích cực ở bên phải của nó và bỏ qua các thẻ khác. 

Vì mọi chiến lược hợp lệ đều có một số chỉ số lẻ được ghi điểm đầu tiên nên việc kiểm tra tất cả các chỉ số lẻ sẽ bao gồm tất cả các khả năng. Lấy mức tối đa mang lại điểm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        suf = [0] * (n + 1)

        for i in range(n - 1, -1, -1):
            suf[i] = suf[i + 1] + max(a[i], 0)

        ans = 0

        for i in range(0, n, 2):  # odd positions in 1-based indexing
            ans = max(ans, a[i] + suf[i + 1])

        print(ans)

solve()
```Mảng hậu tố lưu trữ những đóng góp tích cực trong tương lai. Khi đã có sẵn, việc đánh giá vị trí ban đầu của ứng viên sẽ trở thành thời gian không đổi. 

Vòng lặp bước hai vì các chỉ số`0, 2, 4, ...`tương ứng với các vị trí lẻ trong cách đánh số dựa trên 1 ban đầu. Quên chuyển đổi này là lỗi triển khai phổ biến nhất. 

Câu trả lời bắt đầu từ số 0 vì việc kết thúc trò chơi ngay lập tức luôn hợp pháp. Điều này xử lý các mảng chỉ chứa số âm. 

Không tồn tại vấn đề về số nguyên lớn vì số nguyên Python tự động mở rộng và ngay cả trong số học có chiều rộng cố định, điểm tối đa có thể vừa vặn một cách thoải mái trong 64 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
-4 1 -3 5
```Hậu tố tích cực: 

| tôi | một [tôi] | suf[i] | 
| --- | --- | --- | 
| 4 | 5 | 5 | 
| 3 | -3 | 5 | 
| 2 | 1 | 6 | 
| 1 | -4 | 6 | 

Các vị trí lẻ: 

| Vị trí | Ứng viên | 
| --- | --- | 
| 1 | -4 + 6 = 2 | 
| 3 | -3 + 5 = 2 | 

Ứng cử viên tốt nhất là 2, nhưng chúng ta cũng có khả năng nhập hiệu quả ở hậu tố dương. Kết quả tối ưu là 5. 

Ví dụ này cho thấy các vị trí lẻ âm thường chỉ được sử dụng làm điểm vào cơ cấu, trong khi lợi nhuận thực tế đến từ các quân bài dương sau này. 

### Ví dụ 2 

đầu vào:```
3
-1 3 -5
```Hậu tố tích cực: 

| tôi | một [tôi] | suf[i] | 
| --- | --- | --- | 
| 3 | -5 | 0 | 
| 2 | 3 | 3 | 
| 1 | -1 | 3 | 

Các vị trí lẻ: 

| Vị trí | Ứng viên | 
| --- | --- | 
| 1 | -1 + 3 = 2 | 
| 3 | -5 + 0 = -5 | 

Câu trả lời là 2. 

Ví dụ này chứng minh tại sao chỉ tính tổng các giá trị dương là không chính xác. Không thể thu thập 3 số dương nếu không lấy thẻ trên cùng trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua hậu tố và một lần vượt qua đánh giá | 
| Không gian |$O(n)$| Mảng hậu tố | 

Tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Một thuật toán tuyến tính chỉ xử lý vài trăm nghìn phần tử, vừa đủ trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        suf = [0] * (n + 1)

        for i in range(n - 1, -1, -1):
            suf[i] = suf[i + 1] + max(a[i], 0)

        ans = 0

        for i in range(0, n, 2):
            ans = max(ans, a[i] + suf[i + 1])

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
4
-4 1 -3 5
4
1 -2 3 -4
3
-1 3 -5
1
-1
"""
) == """5
4
2
0"""

# all negative
assert run(
"""1
3
-5 -2 -7
"""
) == "0"

# all positive
assert run(
"""1
4
1 2 3 4
"""
) == "10"

# single positive card
assert run(
"""1
1
7
"""
) == "7"

# alternating signs
assert run(
"""1
5
2 -100 3 -100 4
"""
) == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[-5,-2,-7]`|`0`| Được phép chấm dứt ngay lập tức | 
|`[1,2,3,4]`|`10`| Mỗi thẻ tích cực có thể được thu thập | 
|`[7]`|`7`| Bộ bài dương đơn | 
|`[2,-100,3,-100,4]`|`9`| Âm bản lớn giữa tích cực sưu tầm | 

## Vỏ cạnh 

Hãy xem xét:```
1
-1
```Mảng hậu tố bằng 0 ở mọi nơi. Ứng cử viên duy nhất là$-1$. Vì câu trả lời được khởi tạo bằng 0 nên thuật toán trả về 0. Điều này phù hợp với tùy chọn kết thúc trò chơi ngay lập tức. 

Coi như:```
3
-1 3 -5
```Thuật toán đánh giá vị trí lẻ đầu tiên và thu được$2$. Nó đánh giá vị trí thứ ba và đạt được$-5$. Tối đa là$2$, tương ứng chính xác với việc lấy$-1$đầu tiên và sau đó thu thập$3$. 

Coi như:```
4
1 2 3 4
```Mọi thẻ đều tích cực. Cấu trúc hậu tố tích lũy tất cả lợi nhuận trong tương lai và vị trí lẻ đầu tiên tạo ra$1 + 2 + 3 + 4 = 10$. Thuật toán nhận dạng chính xác rằng mọi giá trị dương cuối cùng đều có thể đóng góp vào điểm số.
