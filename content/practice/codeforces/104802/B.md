---
title: "CF 104802B - Xe Buýt Tuyết"
description: "Mỗi hành khách có hai thuộc tính. Khối lượng của chúng góp phần tạo nên trọng lượng của xe buýt nếu chúng ở bên trong, trong khi lực đẩy của chúng chỉ đóng góp nếu chúng bước ra và đẩy. Giả sử chúng ta chọn một số hành khách làm người đẩy."
date: "2026-06-28T16:44:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104802
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #26 (Readall-Forces)"
rating: 0
weight: 104802
solve_time_s: 101
verified: false
draft: false
---

[CF 104802B - Xe buýt phủ đầy tuyết](https://codeforces.com/problemset/problem/104802/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi hành khách có hai thuộc tính. Khối lượng của chúng góp phần tạo nên trọng lượng của xe buýt nếu chúng ở bên trong, trong khi lực đẩy của chúng chỉ đóng góp nếu chúng bước ra và đẩy. 

Giả sử chúng ta chọn một số hành khách làm người đẩy. Tổng lực đẩy của chúng ít nhất phải bằng trọng lượng còn lại trên xe. Trọng lượng còn lại bao gồm chiếc xe trống cộng với khối lượng của mỗi hành khách không bước ra ngoài. 

Nhiệm vụ là chọn càng ít bộ đẩy càng tốt đồng thời thỏa mãn điều kiện này. Nếu không có sự lựa chọn nào của hành khách có thể di chuyển xe buýt thì câu trả lời là`-1`. 

Tổng số hành khách trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 × 10^5`. Điều này ngay lập tức loại trừ việc thử mọi tập hợp con, vì thậm chí`2^40`đã là không thể rồi, trong khi`2^200000`vượt xa mọi tính toán thực tế. MỘT`O(n^2)`Thuật toán cũng quá đắt vì trường hợp xấu nhất sẽ yêu cầu khoảng`4 × 10^10`hoạt động. MỘT`O(n log n)`giải pháp dễ dàng đủ nhanh cho những giới hạn này. 

Một số trường hợp đáng chú ý. 

Hãy xem xét một hành khách có lực lớn hơn nhiều so với những người khác.```
1
3 5
2 2 2
10 1 1
```Câu trả lời đúng là`1`. Một chiến lược đơn giản là loại bỏ những hành khách nặng nhất trước tiên sẽ chọn sai hai người, mặc dù chỉ riêng hành khách nặng nhất đã thành công. 

Một trường hợp quan trọng khác là việc di chuyển xe buýt là không thể ngay cả khi mọi người đều ra ngoài.```
1
2 100
1 1
30 40
```Ngay cả khi mỗi hành khách đẩy, tổng lực chỉ bằng`70`, trong khi chỉ riêng chiếc xe buýt đã nặng`100`. Câu trả lời đúng là`-1`. 

Một tình huống tế nhị hơn xuất hiện khi hai hành khách có cùng lực nhưng khối lượng khác nhau.```
1
2 5
100 1
10 10
```Chọn hành khách nặng hơn ít nhất cũng tốt như chọn hành khách nhẹ hơn vì cả hai đều đóng góp một lực như nhau, nhưng việc loại bỏ hành khách nặng hơn cũng làm giảm trọng lượng trên xe buýt nhiều hơn. Một giải pháp bỏ qua khối lượng hành khách trong khi phá vỡ các mối quan hệ có thể bỏ lỡ câu trả lời tối ưu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là liệt kê từng tập hợp con hành khách. Đối với mỗi tập hợp con, hãy tính tổng lực đẩy và trọng lượng còn lại, sau đó giữ lại tập hợp con nhỏ nhất thỏa mãn điều kiện. Điều này đúng vì mọi lựa chọn có thể đều được xem xét. Thật không may, nó đòi hỏi`O(2^n · n)`thời gian, điều này hoàn toàn không thể thực hiện được. 

Bất đẳng thức mô tả một lựa chọn hợp lệ là```
sum(force of chosen)
≥
w + sum(mass of unchosen)
```Cho phép`M`là tổng khối lượng của tất cả hành khách. 

Từ```
sum(mass of unchosen)
=
M - sum(mass of chosen),
```điều kiện trở thành```
sum(force) + sum(mass) ≥ w + M.
```Bây giờ mọi hành khách được chọn đều đóng góp độc lập bằng cách thêm```
force + mass
```hướng tới một mục tiêu cố định```
w + M.
```Sự chuyển đổi này loại bỏ sự tương tác giữa hành khách được chọn và không được chọn. Mỗi hành khách đơn giản có một giá trị```
value = force + mass.
```Để giảm thiểu số lượng hành khách được chọn trong khi đạt được tổng mục tiêu, chiến lược tham lam là hiển nhiên. Luôn lấy giá trị sẵn có lớn nhất trước tiên. Nếu một giải pháp sử dụng`k`hành khách tồn tại thì`k`các giá trị lớn nhất đạt được ít nhất tổng mức đóng góp như bất kỳ giá trị nào khác`k`hành khách. 

Sau khi sắp xếp các giá trị này theo thứ tự giảm dần, chúng tôi tiếp tục đón hành khách cho đến khi mức đóng góp tích lũy đạt được mục tiêu. Nếu thậm chí tất cả hành khách cùng nhau vẫn không đủ thì câu trả lời là`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n · n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc khối lượng và lực của tất cả hành khách. 
2. Tính tổng khối lượng hành khách`M`. Sự đóng góp bắt buộc là`target = w + M`. 
3. Với mỗi hành khách, hãy tính`value = mass + force`. Điều này đo lường mức độ giúp ích của việc lựa chọn hành khách đó. Lực của chúng làm tăng vế trái của bất đẳng thức, trong khi loại bỏ khối lượng của chúng sẽ làm giảm trọng lượng cần thiết một lượng bằng nhau. 
4. Sắp xếp tất cả các giá trị theo thứ tự giảm dần. Vì chúng tôi muốn có ít hành khách nhất nên mỗi hành khách được chọn nên đóng góp càng nhiều càng tốt. 
5. Duyệt qua các giá trị được sắp xếp từ lớn nhất đến nhỏ nhất, duy trì phần đóng góp tích lũy. 
6. Ngay khi số tiền đóng góp tích lũy đạt ít nhất`target`, xuất ra bao nhiêu hành khách đã được chở. Không thể có câu trả lời nhỏ hơn vì bất kỳ tập hợp nào khác có cùng số lượng hành khách đều có đóng góp không lớn hơn các giá trị lớn nhất đã được chọn. 
7. Nếu toàn bộ danh sách được xử lý mà không đạt được mục tiêu, hãy xuất`-1`. 

### Tại sao nó hoạt động 

Bất đẳng thức biến đổi chỉ phụ thuộc vào tổng của`mass + force`trên các hành khách được lựa chọn. Trong số tất cả các tập con chứa chính xác`k`hành khách, sự đóng góp lớn nhất có thể đạt được bằng cách lấy`k`những giá trị lớn nhất. Nếu những giá trị lớn nhất này không đạt được mục tiêu thì sẽ không có tập hợp con nào khác có kích thước`k`có thể thành công. Ngược lại, khi chúng đạt được mục tiêu, một tập hợp con có kích thước hợp lệ sẽ`k`tồn tại. Tiền tố thành công đầu tiên của danh sách được sắp xếp chính xác là số lượng bộ đẩy tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, w = map(int, input().split())
        masses = list(map(int, input().split()))
        forces = list(map(int, input().split()))

        target = w + sum(masses)
        values = [m + f for m, f in zip(masses, forces)]
        values.sort(reverse=True)

        cur = 0
        ans = -1
        for i, v in enumerate(values, 1):
            cur += v
            if cur >= target:
                ans = i
                break

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán mức đóng góp mục tiêu, đó là trọng lượng xe buýt trống cộng với tổng khối lượng hành khách. 

Mỗi hành khách được chuyển đổi thành một giá trị duy nhất bằng khối lượng cộng với lực của họ. Điều này xuất phát trực tiếp từ phép biến đổi đại số của bất đẳng thức ban đầu. 

Việc sắp xếp các giá trị này theo thứ tự giảm dần cho phép thuật toán kiểm tra tập hợp con tốt nhất có thể có ở mọi kích thước. Tổng hoạt động thể hiện mức đóng góp tối đa có thể đạt được khi sử dụng số lượng hành khách hiện tại. 

Tiền tố đầu tiên có tổng đạt được mục tiêu ngay lập tức đưa ra câu trả lời tối ưu. Nếu tiền tố đầy đủ vẫn thiếu thì ngay cả việc chọn mọi người cũng không thể di chuyển xe buýt. 

Số nguyên Python tự động xử lý số tiền lớn nhất có thể, do đó không cần phải có biện pháp phòng ngừa tràn. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm sau đây.```
1
3 4
1 1 1
6 3 3
```Tổng khối lượng hành khách là`3`, vậy mục tiêu là`7`. 

Giá trị hành khách là`[7, 4, 4]`. 

| Bước | Giá trị được chọn | Tổng Chạy | Mục tiêu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 7 | 7 | 1 | 

Mục tiêu đạt được sau khi chọn hành khách đầu tiên nên chỉ cần một người đẩy. 

Bây giờ hãy xem xét một ví dụ không thể thực hiện được.```
1
2 100
1 1
30 40
```Tổng khối lượng hành khách là`2`, vậy mục tiêu là`102`. 

Các giá trị là`[41, 31]`. 

| Bước | Giá trị được chọn | Tổng Chạy | Mục tiêu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 41 | 41 | 102 | Chưa đạt | 
| 2 | 31 | 72 | 102 | Chưa đạt | 

Ngay cả sau khi chọn tất cả mọi người, sự đóng góp chỉ là`72`, vậy câu trả lời là`-1`. 

Dấu vết đầu tiên chứng minh rằng một hành khách khỏe mạnh có thể là đủ. Thứ hai xác nhận rằng thuật toán phát hiện chính xác khi không có tập hợp con khả thi nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp chiếm ưu thế trong thời gian chạy. | 
| Không gian |`O(n)`| Lưu trữ các giá trị được chuyển đổi. | 

Vì tổng số hành khách trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 × 10^5`, việc sắp xếp từng trường hợp kiểm thử dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở dưới mức giới hạn cho phép. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, w = map(int, input().split())
        m = list(map(int, input().split()))
        f = list(map(int, input().split()))
        target = w + sum(m)
        vals = sorted((a + b for a, b in zip(m, f)), reverse=True)
        s = 0
        ans = -1
        for i, v in enumerate(vals, 1):
            s += v
            if s >= target:
                ans = i
                break
        out.append(str(ans))
    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue().strip()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return res

assert run("""4
3 4
1 1 1
6 6 6
3 4
1 1 1
3 3 3
1 1000
100
100
6 10
7 5 1 4 2 8
3 1 2 7 5 9
""") == "1\n2\n-1\n3", "sample"

assert run("""1
1 1
1
2
""") == "1", "minimum feasible"

assert run("""1
1 10
1
1
""") == "-1", "minimum impossible"

assert run("""1
4 4
2 2 2 2
2 2 2 2
""") == "3", "all equal"

assert run("""1
2 5
100 1
10 10
""") == "1", "tie on force but different masses"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hành khách duy nhất thành công |`1`| Trường hợp khả thi tối thiểu | 
| Hành khách duy nhất thất bại |`-1`| Trường hợp không thể | 
| Bốn hành khách giống hệt nhau |`3`| Đếm tiền tố đúng | 
| Lực bằng nhau, khối lượng khác nhau |`1`| Sử dụng đúng`mass + force`| 

## Vỏ cạnh 

Hãy xem xét trường hợp một hành khách mạnh hơn rất nhiều so với những hành khách khác.```
1
3 5
2 2 2
10 1 1
```Mục tiêu là`11`. Các giá trị được chuyển đổi là`[12, 3, 3]`. Sau khi chọn giá trị đầu tiên, khoản đóng góp tích lũy đã được`12`, do đó thuật toán trả về ngay`1`. Chỉ chọn hành khách theo khối lượng sẽ bỏ lỡ giải pháp này. 

Bây giờ hãy xem xét một trường hợp không thể xảy ra.```
1
2 100
1 1
30 40
```Mục tiêu bằng`102`, trong khi tổng số tiền đóng góp của mỗi hành khách chỉ là`72`. Tổng chạy không bao giờ đạt được mục tiêu, do đó thuật toán trả về chính xác`-1`. 

Cuối cùng, kiểm tra các hành khách có lực bằng nhau nhưng khối lượng khác nhau.```
1
2 5
100 1
10 10
```Các giá trị được chuyển đổi là`[110, 11]`, và mục tiêu là`106`. Chỉ riêng giá trị đầu tiên đã đạt được mục tiêu, vì vậy câu trả lời là`1`. Điều này chứng tỏ tại sao cả khối lượng và lực phải được kết hợp thành một giá trị đóng góp duy nhất thay vì chỉ xem xét lực.
