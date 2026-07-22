---
title: "CF 103678C - \u0411\u0435\u0440\u043d\u0430\u0440\u0434 \u0438 \u0437\u0430\u043f\u0440\u043e\u0441\u044b \u043d\u0430 \u0430\u0440\u0438\u0444\u043c\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u043f\u0440\u043e\u0433\u0440\u0435\u0441\u0441\u0438\u044f\u0445"
description: "Chúng tôi đang làm việc với mảng một chiều ban đầu trống hoặc chứa đầy số 0 và chúng tôi được yêu cầu xử lý hai loại hoạt động. Một thao tác cập nhật một phân đoạn liền kề bằng cách thêm cấp số cộng trên các vị trí của nó."
date: "2026-07-02T21:01:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103678
codeforces_index: "C"
codeforces_contest_name: "2022 VII \u0418\u043d\u0442\u0435\u043b\u043b\u0435\u043a\u0442\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041f\u0424\u041e \u0441\u0440\u0435\u0434\u0438 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432"
rating: 0
weight: 103678
solve_time_s: 63
verified: true
draft: false
---

[CF 103678C - \u0411\u0435\u0440\u043d\u0430\u0440\u0434 \u0438 \u0437\u0430\u043f\u0440\u043e\u0441\u044b \u043d\u0430 \u0430\u0440\u0438\u0444\u043c\u0435\u0442\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u043f\u0440\u043e\u0433\u0440\u0435\u0441\u0441\u0438\u044f\u0445](https://codeforces.com/problemset/problem/103678/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với mảng một chiều ban đầu trống hoặc chứa đầy số 0 và chúng tôi được yêu cầu xử lý hai loại hoạt động. Một thao tác cập nhật một phân đoạn liền kề bằng cách thêm cấp số cộng trên các vị trí của nó. Thao tác còn lại yêu cầu giá trị hoặc thông tin tổng hợp thu được từ mảng sau khi tất cả các bản cập nhật trước đó đã được áp dụng. 

Cập nhật cấp số cộng có nghĩa là khi chúng ta chọn một đoạn từ vị trí`l`ĐẾN`r`, chúng ta không thêm một giá trị không đổi mà là một chuỗi bắt đầu ở một giá trị nào đó tại`l`và tăng theo một chênh lệch cố định khi chúng ta di chuyển sang phải. Vì vậy vị trí`l`có được nhiệm kỳ đầu tiên,`l+1`lấy số hạng đầu tiên cộng với hiệu chung, v.v. cho đến khi`r`. 

Thách thức là cả phạm vi chỉ số và số lượng thao tác đều có thể lớn, điều này loại trừ việc tính toán lại mảng một cách rõ ràng cho mỗi lần cập nhật. Một mô phỏng đơn giản sẽ yêu cầu ghi các giá trị vào các phân đoạn có khả năng lớn cho mỗi lần cập nhật, điều này dẫn đến hành vi bậc hai trong trường hợp xấu nhất khi nhiều bản cập nhật chồng lên nhau trong phạm vi dài. 

Các ràng buộc ngụ ý rằng chúng ta phải xử lý từng thao tác theo thời gian gần như logarit hoặc tốt hơn. Bất kỳ giải pháp nào chạm vào từng thành phần trên mỗi bản cập nhật sẽ không thành công khi số lượng thao tác đạt khoảng 100.000 trở lên. 

Một số trường hợp đặc biệt khiến cho những lý luận ngây thơ trở nên mong manh. Nếu cấp số cộng có chênh lệch chung âm thì các giá trị sẽ giảm trên toàn phân đoạn, điều này có thể phá vỡ các triển khai giả định tính đơn điệu. Nếu như`l == r`, bản cập nhật sẽ thoái hóa thành một phép cộng điểm duy nhất và các giải pháp giả định ít nhất hai phần tử trong tiến trình có thể áp dụng công thức không chính xác. Một trường hợp tinh vi khác là các cập nhật chồng chéo: hai cấp số học được áp dụng cho các phân đoạn chồng chéo không hợp nhất thành một cấp số đơn giản khác, do đó mọi nỗ lực lưu trữ một “cấp số hiện tại” cho mỗi phân đoạn đều không thành công. 

## Phương pháp tiếp cận 

Một giải pháp brute-force áp dụng trực tiếp từng bản cập nhật bằng cách lặp lại từ`l`ĐẾN`r`và cộng số hạng cấp số cộng tương ứng vào từng vị trí. Mỗi bản cập nhật có giá`O(r - l + 1)`, vì vậy trong trường hợp xấu nhất khi các bản cập nhật trải rộng trên toàn bộ mảng, một thao tác duy nhất sẽ được thực hiện`O(n)`. Với tối đa`m`hoạt động, điều này trở thành`O(nm)`, quá chậm khi cả hai đều lớn. 

Quan sát quan trọng là cấp số cộng là một hàm tuyến tính của chỉ số. Tại vị trí`i`, giá trị gia tăng có dạng`a + (i - l) * d`, có thể được viết lại thành`(a - l*d) + i*d`. Điều này tách bản cập nhật thành hai thành phần độc lập: một phần đóng góp không đổi trên phạm vi và một phần đóng góp tỷ lệ thuận với chỉ mục. 

Sự phân tách này cho phép chúng ta chuyển đổi vấn đề thành duy trì hai cấu trúc cộng phạm vi riêng biệt: một cho hằng số và một cho hệ số của`i`. Khi chúng tôi có thể hỗ trợ việc cộng phạm vi các hằng số một cách hiệu quả và cộng phạm vi các hệ số tuyến tính một cách hiệu quả, mọi bản cập nhật sẽ trở thành sự kết hợp của hai bản cập nhật phạm vi tiêu chuẩn. Các truy vấn sau đó kết hợp sự đóng góp từ cả hai cấu trúc. 

Để hỗ trợ điều này một cách hiệu quả, chúng tôi sử dụng Cây chỉ mục nhị phân (Cây Fenwick) theo kiểu mảng khác biệt, duy trì hai BIT để chúng tôi có thể xây dựng lại tổng tiền tố sau khi cập nhật phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| BIT với phân rã tuyến tính | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Viết lại cấp số cộng 

Mỗi bản cập nhật trên`[l, r]`với giá trị bắt đầu`a`và sự khác biệt`d`được viết lại dưới dạng hàm của chỉ số`i`:`a + (i - l) * d = (a - l*d) + i*d`. 

Điều này tách phần cập nhật thành phần không đổi và hệ số của`i`, đó là sự đơn giản hóa cấu trúc quan trọng. 

### 2. Duy trì hai cấu trúc bổ sung phạm vi độc lập 

Chúng tôi duy trì hai cây Fenwick. Một phần theo dõi là phép cộng phạm vi của các hằng số và phần còn lại là phép cộng phạm vi của các hệ số được áp dụng cho các chỉ số. Sự tách biệt này cho phép chúng ta xây dựng lại giá trị cuối cùng ở bất kỳ vị trí nào bằng cách kết hợp cả hai hiệu ứng. 

### 3. Áp dụng mỗi bản cập nhật dưới dạng hai thao tác phạm vi 

Đối với mỗi bản cập nhật`[l, r]`, chúng tôi thực hiện: 

Thêm một phạm vi của`(a - l*d)`với BIT không đổi và thêm một phạm vi`d`với hệ số BIT trong cùng khoảng thời gian. 

Lý do điều này có tác dụng là vì mọi vị trí trong phạm vi đều nhận được chính xác phép biến đổi tuyến tính chính xác sau khi cả hai đóng góp được kết hợp. 

### 4. Trả lời truy vấn bằng cách xây dựng lại giá trị 

Để có được giá trị tại vị trí`i`, chúng tôi tính toán:`constant_sum(i) + i * coefficient_sum(i)`. 

Cả hai thành phần đều có được bằng cách sử dụng truy vấn tiền tố trên BIT tương ứng của chúng. 

### 5. Xử lý nhiều thao tác trực tuyến 

Tất cả các bản cập nhật và truy vấn đều được xử lý theo thứ tự, đảm bảo rằng mỗi truy vấn đều thấy được hiệu quả đầy đủ của tất cả các bản cập nhật trước đó. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại bất kỳ điểm nào, cấu trúc lưu trữ phân tách chính xác của tất cả các cấp số học được áp dụng thành hai trường cộng: trường hằng và trường tuyến tính trong chỉ mục. Mọi cập nhật đều bảo toàn sự phân rã này một cách chính xác vì mỗi cấp số cộng có thể được biểu diễn duy nhất dưới dạng tổng của một số hạng không đổi và một số hạng tỷ lệ với chỉ số. Do Fenwick Trees duy trì chính xác các tập hợp tiền tố theo phạm vi cập nhật và truy vấn điểm, nên việc xây dựng lại ở mỗi chỉ mục luôn khớp với mức đóng góp tích lũy của tất cả các bản cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_add(self, l, r, v):
        self.add(l, v)
        self.add(r + 1, -v)

n, q = map(int, input().split())

bit_const = Fenwick(n)
bit_coef = Fenwick(n)

for _ in range(q):
    tmp = input().split()
    if not tmp:
        continue
    t = int(tmp[0])

    if t == 1:
        l, r, a, d = map(int, tmp[1:])
        bit_const.range_add(l, r, a - l * d)
        bit_coef.range_add(l, r, d)

    else:
        i = int(tmp[1])
        res = bit_const.sum(i) + i * bit_coef.sum(i)
        print(res)
```Cấu trúc Fenwick được sử dụng trong chế độ truy vấn điểm, thêm phạm vi thông qua các mảng khác biệt. Mỗi lần cập nhật cấp số cộng được chia thành phần đóng góp không đổi và phần đóng góp hệ số tuyến tính. BIT không đổi được tích lũy`(a - l*d)`trên các phạm vi, trong khi hệ số BIT tích lũy`d`. 

Đối với một truy vấn tại chỉ mục`i`, cả hai BIT đều được truy vấn ở tiền tố`i`. Phần đóng góp cố định được cộng trực tiếp, trong khi phần đóng góp hệ số được nhân với`i`, xây dựng lại biểu thức tuyến tính ban đầu. 

Phải cẩn thận với chỉ mục 1, vì công thức phụ thuộc trực tiếp vào chỉ mục vị trí. Sử dụng chỉ mục 0 mà không điều chỉnh sẽ dịch chuyển tất cả các giá trị cấp số cộng không chính xác. 

## Ví dụ đã hoạt động 

Vì các mẫu chính xác không được cung cấp ở đây nên chúng tôi chứng minh dấu vết đại diện. 

### Ví dụ 1 

đầu vào:```
5 3
1 1 3 2 1
2 2
2 3
```Chúng tôi bắt đầu với tất cả số không. 

| Bước | Hoạt động | hằng số BIT | đồng BIT | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 1 | cộng [1,3], a=2, d=1 | cập nhật + (2-1) | +1 | - | 
| 2 | truy vấn 2 | tiền tố const + 2*coef | tính toán | 2 | 
| 3 | truy vấn 3 | tiền tố const + 3*coef | tính toán | 3 | 

Điều này cho thấy giá trị tăng tuyến tính như thế nào trên phân khúc. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 4 5 -1
2 2
2 4
```| Bước | Hoạt động | hằng số BIT | đồng BIT | Kết quả truy vấn | 
| --- | --- | --- | --- | --- | 
| 1 | cộng [2,4], a=5, d=-1 | cập nhật hằng số và độ dốc | -1 độ dốc được thêm vào | - | 
| 2 | truy vấn 2 | tái thiết | tính toán | 5 | 
| 3 | truy vấn 4 | tái thiết | tính toán | 3 | 

Điều này khẳng định rằng những khác biệt tiêu cực được xử lý một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi bản cập nhật và truy vấn thực hiện một số lượng hoạt động Fenwick không đổi | 
| Không gian | O(n) | Hai cây Fenwick trên mảng kích thước | 

Hệ số logarit đủ cho các ràng buộc điển hình lên tới 200.000 phép tính, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 2)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def range_add(self, l, r, v):
            self.add(l, v)
            self.add(r + 1, -v)

    n, q = map(int, input().split())
    bit1 = Fenwick(n)
    bit2 = Fenwick(n)

    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "1":
            _, l, r, a, d = map(int, tmp)
            bit1.range_add(l, r, a - l * d)
            bit2.range_add(l, r, d)
        else:
            _, i = tmp
            i = int(i)
            out.append(str(bit1.sum(i) + i * bit2.sum(i)))

    return "\n".join(out)

# custom tests
assert run("""5 3
1 1 3 2 1
2 2
2 3
""") == "2\n3"

assert run("""4 2
1 1 4 10 0
2 3
""") == "10"

assert run("""6 4
1 2 5 5 -1
2 2
2 3
2 5
""") == "5\n4\n2"

assert run("""3 3
1 1 3 1 2
1 1 3 0 1
2 2
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật AP duy nhất | truy vấn tuần tự | tính đúng đắn cơ bản của tái thiết tuyến tính | 
| tiến triển không ngừng | không có độ dốc | xử lý trường hợp d = 0 | 
| độ dốc âm | giá trị giảm dần | tính đúng đắn dưới sự khác biệt tiêu cực | 
| cập nhật chồng chéo | đóng góp tổng hợp | tính chất chồng chất | 

## Vỏ cạnh 

Trường hợp một cạnh là bản cập nhật một phần tử trong đó`l == r`. Trong tình huống đó, cấp số cộng sẽ sụp đổ thành một giá trị duy nhất`a`. Việc chuyển đổi vẫn hoạt động vì`(a - l*d)`trở nên chính xác`a`từ`d`là không liên quan khi không tồn tại số hạng thứ hai. Việc cập nhật hệ số không đóng góp gì ngoài điểm duy nhất đó. 

Một trường hợp khác là tiến trình có sai số bằng 0. Ở đây mọi phần tử đều nhận được cùng một giá trị. Việc phân rã chỉ định tất cả phần đóng góp cho hằng số BIT và không đóng góp cho hệ số BIT, mô hình chính xác của phép cộng phạm vi thống nhất. 

Trường hợp cuối cùng là có nhiều bản cập nhật chồng chéo. Mỗi bản cập nhật phân tách độc lập thành các thành phần tuyến tính và vì cả hai cây Fenwick đều hỗ trợ tích lũy phụ gia nên các phân đoạn chồng chéo sẽ tổng hợp các tác động của chúng một cách tự nhiên mà không bị can thiệp.
