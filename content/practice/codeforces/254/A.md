---
title: "CF 254A - Thẻ có số"
description: "Chúng ta có 2n thẻ được xếp theo một trình tự. Mỗi lá bài đều có ghi một số nguyên và mỗi lá bài cũng có chỉ số từ 1 đến 2n. Nhiệm vụ là chia tất cả các thẻ thành đúng n cặp sao cho cả hai thẻ trong mỗi cặp đều chứa cùng một số."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 254
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 155 (Div. 2)"
rating: 1200
weight: 254
solve_time_s: 150
verified: false
draft: false
---

[CF 254A - Thẻ có số](https://codeforces.com/problemset/problem/254/A) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`2n`thẻ được sắp xếp theo một trình tự. Mỗi thẻ có một số nguyên được ghi trên đó và mỗi thẻ cũng có chỉ số từ`1`ĐẾN`2n`. Nhiệm vụ là chia tất cả các thẻ thành chính xác`n`các cặp sao cho cả hai thẻ trong mỗi cặp đều chứa cùng một số. 

Đầu ra không yêu cầu các giá trị. Nó yêu cầu chỉ số của các thẻ tạo thành các cặp hợp lệ. Nếu một giá trị chẵn xuất hiện với số lần lẻ thì việc tạo thành các cặp hoàn chỉnh là không thể và chúng ta phải in`-1`. 

Các ràng buộc ngay lập tức cho chúng ta biết lời giải phải gần với thời gian tuyến tính. Có tới`6 * 10^5`tổng số thẻ, vì vậy mọi thứ bậc hai sẽ bùng nổ. Một cách tiếp cận tìm kiếm theo cặp đơn giản là quét liên tục các thẻ còn lại sẽ thực hiện theo thứ tự`(2n)^2`so sánh trong trường hợp xấu nhất, quá lớn so với giới hạn một giây. Mặt khác, việc băm hoặc nhóm các giá trị theo tần số dễ dàng phù hợp vì mỗi thẻ chỉ cần được xử lý một số lần không đổi. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. 

Hãy xem xét đầu vào này:```
2
1 2 1 2
```Một câu trả lời đúng là:```
1 3
2 4
```Việc triển khai có lỗi có thể vô tình sử dụng lại một chỉ mục sau khi ghép nối nó một lần, đặc biệt nếu nó chỉ theo dõi các giá trị chứ không theo dõi những lần xuất hiện cụ thể nào đã được sử dụng. 

Một trường hợp quan trọng khác là khi một giá trị xuất hiện với số lần lẻ:```
2
5 5 5 7
```giá trị`5`xuất hiện ba lần, vì vậy một bản sao sẽ luôn không trùng khớp. Đầu ra đúng là:```
-1
```Một giải pháp tham lam bất cẩn có thể ghép hai`5`s và quên xác minh rằng tất cả các lần xuất hiện đã được sử dụng. 

Ngoài ra còn có trường hợp tất cả các thẻ đều chứa cùng một số:```
3
9 9 9 9 9 9
```Mọi chỉ mục vẫn phải xuất hiện chính xác một lần trong đầu ra. Việc ghép nối hợp lệ là tùy ý, nhưng việc triển khai phải tránh bỏ qua hoặc trùng lặp các vị trí. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi thẻ chưa ghép, hãy quét các thẻ còn lại cho đến khi tìm được thẻ khác có cùng giá trị. Sau khi tìm thấy, đánh dấu cả hai là đã sử dụng và tiếp tục. 

Điều này hiệu quả vì mọi giải pháp hợp lệ chỉ yêu cầu nhóm các giá trị bằng nhau lại với nhau. Vấn đề là hiệu quả. Trong trường hợp xấu nhất, mọi tìm kiếm có thể quét gần như toàn bộ mảng còn lại. Với tối đa`6 * 10^5`thẻ, số lượng so sánh có thể đạt khoảng`3.6 * 10^11`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là vị trí thực tế chỉ quan trọng sau khi nhóm các thẻ theo giá trị. Nếu chúng ta biết tất cả các chỉ số có xuất hiện một số cụ thể thì việc tạo thành các cặp sẽ trở nên đơn giản. Chúng tôi chỉ đơn giản lấy hai chỉ số đó cùng một lúc. 

Điều này biến vấn đề từ việc tìm kiếm lặp đi lặp lại thành vấn đề nhóm. Chúng tôi duyệt mảng một lần, lưu trữ mọi chỉ mục bên trong một nhóm tương ứng với giá trị của nó, sau đó xác minh rằng mọi nhóm đều có kích thước chẵn. Nếu bất kỳ kích thước nhóm nào là số lẻ thì không thể ghép nối được. Nếu không, các chỉ số liên tiếp bên trong nhóm sẽ tự động tạo thành các cặp hợp lệ. 

Các giá trị nhiều nhất là`5000`, do đó, ngay cả một mảng vectơ cũng có thể hoạt động, nhưng bản đồ băm cũng rõ ràng và tổng quát không kém. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và mảng của`2n`giá trị thẻ. 
2. Tạo một từ điển trong đó mỗi khóa là một giá trị thẻ và danh sách liên quan sẽ lưu trữ tất cả các chỉ mục nơi giá trị đó xuất hiện. 
3. Duyệt mảng từ trái sang phải. 
4. Đối với mọi vị trí`i`, nối thêm chỉ mục`i + 1`vào danh sách tương ứng với giá trị`a[i]`. 

Chúng tôi lưu trữ`i + 1`bởi vì vấn đề sử dụng lập chỉ mục dựa trên 1. 
5. Sau khi nhóm tất cả các chỉ số, lặp qua từng nhóm giá trị. 
6. Nếu kích thước của bất kỳ nhóm nào là số lẻ, hãy in`-1`và chấm dứt ngay lập tức. 

Số lẻ có nghĩa là một lá bài không bao giờ có thể trùng khớp với một lá bài giống hệt khác. 
7. Ngược lại, xử lý hai chỉ số của nhóm cùng một lúc. 
8. In`(indices[0], indices[1])`, sau đó`(indices[2], indices[3])`, vân vân. 

Vì tất cả các chỉ số trong nhóm đều có cùng giá trị nên mọi cặp được tạo ra đều hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến đơn giản: mọi danh sách được lưu trữ đều chứa chính xác chỉ số của các thẻ có cùng giá trị. 

Nếu một giá trị xuất hiện với số lần lẻ thì ít nhất một lá bài vẫn chưa khớp trong mọi cách sắp xếp có thể, do đó không có giải pháp hợp lệ nào tồn tại. 

Nếu một giá trị xuất hiện với số lần chẵn thì việc ghép các chỉ mục liên tiếp trong nhóm đó luôn hoạt động vì mọi cặp đều chứa các giá trị giống nhau và không có chỉ mục nào được sử dụng lại. Vì các nhóm phân chia tất cả các chỉ số của mảng nên mỗi thẻ xuất hiện theo đúng một cặp được in. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = defaultdict(list)

    for i, x in enumerate(a, start=1):
        pos[x].append(i)

    ans = []

    for indices in pos.values():
        if len(indices) % 2 == 1:
            print(-1)
            return

        for i in range(0, len(indices), 2):
            ans.append((indices[i], indices[i + 1]))

    print("\n".join(f"{x} {y}" for x, y in ans))

solve()
```Từ điển`pos`là cấu trúc cốt lõi của giải pháp. Mỗi giá trị ánh xạ tới tất cả các chỉ mục nơi nó xuất hiện. Vì chúng tôi nối thêm các chỉ mục trong khi quét từ trái sang phải nên mỗi lần xuất hiện đều được ghi lại chính xác một lần. 

các`enumerate(..., start=1)`cuộc gọi là quan trọng vì vấn đề mong đợi các chỉ số bắt đầu từ`1`. Quên điều này là một trong những sai lầm phổ biến nhất. 

Việc kiểm tra tính chẵn lẻ diễn ra trước khi tạo cặp. Khi một nhóm có kích thước lẻ, không có lý do gì để tiếp tục xử lý vì toàn bộ phiên bản là không thể. 

Vòng lặp ghép nối tiến lên bằng cách`2`mỗi lần lặp. Điều này đảm bảo rằng mọi chỉ mục được sử dụng chính xác một lần và ngăn chặn sự chồng chéo ngẫu nhiên giữa các cặp. 

Giải pháp không dựa vào việc sắp xếp. Thứ tự ban đầu của các chỉ số đã cung cấp đủ cấu trúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
20 30 10 30 20 10
```Sau khi phân nhóm: 

| Giá trị | Chỉ số | 
| --- | --- | 
| 20 | [1, 5] | 
| 30 | [2, 4] | 
| 10 | [3, 6] | 

Cấu trúc cặp: 

| Nhóm hiện tại | Cặp sản xuất | 
| --- | --- | 
| [1, 5] | (1, 5) | 
| [2, 4] | (2, 4) | 
| [3, 6] | (3, 6) | 

Đầu ra có thể:```
1 5
2 4
3 6
```Dấu vết này thể hiện tính bất biến trung tâm của thuật toán. Mỗi nhóm chứa các chỉ mục của chính xác một giá trị, do đó mọi cặp liên tiếp đều tự động hợp lệ. 

### Ví dụ 2 

đầu vào:```
2
5 5 5 7
```Sau khi phân nhóm: 

| Giá trị | Chỉ số | 
| --- | --- | 
| 5 | [1, 2, 3] | 
| 7 | [4] | 

Kiểm tra tính chẵn lẻ: 

| Giá trị | Đếm | Thậm chí? | 
| --- | --- | --- | 
| 5 | 3 | Không | 
| 7 | 1 | Không | 

Đầu ra:```
-1
```Ví dụ này cho thấy tại sao điều kiện chẵn lẻ vừa cần vừa đủ. Khi một giá trị có tần số lẻ thì một lần xuất hiện phải không trùng khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục thẻ được chèn một lần và xử lý một lần nữa trong quá trình ghép nối | 
| Không gian | O(n) | Từ điển lưu trữ tất cả`2n`chỉ số | 

Các ràng buộc cho phép lên đến`6 * 10^5`tổng số thẻ, do đó độ phức tạp tuyến tính dễ dàng đủ nhanh. Việc sử dụng bộ nhớ cũng an toàn vì chúng tôi chỉ lưu trữ các chỉ mục. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    pos = defaultdict(list)

    for i, x in enumerate(a, start=1):
        pos[x].append(i)

    ans = []

    for indices in pos.values():
        if len(indices) % 2 == 1:
            print(-1)
            return

        for i in range(0, len(indices), 2):
            ans.append((indices[i], indices[i + 1]))

    print("\n".join(f"{x} {y}" for x, y in ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
out = run("3\n20 30 10 30 20 10\n")
pairs = set(tuple(map(int, line.split())) for line in out.splitlines())

assert len(pairs) == 3

# minimum size valid
assert run("1\n7 7\n") == "1 2"

# impossible because of odd frequency
assert run("2\n5 5 5 7\n") == "-1"

# all equal values
out = run("3\n9 9 9 9 9 9\n")
pairs = [tuple(map(int, line.split())) for line in out.splitlines()]
used = set()

for x, y in pairs:
    used.add(x)
    used.add(y)

assert len(pairs) == 3
assert used == {1, 2, 3, 4, 5, 6}

# alternating values
out = run("2\n1 2 1 2\n")
pairs = [tuple(map(int, line.split())) for line in out.splitlines()]
used = set()

for x, y in pairs:
    used.add(x)
    used.add(y)

assert len(pairs) == 2
assert used == {1, 2, 3, 4}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7 7`|`1 2`| Đầu vào hợp lệ nhỏ nhất | 
|`2 / 5 5 5 7`|`-1`| Phát hiện tần số lẻ | 
|`3 / 9 9 9 9 9 9`| Bất kỳ ghép nối đầy đủ nào | Các giá trị giống hệt nhau lặp đi lặp lại | 
|`2 / 1 2 1 2`| Bất kỳ ghép nối hợp lệ nào | Các chỉ số khớp không liền kề | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
2
1 2 1 2
```Các nhóm trở thành:```
1 -> [1, 3]
2 -> [2, 4]
```Thuật toán ghép các chỉ số liên tiếp bên trong mỗi nhóm, tạo ra:```
1 3
2 4
```Mặc dù các giá trị bằng nhau được phân tách trong mảng ban đầu, việc nhóm theo giá trị sẽ loại bỏ hoàn toàn sự phức tạp về vị trí. 

Bây giờ hãy xem xét:```
2
5 5 5 7
```Các nhóm được lưu trữ là:```
5 -> [1, 2, 3]
7 -> [4]
```Trong quá trình xác thực, thuật toán phát hiện cả hai nhóm đều có kích thước lẻ. Ngay lúc đó nó in ngay`-1`. Không thử ghép nối từng phần nên thuật toán không bao giờ tạo ra câu trả lời không đầy đủ không hợp lệ. 

Cuối cùng, hãy xem xét trường hợp hoàn toàn bằng nhau:```
3
9 9 9 9 9 9
```Nhóm duy nhất trở thành:```
9 -> [1, 2, 3, 4, 5, 6]
```Vòng lặp ghép nối xử lý các chỉ số theo hai bước:```
(1, 2)
(3, 4)
(5, 6)
```Mỗi chỉ mục được sử dụng chính xác một lần, điều này xác nhận rằng thuật toán xử lý chính xác các nhóm trùng lặp lớn.
