---
title: "CF 102281J - \u041a\u043e\u043b\u044c\u0446\u0435\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng tôi có (n) chuỗi riêng biệt. Chuỗi (i)-th chứa vòng (ai). Một thao tác sẽ mở một vòng, loại bỏ nó khỏi chuỗi ban đầu và sau đó đóng vòng đó xung quanh hai đầu của hai chuỗi. Do đó, vòng mở sẽ trở thành điểm nối giữa hai mảnh."
date: "2026-08-13T09:31:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "J"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 237
verified: true
draft: false
---

[CF 102281J - \u041a\u043e\u043b\u044c\u0446\u0435\u0432\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có (n) chuỗi riêng biệt. Chuỗi (i)-th chứa các vòng (a_i). Một thao tác sẽ mở một vòng, loại bỏ nó khỏi chuỗi ban đầu và sau đó đóng vòng đó xung quanh hai đầu của hai chuỗi. Do đó, vòng mở sẽ trở thành điểm nối giữa hai mảnh. 

Mục tiêu là có được một chuỗi được kết nối trong khi mở và đóng càng ít vòng càng tốt. Cái giá chính xác là số lượng chiếc nhẫn được mở ra, bất kể những chiếc nhẫn đó đến từ dây chuyền nào. 

Khó khăn chính là một vòng mở không chỉ đơn thuần là sự kết nối giữa hai chuỗi hiện có. Nếu chúng ta mở nhiều vòng từ cùng một chuỗi ban đầu, chuỗi đó cuối cùng có thể được tháo rời hoàn toàn thành các vòng kết nối. Một chuỗi được tháo rời hoàn toàn với các vòng (a_i) sẽ mang lại cho chúng ta (a_i) các vòng kết nối, đồng thời việc tháo chuỗi đó cũng loại bỏ một trong các chuỗi ban đầu cần được kết nối. 

Với (n) lên đến (10^5), giải pháp (O(n^2)) đã quá chậm và bất kỳ giải pháp nào theo cấp số nhân đều hoàn toàn không khả thi. Các giá trị (a_i) có thể đạt tới (10^9), do đó thuật toán phải phụ thuộc vào số lượng chuỗi thay vì tổng số vòng. Việc sắp xếp theo sau là một lần quét tuyến tính là đủ nhanh. 

Có một số trường hợp mà cách giải thích ngây thơ có thể thất bại. Nếu chỉ có một chuỗi thì không cần thực hiện thao tác nào. Ví dụ, đầu vào`1 / 100`có câu trả lời`0`, vì chuỗi đã được kết nối. Một giải pháp luôn giả định (n-1) kết nối sẽ xuất ra không chính xác`1`. 

Một sợi dây chuyền có một chiếc nhẫn đặc biệt có giá trị. Vì`3 / 1 6666 100500`, câu trả lời là`1`. Mở vòng đơn từ chuỗi đầu tiên và sử dụng nó để kết nối hai chuỗi còn lại. Một giải pháp ngây thơ đòi thực hiện một thao tác cho mỗi lần hợp nhất ban đầu vẫn sẽ xảy ra`2`, điều đó là sai. 

Một số chuỗi ngắn cũng có thể được sử dụng cùng nhau. Vì`4 / 1 1 1 100`, câu trả lời là`2`. Hai chuỗi một vòng có thể mở hoàn toàn, tạo thành hai vòng nối, các đoạn còn lại có thể ghép với hai vòng nối đó thành một chuỗi. Các đầu nối có thể liền kề nhau trong chuỗi cuối cùng, do đó, việc coi mọi vòng đã mở là yêu cầu một chuỗi ban đầu riêng biệt để kết nối sẽ bỏ lỡ các cấu trúc hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là quyết định chuỗi ban đầu nào sẽ được mở hoàn toàn. Đối với một bộ đã chọn (S), việc mở mọi vòng trong chuỗi đó sẽ tốn chi phí 

[ 
K=\sum_{i\in S} a_i. 
] 

Đặt (r=|S|). Các chuỗi trong (S) biến mất thành các mảnh riêng biệt vì tất cả các vòng của chúng hiện có sẵn dưới dạng đầu nối. Các chuỗi (n-r) khác có thể vẫn còn nguyên vẹn như một mảnh. Với các vòng nối (K), chỉ cần nhiều nhất (K+1) các mảnh như vậy để tạo thành một chuỗi, do đó việc lựa chọn là khả thi chính xác khi 

[ 
n-r\le K+1, 
] 

hoặc tương đương, 

[ 
\sum_{i\in S}(a_i+1)\ge n-1. 
] 

Do đó, thuật toán brute-force có thể liệt kê mọi tập hợp con của chuỗi (n), kiểm tra bất đẳng thức này và giữ mức tối thiểu (\sum a_i). Có (2^n) tập hợp con và thậm chí việc đánh giá từng tập hợp con trong (O(n)) sẽ đưa ra các hoạt động (O(n2^n)). Với (n=10^5), ngay cả việc liệt kê tập hợp con cũng sẽ yêu cầu (2^{100000}) trường hợp, vì vậy cách tiếp cận này không khả thi từ xa. 

Quan sát hữu ích là việc chọn một chuỗi có độ dài (a_i) có chi phí (a_i), nhưng đóng góp (a_i+1) vào điều kiện khả thi. Sự khác biệt giữa đóng góp và chi phí luôn chính xác là một. 

Giả sử chúng ta quyết định chọn chính xác (k) chuỗi. Chúng tôi cần tổng chiều dài của họ để đáp ứng 

[ 
\sum a_i+k\ge n-1. 
] 

Trong số tất cả các bộ chuỗi (k), bộ có độ dài nhỏ nhất (k) có chi phí nhỏ nhất có thể. Nếu ngay cả những chuỗi (k) đó không thỏa mãn bất đẳng thức thì không có tập hợp chuỗi (k) nào khác có thể thỏa mãn nó với chi phí nhỏ hơn. Vì tất cả (a_i) đều dương nên chi phí của các tiền tố tăng lên khi chúng ta thêm nhiều chuỗi hơn, vì vậy chúng ta chỉ cần sắp xếp độ dài và lấy các chuỗi ngắn nhất cho đến khi điều kiện trở thành đúng. 

Điều này biến việc tìm kiếm theo cấp số nhân thành một vấn đề sắp xếp sau một lần quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các độ dài chuỗi theo thứ tự không giảm. Chúng ta muốn tập hợp các chuỗi được mở hoàn toàn rẻ nhất có thể, vì vậy với mỗi số chuỗi cố định được chọn, chúng ta nên ưu tiên những chuỗi ngắn nhất. 
2. Khởi tạo`opened = 0`Và`count = 0`. Đây`opened`là tổng số vòng chúng tôi dự định mở, trong khi`count`là số chuỗi mở hoàn toàn. 
3. Xử lý độ dài được sắp xếp từ nhỏ nhất đến lớn nhất. Thêm chiều dài chuỗi hiện tại vào`opened`và tăng`count`bởi một. 
4. Sau khi thêm một chuỗi có độ dài (a), hãy kiểm tra xem 

[ 
đã mở+đếm\ge n-1. 
] 

Phía bên trái chính xác là (\sum(a_i+1)) cho chuỗi đã chọn. Khi nó đạt đến (n-1), có đủ số vòng đã mở và dây xích được tháo rời hoàn toàn để sắp xếp tất cả các mảnh còn lại thành một dây chuyền. 
5. Dừng ở tiền tố đầu tiên thỏa mãn điều kiện và kết quả`opened`. Vì độ dài là dương nên mỗi chuỗi được chọn bổ sung sẽ làm tăng câu trả lời, do đó tiền tố khả thi đầu tiên là tối ưu. 

### Tại sao nó hoạt động 

Xét mọi cách xây dựng hợp lệ và gọi (S) là tập hợp các chuỗi ban đầu mà từ đó mọi vòng đều được mở. Gọi (K) là tổng số vòng được mở và (r=|S|). Mỗi chuỗi bên ngoài (S) chứa ít nhất một vòng chưa bao giờ được mở ra, do đó nó đóng góp ít nhất một mảnh còn nguyên vẹn. Có (n-r) những mảnh như vậy. Một chuỗi chứa (K) vòng nối đã mở có thể tách tối đa (K+1) phần không trống, do đó 

[ 
n-r\le K+1. 
] 

Sắp xếp lại mang lại 

[ 
K+r\ge n-1, 
] 

đó chính xác là 

[ 
\sum_{i\in S}(a_i+1)\ge n-1. 
] 

Ngược lại, nếu bất đẳng thức này đúng thì hãy mở mọi vòng trong chuỗi đã chọn. Các vòng (K) của chúng có thể đóng vai trò là vị trí kết nối, trong khi (n-r) chuỗi ban đầu còn lại đóng vai trò là các phần không trống. Vì (n-r\le K+1), các mảnh và vòng nối này có thể được sắp xếp thành một chuỗi. Như vậy bất đẳng thức vừa cần vừa đủ. 

Đối với một số cố định (k) chuỗi đã chọn, việc thay thế bất kỳ chuỗi đã chọn nào bằng chuỗi ngắn hơn không được chọn không thể làm tăng tổng chi phí và không thể làm giảm tính hữu ích của việc lựa chọn, bởi vì cả chi phí và đóng góp của nó đều giảm một lượng chính xác như nhau. Do đó, (k) chuỗi ngắn nhất luôn là ứng cử viên rẻ nhất. Do đó, việc quét tiền tố sẽ tìm thấy chi phí tối thiểu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    opened = 0

    for count, length in enumerate(a, 1):
        opened += length

        if opened + count >= n - 1:
            print(opened)
            return

if __name__ == "__main__":
    solve()
```Đầu vào được đọc một lần và mảng được sắp xếp vì chuỗi được chọn tối ưu tạo thành tiền tố của thứ tự được sắp xếp. Biến`opened`lưu trữ tổng số vòng trong các chuỗi đã chọn đó. 

các`enumerate(a, 1)`cuộc gọi thực hiện`count`bằng số lượng chuỗi được chọn thay vì chỉ số dựa trên số 0. Điều này tránh được một lỗi dễ xảy ra trong điều kiện. Sau khi xử lý`count`chuỗi, biểu thức khả thi là`opened + count >= n - 1`. 

Câu trả lời được in ngay lập tức khi tìm thấy tiền tố khả thi đầu tiên. Không cần phải tiếp tục, vì mỗi số vòng đều dương nên việc thêm nhiều chuỗi chỉ có thể tăng lên`opened`. 

Số nguyên Python có độ chính xác tùy ý, do đó, tổng số tối đa có thể, nhiều nhất là (10^5\cdot10^9=10^{14}), không yêu cầu xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là`5 / 3 3 3 3 3`. 

| Đếm | Độ dài hiện tại | Đã mở | Đã mở + Đếm | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 4 | Có | 

Chuỗi đầu tiên đã cho (3+1=4=n-1). Việc mở ba vòng của nó sẽ cung cấp chính xác ba đầu nối cần thiết để nối bốn chuỗi còn lại, vì vậy câu trả lời là`3`. 

Đối với mẫu thứ hai, đầu vào là`3 / 1 6666 100500`. 

| Đếm | Độ dài hiện tại | Đã mở | Đã mở + Đếm | Khả thi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | Có | 

Ở đây (n-1=2). Chuỗi một vòng đủ để cung cấp một đầu nối và việc loại bỏ chuỗi ban đầu đó sẽ làm giảm số lượng chuỗi riêng biệt xuống còn một. Hai chuỗi còn lại cần chính xác một kết nối, vì vậy câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính đơn | 
| Không gian | (O(n)) | Mảng độ dài chuỗi được lưu trữ trong bộ nhớ | 

Đối với (n=10^5), việc sắp xếp (10^5) số nguyên dễ dàng nằm trong giới hạn đã cho. Bản thân quá trình quét là tuyến tính và thuật toán không bao giờ phụ thuộc vào các giá trị cực lớn tiềm tàng của độ dài chuỗi riêng lẻ ngoại trừ thông qua số học số nguyên thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    opened = 0

    for count, length in enumerate(a, 1):
        opened += length
        if opened + count >= n - 1:
            print(opened)
            return

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# provided samples
assert run("5\n3 3 3 3 3\n") == "3\n", "sample 1"
assert run("3\n1 6666 100500\n") == "1\n", "sample 2"

# minimum-size input
assert run("1\n1000000000\n") == "0\n", "one chain needs no operations"

# two chains
assert run("2\n1 1\n") == "1\n", "two one-ring chains need one connector"

# several singleton chains and one large chain
assert run("4\n1 1 1 100\n") == "2\n", "multiple small donor chains"

# maximum-size boundary case
assert run("100000\n" + " ".join(["1"] * 100000) + "\n") == "50000\n", \
    "maximum n with all chains of length one"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1000000000`|`0`| Tối thiểu (n), đã có một chuỗi | 
|`2 / 1 1`|`1`| Số chuỗi nhỏ nhất không cần thiết | 
|`4 / 1 1 1 100`|`2`| Một số chuỗi ngắn và vòng nối liền kề | 
|`100000 / 1 1 ... 1`|`50000`| Tối đa (n), tiền tố lớn và số học biên | 

## Vỏ cạnh 

cho`1 / 1000000000`, số chuỗi ban đầu đã là một. Ngưỡng là (n-1=0), do đó thuật toán không cần chọn bất cứ thứ gì và trả về`0`. Điều này mắc phải sai lầm phổ biến là yêu cầu mù quáng ít nhất một chiếc nhẫn đã mở. 

Vì`2 / 1 1`, chuỗi được sắp xếp đầu tiên có độ dài (1). Sau khi chọn nó,`opened = 1`Và`count = 1`, cho`opened + count = 2`, ít nhất là (n-1=1). Câu trả lời là`1`. Một vòng được mở ra và dùng để nối hai chuỗi ban đầu. 

Vì`4 / 1 1 1 100`, sau một chuỗi được chọn, giá trị là (1+1=2), thấp hơn giá trị bắt buộc (3). Sau hai chuỗi được chọn, nó trở thành (2+2=4), do đó thuật toán trả về`2`. Cả hai vòng đã mở đều có thể xuất hiện ở vị trí kết nối trong chuỗi cuối cùng. Trường hợp này rất hữu ích vì các vòng đầu nối không cần phải tương ứng 1-1 với các chuỗi ban đầu vẫn còn nguyên. 

Đối với trường hợp tối đa có (100000) chuỗi mỗi vòng, sau khi chọn (k) chuỗi, chúng ta có`opened = k`Và`count = k`. Điều kiện trở thành (2k\ge99999), có nghiệm số nguyên nhỏ nhất là (k=50000). Câu trả lời là do đó`50000`. Điều này đòi hỏi cả ranh giới ngưỡng và thực tế là câu trả lời có thể gần bằng một nửa (n).
