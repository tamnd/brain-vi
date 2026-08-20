---
title: "CF 102185B - \u0424\u0438\u043a\u0441\u0438\u0440\u043e\u0432\u0430\u043d\u043d\u0430\u044f \u0446\u0435\u043d\u0430"
description: "Cửa hàng có một mức giá cố định P. Đối với mỗi sản phẩm, chúng ta biết số lượng đơn vị A cần thiết và giá thị trường của một đơn vị S. Cửa hàng hành xử khác nhau tùy thuộc vào việc liệu một đơn vị đã có giá trị ít nhất là P hay chưa. Nếu S = P, có sẵn các đơn vị riêng lẻ, mỗi đơn vị có giá chính xác là P."
date: "2026-08-19T06:24:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "B"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 78
verified: true
draft: false
---

[CF 102185B - \u0424\u0438\u043a\u0441\u0438\u0440\u043e\u0432\u0430\u043d\u043d\u0430\u044f \u0446\u0435\u043d\u0430](https://codeforces.com/problemset/problem/102185/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cửa hàng có một mức giá cố định`P`. Đối với mỗi sản phẩm, chúng tôi biết số lượng đơn vị cần thiết`A`và giá thị trường của một đơn vị`S`. 

Cửa hàng hoạt động khác nhau tùy thuộc vào việc một đơn vị đã có giá trị ít nhất hay chưa`P`. Nếu như`S >= P`, các đơn vị riêng lẻ có sẵn, mỗi đơn vị có giá chính xác`P`. Để có được ít nhất`A`đơn vị, chúng tôi chỉ đơn giản là mua`A`đơn vị, do đó chi phí là`A * P`. 

Nếu như`S < P`, các đơn vị riêng lẻ không thể được mua. Thay vào đó, một gói có giá`P`và chứa`ceil(P / S)`đơn vị. Chúng ta có thể mua số lượng gói hoàn chỉnh bất kỳ, vì vậy nhiệm vụ trở thành tìm số lượng gói nhỏ nhất có tổng số đơn vị ít nhất là`A`. 

Ví dụ, với`P = 100`Và`S = 51`, một gói chứa`ceil(100 / 51) = 2`đơn vị. Để có được ít nhất`6`đơn vị, chúng tôi cần`ceil(6 / 2) = 3`gói, chi phí`300`. 

Có nhiều nhất`1000`sản phẩm và mọi`A`Và`S`nhiều nhất là`1000`. Các giới hạn này đủ nhỏ để ngay cả một mô phỏng yêu cầu lên tới`1000`số lần lặp lại trên mỗi sản phẩm sẽ hoạt động tối đa khoảng`10^6`lần lặp lại. Một giải pháp như vậy sẽ trôi qua một cách thoải mái. Tuy nhiên, cấu trúc của quy tắc mua hàng cho phép chúng ta giải quyết mọi sản phẩm trong thời gian không đổi, đưa ra một cách đặc biệt đơn giản.`O(N)`giải pháp. 

Các trường hợp rủi ro chính xuất phát từ ranh giới trong hai quy tắc định giá. Khi`S = P`, sản phẩm được bán riêng lẻ nên đối với đầu vào```
1 1004 100
```câu trả lời là`400`. Việc triển khai bất cẩn bằng cách sử dụng công thức gói có thể tính toán kích thước gói là`1`và vẫn nhận được kết quả tương tự, nhưng việc điều trị`S = P`vì trường hợp gói có thể che khuất quy tắc thực tế và gây ra lỗi nếu việc triển khai giả định các gói chứa nhiều đơn vị. 

Trường hợp cạnh thứ hai là khi`P`không chia hết cho`S`. Vì```
1 1006 30
```một gói chứa`ceil(100 / 30) = 4`đơn vị. Hai gói cung cấp`8`đơn vị, vì vậy câu trả lời là`200`. Sử dụng phép chia số nguyên`100 // 30 = 3`sẽ kết luận sai rằng một gói chỉ chứa ba đơn vị và tạo ra`200`ở đây là sự trùng hợp ngẫu nhiên cho sáu đơn vị bắt buộc, nhưng đối với```
1 1007 30
```câu trả lời đúng vẫn là`200`, trong khi sử dụng kích thước gói sai là ba sẽ tạo ra`300`. 

Ranh giới thứ ba xảy ra khi số lượng yêu cầu đã nhỏ hơn một gói hàng. Vì```
1 1001 40
```một gói chứa`ceil(100 / 40) = 3`đơn vị, nên mua một gói là đủ và câu trả lời là`100`. Nhân số lượng yêu cầu với giá cố định sẽ cho kết quả không chính xác`40`về mặt số lượng gói, hoặc`100`chỉ khi sự phân biệt được xử lý chính xác. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp có thể giải quyết vấn đề bằng cách thêm liên tục một gói cho đến khi đạt được số lượng đơn vị tích lũy`A`. Đối với một sản phẩm có`S < P`, trước tiên chúng tôi tính toán kích thước gói hàng`K = ceil(P / S)`, sau đó thêm liên tục`K`cho đến khi chúng ta có đủ đơn vị. Trong trường hợp xấu nhất,`K = 1`, vì vậy vòng lặp này thực hiện tối đa`A <= 1000`lặp đi lặp lại cho một sản phẩm. Với`N <= 1000`, đó là nhiều nhất`10^6`tổng số lần lặp. Do đó, mô phỏng lực lượng vũ phu này thực sự đủ nhanh cho các ràng buộc nhất định. 

Quan sát loại bỏ vòng lặp là mọi gói hàng đều có cùng kích thước và giá cả. Nếu mỗi gói đóng góp`K`đơn vị, sau đó sau khi mua`x`gói chúng tôi có chính xác`xK`đơn vị. Chúng ta cần số nguyên nhỏ nhất`x`thỏa mãn`xK >= A`, chính xác là`ceil(A / K)`. Do đó toàn bộ mô phỏng có thể được thay thế bằng một bộ phận trần. 

Hai trường hợp có thể được kết hợp thành một phép tính rất nhỏ. Nếu như`S >= P`, một lần mua sẽ có một đơn vị, vậy số lần mua là`A`. Nếu không, một lần mua sẽ mang lại`K = ceil(P / S)`đơn vị, vì vậy số lượng mua là`ceil(A / K)`. Mỗi chi phí mua hàng`P`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |`O(sum A_i)`, nhiều nhất`10^6`lặp đi lặp lại |`O(1)`| Đã chấp nhận | 
| Cách chia trần tối ưu |`O(N)`|`O(1)`| Đã chấp nhận | 

Cách tiếp cận tối ưu được ưa chuộng hơn vì nó thể hiện trực tiếp điều kiện toán học thay vì mô phỏng việc mua hàng. Việc chứng minh là đúng cũng dễ dàng hơn và không có vòng lặp có số lần lặp phụ thuộc vào số lượng được yêu cầu. 

## Hướng dẫn thuật toán 

1. Đọc`N`và giá cố định`P`. Mỗi cái tiếp theo`N`dòng mô tả một sản phẩm độc lập, vì vậy các sản phẩm có thể được xử lý riêng biệt. 
2. Đối với một sản phẩm, hãy đọc số lượng yêu cầu`A`và giá thị trường`S`. 
3. Nếu`S >= P`, đặt số lượng mua hàng thành`A`. Cửa hàng bán các đơn vị riêng lẻ trong trường hợp này và mỗi đơn vị có giá`P`. 
4. Nếu không, hãy tính số lượng đơn vị trong một gói như sau:`K = ceil(P / S)`. Vì tất cả các giá trị đều là số nguyên nên phép chia trần có thể được viết là`(P + S - 1) // S`. 
5. Tính số lượng gói hàng cần thiết như sau:`ceil(A / K)`, một lần nữa sử dụng số học số nguyên như`(A + K - 1) // K`. 
6. Nhân số lần mua với`P`và xuất ra giá trị đó. 
7. Lặp lại phép tính độc lập cho tất cả các sản phẩm và in kết quả chi phí. 

### Tại sao nó hoạt động 

cho`S >= P`, cửa hàng bán đúng một đơn vị cho mỗi lần thanh toán`P`, vì vậy việc mua hàng`A`đơn vị yêu cầu chính xác`A`thanh toán. 

Vì`S < P`, mỗi gói chứa chính xác`K = ceil(P / S)`đơn vị. Sau khi mua`x`gói, chúng tôi có`xK`đơn vị. Thuật toán chọn số nguyên nhỏ nhất`x`vì cái gì`xK >= A`, cụ thể là`ceil(A / K)`. Vì vậy, nó mua đủ số lượng, trong khi một gói ít hơn sẽ cung cấp ít hơn`A`đơn vị. Vì mỗi gói đều có giá như nhau`P`, số lượng gói tối thiểu này cũng mang lại chi phí tối thiểu có thể. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n, p = map(int, input().split())    ans = []
    for _ in range(n):        a, s = map(int, input().split())
        if s >= p:            purchases = a        else:            package_size = (p + s - 1) // s            purchases = (a + package_size - 1) // package_size
        ans.append(purchases * p)
    print(*ans)

if __name__ == "__main__":    solve()
```Đầu tiên chương trình sẽ đọc số lượng sản phẩm và giá cố định chung. Câu trả lời được tích lũy thành một danh sách để tất cả chi phí có thể được in trên một dòng ở cuối. 

điều kiện`s >= p`tuân theo ranh giới của tuyên bố một cách chính xác. Sự bình đẳng thuộc về trường hợp đơn vị riêng lẻ, mặc dù công thức cũng sẽ tạo ra kích thước gói bằng một khi`s == p`. 

Đối với trường hợp gói,`(p + s - 1) // s`thực hiện phép chia trần số nguyên. Viết`p // s`sẽ làm tròn xuống và có thể đánh giá thấp số lượng đơn vị trong một gói. 

Bộ phận trần thứ hai tính toán cần bao nhiêu gói hàng để đạt được ít nhất`a`đơn vị. Phép nhân với`p`chỉ được thực hiện sau khi số lượng mua hàng cần thiết đã được xác định. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. Với các ràng buộc đã cho, đáp án lớn nhất có thể có cũng rất nhỏ: nhiều nhất là`1000 * 1000 = 10^6`cho các đơn vị được bán riêng lẻ. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp là```
5 1001 1016 5111 1012 94 100
```Quá trình xử lý có thể được theo dõi như sau. 

|`A`|`S`| Trường hợp | Kích thước gói`K`| Mua hàng | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 101 | Cá nhân | 1 | 1 | 100 | 
| 6 | 51 | Gói | 2 | 3 | 300 | 
| 11 | 10 | Gói | 10 | 2 | 200 | 
| 12 | 9 | Gói | 12 | 1 | 100 | 
| 4 | 100 | Cá nhân | 1 | 4 | 400 | 

Đối với sản phẩm thứ hai,`ceil(100 / 51) = 2`, vậy ba gói cung cấp chính xác sáu đơn vị. Đối với sản phẩm thứ ba, mỗi gói chứa mười đơn vị, vì vậy hai gói cung cấp hai mươi đơn vị, đủ cho mười một đơn vị cần thiết. Kết quả đầu ra là```
100 300 200 100 400
```Ví dụ thứ hai thể hiện một số ranh giới:```
4 1001 407 3010 1001000 1
```|`A`|`S`| Trường hợp | Kích thước gói`K`| Mua hàng | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 40 | Gói | 3 | 1 | 100 | 
| 7 | 30 | Gói | 4 | 2 | 200 | 
| 10 | 100 | Cá nhân | 1 | 10 | 1000 | 
| 1000 | 1 | Gói | 100 | 10 | 1000 | 

Sản phẩm đầu tiên chứng minh rằng chỉ cần một thiết bị vẫn cần cả một gói. Phần thứ hai thể hiện sự phân chia trần khi`P`không chia hết cho`S`. Thứ ba kiểm tra chính xác`S = P`ranh giới. Điều cuối cùng cho thấy rằng ngay cả số lượng yêu cầu lớn cũng được xử lý bằng số học theo thời gian không đổi thay vì mô phỏng mua hàng. 

Đầu ra là```
100 200 1000 1000
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`| Mỗi sản phẩm đều yêu cầu một số phép tính số học không đổi. | 
| Không gian |`O(N)`| Danh sách đầu ra lưu trữ một câu trả lời cho mỗi sản phẩm. | 

Với nhiều nhất`1000`sản phẩm, thuật toán chỉ thực hiện vài nghìn phép tính số học. Nó thấp hơn nhiều so với giới hạn thời gian và bộ nhớ có sẵn. 

Danh sách đầu ra cũng có thể tránh được bằng cách in ngay từng câu trả lời, giảm khoảng trống phụ xuống còn`O(1)`. Việc giữ các câu trả lời trong một danh sách giúp dễ dàng tạo ra kết quả đầu ra được phân tách bằng dấu cách. 

## Trường hợp thử nghiệm```python
Python# helper: run solution on input string, return output stringimport sysimport io

def solve():    input = sys.stdin.readline    n, p = map(int, input().split())    ans = []
    for _ in range(n):        a, s = map(int, input().split())
        if s >= p:            purchases = a        else:            package_size = (p + s - 1) // s            purchases = (a + package_size - 1) // package_size
        ans.append(purchases * p)
    print(*ans)

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue().strip()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided sampleassert run(    """5 1001 1016 5111 1012 94 100""") == "100 300 200 100 400", "sample 1"
# Minimum-size inputassert run(    """1 11 1""") == "1", "minimum values"
# Exact boundary S = Passert run(    """3 1001 1005 1001000 100""") == "100 500 100000", "S = P boundary"
# Ceiling division boundariesassert run(    """4 1001 403 404 407 30""") == "100 100 200 200", "ceiling division"
# All values equalassert run(    """4 5050 5050 5050 5050 50""") == "2500 2500 2500 2500", "all equal"
# Maximum-size valuesassert run(    """3 10001000 11000 9991000 1000""") == "10000 2000 1000000", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 1`|`1`| Giá trị tối thiểu được phép | 
|`1 100 / 1 100`và các vụ việc liên quan |`100 500 100000`| Chính xác`S = P`ranh giới | 
|`1 40`,`3 40`,`4 40`,`7 30`|`100 100 200 200`| Phân chia trần và ranh giới gói chính xác | 
| Bốn sản phẩm có`A = S = 50`|`2500 2500 2500 2500`| Giá trị bằng nhau lặp lại và định giá theo đơn vị riêng lẻ | 
|`P = 1000`, tối đa`A`và cực đoan`S`giá trị |`10000 2000 1000000`| Ràng buộc tối đa và số học lớn | 

## Vỏ cạnh 

Khi nào`S = P`, cửa hàng bán từng đơn vị. Vì```
1 1004 100
```điều kiện`S >= P`là đúng, do đó thuật toán đặt`purchases = 4`và đầu ra`400`. Không cần phải tạo một gói trong trường hợp này. 

Khi`S`không chia`P`, việc chia trần là cần thiết. Vì```
1 1007 30
```kích thước gói là`(100 + 30 - 1) // 30 = 4`. Hai gói chứa tám đơn vị, đủ cho bảy đơn vị cần thiết, vì vậy câu trả lời là`200`. Bộ phận phân tầng sẽ tính toán không chính xác kích thước gói hàng là ba. 

Khi`A`nhỏ hơn kích thước gói, vẫn cần có một gói nguyên. Vì```
1 1001 40
```gói chứa ba đơn vị, nhưng chỉ yêu cầu một đơn vị. Vì các gói không thể được chia nhỏ,`ceil(1 / 3) = 1`, đưa ra chi phí là`100`. 

Khi số lượng yêu cầu chia hết cho kích thước gói hàng thì bộ phận trần thứ 2 không được thêm gói hàng không cần thiết. Vì```
1 1006 50
```một gói chứa`2`đơn vị và`6 / 2 = 3`chính xác. Thuật toán tính toán`(6 + 2 - 1) // 2 = 3`, vậy câu trả lời là`300`, không`400`. 

Ranh giới cuối cùng là`S > P`. Vì```
1 1001 101
```sản phẩm được bán dưới dạng một đơn vị riêng lẻ cho`100`mặc dù giá thị trường của nó cao hơn. Thuật toán tuân theo`S >= P`nhánh và đầu ra`100`.
