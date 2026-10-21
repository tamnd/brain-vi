---
title: "CF 105811A - Bể cá"
description: "Nhiệm vụ là vẽ một bể cá hình chữ nhật bằng ký tự ASCII. Bể luôn có kích thước cố định là 15 hàng và 25 cột. Đường viền sử dụng o, - và Đầu vào chứa một số nguyên biểu thị số lượng cá phải xuất hiện."
date: "2026-06-25T15:18:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105811
codeforces_index: "A"
codeforces_contest_name: "UT Open 2025"
rating: 0
weight: 105811
solve_time_s: 37
verified: true
draft: false
---

[CF 105811A - Bể cá](https://codeforces.com/problemset/problem/105811/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là vẽ một bể cá hình chữ nhật bằng ký tự ASCII. Bể luôn có kích thước cố định là 15 hàng và 25 cột. Biên giới sử dụng`o`,`-`, Và`|`, bên trong là khoảng trống. Chúng ta cần đặt chính xác`n`cá bên trong bể, trong đó mỗi con cá dài ba ký tự và trông giống như`><>`hoặc`<><`. Cá không thể chạm vào biên giới và hai con cá không thể chồng lên nhau. Đầu ra được yêu cầu chỉ đơn giản là bản vẽ đã hoàn thành. Bài toán yêu cầu một sự sắp xếp hợp lệ, không phải là một sự sắp xếp duy nhất. 

Dữ liệu đầu vào chứa một số nguyên biểu thị số lượng cá phải xuất hiện. Vì giới hạn chỉ tối đa 10 con cá nên thách thức chính không phải là hiệu suất mà là tìm ra một công trình đơn giản luôn để lại đủ vị trí hợp lệ. Bất kỳ cách tiếp cận nào thực hiện tìm kiếm trên các bản vẽ có thể đều không cần thiết. Việc xây dựng thời gian không đổi là đủ vì bản thân kích thước đầu ra được cố định ở mức 15 x 25 ký tự, nghĩa là chúng tôi chỉ tạo 375 ô. 

Một lỗi phổ biến là đặt cá mà không kiểm tra chiều rộng bên trong sẵn có. Một con cá chiếm ba cột liên tiếp, do đó, đặt một cột ở hai cột cuối cùng của khu vực bên trong sẽ khiến nó vượt qua ranh giới. 

Ví dụ:```
1
```Đầu ra không thể đặt cá bắt đầu từ cột 23 của hàng vì chiều rộng bể kết thúc ở cột 24. Ý tưởng đúng là chỉ sử dụng các vị trí bên trong an toàn. 

Một trường hợp khác là khi tất cả cá được xếp vào cùng một hàng. Việc thực hiện bất cẩn có thể khiến họ chạm vào nhau. Ví dụ:```
3
```Một hàng như:```
|><><><><>|
```không hợp lệ vì cá chồng lên nhau. Con cá phải được phân tách bằng ít nhất một ký tự trống. 

Trường hợp cạnh cuối cùng là số lượng cá tối đa. Vì chỉ có 10 con cá nên công trình phải dành đủ không gian cho tất cả chúng. Mẫu hàng và cột cố định sẽ tránh hết dung lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi vị trí đặt cá có thể có trong bể, kiểm tra xem mỗi bức vẽ ứng cử viên có chứa chính xác số lượng cá không chồng lên nhau theo yêu cầu hay không. Điều này đúng vì cuối cùng mọi bản vẽ có thể đều được xem xét, nhưng không gian tìm kiếm rất lớn so với đầu vào nhỏ bé. Có hàng trăm vị trí có thể có cho mỗi con cá và việc thử kết hợp các vị trí sẽ tăng theo cấp số nhân. Ngay cả trước khi kiểm tra tất cả các cách sắp xếp, số lượng ứng viên vẫn lớn hơn nhiều so với số lượng cần thiết cho bài toán đầu ra có kích thước cố định. 

Nhận xét làm thay đổi vấn đề là trọng tài chỉ xác minh xem bức ảnh cuối cùng có hợp lệ hay không. Nó không quan tâm đến vị trí hay hướng đi của cá. Vì chỉ có 10 con cá có thể có nên chúng ta có thể chuẩn bị một bố cục xác định với đủ các ô và điền vào ô đầu tiên.`n`khe cắm. Vấn đề trở thành một vấn đề xây dựng đơn giản. 

Bể có thể được khởi động lại hoàn toàn, sau đó cá có thể được đưa vào vị trí an toàn. Việc chọn các vị trí cách nhau bằng các ô trống sẽ đảm bảo không có hai con cá nào trùng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng vị trí có thể có | O(15 * 25) | Quá chậm và không cần thiết | 
| Tối ưu | O(15 * 25) | O(15 * 25) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một lưới 15 x 25 chứa đầy khoảng trắng. Điều này cho chúng ta phần bên trong trống rỗng của bể. 
2. Vẽ đường viền bằng cách đặt`o`ở bốn góc,`-`trên và dưới, và`|`dọc theo bên trái và bên phải. Điều này tạo lại hình dạng bể cần thiết. 
3. Chuẩn bị danh sách các vị trí an toàn có thể đặt cá. Mỗi vị trí phải nằm trong đường viền và có sẵn ba cột liên tiếp. 
4. Đối với mỗi con cá được yêu cầu, hãy đặt một`><>`mẫu ở vị trí có sẵn tiếp theo. Hướng chính xác không quan trọng vì yêu cầu duy nhất là mỗi con cá phải có một trong hai hình dạng hợp lệ. 
5. In từng hàng lưới đã hoàn thành. 

Tại sao nó hoạt động: 

Điều duy nhất có thể khiến bức vẽ không hợp lệ là hình con cá bị sai, chồng lên nhau hoặc chạm vào đường viền. Mọi vị trí được chọn đều ở bên trong xe tăng và có đủ chỗ cho ba nhân vật. Vì các vị trí được chọn mà không cần sử dụng lại và có khoảng cách nên mỗi con cá được chèn vào đều độc lập. Sau khi đặt chính xác`n`cá, lưới chứa chính xác số lượng cá hợp lệ cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    h, w = 15, 25
    grid = [[' ' for _ in range(w)] for _ in range(h)]

    for j in range(w):
        grid[0][j] = '-'
        grid[h - 1][j] = '-'

    for i in range(h):
        grid[i][0] = '|'
        grid[i][w - 1] = '|'

    grid[0][0] = 'o'
    grid[0][w - 1] = 'o'
    grid[h - 1][0] = 'o'
    grid[h - 1][w - 1] = 'o'

    positions = []
    for r in range(1, h - 1):
        for c in range(2, w - 3, 4):
            positions.append((r, c))

    for i in range(n):
        r, c = positions[i]
        grid[r][c] = '>'
        grid[r][c + 1] = '<'
        grid[r][c + 2] = '>'

    print('\n'.join(''.join(row) for row in grid))

if __name__ == "__main__":
    solve()
```The grid is represented as a mutable list of character lists because individual cells need to be changed. Building the border first guarantees the container is valid before adding anything inside it.

 The position generation starts from an interior row and leaves enough distance between starting columns. Bước đi của bốn cột là có chủ ý. A fish uses three columns, and the extra column between fish prevents accidental touching.

 Vòng lặp chèn chỉ sử dụng đầu tiên`n`các vị trí được tạo ra. Vì vấn đề đảm bảo`n`nhiều nhất là 10, danh sách luôn chứa đủ vị trí an toàn. 

Việc chuyển đổi đầu ra nối mỗi hàng thành một chuỗi. Không cần xử lý đặc biệt khoảng trắng vì Python giữ nguyên chúng khi nối các ký tự. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2
```Thuật toán chọn hai vị trí đầu tiên. 

| Bước | Số lượng cá được đặt | Hành động hiện tại | 
| --- | --- | --- | 
| 1 | 0 | Bể rỗng có viền | 
| 2 | 1 | Đặt đầu tiên`><>`| 
| 3 | 2 | Vị trí thứ hai`><>`| 

Kết quả có hai con cá tách biệt. Dấu vết này chứng tỏ rằng quy tắc sắp xếp giữ cho cá luôn độc lập. 

Đối với đầu vào:```
5
```Thuật toán tiếp tục lấp đầy các vị trí có sẵn tiếp theo. 

| Bước | Số lượng cá được đặt | Hành động hiện tại | 
| --- | --- | --- | 
| 1 | 0 | Tạo xe tăng | 
| 2 | 1 | Đặt cá vào ô đầu tiên | 
| 3 | 2 | Đặt cá vào ô thứ hai | 
| 4 | 3 | Đặt cá ở ô thứ ba | 
| 5 | 4 | Đặt cá vào ô thứ tư | 
| 6 | 5 | Đặt cá vào ô thứ năm | 

Dấu vết cho thấy rằng ngay cả với số lượng yêu cầu lớn nhất, việc xây dựng tương tự vẫn tiếp tục mà không có bất kỳ xung đột nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(15 * 25) | Lưới có số ô cố định và mỗi ô được xử lý một số lần không đổi | 
| Không gian | O(15 * 25) | Toàn bộ bản vẽ được lưu trữ trước khi in | 

Các ràng buộc rất nhỏ và giải pháp thực hiện một lượng công việc cố định bất kể số lượng cá. Thuật toán dễ dàng phù hợp với mọi giới hạn thời gian của cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())

    h, w = 15, 25
    grid = [[' ' for _ in range(w)] for _ in range(h)]

    for j in range(w):
        grid[0][j] = '-'
        grid[h - 1][j] = '-'

    for i in range(h):
        grid[i][0] = '|'
        grid[i][w - 1] = '|'

    grid[0][0] = 'o'
    grid[0][w - 1] = 'o'
    grid[h - 1][0] = 'o'
    grid[h - 1][w - 1] = 'o'

    positions = []
    for r in range(1, h - 1):
        for c in range(2, w - 3, 4):
            positions.append((r, c))

    for i in range(n):
        r, c = positions[i]
        grid[r][c] = '>'
        grid[r][c + 1] = '<'
        grid[r][c + 2] = '>'

    ans = '\n'.join(''.join(row) for row in grid)
    sys.stdin = old_stdin
    return ans

def count_fish(out):
    return out.count('><>') + out.count('<><')

assert count_fish(run("1\n")) == 1, "minimum case"
assert count_fish(run("10\n")) == 10, "maximum case"
assert count_fish(run("5\n")) == 5, "sample style case"
assert count_fish(run("3\n")) == 3, "spacing case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| Một con cá | Xử lý kích thước tối thiểu | 
|`10`| Mười con cá | Số lượng cá tối đa cho phép | 
|`5`| Năm con cá | Nhiều vị trí | 
|`3`| Ba con cá | Khoảng cách giữa các cá | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu:```
1
```Thuật toán tạo đường viền và chỉ chèn vị trí cá đầu tiên. Vì vị trí đó không nằm trên đường viền và có chính xác ba ô khả dụng nên kết quả đầu ra chứa một con cá hợp lệ. 

Đối với trường hợp tối đa:```
10
```Danh sách vị trí được tạo chứa đủ vị trí vì nó sử dụng nhiều hàng. Thuật toán không bao giờ cố gắng sử dụng lại một ô, vì vậy tất cả mười con cá vẫn tách biệt. 

Đối với trường hợp khoảng cách:```
3
```Cá được xếp ở các cột cách nhau 4 vị trí. Một con cá chiếm ba cột, sau đó để lại một cột trống. Điều này ngăn ngừa việc vẽ nhầm trong đó hai con cá hợp nhất thành một mẫu dài hơn không hợp lệ.
