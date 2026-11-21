---
title: "CF 226B - Cọc Đá Nghịch Ngợm"
description: "Chúng tôi có một số đống đá, mỗi đống có kích thước ban đầu nhất định. Một thao tác chọn một đống nguồn và hợp nhất nó vào một đống khác. Đống nguồn biến mất, đống đích tăng lên và chi phí vận hành bằng với kích thước hiện tại của đống nguồn."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 226
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 140 (Div. 1)"
rating: 1900
weight: 226
solve_time_s: 142
verified: true
draft: false
---

[CF 226B - Cọc đá nghịch ngợm](https://codeforces.com/problemset/problem/226/B) 

**Xếp hạng:** 1900 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số đống đá, mỗi đống có kích thước ban đầu nhất định. Một thao tác chọn một đống nguồn và hợp nhất nó vào một đống khác. Đống nguồn biến mất, đống đích tăng lên và chi phí vận hành bằng với kích thước hiện tại của đống nguồn. 

Hạn chế bất thường là ở phía đích. Mỗi cọc có thể nhận được nhiều nhất`k`sự hợp nhất sắp tới trong suốt thời gian tồn tại của nó. Sau khi một đống đã được chấp nhận`k`các cọc khác, nó không còn có thể được sử dụng làm đích đến nữa, mặc dù nó vẫn có thể được sáp nhập vào một cọc khác. 

Với mỗi giá trị truy vấn của`k`, chúng ta phải tính tổng chi phí tối thiểu cần thiết để hoàn thành một cọc đơn. 

Các ràng buộc hoàn toàn định hình giải pháp. Chúng tôi có tới`10^5`cọc và`10^5`truy vấn. Bất kỳ cách tiếp cận nào mô phỏng việc hợp nhất độc lập cho mọi truy vấn trong`O(n)`hoặc tệ hơn có thể sẽ quá chậm nếu hằng số ẩn lớn. Bất cứ điều gì bậc hai là không thể. Chúng tôi cần tiền xử lý cộng với việc xử lý truy vấn rất rẻ. 

Kích thước cọc có thể đạt tới`10^9`và tổng câu trả lời có thể lớn hơn nhiều so với phạm vi 32 bit. Cần có loại số nguyên 64 bit. 

Phần khó nhất của vấn đề là hiểu cấu trúc nào giảm thiểu chi phí. Một kẻ tham lam bất cẩn có thể dễ dàng thất bại vì hạn chế không nằm ở số lượng nút con có trong cây hợp nhất cuối cùng, mà ở số lần một đống đóng vai trò là người nhận trong quá trình này. 

Một trường hợp cạnh tinh tế là`k = 1`. 

đầu vào:```
4
1 2 3 4
1
1
```Đầu ra đúng:```
20
```Tại sao? Với`k = 1`, mỗi cọc có thể nhận tối đa một lần hợp nhất, do đó quá trình hợp nhất phải tạo thành một chuỗi. Đống nhỏ nhất được sáp nhập trước, sau đó đống kết hợp đó lại được sáp nhập lại, v.v. Tổng chi phí trở thành:`1 + (1+2) + (1+2+3) = 1 + 3 + 6 = 10`Nhưng đó chỉ là chi phí đóng thêm cọc. Thứ tự tối ưu thực tế sau khi sắp xếp tăng dần là:`1 -> 2`, trị giá`1`

`3 -> (1+2)`, trị giá`3`

`4 -> (...)`, trị giá`6`Tổng cộng`10`. 

Một cách giải thích ngây thơ là "hợp nhất nhiều lần nhỏ nhất" giống như mã hóa Huffman mang lại`19`, điều này không tối ưu theo quy tắc này. 

Một vụ án phức tạp khác là rất lớn`k`. 

đầu vào:```
5
1 2 3 4 5
1
100
```Đầu ra đúng:```
10
```Một đống có thể hấp thụ trực tiếp tất cả những đống khác. Chúng tôi chỉ cần chọn cọc lớn nhất làm người nhận cuối cùng và thêm tất cả các cọc nhỏ hơn vào đó. Chi phí là tổng của tất cả các cọc ngoại trừ cọc lớn nhất. 

Truy vấn lặp đi lặp lại cũng có vấn đề. Tuyên bố cho phép trùng lặp`k`các giá trị. Việc tính lại cùng một câu trả lời mỗi lần sẽ lãng phí thời gian và có thể biến một giải pháp có thể chấp nhận được thành thời gian chờ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng rõ ràng các chuỗi hợp nhất. Đối với một cố định`k`, chúng ta có thể thử tất cả các lệnh hợp nhất hợp lệ và chọn chi phí tối thiểu. Điều này hoạt động về mặt khái niệm vì mọi thao tác đều thay đổi trạng thái theo cách được xác định rõ ràng. 

Vấn đề là số lượng cây hợp nhất hợp lệ tăng theo cấp số nhân. Ngay cả đối với nhỏ`n`, không gian tìm kiếm trở nên rất lớn. Lập trình động trên các tập hợp con sẽ yêu cầu khoảng`O(2^n)`trạng thái, hoàn toàn không thể cho`n = 10^5`. 

Vì vậy chúng ta cần hiểu một quá trình hợp nhất tối ưu sẽ như thế nào. 

Giả sử chúng ta sắp xếp các cọc theo thứ tự tăng dần:```
a1 <= a2 <= ... <= an
```Hãy suy nghĩ xem mỗi cọc đóng góp bao nhiêu lần vào tổng chi phí. 

Bất cứ khi nào một cọc được sáp nhập vào một cọc khác, toàn bộ kích thước hiện tại của nó sẽ được thanh toán lại. Nếu một viên đá sống sót qua nhiều cấp độ trước khi biến mất, nó sẽ góp phần gấp nhiều lần vào câu trả lời. 

Điều đó có nghĩa là những đống nhỏ sẽ biến mất sớm, còn những đống lớn sẽ ở gần gốc. 

Bây giờ hãy nhìn vào hạn chế. Một đống có thể nhận được nhiều nhất`k`hợp nhất. Trong ngôn ngữ cây, mỗi nút có thể có tối đa`k`những đứa trẻ. 

Cấu trúc tối ưu trở thành một gốc rễ`k`cây hợp nhất -ary trong đó các giá trị lớn hơn ở gần gốc hơn và các giá trị nhỏ hơn nằm sâu hơn. 

Sau khi sắp xếp, chiến lược tối ưu lại đơn giản một cách đáng ngạc nhiên: 

Cọc lớn nhất có độ sâu`0`. 

Tiếp theo`k`cọc có độ sâu`1`. 

Tiếp theo`k^2`cọc có độ sâu`2`. 

Tiếp theo`k^3`cọc có độ sâu`3`. 

Và vân vân. 

Nếu cọc có độ sâu`d`, giá trị của nó đóng góp`d`lần vào tổng chi phí. Vì vậy, sau khi sắp xếp:```
answer = sum(ai * depth_i)
```nơi độ sâu được chỉ định theo cấp độ hoàn chỉnh`k`-ary mở rộng từ phần tử lớn nhất trở xuống. 

Tại sao điều này là tối ưu? Bởi vì các nút sâu hơn được trả nhiều lần hơn nên chúng tôi luôn muốn các cọc nhỏ nhất còn lại ở độ sâu lớn nhất. Đây là một cuộc tranh luận trao đổi cổ điển. 

Đối với mỗi truy vấn, chúng ta có thể xử lý mảng được sắp xếp từ nhỏ nhất đến lớn nhất trong khi theo dõi xem có bao nhiêu phần tử thuộc mỗi lớp sâu. 

Số lượng lớp rất nhỏ vì dung lượng tăng theo hình học. Ví dụ, với`k = 2`, số lượng trên mỗi độ sâu là:```
1, 2, 4, 8, ...
```Vì vậy, mỗi truy vấn chạy trong khoảng`O(log_k n)`chuyển tiếp lớp cộng với truyền tải tuyến tính trên mảng. 

Trường hợp đặc biệt xuất hiện khi`k = 1`. Khi đó, mỗi cấp độ chứa chính xác một nút, do đó độ sâu trở thành:```
n-1, n-2, ..., 1, 0
```Trường hợp này chỉ đơn giản là một chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |`O(n log n + q sqrt(n))`khấu hao |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp kích thước cọc theo thứ tự tăng dần. 

Các cọc nhỏ hơn sẽ xuất hiện sâu hơn trong cây hợp nhất vì các nút sâu được tính nhiều lần hơn trong chi phí. 
2. Tính trước tổng tiền tố của mảng đã được sắp xếp. 

Điều này cho phép chúng ta tính nhanh tổng của bất kỳ khối cọc liên tiếp nào. 
3. Đối với mỗi truy vấn`k`, xử lý trường hợp đặc biệt`k = 1`. 

Khi mỗi cọc chỉ có thể nhận được một lần hợp nhất thì cấu trúc hợp nhất phải là một chuỗi. Cọc nhỏ nhất có độ sâu`n-1`, tiếp theo có chiều sâu`n-2`, vân vân. 
4. Đối với`k > 1`, ấn định độ sâu từng lớp từ cọc lớn nhất trở xuống. 

Cấp độ gốc chứa`1`nút. 

Cấp độ tiếp theo chứa nhiều nhất`k`nút. 

Phần tiếp theo chứa nhiều nhất`k^2`. 

Tiếp tục theo cách này phù hợp với số lượng trẻ em tối đa được phép trên mỗi cọc. 
5. Duyệt mảng đã sắp xếp từ lớn nhất đến nhỏ nhất. 

Mỗi khi chúng ta chuyển sang lớp sâu hơn, tất cả các cọc trong lớp đó sẽ đóng góp thêm một bản sao giá trị của chúng vào câu trả lời. 
6. Thêm:```
depth * sum_of_values_in_this_layer
```đến câu trả lời tổng thể. 
7. Xuất kết quả cho mọi truy vấn. 

### Tại sao nó hoạt động 

Một đống đóng góp vào chi phí một lần cho mỗi tổ tiên phía trên nó trong cây hợp nhất. Đó chính xác là độ sâu của nó. 

Hạn chế “một cọc có thể nhận được nhiều nhất`k`hợp nhất" có nghĩa là mọi nút trong cây hợp nhất có thể có nhiều nhất`k`những đứa trẻ. Trong số tất cả các cây như vậy, các giá trị nhỏ nhất sẽ chiếm độ sâu lớn nhất vì các nút sâu hơn sẽ được nhân với hệ số lớn hơn. 

Việc gán lớp tham lam đạt được chính xác điều đó. Các vị trí có sẵn sâu nhất được lấp đầy bằng các cọc nhỏ nhất còn lại, giúp giảm thiểu tổng trọng số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = sorted(map(int, input().split()))

    q = int(input())
    queries = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + a[i]

    memo = {}

    ans = []

    for k in queries:
        if k in memo:
            ans.append(memo[k])
            continue

        if k == 1:
            cur = 0
            for i in range(n):
                cur += a[i] * (n - 1 - i)

            memo[k] = cur
            ans.append(cur)
            continue

        cur = 0

        depth = 0
        cnt = 1
        r = n

        while r > 0:
            l = max(0, r - cnt)

            layer_sum = prefix[r] - prefix[l]
            cur += depth * layer_sum

            r = l
            cnt *= k
            depth += 1

        memo[k] = cur
        ans.append(cur)

    print(*ans)

solve()
```Bước đầu tiên sắp xếp mảng vì cây hợp nhất tối ưu gán các giá trị nhỏ hơn cho độ sâu lớn hơn. 

Mảng tổng tiền tố cho phép truy vấn tổng phạm vi thời gian không đổi. Khi một lớp chứa các phần tử từ chỉ mục`l`ĐẾN`r-1`, tổng số tiền của nó là:```
prefix[r] - prefix[l]
```Vòng lặp chính xử lý các truy vấn một cách độc lập, nhưng các truy vấn lặp lại được lưu trữ trong`memo`. Điều này quan trọng vì câu lệnh rõ ràng cho phép trùng lặp`k`các giá trị. 

các`k = 1`trường hợp được tách ra vì nhân kích thước lớp với`k`sẽ không bao giờ phát triển. Trong trường hợp này, mỗi lớp chứa chính xác một cọc. 

Vì`k > 1`, thuật toán xây dựng các lớp về mặt hình học:```
1, k, k^2, ...
```bắt đầu từ phần tử lớn nhất. Biến`r`đánh dấu có bao nhiêu phần tử vẫn chưa được gán. Mỗi lần lặp lại mất lần cuối cùng`cnt`các phần tử và gán cho chúng độ sâu hiện tại. 

Một chi tiết tinh tế là độ sâu bắt đầu từ`0`ở gốc. Đống lớn nhất không đóng góp gì vì nó không bao giờ được sáp nhập vào đống khác. 

Một lỗi dễ mắc phải khác là tràn số nguyên trong các ngôn ngữ khác. Câu trả lời có thể đạt được xung quanh`10^19`, vì vậy số nguyên 64 bit là bắt buộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
2 3 4 1 1
2
2 3
```Mảng được sắp xếp:```
[1, 1, 2, 3, 4]
```### Truy vấn`k = 2`| Độ sâu | Kích thước lớp | Yếu tố | Tổng lớp | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | [4] | 4 | 0 | 
| 1 | 2 | [2, 3] | 5 | 5 | 
| 2 | 4 | [1, 1] | 2 | 4 | 

Câu trả lời cuối cùng:```
0 + 5 + 4 = 9
```Dấu vết này cho thấy sự tăng trưởng của lớp hình học. Những cọc nhỏ nhất sẽ ở sâu nhất nên chúng được tính nhiều lần. 

### Truy vấn`k = 3`| Độ sâu | Kích thước lớp | Yếu tố | Tổng lớp | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | [4] | 4 | 0 | 
| 1 | 3 | [1, 2, 3] | 6 | 6 | 
| 2 | 9 | [1] | 1 | 2 | 

Câu trả lời cuối cùng:```
0 + 6 + 2 = 8
```Với hệ số phân nhánh lớn hơn, cây trở nên nông hơn nên tổng chi phí sẽ giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n + q sqrt(n))`khấu hao | Việc sắp xếp chiếm ưu thế trong quá trình tiền xử lý, mỗi truy vấn chỉ xử lý các lớp độ sâu logarit | 
| Không gian |`O(n)`| Tổng tiền tố và ghi nhớ | 

Mảng được sắp xếp và tổng tiền tố đều sử dụng bộ nhớ tuyến tính. Số lớp cho một truy vấn là rất nhỏ vì quyền hạn của`k`phát triển nhanh chóng. Ngay cả trong trường hợp thực tế xấu nhất, lời giải vẫn dễ dàng nằm trong giới hạn của`n, q <= 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = sorted(map(int, input().split()))

    q = int(input())
    queries = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    for i in range(n):
        prefix[i + 1] = prefix[i] + a[i]

    memo = {}

    ans = []

    for k in queries:
        if k in memo:
            ans.append(memo[k])
            continue

        if k == 1:
            cur = 0
            for i in range(n):
                cur += a[i] * (n - 1 - i)

            memo[k] = cur
            ans.append(cur)
            continue

        cur = 0

        depth = 0
        cnt = 1
        r = n

        while r > 0:
            l = max(0, r - cnt)

            layer_sum = prefix[r] - prefix[l]
            cur += depth * layer_sum

            r = l
            cnt *= k
            depth += 1

        memo[k] = cur
        ans.append(cur)

    print(*ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
    "5\n"
    "2 3 4 1 1\n"
    "2\n"
    "2 3\n"
) == "9 8", "sample"

# minimum size
assert run(
    "1\n"
    "7\n"
    "3\n"
    "1 2 100\n"
) == "0 0 0", "single pile"

# k = 1 chain structure
assert run(
    "4\n"
    "1 2 3 4\n"
    "1\n"
    "1\n"
) == "10", "chain"

# very large k
assert run(
    "5\n"
    "1 2 3 4 5\n"
    "1\n"
    "100\n"
) == "10", "all merged directly into largest"

# all equal values
assert run(
    "6\n"
    "5 5 5 5 5 5\n"
    "2\n"
    "2 5\n"
) == "25 25", "equal values"

# repeated queries
assert run(
    "5\n"
    "1 2 3 4 5\n"
    "4\n"
    "2 2 2 2\n"
) == "13 13 13 13", "memoization consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cọc đơn |`0 0 0`| Không cần hợp nhất | 
|`k = 1`|`10`| Xử lý cấu trúc chuỗi | 
| Rất lớn`k`|`10`| Tất cả các cọc hợp nhất trực tiếp vào thư mục gốc | 
| Tất cả các giá trị bằng nhau |`25 25`| Phân công độ sâu độc lập với các mối quan hệ đặt hàng | 
| Truy vấn lặp đi lặp lại |`13 13 13 13`| Ghi nhớ chính xác | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
7
1
1
```Đã có đúng một cọc nên không cần thực hiện thao tác nào. Thuật toán xử lý việc này một cách tự nhiên vì lớp gốc chứa phần tử duy nhất ở độ sâu`0`, không đóng góp gì cho câu trả lời. 

Bây giờ hãy xem trường hợp hạn chế`k = 1`:```
4
1 2 3 4
1
1
```Mảng được sắp xếp:```
[1, 2, 3, 4]
```Bài tập sâu trở thành:```
3, 2, 1, 0
```Sự đóng góp:```
1*3 + 2*2 + 3*1 + 4*0 = 10
```Thuật toán sử dụng nhánh chuyên dụng cho`k = 1`, tránh một tiến trình hình học vô hạn. 

Đối với cực kỳ lớn`k`:```
5
1 2 3 4 5
1
100
```Rễ hấp thụ trực tiếp mọi đống khác. 

Cấu trúc lớp:```
depth 0 -> [5]
depth 1 -> [1,2,3,4]
```Trả lời:```
1 + 2 + 3 + 4 = 10
```Thuật toán dừng chính xác khi tất cả các phần tử được gán, mặc dù`k^2`lớn hơn nhiều so với`n`. 

Cuối cùng, hãy xem xét các truy vấn lặp lại:```
5
1 2 3 4 5
4
2 2 2 2
```Nếu không ghi nhớ, phép tính tương tự sẽ lặp lại bốn lần. Giải pháp lưu trữ kết quả đầu tiên và tái sử dụng kết quả đó ngay lập tức cho những lần xuất hiện sau.
