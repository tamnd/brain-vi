---
title: "CF 172B - Chu kỳ chuỗi giả ngẫu nhiên"
description: "Chúng ta được cung cấp một trình tạo đồng dư tuyến tính, một công thức chuỗi giả ngẫu nhiên cổ điển: $$ri = (a cdot r{i-1} + b) bmod m$$ Chuỗi bắt đầu từ r0 và mọi giá trị tiếp theo được tính từ giá trị trước đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 172
codeforces_index: "B"
codeforces_contest_name: "Croc Champ 2012 - Qualification Round"
rating: 1200
weight: 172
solve_time_s: 213
verified: true
draft: false
---

[CF 172B - Chu kỳ trình tự giả ngẫu nhiên](https://codeforces.com/problemset/problem/172/B) 

**Đánh giá:** 1200 
**Tags:** *đặc biệt, thực hiện, lý thuyết số 
**Thời gian giải:** 3m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một trình tạo đồng dư tuyến tính, một công thức chuỗi giả ngẫu nhiên cổ điển:$$r_i = (a \cdot r_{i-1} + b) \bmod m$$Trình tự bắt đầu từ`r0`, and every next value is computed from the previous one. Vì mọi giá trị đều được lấy modulo`m`, chỉ có`m`các trạng thái có thể:`0`bởi vì`m - 1`. 

Nhiệm vụ không phải là tìm khi trình tự lặp lại lần đầu tiên. Chúng tôi đặc biệt cần độ dài của chu kỳ lặp lại mà cuối cùng sẽ xuất hiện. Một chuỗi có thể có một số tiền tố không lặp lại trước khi bước vào chu trình. 

Ví dụ: với:```
a = 2
b = 6
m = 12
r0 = 11
```trình tự trở thành:```
11 -> 4 -> 2 -> 10 -> 2 -> 10 -> ...
```giá trị`11`chỉ xuất hiện một lần. Sau đó, trình tự luân phiên mãi mãi giữa`2`Và`10`, vậy chu kì là`2`. 

Ràng buộc`m ≤ 100000`là quan sát chính. Vì mọi giá trị được tạo đều nằm trong khoảng`0`Và`m - 1`, trình tự có thể truy cập nhiều nhất`m`trạng thái riêng biệt trước khi một số giá trị lặp lại. Điều đó ngay lập tức loại trừ bất kỳ mối quan tâm khám phá vô hạn nào. Thậm chí một`O(m)`hoặc`O(m log m)`giải pháp dễ dàng phù hợp trong giới hạn. 

Một cách tiếp cận ngây thơ so sánh từng cặp vị trí sẽ trở thành phương trình bậc hai. Với`m = 100000`, MỘT`O(m²)`thuật toán có thể yêu cầu khoảng`10^10`hoạt động vượt xa thời hạn. 

Phần khó khăn là phân biệt giai đoạn tiền kỳ với chu kỳ thực tế. 

Hãy xem xét đầu vào này:```
0 0 10 7
```Trình tự là:```
7 -> 0 -> 0 -> 0 -> ...
```Câu trả lời đúng là`1`, không`2`. Việc triển khai bất cẩn để đo khoảng cách giữa lần xuất hiện đầu tiên và lần thứ hai của bất kỳ giá trị nào có thể tính không chính xác một phần của tiền kỳ. 

Một trường hợp tinh tế khác là khi chu kỳ bắt đầu ngay lập tức:```
1 1 5 0
```Trình tự trở thành:```
0 -> 1 -> 2 -> 3 -> 4 -> 0 -> ...
```Toàn bộ chuỗi đều có tính tuần hoàn ngay từ đầu, vì vậy câu trả lời là`5`. 

Trường hợp cạnh thứ ba xảy ra khi chuỗi đạt đến một điểm cố định:```
5 3 100 15
```Giả sử chuỗi cuối cùng trở thành:```
78 -> 78 -> 78 -> ...
```Thời kỳ vẫn còn`1`, bởi vì một giá trị duy nhất lặp lại mãi mãi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo trình tự từng bước và lưu trữ mọi giá trị được tạo ra. Bất cứ khi nào một giá trị xuất hiện lại, hãy thử xác định độ dài chu kỳ bằng cách so sánh các phần tử trong tương lai. 

Điều này hoạt động vì trình tự là xác định. Khi chúng ta xem lại trạng thái trước đó, mọi thứ sau thời điểm đó sẽ lặp lại giống hệt nhau. Vấn đề là hiệu quả. Nếu chúng ta liên tục so sánh các hậu tố lớn để xác minh các giai đoạn ứng cử viên thì độ phức tạp trong trường hợp xấu nhất sẽ trở thành bậc hai. 

Quan sát quan trọng là trình tạo hoạt động giống như một đồ thị có hướng trong đó mỗi nút có chính xác một cạnh đi ra. Từ bất kỳ trạng thái nào, chỉ có duy nhất một trạng thái tiếp theo. Những đồ thị như vậy luôn bao gồm một đường dẫn vào một chu trình. 

Vì chỉ có`m`trạng thái có thể, giá trị lặp lại đầu tiên ngay lập tức xác định điểm bắt đầu của chu kỳ. 

Giả sử giá trị`x`xuất hiện đầu tiên ở vị trí`p`, và sau đó lại xuất hiện ở vị trí`q`. 

Bởi vì hàm chuyển đổi có tính xác định:```
next(x) is always the same
```toàn bộ hậu tố sau vị trí`p`phải khớp với hậu tố sau vị trí`q`. Điều đó có nghĩa là độ dài chu kỳ chỉ đơn giản là:```
q - p
```Vì vậy, thay vì lưu trữ toàn bộ chuỗi và thực hiện so sánh, chúng ta chỉ cần nhớ vị trí đầu tiên mà mỗi giá trị xuất hiện. 

Thuật toán trở thành: 

1. Tạo từng giá trị một. 
2. Lưu trữ chỉ mục đầu tiên nơi mỗi giá trị xuất hiện. 
3. Khi một giá trị lặp lại, hãy trả về chênh lệch giữa chỉ mục hiện tại và chỉ mục xuất hiện đầu tiên. 

Vì mỗi trạng thái được xử lý tối đa một lần trước khi lặp lại nên thuật toán chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(m) | Quá chậm | 
| Tối ưu | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`,`b`,`m`, Và`r0`. 
2. Tạo từ điển hoặc mảng`first_seen`lưu trữ chỉ mục đầu tiên nơi mỗi giá trị xuất hiện. 
3. Đánh dấu giá trị bắt đầu`r0`như đã thấy ở vị trí`0`. 
4. Tạo giá trị tiếp theo nhiều lần bằng cách sử dụng:$$r = (a \cdot r + b) \bmod m$$5. Tăng bộ đếm vị trí hiện tại sau khi tạo mỗi giá trị mới. 
6. Kiểm tra xem giá trị mới đã xuất hiện trước đó chưa. 
7. Nếu giá trị là mới, hãy lưu vị trí xuất hiện đầu tiên của nó và tiếp tục. 
8. Nếu giá trị đã xuất hiện ở vị trí`p`, dãy đã bước vào một chu kỳ. Độ dài chu kỳ là:$$\text{current position} - p$$9. In độ dài đó và dừng lại. 

### Tại sao nó hoạt động 

Trình tự được xác định hoàn toàn bởi giá trị hiện tại của nó. Nếu một số giá trị`x`xuất hiện ở hai vị trí khác nhau, sự phát triển trong tương lai từ cả hai vị trí phải giống hệt nhau. 

Giả định:```
r_p = r_q
```với`p < q`. 

Sau đó:```
r_{p+1} = r_{q+1}
r_{p+2} = r_{q+2}
...
```bởi vì cả hai vị trí đều áp dụng cùng một công thức chuyển đổi cho cùng một giá trị. 

Điều đó có nghĩa là trình tự lặp lại mỗi`q - p`bước bắt đầu từ vị trí`p`. Vì chúng tôi dừng ở giá trị lặp lại đầu tiên nên chênh lệch này là khoảng thời gian hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, m, r = map(int, input().split())

first_seen = [-1] * m
first_seen[r] = 0

pos = 0

while True:
    r = (a * r + b) % m
    pos += 1

    if first_seen[r] != -1:
        print(pos - first_seen[r])
        break

    first_seen[r] = pos
```Mảng`first_seen`lưu trữ vị trí đầu tiên nơi mỗi phần còn lại xuất hiện. Vì mọi giá trị đều nằm trong phạm vi`[0, m - 1]`, sử dụng mảng nhanh hơn và đơn giản hơn so với sử dụng từ điển. 

Biến`pos`đại diện cho chỉ mục của giá trị hiện tại trong chuỗi. Chúng tôi bắt đầu với`r0`ở vị trí`0`. 

Bên trong vòng lặp, trước tiên chúng ta tạo giá trị tiếp theo, sau đó tăng vị trí. Thứ tự quan trọng. Nếu chúng tôi tăng trước khi tạo trạng thái tiếp theo, các chỉ số được lưu trữ sẽ dịch chuyển không chính xác và tạo ra các lỗi sai lệch một trong độ dài chu kỳ. 

Thời điểm chúng ta gặp một giá trị đã thấy trước đó, chúng ta trừ vị trí xuất hiện đầu tiên của nó khỏi vị trí hiện tại. Sự khác biệt đó chính xác là độ dài chu kỳ. 

Vòng lặp được đảm bảo kết thúc vì chỉ có`m`các trạng thái riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 6 12 11
```Trình tự được tạo:```
11 -> 4 -> 2 -> 10 -> 2 -> ...
```| Vị trí | Giá trị hiện tại | Lần đầu tiên nhìn thấy trước đây? | Hành động | 
| --- | --- | --- | --- | 
| 0 | 11 | Không | lưu trữ 11 lúc 0 | 
| 1 | 4 | Không | lưu trữ 4 lúc 1 | 
| 2 | 2 | Không | lưu trữ 2 lúc 2 | 
| 3 | 10 | Không | lưu trữ 10 lúc 3 | 
| 4 | 2 | Có, lúc 2 | đáp án = 4 - 2 = 2 | 

Giá trị lặp lại là`2`. Lần xuất hiện đầu tiên của nó là ở vị trí`2`, và nó lại xuất hiện ở vị trí`4`, vậy chu kì là`2`. 

### Ví dụ 2 

đầu vào:```
1 3 5 2
```Trình tự được tạo:```
2 -> 0 -> 3 -> 1 -> 4 -> 2 -> ...
```| Vị trí | Giá trị hiện tại | Lần đầu tiên nhìn thấy trước đây? | Hành động | 
| --- | --- | --- | --- | 
| 0 | 2 | Không | lưu trữ 2 lúc 0 | 
| 1 | 0 | Không | lưu trữ 0 lúc 1 | 
| 2 | 3 | Không | lưu trữ 3 lúc 2 | 
| 3 | 1 | Không | lưu trữ 1 lúc 3 | 
| 4 | 4 | Không | cửa hàng 4 lúc 4 | 
| 5 | 2 | Có, lúc 0 | đáp án = 5 - 0 = 5 | 

Dấu vết này cho thấy một chu kỳ bắt đầu ngay lập tức. Mọi giá trị trong modulo`5`xuất hiện đúng một lần trước khi lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | mỗi trạng thái được xử lý nhiều nhất một lần | 
| Không gian | O(m) | lưu trữ lần xuất hiện đầu tiên cho mỗi phần còn lại có thể | 

Với`m ≤ 100000`, thuật toán thực hiện tối đa một lần lặp cho mỗi trạng thái có thể. Cả thời gian chạy và mức sử dụng bộ nhớ đều nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a, b, m, r = map(int, input().split())

    first_seen = [-1] * m
    first_seen[r] = 0

    pos = 0

    while True:
        r = (a * r + b) % m
        pos += 1

        if first_seen[r] != -1:
            print(pos - first_seen[r])
            return

        first_seen[r] = pos

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run("2 6 12 11\n") == "2", "sample 1"

# minimum-size input
assert run("0 0 1 0\n") == "1", "single state"

# cycle starts immediately
assert run("1 1 5 0\n") == "5", "full modulo cycle"

# fixed point after one step
assert run("0 0 10 7\n") == "1", "constant sequence"

# off-by-one detection
assert run("2 0 7 1\n") == "3", "cycle length should be 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 0`|`1`| không gian trạng thái nhỏ nhất có thể | 
|`1 1 5 0`|`5`| chu kỳ bắt đầu ngay lập tức | 
|`0 0 10 7`|`1`| chu kỳ điểm cố định sau kỳ trước | 
|`2 0 7 1`|`3`| bắt lỗi dịch chuyển chỉ số | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
0 0 10 7
```Trình tự được tạo ra là:```
7 -> 0 -> 0 -> 0 -> ...
```Dấu vết: 

| Vị trí | Giá trị | 
| --- | --- | 
| 0 | 7 | 
| 1 | 0 | 
| 2 | 0 | 

giá trị`0`xuất hiện đầu tiên ở vị trí`1`và lặp lại ở vị trí`2`. Thuật toán trả về`2 - 1 = 1`, điều đó đúng. Giá trị ban đầu`7`thuộc về thời kỳ tiền và không phải là một phần của chu kỳ. 

Bây giờ hãy xem xét:```
1 1 5 0
```Trình tự trở thành:```
0 -> 1 -> 2 -> 3 -> 4 -> 0
```Dấu vết: 

| Vị trí | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 4 | 
| 5 | 0 | 

Giá trị lặp lại`0`lần đầu tiên được nhìn thấy ở vị trí`0`, do đó thuật toán trả về`5`. Điều này xác nhận rằng các chu kỳ bắt đầu ngay lập tức được xử lý chính xác. 

Cuối cùng, hãy xem xét:```
5 3 100 15
```Giả sử dãy đạt:```
78 -> 78 -> 78 -> ...
```Lần lặp lại đầu tiên xảy ra sau một bước, do đó thuật toán tính toán:```
current_position - first_seen[78] = 1
```Vòng lặp tự vẫn là một chu trình hợp lệ và thuật toán xử lý nó một cách tự nhiên mà không có trường hợp đặc biệt nào.
