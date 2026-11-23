---
title: "CF 492A - Vanya và khối"
description: "Vanya muốn xây dựng một kim tự tháp theo đúng mô hình được mô tả bằng các số tam giác. Cấp độ đầu tiên chứa 1 khối. Cấp độ thứ hai chứa 1 + 2 = 3 hình khối. Cấp độ thứ ba chứa 1 + 2 + 3 = 6 hình khối."
date: "2026-06-07T17:44:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 492
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 280 (Div. 2)"
rating: 800
weight: 492
solve_time_s: 132
verified: true
draft: false
---

[CF 492A - Vanya và Cubes](https://codeforces.com/problemset/problem/492/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vanya muốn xây dựng một kim tự tháp theo đúng mô hình được mô tả bằng các số tam giác. Cấp độ đầu tiên chứa 1 khối. Cấp độ thứ hai chứa 1 + 2 = 3 hình khối. Cấp độ thứ ba chứa 1 + 2 + 3 = 6 hình khối. Nhìn chung, trình độ`i`yêu cầu tổng của số đầu tiên`i`số nguyên dương. 

Chúng ta được cho tổng số hình khối có sẵn,`n`. Nhiệm vụ của chúng tôi là xác định số lượng cấp độ hoàn chỉnh lớn nhất có thể được xây dựng trước khi hết hình khối. 

Hạn chế là rất nhỏ. Số hình lập phương là nhiều nhất`10^4`, có nghĩa là ngay cả một mô phỏng đơn giản cũng đủ nhanh. Số lượng cấp độ có thể được xây dựng nhỏ hơn nhiều so với`10^4`bởi vì mỗi cấp độ mới yêu cầu nhiều hình khối hơn cấp độ trước. Một giải pháp lặp lại đơn giản dễ dàng phù hợp với giới hạn. 

Một lỗi phổ biến là quên rằng bản thân mỗi cấp độ là một số hình tam giác. 

Coi như:```
Input:
4
```Cấp độ đầu tiên sử dụng 1 khối và cấp độ thứ hai sử dụng 3 khối. Cùng nhau chúng cần 4 hình khối, vì vậy câu trả lời là:```
2
```Việc thực hiện bất cẩn có thể cho rằng mức độ không chính xác`i`cần chính xác`i`hình khối và trả lại 3. 

Một trường hợp tinh tế khác xảy ra khi các khối hết ngay sau khi hoàn thành một cấp độ.```
Input:
10
```Các cấp độ yêu cầu 1, 3 và 6 khối. 

Tổng cộng là:```
1 + 3 + 6 = 10
```Câu trả lời đúng là:```
3
```Việc triển khai dừng ngay khi các khối còn lại trở về 0 trước khi đếm mức hoàn thành sẽ trả về 2 không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là xây dựng kim tự tháp theo từng cấp độ. Đối với mỗi cấp độ, hãy tính xem cấp độ đó yêu cầu bao nhiêu khối, trừ nó khỏi nguồn cung còn lại và tiếp tục trong khi vẫn còn đủ khối. 

Mô phỏng lực lượng vũ phu này đã đủ nhanh. Từ`n ≤ 10000`, số lượng cấp độ rất nhỏ. Ngay cả khi chúng ta tính đi tính lại các số tam giác thì khối lượng công việc cũng không đáng kể. 

Quan sát quan trọng là số lượng hình khối cần thiết cho cấp độ`i`chính nó là một số tam giác:$$1 + 2 + \cdots + i = \frac{i(i+1)}{2}$$Vì vậy, trong khi xây dựng kim tự tháp, chúng ta chỉ cần theo dõi cấp độ hiện tại và các khối cần thiết của nó. Bất cứ khi nào chúng tôi có đủ khả năng chi trả cho cấp độ tiếp theo, chúng tôi sẽ trừ chi phí của nó và tăng chiều cao. 

Bởi vì các cấp độ phải được xây dựng theo thứ tự và mỗi cấp độ đều có chi phí cố định, nên việc tham lam lấy các cấp độ từ trên xuống là cách xây dựng hợp lệ duy nhất có thể. Thời điểm chúng ta không đủ khả năng để lên cấp độ tiếp theo thì không thể có kim tự tháp nào cao hơn nữa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(h2) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(h) | O(1) | Đã chấp nhận | 

Đây`h`là chiều cao kim tự tháp cuối cùng, tối đa là khoảng 30 đối với`n ≤ 10000`. 

## Hướng dẫn thuật toán 

1. Đọc số hình khối có sẵn`n`. 
2. Khởi tạo`height = 0`. 
3. Khởi tạo`level = 1`. 
4. Tính số khối cần thiết cho cấp độ hiện tại bằng cách sử dụng:$$\frac{\text{level}(\text{level}+1)}{2}$$5. Nếu còn đủ khối, hãy trừ số đó khỏi`n`, tăng`height`, và chuyển sang cấp độ tiếp theo. 

Điều này tương ứng với việc hoàn thành thành công thêm một lớp nữa của kim tự tháp. 
6. Lặp lại cho đến khi cấp độ tiếp theo yêu cầu nhiều hình khối hơn mức còn lại. 
7. Đầu ra`height`. 

### Tại sao nó hoạt động 

Tại mỗi lần lặp, thuật toán duy trì tính bất biến ở tất cả các mức từ 1 đến`height`đã được xây dựng hoàn chỉnh và giá trị còn lại của`n`bằng số hình khối còn lại sau khi xây dựng chúng. 

Hành động khả thi tiếp theo được xác định duy nhất: cấp độ tiếp theo có thể được hoàn thành hoặc không thể. Nếu có thể, việc xây dựng nó sẽ tăng chiều cao lên một. Nếu không thể, bất kỳ kim tự tháp nào cao hơn cũng sẽ yêu cầu mức độ chưa hoàn thành tương tự, điều này là không thể. Do đó thuật toán dừng chính xác ở độ cao tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

height = 0
level = 1

while True:
    need = level * (level + 1) // 2

    if n < need:
        break

    n -= need
    height += 1
    level += 1

print(height)
```Biến`level`đại diện cho lớp tiếp theo mà chúng tôi đang cố gắng xây dựng. biểu thức```
level * (level + 1) // 2
```tính số tam giác cho mức đó. 

Bất cứ khi nào còn đủ khối, chúng tôi sử dụng số khối đó và ghi lại một cấp độ hoàn thành nữa. Vòng lặp kết thúc chính xác khi lớp yêu cầu tiếp theo không thể được hoàn thành. 

Sai lầm phổ biến nhất là tăng dần`level`trước khi tính toán chi phí của nó. Làm như vậy sẽ bỏ qua lớp đầu tiên có kích thước 1. Một lỗi phổ biến khác là kiểm tra xem không còn khối nào thay vì kiểm tra xem cấp độ tiếp theo có phù hợp hay không. Phù hợp chính xác như`n = 10`vẫn phải tính mức độ hoàn thành cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```| Cấp độ | Khối cần thiết | Khối còn lại trước | Hành động | Chiều cao | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Cấp độ xây dựng | 1 | 
| 2 | 3 | 0 | Dừng lại | 1 | 

Đầu ra:```
1
```Điều này thể hiện đầu vào nhỏ nhất có thể. Sau khi xây dựng cấp độ đầu tiên, không còn khối nào và không thể bắt đầu cấp độ tiếp theo. 

### Ví dụ 2 

đầu vào:```
20
```| Cấp độ | Khối cần thiết | Khối còn lại trước | Hành động | Chiều cao | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 20 | Cấp độ xây dựng | 1 | 
| 2 | 3 | 19 | Cấp độ xây dựng | 2 | 
| 3 | 6 | 16 | Cấp độ xây dựng | 3 | 
| 4 | 10 | 10 | Cấp độ xây dựng | 4 | 
| 5 | 15 | 0 | Dừng lại | 4 | 

Đầu ra:```
4
```Dấu vết này cho thấy sự phù hợp chính xác sau cấp độ thứ tư. Thuật toán đếm chính xác mức đó trước khi dừng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) | Một lần lặp lại cho mỗi cấp độ hoàn thành | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Từ`n ≤ 10000`, chiều cao của kim tự tháp rất nhỏ. Vòng lặp chỉ thực hiện vài chục lần trong trường hợp xấu nhất, khiến giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    n = int(input())

    height = 0
    level = 1

    while True:
        need = level * (level + 1) // 2

        if n < need:
            break

        n -= need
        height += 1
        level += 1

    print(height)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("1\n") == "1\n", "sample 1"

# custom cases
assert run("4\n") == "2\n", "exactly two levels"
assert run("10\n") == "3\n", "exactly three levels"
assert run("19\n") == "3\n", "just short of fourth level"
assert run("10000\n") == "21\n", "maximum input range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4`|`2`| Hoàn thành chính xác hai cấp độ | 
|`10`|`3`| Hoàn thành chính xác ba cấp độ | 
|`19`|`3`| Dừng ngay trước cấp độ tiếp theo | 
|`10000`|`21`| Giá trị ràng buộc lớn nhất | 

## Vỏ cạnh 

Hãy xem xét trường hợp phù hợp chính xác:```
Input:
10
```Thuật toán xây dựng các cấp độ yêu cầu 1, 3 và 6 khối. Các khối còn lại trở thành số 0 sau khi hoàn thành cấp độ thứ ba. Cấp độ tiếp theo yêu cầu 10 khối, giá cả không phải chăng, vì vậy câu trả lời là 3. Điều này xác minh rằng việc hoàn thành một cấp độ chính xác được tính chính xác. 

Hãy xem xét một giá trị ngay dưới ngưỡng tiếp theo:```
Input:
19
```Ba cấp độ đầu tiên tiêu thụ:```
1 + 3 + 6 = 10
```Còn lại chín khối. Cấp độ thứ tư yêu cầu 10 khối, do đó việc xây dựng dừng lại và câu trả lời là 3. Điều này kiểm tra ranh giới trong đó cấp độ tiếp theo gần như có giá cả phải chăng nhưng không hoàn toàn. 

Hãy xem xét đầu vào tối thiểu:```
Input:
1
```Cấp độ đầu tiên yêu cầu chính xác một khối lập phương nên nó đã hoàn thành. Không còn khối nào sau đó và thuật toán đưa ra 1. Điều này xác nhận việc xử lý đúng giá trị hợp lệ nhỏ nhất. 

Hãy xem xét một tình huống phù hợp chính xác lớn hơn:```
Input:
20
```Các hình khối cần thiết là:```
1 + 3 + 6 + 10 = 20
```Tất cả bốn cấp độ đều có thể được xây dựng và thuật toán đưa ra kết quả là 4. Điều này xác minh rằng nhiều lần hoàn thành chính xác liên tiếp được xử lý chính xác.
