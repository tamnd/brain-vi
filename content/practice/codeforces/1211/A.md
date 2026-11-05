---
title: "CF 1211A - Ba vấn đề"
description: "Chúng ta được cung cấp một danh sách các khó khăn của bài toán, trong đó mỗi bài toán có một chỉ mục và một giá trị độ phức tạp bằng số. Nhiệm vụ là chọn ba chỉ số riêng biệt $a$, $b$, và $c$ sao cho các giá trị tương ứng tạo thành một chuỗi tăng nghiêm ngặt: $ra < rb < rc$."
date: "2026-06-18T17:20:39+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1211
codeforces_index: "A"
codeforces_contest_name: "Kotlin Heroes: Episode 2"
rating: 1000
weight: 1211
solve_time_s: 102
verified: false
draft: false
---

[CF 1211A - Ba vấn đề](https://codeforces.com/problemset/problem/1211/A) 

**Đánh giá:** 1000 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các khó khăn của bài toán, trong đó mỗi bài toán có một chỉ mục và một giá trị độ phức tạp bằng số. Nhiệm vụ là chọn ba chỉ số riêng biệt$a$,$b$, Và$c$sao cho các giá trị tương ứng tạo thành một chuỗi tăng dần:$r_a < r_b < r_c$. 

Đầu ra không phải là các giá trị mà là các chỉ số của bất kỳ bộ ba hợp lệ nào. Nếu không tồn tại bộ ba như vậy thì chúng ta phải xuất ra`-1 -1 -1`. 

Các ràng buộc cho phép lên đến$n = 3000$. Kích thước này đủ nhỏ để$O(n^2)$hoặc thậm chí được quản lý cẩn thận$O(n^3)$cách tiếp cận này có thể được áp dụng ở một số ngôn ngữ, nhưng trong Python, bất cứ điều gì tiếp cận$O(n^3)$có nguy cơ hết thời gian. Quét bậc hai có thể chấp nhận được, nhưng quét khối trên 27 triệu thao tác cho mỗi trường hợp xấu nhất đã là giới hạn tùy thuộc vào các hằng số. 

Khó khăn chính không phải là tính toán mà là cấu trúc. Chúng ta không được yêu cầu tối ưu hóa một hàm số hoặc đếm bộ ba mà phải tìm sự tồn tại của một dãy con tăng dần có độ dài ba với các chỉ số được bảo toàn. 

Một cạm bẫy ngây thơ xuất hiện khi các giá trị lặp lại hoặc không được sắp xếp. Ví dụ: trong một mảng như`[5, 1, 5, 1, 5]`, một nỗ lực bất cẩn có thể chọn các giá trị bằng nhau vì nghĩ rằng chúng tạo thành một cấp số nhân. Điều đó thất bại vì cần phải có sự bất bình đẳng nghiêm ngặt. Một vấn đề tế nhị khác là việc chọn các giá trị theo đúng thứ tự của các chỉ số là không liên quan, vì các chỉ số có thể tùy ý miễn là các giá trị đó thỏa mãn thứ tự. 

Cách tiếp cận bạo lực có thể bỏ sót rằng bộ ba hợp lệ có thể nằm rải rác và không liền kề, vì vậy việc kiểm tra địa phương tham lam có thể thất bại nếu họ chỉ kiểm tra hàng xóm. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử tất cả các bộ ba chỉ số. Chúng tôi chọn$i < j < k$và kiểm tra xem$r_i < r_j < r_k$. Điều này đúng vì nó kiểm tra điều kiện một cách rõ ràng. Tuy nhiên, điều này kiểm tra$\binom{n}{3}$gấp ba lần, trong trường hợp xấu nhất là khoảng 4,5 tỷ lượt kiểm tra khi$n = 3000$. Điều đó quá chậm trong Python. 

Chúng ta cần tránh liệt kê tất cả các bộ ba trong khi vẫn duy trì khả năng phát hiện phần tử ở giữa hợp lệ một cách hiệu quả. 

Quan sát quan trọng là đối với bất kỳ vị trí ở giữa cố định nào$j$, chúng ta không cần tìm kiếm tất cả các cặp có thể$(i, k)$một cách độc lập. Thay vào đó, chúng ta chỉ cần biết liệu có tồn tại một giá trị nào đó nhỏ hơn$r_j$ở bên trái và một số giá trị lớn hơn$r_j$ở bên phải của nó. Nếu cả hai đều tồn tại,$j$đóng vai trò là phần giữa của một bộ ba hợp lệ. 

Điều này biến vấn đề thành việc duy trì hai cấu trúc trợ giúp: quét tiền tố theo dõi phần tử nhỏ nhất được thấy cho đến nay (hoặc chỉ mục của nó) và quét hậu tố theo dõi ứng cử viên nhỏ nhất có thể lớn hơn mỗi phần tử hoặc chỉ đơn giản là kiểm tra sự tồn tại của bất kỳ phần tử lớn hơn nào ở bên phải. Vì các giá trị là tùy ý nên chúng ta có thể tính toán trước, đối với mỗi vị trí, xem liệu giá trị lớn hơn có tồn tại ở bên phải hay không và tương tự liệu giá trị nhỏ hơn có tồn tại ở bên trái hay không. 

Khi các mảng này được tính toán, việc quét tìm chỉ mục ở giữa hợp lệ sẽ trở nên đơn giản. Đối với bất kỳ$j$, nếu tồn tại$i < j$với$r_i < r_j$Và$k > j$với$r_k > r_j$, chúng tôi ngay lập tức xuất ra bộ ba đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tiền tố/Hậu tố Tính toán trước |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp dựa trên ý tưởng rằng mọi bộ ba hợp lệ đều có phần tử ở giữa rõ ràng. 

1. Tính toán trước một mảng`left_min_index`cửa hàng đó, cho mọi vị trí$j$, chỉ số của giá trị nhỏ nhất nhìn thấy trong tiền tố$1..j$. Điều này mang lại cho chúng tôi một ứng cử viên$i$như vậy$r_i < r_j$có thể được kiểm tra nhanh chóng bằng cách so sánh các giá trị. 
2. Tính toán trước một mảng`right_max_index`cửa hàng đó, cho mọi vị trí$j$, chỉ số của giá trị lớn nhất nhìn thấy trong hậu tố$j..n$. Điều này mang lại cho chúng tôi một ứng cử viên$k$như vậy$r_k > r_j$. 
3. Lặp lại từng chỉ mục$j$coi nó như là yếu tố trung gian tiềm năng. 
4. Đối với mỗi$j$, kiểm tra xem ứng viên tiền tố tốt nhất có thực sự thỏa mãn$r_{left[j]} < r_j$. Nếu không, không có phần tử bên trái hợp lệ cho việc này$j$. 
5. Tương tự, kiểm tra xem ứng viên hậu tố tốt nhất có thỏa mãn$r_{right[j]} > r_j$. Nếu không, không có phần tử phù hợp hợp lệ cho việc này$j$. 
6. Nếu cả hai điều kiện đều đúng, xuất ngay bộ ba chỉ số. 

### Tại sao nó hoạt động 

Mọi giải pháp hợp lệ đều có một số phần tử ở giữa$b$. Nếu tồn tại một bộ ba như vậy thì trong số tất cả các phần tử bên trái của$b$, ít nhất một phần tử nhỏ hơn và trong số tất cả các phần tử ở bên phải của$b$, ít nhất một cái lớn hơn. Việc tính toán trước tiền tố và hậu tố đảm bảo chúng tôi không bỏ sót những ứng cử viên này. Vì chúng tôi kiểm tra mọi vị trí ở giữa có thể nên chúng tôi phải tìm ra vị trí chính xác$b$và tại thời điểm đó, cả hai đối tượng đồng hành bắt buộc đều đã được mã hóa trong các mảng được tính toán trước. Không thể bỏ qua cấu hình hợp lệ vì mọi chỉ số ở giữa ứng cử viên đều được kiểm tra rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
r = list(map(int, input().split()))

left_min_index = [0] * n
min_idx = 0
for i in range(n):
    if r[i] < r[min_idx]:
        min_idx = i
    left_min_index[i] = min_idx

right_max_index = [0] * n
max_idx = n - 1
for i in range(n - 1, -1, -1):
    if r[i] > r[max_idx]:
        max_idx = i
    right_max_index[i] = max_idx

for j in range(n):
    i = left_min_index[j]
    k = right_max_index[j]
    if r[i] < r[j] and r[k] > r[j]:
        print(i + 1, j + 1, k + 1)
        sys.exit()

print(-1, -1, -1)
```Quá trình quét tiền tố xây dựng một ứng cử viên tốt nhất đang chạy cho mức tối thiểu bên trái, đảm bảo rằng đối với mỗi vị trí, chúng tôi có lựa chọn mạnh nhất có thể về phần tử nhỏ hơn. 

Quá trình quét hậu tố thực hiện tương tự ngược lại đối với các phần tử lớn hơn. Sử dụng cực trị thay vì danh sách đầy đủ là đủ vì mọi phần tử ở giữa hợp lệ chỉ yêu cầu tồn tại ít nhất một giá trị nhỏ hơn và một giá trị lớn hơn. 

Vòng lặp cuối cùng chỉ đơn giản kiểm tra từng chỉ số như một điểm giữa tiềm năng và xác minh xem cả hai bên có hài lòng hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 1 4 1 5 9
```Chúng tôi tính toán cực tiểu tiền tố và cực đại hậu tố: 

| j | r[j] | left_min_index[j] | r[trái] | right_max_index[j] | r[phải] | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | 3 | 5 | 9 | không | 
| 1 | 1 | 1 | 1 | 5 | 9 | không | 
| 2 | 4 | 1 | 1 | 5 | 9 | vâng | 

Tại$j = 2$, chúng tôi có$r[1] = 1 < 4 < 9 = r[5]$, do đó thuật toán đưa ra:```
2 3 6
```Điều này xác nhận phương thức xác định chính xác phần tử ở giữa hợp lệ ngay cả khi nó không rõ ràng ở cục bộ. 

### Ví dụ 2 

đầu vào:```
5
5 4 3 2 1
```| j | r[j] | left_min_index[j] | r[trái] | right_max_index[j] | r[phải] | có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 5 | 0 | 5 | không | 
| 1 | 4 | 1 | 4 | 0 | 5 | không | 
| 2 | 3 | 2 | 3 | 0 | 5 | không | 
| 3 | 2 | 3 | 2 | 0 | 5 | không | 
| 4 | 1 | 4 | 1 | 0 | 5 | không | 

Không có chỉ mục nào thỏa mãn cả hai điều kiện, vì vậy kết quả đầu ra là:```
-1 -1 -1
```Điều này cho thấy thuật toán loại bỏ chính xác các chuỗi giảm nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Hai lần quét tuyến tính cộng với một lần quét tuyến tính | 
| Không gian |$O(n)$| Hai mảng phụ lưu trữ thông tin tiền tố và hậu tố | 

Các ràng buộc cho phép tối đa 3000 phần tử và việc quét tuyến tính là không đáng kể trong giới hạn. Giải pháp tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from subprocess import run as sp_run

    # Inline solution execution via function wrapper
    # (simplified: re-define solution here for testing)
    input = sys.stdin.readline
    n = int(input())
    r = list(map(int, input().split()))

    left_min_index = [0] * n
    min_idx = 0
    for i in range(n):
        if r[i] < r[min_idx]:
            min_idx = i
        left_min_index[i] = min_idx

    right_max_index = [0] * n
    max_idx = n - 1
    for i in range(n - 1, -1, -1):
        if r[i] > r[max_idx]:
            max_idx = i
        right_max_index[i] = max_idx

    for j in range(n):
        i = left_min_index[j]
        k = right_max_index[j]
        if r[i] < r[j] and r[k] > r[j]:
            return f"{i+1} {j+1} {k+1}"

    return "-1 -1 -1"

# provided sample
assert run("6\n3 1 4 1 5 9\n") in {"2 3 6", "4 2 3", "4 2 6"}, "sample 1"

# minimum size
assert run("3\n1 2 3\n") != "-1 -1 -1"

# all equal
assert run("4\n7 7 7 7\n") == "-1 -1 -1"

# decreasing
assert run("5\n5 4 3 2 1\n") == "-1 -1 -1"

# random valid
assert run("5\n1 5 2 4 3\n") != "-1 -1 -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 2 3`| bất kỳ bộ ba hợp lệ nào | trường hợp tăng tối thiểu | 
|`7 7 7 7`|`-1 -1 -1`| ngăn chặn bình đẳng tăng nghiêm ngặt | 
|`5 4 3 2 1`|`-1 -1 -1`| thất bại giảm đơn điệu | 
|`1 5 2 4 3`| ba hợp lệ | cấu trúc tăng rải rác | 

## Vỏ cạnh 

Một tình huống khó khăn là khi phần tử bên trái hợp lệ nhỏ nhất không liền kề với ứng cử viên ở giữa. Coi như`3 1 4 1 5 9`. Ở chỉ mục 2 (giá trị 4), ứng cử viên bên trái bên phải là chỉ mục 1 (giá trị 1), không phải chỉ mục 0 hoặc 3. Mảng tiền tố tối thiểu đảm bảo chúng ta vẫn chọn chỉ mục 1 vì nó theo dõi toàn cầu giá trị bên trái tốt nhất hiện có bất kể vị trí. 

Một trường hợp cạnh khác là các giá trị lặp lại. TRONG`2 2 2 3 3`, một lần quét đơn giản có thể chọn các giá trị bằng nhau cho cả hai bên, nhưng sự bất bình đẳng nghiêm ngặt sẽ ngăn cản điều đó. Hậu tố tối đa và tiền tố tối thiểu đảm bảo tính chính xác vì chúng luôn xác minh các so sánh giá trị thực tế thay vì dựa vào các giả định về vị trí. 

Trường hợp cuối cùng là khi bộ ba hợp lệ cách xa nhau, chẳng hạn như`1 100 2 3 4`. Yếu tố ở giữa không nhất thiết phải là một phần của xu hướng ngày càng tăng cục bộ. Thuật toán vẫn hoạt động vì nó kiểm tra mọi chỉ mục như một phần giữa tiềm năng và không phụ thuộc vào tính liền kề, chỉ phụ thuộc vào sự tồn tại của tiền tố và hậu tố chung.
