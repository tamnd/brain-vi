---
title: "CF 197A - Đĩa Game"
description: "Chúng ta có một cái bàn hình chữ nhật có kích thước a × b và có vô số tấm hình tròn giống hệt nhau có bán kính r. Hai người chơi luân phiên đặt các đĩa lên bàn. Mỗi tấm phải nằm hoàn toàn bên trong hình chữ nhật, các tấm có thể chạm vào nhau nhưng không được chồng lên nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 197
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 124 (Div. 2)"
rating: 1600
weight: 197
solve_time_s: 82
verified: true
draft: false
---

[CF 197A - Trò chơi trên đĩa](https://codeforces.com/problemset/problem/197/A) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, trò chơi, toán học 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có một cái bàn hình chữ nhật có kích thước`a × b`và nguồn cung cấp không giới hạn các tấm hình tròn giống hệt nhau có bán kính`r`. Hai người chơi luân phiên đặt các đĩa lên bàn. Mỗi tấm phải nằm hoàn toàn bên trong hình chữ nhật, các tấm có thể chạm vào nhau nhưng không được chồng lên nhau. 

Trò chơi kết thúc khi người chơi hiện tại không thể đặt thêm đĩa khác. Cả hai người chơi đều chơi tối ưu và chúng ta phải xác định xem người chơi thứ nhất hay thứ hai sẽ thắng. 

Hình học đơn giản hơn nhiều so với lần đầu tiên nó xuất hiện. Một tấm bán kính`r`chỉ vừa khít bên trong bàn nếu tâm của nó ít nhất vẫn ở vị trí`r`các đơn vị ở xa mọi biên giới. Điều đó có nghĩa là tâm phải nằm bên trong một hình chữ nhật có kích thước nhỏ hơn`(a - 2r) × (b - 2r)`. 

Các ràng buộc rất nhỏ, mọi giá trị đều nhiều nhất`100`, do đó, ngay cả mô phỏng hình học mạnh mẽ về mặt kỹ thuật cũng sẽ nằm trong giới hạn. Thử thách thực sự là nhận ra cấu trúc trò chơi và đơn giản hóa hình học một cách chính xác. 

Trường hợp cạnh chính là khi có chính xác một tấm vừa khít. Ví dụ:```
5 5 2
```Câu trả lời đúng là`"First"`bởi vì một tấm có thể được đặt ở giữa và sau đó trò chơi kết thúc. 

Một cách tiếp cận bất cẩn có thể nghĩ sai rằng có thể có nhiều vị trí hơn vì diện tích chưa được che chắn còn lại rất lớn. Hạn chế thực tế đến từ sự chồng chéo vòng tròn chứ không phải khu vực chưa sử dụng. 

Một trường hợp tinh tế khác là khi không có tấm nào vừa khít:```
3 3 2
```Đường kính là`4`, lớn hơn cả hai kích thước bàn, do đó không thể đặt được ngay cả một tấm đơn. Câu trả lời đúng là`"Second"`bởi vì người chơi đầu tiên không được di chuyển. 

Một lỗi phổ biến là kiểm tra`a >= r`Và`b >= r`thay vì yêu cầu chỗ cho đường kính đầy đủ. Điều kiện đúng là`a >= 2r`Và`b >= 2r`. 

Ngoài ra còn có một trường hợp chạm vào ranh giới:```
4 7 2
```Đường kính tấm bằng chính xác chiều cao của bàn. Điều này vẫn hợp lệ vì được phép chạm vào đường viền. Một tấm vừa vặn, vì vậy câu trả lời là`"First"`. 

Việc triển khai sử dụng các bất đẳng thức nghiêm ngặt thay vì các bất đẳng thức không nghiêm ngặt sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là mô hình hóa trò chơi về mặt hình học. Người ta có thể thử tạo mọi vị trí đĩa hợp lệ, kiểm tra sự trùng lặp với các vòng tròn đã đặt trước đó và đánh giá đệ quy các trạng thái thắng và thua bằng minimax. 

Cách tiếp cận đó đúng về nguyên tắc vì trò chơi là hữu hạn và mang tính quyết định. Vấn đề là không gian trạng thái bùng nổ ngay lập tức. Ngay cả sau khi rời rạc hóa các vị trí, số lượng cấu hình vẫn trở nên rất lớn vì tâm tấm có thể thay đổi liên tục. Việc tìm kiếm trò chơi hình học đầy đủ là hoàn toàn không thực tế. 

Quan sát mang tính đột phá là bảng không bao giờ có thể chứa nhiều hơn một tấm. 

Một tấm có đường kính`2r`. Nếu hai chiếc đĩa được đặt trên bàn thì tâm của chúng ít nhất phải bằng nhau.`2r`riêng biệt. Nhưng bất kỳ trung tâm hợp lệ nào cũng phải nằm trong hình chữ nhật:```
[r, a-r] × [r, b-r]
```Nếu cả hai`a < 4r`hoặc`b < 4r`, không có đủ chỗ để ngăn cách hai tâm bằng khoảng cách`2r`. 

Trực tiếp hơn, thủ thuật Codeforces ban đầu thậm chí còn đơn giản hơn. Vì cả hai tấm phải nằm hoàn toàn bên trong bàn, nên đặt hai vòng tròn bán kính không chồng lên nhau`r`yêu cầu ít nhất một thứ nguyên bảng phải có ít nhất`4r`. Dưới các ràng buộc của bài toán và hình học, điều duy nhất quan trọng là liệu có ít nhất một tấm phù hợp hay không. 

Nếu không có đĩa nào vừa, người chơi đầu tiên sẽ thua ngay lập tức. 

Nếu có ít nhất một tấm vừa khít, người chơi đầu tiên đặt nó vào và thắng vì không có nước đi thứ hai. 

Vì vậy, trò chơi giảm xuống một điều kiện duy nhất:```
a >= 2r and b >= 2r
```Nếu đúng thì in`"First"`. Nếu không thì in`"Second"`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Trạng thái lũy thừa / vô hạn | Rất lớn | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng`a`Và`b`, cùng với bán kính tấm`r`. 
2. Kiểm tra xem một chiếc đĩa có thể đặt vừa trên bàn hay không. 

Một tấm chỉ vừa khít nếu đường kính của nó`2r`không vượt quá kích thước của bảng. Điều này đưa ra điều kiện:$a \ge 2r \text{ and } b \ge 2r$3. Nếu điều kiện đúng, in`"First"`. 

Người chơi đầu tiên đặt chiếc đĩa duy nhất có thể và không được phép di chuyển hợp pháp. 
4. Nếu không, hãy in`"Second"`. 

Không thể đặt đĩa nào cả nên người chơi đầu tiên sẽ thua ngay lập tức. 

### Tại sao nó hoạt động 

Toàn bộ trò chơi xoay quanh việc liệu nước đi đầu tiên hợp pháp có tồn tại hay không. Một tấm bán kính`r`cần một hình vuông cạnh`2r`chỉ để vừa với cái bàn. Nếu một trong hai kích thước bảng nhỏ hơn`2r`, không có chuyển động nào tồn tại. 

Nếu có nước đi, người chơi đầu tiên luôn có thể đặt một đĩa. Sau đó, không thể thêm một tấm khác nếu không chồng lên nhau vì không còn đủ chỗ cho hình tròn thứ hai có cùng bán kính. Trò chơi luôn kéo dài không nước đi hoặc một nước đi, vì vậy người chiến thắng hoàn toàn được xác định bởi sự tồn tại của nước đi đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, r = map(int, input().split())

    if a >= 2 * r and b >= 2 * r:
        print("First")
    else:
        print("Second")

solve()
```Việc triển khai phản ánh trực tiếp quan sát toán học từ hướng dẫn. 

biểu hiện`2 * r`là đường kính tấm. Một tấm chỉ vừa vặn nếu cả hai kích thước của bàn ít nhất phải lớn như vậy. 

Việc so sánh phải sử dụng`>=`thay vì`>`. Vấn đề rõ ràng cho phép chạm vào đường viền, do đó, một tấm có đường kính khớp chính xác với kích thước bảng vẫn hợp lệ. 

Không cần vòng lặp hoặc bộ nhớ bổ sung vì kết quả phụ thuộc vào một lần kiểm tra hình học duy nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5 2
```| Biến | Giá trị | 
| --- | --- | 
| một | 5 | 
| b | 5 | 
| r | 2 | 
| 2r | 4 | 
| a >= 2r | Đúng | 
| b >= 2r | Đúng | 
| Đầu ra | Đầu tiên | 

Một đường tròn có đường kính`4`phù hợp bên trong một`5 × 5`bàn. Người chơi đầu tiên đặt một đĩa và không có động thái nào nữa. 

### Mẫu 2 

đầu vào:```
3 3 2
```| Biến | Giá trị | 
| --- | --- | 
| một | 3 | 
| b | 3 | 
| r | 2 | 
| 2r | 4 | 
| a >= 2r | Sai | 
| b >= 2r | Sai | 
| Đầu ra | Thứ hai | 

Đường kính lớn hơn cả hai kích thước của bàn nên không có vị trí hợp pháp nào tồn tại. Người chơi đầu tiên thua ngay lập tức. 

Những dấu vết này xác nhận tính bất biến cốt lõi: kết quả của trò chơi chỉ phụ thuộc vào việc liệu vị trí đầu tiên hợp pháp có tồn tại hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và so sánh | 
| Không gian | O(1) | Không sử dụng thêm bộ nhớ | 

Các ràng buộc là cực kỳ nhỏ, nhưng lời giải theo thời gian không đổi thậm chí còn mạnh hơn. Chương trình thực hiện một phép kiểm tra hình học duy nhất và ngay lập tức đưa ra câu trả lời, dễ dàng khớp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a, b, r = map(int, input().split())

    if a >= 2 * r and b >= 2 * r:
        print("First")
    else:
        print("Second")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("5 5 2\n") == "First\n", "sample 1"

# no plate fits
assert run("3 3 2\n") == "Second\n", "no valid move"

# exact boundary fit
assert run("4 7 2\n") == "First\n", "touching border allowed"

# minimum values
assert run("1 1 1\n") == "Second\n", "smallest table"

# large valid case
assert run("100 100 50\n") == "First\n", "maximum exact fit"

# one dimension too small
assert run("10 3 2\n") == "Second\n", "fails on one dimension only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3 2`|`Second`| Không có tấm nào có thể vừa | 
|`4 7 2`|`First`| Được phép chạm vào biên giới | 
|`1 1 1`|`Second`| Vỏ cạnh kích thước tối thiểu | 
|`100 100 50`|`First`| Trường hợp phù hợp chính xác tối đa | 
|`10 3 2`|`Second`| Cả hai chiều phải thỏa mãn điều kiện | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
4 7 2
```Đường kính là`4`. Chiều cao của bàn cũng chính xác`4`, do đó đường tròn chạm vào đường viền trên và dưới cùng một lúc. 

Thuật toán kiểm tra:```
4 >= 4  -> True
7 >= 4  -> True
```và in`"First"`. 

Điều này xác nhận việc triển khai xử lý chính xác các bất bình đẳng không nghiêm ngặt. 

Bây giờ hãy xem xét:```
3 3 2
```Đường kính là`4`, lớn hơn cả hai chiều. Việc kiểm tra trở thành:```
3 >= 4  -> False
3 >= 4  -> False
```Thuật toán in`"Second"`bởi vì không có vị trí hợp pháp tồn tại. 

Cuối cùng, hãy xem xét một trường hợp hỗn hợp:```
10 3 2
```Chiều rộng đủ lớn nhưng chiều cao thì không:```
10 >= 4 -> True
3 >= 4  -> False
```Thuật toán vẫn in`"Second"`vì tấm phải vừa khít hoàn toàn theo cả hai chiều. Một giải pháp sai phổ biến chỉ kiểm tra một chiều hoặc sử dụng lý luận dựa trên khu vực, cả hai đều không thành công ở đây.
