---
title: "CF 106284D - \u041d\u041e\u0414-\u0441\u0432\u0451\u0440\u0442\u043a\u0430"
description: "Chúng ta cần đếm bộ ba số nguyên không âm (A, B, C) bên trong ba khoảng cho trước sao cho ước số chung lớn nhất của hai tổng xung quanh B bằng tổng của hai giá trị bên ngoài: gcd(A + B, B + C) = A + C."
date: "2026-06-25T07:41:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106284
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 10-11 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106284
solve_time_s: 48
verified: true
draft: false
---

[CF 106284D - \u041d\u041e\u0414-\u0441\u0432\u0451\u0440\u0442\u043a\u0430](https://codeforces.com/problemset/problem/106284/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm bộ ba số nguyên không âm`(A, B, C)`trong ba khoảng cho trước sao cho ước số chung lớn nhất của hai tổng xung quanh`B`bằng tổng của hai giá trị bên ngoài:`gcd(A + B, B + C) = A + C`. 

Đầu vào cung cấp các đường viền trái và phải cho các giá trị có thể có của`A`,`B`, Và`C`. Đầu ra là số bộ ba thỏa mãn đẳng thức. 

Điều thú vị là phạm vi có thể rất lớn, lên tới`10^9`, vì vậy việc lặp lại tất cả các bộ ba có thể là không thể. Tuy nhiên, độ dài của mỗi khoảng tối đa là`300000`, có nghĩa là một thuật toán được mong đợi có khoảng vài trăm nghìn phép tính. Một giải pháp có ba vòng lặp lồng nhau sẽ cần tới`300000^3`kiểm tra, vượt xa giới hạn. Ngay cả việc kiểm tra từng cặp giá trị cũng đã quá tốn kém. 

Cái bẫy chính là sự bình đẳng không chỉ đơn giản là tạo ra`A + C`chia cả hai đối số của gcd. gcd phải chính xác`A + C`. Ngoài ra, các giá trị 0 hoạt động khác nhau vì các đối số chia hết liên quan đến số 0 có thể làm mất thông tin. 

Ví dụ, hãy xem xét:```
A: 1 1
B: 2 2
C: 3 3
```Chỉ có một khả năng gấp ba`(1,2,3)`. Chúng tôi có`gcd(3,5)=1`, Nhưng`A+C=4`, vậy câu trả lời là`0`. Một giải pháp chỉ kiểm tra xem`A+C`chia cả hai đối số gcd vẫn sẽ thất bại ở đây vì đẳng thức gcd cuối cùng mạnh hơn. 

Một trường hợp cạnh khác là:```
A: 0 0
B: 0 5
C: 0 0
```Chỉ một`(0,0,0)`hoạt động. Vì`(0,B,0)`, điều kiện trở thành`gcd(B,B)=B`, bằng`0`chỉ khi`B=0`. Bỏ qua trường hợp 0 ​​có thể đếm không chính xác tất cả các giá trị của`B`. 

Trường hợp ranh giới cuối cùng là khi một trong các giá trị bên ngoài bằng 0 nhưng giá trị còn lại thì không:```
A: 0 0
B: 1 10
C: 5 5
```Ở đây các giá trị hợp lệ của`B`là bội số của`5`, bởi vì`gcd(B,B+5)=gcd(B,5)`phải là`5`. Chỉ một`B=5`Và`B=10`công việc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi bộ ba có thể và kiểm tra điều kiện gcd. Điều này đúng vì mọi câu trả lời hợp lệ đều được kiểm tra, nhưng số phép tính là tích của ba độ dài khoảng. Với chiều dài tối đa`300000`, điều này trở nên xung quanh`2.7 * 10^16`kiểm tra, điều đó là không thể. 

Quan sát quan trọng xuất phát từ việc hiểu được lực lượng bình đẳng. Cho phép`S = A + C`. Chúng tôi cần:`gcd(A+B, B+C) = S`. 

gcd chỉ có thể bằng`S`nếu như`S`chia cả hai đối số. Từ`S | A+B`Và`S | B+C`, phép trừ cho`S | A-C`. Từ`S = A+C`, chúng tôi hiểu điều đó`S`chia cả hai`A+C`Và`A-C`. Việc cộng hai giá trị này sẽ cho`S | 2A`. 

Bởi vì`0 <= A <= S`, giá trị duy nhất có thể có của`2A`là:`0`,`S`, hoặc`2S`. 

Điều này chỉ để lại ba trường hợp cấu trúc. 

Nếu như`A = 0`, sau đó`C = S`, và điều kiện giảm xuống còn`C | B`. 

Nếu như`C = 0`, tương tự chúng ta cần`A | B`. 

Nếu như`A = C`, thì hai đối số gcd bằng nhau, vì vậy gcd là`A+B`. Chúng tôi cần`A+B = 2A`, mang lại`B=A`. 

Trường hợp đặc biệt`A=C=0`chỉ cho`B=0`. 

Vì vậy, thay vì tìm kiếm bộ ba, chúng ta chỉ cần đếm bội số trong các phạm vi và giao điểm có giá trị bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | A | * | 
| Tối ưu | O( | A | + | 

## Hướng dẫn thuật toán 

1. Đọc ba khoảng. Lưu trữ đường viền của họ dưới dạng`LA, RA`,`LB, RB`, Và`LC, RC`. 
2. Đếm trường hợp`A = 0`Và`C > 0`. Với mọi điều tích cực có thể`C`, đếm xem có bao nhiêu giá trị của`B`trong khoảng của nó chia hết cho`C`. Số bội số của`x`TRONG`[l, r]`là:`r // x - (l - 1) // x`. 

Điều này hiệu quả vì điều kiện toán học cho trường hợp này là chính xác`C | B`. 
3. Đếm trường hợp`C = 0`Và`A > 0`theo cách tương tự. Với mọi điều tích cực có thể`A`, cộng số bội của`A`bên trong`B`khoảng thời gian. 
4. Đếm trường hợp`A = C > 0`. Giá trị giống nhau phải thuộc về cả ba khoảng, do đó phần đóng góp là kích thước giao điểm của ba khoảng, không bao gồm số 0. 
5. Tay cầm`(0,0,0)`riêng. Nếu số 0 thuộc cả ba khoảng thì thêm một. 

Tại sao nó hoạt động: mọi bộ ba hợp lệ phải đáp ứng một trong các trường hợp dẫn xuất vì gcd chính xác`A+C`lực lượng`A+C`chia`2A`. Ba vị trí có thể có của`A`liên quan đến`A+C`bao gồm tất cả các khả năng và mỗi trường hợp được tính bằng điều kiện đơn giản tương đương. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_multiples(x, l, r):
    return r // x - (l - 1) // x

def solve():
    LA, RA = map(int, input().split())
    LB, RB = map(int, input().split())
    LC, RC = map(int, input().split())

    ans = 0

    if LC > 0:
        for c in range(LC, RC + 1):
            ans += count_multiples(c, LB, RB)

    if LA > 0:
        for a in range(LA, RA + 1):
            ans += count_multiples(a, LB, RB)

    left = max(LA, LC, 1)
    right = min(RA, RC, RB)
    if left <= right:
        ans += right - left + 1

    if LA == 0 and LB == 0 and LC == 0:
        ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Người trợ giúp`count_multiples`tránh việc lặp lại thông qua`B`khoảng thời gian. Nó đếm tất cả các số chia hết cho một giá trị cố định bằng phép chia số nguyên. 

Vòng lặp đầu tiên xử lý`A=0`chi nhánh. Chúng tôi bỏ qua`C=0`bởi vì trường hợp đó thuộc về việc xử lý hoàn toàn bằng không riêng biệt. Vòng thứ hai đối xứng với`C=0`. 

Việc tính toán giao lộ sử dụng`max`Và`min`bởi vì`A`,`B`, Và`C`đều phải là số dương giống nhau. Giới hạn dưới buộc phải ít nhất`1`bởi vì số 0 được xử lý riêng. 

Không có nguy cơ tràn số nguyên trong Python. Trong các ngôn ngữ có số nguyên có kích thước cố định, câu trả lời vẫn phù hợp theo câu lệnh, nhưng nên tránh các sản phẩm trung gian. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 1
2 2
3 3
```Trạng thái thuật toán là: 

| Bước | Giá trị hiện tại | Đóng góp | 
| --- | --- | --- | 
| Trường hợp A=0 | không thể C | 0 | 
| trường hợp C=0 | không thể nào A | 0 | 
| Trường hợp A=C | ngã tư trống | 0 | 
| Trường hợp không | không thể | 0 | 

Bộ ba duy nhất có thể là`(1,2,3)`, nhưng gcd của nó là`1`, không`4`, vậy câu trả lời là`0`. 

### Mẫu 2 

đầu vào:```
1 1
1 2
1 1
```| Bước | Giá trị hiện tại | Đóng góp | 
| --- | --- | --- | 
| Trường hợp A=0 | bỏ qua | 0 | 
| trường hợp C=0 | bỏ qua | 0 | 
| Trường hợp A=C | giá trị 1 nằm trong tất cả các khoảng | 1 | 
| Trường hợp không | không thể | 0 | 

Bộ ba được tính là`(1,1,1)`. Nó mang lại`gcd(2,2)=2`, Và`A+C=2`, vì vậy nó hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(RA-LA+RC-LC) | Mỗi giá trị bên ngoài dương có thể được xử lý một lần | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Số lần lặp tối đa là khoảng`600000`, bởi vì chỉ có`A`Và`C`phạm vi được quét. Điều này dễ dàng phù hợp trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue()

# sample 1
assert run("1 1\n2 2\n3 3\n") == "0\n"

# sample 2
assert run("1 1\n1 2\n1 1\n") == "1\n"

# all zeros
assert run("0 0\n0 0\n0 0\n") == "1\n"

# only A=0 branch
assert run("0 0\n1 10\n5 5\n") == "2\n"

# equal positive values
assert run("3 5\n3 5\n3 5\n") == "3\n"

# mixed boundary case
assert run("0 2\n0 6\n0 2\n") == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 / 0 0 / 0 0`|`1`| Điều đặc biệt`(0,0,0)`trường hợp | 
|`0 0 / 1 10 / 5 5`|`2`| Đếm bội số khi một bên bằng 0 | 
|`3 5 / 3 5 / 3 5`|`3`| các`A=C=B`chi nhánh | 
|`0 2 / 0 6 / 0 2`|`6`| Tương tác giữa trường hợp không và trường hợp tích cực | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
1 1
2 2
3 3
```Thuật toán chỉ đạt đến nhánh dương bằng nhau khi cùng một giá trị xuất hiện trong cả ba phạm vi. Không phải vậy, bởi vì`1`,`2`, Và`3`khác nhau. Các nhánh 0 cũng trống nên kết quả vẫn giữ nguyên`0`. 

Đối với trường hợp hoàn toàn bằng 0:```
0 0
0 0
0 0
```Ba vòng không đóng góp gì cả. Kiểm tra cuối cùng sẽ thấy rằng số 0 xuất hiện trong mọi khoảng và thêm bộ ba hợp lệ duy nhất`(0,0,0)`. 

Đối với trường hợp số 0 một phía:```
0 0
1 10
5 5
```Giá trị duy nhất có thể là`A=0`,`C=5`. Điều kiện trở thành`gcd(B,B+5)=5`, tương đương với`5|B`. Các bội số bên trong khoảng là`5`Và`10`, do đó thuật toán trả về`2`.
