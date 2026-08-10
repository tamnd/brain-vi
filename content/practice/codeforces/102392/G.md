---
title: "CF 102392G - Trình chiếu"
description: "Đối với tọa độ độ sâu x cố định, phép chiếu đầu tiên cho chúng ta biết vị trí y nào phải chứa ít nhất một khối, trong khi phép chiếu thứ hai cho chúng ta biết vị trí z nào phải chứa ít nhất một khối. Khối lập phương tại (x,y,z) đồng thời tạo ra các ô chiếu (x,y) và (x,z)."
date: "2026-08-10T19:35:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 131
verified: true
draft: false
---

[CF 102392G - Phép chiếu](https://codeforces.com/problemset/problem/102392/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với tọa độ độ sâu x cố định, phép chiếu đầu tiên cho chúng ta biết vị trí y nào phải chứa ít nhất một khối, trong khi phép chiếu thứ hai cho chúng ta biết vị trí z nào phải chứa ít nhất một khối. Khối lập phương tại (x,y,z) đồng thời tạo ra các ô chiếu (x,y) và (x,z). 

Đặt Y x ​ là tập hợp tọa độ y trong đó hình chiếu đầu tiên chứa một`1`trong hàng x và đặt Z x ​ là tập tọa độ z tương ứng từ hình chiếu thứ hai. Một khối ở độ sâu x hữu ích chính xác khi y∈Y x ​ và z∈Z x ​. Do đó, khi đã biết cả hai tập hợp, mọi cặp trong Y x ​ ×Z x ​ đều là một khối hợp pháp. 

Điều này tách hoàn toàn bài toán ba chiều thành n bài toán hai chiều độc lập. Với mỗi x, ta chỉ cần nối mọi đỉnh của Y x ​ với ít nhất một đỉnh của Z x ​. 

Điều kiện khả thi đầu tiên được đưa ra ngay sau đó. Nếu Y x ​ không trống nhưng Z x ​ trống thì không bao giờ có thể tạo ra ô chiếu đầu tiên bắt buộc. Điều tương tự cũng đúng với hai bên đảo ngược. Nếu cả hai bộ đều trống thì lát cắt đó đơn giản là không chứa khối nào và hợp lệ. 

Kích thước nhiều nhất là 100, vì vậy có nhiều nhất 100⋅100⋅100=10 6 hình lập phương có thể có. Giá trị này đủ nhỏ để liệt kê tất cả các khối một lần, nhưng quá lớn để có thể tìm kiếm theo cấp số nhân trên các tập hợp con. Giải pháp cần thiết về cơ bản phải tuyến tính về số lượng khối có thể, nằm trong giới hạn 1 giây trong quá trình triển khai được tối ưu hóa. 

Một trường hợp tinh tế là một lát cắt hoàn toàn trống rỗng. Ví dụ,```
1 2 2
00
00
```không có bóng cần thiết nào cả. Đầu ra đúng là```
0
0
```Việc triển khai bất cẩn cho rằng mỗi lát cần ít nhất một khối có thể từ chối nó một cách không chính xác. 

Một trường hợp quan trọng khác là một lát cắt không nhất quán:```
1 2 1
10
0
```Phép chiếu đầu tiên yêu cầu một khối có y=0, nhưng phép chiếu thứ hai không yêu cầu khối nào cả vì mục nhập duy nhất của nó là 0. Đầu ra đúng là```
-1
```Đang cố gắng xây dựng một khối lập phương từ mọi`1`trong lần chiếu đầu tiên sẽ âm thầm tạo ra một kết quả không mong muốn`1`trong hình chiếu thứ hai. 

Yêu cầu về từ điển cũng có vấn đề. Coi như```
1 3 2
111
11
```Có ba vị trí y bắt buộc và hai vị trí z bắt buộc. Ba khối là cần thiết và đủ. Giải pháp tối thiểu nhỏ nhất về mặt từ điển là```
3
0 0 0
0 1 0
0 2 1
```Một cấu trúc ghép các tọa độ theo đường chéo mà không xem xét thứ tự từ điển có thể tạo ra một nghiệm hợp lệ với cùng số khối nhưng có chuỗi tọa độ lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là xem xét mọi tập hợp con của các khối có thể có nmh, chiếu tập hợp con đó lên hai ma trận và giữ tập hợp con hợp lệ với kích thước tối đa và tập hợp con hợp lệ với kích thước tối thiểu. Điều này đúng vì mọi cách sắp xếp vật lý có thể đều được biểu diễn bằng chính xác một tập hợp con các hình khối. Tuy nhiên, có tập hợp con 2 nmh. Ở kích thước tối đa, điều này trở thành 2 1.000.000 ứng cử viên và thậm chí việc kiểm tra một ứng cử viên sẽ yêu cầu tới 10 phép toán 6 khối. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là các hình khối có tọa độ x khác nhau không bao giờ tương tác với nhau. Đối với một x cố định, các khối được phép tạo thành một biểu đồ lưỡng cực hoàn chỉnh giữa Y x ​ và Z x ​. Có một cạnh giữa mọi tọa độ y bắt buộc và mọi tọa độ z bắt buộc. 

Để có giải pháp tối đa, mọi cạnh cho phép đều có thể có mặt. Việc thêm bất kỳ khối nào có hai ô chiếu đã được yêu cầu không thể làm mất hiệu lực các phép chiếu, vì vậy nghiệm tối đa chứa chính xác ∣Y x ​ ∣∣Z x ​ ∣ khối trong lát x. 

Để có lời giải tối thiểu, chúng ta cần ít cạnh nhất chạm vào mọi đỉnh của đồ thị hai bên hoàn chỉnh này. Nếu hai cạnh chứa các đỉnh a và b thì mỗi khối chạm vào nhiều nhất một đỉnh ở mỗi cạnh, do đó cần có ít nhất các khối max(a,b). Nhiều như vậy cũng đủ: gán một cạnh cho mỗi đỉnh của cạnh lớn hơn và sử dụng lại các đỉnh từ cạnh nhỏ hơn nếu cần. Do đó, mức tối thiểu của lát x là max(∣Y x ​ ∣,∣Z x ​ ∣). 

Vấn đề còn lại là chọn các cạnh đó theo từ điển. Giả sử a ≥b. Phải có chính xác một cạnh cho mỗi y, vì cần có một cạnh và tất cả các tọa độ y khác nhau phải xuất hiện. Vì đầu ra được sắp xếp theo (x,y,z), tọa độ y buộc phải xuất hiện dưới dạng y 0 ​ ,y 1 ​ ,…,y a−1 ​. Để làm cho chuỗi tọa độ z nhỏ nhất về mặt từ điển trong khi vẫn sử dụng mọi z, chúng ta sử dụng z nhỏ nhất nhiều lần nhất có thể, sau đó chuyển sang cái tiếp theo. Do đó dãy là z 0 ​ lặp lại a−b+1 lần, theo sau là z 1 ​ ,z 2 ​ ,…,z b−1 ​. Trường hợp b>a là đối xứng. 

Phương pháp vũ phu hoạt động vì nó xem xét rõ ràng mọi cách sắp xếp, nhưng không thành công vì số lượng sắp xếp theo cấp số nhân. Việc quan sát rằng mỗi lát cắt là một biểu đồ hai bên hoàn chỉnh sẽ làm giảm vấn đề về việc đếm đơn giản và xây dựng từ điển trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2 nmh ⋅nmh) | O(nmh) | Quá chậm | 
| Tối ưu | O(nmh) | O(n(m+h)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai ma trận chiếu và với mỗi x, lưu trữ tọa độ y và tọa độ z chứa`1`. 

Hai danh sách mô tả đầy đủ mọi thứ có thể xảy ra trong lát cắt đó. Không cần phải lưu trữ khối lượng ba chiều. 
2. Với mỗi x, hãy kiểm tra xem chính xác một trong Y x ​ và Z x ​ có trống không. 

Nếu điều đó xảy ra, các dự đoán sẽ mâu thuẫn với nhau. Một khối lập phương tạo ra vị trí y cần thiết sẽ nhất thiết tạo ra vị trí z nào đó và ngược lại. Nếu việc kiểm tra không thành công đối với bất kỳ x nào, hãy in`-1`. 
3. Để có nghiệm tối đa, hãy lặp qua mọi y∈Y x ​ và mọi z∈Z x ​, rồi xuất ra khối lập phương (x,y,z). 

Mỗi cặp như vậy đều được cho phép và việc thêm tất cả chúng vẫn tạo ra cùng một phép chiếu vì cả hai điểm cuối đều đã được đánh dấu là`1`. Vì mọi khối hợp lệ có thể được bao gồm nên số khối là tối đa. 
4. Đối với nghiệm tối thiểu, đặt a=∣Y x ​ ∣, b=∣Z x ​ ∣ và k=max(a,b). 

Cần ít nhất k khối vì mỗi khối có thể đưa vào nhiều nhất một tọa độ y mới và nhiều nhất một tọa độ z mới. Tồn tại một cấu trúc có k khối nên k là tối ưu. 
5. Tạo nghiệm tối thiểu bằng cách lặp i từ k xuống 1. Sử dụng 

y[max(0,a−i)] 

và 

z[max(0,b−i)]. 

Khi một bên lớn hơn, tọa độ của nó sẽ di chuyển qua toàn bộ danh sách trong khi bên nhỏ hơn lặp lại tọa độ nhỏ nhất cho đến khi mọi đỉnh ở phía lớn hơn đều được bao phủ. Bởi vì các danh sách được lưu trữ theo thứ tự tăng dần nên các bộ ba được tạo ra đã được sắp xếp theo từ điển. 
6. Xử lý x theo thứ tự tăng dần và từng tọa độ chiếu được lưu theo thứ tự tăng dần. 

Điều này làm cho đầu ra hoàn chỉnh được sắp xếp theo thứ tự từ điển, điều này được yêu cầu bởi câu lệnh. Vì các lát cắt x khác nhau là độc lập nên việc làm cho mỗi lát cắt nhỏ nhất về mặt từ điển cũng làm cho giải pháp được nối nhỏ nhất về mặt từ điển. 

### Tại sao nó hoạt động 

Đối với mọi x cố định, các ô chiếu cần thiết sẽ xác định một biểu đồ lưỡng cực hoàn chỉnh giữa Y x ​ và Z x ​. Một tập hợp khối hợp lệ chính xác là một tập cạnh chạm vào mọi đỉnh. Nếu một trong hai bên trống trong khi bên kia thì không, tập cạnh đó không thể tồn tại. Ngược lại, tất cả các cạnh ab đều hợp lệ, cho nghiệm tối đa. 

Đối với giải pháp tối thiểu, cần có ít nhất các cạnh max(a,b) vì một cạnh có thể bao phủ nhiều nhất một đỉnh ở mỗi cạnh. Việc xây dựng sử dụng chính xác nhiều cạnh và bao phủ mọi đỉnh, vì vậy nó là tối ưu. Trong số tất cả các nghiệm như vậy, nếu a ≥ b thì mỗi y phải xảy ra đúng một lần. Do đó, trật tự của họ được cố định. Z nhỏ nhất có thể được chọn ở mọi vị trí mà làm như vậy vẫn để lại đủ vị trí để đưa vào tất cả tọa độ z còn lại. Điều này mang lại chuỗi z nhỏ nhất có thể, chứng tỏ tính tối ưu về mặt từ điển. Đối số đối xứng được áp dụng khi b>a. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, h = map(int, input().split())

    ys = [[] for _ in range(n)]
    zs = [[] for _ in range(n)]

    for x in range(n):
        row = input().strip()
        for y, ch in enumerate(row):
            if ch == '1':
                ys[x].append(y)

    for x in range(n):
        row = input().strip()
        for z, ch in enumerate(row):
            if ch == '1':
                zs[x].append(z)

    for x in range(n):
        if bool(ys[x]) != bool(zs[x]):
            print(-1)
            return

    out = []

    # Maximum solution.
    kmax = 0
    for x in range(n):
        kmax += len(ys[x]) * len(zs[x])

    out.append(str(kmax))

    for x in range(n):
        for y in ys[x]:
            for z in zs[x]:
                out.append(f"{x} {y} {z}")

    # Minimum solution.
    kmin = 0
    for x in range(n):
        kmin += max(len(ys[x]), len(zs[x]))

    out.append(str(kmin))

    for x in range(n):
        a = len(ys[x])
        b = len(zs[x])
        k = max(a, b)

        for i in range(k, 0, -1):
            y = ys[x][max(0, a - i)]
            z = zs[x][max(0, b - i)]
            out.append(f"{x} {y} {z}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào được lưu dưới dạng hai danh sách trên mỗi x. MỘT`1`trong phép chiếu đầu tiên trở thành một mục trong`ys[x]`, trong khi một`1`trong cái thứ hai trở thành một mục trong`zs[x]`. Các ma trận ban đầu không cần lưu lại trong bộ nhớ sau khi các danh sách này được xây dựng. 

Việc kiểm tra tính khả thi sẽ so sánh xem hai danh sách có trống hay không bằng cách sử dụng`bool`. Sự bằng nhau của hai giá trị Boolean này chính xác là điều kiện cả hai tập chiếu đều trống hoặc cả hai đều khác trống. 

Số lượng tối đa sử dụng tích của hai kích thước danh sách. Sau đó, các vòng lặp lồng nhau sẽ liệt kê từng cặp theo y tăng dần, tiếp theo là tăng z. Điều này tạo ra bộ ba theo thứ tự từ điển vì x đã được xử lý từ nhỏ đến lớn. 

Đối với việc xây dựng tối thiểu, biểu thức`max(0, a - i)`là hoạt động ranh giới quan trọng. Khi a nhỏ hơn b, chỉ số vẫn bằng 0 trong vài lần lặp đầu tiên, do đó y nhỏ nhất được lặp lại. Khi chỉ số lớn hơn đến cuối danh sách nhỏ hơn, tất cả tọa độ cạnh nhỏ hơn đã được sử dụng. Biểu thức tương tự xử lý sự mất cân bằng ngược lại mà không có nhánh riêng biệt. 

Số nguyên Python có độ chính xác tùy ý, do đó số lượng tối đa, nhiều nhất là 10 6, không cần xử lý số nguyên đặc biệt. Đầu ra được tích lũy trong một danh sách và được ghi một lần, tránh chi phí cho hàng triệu đầu ra riêng biệt.`print`cuộc gọi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên, các hàng chiếu tạo ra các kích thước lát cắt sau. 

| x | Y x | Z x | Tối đa | Tối thiểu | 
| --- | --- | --- | --- | --- | 
| 0 | 0,1,2 | 0,1,2 | 9 | 3 | 
| 1 | 1 | 0 | 1 | 1 | 
| 2 | 1 | 0,1 | 2 | 2 | 
| 3 | 1 | 0 | 1 | 1 | 
| 4 | 1 | 0 | 1 | 1 | 

Số lượng tối đa là 

9+1+2+1+1=14. 

Với x=0, cả hai bên đều có kích thước bằng 3, do đó việc xây dựng tối thiểu sử dụng```
(0,0,0)
(0,1,1)
(0,2,2)
```Với x=2, kích thước là một và hai, do đó tọa độ y nhỏ hơn được lặp lại:```
(2,1,0)
(2,1,1)
```Việc xây dựng tối thiểu hoàn chỉnh có tám hình khối. 

| x | một | b | k | Đã tạo cặp tối thiểu | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 3 | (0,0), (1,1), (2,2) | 
| 1 | 1 | 1 | 1 | (1,0) | 
| 2 | 1 | 2 | 2 | (1,0), (1,1) | 
| 3 | 1 | 1 | 1 | (1,0) | 
| 4 | 1 | 1 | 1 | (1,0) | 

Điều này thể hiện cả trường hợp có kích thước bằng nhau và trường hợp một phép chiếu chứa nhiều tọa độ cần thiết hơn phép chiếu kia. 

### Mẫu 2 

Mẫu thứ hai là```
2 2 2
00
00
11
11
```Phép chiếu đầu tiên không chứa`1`ở một trong hai hàng, vì vậy cả Y 0 ​ và Y 1 ​ đều trống. Phép chiếu thứ hai chứa hai tọa độ z bắt buộc trong mỗi hàng. 

| x | Y x | Z x | Khả thi | 
| --- | --- | --- | --- | 
| 0 | trống | 0,1 | không | 
| 1 | trống | 0,1 | không | 

Lát cắt đầu tiên đã có Y x ​ trống và Z x ​ không trống. Một khối lập phương không thể tạo ra một`1`trong phép chiếu thứ hai mà không đồng thời tạo ra một`1`trong phép chiếu đầu tiên nên kết quả đúng là ngay lập tức`-1`. 

Dấu vết này thực hiện kiểm tra tính nhất quán trước khi bắt đầu xây dựng tối đa hoặc tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nmh) | Đọc các dự báo có chi phí O(nm+nh), liệt kê chi phí giải pháp tối đa tối đa là O(nmh) và xây dựng chi phí giải pháp tối thiểu O(n(m+h)). | 
| Không gian | O(n(m+h)+nmh) | Danh sách phép chiếu sử dụng không gian O(n(m+h)) và bộ đệm đầu ra có thể chứa bộ ba O(nmh). | 

Bản thân đầu ra lớn nhất có thể chứa 10 6 khối, do đó, mọi triển khai được chấp nhận đều phải dành ít nhất thời gian tuyến tính cho kích thước đầu ra trong trường hợp xấu nhất. Thuật toán chỉ thực hiện công việc không đổi trên mỗi khối đầu ra và tránh khớp, luồng, lập trình động hoặc liệt kê theo cấp số nhân. Giới hạn bộ nhớ 512 MB là đủ cho danh sách chiếu và bộ đệm đầu ra. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, h = map(int, input().split())

    ys = [[] for _ in range(n)]
    zs = [[] for _ in range(n)]

    for x in range(n):
        row = input().strip()
        for y, ch in enumerate(row):
            if ch == '1':
                ys[x].append(y)

    for x in range(n):
        row = input().strip()
        for z, ch in enumerate(row):
            if ch == '1':
                zs[x].append(z)

    for x in range(n):
        if bool(ys[x]) != bool(zs[x]):
            print(-1)
            return

    out = []

    kmax = sum(len(ys[x]) * len(zs[x]) for x in range(n))
    out.append(str(kmax))

    for x in range(n):
        for y in ys[x]:
            for z in zs[x]:
                out.append(f"{x} {y} {z}")

    kmin = sum(max(len(ys[x]), len(zs[x])) for x in range(n))
    out.append(str(kmin))

    for x in range(n):
        a = len(ys[x])
        b = len(zs[x])
        k = max(a, b)

        for i in range(k, 0, -1):
            y = ys[x][max(0, a - i)]
            z = zs[x][max(0, b - i)]
            out.append(f"{x} {y} {z}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample1_in = """\
5 3 3
111
010
010
010
010
111
100
110
100
100
"""

sample1_out = """\
14
0 0 0
0 0 1
0 0 2
0 1 0
0 1 1
0 1 2
0 2 0
0 2 1
0 2 2
1 1 0
2 1 0
2 1 1
3 1 0
4 1 0
8
0 0 0
0 1 1
0 2 2
1 1 0
2 1 0
2 1 1
3 1 0
4 1 0
"""

sample2_in = """\
2 2 2
00
00
11
11
"""

sample2_out = """\
-1
"""

sample3_in = """\
2 3 2
101
011
10
11
"""

sample3_out = """\
6
0 0 0
0 2 0
1 1 0
1 1 1
1 2 0
1 2 1
4
0 0 0
0 2 0
1 1 0
1 2 1
"""

assert run(sample1_in) == sample1_out, "sample 1"
assert run(sample2_in) == sample2_out, "sample 2"
assert run(sample3_in) == sample3_out, "sample 3"

assert run("""\
1 1 1
1
1
""") == """\
1
0 0 0
1
0 0 0
""", "minimum-size nonempty case"

assert run("""\
2 2 2
00
00
00
00
""") == """\
0
0
""", "all-zero projections"

assert run("""\
1 2 1
10
0
""") == """\
-1
""", "inconsistent projections"

assert run("""\
1 2 3
11
111
""") == """\
6
0 0 0
0 0 1
0 0 2
0 1 0
0 1 1
0 1 2
3
0 0 0
0 0 1
0 1 2
""", "unequal sides"

assert run(
    "100 100 100\n" +
    ("0" * 100 + "\n") * 100 +
    ("0" * 100 + "\n") * 100
) == "0\n0", "maximum dimensions with all zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`, cả hai hình chiếu`1`| Một khối giống hệt nhau trong mỗi giải pháp | Kích thước tối thiểu và khối lượng không trống nhỏ nhất có thể | 
|`2 2 2`, tất cả số không |`0`hình khối cho cả hai giải pháp | Khối lượng trống và xử lý lát cắt có kích thước bằng 0 | 
|`1 2 1`,`10`so với`0`|`-1`| Kiểm tra tính nhất quán của phép chiếu | 
|`1 2 3`,`11`so với`111`| Tối đa 6, tối thiểu 3 | Kích thước hình chiếu không đồng đều và sự lặp lại từ điển | 
|`100 100 100`, tất cả số không |`0`Và`0`| Kích thước tối đa, phân tích cú pháp đầu vào lớn và các giá trị hoàn toàn bằng nhau | 

## Vỏ cạnh 

### Một lát cắt hoàn toàn trống 

cho```
1 2 2
00
00
```chúng ta có Y 0 ​ =∅ và Z 0 ​ =∅. Việc kiểm tra tính khả thi chấp nhận lát cắt vì cả hai bên đều trống. Đóng góp của nó tới mức tối đa là 0⋅0=0 và đóng góp của nó đến mức tối thiểu là max(0,0)=0. Đầu ra là```
0
0
```Không có khối nhân tạo nào được đưa vào, điều này là cần thiết vì bất kỳ khối nào cũng sẽ tạo ra các ô chiếu không mong muốn. 

### Yêu cầu chỉ chiếu một bên 

cho```
1 2 1
10
0
```chúng ta có Y 0 ​ ={0} và Z 0 ​ =∅. Các giá trị trống Boolean khác nhau nên thuật toán in ra`-1`trước khi xây dựng bất cứ điều gì. Điều này ngăn ngừa sai lầm phổ biến khi phát minh ra tọa độ z có thể làm thay đổi hình chiếu thứ hai. 

### Nhiều tọa độ y hơn tọa độ z 

cho```
1 3 2
111
11
```chúng ta có Y 0 ​ =(0,1,2) và Z 0 ​ =(0,1). Giới hạn dưới cho biết cần ít nhất ba khối vì có ba tọa độ y riêng biệt. Việc xây dựng sử dụng```
0 0 0
0 1 0
0 2 1
```Hai khối đầu tiên sử dụng lại tọa độ z nhỏ nhất vì làm như vậy về mặt từ điển sẽ tốt hơn so với sử dụng z=1 trước đó. Khối cuối cùng giới thiệu z=1 còn thiếu. Cả ba tọa độ y và cả tọa độ z đều được che phủ. 

### Nhiều tọa độ z hơn tọa độ y 

cho```
1 2 3
11
111
```chúng ta có Y 0 ​ =(0,1) và Z 0 ​ =(0,1,2). Ba khối lập phương là cần thiết vì có ba tọa độ z riêng biệt. Việc xây dựng tối thiểu là```
0 0 0
0 0 1
0 1 2
```Ở đây y=0 được lặp lại vì cạnh y nhỏ hơn. Tọa độ z được đưa vào theo thứ tự tăng dần, tạo ra chuỗi nhỏ nhất có thể về mặt từ điển. 

### Kích thước đầu ra tối đa 

Khi n=m=h=100 và mọi ô chiếu đều`1`, mỗi một trong 10 6 lập phương có thể có đều thuộc nghiệm tối đa. Thuật toán không cố gắng lưu trữ cấu trúc Boolean ba chiều hoặc tìm kiếm thông qua các cấu hình. Nó chỉ đơn giản liệt kê hàng triệu cặp hợp pháp. Giải pháp tối thiểu chỉ chứa 100⋅100=10 4 khối vì mỗi một trong số 100 lát cắt yêu cầu tối đa(100.100)=100 khối.
