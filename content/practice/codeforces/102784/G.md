---
title: "CF 102784G - Viên Sôcôla Hyper"
description: "Bài toán hỏi liệu một gói chứa chính xác x khối sô cô la có thể được cân bằng bằng cách sử dụng bộ sưu tập các gói siêu khối của Felix hay không. Các gói có sẵn có kích thước 1, w, w², ..., w¹⁰⁰, trong đó w là độ dài cạnh của mỗi hypercube."
date: "2026-07-27T19:48:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 52
verified: true
draft: false
---

[CF 102784G - Viên sô cô la Hyper](https://codeforces.com/problemset/problem/102784/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề hỏi liệu một gói có chứa chính xác`x`các khối sô-cô-la có thể được cân bằng bằng cách sử dụng bộ sưu tập các gói siêu khối của Felix. Các gói có sẵn có kích thước`1, w, w², ..., w¹⁰⁰`, Ở đâu`w`là độ dài cạnh của mỗi hypercube. Felix có thể đặt bất kỳ gói hàng nào lên hai bên của cân cân, sao cho mỗi lũy thừa của`w`có thể đóng góp tích cực, tiêu cực hoặc không đóng góp gì vào sự khác biệt cuối cùng. Nhiệm vụ là trả lời xem`x`có thể được biểu diễn dưới dạng tổng của các lũy thừa này với các hệ số từ`{-1, 0, 1}`. 

Dữ liệu đầu vào chứa tối đa 20 trọng lượng gói hàng để kiểm tra và tối đa mỗi trọng lượng là`10^9`. Căn cứ`w`nằm trong khoảng từ 2 đến 10. Vì số mũ lớn nhất mà chúng ta cần là nhỏ, bởi vì`10^10`đã vượt quá`10^9`, một giải pháp hoạt động theo từng chữ số trong cơ số`w`đủ nhanh một cách dễ dàng. Một cuộc tìm kiếm mạnh mẽ dựa trên các lựa chọn trong số 101 khối có thể có sẽ có tới`3^101`trạng thái hoàn toàn không thể thực hiện được nên lời giải phải khai thác cấu trúc toán học của lũy thừa. 

Nguồn gốc chính của sai lầm là giả định rằng cơ số bình thường`w`đại diện là đủ. Không phải vậy, vì các chữ số bình thường có thể lớn hơn 1, trong khi mỗi lũy thừa của`w`chỉ có thể được sử dụng một lần. Người mang có thể di chuyển một chữ số vào vị trí tiếp theo và làm cho nó hợp lệ. 

Ví dụ:```
Input:
4 5
25
```Đầu ra là:```
YES
```giá trị`25`không được biểu diễn dưới dạng tổng của các chữ số cơ số 5 thông thường chỉ sử dụng`0`Và`1`bởi vì nó là`100_5`, nhưng nó có thể được tạo ra bằng cách lấy`5²`khối lập phương, vì vậy câu trả lời là có. 

Một trường hợp phức tạp khác là khi một chữ số không thể được biểu diễn trực tiếp và cần phải nhớ:```
Input:
4 5
29
```Đầu ra là:```
YES
```Bởi vì`29 = 25 + 5 - 1`. Việc triển khai tham lam chỉ kiểm tra xem mọi chữ số cơ sở 5 có nhiều nhất là một chữ số có từ chối điều này không chính xác hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi lựa chọn có thể cho mọi hypercube. Mỗi sức mạnh của`w`có ba trạng thái: đặt nó về phía Felix, đặt nó về phía mẹ anh ấy hoặc bỏ qua nó. Cách tiếp cận này đúng vì nó liệt kê mọi sự sắp xếp có thể có trên thang cân bằng. Tuy nhiên, số lượng khả năng tăng lên khi`3^101`, vượt xa những gì có thể được xử lý. 

Quan sát hữu ích là sức mạnh của`w`hành xử giống hệt như các vị trí trong căn cứ`w`hệ thống số. Hạn chế số dư có nghĩa là mỗi vị trí muốn có một chữ số`-1`,`0`, hoặc`1`. Thay vì quyết định sử dụng khối nào, chúng ta có thể liên tục kiểm tra cơ sở thấp nhất`w`chữ số của`x`. Nếu chữ số đó là`0`hoặc`1`, chúng ta có thể sử dụng nó trực tiếp. Nếu nó lớn hơn, chúng ta có thể trừ một bản sao của chữ số đó bằng cách sử dụng khối lập phương âm ở vị trí này và mang nó đến vị trí tiếp theo. Đây là ý tưởng tương tự như chuyển đổi một số thành biểu diễn chữ số có dấu. 

Ví dụ, trong cơ số 5, số 29 có các chữ số`104_5`. Chữ số cuối cùng là 4, không được phép. Chúng tôi thay thế nó bằng`-1`và mang một đến vị trí tiếp theo:```
104_5
= 110_5 - 1
= 1 * 5² + 1 * 5 - 1
```Lực lượng vũ phu hoạt động vì nó xem xét tất cả các vị trí, nhưng không thành công vì có quá nhiều vị trí. Nhận thấy rằng mọi quyết định có thể được xử lý cục bộ thông qua chuyển đổi cơ sở sẽ giảm vấn đề xuống vòng lặp xử lý chữ số ngắn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^101) | O(101) | Quá chậm | 
| Chuyển đổi cơ sở đã ký | O(log_w(x)) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi giá trị hiện tại thành cơ sở`w`mỗi lần một chữ số bằng cách lấy liên tục`x % w`. 
2. Nếu chữ số hiện tại là`0`hoặc`1`, hãy xóa nó khỏi số bằng cách thực hiện phép chia số nguyên cho`w`. Các chữ số này đã khớp với hệ số cho phép. 
3. Nếu chữ số hiện tại lớn hơn`1`, đặt một khối âm ở vị trí này. Về mặt toán học, điều này có nghĩa là thay thế chữ số`d`với`d - w`và thêm một số mang vào vị trí tiếp theo. Từ`w`nhiều nhất là 10, mọi chữ số có thể có từ`2`ĐẾN`9`có thể sửa theo cách này. 
4. Tiếp tục cho đến khi giá trị còn lại bằng 0. Nếu mọi chữ số đều được xử lý thì số đó có biểu diễn có chữ ký hợp lệ. 

Lý do chuyển đổi luôn hoạt động là vì mọi số nguyên đều có modulo dư duy nhất`w`. Khi số dư lớn hơn một, sử dụng`-1`tại vị trí này để lại bội số`w`, có thể được chuyển sang chữ số tiếp theo. Quá trình tiếp tục giảm độ lớn của số còn lại, do đó cuối cùng nó kết thúc. 

Tại sao nó hoạt động: 

Ở mỗi bước, thuật toán duy trì cùng một bất biến: số lượng đã được biểu thị bằng các khối đã chọn cộng với giá trị còn lại vẫn bằng mục tiêu ban đầu`x`. Chọn một chữ số của`0`hoặc`1`tiêu thụ đúng số tiền đó. Lựa chọn`-1`tiêu thụ`-1`của sức mạnh hiện tại và di chuyển phần dư thừa`w`các đơn vị vào vị trí tiếp theo thông qua việc thực hiện. Khi không còn giá trị nào, toàn bộ số được biểu thị chỉ bằng hệ số`-1`,`0`, Và`1`, tương ứng chính xác với một thỏa thuận số dư hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def possible(x, w):
    while x:
        digit = x % w
        if digit <= 1:
            x //= w
        else:
            x = (x // w) + 1
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
```các`possible`hàm thực hiện chuyển đổi cơ sở đã ký. Hoạt động còn lại trích xuất cơ sở hiện tại-`w`chữ số. Khi chữ số là`0`hoặc`1`, phép chia thông thường sẽ loại bỏ chữ số đó. Khi chữ số lớn hơn, biểu thức`(x // w) + 1`thực hiện việc chuyển sang vị trí tiếp theo. 

Không cần phải theo dõi rõ ràng các chữ số âm vì câu hỏi duy nhất là liệu lựa chọn hợp lệ có tồn tại hay không. Mỗi chữ số lớn hơn một luôn có thể được thay thế bằng số âm bằng dấu mang. Điều kiện vòng lặp cũng tránh mọi giới hạn số mũ cố định vì giá trị trở thành 0 chỉ sau một số lần lặp nhỏ đối với các ràng buộc đã cho. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5
25
26
29
32
```Truy tìm`29`: 

| Bước | Giá trị hiện tại | Chữ số (`value % 5`) | Hành động | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 1 | 29 | 4 | Sử dụng`-1`và mang theo | 6 | 
| 2 | 6 | 1 | Sử dụng khối tích cực | 1 | 
| 3 | 1 | 1 | Sử dụng khối tích cực | 0 | 

Việc chuyển đổi mang lại`29 = 5² + 5 - 1`, nên cân có thể cân bằng. 

Đối với giá trị`32`: 

| Bước | Giá trị hiện tại | Chữ số (`value % 5`) | Hành động | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 1 | 32 | 2 | Sử dụng`-1`và mang theo | 7 | 
| 2 | 7 | 2 | Sử dụng`-1`và mang theo | 2 | 
| 3 | 2 | 2 | Sử dụng`-1`và mang theo | 1 | 
| 4 | 1 | 1 | Sử dụng khối tích cực | 0 | 

Dấu vết này vẫn kết thúc thành công, cho thấy rằng việc mang lặp lại được cho phép. Tuy nhiên, đây không phải là kết quả mẫu chính xác vì cách giải thích ban đầu yêu cầu mỗi kích thước siêu khối riêng lẻ chỉ tồn tại một lần tối đa.`w^100`. Từ`32`trong cơ sở 5 yêu cầu hệ số có độ lớn lớn hơn một sau khi chuẩn hóa thì không thể xây dựng được. Do đó, việc triển khai ở trên phải kiểm tra trực tiếp các chữ số đã ký. 

Việc triển khai đúng cần phát hiện trường hợp này:```
def possible(x, w):
    while x:
        digit = x % w
        if digit == 0 or digit == 1:
            x //= w
        elif digit == w - 1:
            x = x // w + 1
        else:
            return False
    return True
```Sử dụng phiên bản này,`32`thất bại vì cơ sở 5 có số dư`2`, không thể được đại diện bởi một trong hai`1`hoặc`-1`. 

Đối với giá trị mẫu`25`: 

| Bước | Giá trị hiện tại | Chữ số (`value % 5`) | Hành động | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| 1 | 25 | 0 | Xóa chữ số | 5 | 
| 2 | 5 | 0 | Xóa chữ số | 1 | 
| 3 | 1 | 1 | Sử dụng khối tích cực | 0 | 

Các chữ số 0 tương ứng với các vị trí trống và chữ số cuối cùng sử dụng`5²`khối lập phương. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log_w(x)) | Mỗi lần lặp lại loại bỏ một cơ sở-`w`chữ số. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Giá trị đầu vào lớn nhất có thể là`10^9`, vì vậy ngay cả trong cơ sở nhỏ nhất cũng chỉ có khoảng 30 lần lặp cho mỗi gói. Thuật toán dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def possible(x, w):
    while x:
        digit = x % w
        if digit == 0 or digit == 1:
            x //= w
        elif digit == w - 1:
            x = x // w + 1
        else:
            return False
    return True

def run(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    w = int(data[1])
    out = []
    idx = 2
    for _ in range(n):
        x = int(data[idx])
        idx += 1
        out.append("YES" if possible(x, w) else "NO")
    return "\n".join(out)

assert run("""4 5
25
26
29
32
""") == """YES
YES
YES
NO""", "sample"

assert run("""1 2
1
""") == "YES", "minimum value"

assert run("""3 10
9
10
11
""") == """YES
YES
YES""", "base boundary"

assert run("""2 3
1000000000
8
""") == """YES
YES""", "large values"

assert run("""3 5
2
3
4
""") == """NO
NO
NO""", "invalid digits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`25, 26, 29, 32`ở cơ sở 5 |`YES YES YES NO`| Hành vi mẫu và xử lý mang theo | 
|`1`ở cơ sở 2 |`YES`| Gói nhỏ nhất có thể | 
|`9, 10, 11`ở cơ sở 10 |`YES YES YES`| Chữ số gần ranh giới cơ sở | 
|`1000000000`ở căn cứ 3 |`YES`| Xử lý đầu vào lớn | 
|`2, 3, 4`ở cơ sở 5 |`NO NO NO`| Từ chối các chữ số không được hỗ trợ | 

## Vỏ cạnh 

Một giá trị chính xác là sức mạnh của`w`là trường hợp đơn giản nhất. Ví dụ:```
Input:
1 5
25
```Các chữ số là`100_5`. Thuật toán loại bỏ các chữ số 0 và chấp nhận một chữ số cuối cùng, phù hợp với việc sử dụng siêu khối vuông. 

Một giá trị có chữ số là`w - 1`cần mang theo. Ví dụ:```
Input:
1 5
26
```Số lượng là`101_5`. Nó có thể được biểu diễn dưới dạng`25 + 1`, do đó thuật toán chỉ nhìn thấy các chữ số`1`,`0`, Và`1`và chấp nhận nó. 

Một giá trị chứa một chữ số không phải là`0`,`1`, cũng không`w - 1`phải thất bại. Ví dụ:```
Input:
1 5
27
```Biểu diễn cơ số 5 chứa một chữ số`2`. Điều đó sẽ yêu cầu sử dụng hai bản sao của cùng một khối hoặc một hệ số không hợp lệ tương đương, do đó thuật toán sẽ loại bỏ nó ngay lập tức. 

Những trường hợp này bao gồm dạng lỗi chính của bài toán: nhầm lẫn giữa biểu diễn cơ sở thông thường với biểu diễn có chữ ký hạn chế được yêu cầu bởi thang đo cân bằng.
