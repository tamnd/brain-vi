---
title: "CF 102947E - Phân bổ lương thực I"
description: "Chúng ta được cung cấp một ma trận vuông có kích thước $n nhân n$, trong đó mỗi mục mô tả giá trị mà một người sống sót cụ thể đóng góp nếu được chỉ định cho một loại thực phẩm cụ thể."
date: "2026-07-04T07:27:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102947
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 02-05-21 Div. 1 (Advanced)"
rating: 0
weight: 102947
solve_time_s: 42
verified: true
draft: false
---

[CF 102947E - Phân bổ thực phẩm I](https://codeforces.com/problemset/problem/102947/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một ma trận vuông có kích thước$n \times n$, trong đó mỗi mục mô tả giá trị mà một người sống sót cụ thể đóng góp nếu được chỉ định cho một loại thực phẩm cụ thể. Mỗi người sống sót phải được chỉ định cho chính xác một loại thực phẩm và mỗi loại thực phẩm phải nhận chính xác một người sống sót, vì vậy nhiệm vụ này phải khớp hoàn hảo giữa các hàng và cột. 

Mục tiêu là chọn phép gán một-một này sao cho tổng các giá trị ma trận đã chọn càng lớn càng tốt. 

Mặc dù tuyên bố nói về những người sống sót và các loại thực phẩm, nhưng cấu trúc hoàn toàn là một bài toán gán: chọn chính xác một phần tử từ mỗi hàng và mỗi cột, tối đa hóa tổng số. 

Ràng buộc$n \le 10$là tín hiệu chính. Một bài toán gán chung với$n$lên tới 500 sẽ yêu cầu thuật toán Hungary hoặc kỹ thuật luồng tối đa chi phí tối thiểu, nhưng ở đây không gian tìm kiếm đủ nhỏ để liệt kê tất cả các hoán vị là khả thi. 

Các trường hợp cạnh có ý nghĩa duy nhất xuất phát từ việc hiểu rằng mỗi hàng phải được sử dụng chính xác một lần và mỗi cột phải được sử dụng chính xác một lần. Một cách tiếp cận tham lam ngây thơ sẽ thất bại ngay lập tức vì những lựa chọn tốt nhất tại địa phương có thể cản trở các cặp đôi tối ưu toàn cầu. 

Ví dụ, hãy xem xét:```
n = 3
matrix:
10 1 1
1 10 1
1 1 10
```Tham lam trên mỗi hàng hoặc cột hoạt động ở đây, nhưng sửa đổi một chút:```
n = 3
matrix:
10 9 1
9 10 1
1 1 100
```Việc chọn mục nhập tốt nhất trong mỗi hàng một cách độc lập có thể khiến hàng cuối cùng rơi vào cột có giá trị thấp, làm mất cấu trúc ghép nối tối ưu. 

Vì vậy, vấn đề cơ bản là về hoán vị hơn là các lựa chọn độc lập. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là thử mọi cách phân công những người sống sót có thể cho các loại thực phẩm. Vì mỗi phép gán tương ứng với một hoán vị của các cột cho$n$hàng, chúng ta có thể liệt kê tất cả$n!$hoán vị và tính tổng của mỗi hoán vị. 

Điều này đúng vì mọi phép gán hợp lệ đều tương ứng với chính xác một hoán vị và mỗi hoán vị thể hiện một phép gán hợp lệ. 

Tuy nhiên, chi phí tăng theo cấp số nhân. Ngay cả đối với$n = 10$, chúng tôi nhận được$10! = 3,628,800$hoán vị. Mỗi chi phí đánh giá$O(n)$, đưa ra một cách đại khái$3.6 \times 10^7$các hoạt động nằm ở ranh giới nhưng vẫn chỉ được chấp nhận trong Python nếu được triển khai cẩn thận. Cấu trúc cho thấy chúng ta có thể làm rõ ràng hơn thay vì cố gắng tối ưu hóa mạnh mẽ lực lượng vũ phu. 

Quan sát quan trọng là không tồn tại cấu trúc bổ sung nào như tính đơn điệu hoặc tính khả thi tham lam, vì vậy giải pháp đúng đơn giản nhất là phép liệt kê hoán vị thuần túy bằng cách quay lui hoặc`itertools.permutations`. 

Điều này làm giảm vấn đề thành một tìm kiếm toàn diện rõ ràng theo thứ tự cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n! \cdot n)$|$O(n)$| Được chấp nhận cho$n \le 10$| 
| DP / Hungary (ở đây quá mức cần thiết) |$O(n^3)$|$O(n^2)$| Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc ma trận kích thước$n \times n$, nơi nhập cảnh$(i, j)$là giá trị của việc chỉ định người sống sót$i$đến loại thực phẩm$j$. Điều này xác định đầy đủ chi phí của bất kỳ nhiệm vụ nào. 
2. Tạo tất cả các hoán vị của số$0$ĐẾN$n-1$. Mỗi hoán vị thể hiện một lựa chọn trong đó hàng$i$được gán cho cột`perm[i]`. Điều này đảm bảo mỗi cột được sử dụng chính xác một lần. 
3. Với mỗi hoán vị, hãy tính tổng bằng cách lặp lại tất cả các hàng và cộng`matrix[i][perm[i]]`. Điều này đánh giá giá trị của nhiệm vụ đó. 
4. Theo dõi số tiền tối đa nhìn thấy trên tất cả các hoán vị. Điều này thể hiện sự phân công tốt nhất có thể. 
5. Xuất giá trị lớn nhất sau khi kiểm tra tất cả các hoán vị. 

Điểm tinh tế duy nhất là đảm bảo chúng tôi diễn giải chính xác các hoán vị dưới dạng phép gán cột. Bất kỳ sự không khớp nào giữa việc lập chỉ mục hàng và cột sẽ phá vỡ tính chính xác. 

### Tại sao nó hoạt động 

Mọi phép gán hợp lệ của những người sống sót cho các loại thực phẩm đều tương ứng với một phép đối đôi giữa các hàng và cột, và mỗi phép đối đôi chính xác là một hoán vị của các chỉ số cột. Vì chúng ta đánh giá tất cả các hoán vị nên không có phép gán hợp lệ nào bị bỏ sót. Vì chúng ta tính tổng chính xác cho mỗi hoán vị nên giá trị lớn nhất tìm được là nghiệm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import permutations

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]

best = 0

for perm in permutations(range(n)):
    total = 0
    for i in range(n):
        total += a[i][perm[i]]
    if total > best:
        best = total

print(best)
```Giải pháp phản ánh trực tiếp việc giải thích bài tập. Ma trận được lưu trữ nguyên trạng và mỗi hoán vị được coi là một lựa chọn cột cho mỗi hàng. Mức tối đa đang chạy duy trì nhiệm vụ tốt nhất được thấy. 

Một lỗi phổ biến ở đây là thử lựa chọn theo từng hàng một cách tham lam, nhưng việc này không thành công vì các lựa chọn trước đó hạn chế tính khả dụng của cột trong tương lai. Cách tiếp cận hoán vị tránh được điều này bằng cách thực thi tính nhất quán toàn cầu ngay từ đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3 4
0 2
```Chúng tôi liệt kê các hoán vị của cột$[0,1]$. 

| Hoán vị | Lựa chọn hàng 0 | Lựa chọn hàng 1 | Tổng cộng | 
| --- | --- | --- | --- | 
| (0, 1) | 3 | 2 | 5 | 
| (1, 0) | 4 | 0 | 4 | 

Tốt nhất là 5, đạt được bằng cách gán hàng 0 cho cột 0 và hàng 1 cho cột 1. 

Điều này cho thấy các hoán vị khác nhau tương ứng với các kết quả khớp hợp lệ khác nhau như thế nào. 

### Ví dụ 2 

đầu vào:```
3
3 2 3
2 1 1
1 2 3
```Chúng tôi kiểm tra tất cả các hoán vị của$(0,1,2)$. 

| Hoán vị | Bài tập hàng | Tổng cộng | 
| --- | --- | --- | 
| (0,1,2) | 3 + 1 + 3 | 7 | 
| (0,2,1) | 3 + 1 + 2 | 6 | 
| (1,0,2) | 2 + 2 + 3 | 7 | 
| (1,2,0) | 2 + 1 + 1 | 4 | 
| (2,0,1) | 3 + 2 + 2 | 7 | 
| (2,1,0) | 3 + 1 + 1 | 5 | 

Giá trị tối đa là 7. 

Ví dụ này cho thấy có thể tồn tại nhiều nhiệm vụ tối ưu và vũ lực sẽ nắm bắt tất cả chúng một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n! \cdot n)$| Chúng tôi liệt kê tất cả các hoán vị và tính tổng$n$hàng cho mỗi | 
| Không gian |$O(n)$| Chúng tôi lưu trữ ma trận và hoán vị hiện tại | 

Với$n \le 10$, sự tăng trưởng giai thừa vẫn đủ nhỏ để thực hiện trong thời gian giới hạn, đặc biệt vì mỗi thao tác là phép cộng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    best = 0
    for p in permutations(range(n)):
        total = 0
        for i in range(n):
            total += a[i][p[i]]
        best = max(best, total)
    return str(best)

# provided sample
assert run("2\n3 4\n0 2\n") == "5", "sample 1"

# custom: all equal values
assert run("2\n1 1\n1 1\n") == "2", "uniform matrix"

# custom: diagonal dominant
assert run("3\n10 1 1\n1 10 1\n1 1 10\n") == "30", "diagonal best"

# custom: forcing permutation choice
assert run("3\n1 2 3\n3 2 1\n2 3 1\n") == "8", "mixed case"

# custom: minimum size
assert run("1\n7\n") == "7", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ma trận đồng nhất 2x2 | 2 | sự lựa chọn đối xứng và bình đẳng | 
| ma trận chéo | 30 | khớp đúng đường chéo tốt nhất | 
| hỗn hợp 3x3 | 8 | lựa chọn hoán vị không tầm thường | 
| phần tử đơn | 7 | xử lý trường hợp cơ bản | 

## Vỏ cạnh 

Đối với một người sống sót duy nhất, thuật toán ngay lập tức trả về giá trị khả dụng duy nhất vì hoán vị duy nhất trống và tổng giảm xuống còn mục nhập ma trận đơn. 

Đối với các ma trận đều trong đó tất cả các phần tử đều bằng nhau thì mọi hoán vị đều mang lại tổng bằng nhau. Thuật toán vẫn khám phá tất cả các hoán vị nhưng luôn theo dõi mức tối đa giống nhau, xác nhận rằng không tồn tại sai lệch trong thứ tự lựa chọn. 

Đối với các ma trận trong đó các phép gán tối ưu yêu cầu hoán đổi không tham lam, chẳng hạn như khi các giá trị cao bị phân tán, phép liệt kê hoán vị đảm bảo tính chính xác. Mỗi sự kết hợp có thể được đánh giá một cách rõ ràng, do đó không có phép gán nào bị bỏ sót ngay cả khi các lựa chọn trung gian có vẻ dưới mức tối ưu.
