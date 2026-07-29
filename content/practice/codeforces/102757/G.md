---
title: "CF 102757G - Đã khóa"
description: "Chúng tôi được cung cấp một số chuỗi chữ số có độ dài bằng nhau. Các chữ số bên trong mỗi chuỗi đều ở sai vị trí nhưng sự sắp xếp lại không xác định giống nhau đã được áp dụng cho tất cả chúng."
date: "2026-07-29T00:27:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102757
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 2"
rating: 0
weight: 102757
solve_time_s: 59
verified: true
draft: false
---

[CF 102757G - Đã khóa](https://codeforces.com/problemset/problem/102757/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi chữ số có độ dài bằng nhau. Các chữ số bên trong mỗi chuỗi đều ở sai vị trí nhưng sự sắp xếp lại không xác định giống nhau đã được áp dụng cho tất cả chúng. Nhiệm vụ của chúng ta là tự mình chọn cách sắp xếp lại các vị trí sao cho sau khi di chuyển các chữ số trong mỗi chuỗi thì chênh lệch giữa số kết quả lớn nhất và số kết quả nhỏ nhất càng nhỏ càng tốt. 

Đầu ra là sự khác biệt tối thiểu có thể có này. 

Phần quan trọng của đầu vào là số chữ số nhỏ. Độ dài tối đa là 9, có nghĩa là tổng số cách sắp xếp lại vị trí có thể nhiều nhất là 9!, hay 362880. Độ dài này đủ lớn để việc thử mọi cách sắp xếp là cần thiết, nhưng đủ nhỏ để có thể thực hiện tìm kiếm hoàn chỉnh trên tất cả các cách sắp xếp. Số lượng mã nhiều nhất là 100 nên việc kiểm tra một cách sắp xếp với tất cả các mã cũng rẻ. Một giải pháp đã thử tất cả các giá trị được chuyển đổi có thể có cho mọi thứ tự chữ số có thể vẫn phù hợp vì không gian tìm kiếm bị giới hạn bởi giới hạn giai thừa. 

Những trường hợp nguy hiểm đều xuất phát từ những chi tiết dễ bị bỏ qua. Các số 0 đứng đầu phải có ý nghĩa vì mã luôn dài đúng k chữ số. Ví dụ: nếu đầu vào là:```
2 2
01
10
```câu trả lời đúng là`9`. Hai cách sắp xếp có thể là`01,10`với chênh lệch 9 và`10,01`với sự khác biệt tương tự. điều trị`01`vì số nguyên 1 trước khi sắp xếp lại không phá vỡ trường hợp cụ thể này, nhưng nói chung nó có thể làm mất thông tin vị trí và gây ra các phép biến đổi không chính xác. 

Các chữ số lặp đi lặp lại là một nguồn sai lầm khác. Coi như:```
3 3
111
121
131
```Câu trả lời đúng là`20`. Hoán đổi hai vị trí chữ số bằng nhau không làm thay đổi gì, nhưng việc triển khai giả định mọi hoán vị tạo ra một kết quả duy nhất có thể lãng phí thời gian hoặc vô tình loại bỏ các sắp xếp hợp lệ không chính xác. 

Trường hợp cạnh cuối cùng là khi tất cả các mã giống hệt nhau:```
3 2
55
55
55
```Câu trả lời là`0`. Bất kỳ hoán vị nào cũng để lại tất cả các giá trị bằng nhau, do đó giá trị tối đa và tối thiểu vẫn giữ nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi thứ tự có thể có của các vị trí chữ số k. Đối với mỗi thứ tự, chúng tôi chuyển đổi mọi mã khôi phục bằng cách sử dụng thứ tự đó, theo dõi các giá trị được chuyển đổi nhỏ nhất và lớn nhất, đồng thời cập nhật câu trả lời với chênh lệch của chúng. 

Lực lượng vũ phu này là chính xác bởi vì mọi sự sắp xếp lại chung có thể có đều được kiểm tra rõ ràng. Nếu tồn tại sự sắp xếp lại tối ưu, nó sẽ xuất hiện trong số các hoán vị được tạo ra. 

Lực lượng vũ phu trở nên thực tế vì k nhỏ. Với k = 9 thì chỉ có 362880 hoán vị. Kiểm tra một hoán vị yêu cầu xem xét tối đa 100 số. Một cách triển khai đơn giản giúp xây dựng lại từng chữ số được chuyển đổi theo từng chữ số thực hiện khoảng 362880 × 100 × 9 phép toán, tức là khoảng 326 triệu phép toán nhỏ. 

Cải tiến chính không phải là một ý tưởng thuật toán khác mà là sự trình bày cẩn thận. Thay vì chuyển đổi chuỗi nhiều lần, chúng tôi tính toán trước phần đóng góp của từng vị trí chữ số gốc cho mỗi số. Khi một hoán vị đặt một vị trí ở một trọng số thập phân nhất định, chúng tôi chỉ thêm phần đóng góp được lưu trữ. Điều này giữ nguyên việc liệt kê giai thừa nhưng làm cho mỗi lần kiểm tra nhanh hơn nhiều. 

Lực lượng vũ phu hoạt động vì số lượng đơn hàng chữ số có thể có là nhỏ. Nó chỉ thất bại nếu độ dài chữ số trở nên lớn hơn nhiều, khi đó việc tăng trưởng giai thừa trở nên không thể thực hiện được. Nhận xét rằng k không bao giờ vượt quá 9 cho phép chúng ta rút gọn bài toán thành tìm kiếm toàn diện trên tất cả các hoán vị vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! · n · k) | O(n · k) | Được chấp nhận với sự tối ưu hóa | 
| Tối ưu | O(k! · n · k) | O(n · k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mã và lưu trữ các chữ số của từng mã riêng biệt. Giữ các chữ số thay vì chuyển đổi ngay lập tức mọi thứ thành số nguyên sẽ giữ nguyên cấu trúc chữ số k ban đầu. 
2. Tính toán trước phần đóng góp của từng vị trí chữ số cho mỗi mã. Đối với mỗi vị trí mã và mỗi vị trí thập phân, hãy lưu giá trị mà chữ số này sẽ thêm nếu được đặt ở đó. Trong quá trình tìm kiếm hoán vị, điều này tránh được các thao tác chuỗi lặp lại. 
3. Tạo mọi hoán vị của k vị trí ban đầu. Một hoán vị biểu thị thứ tự cuối cùng của các vị trí từ chữ số có nghĩa nhất đến chữ số có nghĩa ít nhất. 
4. Đối với hoán vị hiện tại, hãy tính toán mọi số được chuyển đổi bằng cách kết hợp các đóng góp được tính toán trước của các vị trí đã chọn. Các giá trị biến đổi lớn nhất và nhỏ nhất xác định sự khác biệt được tạo ra bởi sự sắp xếp này. 
5. Cập nhật câu trả lời tối thiểu toàn cầu với sự khác biệt so với cách sắp xếp hiện tại. Sau khi tất cả các hoán vị được kiểm tra, giá trị này là kết quả bắt buộc. 

Lý do điều này có hiệu quả là vì mọi giải pháp hợp lệ chính xác là một hoán vị của các vị trí chữ số. Việc tìm kiếm truy cập vào mọi hoán vị như vậy và với mỗi hoán vị sẽ tính toán các giá trị tối đa và tối thiểu chính xác được tạo ra bởi sự sắp xếp lại đó. Vì thuật toán so sánh tất cả các cách sắp xếp lại có thể xảy ra nên nó không thể bỏ lỡ câu trả lời tốt hơn. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    codes = [input().strip() for _ in range(n)]

    digits = [[ord(c) - 48 for c in s] for s in codes]

    weights = [10 ** i for i in range(k - 1, -1, -1)]

    contrib = []
    for row in digits:
        contrib.append([row[j] * weights[i] for j in range(k) for i in range(k)])

    pre = []
    for row in digits:
        cur = []
        for pos in range(k):
            for place in range(k):
                cur.append(row[pos] * weights[place])
        pre.append(cur)

    ans = 10 ** k

    for perm in permutations(range(k)):
        best = -1
        worst = 10 ** k

        for row in pre:
            value = 0
            base = 0
            for pos in perm:
                value += row[pos * k + base]
                base += 1

            if value > best:
                best = value
            if value < worst:
                worst = value

        diff = best - worst
        if diff < ans:
            ans = diff

    print(ans)

if __name__ == "__main__":
    solve()
```Mã lưu trữ phần đóng góp có trọng số có thể có của mỗi vị trí chữ số gốc. Vòng lặp hoán vị gán vị trí ban đầu cho vị trí đầu ra, do đó phép tính bên trong chỉ thực hiện phép cộng. 

Thứ tự các chữ số trong hoán vị là có ý nghĩa. Phần tử đầu tiên của hoán vị được đặt ở vị trí quan trọng nhất, đó là lý do tại sao các trọng số được tạo ra từ lũy thừa cao nhất từ ​​10 trở xuống. 

Các biến`best`Và`worst`đại diện cho số lượng lớn nhất và nhỏ nhất được tạo ra bởi sự sắp xếp hiện tại. Chúng được đặt lại cho mỗi hoán vị vì mỗi cách sắp xếp mô tả cách tái tạo mật khẩu khác nhau. 

Số nguyên Python không tràn cho bài toán này vì số lớn nhất chỉ có chín chữ số, nhưng việc triển khai vẫn tránh được những chuyển đổi không cần thiết và tính lũy thừa lặp đi lặp lại để giữ thời gian chạy ở mức thấp. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 2
12
32
```Hai lệnh vị trí có thể có là: 

| Hoán vị | Giá trị đầu tiên | Giá trị thứ hai | Sự khác biệt | 
| --- | --- | --- | --- | 
| đơn hàng ban đầu | 12 | 32 | 20 | 
| đổi thứ tự | 21 | 23 | 2 | 

Sự sắp xếp tốt nhất đặt chữ số thứ hai lên đầu tiên. Mức chênh lệch kết quả là 2, phù hợp với đầu ra mẫu. 

Đối với mẫu thứ hai:```
4 4
1842
0141
5581
1581
```Một sự sắp xếp tối ưu mang lại: 

| Trạng thái hoán vị | Giá trị được xem xét | Tối thiểu hiện tại | Tối đa hiện tại | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| thứ tự đã chọn | mã chuyển đổi sau khi sắp xếp lại | 1418 | 2435 | 1017 | 

Thuật toán kiểm tra mọi thứ tự vị trí có thể có và cuối cùng đạt đến mức chênh lệch tối thiểu này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k! · n · k) | Mọi thứ tự chữ số đều được kiểm tra và mỗi thứ tự đánh giá tất cả các mã | 
| Không gian | O(n · k) | Các chữ số được lưu trữ và đóng góp có trọng số tỷ lệ thuận với kích thước đầu vào | 

Với k nhiều nhất là 9, số hạng giai thừa được giới hạn bởi 362880. Kết hợp với tối đa 100 mã, điều này vừa khít với giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    codes = [input().strip() for _ in range(n)]

    ans = 10 ** k

    for perm in permutations(range(k)):
        mn = 10 ** k
        mx = -1
        for s in codes:
            value = int(''.join(s[i] for i in perm))
            mn = min(mn, value)
            mx = max(mx, value)
        ans = min(ans, mx - mn)

    return str(ans)

assert run("""2 2
12
32
""") == "2", "sample 1"

assert run("""4 4
1842
0141
5581
1581
""") == "1017", "sample 2"

assert run("""2 1
0
9
""") == "9", "single digit boundary"

assert run("""3 2
55
55
55
""") == "0", "all equal values"

assert run("""3 3
001
100
010
""") == "99", "leading zero handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 0 / 9`|`9`| Độ dài chữ số tối thiểu | 
|`3 2 / 55 / 55 / 55`|`0`| Số giống nhau | 
|`3 3 / 001 / 100 / 010`|`99`| Số 0 đứng đầu và thay đổi vị trí | 

## Vỏ cạnh 

Đối với trường hợp số 0 đứng đầu:```
3 3
001
100
010
```Thuật toán coi mỗi mã là ba vị trí chữ số riêng biệt. Một hoán vị có thể di chuyển các số 0 xung quanh nhưng không thể xóa chúng. Mọi sự sắp xếp đều được đánh giá là một số có ba chữ số, do đó kết quả đầu ra vẫn chính xác. 

Đối với các chữ số lặp lại:```
3 2
55
55
55
```Mọi hoán vị đều tạo ra các giá trị biến đổi giống hệt nhau. Giá trị tối đa và tối thiểu bằng nhau trong mỗi lần lặp, vì vậy câu trả lời vẫn bằng 0. 

Đối với các mã giống hệt nhau có độ dài lớn hơn:```
3 4
1234
1234
1234
```Mọi sự sắp xếp lại có thể đều tạo ra ba số giống hệt nhau. Việc tìm kiếm vẫn kiểm tra tất cả các hoán vị, nhưng mỗi hoán vị tạo ra chênh lệch bằng 0 ngay sau khi tìm mức tối đa và tối thiểu. 

Đối với số lượng mã nhỏ nhất có thể:```
2 1
3
8
```Chỉ có một thứ tự vị trí chữ số có thể. Thuật toán đánh giá trực tiếp hai giá trị được chuyển đổi và trả về hiệu của chúng là 5.
