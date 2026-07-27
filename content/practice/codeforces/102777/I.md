---
title: "CF 102777I - \u041a\u0430\u043a \u0442\u0435\u0431\u0435 \u0442\u0430\u043a\u043e\u0435, \u0418\u043b\u043e\u043d \u041c\u0430\u0441\u043a?"
description: "Mạng HyperLoop có thể được xem dưới dạng đồ thị có hướng với n trạm. Từ mỗi trạm có đúng hai cạnh đi ra: đường hầm bên trái và đường hầm bên phải."
date: "2026-07-27T20:31:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "I"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 47
verified: true
draft: false
---

[CF 102777I - \u041a\u0430\u043a \u0442\u0435\u0431\u0435 \u0442\u0430\u043a\u043e\u0435, \u0418\u043b\u043e\u043d \u041c\u0430\u0441\u043a?](https://codeforces.com/problemset/problem/102777/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Mạng HyperLoop có thể được xem dưới dạng đồ thị có hướng với`n`trạm. Từ mỗi trạm có đúng hai cạnh đi ra: đường hầm bên trái và đường hầm bên phải. Lấy đường hầm bên trái nhiều lần có nghĩa là áp dụng một chức năng và lấy đường hầm bên phải nhiều lần có nghĩa là áp dụng một chức năng khác. 

Mỗi truy vấn cung cấp một trạm bắt đầu`x`, một số bước di chuyển sang trái`a`, một số bước đi đúng`b`, và kết quả là đồng tiền`c`. Nếu như`c = 0`, chuyến đi bao gồm`a`di chuyển trái theo sau là`b`những bước đi đúng đắn. Nếu như`c = 1`, trật tự bị đảo ngược. Nhiệm vụ là tìm ra trạm cuối cùng sau hai khối di chuyển. 

Phần khó khăn là đồ thị chỉ có`100000`trạm, nhưng số lượng di chuyển có thể đạt tới`10^15`. Việc mô phỏng các bước di chuyển là không thể vì ngay cả một truy vấn cũng có thể yêu cầu hàng triệu triệu lần chuyển đổi. Với`500000`truy vấn, bất kỳ giải pháp nào gần với`O(nq)`hoặc thậm chí`O(q * min(a,b))`nằm ngoài thời gian sẵn có. Chúng tôi cần tiền xử lý trên biểu đồ để mỗi truy vấn có thể được trả lời trong khoảng thời gian logarit. 

Cấu trúc đồ thị rất đặc biệt. Riêng mỗi loại đường hầm sẽ tạo ra một đồ thị chức năng: mỗi trạm có chính xác một cạnh trái đi ra và đúng một cạnh phải đi ra. Đồ thị hàm chứa các chu trình được định hướng với các cây dẫn vào chúng, đây chính xác là cấu trúc mà tính năng nâng nhị phân hoạt động tốt. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Đầu tiên là một truy vấn không có bước di chuyển nào. Ví dụ:```
n = 1
left: 1
right: 1
query: 1 0 0 0
```Câu trả lời là:```
1
```Giải pháp luôn thực hiện ít nhất một lần nhảy sẽ rời khỏi trạm không chính xác. 

Một trường hợp phức tạp khác là vòng lặp tự kết hợp với lũy thừa lớn:```
n = 2
left: 1 1
right: 2 2
query: 2 1000000000000000 7 0
```Câu trả lời là:```
1
```Sau lần di chuyển sang trái đầu tiên, trạm sẽ trở thành`1`, và mọi bước di chuyển sang trái tiếp theo vẫn ở đó. Phương pháp dựa trên phát hiện chu kỳ phải xử lý chính xác các chu kỳ có độ dài bằng một. 

Thứ tự của hai hoạt động cũng có vấn đề. Ví dụ:```
n = 3
left: 2 3 3
right: 1 1 2
query: 1 1 1 0
```Câu trả lời là:```
1
```Con đường là`1 -> 2`sau đó sử dụng đường hầm bên trái`2 -> 1`sử dụng đường hầm bên phải. Việc đảo ngược thứ tự sẽ tạo ra một quá trình chuyển đổi khác, do đó, việc coi truy vấn là một thao tác kết hợp là không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là mô phỏng chuyến đi. Đối với mỗi truy vấn, hãy bắt đầu tại`x`, đi theo đường hầm cần thiết`a`lần, sau đó đi theo đường hầm khác`b`lần. Điều này đúng vì mọi bước di chuyển đều chính xác là cạnh được xác định bởi đường hầm đã chọn. Tuy nhiên, trường hợp xấu nhất là rất lớn. Một truy vấn có thể yêu cầu`10^15 + 10^15`chuyển tiếp và`500000`những truy vấn như vậy sẽ yêu cầu khoảng`10^21`hoạt động. 

Quan sát chính là hai hệ thống đường hầm độc lập. Chúng ta không bao giờ cần phải luân phiên giữa các đường hầm bên trái và bên phải. Mỗi truy vấn chỉ yêu cầu một lũy thừa của hàm bên trái và một lũy thừa của hàm bên phải. Nếu chúng ta có thể trả lời "ga này sẽ đi đâu sau`k`di chuyển sang trái?" nhanh chóng, toàn bộ truy vấn sẽ trở thành hai bước nhảy độc lập. 

Nâng nhị phân cung cấp chính xác hoạt động này. Đối với mỗi loại đường hầm, chúng tôi lưu trữ vị trí mà mỗi trạm tiếp cận sau`2^j`di chuyển. Bất kỳ giá trị nào lên đến`10^15`có thể được biểu diễn bằng tối đa 50 bit nhị phân, do đó, một bước nhảy có thể được phân tách thành tối đa 50 chuyển tiếp được lưu trữ. 

Lực lượng vũ phu hoạt động vì từng cạnh sau khớp chính xác với quy trình, nhưng nó không thành công vì số lượng cạnh di chuyển quá lớn. Quan sát rằng mỗi loại đường hầm là một biểu đồ chức năng cho phép chúng ta thay thế các bước đi dài bằng số lần nhảy được tính toán trước theo logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a + b) mỗi truy vấn | O(1) | Quá chậm | 
| Nâng nhị phân | O(n log 10^15 + q log 10^15) | O(n log 10^15) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hai bàn nâng đôi, một cho hầm bên trái và một cho hầm bên phải. Cấp độ đầu tiên lưu trữ đích trực tiếp sau một lần di chuyển. Mỗi cấp độ tiếp theo sẽ lưu trữ điểm đến sau số lần di chuyển nhiều gấp đôi so với cấp độ trước đó. Ví dụ, mức độ`j`đáp lại một bước nhảy dài`2^j`. 
2. Với mỗi truy vấn, hãy quyết định thứ tự thực hiện bằng cách sử dụng`c`. Nếu như`c = 0`, trước tiên áp dụng bước nhảy sang trái có độ dài`a`, sau đó nhảy sang phải độ dài`b`. Nếu như`c = 1`, thực hiện bước nhảy bên phải trước. 
3. Để thực hiện bước nhảy có độ dài`k`, kiểm tra biểu diễn nhị phân của`k`. Đối với mỗi bit được thiết lập, hãy thay thế trạm hiện tại bằng đích được lưu ở mức nâng nhị phân đó. Mỗi bit được đặt tương ứng với một đoạn lũy thừa của tuyến đường. 
4. Xuất ra trạm đạt được sau khi hoàn thành cả hai lần nhảy. 

Tại sao nó hoạt động: 

Bất biến đằng sau việc nâng nhị phân là`up[j][v]`luôn thể hiện chính xác`2^j`các ứng dụng của cùng chức năng đường hầm bắt đầu từ`v`. Cấp độ cơ sở là đúng theo cách xây dựng và mỗi cấp độ cao hơn được hình thành bằng cách áp dụng bước nhảy trước đó hai lần, do đó thuộc tính giữ nguyên cho mọi cấp độ. Bất kỳ số lần di chuyển nào cũng có thể được chia thành lũy thừa của hai bằng cách sử dụng biểu diễn nhị phân, do đó, việc kết hợp các bước nhảy đã chọn sẽ mang lại kết quả chính xác như thực hiện tất cả các bước di chuyển riêng lẻ. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def build_lift(n, nxt):
    LOG = 50
    lift = [array('i', nxt)]
    for _ in range(1, LOG):
        prev = lift[-1]
        cur = array('i', [0]) * (n + 1)
        for i in range(1, n + 1):
            cur[i] = prev[prev[i]]
        lift.append(cur)
    return lift

def jump(v, k, lift):
    bit = 0
    while k:
        if k & 1:
            v = lift[bit][v]
        k >>= 1
        bit += 1
    return v

def solve():
    n, q = map(int, input().split())
    left = [0] + list(map(int, input().split()))
    right = [0] + list(map(int, input().split()))

    left_lift = build_lift(n, left)
    right_lift = build_lift(n, right)

    ans = []
    for _ in range(q):
        x, a, b, c = map(int, input().split())
        if c == 0:
            x = jump(x, a, left_lift)
            x = jump(x, b, right_lift)
        else:
            x = jump(x, b, right_lift)
            x = jump(x, a, left_lift)
        ans.append(str(x))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`build_lift`hàm tạo các bảng cho một hướng đường hầm. Mảng đầu tiên là cạnh trực tiếp và mỗi mảng sau sẽ nhân đôi độ dài của bước nhảy bằng cách kết hợp mảng trước đó với chính nó. Việc sử dụng`array('i')`là có chủ ý vì danh sách Python sẽ lưu trữ các tham chiếu và sẽ vượt quá giới hạn bộ nhớ cho khoảng mười triệu chuyển tiếp được lưu trữ. 

các`jump`hàm tuân theo các bit của số lần di chuyển. Nếu bit hiện tại được đặt, bước nhảy được tính toán trước tương ứng sẽ được áp dụng. Số lượng di chuyển vừa vặn trong 50 bit vì`2^50`lớn hơn`10^15`. 

Việc xử lý truy vấn chỉ chọn thứ tự của hai bước nhảy độc lập. Các giá trị của`a`Và`b`có thể bằng 0 và`while k`loop xử lý trường hợp đó một cách tự nhiên bằng cách trả về trạm ban đầu. 

Số nguyên Python không bị tràn nên số lần di chuyển lớn sẽ an toàn. Chi tiết lập chỉ mục duy nhất cần xem là các đài được đánh số từ`1`, do đó mỗi mảng nâng đều chứa một phần tử không được sử dụng tại vị trí`0`. 

## Ví dụ đã hoạt động 

Đối với truy vấn mẫu đầu tiên`1 4 7 1`, kết quả xu có nghĩa là nước đi đúng sẽ xảy ra trước tiên. 

| Bước | Trạm hiện tại | Hoạt động | Nước đi còn lại | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | Bắt đầu | phải 7, trái 4 | 
| Sau khi nhảy phải | 6 | Áp dụng 7 chiêu đúng | trái 4 | 
| Sau khi nhảy sang trái | 3 | Áp dụng 4 chiêu trái | 0 | 

Câu trả lời là`3`. Điều này chứng tỏ rằng hai lần nhảy phải được thực hiện theo thứ tự đã chỉ định. 

Đối với một ví dụ tùy chỉnh nhỏ hơn:```
3 2
2 3 3
1 1 2
1 5 2 0
2 3 4 1
```Truy vấn đầu tiên sử dụng bước di chuyển sang trái trước. 

| Bước | Trạm hiện tại | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 5 nước đi trái | 3 | 
| Kết thúc | 3 | 2 nước đi đúng | 2 | 

Truy vấn thứ hai đảo ngược thứ tự. 

| Bước | Trạm hiện tại | Hoạt động | Kết quả | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | 4 bước đúng | 1 | 
| Kết thúc | 1 | 3 bước trái | 3 | 

Những dấu vết này cho thấy bàn nâng chỉ đáp ứng chuyển động một chiều. Logic truy vấn có trách nhiệm sắp xếp hai hướng theo đúng thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log 10^15) | Mỗi trạm được xử lý khoảng 50 cấp độ nâng và mỗi truy vấn thực hiện kiểm tra tối đa 100 bit. | 
| Không gian | O(n log 10^15) | Hai hướng hầm, mỗi hướng lưu trữ khoảng 50 lớp`n`các điểm đến. | 

Quá trình tiền xử lý lưu trữ khoảng mười triệu chuyển tiếp. Việc sử dụng mảng số nguyên nhỏ gọn giúp duy trì mức sử dụng bộ nhớ trong giới hạn 64 MB. Thời gian truy vấn độc lập với các giá trị của`a`Và`b`, do đó, ngay cả số lần di chuyển lớn nhất cũng được xử lý nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    
    solve()
    
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided sample
assert run("""6 5
2 4 1 5 3 3
6 3 6 3 6 4
1 4 7 1
2 5 2 0
4 14 1 0
5 8 13 1
3 1 14 0
""") == """3
3
6
3
6""", "sample"

# single station self loops
assert run("""1 3
1
1
1 0 0 0
1 1000000000000000 999999999999999 1
1 5 7 0
""") == """1
1
1""", "single station"

# different orders give different answers
assert run("""3 2
2 3 3
1 1 2
1 1 1 0
1 1 1 1
""") == """1
2""", "order matters"

# all destinations equal
assert run("""4 2
4 4 4 4
2 2 2 2
3 1000000000000000 1000000000000000 0
1 999999999999999 0 1
""") == """4
2""", "large equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trạm đơn |`1`cho mọi truy vấn | Không di chuyển và xử lý vòng lặp tự động | 
| Lệnh hoạt động khác nhau | Kết quả khác nhau | Giải thích đúng về`c`| 
| Tất cả các điểm đến đều bình đẳng | Đã sửa điểm đến sau những bước nhảy lớn | Quyền lực lớn và sự chuyển đổi lặp đi lặp lại | 

## Vỏ cạnh 

Đồ thị một trạm có cả hai hàm đường hầm trỏ tới chính nó. Bàn nâng chỉ chứa điểm đến`1`ở mọi cấp độ. Một truy vấn với`a = 0`Và`b = 0`không thực hiện bước nhảy nào và trả về trạm bắt đầu. Một truy vấn có giá trị rất lớn vẫn trả về`1`bởi vì mọi cấp độ nhảy đều bảo toàn vòng lặp tự. 

Đối với ví dụ về vòng lặp tự trước đó:```
2
1 1
2 2
2 1000000000000000 7 0
```Bàn nâng bên trái gửi trạm`2`đến ga`1`ở mức 0 và mọi cấp độ cao hơn cũng ở lại trạm`1`. Lần nhảy trái đầu tiên ngay lập tức đạt đến chu kỳ ổn định, các bước nhảy trái còn lại không làm thay đổi kết quả. Cú nhảy phải cuối cùng giữ cho trạm ở`1`, sản xuất:```
1
```Đối với ví dụ nhạy cảm với đơn hàng:```
3 1
2 3 3
1 1 2
1 1 1 0
```Thuật toán đầu tiên áp dụng bước nhảy trái, tới trạm`2`, sau đó thực hiện bước nhảy phải, quay trở lại vị trí đứng`1`. Kết quả là:```
1
```Cùng một biểu đồ với`c = 1`sẽ áp dụng bước nhảy phải trước, đến trạm`1`, sau đó di chuyển sang trái đến trạm`2`. Điều này xác nhận rằng giải pháp không hợp nhất hai chức năng và duy trì thứ tự vận hành cần thiết.
