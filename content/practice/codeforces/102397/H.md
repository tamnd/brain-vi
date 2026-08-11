---
title: "CF 102397H - Mahmoud và những viên đá lát đường"
description: "Chúng ta có một dãy gồm n phiến đá và mỗi phiến đá có một màu a[i]. Chúng ta cần đếm mọi tập hợp con khác trống của các vị trí có các cột cờ có cùng màu."
date: "2026-08-10T18:07:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "H"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 269
verified: true
draft: false
---

[CF 102397H - Mahmoud và những phiến đá](https://codeforces.com/problemset/problem/102397/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`n`đá cờ và mỗi đá cờ có một màu`a[i]`. Chúng ta cần đếm mọi tập hợp con khác trống của các vị trí có các cột cờ có cùng màu. Các vị trí khác nhau nên việc chọn các cột cờ khác nhau sẽ tạo ra các tập hợp con khác nhau ngay cả khi màu của chúng bằng nhau. 

Giả sử một màu cụ thể xuất hiện`c`lần. Mọi tập hợp con hợp lệ sử dụng màu này chỉ đơn giản là một tập hợp con khác rỗng của các tập hợp con đó.`c`đá lát đường. có`2^c`tổng cộng các tập hợp con, bao gồm cả tập hợp con trống, vì vậy màu này góp phần`2^c - 1`những lựa chọn hợp lệ. Câu trả lời cuối cùng là tổng số lượng này trên mỗi màu xuất hiện. 

Trình tự có thể chứa tới`10^5`đá cờ, trong khi mỗi màu nhiều nhất`10^5`. Với kích thước đầu vào này, thuật toán kiểm tra tất cả các tập hợp con là không thể vì có thể có`2^100000 - 1`các tập con không rỗng. Thậm chí một`O(n^2)`giải pháp sẽ thực hiện xung quanh`10^10`trong trường hợp xấu nhất, vượt xa giới hạn lập trình cạnh tranh khoảng hai giây có thể hỗ trợ. Chúng ta cần một thuật toán có công việc về cơ bản là tuyến tính theo số lượng cột cờ. 

Có một số trường hợp nguy hiểm có thể khiến việc triển khai bất cẩn không thành công. Với một tấm cờ duy nhất, chẳng hạn như`1`theo sau là`5`, có chính xác một tập hợp con hợp lệ, chính là phiến đá. Một công thức quên loại bỏ tập hợp con trống sẽ trả về`2`thay vì`1`. 

Với tất cả các màu sắc khác nhau, ví dụ`3`theo sau là`1 2 3`, mỗi tập hợp con hợp lệ chứa chính xác một cột cờ, vì vậy câu trả lời là`3`. Giải pháp đếm tất cả các tập hợp con mà không nhóm theo màu sẽ bao gồm không chính xác các tập hợp con có màu hỗn hợp như`{1, 2}`. 

Với những màu sắc lặp đi lặp lại, chẳng hạn như`5`theo sau là`5 5 1 2 3`, hai phiến đá có màu`5`tạo ra ba tập hợp con hợp lệ: một trong số chúng hoặc cả hai cùng nhau. Tổng cộng là`3 + 1 + 1 + 1 = 6`. Chỉ đếm một tập hợp con cho mỗi màu sẽ bỏ lỡ các kết hợp được hình thành bằng cách chọn nhiều viên đá cờ có màu đó. 

Cuối cùng, màu sắc có thể xuất hiện ở giá trị tối đa cho phép. Ví dụ,`2`theo sau là`1 100000`có câu trả lời`2`, vì cả hai màu đều xuất hiện một lần. Một triển khai phân bổ một mảng tần số nhỏ hơn`100001`sẽ truy cập bên ngoài phạm vi dự định của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi tập con khác rỗng của`n`các vị trí. Có chính xác`2^n - 1`các tập hợp con như vậy. Đối với mỗi tập hợp con, chúng ta có thể kiểm tra các phiến đá đã chọn của nó và kiểm tra xem tất cả các màu của chúng có bằng nhau hay không. Điều này đúng vì mọi tập hợp con có thể đều được kiểm tra và chấp nhận một cách chính xác khi nó thỏa mãn điều kiện. 

Vấn đề là số lượng tập hợp con. Khi`n = 100000`, có`2^100000 - 1`các tập con khác rỗng, vốn đã vượt xa bất kỳ khối lượng công việc khả thi nào. Nếu việc kiểm tra một tập hợp con yêu cầu phải xem xét tối đa`n`vị trí, tổng số công việc trong trường hợp xấu nhất là`Θ(n 2^n)`. Ngay cả với cách biểu diễn tập con thông minh hơn giúp giảm chi phí kiểm tra,`2^n`việc liệt kê bản thân nó đã là không thể. 

Quan sát hữu ích là vị trí thực tế của các phiến đá không quan trọng một khi chúng ta biết mỗi màu xuất hiện bao nhiêu lần. Nếu màu sắc`x`xuất hiện`c`lần, mọi tập hợp con hợp lệ có màu là`x`phải chọn một số bộ sưu tập khác trống từ chính xác những bộ sưu tập đó`c`các vị trí. Số cách chọn một tập hợp như vậy là`2^c - 1`. 

Điều này biến bài toán tập hợp con ban đầu thành bài toán đếm tần số. Đầu tiên chúng ta đếm xem mỗi màu có bao nhiêu viên đá, sau đó tính toán`2^c - 1`cho mỗi tần số và thêm các modulo đóng góp đó`10^9 + 7`. 

Ví dụ, trong`3 5 5 1 2`, màu sắc`3`xảy ra một lần và góp phần`1`, màu sắc`5`xảy ra hai lần và góp phần`3`, màu sắc`1`xảy ra một lần và góp phần`1`, và màu sắc`2`xảy ra một lần và góp phần`1`. Tổng cộng là`1 + 3 + 1 + 1 = 6`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n 2^n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo cấu trúc tần số ghi lại số lượng đá cờ có mỗi màu. Vì mọi màu sắc đều ở giữa`1`Và`100000`, một mảng có kích thước`100001`là đủ. Việc đếm tần số mất một lần qua đầu vào. 
2. Tính toán trước lũy thừa của hai từ`2^0`bởi vì`2^n`, luôn lấy kết quả theo modulo`10^9 + 7`. Chúng tôi cần`2^c`cho mọi tần số có thể`c`và không có màu nào có thể xuất hiện nhiều hơn`n`lần. 
3. Lặp lại các màu sắc và xem tần số của chúng. Nếu xuất hiện một màu`c`có lúc, có`2^c`các cách để chọn một tập hợp con của những viên đá cờ đó bởi vì mỗi`c`các vị trí độc lập có hai lựa chọn, được chọn hoặc không được chọn. 
4. Trừ một từ`2^c`để loại trừ tập hợp con trống. Vì vậy sự đóng góp của màu sắc này là`2^c - 1`. 
5. Thêm phần đóng góp của mỗi màu vào câu trả lời theo modulo`10^9 + 7`. Màu sắc có tần số bằng 0 không đóng góp gì nên có thể bỏ qua chúng. 

### Tại sao nó hoạt động 

Đối với mỗi màu`x`, gọi tần số của nó là`c`. Một tập hợp con có các phiến đá đều có màu`x`chỉ có thể chứa`c`vị trí mang`x`. Mỗi vị trí đó có hai lựa chọn độc lập, do đó có chính xác`2^c`tập hợp con của các vị trí này. Chính xác một trong số chúng trống rỗng, để lại`2^c - 1`tập hợp con hợp lệ không trống. 

Mỗi tập hợp con hợp lệ có chính xác một màu, vì vậy nó thuộc về phần đóng góp của đúng một màu. Ngược lại, mỗi tập hợp con được tính bằng một màu`2^c - 1`thuật ngữ chỉ chứa các phiến đá có màu đó và do đó hợp lệ. Do đó, các đóng góp rời rạc và chứa chung mọi tập hợp con hợp lệ đúng một lần, điều này chứng tỏ rằng tổng của chúng là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAX_COLOR = 100000

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    freq = [0] * (MAX_COLOR + 1)

    for color in a:
        freq[color] += 1

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = 0

    for color in range(1, MAX_COLOR + 1):
        c = freq[color]
        if c:
            ans = (ans + pow2[c] - 1) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`đọc số lượng phiến đá và màu sắc của chúng. Mảng tần số sử dụng thực tế là`a[i]`nhiều nhất là`100000`, Vì thế`freq[color]`lưu trữ trực tiếp số lần xuất hiện của màu đó. 

các`pow2`mảng chứa mọi lũy thừa của hai số có thể cần thiết.`pow2[0]`là`1`và mỗi giá trị sau thu được bằng cách nhân đôi giá trị trước đó theo modulo`MOD`. Điều này tránh việc tính toán lại quyền hạn nhiều lần và giữ cho mọi số nguyên được lưu trữ ở mức nhỏ. 

Vòng lặp trả lời sử dụng`pow2[c] - 1`, khớp chính xác với đối số tổ hợp. Phép trừ được thực hiện trước phép toán modulo cuối cùng trong biểu thức, nhưng kết quả tích lũy được chuẩn hóa bằng`% MOD`trên mỗi lần lặp. Số nguyên Python không bị tràn, mặc dù số học mô-đun vẫn được yêu cầu vì câu trả lời được yêu cầu rõ ràng là modulo`10^9 + 7`. 

Điều kiện tần số`if c:`ngăn chặn việc thêm bất kỳ thứ gì cho các màu không xuất hiện. Vòng lặp bắt đầu lúc`1`và kết thúc tại`100000`, do đó cả hai cực trị của dải màu được phép đều được xử lý. 

Bài toán chỉ có một ca kiểm thử nên không có vòng lặp ca kiểm thử. Định dạng mẫu được hiển thị của câu lệnh được cung cấp không nhất quán, nhưng mẫu dự định là`n = 5`với màu sắc`3 5 5 1 2`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào mẫu dự kiến là:```
5
3 5 5 1 2
```Các tần số là`1`cho màu sắc`1`,`2`, Và`3`, Và`2`cho màu sắc`5`. 

| Màu sắc | Tính thường xuyên`c`|`2^c`| Sự đóng góp`2^c - 1`| Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 1 | 
| 2 | 1 | 2 | 1 | 2 | 
| 3 | 1 | 2 | 1 | 3 | 
| 5 | 2 | 4 | 3 | 6 | 

Ba màu xuất hiện khi mỗi màu đóng góp những viên đá cờ riêng lẻ. Màu sắc`5`đóng góp ba tập hợp con: chọn tập đầu tiên`5`, chọn cái thứ hai`5`, hoặc chọn cả hai. Câu trả lời cuối cùng là`6`. 

### Mẫu 2 

Hãy xem xét:```
4
7 7 7 2
```Màu sắc`7`xảy ra ba lần, trong khi màu sắc`2`xảy ra một lần. 

| Màu sắc | Tính thường xuyên`c`|`2^c`| Sự đóng góp`2^c - 1`| Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 2 | 1 | 1 | 
| 7 | 3 | 8 | 7 | 8 | 

Có bảy tập con khác rỗng của ba tấm cờ được tô màu`7`, cộng với một tập hợp con chứa đá cờ có màu`2`. Câu trả lời là`8`. 

Dấu vết này chứng minh tại sao các màu lặp lại được xử lý bằng lũy ​​thừa hai thay vì chỉ đơn giản đếm xem có bao nhiêu màu riêng biệt tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + C)`| Đếm mất`O(n)`, sức mạnh tính toán trước mất`O(n)`và việc quét dải màu sẽ mất`O(C)`Ở đâu`C = 100000`| 
| Không gian |`O(n + C)`| Mảng quyền hạn có`n + 1`các phần tử và mảng tần số có`100001`yếu tố | 

Với`n <= 100000`và phạm vi màu cũng được giới hạn bởi`100000`, cả hai mảng đều đủ nhỏ để`256 MB`giới hạn bộ nhớ. Tổng khối lượng công việc là tuyến tính trong kích thước đầu vào và dải màu nên dễ dàng phù hợp với thời hạn đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7
MAX_COLOR = 100000

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = [0] * (MAX_COLOR + 1)

    for color in a:
        freq[color] += 1

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    ans = 0

    for color in range(1, MAX_COLOR + 1):
        c = freq[color]
        if c:
            ans = (ans + pow2[c] - 1) % MOD

    print(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("5\n3 5 5 1 2\n") == "6", "sample 1"

# Single flagstone
assert run("1\n1\n") == "1", "minimum-size input"

# All flagstones have the same color
assert run("5\n7 7 7 7 7\n") == "31", "all equal"

# Every flagstone has a different color
assert run("4\n1 2 3 4\n") == "4", "all different"

# Boundary colors 1 and 100000
assert run("6\n1 100000 100000 1 100000 1\n") == "14", "color boundaries"

# Maximum-size all-equal case
assert run("100000\n" + "100000 " * 99999 + "100000\n") == "607723519", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 3 5 5 1 2`|`6`| Cung cấp mẫu và đếm màu lặp lại | 
|`1 / 1`|`1`| Đầu vào tối thiểu và loại trừ tập hợp con trống | 
|`5 / 7 7 7 7 7`|`31`| Trường hợp đều bằng nhau, sử dụng`2^5 - 1`| 
|`4 / 1 2 3 4`|`4`| Tất cả các màu đều khác nhau nên chỉ có các tập hợp con đơn lẻ mới hoạt động | 
|`6 / 1 100000 100000 1 100000 1`|`14`| Cả ranh giới màu và nhiều màu lặp lại | 
|`100000 / all 100000`|`607723519`| Kích thước đầu vào tối đa và lũy thừa mô-đun | 

Giá trị mong đợi kích thước tối đa đến từ`2^100000 - 1`modulo`10^9 + 7`. Nó kiểm tra cả hiệu suất và thực tế là việc triển khai thực hiện số học mô-đun thay vì cố gắng hiện thực hóa con số chính xác khổng lồ. 

## Vỏ cạnh 

Một tấm cờ duy nhất là đầu vào nhỏ nhất có thể. Vì`n = 1`và mảng`1`, tần số màu`1`là`1`, vậy đóng góp của nó là`2^1 - 1 = 1`. Thuật toán in`1`. Ở đây, phép trừ một là cần thiết vì tập con thứ hai được tạo ra bởi lựa chọn nhị phân là tập con trống, điều này không được phép. 

Ví dụ: khi tất cả các màu đều khác biệt`4`theo sau là`1 2 3 4`, mọi tần số đều là`1`. Mỗi màu sắc góp phần`2^1 - 1 = 1`, mang lại tổng cộng`4`. Một tập hợp con chứa hai vị trí khác nhau sẽ tự động bị từ chối bằng cách đếm dựa trên tần số vì không có một màu nào nhận được tập hợp con như vậy. 

Ví dụ: khi mọi viên đá đều có cùng một màu`5`theo sau là`7 7 7 7 7`, toàn bộ vấn đề quy về việc chọn bất kỳ tập con nào khác trống của năm vị trí. có`2^5 - 1 = 31`các tập hợp con như vậy và thuật toán thu được chính xác giá trị đó từ tần số đơn`c = 5`. 

Đối với màu ranh giới, hãy xem xét`6`theo sau là`1 100000 100000 1 100000 1`. Màu sắc`1`xuất hiện ba lần và đóng góp`7`, trong khi màu sắc`100000`cũng xuất hiện ba lần và đóng góp khác`7`. Câu trả lời là`14`. Mảng tần số có các chỉ số thông qua`100000`, do đó cả hai giá trị biên đều được biểu diễn chính xác. 

Đối với kích thước đầu vào tối đa, nếu tất cả`100000`các phiến đá có cùng màu, thuật toán không liệt kê bất kỳ tập hợp con nào. Nó lưu trữ tần số`100000`, tính toán`2^100000`modulo`10^9 + 7`, trừ đi một và trả về`607723519`. Việc thực thi vẫn tuyến tính mặc dù số lượng tập hợp con hợp lệ về mặt toán học có khoảng ba mươi nghìn chữ số thập phân.
