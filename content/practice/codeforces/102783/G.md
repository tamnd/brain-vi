---
title: "CF 102783G - Viên Sôcôla Hyper"
description: "Felix nhận được các khối sôcôla có kích thước tương ứng với lũy thừa của chiều dài cạnh đã chọn w. Các quân có sẵn có trọng số: 1, w, w², w³, ..., w¹⁰⁰."
date: "2026-07-27T20:01:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102783
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 2"
rating: 0
weight: 102783
solve_time_s: 77
verified: true
draft: false
---

[CF 102783G - Viên sô cô la Hyper](https://codeforces.com/problemset/problem/102783/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Felix nhận được sô cô la siêu khối với kích thước tương ứng với lũy thừa của chiều dài cạnh đã chọn`w`. Các mảnh có sẵn có trọng lượng:`1, w, w², w³, ..., w¹⁰⁰`. 

Đối với mỗi trọng lượng gói hàng`x`, chúng ta cần xác định xem liệu Felix có thể đặt một số khối lập phương này ở hai bên của cân cân để hai bên bằng nhau, với gói hàng ở một bên hay không. 

Đặt một miếng sô cô la ở cùng một phía với gói hàng sẽ trừ đi giá trị của nó khỏi trọng lượng hiệu dụng của gói hàng, trong khi đặt nó ở phía đối diện sẽ cộng thêm giá trị của nó. Vì mọi hypercube có sẵn chỉ có thể được sử dụng nhiều nhất một lần nên câu hỏi đặt ra là liệu`x`có thể được viết dưới dạng tổng lũy ​​thừa của`w`trong đó mọi hệ số là một trong:`-1, 0, 1`. 

Dữ liệu đầu vào chứa tối đa 20 trọng lượng gói hàng, tối đa mỗi trọng lượng`10^9`. Việc tìm kiếm trực tiếp trên các tập hợp con là không thể vì có 101 lũy thừa có thể có của`w`, đưa ra nhiều sự kết hợp hơn mức có thể khám phá. Ngay cả việc hạn chế chúng ta trong những sức mạnh hữu ích vẫn sẽ để lại một không gian tìm kiếm theo cấp số nhân, vì vậy giải pháp phải khai thác cấu trúc của sức mạnh. 

Đặc tính quan trọng là quyền hạn của`w`tạo thành một hệ thống số vị trí. Thay vì chọn các phần trên toàn cục, chúng ta có thể quyết định hệ số của từng lũy ​​thừa từ lũy thừa nhỏ nhất trở lên. Một khi hệ số của`w^i`cố định thì giá trị còn lại phải chia hết cho`w^(i+1)`, điều này cho phép chúng ta giải quyết vấn đề từng chữ số một. 

Các trường hợp phức tạp đến từ các giá trị trông gần có thể biểu thị nhưng yêu cầu chữ số không hợp lệ. Ví dụ, với`w = 5`, giá trị`2`không thể được tạo ra:```
1
5
25
```Các kết hợp có dấu có thể có gần phạm vi này là`-5, -4, -1, 0, 1, 4, 5`, vậy câu trả lời là`NO`. Một cách tiếp cận ngây thơ chỉ kiểm tra các chữ số cơ sở 5 bình thường có thể cho rằng mọi chữ số đều có thể được điều chỉnh một cách không chính xác. 

Một trường hợp cạnh khác là khi phần còn lại là`w - 1`. Ví dụ, với`w = 5`Và`x = 4`, biểu diễn đúng là:`5 - 1 = 4`. 

Chữ số đầu tiên thực sự là`-1`, không`4`. Bất kỳ cách tiếp cận nào chỉ cho phép chữ số`0`bởi vì`w-1`sẽ thất bại ở đây. 

Một ví dụ nhỏ:```
Input
1 5
4

Output
YES
```Gói hàng có thể được cân bằng bằng cách đặt`5`khối lập phương ở một bên và khối đơn ở phía bên kia. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi phép gán có thể có của các siêu khối có sẵn. Mỗi sức mạnh có thể có ba trạng thái: không sử dụng, được đặt cùng với gói hàng hoặc đặt trên gói hàng. Nếu chúng ta xem xét tất cả 101 lũy thừa, điều này mang lại`3^101`khả năng, đó là lớn về mặt thiên văn. Mặc dù chỉ có sức mạnh tối đa`10^9`quan trọng về mặt số học, không gian tìm kiếm vẫn mang tính hàm mũ và không thể hoạt động được. 

Quan sát hữu ích là đây chính xác là một bài toán biểu diễn vị trí cân bằng. Trong một căn cứ bình thường-`w`đại diện, mỗi chữ số phải nằm giữa`0`Và`w-1`. Ở đây, mỗi chữ số có thể thay thế`-1`,`0`, hoặc`1`. Chúng ta chỉ cần kiểm tra modulo còn lại hiện tại`w`. 

Giả sử giá trị hiện tại là`x`. Hệ số của`1`-power xác định số dư khi chia cho`w`. Nếu như`x % w`bằng 0 thì chữ số hiện tại phải bằng 0. Nếu phần còn lại là một, chúng tôi sử dụng số dương. Nếu phần còn lại là`w-1`, chúng ta sử dụng số âm vì trừ đi một sẽ làm cho giá trị chia hết cho`w`. 

Sau khi loại bỏ chữ số đó ta chia cho`w`và giải quyết vấn đề tương tự cho sức mạnh tiếp theo. Nếu xuất hiện phần dư không thể biểu diễn bằng`-1`,`0`, hoặc`1`, câu trả lời là không thể. 

Brute-force hoạt động vì nó thử mọi vị trí hợp lệ nhưng không thành công vì nó bỏ qua cấu trúc vị trí. Quá trình tham lam từng chữ số thành công vì mọi quyết định đều bị ép buộc bởi tính chia hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^101) | O(101) | Quá chậm | 
| Tối ưu | O(100) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc trọng lượng của từng gói hàng và liên tục kiểm tra phần còn lại của nó khi chia cho`w`. Công suất thấp nhất`w^0`là công suất duy nhất ảnh hưởng đến modulo còn lại`w`, vì vậy nó phải được quyết định trước. 
2. Nếu số dư bằng 0 thì gán hệ số`0`đến sức mạnh hiện tại. Giá trị hiện tại đã chia hết cho`w`, vì vậy không cần khối lập phương có kích thước này. 
3. Nếu số dư bằng 1 thì gán hệ số`1`. Lấy một khối có kích thước hiện tại sẽ loại bỏ phần còn lại. 
4. Nếu phần còn lại là`w - 1`, ấn định hệ số`-1`. Điều này cũng giống như việc thêm một vào chữ số tiếp theo vì`-1 ≡ w-1 (mod w)`và nó làm cho giá trị còn lại chia hết cho`w`. 
5. Nếu phần dư là bất kỳ giá trị nào khác thì không tồn tại hệ số hợp lệ cho lũy thừa hiện tại, vì vậy câu trả lời là`NO`. 
6. Chia giá trị còn lại cho`w`và tiếp tục với sức mạnh tiếp theo. Quá trình dừng lại khi giá trị trở thành 0. 

Tại sao nó hoạt động: 

Ở mỗi bước, giá trị hiện tại phải được biểu thị dưới dạng:`current = digit + w * remaining`Ở đâu`digit`là một trong`-1`,`0`, hoặc`1`. Chữ số được chọn là chữ số duy nhất có thể tạo nên`current - digit`chia hết cho`w`. Sau khi loại bỏ nó, vấn đề còn lại giống hệt nhau nhưng tăng thêm một công suất. Vì mọi quyết định chữ số đều bảo toàn sự tương đương với bài toán số dư ban đầu, nên đạt đến số 0 có nghĩa là chúng ta đã tìm thấy một cách sắp xếp hợp lệ, trong khi việc gặp phải số dư không thể chứng tỏ rằng không có sự sắp xếp nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(x, w):
    while x > 0:
        r = x % w

        if r == 0:
            digit = 0
        elif r == 1:
            digit = 1
        elif r == w - 1:
            digit = -1
        else:
            return False

        x = (x - digit) // w

    return True

def solve():
    n, w = map(int, input().split())

    ans = []
    for _ in range(n):
        x = int(input())
        ans.append("YES" if possible(x, w) else "NO")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`possible`hàm thực hiện chuyển đổi cơ số cân bằng. Biến`x`luôn đại diện cho phần trọng lượng gói hàng chưa được các quyền lực nhỏ hơn xử lý. 

Việc kiểm tra phần còn lại là cốt lõi của việc thực hiện. Ba trường hợp hợp lệ tương ứng chính xác với ba hệ số có thể có của công suất dòng điện. dòng`x = (x - digit) // w`đầu tiên loại bỏ hệ số đã chọn và sau đó chuyển sang lũy ​​thừa tiếp theo. 

Không cần thiết phải tạo ra quyền lực một cách rõ ràng`w`. Sự phân chia lặp đi lặp lại tự nhiên đi qua các vị trí giống nhau. Vì tất cả các đầu vào nhiều nhất là`10^9`, cần ít hơn 100 lần lặp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
25
26
29
32
```Vì`w = 5`, quá trình này trông như thế này: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 25 | 0 | 0 | 5 | 
| 5 | 0 | 0 | 1 | 
| 1 | 1 | 1 | 0 | 

số`25`chính xác là`5²`, do đó một hypercube vuông sẽ cân bằng nó. 

Vì`26`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 26 | 1 | 1 | 5 | 
| 5 | 0 | 0 | 1 | 
| 1 | 1 | 1 | 0 | 

Điều này tương ứng với`25 + 1`. 

Vì`29`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 29 | 4 | -1 | 6 | 
| 6 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 0 | 

Sự đại diện là`25 + 5 - 1`. 

Vì`32`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 32 | 2 | không hợp lệ | - | 

Phần còn lại không thể biểu diễn được nên đáp án là`NO`. 

### Ví dụ 2 

đầu vào:```
3 3
1
2
5
```Vì`w = 3`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 |`1`có sẵn trực tiếp. 

Vì`2`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 2 | 2 | -1 | 1 | 
| 1 | 1 | 1 | 0 | 

Sự đại diện là`3 - 1`. 

Vì`5`: 

| Giá trị | Phần còn lại | Chữ số được chọn | Giá trị mới | 
| --- | --- | --- | --- | 
| 5 | 2 | -1 | 2 | 
| 2 | 2 | -1 | 1 | 
| 1 | 1 | 1 | 0 | 

Sự đại diện là`9 - 3 - 1`, nhưng vì chỉ có lũy thừa lên tới 100 nên điều này hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(100n) | Mỗi số được chia cho`w`cho đến khi nó trở thành số không. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Kích thước đầu vào tối đa chỉ có 20 giá trị, do đó việc chuyển đổi logarit này dễ dàng nằm trong giới hạn. Thuật toán không bao giờ khám phá sự kết hợp của các hình khối, đây là nguồn gốc của độ khó theo cấp số nhân trong phương pháp tiếp cận vũ phu. 

## Trường hợp thử nghiệm```python
import sys
import io

def possible(x, w):
    while x > 0:
        r = x % w
        if r == 0:
            digit = 0
        elif r == 1:
            digit = 1
        elif r == w - 1:
            digit = -1
        else:
            return False
        x = (x - digit) // w
    return True

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, w = map(int, sys.stdin.readline().split())
    res = []
    for _ in range(n):
        x = int(sys.stdin.readline())
        res.append("YES" if possible(x, w) else "NO")
    return "\n".join(res)

assert run("""4 5
25
26
29
32
""") == """YES
YES
YES
NO""", "sample 1"

assert run("""3 3
1
2
5
""") == """YES
YES
YES""", "balanced ternary"

assert run("""1 5
2
""") == "NO", "invalid remainder"

assert run("""1 2
1000000000
""") == "YES", "large binary case"

assert run("""3 10
1
9
11
""") == """YES
YES
YES""", "decimal balanced representation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 5 / 25 26 29 32`|`YES YES YES NO`| Hành vi mẫu chính thức và phát hiện chữ số không hợp lệ | 
|`3 3 / 1 2 5`|`YES YES YES`| Biểu diễn phong cách ternary cân bằng | 
|`1 5 / 2`|`NO`| Từ chối phần còn lại không thể | 
|`1 2 / 1000000000`|`YES`| Giá trị lớn và phép chia lặp đi lặp lại | 
|`3 10 / 1 9 11`|`YES YES YES`| Phần dư biên bao gồm`w-1`| 

## Vỏ cạnh 

Khi phần còn lại cũng không`0`,`1`, cũng không`w-1`, thuật toán ngay lập tức từ chối giá trị. Ví dụ:```
Input
1 5
2
```Số dư đầu tiên là`2`. Chữ số hiện tại sẽ cần phải là`2`, nhưng một khối lập phương chỉ có thể đóng góp`-1`,`0`, hoặc`1`ở vị trí này. Thuật toán trả về`NO`. 

Khi phần còn lại là`w-1`, lời giải phải sử dụng chữ số âm. Ví dụ:```
Input
1 5
4
```Số dư đầu tiên là`4`, nên ta chọn chữ số`-1`:`4 - (-1) = 5`Sau khi chia cho`5`, giá trị còn lại là`1`, được xử lý bằng cách chọn chữ số`1`. Sự đại diện là`5 - 1`, vậy câu trả lời là`YES`. 

Các giá trị tối đa cũng an toàn. Ví dụ:```
Input
1 2
1000000000
```Thuật toán chỉ thực hiện rút gọn nhị phân. Nó kết thúc sau khoảng 30 lần lặp, thấp hơn nhiều so với 100 lũy thừa có sẵn và xác định chính xác liệu biểu diễn đã ký có tồn tại hay không.
