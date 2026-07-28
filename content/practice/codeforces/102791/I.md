---
title: "CF 102791I - Đảo ngược chuỗi"
description: "Nhiệm vụ là tìm số lần hoán đổi liền kề tối thiểu cần thiết để biến một chuỗi đã cho thành chuỗi đảo ngược của nó. Việc hoán đổi chỉ có thể trao đổi hai ký tự lân cận, do đó chi phí đo lường khoảng cách các ký tự phải di chuyển trong chuỗi."
date: "2026-07-27T18:15:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102791
codeforces_index: "I"
codeforces_contest_name: "ICPC 2020-2021 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102791
solve_time_s: 67
verified: true
draft: false
---

[CF 102791I - Đảo ngược chuỗi](https://codeforces.com/problemset/problem/102791/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm số lần hoán đổi liền kề tối thiểu cần thiết để biến một chuỗi đã cho thành chuỗi đảo ngược của nó. Việc hoán đổi chỉ có thể trao đổi hai ký tự lân cận, do đó chi phí đo lường khoảng cách các ký tự phải di chuyển trong chuỗi. 

Đầu vào chứa độ dài của chuỗi và chính chuỗi đó. Đầu ra là số lượng hoán đổi lân cận nhỏ nhất cần thiết để sắp xếp lại chuỗi gốc sao cho ký tự ở mọi vị trí khớp với ký tự ở vị trí được phản chiếu trước khi đảo ngược. 

Độ dài có thể lớn tới 200000. Giải pháp cố gắng mô phỏng trực tiếp các giao dịch hoán đổi là không thể vì một chuỗi có thể yêu cầu số chuyển động bậc hai. Mặc dù câu trả lời cuối cùng có thể vào khoảng n2 nhưng số lượng thao tác được thực hiện bởi thuật toán vẫn cần phải gần với tuyến tính. Điều này loại trừ việc liên tục tìm kiếm các ký tự hoặc từng ký tự chuyển động vật lý. 

Khó khăn chính đến từ các ký tự lặp đi lặp lại. Một ký tự không có đích duy nhất khi đảo ngược chuỗi. Ví dụ, trong`aaaza`, ba`a`các ký tự ở giữa không thể phân biệt được và việc chọn sai lần xuất hiện có thể làm tăng số lần hoán đổi được tính. 

Hãy xem xét đầu vào:```
5
aaaza
```Đầu ra đúng là:```
2
```Một giải pháp bất cẩn có thể phù hợp với giải pháp đầu tiên`a`từ phía bên trái với cái đầu tiên`a`từ mục tiêu bị đảo ngược và bỏ qua thứ tự các ký tự bằng nhau. Điều đó có thể đếm những chuyển động không cần thiết vì các ký tự giống hệt nhau nên được ghép theo thứ tự để giảm thiểu việc vượt qua. 

Một trường hợp quan trọng khác là bảng màu:```
6
cbaabc
```Đầu ra là:```
0
```Một giải pháp chỉ kiểm tra xem các ký tự có khác với vị trí được phản chiếu của chúng hay không và sau đó di chuyển tất cả các điểm không khớp có thể bị tính quá mức. Chuỗi đã bằng đảo ngược của nó. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là cố định nhiều lần hai đầu của sợi dây. Chúng tôi so sánh các ký tự đầu tiên và cuối cùng còn lại. Nếu chúng bằng nhau thì cả hai đều đã ở đúng vị trí cuối cùng. Nếu chúng khác nhau, chúng tôi tìm kiếm ký tự phù hợp ở một bên và di chuyển ký tự đó bằng cách sử dụng các hoán đổi liền kề cho đến khi hai đầu trở nên chính xác. Phương pháp này đúng vì mỗi lần hoán đổi liền kề đại diện cho một đơn vị chuyển động và nhân vật được chọn bằng cách nào đó phải di chuyển đến đích. 

Vấn đề là số lượng chuyển động. Trong trường hợp xấu nhất, việc di chuyển một ký tự từ đầu này của chuỗi có độ dài n sang đầu kia sẽ tốn O(n) và chúng ta có thể phải thực hiện việc này cho các vị trí O(n). Tổng công việc trở thành O(n2), quá chậm đối với n = 200000. 

Quan sát chính là thao tác đảo ngược chỉ thay đổi vị trí chứ không thay đổi số ký tự. Mỗi ký tự trong chuỗi gốc đều có sự xuất hiện trùng khớp trong chuỗi đích bị đảo ngược. Thay vì mô phỏng các lần hoán đổi vật lý, chúng ta có thể tính toán tổng khoảng cách mà các nhân vật phải di chuyển. 

Nếu chúng ta tạo ra chuỗi vị trí mà các ký tự chiếm giữ trong cách sắp xếp mục tiêu thì câu trả lời sẽ trở thành số lần đảo ngược trong chuỗi đó. Đảo ngược đại diện cho hai ký tự xuất hiện sai thứ tự và phải vượt qua các hoán đổi liền kề. Đây là lý do tương tự như việc sắp xếp hợp nhất đếm các cặp đảo ngược. 

Thử thách còn lại là xử lý các ký tự giống nhau. Chúng ta cần biết lần xuất hiện nào của nhân vật sẽ di chuyển đến vị trí mục tiêu nào. Sự lựa chọn tối ưu là ghép các lần xuất hiện từ trái sang phải. Đối với mỗi ký tự, lần xuất hiện đầu tiên của nó trong chuỗi gốc phải khớp với lần xuất hiện đầu tiên trong chuỗi đảo ngược, lần xuất hiện thứ hai của nó phải khớp với lần xuất hiện thứ hai, v.v. Điều này tránh sự giao thoa không cần thiết giữa các ký tự giống hệt nhau. 

Sau khi xây dựng các vị trí mục tiêu, cây Fenwick có thể đếm xem có bao nhiêu vị trí đã được xử lý sau vị trí hiện tại. Điều này đưa ra số lượng đảo ngược trong O (n log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ vị trí của từng ký tự trong chuỗi gốc. Các vị trí được giữ theo thứ tự tăng dần vì các ký tự bằng nhau phải được khớp từ trái sang phải. 
2. Nhìn vào chuỗi đảo ngược. Đối với mỗi ký tự theo thứ tự đảo ngược này, hãy lấy lần xuất hiện sớm nhất không được sử dụng của ký tự đó từ chuỗi gốc. Ghi lại vị trí ban đầu đó trong một mảng mới. 

Mảng kết quả mô tả các ký tự của chuỗi đảo ngược đến từ đâu trong chuỗi gốc. Nếu các vị trí này đã tăng lên thì chuỗi đã ở đúng thứ tự. Mỗi lần giảm giữa các vị trí đều thể hiện các ký tự cần vượt qua. 
3. Đếm số lần đảo ngược trong mảng vị trí bằng cây Fenwick. 

Khi xử lý một vị trí x, số vị trí trước đó lớn hơn x là số ký tự phải vượt qua ký tự này. Cây Fenwick lưu trữ bao nhiêu vị trí đã xuất hiện cho đến nay, cho phép truy vấn này theo thời gian logarit. 
4. Thêm tất cả các đóng góp đảo ngược và in kết quả. 

Tại sao nó hoạt động: 

Chuỗi đảo ngược cuối cùng có thứ tự ký tự cố định. Bằng cách khớp các ký tự giống nhau theo thứ tự từ trái sang phải ban đầu, chúng tôi chọn cách sắp xếp duy nhất tránh được sự hoán đổi không cần thiết giữa các chữ cái giống hệt nhau. Vấn đề còn lại chỉ là thứ tự của những lần xuất hiện trùng khớp này. Mỗi cặp lần xuất hiện xuất hiện theo thứ tự ngược lại với mục tiêu phải vượt qua một lần và mỗi lần hoán đổi liền kề sẽ khắc phục chính xác một lần giao nhau như vậy. Do đó, số lần hoán đổi tối thiểu chính xác là số lần đảo ngược của các vị trí được ánh xạ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0
        while i > 0:
            res += self.bit[i]
            i -= i & -i
        return res

def solve():
    n = int(input())
    s = input().strip()

    pos = [[] for _ in range(26)]
    for i, c in enumerate(s):
        pos[ord(c) - 97].append(i)

    used = [0] * 26
    order = []

    for c in reversed(s):
        x = ord(c) - 97
        order.append(pos[x][used[x]])
        used[x] += 1

    fw = Fenwick(n)
    ans = 0

    for i, x in enumerate(order):
        x += 1
        already = fw.sum(n) - fw.sum(x)
        ans += already
        fw.add(x, 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách vị trí lưu trữ mọi lần xuất hiện của mỗi chữ cái. Bởi vì các danh sách được sắp xếp một cách tự nhiên trong khi quét chuỗi gốc nên việc sắp xếp các vị trí theo thứ tự sẽ mang lại sự ghép nối chính xác giữa các ký tự bằng nhau. 

Việc xây dựng`order`quét ngược chuỗi cuối cùng mong muốn, là chuỗi gốc. Mỗi ký tự nhận được vị trí ban đầu có sẵn tiếp theo của cùng ký tự đó. các`used`mảng ngăn chặn sự xuất hiện tương tự được chỉ định hai lần. 

Cây Fenwick sử dụng chỉ mục dựa trên một, vì vậy mỗi vị trí được lưu trữ sẽ tăng thêm một vị trí trước khi chèn. Đối với vị trí hiện tại, biểu thức`fw.sum(n) - fw.sum(x)`đếm các vị trí trước đó lớn hơn nó. Đây chính xác là những ký tự trước đó nằm sai phía và phải vượt qua nó. 

Câu trả lời được lưu trữ dưới dạng số nguyên của Python, điều này cần thiết vì số lần hoán đổi tối đa có thể vào khoảng n2, lớn hơn nhiều so với giới hạn số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
aaaza
```các vị trí được ánh xạ là: 

| Bước | Ký tự được đặt trong chuỗi đảo ngược | Vị trí ban đầu được chọn | Đã thêm đảo ngược | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 0 | 
| 2 | z | 3 | 0 | 0 | 
| 3 | một | 2 | 1 | 1 | 
| 4 | một | 1 | 1 | 2 | 
| 5 | một | 4 | 0 | 2 | 

Câu trả lời là 2. Dấu vết cho thấy tại sao các ký tự lặp lại phải được ghép nối cẩn thận. Bình đẳng`a`các ký tự được gán theo thứ tự và chỉ`z`chuyển động góp phần hoán đổi. 

Đối với đầu vào:```
9
icpcsguru
```các vị trí được ánh xạ là: 

| Bước | Ký tự được đặt trong chuỗi đảo ngược | Vị trí ban đầu được chọn | Đã thêm đảo ngược | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | bạn | 8 | 0 | 0 | 
| 2 | r | 7 | 1 | 1 | 
| 3 | bạn | 5 | 2 | 3 | 
| 4 | g | 6 | 1 | 4 | 
| 5 | s | 4 | 4 | 8 | 
| 6 | c | 3 | 5 | 13 | 
| 7 | p | 2 | 4 | 17 | 
| 8 | c | 1 | 6 | 23 | 
| 9 | tôi | 0 | 7 | 30 | 

Số cuối cùng là 30. Mỗi lần đảo ngược tương ứng với một cặp ký tự có thứ tự tương đối phải thay đổi trong quá trình đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi ký tự được chèn vào và truy vấn từ cây Fenwick một lần. | 
| Không gian | O(n) | Danh sách sự kiện, vị trí được ánh xạ và cây Fenwick đều lưu trữ thông tin tuyến tính. | 

Các ràng buộc cho phép O(n log n) vì n là 200000. Giải pháp này tránh được hành vi bậc hai khi thực hiện hoán đổi một cách rõ ràng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            r = 0
            while i:
                r += self.bit[i]
                i -= i & -i
            return r

    n = int(input())
    s = input().strip()

    pos = [[] for _ in range(26)]
    for i, c in enumerate(s):
        pos[ord(c) - 97].append(i)

    used = [0] * 26
    arr = []
    for c in reversed(s):
        x = ord(c) - 97
        arr.append(pos[x][used[x]])
        used[x] += 1

    fw = Fenwick(n)
    ans = 0
    for x in arr:
        x += 1
        ans += fw.sum(n) - fw.sum(x)
        fw.add(x, 1)

    return str(ans)

assert solve_case("5\naaaza\n") == "2"
assert solve_case("6\ncbaabc\n") == "0"
assert solve_case("9\nicpcsguru\n") == "30"

assert solve_case("2\naa\n") == "0"
assert solve_case("3\nabc\n") == "3"
assert solve_case("7\naaaaaaa\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 aa`|`0`| Độ dài tối thiểu và trường hợp đã đảo ngược | 
|`3 abc`|`3`| Nhân vật đều cần phải di chuyển | 
|`7 aaaaaaa`|`0`| Tất cả các ký tự bằng nhau | 
|`5 aaaza`|`2`| Các ký tự lặp đi lặp lại với một chuyển động không tầm thường | 

## Vỏ cạnh 

cho`aaaza`, thuật toán xây dựng thứ tự mục tiêu đảo ngược như`azaaa`. Những lần xuất hiện của`a`được khớp từ trái sang phải, tạo ra chuỗi vị trí`[0, 3, 2, 1, 4]`. Cây Fenwick đếm ba lần vượt qua do`z`đặt sai chỗ, dẫn đến câu trả lời đúng là 2 sau khi chỉ đếm những đảo ngược cần thiết. 

Vì`cbaabc`, chuỗi đảo ngược giống hệt chuỗi ban đầu. Các vị trí được ánh xạ đã được sắp xếp nên số lần đảo ngược vẫn bằng 0. Thuật toán xử lý các palindrome một cách tự nhiên vì không có cặp ký tự nào cần phải giao nhau. 

Đối với một chuỗi chỉ chứa một ký tự lặp lại, chẳng hạn như`aaaaa`, mọi lần xuất hiện có thể được so khớp với lần xuất hiện tương ứng mà không thay đổi thứ tự. Vị trí được ánh xạ ngày càng tăng nên câu trả lời là 0. Điều này tránh được lỗi phổ biến khi tính số lần hoán đổi giữa các ký tự giống hệt nhau nhưng thực sự có thể hoán đổi cho nhau. 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản bài xã luận theo phong cách Codeforces ngắn hơn để phù hợp với độ dài và giọng điệu của các bài xã luận chính thức của cuộc thi.
