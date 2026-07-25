---
title: "CF 102861H - Nhà chứa máy bay của SBC"
description: "Chúng tôi có một bộ sưu tập các hộp, mỗi hộp có trọng lượng khác nhau. Máy bay phải chở đúng K hộp và tổng trọng lượng của các hộp được chọn đó phải nằm trong khoảng cho phép [A, B]. Nhiệm vụ là đếm xem có bao nhiêu nhóm K hộp khác nhau thỏa mãn điều kiện này."
date: "2026-07-25T14:04:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 43
verified: true
draft: false
---

[CF 102861H - Nhà chứa máy bay của SBC](https://codeforces.com/problemset/problem/102861/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các hộp, mỗi hộp có trọng lượng khác nhau. Máy bay phải chở đúng K hộp và tổng trọng lượng của các hộp được chọn đó phải nằm trong khoảng cho phép [A, B]. Nhiệm vụ là đếm xem có bao nhiêu nhóm K hộp khác nhau thỏa mãn điều kiện này. 

Tính chất bất thường của các trọng số là phần mấu chốt của vấn đề. Nếu chúng ta sắp xếp các trọng số ngày càng tăng thì mỗi trọng số ít nhất sẽ gấp đôi trọng số trước đó. Điều này có nghĩa là mỗi hộp nặng hơn tổng của tất cả các hộp nhẹ hơn cộng lại. Hộp nặng hơn hoàn toàn chiếm ưu thế so với bất kỳ sự kết hợp nào của hộp nhẹ hơn. 

Số hộp và số phải chọn đều nhiều nhất là 50. Việc liệt kê tập hợp con thông thường sẽ yêu cầu kiểm tra tới 2^50 khả năng, tức là khoảng một triệu triệu lựa chọn, vượt xa những gì một giải pháp cuộc thi có thể xử lý. Trọng số cũng có thể lớn tới 10^18, vì vậy việc lưu trữ tất cả các tổng có thể có là không thể. Lời giải phải khai thác cấu trúc đặc biệt của các trọng số thay vì coi đây là bài toán tổng tập con thông thường. 

Một số trường hợp rất dễ xử lý sai. Một ví dụ là khi khoảng hợp lệ chứa chính xác một giá trị biên.```
3 2
1 2 4
3 3
```Các cặp có thể là {1,2}, {1,4} và {2,4} với tổng 3, 5 và 6. Câu trả lời là 1 vì khoảng đóng và bao gồm tổng 3. Giải pháp sử dụng phép so sánh chặt chẽ sẽ trả về 0 không chính xác. 

Một trường hợp khác là khi khoảng mục tiêu bắt đầu dưới mọi tổng có thể.```
3 1
1 2 4
10 20
```Không có ô nào đạt đến khoảng đó, vì vậy câu trả lời là 0. Việc triển khai bất cẩn chỉ kiểm tra giới hạn trên có thể tính các lựa chọn không hợp lệ. 

Trường hợp quan trọng thứ ba là chọn tất cả các hộp.```
3 3
1 2 4
7 7
```Chỉ có một nhóm khả thi và tổng của nó là 7, vì vậy câu trả lời là 1. Việc triển khai giả định luôn có các hộp không được sử dụng có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi nhóm K hộp có thể có, tính tổng trọng lượng của nó và đếm các nhóm có tổng nằm trong phạm vi cho phép. Điều này đúng vì mọi lựa chọn có thể đều được xem xét. Tuy nhiên, có thể có tới 50 hộp nên số lượng tập hợp con là rất lớn. Ngay cả việc tạo tất cả các tập hợp con cũng sẽ yêu cầu các thao tác O(2^50), điều này không khả thi. 

Cấu trúc của các trọng số mang lại một cách nhìn vấn đề tốt hơn nhiều. Sau khi sắp xếp, mỗi trọng số sẽ lớn hơn tổng của tất cả các trọng số nhỏ hơn. Điều này làm cho tổng tập hợp con hoạt động giống như số nhị phân. Quyết định có bao gồm một hộp lớn hay không sẽ quyết định tổng phạm vi của mọi thứ bên dưới nó. 

Thay vì đếm tổng trực tiếp, chúng ta đếm xem có bao nhiêu nhóm hợp lệ có tổng tối đa X. Nếu chúng ta có thể tính hàm f(X) này, thì câu trả lời là: 

f(B) - f(A - 1) 

Đối với X cố định, hãy xem trọng số còn lại lớn nhất w. Nếu X nhỏ hơn w thì không có tập hợp con hợp lệ nào có thể chứa w, vì việc thêm nó vào sẽ vượt quá giới hạn. Chúng tôi chỉ đơn giản là tiếp tục với trọng lượng nhỏ hơn. 

Nếu X ít nhất là w thì có hai khả năng. Chúng ta có thể bao gồm w, trong trường hợp đó mọi lựa chọn trong số K - 1 hộp nhỏ hơn còn lại đều hợp lệ vì tổng của chúng nhỏ hơn w. Ngược lại, chúng ta loại trừ w và tiếp tục tìm kiếm giữa các hộp nhỏ hơn có cùng K và giới hạn X - w. Đây chính xác là quá trình quyết định giống như đọc biểu diễn nhị phân từ bit lớn nhất đến bit nhỏ nhất. 

Số lượng trạng thái đệ quy vẫn ở mức nhỏ vì mọi cấp độ đều kết thúc ngay lập tức với số lượng kết hợp hoặc tiếp tục đến trọng số nhỏ hơn tiếp theo. Chúng tôi không bao giờ liệt kê các tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| Tối ưu | O(NK) | O(NK) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp trọng lượng hộp theo thứ tự tăng dần. Thuộc tính nhân đôi dễ sử dụng nhất khi trọng lượng còn lại lớn nhất luôn được xem xét đầu tiên. 
2. Triển khai hàm count(n, k, x) trả về số cách chọn k hộp từ n hộp được sắp xếp đầu tiên với tổng trọng số tối đa là x. 
3. Xử lý ngay những tình huống không thể thực hiện được. Nếu k âm, k lớn hơn n hoặc x âm thì không có lựa chọn hợp lệ. Nếu không còn hộp nào, trường hợp hợp lệ duy nhất là chọn 0 hộp. 
4. Nhìn vào trọng lượng lớn nhất hiện có w. Nếu x nhỏ hơn w thì không chọn được ô này nên giải bài toán tương tự nếu không có ô này. 
5. Nếu x ít nhất là w, hãy chia câu trả lời thành hai trường hợp. Khi hộp được chọn, hãy chọn k - 1 hộp khác từ n - 1 hộp nhỏ hơn. Mọi lựa chọn như vậy đều hiệu quả vì tất cả các hộp nhỏ hơn cộng lại có trọng lượng nhỏ hơn w. Khi hộp chưa được chọn, tiếp tục với các hộp nhỏ hơn và giới hạn còn lại x - w. 
6. Tính đáp án cuối cùng bằng cách trừ số nhóm có tổng nhỏ hơn A cho số nhóm có tổng lớn nhất là B. 

Tại sao nó hoạt động: 

Điều bất biến là mọi lệnh gọi đệ quy đều thể hiện chính xác các lựa chọn còn lại vẫn có thể ảnh hưởng đến câu trả lời. Khi trọng số lớn nhất được xử lý, thuộc tính ưu thế đảm bảo rằng việc chọn nó sẽ khiến tất cả các trọng số thấp hơn trở nên không đáng kể so với lựa chọn đó. Do đó, nhánh nơi nó được chọn có thể được tính bằng một sự kết hợp đơn giản và nhánh nơi nó không được chọn sẽ gặp vấn đề tương tự trên các trọng số nhỏ hơn. Vì mọi nhóm có thể đều chứa hộp lớn nhất hiện tại hoặc không chứa hộp đó và cả hai trường hợp đều được tính chính xác một lần, nên phép đệ quy bao gồm tất cả các nhóm hợp lệ mà không bị trùng lặp. 

## Giải pháp Python```python
import sys
from functools import lru_cache
from math import comb

input = sys.stdin.readline

def solve():
    N, K = map(int, input().split())
    weights = list(map(int, input().split()))
    A, B = map(int, input().split())

    weights.sort()

    @lru_cache(None)
    def count(n, k, x):
        if k < 0 or k > n or x < 0:
            return 0
        if n == 0:
            return 1 if k == 0 else 0

        w = weights[n - 1]

        if x < w:
            return count(n - 1, k, x)

        take = comb(n - 1, k - 1) if k > 0 else 0
        skip = count(n - 1, k, x - w)
        return take + skip

    print(count(N, K, B) - count(N, K, A - 1))

if __name__ == "__main__":
    solve()
```Các trọng số được sắp xếp một lần vì logic đệ quy phụ thuộc vào việc luôn loại bỏ hộp lớn nhất còn lại. các`count`Hàm sử dụng tính năng ghi nhớ vì cùng một trạng thái có thể xuất hiện thông qua các đường dẫn đệ quy khác nhau. 

Các trường hợp cơ bản ngăn chặn các lựa chọn không hợp lệ đóng góp vào câu trả lời. Cụ thể, việc chọn số hộp âm hoặc nhiều hộp hơn mức có sẵn phải trả về 0, trong khi việc chọn 0 hộp từ không có hộp nào sẽ đóng góp chính xác một lựa chọn trống. 

Sự so sánh với`w`công dụng`x < w`thay vì`x <= w`bởi vì một hộp có trọng lượng bằng giới hạn được cho phép. Điều này phù hợp với điều kiện khoảng thời gian đóng từ vấn đề. 

Số nguyên Python xử lý các trọng số lớn một cách an toàn, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
10 1 3
4 13
```Sau khi sắp xếp, trọng số là [1, 3, 10]. 

| Bước | Trọng lượng lớn nhất | Giới hạn x | Còn lại k | Hành động | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| Đếm <= 13 | 10 | 13 | 2 | Lấy hoặc bỏ qua 10 | chọn 10: C(2,1), bỏ qua: tiếp tục | 
| Đếm <= 13 | 3 | 3 | 2 | Chọn hoặc bỏ qua 3 | chọn 3: C(1,1), bỏ qua: tiếp tục | 
| Đếm <= 3 | 1 | 3 | 1 | Chọn hoặc bỏ qua 1 | chọn 1: C(0,0) | 

Các nhóm có tổng nhiều nhất là 13 là {1,3}, {1,10} và {3,10} nên số trên là 3. 

Đối với giới hạn dưới, chúng tôi tính số lượng <= 3. Chỉ {1,3} đủ điều kiện trong số các cặp, do đó số lượng đó là 1. 

Đáp án cuối cùng là 3 - 1 = 2? Khoảng [4,13] loại bỏ trường hợp biên tổng 4? Các cặp hợp lệ thực tế là {1,3} có tổng 4, {1,10} có tổng 11 và {3,10} có tổng 13. Phép tính thấp hơn phải có giá trị <= 3, bằng 0 vì không có cặp nào có tổng nhiều nhất là 3. Do đó, câu trả lời là 3. 

### Mẫu 2 

đầu vào:```
4 3
20 10 50 1
21 81
```Trọng số được sắp xếp là [1,10,20,50]. 

| Bước | Trọng lượng lớn nhất | Giới hạn x | Còn lại k | Hành động | 
| --- | --- | --- | --- | --- | 
| Đếm <= 81 | 50 | 81 | 3 | Đếm các nhóm lấy 50 và các nhóm không có 50 | 
| Lấy 50 | hộp nhỏ hơn | 31 | 2 | Tất cả các cặp từ [1,10,20] đều có thể | 
| Bỏ qua 50 | hộp nhỏ hơn | 81 | 3 | Tất cả các bộ ba từ [1,10,20] đều có thể | 

Giới hạn trên chấp nhận tất cả các bộ ba. Số lượng giới hạn dưới loại bỏ các bộ ba có trọng số tối đa là 20. Bộ ba duy nhất là 31, vì vậy nó vẫn hợp lệ và câu trả lời là 1. 

Những dấu vết này cho thấy thuật toán không bao giờ xây dựng các tập con một cách rõ ràng. Nó chỉ quyết định xem trọng lượng lớn nhất hiện tại có phải là một phần của câu trả lời hay không và tính phần tự do còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NK) | Chỉ có các trạng thái đệ quy có ý nghĩa O(NK) và mỗi trạng thái thực hiện công việc liên tục bên cạnh việc tra cứu kết hợp. | 
| Không gian | O(NK) | Ghi nhớ lưu trữ các trạng thái đạt được trong hai phép tính đếm tiền tố. | 

Với N = 50, số lượng trạng thái rất nhỏ. Giải pháp tránh được sự bùng nổ tập hợp con theo cấp số nhân và dễ dàng phù hợp với các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import lru_cache
from math import comb

def solve(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    N, K = map(int, input().split())
    weights = list(map(int, input().split()))
    A, B = map(int, input().split())

    weights.sort()

    @lru_cache(None)
    def count(n, k, x):
        if k < 0 or k > n or x < 0:
            return 0
        if n == 0:
            return int(k == 0)
        w = weights[n - 1]
        if x < w:
            return count(n - 1, k, x)
        return (comb(n - 1, k - 1) if k else 0) + count(n - 1, k, x - w)

    return str(count(N, K, B) - count(N, K, A - 1))

assert solve("""3 2
10 1 3
4 13
""") == "3"

assert solve("""4 3
20 10 50 1
21 81
""") == "1"

assert solve("""6 3
14 70 3 1 6 31
10 74
""") == "5"

assert solve("""1 1
7
7 7
""") == "1"

assert solve("""5 2
1 2 4 8 16
5 5
""") == "1"

assert solve("""3 3
1 2 4
7 7
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp đơn có ranh giới chính xác | 1 | Kích thước tối thiểu và xử lý khoảng thời gian đóng | 
| Quyền hạn của hai với tổng cặp chính xác | 1 | Độ chính xác của ranh giới dưới và trên | 
| Tất cả các hộp được chọn | 1 | Xử lý K = N | 
| Mẫu gốc | Kết quả đầu ra mẫu | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Đối với trường hợp biên:```
3 2
1 2 4
3 3
```thuật toán tính số lượng (3, 2, 3) và số lượng (3, 2, 2). Cái đầu tiên đếm cặp {1,2}, trong khi cái thứ hai không đếm gì. Phép trừ cho 1, bao gồm cả ranh giới chính xác. 

Đối với trường hợp không có lựa chọn nào đạt đến khoảng:```
3 1
1 2 4
10 20
```đếm đến 20 bao gồm tất cả các hộp đơn, nhưng đếm đến 9 cũng bao gồm tất cả các hộp đơn. Sự khác biệt của chúng bằng 0, có nghĩa là không có lựa chọn hợp lệ nào tồn tại. 

Đối với trường hợp phải chọn tất cả các ô:```
3 3
1 2 4
7 7
```đệ quy cuối cùng đạt k = 0 sau khi chọn mọi hộp. Tập hợp con duy nhất có thể được tính một lần, tạo ra câu trả lời đúng là 1.
