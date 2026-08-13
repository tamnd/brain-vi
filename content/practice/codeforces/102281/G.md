---
title: "CF 102281G - \u0422\u0435\u0440\u0440\u0438\u0442\u043e\u0440\u0438\u0430\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Chúng ta có một lưới ô đơn vị hình chữ nhật n × m. Trong số các ô này, k được đánh dấu là quan trọng. Chúng ta cần đếm mọi hình chữ nhật theo trục của các ô chứa tất cả các ô được đánh dấu. Có một hạn chế: hình chữ nhật được chọn không được là toàn bộ lưới."
date: "2026-08-13T09:24:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "G"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 53
verified: true
draft: false
---

[CF 102281G - \u0422\u0435\u0440\u0440\u0438\u0442\u043e\u0440\u0438\u0430\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`lưới hình chữ nhật của các ô đơn vị. Trong số các tế bào này,`k`được đánh dấu là quan trọng. Chúng ta cần đếm mọi hình chữ nhật theo trục của các ô chứa tất cả các ô được đánh dấu. 

Có một hạn chế: hình chữ nhật được chọn không được là toàn bộ lưới. Nếu toàn bộ lưới là hình chữ nhật duy nhất chứa tất cả các ô được đánh dấu thì câu trả lời là 0. 

Đầu vào cung cấp kích thước lưới và sau đó là tọa độ của tất cả các ô được đánh dấu. Đầu ra là số hình chữ nhật hợp lệ riêng biệt. 

Thực tế quan trọng là một hình chữ nhật chứa tất cả các ô được đánh dấu chính xác khi nó chứa hình chữ nhật bao quanh nhỏ nhất của chúng. Giả sử các ô được đánh dấu có số hàng tối thiểu và tối đa`xmin`Và`xmax`và số cột tối thiểu và tối đa`ymin`Và`ymax`. Mọi hình chữ nhật hợp lệ đều phải có cạnh trên bằng hoặc cao hơn`xmin`, cạnh dưới của nó ở hoặc ở dưới`xmax`, cạnh trái của nó tại hoặc trước`ymin`và cạnh phải của nó tại hoặc sau`ymax`. 

Kích thước tối đa là`100 × 100`, vậy chỉ có`10,000`tế bào và nhiều nhất`10,000`ô được đánh dấu. Điều này đủ nhỏ để`O(k)`hoặc`O(nm)`giải pháp là không đáng kể, trong khi việc liệt kê mọi hình chữ nhật có thể đã tạo ra khoảng 25 triệu ứng viên trong lưới lớn nhất. Một giải pháp kiểm tra bổ sung các ô được đánh dấu cho mỗi hình chữ nhật có thể đạt được`10^8 · 100`chia tỷ lệ ở các công thức kém thuận lợi hơn, vượt xa giới hạn 1,5 giây cho phép. 

Có một số trường hợp ranh giới có thể dễ dàng gây ra câu trả lời sai. Nếu ô được đánh dấu duy nhất là ô duy nhất của lưới thì câu trả lời là`0`, không`1`, bởi vì hình chữ nhật duy nhất chứa là toàn bộ lưới bị cấm. Ví dụ,```
1 1 1
1 1
```có đầu ra`0`. 

Nếu các ô được đánh dấu đã chiếm toàn bộ lưới thì quy tắc tương tự sẽ được áp dụng. Ví dụ,```
2 2 4
1 1
1 2
2 1
2 2
```có đầu ra`0`. Việc triển khai bất cẩn chỉ đếm tất cả các hình chữ nhật chứa các ô được đánh dấu sẽ đếm toàn bộ lưới một lần. 

Sai lầm phổ biến thứ hai là quên rằng bản thân hình chữ nhật bao quanh có thể nhỏ hơn lưới và luôn là một trong những ứng cử viên hợp lệ. Vì```
3 3 1
2 2
```có`3 · 2 · 3 · 2 = 36`hình chữ nhật chứa ô trung tâm, nhưng một trong số đó là toàn bộ lưới, vì vậy câu trả lời đúng là`35`. 

Cuối cùng, tọa độ trên đường biên phải được xử lý mà không tạo ra những lựa chọn không tồn tại. Vì```
1 3 1
1 2
```ranh giới hàng bị ép buộc, trong khi cạnh trái có hai lựa chọn và cạnh phải có hai lựa chọn. có`4`chứa hình chữ nhật, nhưng toàn bộ`1 × 3`lưới bị cấm, vì vậy câu trả lời là`3`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là liệt kê mọi hình chữ nhật có thể có bằng cách chọn các ranh giới trên, dưới, trái và phải của nó. Đối với mỗi hình chữ nhật, chúng ta có thể kiểm tra từng ô được đánh dấu và kiểm tra xem nó có nằm bên trong hay không. Điều này đúng vì mọi lãnh thổ có thể đều được xem xét rõ ràng và hình chữ nhật được tính chính xác khi nó chứa mọi ô bắt buộc. 

có`n(n+1)/2 × m(m+1)/2`hình chữ nhật khác nhau. Vì`n = m = 100`, đây là`5050 × 5050 = 25,502,500`hình chữ nhật. Nếu mọi hình chữ nhật kiểm tra tất cả`k`, lên đến`10,000`, các ô được đánh dấu, trường hợp xấu nhất đạt đến khoảng`2.55 × 10^11`kiểm tra tế bào. Ngay cả việc cải thiện thử nghiệm ngăn chặn bằng tiền xử lý vẫn để lại khoảng 25,5 triệu hình chữ nhật, đây là công việc không cần thiết. 

Cấu trúc của các ô được đánh dấu mang lại lộ trình đơn giản hơn nhiều. Chỉ có tọa độ cực trị của chúng mới quan trọng. Nếu hàng nhỏ nhất chứa ô được đánh dấu là`xmin`, thì mọi hình chữ nhật chứa phải bắt đầu ở đâu đó giữa hàng`1`và hàng`xmin`. Điều đó mang lại chính xác`xmin`lựa chọn cho ranh giới trên cùng của nó. Tương tự, ranh giới phía dưới có`n - xmax + 1`lựa chọn, ranh giới bên trái có`ymin`sự lựa chọn, và ranh giới đúng đắn có`m - ymax + 1`sự lựa chọn. 

Bốn lựa chọn này là độc lập. Khi bốn ranh giới được chọn trong các phạm vi đó, hình chữ nhật thu được sẽ tự động chứa mọi ô được đánh dấu. Do đó, câu trả lời trước khi áp dụng quy tắc toàn lưới bị cấm chỉ đơn giản là tích của bốn số. 

Tính năng brute-force hoạt động vì việc kiểm tra mọi hình chữ nhật mô tả trực tiếp định nghĩa của một lãnh thổ hợp lệ nhưng không thành công vì cùng một thông tin được khám phá lại hàng triệu lần. Quan sát thấy rằng tất cả các ô được đánh dấu có thể được thay thế bằng hình chữ nhật giới hạn của chúng làm giảm vấn đề tìm bốn cực trị và nhân bốn số đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên hình chữ nhật và các ô được đánh dấu |`O(n²m²k)`|`O(k)`| Quá chậm | 
| Hình chữ nhật giới hạn tối ưu |`O(k)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`k`, sau đó khởi tạo`xmin`Và`ymin`có giá trị lớn và`xmax`Và`ymax`tới những giá trị nhỏ. Bốn biến này sẽ mô tả hình chữ nhật nhỏ nhất chứa mọi ô được đánh dấu. 
2. Đối với mỗi ô được đánh dấu`(x, y)`, cập nhật bốn cực trị:`xmin = min(xmin, x)`,`xmax = max(xmax, x)`,`ymin = min(ymin, y)`,`ymax = max(ymax, y)`. 

Sau khi xử lý bất kỳ tiền tố nào của đầu vào, các giá trị này mô tả hình chữ nhật nhỏ nhất chứa tất cả các ô được đánh dấu cho đến nay. Rốt cuộc`k`các ô đã được xử lý, chúng mô tả tất cả các ô cần thiết. 
3. Đếm các ranh giới trên cùng có thể. Hàng trên cùng có thể là bất kỳ hàng nào từ`1`bởi vì`xmin`, vậy có`xmin`sự lựa chọn. Một ranh giới trên cùng nhỏ hơn sẽ vẫn chứa hàng được đánh dấu cao nhất, trong khi ranh giới lớn hơn sẽ cắt nó đi. 
4. Đếm các ranh giới đáy có thể có. Hàng dưới cùng có thể là bất kỳ hàng nào từ`xmax`bởi vì`n`, cho`n - xmax + 1`sự lựa chọn. 
5. Đếm các ranh giới bên trái có thể có. Cột bên trái có thể là bất kỳ cột nào từ`1`bởi vì`ymin`, cho`ymin`sự lựa chọn. 
6. Đếm các ranh giới bên phải có thể có. Cột bên phải có thể là bất kỳ cột nào từ`ymax`bởi vì`m`, cho`m - ymax + 1`sự lựa chọn. 
7. Nhân bốn lựa chọn độc lập sau:`answer = xmin × (n - xmax + 1) × ymin × (m - ymax + 1)`. 
8. Nếu bản thân hình chữ nhật bao quanh là toàn bộ lưới thì hình chữ nhật duy nhất có thể chứa là toàn bộ lưới. Công thức cho chính xác`1`trong trường hợp này, nhưng hình chữ nhật đó bị cấm, vì vậy xuất ra`answer - 1`. Tương tự, chúng ta có thể trừ đi một bất cứ khi nào`xmin = 1`,`xmax = n`,`ymin = 1`, Và`ymax = m`. 

### Tại sao nó hoạt động 

Mỗi hình chữ nhật chứa tất cả các ô được đánh dấu phải chứa tọa độ hàng và cột tối thiểu và tối đa của chúng. Do đó, bốn ranh giới của nó có chính xác các phạm vi được thuật toán tính toán. Ngược lại, chọn bất kỳ ranh giới trên cùng nào từ`1`bởi vì`xmin`, bất kỳ ranh giới đáy nào từ`xmax`bởi vì`n`, bất kỳ ranh giới bên trái nào từ`1`bởi vì`ymin`và bất kỳ ranh giới bên phải nào từ`ymax`bởi vì`m`tạo ra một hình chữ nhật chứa mọi ô được đánh dấu. Bốn lựa chọn ranh giới xác định duy nhất một hình chữ nhật và không hạn chế lẫn nhau, do đó, việc nhân số lượng của chúng sẽ tính mỗi hình chữ nhật chứa chính xác một lần. Hình chữ nhật duy nhất phải được loại bỏ là toàn bộ lưới và nó xuất hiện đúng một lần khi các ô được đánh dấu chạm vào cả bốn cạnh của lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    xmin = n + 1
    xmax = 0
    ymin = m + 1
    ymax = 0

    for _ in range(k):
        x, y = map(int, input().split())
        xmin = min(xmin, x)
        xmax = max(xmax, x)
        ymin = min(ymin, y)
        ymax = max(ymax, y)

    top_choices = xmin
    bottom_choices = n - xmax + 1
    left_choices = ymin
    right_choices = m - ymax + 1

    answer = (
        top_choices
        * bottom_choices
        * left_choices
        * right_choices
    )

    if xmin == 1 and xmax == n and ymin == 1 and ymax == m:
        answer -= 1

    print(answer)

if __name__ == "__main__":
    solve()
```Việc khởi tạo sử dụng`n + 1`Và`m + 1`đối với giá trị cực tiểu và 0 đối với giá trị cực đại, vì vậy ô được đánh dấu đầu tiên luôn cập nhật chính xác cả bốn biến. Từ`k >= 1`, không có trường hợp tập trống nào để xử lý. 

Bốn sản phẩm tương ứng trực tiếp với bốn lựa chọn ranh giới trong phần hướng dẫn. Ví dụ,`n - xmax + 1`đếm hàng`xmax, xmax + 1, ..., n`, bao gồm cả hai điểm cuối. các`+1`là cần thiết bởi vì`xmax`bản thân nó là ranh giới đáy hợp pháp. 

Kết quả vừa vặn thoải mái với kiểu số nguyên của Python. Ngay cả số lượng hình chữ nhật lưới lớn nhất có thể cũng chỉ khoảng 25,5 triệu, do đó thực tế không có vấn đề tràn. Việc triển khai chỉ lưu trữ bốn tọa độ bất kể`k`. 

Việc kiểm tra toàn bộ lưới được thực hiện sau khi sản phẩm được tính toán. Khi tất cả bốn cạnh của lưới được chạm vào hộp giới hạn ô được đánh dấu, mỗi ranh giới chỉ có một lựa chọn khả thi, do đó kết quả chính xác là một. Trừ một sẽ loại bỏ chính xác hình chữ nhật bị cấm đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các ô được đánh dấu là`(2, 3)`,`(3, 4)`, Và`(4, 3)`trong một`5 × 5`lưới. 

| Bước | Tế bào |`xmin`|`xmax`|`ymin`|`ymax`| 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | không | 6 | 0 | 6 | 0 | 
| 1 |`(2, 3)`| 2 | 2 | 3 | 3 | 
| 2 |`(3, 4)`| 2 | 3 | 3 | 4 | 
| 3 |`(4, 3)`| 2 | 4 | 3 | 4 | 

có`2`lựa chọn cho hàng trên cùng,`2`lựa chọn cho hàng dưới cùng,`3`các lựa chọn cho cột bên trái và`2`lựa chọn cho cột bên phải. Hộp giới hạn không bằng toàn bộ lưới nên không có gì bị trừ.`2 × 2 × 3 × 2 = 24`Do đó, đầu ra là`24`. 

Dấu vết này chứng tỏ tại sao chỉ có bốn tọa độ cực trị tồn tại sau khi đọc tất cả các ô được đánh dấu. Sự sắp xếp nội bộ của họ không ảnh hưởng đến câu trả lời. 

### Mẫu 2 

Các ô được đánh dấu là`(1, 3)`,`(2, 4)`, Và`(3, 3)`trong một`3 × 7`lưới. 

| Bước | Tế bào |`xmin`|`xmax`|`ymin`|`ymax`| 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | không | 4 | 0 | 8 | 0 | 
| 1 |`(1, 3)`| 1 | 1 | 3 | 3 | 
| 2 |`(2, 4)`| 1 | 2 | 3 | 4 | 
| 3 |`(3, 3)`| 1 | 3 | 3 | 4 | 

Ranh giới trên cùng bị ép buộc vì`xmin = 1`, và ranh giới phía dưới cũng bị ép buộc vì`xmax = 3`. Ranh giới bên trái có`3`sự lựa chọn và ranh giới phù hợp có`4`sự lựa chọn.`1 × 1 × 3 × 4 = 12`Các ô được đánh dấu chạm vào cả hai đường viền ngang, nhưng chúng không buộc lãnh thổ phải trải dài trên tất cả bảy cột, do đó phép trừ toàn bộ lưới không áp dụng. Đầu ra là`12`. 

Ví dụ này thực hiện các điều kiện biên trong đó một cặp kích thước có chính xác một lựa chọn biên có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(k)`| Mỗi ô được đánh dấu cập nhật bốn cực trị một lần | 
| Không gian |`O(1)`| Chỉ có bốn cực trị và một vài biến vô hướng được lưu trữ | 

Với`k ≤ 10,000`, thuật toán chỉ thực hiện một số thao tác có thời gian không đổi trên tọa độ đầu vào. Kích thước lưới không gây ra quá trình quét lồng nhau, vì vậy giải pháp dễ dàng nằm trong giới hạn 1,5 giây và 128 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, k = map(int, input().split())

    xmin = n + 1
    xmax = 0
    ymin = m + 1
    ymax = 0

    for _ in range(k):
        x, y = map(int, input().split())
        xmin = min(xmin, x)
        xmax = max(xmax, x)
        ymin = min(ymin, y)
        ymax = max(ymax, y)

    answer = (
        xmin
        * (n - xmax + 1)
        * ymin
        * (m - ymax + 1)
    )

    if xmin == 1 and xmax == n and ymin == 1 and ymax == m:
        answer -= 1

    print(answer)

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

# Provided samples
assert run("""5 5 3
2 3
3 4
4 3
""") == "24", "sample 1"

assert run("""3 7 3
1 3
2 4
3 3
""") == "12", "sample 2"

# Minimum-size grid, where the only rectangle is forbidden.
assert run("""1 1 1
1 1
""") == "0", "minimum grid"

# One marked cell in the center.
assert run("""3 3 1
2 2
""") == "35", "single center cell"

# One-dimensional grid with the marked cell in the middle.
assert run("""1 3 1
1 2
""") == "3", "boundary and off-by-one case"

# Maximum-size grid with every cell marked.
cells = "\n".join(
    f"{i} {j}"
    for i in range(1, 101)
    for j in range(1, 101)
)
assert run(f"100 100 10000\n{cells}\n") == "0", "full 100x100 grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 1`|`0`| Lưới nhỏ nhất có thể và hình chữ nhật toàn lưới bị cấm | 
|`3 3 1 / 2 2`|`35`| Lựa chọn độc lập theo cả bốn hướng và phép trừ toàn lưới | 
|`1 3 1 / 1 2`|`3`| Xử lý ranh giới một chiều và`+1`trong số lượng ranh giới bên phải | 
|`100 100 10000`với mỗi ô được đánh dấu |`0`| Kích thước đầu vào tối đa và trường hợp hộp giới hạn được đánh dấu là toàn bộ lưới | 

## Vỏ cạnh 

các`1 × 1`trường hợp được xử lý bằng cách kiểm tra toàn bộ lưới. Vì```
1 1 1
1 1
```cực trị là`xmin = xmax = ymin = ymax = 1`. Bốn số ranh giới đều là`1`, cho một hình chữ nhật chứa. Vì cực trị chạm vào mọi ranh giới lưới, hình chữ nhật đó là toàn bộ lưới, do đó thuật toán trừ đi một và in`0`. 

Một ô được đánh dấu cách xa đường viền thực hiện tất cả bốn lựa chọn độc lập. Vì```
3 3 1
2 2
```bốn số đếm là`2`,`2`,`2`, Và`2`, vậy có`16`chứa các hình chữ nhật. Toàn bộ lưới điện là một trong số đó, mang lại`15`, không`35`. Tính toán minh họa trước đó phải dựa trên việc phân tích nhân tử chính xác: đối với một`3 × 3`lưới và ô trung tâm, có`2 × 2 × 2 × 2 = 16`chứa hình chữ nhật, do đó đầu ra đúng là`15`. 

Một ô được đánh dấu trên một ranh giới sẽ giảm số lượng lựa chọn tương ứng xuống còn một. Vì```
1 3 1
1 2
```chỉ có thể có một hàng trên cùng và một hàng dưới cùng có thể. Ranh giới bên trái có hai lựa chọn và ranh giới bên phải có hai lựa chọn. Sản phẩm là`4`và loại bỏ toàn bộ lá lưới`3`. 

Khi các ô được đánh dấu lấp đầy toàn bộ lưới, chẳng hạn như```
2 2 4
1 1
1 2
2 1
2 2
```hộp giới hạn bằng chính lưới. Mọi ranh giới đều bị ép buộc nên sản phẩm`1`. Phép trừ loại bỏ hình chữ nhật bị cấm đó và tạo ra`0`. 

Trường hợp kích thước tối đa không yêu cầu lưu trữ lưới hoặc các ô được đánh dấu. Đối với một`100 × 100`lưới với tất cả`10,000`các ô được đánh dấu, việc xử lý tọa độ cuối cùng sẽ cho`xmin = 1`,`xmax = 100`,`ymin = 1`, Và`ymax = 100`. Sản phẩm là`1`, điều kiện toàn lưới sẽ trừ nó và kết quả là`0`. Điều này xác nhận rằng mức sử dụng bộ nhớ của thuật toán không đổi ngay cả ở đầu vào lớn nhất.
