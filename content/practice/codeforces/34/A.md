---
title: "CF 34A - Trinh Sát 2"
description: "Chúng tôi được cho biết chiều cao của những người lính đứng thành vòng tròn. Hai người lính có thể tạo thành một đơn vị trinh sát nếu họ đứng cạnh nhau và chênh lệch chiều cao của họ càng nhỏ càng tốt giữa tất cả các cặp lân cận."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 34
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 34 (Div. 2)"
rating: 800
weight: 34
solve_time_s: 78
verified: true
draft: false
---
[CF 34A - Trinh sát 2](https://codeforces.com/problemset/problem/34/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho biết chiều cao của những người lính đứng thành vòng tròn. Hai người lính có thể tạo thành một đơn vị trinh sát nếu họ đứng cạnh nhau và chênh lệch chiều cao của họ càng nhỏ càng tốt giữa tất cả các cặp lân cận. 

Sự sắp xếp hình tròn thay đổi một chi tiết quan trọng. Người lính đầu tiên và cuối cùng cũng là hàng xóm của nhau. Quét tuyến tính chỉ kiểm tra các vị trí liền kề từ trái sang phải sẽ bỏ lỡ cặp này. 

Đầu vào là một mảng nhỏ có chiều cao. Chúng ta cần xuất ra các chỉ số dựa trên 1 của bất kỳ cặp lân cận nào có chênh lệch tuyệt đối tối thiểu. 

Những hạn chế là rất nhỏ. Có tối đa 100 binh sĩ tồn tại, nên ngay cả một$O(n^2)$giải pháp sẽ chạy thoải mái trong giới hạn. Tuy nhiên, cấu trúc của bài toán đương nhiên dẫn đến việc quét tuyến tính đơn giản hơn. Vì mọi cặp hợp lệ đều phải là hàng xóm trong vòng tròn nên chúng ta không bao giờ cần so sánh từng người lính với mọi người lính khác. 

Trường hợp cạnh chính là kết nối vòng tròn giữa người lính cuối cùng và người lính đầu tiên. 

Hãy xem xét đầu vào này:```
5
10 12 13 15 10
```Sự khác biệt nhỏ nhất là giữa lính 5 và 1:```
|10 - 10| = 0
```Triển khai bất cẩn chỉ kiểm tra các cặp`(1,2), (2,3), (3,4), (4,5)`sẽ bỏ lỡ câu trả lời tối ưu một cách sai lầm. 

Một trường hợp tinh tế khác xảy ra khi nhiều cặp có cùng mức chênh lệch tối thiểu.```
4
5 7 9 11
```Mỗi cặp lân cận đều có sự khác biệt`2`. Bất kỳ cặp hợp lệ nào cũng có thể được in. Việc triển khai không nên giả định tính duy nhất. 

Kích thước đầu vào tối thiểu cũng có vấn đề:```
2
100 1
```Chỉ có hai người lính, vì sắp xếp theo vòng tròn nên hai hướng đều là láng giềng. Câu trả lời vẫn phải là`1 2`hoặc`2 1`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. So sánh từng cặp binh sĩ, kiểm tra xem họ có phải là hàng xóm trong vòng tròn hay không và theo dõi chênh lệch chiều cao tối thiểu. Vì có$n^2$cặp, số lượng hoạt động trong trường hợp xấu nhất là khoảng$100^2 = 10{,}000$, điều này vẫn hoàn toàn có thể chấp nhận được. 

Vấn đề với vũ lực ở đây không phải là hiệu suất mà là công việc không cần thiết. Hầu hết các cặp đều không liên quan vì chỉ những người lính lân cận mới được phép. Cấu trúc của bài toán đã cho chúng ta biết chính xác sự so sánh nào là quan trọng. 

Mỗi người lính chỉ có hai người hàng xóm. Nếu chúng ta đi một vòng quanh vòng tròn và so sánh từng cặp liền kề, chúng ta sẽ kiểm tra mọi ứng cử viên hợp lệ chính xác một lần. Điều đó làm giảm nhiệm vụ xuống còn quét tuyến tính đơn giản. 

Quan sát quan trọng là câu trả lời phải đến từ một trong các cặp sau:```
(1,2), (2,3), ..., (n-1,n), (n,1)
```Không có cặp nào khác là hợp pháp. 

Vì vậy, giải pháp tối ưu sẽ kiểm tra tất cả các cặp lân cận, tính toán sự khác biệt tuyệt đối của chúng và ghi nhớ giá trị nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng binh lính và chiều cao của họ. 
2. Khởi tạo chênh lệch tốt nhất hiện tại với giá trị rất lớn. 
3. Lặp lại tất cả các vị trí từ`0`ĐẾN`n-1`. 
4. Đối với từng vị trí`i`, xác định hàng xóm của nó là`(i + 1) % n`. 

Phép toán modulo làm cho mảng trở thành hình tròn. Khi`i`là chỉ số cuối cùng, người hàng xóm trở thành người lính đầu tiên. 
5. Tính chênh lệch tuyệt đối giữa hai độ cao lân cận. 
6. Nếu chênh lệch này nhỏ hơn giá trị tốt nhất hiện tại, hãy cập nhật chỉ số trả lời và lưu trữ chênh lệch tối thiểu mới. 
7. Sau khi quét xong, hãy in cặp đã lưu bằng cách sử dụng chỉ mục dựa trên 1. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp lân cận hợp lệ chính xác một lần. Vì bài toán chỉ cho phép những người lính lân cận vào vòng tròn nên không có câu trả lời hợp lệ nào có thể tồn tại bên ngoài tập hợp này. Bằng cách duy trì sự khác biệt tuyệt đối nhỏ nhất cho đến nay, thuật toán đảm bảo rằng cặp được lưu trữ là tối ưu sau khi quá trình quét kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

best_diff = float('inf')
ans_l = 1
ans_r = 2

for i in range(n):
    j = (i + 1) % n
    diff = abs(a[i] - a[j])

    if diff < best_diff:
        best_diff = diff
        ans_l = i + 1
        ans_r = j + 1

print(ans_l, ans_r)
```Chương trình bắt đầu bằng cách đọc chiều cao của người lính vào một mảng.`best_diff`lưu trữ sự khác biệt tối thiểu được tìm thấy cho đến nay. Nó bắt đầu ở vô cùng nên cặp đầu tiên luôn thay thế nó. 

Vòng lặp quét từng người lính một lần. Đối với mỗi chỉ số`i`, hàng xóm tiếp theo được tính bằng:```
j = (i + 1) % n
```Đây là chi tiết triển khai quan trọng nhất. Nếu không có modulo, người lính cuối cùng sẽ không có người hàng xóm ở bên phải và cặp hình tròn`(n,1)`sẽ bị bỏ qua. 

Việc so sánh sử dụng sự khác biệt tuyệt đối vì thứ tự độ cao không quan trọng:```
diff = abs(a[i] - a[j])
```Các chỉ mục được chuyển đổi trở lại dạng dựa trên 1 khi được lưu trữ vì các vấn đề của Codeforce thường mong đợi việc lập chỉ mục bắt đầu từ 1. 

Thuật toán sử dụng bộ nhớ bổ sung không đổi và chỉ duyệt một mảng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
10 12 13 15 10
```| tôi | Đã kiểm tra cặp | Sự khác biệt | Sự khác biệt tốt nhất | Câu trả lời được lưu trữ | 
| --- | --- | --- | --- | --- | 
| 0 | (1,2) | 2 | 2 | (1,2) | 
| 1 | (2,3) | 1 | 1 | (2,3) | 
| 2 | (3,4) | 2 | 1 | (2,3) | 
| 3 | (4,5) | 5 | 1 | (2,3) | 
| 4 | (5,1) | 0 | 0 | (5,1) | 

Bước cuối cùng chứng minh tại sao kết nối vòng tròn lại quan trọng. Cặp tối ưu chỉ xuất hiện khi người lính cuối cùng được so sánh với người đầu tiên. 

### Ví dụ 2 

đầu vào:```
4
5 7 9 11
```| tôi | Đã kiểm tra cặp | Sự khác biệt | Sự khác biệt tốt nhất | Câu trả lời được lưu trữ | 
| --- | --- | --- | --- | --- | 
| 0 | (1,2) | 2 | 2 | (1,2) | 
| 1 | (2,3) | 2 | 2 | (1,2) | 
| 2 | (3,4) | 2 | 2 | (1,2) | 
| 3 | (4,1) | 6 | 2 | (1,2) | 

Ví dụ này cho thấy có thể tồn tại nhiều câu trả lời tối ưu. Thuật toán chỉ đơn giản giữ lại cái đầu tiên nó gặp, cái này hoàn toàn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cặp lân cận được kiểm tra một lần | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với tối đa 100 binh sĩ, giải pháp dễ dàng nằm trong giới hạn. Ngay cả những cách tiếp cận chậm hơn nhiều cũng có thể vượt qua, nhưng việc quét tuyến tính phù hợp trực tiếp với cấu trúc của vấn đề và tránh những so sánh không cần thiết. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    best_diff = float('inf')
    ans_l = 1
    ans_r = 2

    for i in range(n):
        j = (i + 1) % n
        diff = abs(a[i] - a[j])

        if diff < best_diff:
            best_diff = diff
            ans_l = i + 1
            ans_r = j + 1

    print(ans_l, ans_r)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run("5\n10 12 13 15 10\n") == "5 1", "sample 1"

# minimum size
assert run("2\n100 1\n") == "1 2", "minimum n"

# all equal values
assert run("4\n7 7 7 7\n") == "1 2", "all equal"

# circular edge case
assert run("5\n10 20 30 40 11\n") == "5 1", "last-first pair"

# multiple optimal answers
assert run("4\n1 3 5 7\n") == "1 2", "many valid answers"

# decreasing sequence
assert run("6\n20 18 16 14 12 10\n") == "1 2", "consistent differences"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 100 1`|`1 2`| Kích thước đầu vào tối thiểu | 
|`4 / 7 7 7 7`|`1 2`| Không có sự khác biệt ở mọi nơi | 
|`5 / 10 20 30 40 11`|`5 1`| Xử lý hàng xóm tròn | 
|`4 / 1 3 5 7`|`1 2`| Nhiều câu trả lời tối ưu | 

## Vỏ cạnh 

Xét trường hợp ranh giới hình tròn:```
5
10 12 13 15 10
```Thuật toán kiểm tra các cặp theo thứ tự sau:```
(1,2), (2,3), (3,4), (4,5), (5,1)
```Sự so sánh cuối cùng mang lại sự khác biệt`0`, trở thành mức tối thiểu mới. Bởi vì phép toán modulo kết nối người lính cuối cùng trở lại người lính đầu tiên nên câu trả lời đúng`5 1`được tìm thấy. 

Bây giờ hãy xem xét chiều cao bằng nhau:```
4
7 7 7 7
```Mỗi cặp lân cận đều có sự khác biệt`0`. Sự so sánh đầu tiên đã đặt mức tối thiểu là`0`, và không có cặp nào sau này cải thiện nó. Thuật toán trả về`1 2`, điều này hợp lệ vì bất kỳ cặp tối ưu nào cũng có thể được in. 

Cuối cùng, kiểm tra đầu vào nhỏ nhất có thể:```
2
100 1
```Vòng lặp kiểm tra:```
(1,2)
(2,1)
```Cặp đầu tiên đã đạt được mức chênh lệch tối thiểu`99`. Thuật toán lưu trữ và in`1 2`. Mặc dù khái niệm vòng tròn trùng lặp cùng một cặp theo thứ tự ngược lại, câu trả lời vẫn đúng.
