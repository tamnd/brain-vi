---
title: "CF 102168E - \u041a\u0443\u0431\u0438\u043a\u0438"
description: "Chúng ta có một hộp hình chữ nhật gồm các khối lập phương đơn vị có kích thước x × y × z. Một khối lập phương được xác định bởi tọa độ (x, y, z). Ba mảng hai chiều mô tả vị trí nào được hiển thị trong ba hình chiếu tọa độ."
date: "2026-08-19T07:22:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "E"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 138
verified: true
draft: false
---

[CF 102168E - \u041a\u0443\u0431\u0438\u043a\u0438](https://codeforces.com/problemset/problem/102168/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một hình hộp chữ nhật gồm các hình lập phương có kích thước`x × y × z`. Một khối được xác định bởi tọa độ`(x, y, z)`. Ba mảng hai chiều mô tả vị trí nào được hiển thị trong ba hình chiếu tọa độ. 

Hình chiếu bên trái có kích thước`z × y`, Vì thế`left[z][y]`là`#`chính xác khi có ít nhất một khối với những khối đó được cố định`y`Và`z`tọa độ tồn tại. Hình chiếu phía trước có kích thước`z × x`, Vì thế`front[z][x]`là`#`khi một số khối với những cái đó`x`Và`z`tọa độ tồn tại. Hình chiếu trên cùng có kích thước`y × x`, Vì thế`top[y][x]`là`#`khi một số khối với những cái đó`x`Và`y`tọa độ tồn tại. 

Chúng ta phải xây dựng lại một hình ba chiều thực tế có ba hình chiếu chính xác là các mảng đã cho. Trong số tất cả những hình như vậy, chúng ta muốn một hình chứa càng nhiều hình khối càng tốt. 

Đối với một vị trí cụ thể`(x, y, z)`, một khối lập phương chỉ có thể tồn tại nếu cả ba ô chiếu tương ứng đều`#`. Nếu thậm chí một trong số họ là`.`, việc đặt một khối lập phương vào đó sẽ ngay lập tức làm cho hình chiếu đó chứa một phần không mong muốn`#`. 

Kích thước tối đa là`100`, vậy có nhiều nhất`100^3 = 1,000,000`vị trí khối có thể. Việc vượt qua tuyến tính trên tất cả các vị trí là điều dễ dàng thực hiện được. Điều này cũng có nghĩa là chúng ta nên tránh các thuật toán liệt kê các tập hợp con của các khối hoặc khám phá nhiều cấu hình theo cấp số nhân. Ngay cả phép tính bậc hai trên toàn bộ không gian ba chiều cũng đã tốn kém một cách không cần thiết. 

Các trường hợp khó khăn chính xuất phát từ việc nhầm lẫn giữa điều kiện cần và điều kiện đủ. Ví dụ, hãy xem xét```
2 2 1#..#
.##.
#...
```duy nhất`#`trong hình chiếu bên trái là tại`(y=0,z=0)`, do đó một khối lập phương nhận ra rằng nó phải có`y=0`. duy nhất`#`trong hình chiếu phía trước yêu cầu`x=1`, trong khi duy nhất`#`trong hình chiếu trên cùng tại`y=0`yêu cầu`x=0`. Không có khối lập phương nào có thể thỏa mãn cả ba yêu cầu trên, vì vậy câu trả lời đúng là`NO`. Một giải pháp bất cẩn chỉ kiểm tra xem mọi phép chiếu có chứa một số`#`sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp cạnh khác là một ô chiếu được`#`nhưng không có khối tương thích mặc dù có thể đặt một số khối khác. Ví dụ,```
2 2 1##..
.#..
#...
```Phép chiếu bên trái yêu cầu cả hai`y=0`Và`y=1`để chứa một khối lập phương. Hình chiếu phía trước chỉ cho phép`x=1`, trong khi hình chiếu trên cùng chỉ cho phép`x=0`Tại`y=0`. các`y=0`ô bên trái không thể được nhận ra, vì vậy câu trả lời là`NO`. Một công trình chỉ lấp đầy mọi vị trí được cho phép bởi hai trong số các hình chiếu có thể âm thầm tạo ra hình chiếu thứ ba sai. 

Ở thái cực đối lập, khi cả ba hình chiếu đều bao gồm toàn bộ`#`, mỗi một trong số`x*y*z`có thể lấp đầy các vị trí. Câu trả lời là hộp hoàn toàn đầy đủ, đạt số khối tối đa có thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là xem xét mọi hình ba chiều có thể có và kiểm tra các hình chiếu của nó. có`x*y*z`các vị trí khối lập phương có thể có, vì vậy số lượng các hình khác nhau là`2^(x*y*z)`. Trong trường hợp lớn nhất đây là`2^1,000,000`cấu hình có thể, vượt xa mọi thứ có thể được xử lý. 

Một cách tiếp cận ngây thơ hữu ích hơn là kiểm tra mọi khối có thể có và quyết định xem nó có tương thích với ba hình chiếu hay không. Điều đó đã gợi ý quan sát quan trọng. Đối với một khối lập phương`(x, y, z)`, sự hiện diện của nó được phép chính xác khi```
left[z][y] = '#'front[z][x] = '#'top[y][x] = '#'
```Giả sử tất cả các khối được phép như vậy được đặt. Công trình này không thể giới thiệu một`#`vào bất kỳ ô chiếu nào ban đầu`.`, bởi vì mọi khối được đặt đều được yêu cầu rõ ràng phải phù hợp với cả ba hình chiếu. 

Câu hỏi còn lại là liệu mỗi bản gốc`#`thực sự được đại diện bởi ít nhất một khối lập phương được đặt. Chúng tôi có thể trả lời điều đó trong cùng một lần quét. Bất cứ khi nào một vị trí thỏa mãn cả ba điều kiện, chúng tôi đánh dấu ba ô chiếu của nó là được che phủ. Sau khi xử lý xong tất cả`x*y*z`các vị trí, mọi`#`trong mọi hình chiếu đều phải được che đậy. Nếu một số`#`vẫn chưa được khám phá, không có hình hợp lệ nào có thể tồn tại, bởi vì mọi khối có khả năng che phủ ô chiếu đó cũng sẽ phải thỏa mãn hai hình chiếu còn lại. 

Quan sát tương tự cũng chứng minh tính cực đại. Mỗi khối trong bất kỳ giải pháp hợp lệ nào đều phải thuộc tập hợp các vị trí tương thích với cả ba hình chiếu. Công trình của chúng tôi chứa đựng mọi vị trí như vậy. Vì vậy không có hình hợp lệ nào khác có thể chứa nhiều hình khối hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các số liệu |`O(2^(xyz))`hoặc tệ hơn | Hàm mũ | Quá chậm | 
| Kiểm tra mọi hình khối và che các hình chiếu |`O(xyz)`|`O(xy + xz + yz)`ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba mảng chiếu. Bỏ qua các dòng phân cách trống vì các hàng chiếu thực tế chỉ chứa`#`Và`.`. 
2. Phân bổ ba mảng bao phủ boolean có hình dạng giống như hình chiếu đầu vào.`covered_left[z][y]`có nghĩa là chúng ta đã tìm thấy một khối hợp lệ có hình chiếu bao phủ ô bên trái đó. Hai mảng còn lại có ý nghĩa tương tự. 
3. Lặp lại mọi vị trí khối có thể`(z, y, x)`. Một khối lập phương là một ứng cử viên chính xác khi`left[z][y]`,`front[z][x]`, Và`top[y][x]`là tất cả`#`. 
4. Đối với mỗi khối ứng cử viên, hãy đánh dấu ba ô chiếu của nó là được che phủ. Bản thân ứng cử viên cũng là một phần của câu trả lời cuối cùng, bởi vì việc thêm nó không thể làm hỏng bất kỳ phép chiếu nào. 
5. Sau khi quét, hãy kiểm tra mọi`#`trong mỗi hình chiếu. Nếu bất kỳ ô nào như vậy không được che phủ, hãy in`NO`. Không có vị trí thay thế nào có thể khắc phục được nó, bởi vì mọi khối có thể bao phủ ô đó đều đã được kiểm tra và loại bỏ bởi ít nhất một trong các phép chiếu khác. 
6. Nếu tất cả`#`các tế bào được bao phủ, in`YES`. Tạo lại mọi ô đầu ra bằng cách sử dụng cùng một điều kiện tương thích. Một vị trí là`#`chính xác khi cả ba ô chiếu tương ứng đều`#`; nếu không thì nó là`.`. 
7. In các lớp tăng dần`z`theo thứ tự, với một dòng trống giữa các lớp liên tiếp. Điều này phù hợp với định dạng lớp được yêu cầu. 

### Tại sao nó hoạt động 

Hãy xem xét tập hợp`C`của tất cả các vị trí khối có ba ô chiếu`#`. Mọi hình hợp lệ chỉ có thể chứa các hình khối từ`C`, bởi vì một khối lập phương bên ngoài`C`sẽ tạo ra một`#`ở vị trí chiếu được cho là trống. Công trình của chúng tôi chứa mọi khối lập phương trong`C`, do đó nó chứa ít nhất nhiều hình khối bằng bất kỳ hình hợp lệ nào. 

Việc xây dựng tạo ra các phép chiếu chính xác một cách chính xác khi mọi đầu vào`#`thuộc về ít nhất một khối trong`C`. Mảng phủ sóng kiểm tra chính xác tình trạng này. Nếu mỗi`#`được che phủ, mọi ô chiếu cần thiết đều được tạo ra, đồng thời không bị cấm`.`có thể được tạo ra vì tất cả các hình khối được đặt đều thuộc về`C`. Nếu một số`#`không được bao phủ, không có hình hợp lệ nào tồn tại vì không có khối tương thích nào có khả năng tạo ra nó. Điều này chứng tỏ cả tính khả thi và tính tối đa. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    x, y, z = map(int, input().split())
    # Empty lines are separators between projections.    lines = [line.strip() for line in sys.stdin if line.strip()]
    pos = 0
    left = lines[pos:pos + z]    pos += z
    front = lines[pos:pos + z]    pos += z
    top = lines[pos:pos + y]
    covered_left = [[False] * y for _ in range(z)]    covered_front = [[False] * x for _ in range(z)]    covered_top = [[False] * x for _ in range(y)]
    # Find every cube that is compatible with all three projections.    for zz in range(z):        for yy in range(y):            if left[zz][yy] != '#':                continue
            for xx in range(x):                if front[zz][xx] != '#':                    continue                if top[yy][xx] != '#':                    continue
                covered_left[zz][yy] = True                covered_front[zz][xx] = True                covered_top[yy][xx] = True
    # Every '#' in every projection must be represented.    for zz in range(z):        for yy in range(y):            if left[zz][yy] == '#' and not covered_left[zz][yy]:                print("NO")                return
    for zz in range(z):        for xx in range(x):            if front[zz][xx] == '#' and not covered_front[zz][xx]:                print("NO")                return
    for yy in range(y):        for xx in range(x):            if top[yy][xx] == '#' and not covered_top[yy][xx]:                print("NO")                return
    print("YES")
    # Every compatible cube is present in the maximum construction.    for zz in range(z):        for yy in range(y):            row = []            for xx in range(x):                if (                    left[zz][yy] == '#'                    and front[zz][xx] == '#'                    and top[yy][xx] == '#'                ):                    row.append('#')                else:                    row.append('.')            print(''.join(row))
        if zz + 1 < z:            print()

if __name__ == "__main__":    solve()
```Phần đầu tiên của quá trình triển khai sẽ đọc tất cả các dòng không trống sau các kích thước. Điều này thuận tiện vì đầu vào phân tách rõ ràng ba hình chiếu bằng các dòng trống, nhưng các dấu phân cách đó không mang thông tin. 

Ba mảng bao phủ chỉ lưu trữ thông tin chiếu chứ không phải toàn bộ hình ba chiều. Tổng kích thước của chúng là`xy + xz + yz`, nhỏ hơn nhiều so với khả năng`xyz`vị trí khối và đã đủ để xác định xem mọi ô chiếu được yêu cầu đã được thực hiện hay chưa. 

Các vòng lặp lồng nhau thực hiện trực tiếp điều kiện trung tâm. Các chỉ số được cố tình viết là`zz`,`yy`, Và`xx`vì vậy mối quan hệ của chúng với ba hình chiếu vẫn còn rõ ràng. Phép chiếu bên trái sử dụng`(zz, yy)`, hình chiếu phía trước sử dụng`(zz, xx)`và phép chiếu trên cùng sử dụng`(yy, xx)`. 

Không cần phải lưu trữ hình được xây dựng. Sau khi đã thiết lập tính khả thi, ba điều kiện tương tự có thể được đánh giá lại trong khi in. Điều này giúp việc triển khai đơn giản và tránh phân bổ cấu trúc ba chiều gồm một triệu phần tử khác. 

Việc kiểm tra mức độ phù hợp là riêng biệt đối với ba dự đoán vì nếu một trong số chúng bị lỗi sẽ khiến toàn bộ quá trình tái thiết không thể thực hiện được. Không có vấn đề tràn số nguyên trong Python và vòng lặp lớn nhất chỉ chứa một triệu lần lặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với mẫu đầu tiên, kích thước là`x=4`,`y=3`,`z=2`. Lớp đầu tiên có các hàng tương thích với phép chiếu sau:```
#####.#####.
```Lớp thứ hai là```
####....###.
```Thuật toán tiếp cận từng`#`trong các hình chiếu thông qua ít nhất một khối tương thích. 

|`z`|`y`|`x`| Trái | Mặt trận | Đầu trang | Khối lập phương | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | # | # | # | # | 
| 0 | 0 | 1 | # | # | # | # | 
| 0 | 0 | 2 | # | # | # | # | 
| 0 | 0 | 3 | # | # | # | # | 
| 0 | 1 | 0 | # | # | # | # | 
| 0 | 1 | 1 | # | . | # | . | 
| 0 | 1 | 2 | # | # | # | # | 
| 0 | 1 | 3 | # | # | # | # | 
| 0 | 2 | 0 | # | # | # | # | 
| 0 | 2 | 1 | # | # | # | # | 
| 0 | 2 | 2 | # | # | # | # | 
| 0 | 2 | 3 | # | # | . | . | 

Thử nghiệm tương tự được thực hiện cho`z=1`. Mọi ô chiếu được yêu cầu sẽ bị che phủ, vì vậy câu trả lời là`YES`. Việc lấp đầy mọi vị trí tương thích sẽ tạo ra con số tối đa có thể. 

### Mẫu 2 

Đối với tất cả-`#` `2 × 2 × 2`Ví dụ, mọi khối đều tương thích với mọi ô chiếu. 

|`z`| Vị trí ứng viên | Che tế bào bên trái | Các ô phía trước có mái che | Các ô trên cùng được che phủ | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | tất cả 2 | tất cả 2 | cả 4 | 
| 1 | 4 | tất cả 2 | tất cả 2 | cả 4 | 

Có tám khối tương thích, vì vậy tất cả tám khối đều được đặt. Đầu ra bao gồm hai lớp, mỗi lớp chứa```
####
```Ví dụ thể hiện đặc tính cực đại một cách đặc biệt rõ ràng. Vì không có phép chiếu nào chứa`.`, không có lý do gì để để trống bất kỳ khối tương thích nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(xyz)`| Mọi vị trí khối có thể được kiểm tra một lần, sau đó là`O(xy + xz + yz)`kiểm tra chiếu và đầu ra. | 
| Không gian |`O(xy + xz + yz)`| Các phép chiếu đầu vào và ba mảng phủ sóng được lưu trữ; hình ba chiều được tạo trực tiếp trong quá trình xuất. | 

Với`x,y,z <= 100`, vòng lặp chính thực hiện tối đa một triệu lần kiểm tra khối. Bản thân đầu ra được tạo có thể chứa khoảng một triệu ký tự, do đó thời gian chạy tỷ lệ thuận với lượng dữ liệu mà chương trình có thể cần in. 

## Trường hợp thử nghiệm 

Bởi vì việc xây dựng lại hợp lệ không nhất thiết phải là duy nhất nên khai thác thử nghiệm không nên so sánh tùy ý`YES`xuất ra ký tự cho ký tự. Trình trợ giúp bên dưới xác thực hình được trả về dựa trên ba hình chiếu và cũng kiểm tra xem hình có số khối tối đa hay không.```python
Python# helper: run solution on input string, return output stringimport sysimport io

def solve():    x, y, z = map(int, input().split())
    lines = [line.strip() for line in sys.stdin if line.strip()]    pos = 0
    left = lines[pos:pos + z]    pos += z
    front = lines[pos:pos + z]    pos += z
    top = lines[pos:pos + y]
    covered_left = [[False] * y for _ in range(z)]    covered_front = [[False] * x for _ in range(z)]    covered_top = [[False] * x for _ in range(y)]
    for zz in range(z):        for yy in range(y):            if left[zz][yy] != '#':                continue            for xx in range(x):                if front[zz][xx] == '#' and top[yy][xx] == '#':                    covered_left[zz][yy] = True                    covered_front[zz][xx] = True                    covered_top[yy][xx] = True
    for zz in range(z):        for yy in range(y):            if left[zz][yy] == '#' and not covered_left[zz][yy]:                print("NO")                return
    for zz in range(z):        for xx in range(x):            if front[zz][xx] == '#' and not covered_front[zz][xx]:                print("NO")                return
    for yy in range(y):        for xx in range(x):            if top[yy][xx] == '#' and not covered_top[yy][xx]:                print("NO")                return
    print("YES")
    for zz in range(z):        for yy in range(y):            print(''.join(                '#'                if left[zz][yy] == '#'                and front[zz][xx] == '#'                and top[yy][xx] == '#'                else '.'                for xx in range(x)            ))        if zz + 1 < z:            print()

def run(inp: str) -> str:    global input
    old_stdin = sys.stdin    old_stdout = sys.stdout    old_input = input
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()    input = sys.stdin.readline
    try:        solve()        return sys.stdout.getvalue()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout        input = old_input

def parse_result(inp: str, out: str):    data = [line.strip() for line in inp.splitlines() if line.strip()]    x, y, z = map(int, data[0].split())
    p = 1    left = data[p:p + z]    p += z    front = data[p:p + z]    p += z    top = data[p:p + y]
    out_lines = out.splitlines()    assert out_lines, "empty output"
    if out_lines[0] == "NO":        return False, None, (left, front, top, x, y, z)
    assert out_lines[0] == "YES"
    figure = []    p = 1
    for zz in range(z):        layer = []        for yy in range(y):            row = out_lines[p]            p += 1            assert len(row) == x            assert all(c in ".#" for c in row)            layer.append(row)        figure.append(layer)
        if zz + 1 < z:            assert out_lines[p] == ""            p += 1
    return True, figure, (left, front, top, x, y, z)

def validate(inp: str, out: str) -> bool:    ok, figure, info = parse_result(inp, out)    left, front, top, x, y, z = info
    expected_exists = True
    for zz in range(z):        for yy in range(y):            if left[zz][yy] == '#':                if not any(                    figure[zz][yy][xx] == '#'                    for xx in range(x)                ):                    expected_exists = False
    for zz in range(z):        for xx in range(x):            if front[zz][xx] == '#':                if not any(                    figure[zz][yy][xx] == '#'                    for yy in range(y)                ):                    expected_exists = False
    for yy in range(y):        for xx in range(x):            if top[yy][xx] == '#':                if not any(                    figure[zz][yy][xx] == '#'                    for zz in range(z)                ):                    expected_exists = False
    if not ok:        return not expected_exists
    # The construction must contain exactly every position compatible    # with all three projections.    for zz in range(z):        for yy in range(y):            for xx in range(x):                allowed = (                    left[zz][yy] == '#'                    and front[zz][xx] == '#'                    and top[yy][xx] == '#'                )                assert (figure[zz][yy][xx] == '#') == allowed
    return expected_exists

# Provided Sample 1.sample1 = """\4 3 2####.##############.#####."""
out = run(sample1)assert out == """\YES#####.#####.####....###.""", "sample 1"

# Provided NO sample, reconstructed from the three projection groups# shown in the statement.sample_no = """\3 3 3#...#...#.#...##....##...#."""
assert run(sample_no).strip() == "NO", "provided NO sample"

# Minimum-size valid instance.minimum = """\1 1 1#
#
#"""
assert run(minimum) == """\YES#""", "minimum valid instance"

# Minimum-size impossible instance. At least one projection requests# a cube, but the three requests cannot refer to the same cube.minimum_no = """\1 1 1.
#
#"""
assert run(minimum_no).strip() == "NO", "minimum impossible instance"

# Boundary case where every projection is full.all_full = """\2 2 2####
####
####"""
assert validate(all_full, run(all_full)), "all projections full"

# A compatibility conflict: every projection has '#', but no cube can# satisfy all three projections simultaneously.conflict = """\2 2 1#...
.#..
#..."""
assert run(conflict).strip() == "NO", "incompatible projections"

# A larger boundary case with one compatible cube and many empty cells.single_cube = """\3 3 2#........
.#.......
.#......."""
assert validate(single_cube, run(single_cube)), "single compatible cube"
```Xác nhận đầu tiên so sánh kết quả đầu ra hoàn chỉnh vì việc xây dựng xác định cho mẫu đầu tiên có kết quả duy nhất trong quá trình triển khai này. Các thử nghiệm tích cực còn lại sử dụng`validate`, vì các bài toán mang tính xây dựng thường cho phép một số kết quả đầu ra hợp lệ khác nhau. 

Kiểm tra toàn bộ đặc biệt hữu ích để kiểm tra giới hạn trên của mỗi vòng lặp. Kiểm tra xung đột phát hiện ra lỗi logic phổ biến nhất, chấp nhận phiên bản chỉ vì mọi phép chiếu đều chứa yêu cầu`#`tế bào một cách độc lập. Kiểm tra khối đơn thực hiện các tọa độ khác 0 bên trong hộp và kiểm tra xem các ô chiếu trống có buộc các vị trí ba chiều tương ứng vẫn trống hay không. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp đầu tiên | Chính xác`YES`xây dựng | Tái thiết bình thường và lấp đầy tối đa | 
| Cung cấp`NO`mẫu |`NO`| Ràng buộc chiếu không tương thích | 
|`1 × 1 × 1`, tất cả`#`| Một khối | Kích thước tối thiểu | 
|`1 × 1 × 1`, một hình chiếu`.`|`NO`| Trường hợp nhỏ nhất không thể | 
|`2 × 2 × 2`, tất cả`#`| Full hộp 8 viên | Đầy đủ ranh giới và xây dựng tối đa | 
| Mâu thuẫn`2 × 2 × 1`dự đoán |`NO`| Phát hiện không tương thích`#`tế bào | 
| thưa thớt`3 × 3 × 2`ví dụ |`YES`, một khối tương thích | Phối hợp ánh xạ và các ô trống | 

## Vỏ cạnh 

Đối với đầu vào hợp lệ tối thiểu,```
1 1 1#
#
#
```có chính xác một khối có thể. Cả ba phép chiếu đều yêu cầu điều đó, do đó bộ ba`(0,0,0)`thỏa mãn ba điều kiện. Tất cả các ô chiếu được đánh dấu được che phủ và đầu ra là```
YES#
```Thuật toán không yêu cầu bất kỳ xử lý đặc biệt nào đối với kích thước`1`; các vòng lặp thông thường tự nhiên thực hiện chính xác một lần. 

Đối với đầu vào không thể tối thiểu,```
1 1 1.
#
#
```khối lập phương duy nhất có thể bị loại bỏ ngay lập tức vì hình chiếu bên trái bị`.`. hai`#`các ô trong các hình chiếu khác không thể được thực hiện nếu không có khối đó, vì vậy việc kiểm tra mức độ bao phủ sẽ tìm thấy một ô chưa được che phủ`#`và trả về`NO`. 

Vụ xung đột```
2 2 1#...
.#..
#...
```cho thấy tại sao cả ba phép chiếu phải được kiểm tra cùng nhau. Hình chiếu bên trái yêu cầu`y=0`, hình chiếu phía trước yêu cầu`x=1`, trong khi hình chiếu trên cùng yêu cầu`x=0`Tại`y=0`. Trong lớp chiều cao duy nhất, không`(x,y)`cặp thỏa mãn cả ba điều kiện. Do đó`covered_left[0][0]`vẫn sai và thuật toán từ chối thể hiện. 

Để được lấp đầy hoàn toàn`2 × 2 × 2`hộp,```
2 2 2####
####
####
```mọi sự kết hợp của`x`,`y`, Và`z`thỏa mãn cả ba điều kiện. Quá trình quét bao gồm mọi ô chiếu và đầu ra chứa tất cả tám hình khối. Vì mọi khối có thể đều tương thích nên không có giải pháp hợp lệ nào có thể chứa nhiều hơn tám khối, do đó việc xây dựng là tối ưu. 

Trường hợp thưa thớt cũng được xử lý mà không có trường hợp đặc biệt. Một ô chiếu được đánh dấu`.`loại bỏ mọi vị trí ba chiều ánh xạ lên nó. Vì hình cuối cùng chính xác là giao điểm của ba hình chiếu được nâng lên nên các ranh giới một chiều và các vùng trống bên trong được xử lý giống hệt nhau. Đây là bất biến trung tâm đằng sau toàn bộ công trình.
