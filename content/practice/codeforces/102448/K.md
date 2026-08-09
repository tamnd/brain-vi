---
title: "CF 102448K - Kongey Donk"
description: "Hãy nghĩ về những cái cây như những cột của một mạng lưới. Có n cây được đánh số từ trái qua phải, mỗi cây có h vị trí quả chuối được đánh số từ trên xuống dưới. Đầu vào cung cấp số lượng chuối ở mỗi ô của lưới n × h này. Kongey có thể bắt đầu ở ngọn của bất kỳ cây nào."
date: "2026-08-08T12:43:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "K"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 926
verified: true
draft: false
---

[CF 102448K - Kongey Donk](https://codeforces.com/problemset/problem/102448/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 15 phút 26 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy nghĩ về những cái cây như những cột của một mạng lưới. có`n`cây được đánh số từ trái qua phải và mỗi cây đều có`h`vị trí chuối, đánh số từ trên xuống dưới. Đầu vào cung cấp số lượng chuối ở mỗi ô của ô này`n × h`lưới. 

Kongey có thể bắt đầu ở ngọn của bất kỳ cây nào. Từ một vị trí, anh ta có thể di chuyển xuống trong cùng một cây hoặc di chuyển đến một trong hai cây lân cận, nhưng mỗi lần nhảy phải kết thúc ở vị trí thấp hơn nghiêm ngặt. Mục tiêu của anh ấy là chạm tới đáy đồng thời thu thập càng nhiều chuối càng tốt. 

Hệ quả hình học quan trọng là chúng ta có thể hạn chế chuyển động từ độ cao này sang độ cao tiếp theo ngay lập tức. Giả sử một bước nhảy hợp lệ đi từ độ cao`r`trên cây`i`chiều cao`r + k`trên cây`i + 1`, Ở đâu`k > 1`. Thay vào đó, Kongey có thể di chuyển xuống bên trong cây`i`qua các vị trí còn thiếu rồi chuyển sang cây`i + 1`ở độ cao`r + k`. Tất cả những vị trí bổ sung đó đều chứa số lượng chuối không âm, vì vậy việc lấy chúng không thể làm cho kết quả trở nên tồi tệ hơn. Đối số tương tự hoạt động khi đích đến là cùng một cây. 

Do đó, một giải pháp tối ưu có thể được xem như một đường dẫn qua lưới trong đó mỗi bước đi từ độ cao`r`chiều cao`r + 1`và chỉ số cây thay đổi nhiều nhất là một. 

Những ràng buộc cho chúng ta tới`n * h = 10^6`tế bào. Điều này đủ lớn để một thuật toán thực hiện phép tính bậc hai theo số lượng ô là không thể, nhưng nó đủ nhỏ để thực hiện một lượng công việc không đổi trên mỗi ô. Các giới hạn riêng biệt của`2 * 10^5`trên cả hai chiều cũng loại trừ các thuật toán giữ cấu trúc bậc hai ở một trong hai chiều. Một đường truyền tuyến tính trên tất cả`n * h`vị trí chính xác là quy mô chúng tôi muốn. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ quá trình triển khai. 

Đầu tiên là một cây duy nhất. Ví dụ:```
1 3
1 2 3
```Câu trả lời là`6`, bởi vì Kongey đơn giản đi xuống ở cả ba vị trí. Việc triển khai giả định cả hai cây lân cận luôn tồn tại có thể truy cập vào một chỉ mục không hợp lệ hoặc vô tình sử dụng hành vi chỉ mục phủ định của Python. 

Thứ hai là một cấp độ duy nhất:```
2 1
5
7
```Câu trả lời là`7`. Kongey có thể bắt đầu trên một trong hai cây và không có quá trình chuyển đổi đi xuống vì vị trí trên cùng đã là ở dưới cùng. Việc triển khai DP chỉ khởi tạo câu trả lời của nó sau khi xử lý quá trình chuyển đổi sẽ bỏ lỡ trường hợp này. 

Thứ ba là các vị trí có giá trị bằng 0 vẫn là trạng thái hợp lệ:```
2 3
5 0 0
0 0 7
```Câu trả lời là`12`, sử dụng đường dẫn`5 -> 0 -> 7`. Việc coi số 0 là trạng thái không thể truy cập sẽ loại bỏ đường dẫn này một cách không chính xác. 

Thứ tư là số tiền tích lũy lớn. Ví dụ:```
1 5
1000000 1000000 1000000 1000000 1000000
```Câu trả lời là`5000000`, và với`h`lớn như`200000`, câu trả lời có thể đạt được`2 * 10^11`. Việc triển khai C++ cần loại số nguyên 64 bit. Số nguyên Python đã xử lý phạm vi này một cách an toàn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp bắt đầu từ mọi vị trí trên cùng có thể và thử đệ quy mọi cây hợp pháp tiếp theo. Tại cây bên trong có thể có ba lựa chọn, ở trên cùng một cây hoặc di chuyển sang trái hoặc phải. Gần điểm cuối có ít lựa chọn hơn, nhưng hệ số phân nhánh trong trường hợp xấu nhất vẫn là ba. Với`h`cấp độ, điều này mang lại cho`3^(h-1)`các đường đi từ một cây khởi đầu, do đó việc liệt kê tất cả các đường đi mất thời gian theo cấp số nhân, đại khái là`O(n * h * 3^(h-1))`nếu tổng chuối được đánh giá rõ ràng cho mọi đường dẫn. Ngay cả chiều cao 30 cũng đã mang lại nhiều hơn thế`2 * 10^14`trình tự chuyển tiếp có thể có. Ràng buộc sản phẩm`n * h <= 10^6`không giúp ích gì cho thuật toán hàm mũ vì bản thân chiều cao có thể`2 * 10^5`. 

Phương pháp brute-force hoạt động vì mọi tuyến đường hoàn chỉnh đều được kiểm tra, do đó nó không thể bỏ lỡ tuyến đường tối ưu. Nó không thành công vì các tuyến đường khác nhau liên tục chia sẻ cùng một tiền tố. Ví dụ: khi chúng ta biết số lượng chuối tốt nhất được thu thập khi đến cây`i`ở độ cao`r`, không có lý do gì để nhớ chính xác lộ trình đã tạo ra giá trị đó. Mọi bước di chuyển trong tương lai chỉ phụ thuộc vào cây và chiều cao hiện tại. 

Quan sát đó đưa ra trạng thái lập trình động. Cho phép`dp[i][r]`là số lượng chuối tối đa có thể thu thập được khi Kongey đạt đến độ cao`r`trên cây`i`. 

Ở độ cao 0, Kongey có thể xuất phát trên bất kỳ cây nào, vì vậy:`dp[i][0] = a[i][0]`. 

Đối với mỗi độ cao sau này, vị trí trước đó phải nằm trên cùng một cây, cây ngay bên trái hoặc cây ngay bên phải. Vì thế:`dp[i][r] = a[i][r] + max(dp[i-1][r-1], dp[i][r-1], dp[i+1][r-1])`nơi những người hàng xóm không tồn tại đơn giản bị bỏ qua. 

Cái nhìn sâu sắc quan trọng là phép truy toán chỉ sử dụng chiều cao trước đó. Chúng ta không cần phải xem xét mọi độ cao trước đó, bởi vì bất kỳ tuyến đường nào đã bỏ qua một cấp độ đều có thể được thay thế bằng tuyến đường đi xuống từng cấp độ một và số lượng chuối được chèn vào là không âm. 

Có chính xác`n * h`tiểu bang và chỉ có ba kiểm tra tiền thân cho mỗi tiểu bang. Điều đó biến tìm kiếm theo cấp số nhân thành quét tuyến tính của lưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * h * 3^(h-1))`|`O(h)`trạng thái đệ quy/đường dẫn | Quá chậm | 
| Tối ưu |`O(n * h)`|`O(n * h)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới chuối, ở đâu`a[i][r]`là số chuối ở độ cao`r`của cây`i`. Chúng tôi giữ lại lưới hoàn chỉnh vì đầu vào được sắp xếp theo cây, trong khi DP cần xử lý nó theo chiều cao. 
2. Coi vị trí đầu tiên của mỗi cây là trạng thái bắt đầu hợp lệ. Bộ`a[i][0]`để thể hiện giá trị tốt nhất để tiếp cận ô đó, vì Kongey có thể nhảy trực tiếp từ nền lên ngọn của bất kỳ cây nào. 
3. Xử lý độ cao từ`1`bởi vì`h - 1`. Đối với mỗi cây`i`, hãy nhìn vào ba cây tiền thân có thể có:`i - 1`,`i`, Và`i + 1`. 
4. Lấy giá trị tiền nhiệm lớn nhất và thêm chuối vào vị trí hiện tại. Điều này tính toán tuyến đường tốt nhất kết thúc chính xác tại ô hiện tại, bởi vì mọi chuyển đổi một cấp hợp pháp phải đến từ một trong ba cây đó. 
5. Lưu trực tiếp giá trị DP đã tính vào lưới. Khi xử lý chiều cao`r`, tất cả các giá trị ở độ cao`r - 1`đã được tính toán cho từng cây, trong khi chiều cao hiện tại vẫn chứa số lượng chuối ban đầu. Điều đó làm cho lưới có thể được sử dụng làm cả mảng đầu vào và bảng DP. 
6. Sau khi xử lý chiều cao cuối cùng, lấy giá trị lớn nhất trong số tất cả các cây. Kongey có thể chạm tới sàn thông qua bất kỳ cái cây nào, vì vậy trạng thái cuối cùng tốt nhất là câu trả lời. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý chiều cao`r`,`a[i][r]`bằng số lượng chuối tối đa có thể đạt được khi đạt được chính xác vị trí đó. Bất biến đúng ở độ cao bằng 0 vì mọi cây đều có thể được chọn làm cây bắt đầu. Để chuyển sang chiều cao`r`, mọi vị trí hợp lệ trước đó phải ở độ cao`r - 1`trên cùng một cây hoặc cây liền kề sau khi chuyển đổi các bước nhảy bị bỏ qua thành các bước di chuyển một cấp. Phép truy hồi kiểm tra chính xác những khả năng đó và chọn khả năng tốt nhất trước khi thêm chuối vào ô hiện tại. Do đó, bất biến vẫn đúng ở mọi độ cao và giá trị tối đa ở độ cao cuối cùng là số lượng chuối tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, h = map(int, input().split())

    dp = [list(map(int, input().split())) for _ in range(n)]

    ans = max(row[0] for row in dp)

    for r in range(1, h):
        for i in range(n):
            best = dp[i][r - 1]

            if i > 0 and dp[i - 1][r - 1] > best:
                best = dp[i - 1][r - 1]

            if i + 1 < n and dp[i + 1][r - 1] > best:
                best = dp[i + 1][r - 1]

            dp[i][r] += best

            if dp[i][r] > ans:
                ans = dp[i][r]

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc từng cây vào một danh sách. Vì tổng số giá trị đầu vào nhiều nhất là`10^6`, việc lưu trữ lưới là khả thi trong giới hạn bộ nhớ. 

Lớp DP đầu tiên không yêu cầu lặp lại. Kongey có thể chọn bất kỳ cây nào từ nền tảng, vì vậy mọi ô trên cùng đều có thể truy cập được bằng chính xác những quả chuối được viết ở đó. 

Đối với mỗi độ cao sau này,`dp[i][r - 1]`đại diện cho việc ở trên cây hiện tại,`dp[i - 1][r - 1]`đại diện cho việc đến từ bên trái, và`dp[i + 1][r - 1]`đại diện cho việc đến từ bên phải. Việc kiểm tra ranh giới là cần thiết vì cây`0`không còn hàng xóm và cây`n - 1`không có hàng xóm phù hợp. 

bản cập nhật`dp[i][r] += best`được thực hiện một cách có chủ ý sau khi đọc các giá trị trước đó. Vì tất cả các truy cập trước đó đều sử dụng cột`r - 1`, việc thay đổi ô hiện tại không thể ảnh hưởng đến bất kỳ phép tính nào khác ở độ cao này. 

Câu trả lời được cập nhật sau mỗi độ cao thay vì chỉ ở độ cao cuối cùng. Điều này là an toàn vì Kongey luôn có thể tiếp tục đi xuống từ một trạng thái, nhưng việc theo dõi mọi độ cao cũng khiến việc khởi tạo cho`h = 1`rõ ràng và tránh trường hợp đặc biệt. 

Không cần xử lý tràn trong Python. Câu trả lời tối đa có thể là nhiều nhất`10^6 * 2 * 10^5 = 2 * 10^11`, mà Python đại diện chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Lưới đầu vào là:```
Tree 1:  1  5  5
Tree 2:  9  0  0
Tree 3: 15  2  1
```Giá trị DP sau mỗi chiều cao là: 

| Chiều cao | Cây 1 | Cây 2 | Cây 3 | Tốt nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 9 | 15 | 15 | 
| 1 | 14 | 15 | 17 | 17 | 
| 2 | 20 | 17 | 18 | 20 | 

Ở độ cao 0, điểm xuất phát tốt nhất là đỉnh cây 3 với`15`chuối. Ở độ cao một, cây 3 có thể đạt được từ cây 2 hoặc cây 3, mang lại`2 + max(9, 15) = 17`. Ở dưới cùng của cây 1, cây tiền thân tốt nhất là cây 2 ở độ cao 1, cho`5 + max(14, 15) = 20`. 

Tuyến đường kết quả có thể là cây 3 ở độ cao 0, cây 2 ở độ cao một, sau đó là cây 1 ở độ cao hai. Giá trị của nó là`15 + 0 + 5 = 20`, phù hợp với đầu ra mẫu. 

### Mẫu 2 

Các trạng thái DP là: 

| Chiều cao | Cây 1 | Cây 2 | Cây 3 | Cây 4 | Cây 5 | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 10 | 30 | 50 | 1 | 50 | 
| 1 | 110 | 85 | 60 | 50 | 180 | 180 | 
| 2 | 112 | 140 | 285 | 181 | 189 | 285 | 
| 3 | 148 | 287 | 295 | 287 | 218 | 295 | 
| 4 | 296 | 297 | 298 | 298 | 290 | 298 | 
| 5 | 305 | 300 | 398 | 307 | 338 | 398 | 

Trạng thái mạnh nhất ở độ cao cuối cùng là cây 3 có giá trị`398`. Người tiền nhiệm của nó ở độ cao 4 có thể là cây 2, cây 3 hoặc cây 4 và giá trị của cây 3 thu được từ trạng thái tốt nhất trong số đó. 

Bảng này cũng cho thấy tại sao chỉ giữ chiều cao ngay trước đó là đủ. Mọi giá trị trong một hàng đều được lấy hoàn toàn từ hàng trước đó mà không cần phải nhớ đường dẫn cụ thể đã tạo ra từng hàng trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * h)`| Mỗi một trong số`n * h`các ô được xử lý một lần, với tối đa ba so sánh trước đó. | 
| Không gian |`O(n * h)`| Lưới đầu vào được sửa đổi tại chỗ để lưu trữ các giá trị DP. | 

Từ`n * h <= 10^6`, thuật toán chỉ thực hiện vài triệu thao tác đơn giản. Điều này phù hợp với giới hạn 1 giây, trong khi lưới được lưu trữ chứa tối đa một triệu số nguyên và vừa vặn thoải mái trong giới hạn bộ nhớ 256 MB trong môi trường lập trình cạnh tranh Python điển hình. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng cùng cách triển khai DP nhưng làm cho giao diện đầu vào/đầu ra có thể thay thế được, do đó các trường hợp có thể chạy như các xác nhận thông thường.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, h = map(int, input().split())
    dp = [list(map(int, input().split())) for _ in range(n)]

    ans = max(row[0] for row in dp)

    for r in range(1, h):
        for i in range(n):
            best = dp[i][r - 1]

            if i > 0 and dp[i - 1][r - 1] > best:
                best = dp[i - 1][r - 1]

            if i + 1 < n and dp[i + 1][r - 1] > best:
                best = dp[i + 1][r - 1]

            dp[i][r] += best
            ans = max(ans, dp[i][r])

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

# Provided sample 1
assert run("""\
3 3
1 5 5
9 0 0
15 2 1
""") == "20", "sample 1"

# Provided sample 2
assert run("""\
5 6
1 100 2 8 9 8
10 55 30 2 2 2
30 10 200 10 3 100
50 0 1 2 3 9
1 130 9 29 3 40
""") == "398", "sample 2"

# Minimum-size input
assert run("""\
1 1
7
""") == "7", "minimum size"

# Single tree, no horizontal movement
assert run("""\
1 5
1 2 3 4 5
""") == "15", "single tree"

# Boundary movement between two trees
assert run("""\
2 2
5 0
0 7
""") == "12", "boundary movement"

# All equal values
assert run("""\
3 4
1 1 1 1
1 1 1 1
1 1 1 1
""") == "4", "all equal values"

# Large product: 1000 * 1000 = 10^6 cells
n = 1000
h = 1000
large_input = f"{n} {h}\n" + (" ".join(["1"] * h) + "\n") * n
assert run(large_input) == "1000", "maximum product size"

# Large individual values
assert run("""\
1 5
1000000 1000000 1000000 1000000 1000000
""") == "5000000", "large sums"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 7`|`7`| Kích thước tối thiểu và không có chuyển tiếp | 
|`1 5 / 1 2 3 4 5`|`15`| Một cây duy nhất không có hàng xóm ngang | 
|`2 2 / 5 0 / 0 7`|`12`| Di chuyển đến cây ranh giới | 
|`3 4`với mọi giá trị đều bằng`1`|`4`| Chính xác một ô được truy cập ở mọi độ cao | 
|`1000 × 1000`lưới của những cái |`1000`| Đầy`n * h = 10^6`kích thước đầu vào | 
| Một cây có năm giá trị`10^6`|`5000000`| Giá trị số nguyên tích lũy lớn | 

## Vỏ cạnh 

Một cây duy nhất loại bỏ tất cả các lựa chọn theo chiều ngang. Vì```
1 3
1 2 3
```giá trị DP ban đầu là`1`. Ở độ cao một, cây tiền thân duy nhất là cùng một cây, do đó giá trị trở thành`2 + 1 = 3`. Ở độ cao hai nó trở thành`3 + 3 = 6`. Câu trả lời là`6`. Việc kiểm tra ranh giới ngăn việc triển khai coi cây lân cận không tồn tại như cây tiền thân thực sự. 

Một cấp độ duy nhất không có chuyển tiếp đi xuống. Vì```
2 1
5
7
```hai giá trị DP ban đầu là`5`Và`7`, và cực đại ngay lập tức`7`. Vòng lặp ở các độ cao sau này thực hiện 0 lần, do đó việc khởi tạo`ans`là những gì xử lý trường hợp này một cách chính xác. 

Các ô có giá trị bằng 0 vẫn có thể truy cập được. Vì```
2 3
5 0 0
0 0 7
```chiều cao đầu tiên có giá trị DP`5`Và`0`. Ở độ cao một, các giá trị trở thành`5`Và`5`. Ở độ cao thứ hai, cây 1 được`5`và cây 2 được`12`, bởi vì cái sau có thể theo sau`5 -> 0 -> 7`. Câu trả lời là`12`. Không có trạng thái nào bị loại bỏ chỉ vì số lượng chuối của nó bằng 0. 

Trường hợp hoàn toàn bằng nhau chứng minh tại sao câu trả lời không phải là tổng của mọi cây. Vì```
3 4
1 1 1 1
1 1 1 1
1 1 1 1
```lớp DP đầu tiên là`[1, 1, 1]`. Mỗi lớp tiếp theo là`[2, 2, 2]`, sau đó`[3, 3, 3]`, sau đó`[4, 4, 4]`. Ở mỗi độ cao Kongey chỉ chiếm đúng một cây nên giá trị lớn nhất là`4`, không`12`. 

Các giá trị lớn yêu cầu DP phải bảo toàn toàn bộ số tiền tích lũy. Với```
1 5
1000000 1000000 1000000 1000000 1000000
```các giá trị DP trở thành`1000000`,`2000000`,`3000000`,`4000000`, Và`5000000`. Thuật toán không thực hiện phép toán số học đặc biệt nào cho các giá trị lớn và các số nguyên có độ chính xác tùy ý của Python bảo toàn kết quả một cách chính xác.
