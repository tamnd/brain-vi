---
title: "CF 102375C - \u0421\u043e\u0432\u043f\u0430\u0434\u0435\u043d\u0438\u044f"
description: "Có chính xác (N) phòng, được đánh số từ (1) đến (N) và có chính xác (N) người tham gia. Người tham gia (i) có số hộ chiếu (ai)."
date: "2026-08-15T07:02:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "C"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 338
verified: false
draft: false
---

[CF 102375C - \u0421\u043e\u0432\u043f\u0430\u0434\u0435\u043d\u0438\u044f](https://codeforces.com/problemset/problem/102375/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có chính xác (N) phòng, được đánh số từ (1) đến (N) và có chính xác (N) người tham gia. Người tham gia (i) có số hộ chiếu (a_i). Chúng tôi có thể chọn bất kỳ sự chỉ định từng người tham gia nào cho các phòng và người tham gia sẽ tạo một kết quả trùng khớp khi số hộ chiếu của họ bằng với số phòng họ nhận được. 

Nhiệm vụ là tối đa hóa số lượng trận đấu. Số hộ chiếu lớn hơn (N) không bao giờ có thể tạo ra kết quả trùng khớp vì không còn chỗ cho số đó. Đối với số hộ chiếu (x) có (1 \le x \le N), chỉ có thể xếp một người tham gia vào phòng (x), do đó, các bản sao của (x) không thể tạo ra nhiều kết quả trùng khớp. 

Hệ quả quan trọng là mỗi số hộ chiếu riêng biệt trong khoảng ([1,N]) đóng góp chính xác một kết quả trùng khớp có thể có. Chúng ta chỉ cần đếm xem có bao nhiêu giá trị khác nhau từ khoảng đó xuất hiện trong mảng. 

Giới hạn (N \le 10^5) đủ nhỏ cho thuật toán tuyến tính hoặc (O(N\log N)), nhưng nó loại trừ cách tiếp cận (O(N^2)) theo giới hạn thời gian lập trình cạnh tranh điển hình. Với (N=10^5), quét bậc hai có thể thực hiện khoảng (10^{10}) so sánh, vượt xa những gì thực tế. Số hộ chiếu có thể đạt tới (10^9), do đó việc phân bổ một mảng được lập chỉ mục theo số hộ chiếu sẽ lãng phí rất nhiều bộ nhớ. Bộ băm phù hợp tự nhiên vì nó chỉ lưu trữ các giá trị thực sự xảy ra. 

Một số trường hợp có thể khiến việc thực hiện bất cẩn tạo ra câu trả lời sai. Đầu tiên, các bản sao phải được tính một lần. Đối với đầu vào```
5
1
3
5
7
5
```Câu trả lời là (3), không phải (4), vì phòng (1), (3) và (5) có thể trùng nhau, trong khi hai người tham gia có hộ chiếu (5) tranh giành cùng một phòng. 

Thứ hai, các giá trị nằm ngoài phạm vi phòng phải được bỏ qua. Vì```
4
1000000000
1000000000
1000000000
1000000000
```đáp án là (0), vì không có phòng nào có số (10^9). 

Thứ ba, giá trị biên (N) là hợp lệ và phải được đưa vào. Vì```
3
1
2
3
```câu trả lời là (3). Một triển khai kiểm tra`a[i] < N`thay vì`a[i] <= N`sẽ trả lại không chính xác (2). 

Cuối cùng, phạm vi phòng nhỏ nhất phải hoạt động chính xác. Vì```
1
1
```câu trả lời là (1), trong khi đối với```
1
2
```câu trả lời là (0). Những trường hợp này bộc lộ những sai sót xung quanh điểm cuối của khoảng thời gian hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét từng phòng và tìm kiếm trong tất cả những người tham gia để tìm người có hộ chiếu bằng số phòng đó. Đối với mỗi phòng (r), chúng tôi quét số hộ chiếu (N) và kiểm tra xem một số người tham gia có hộ chiếu (r) hay không. Nếu có một người tham gia như vậy tồn tại, chúng tôi sẽ tính một trận đấu. Điều này đúng vì mỗi phòng có thể chứa chính xác một người tham gia, do đó sự tồn tại của ít nhất một người tham gia có hộ chiếu (r) là đủ để phòng (r) đóng góp một trận đấu. 

Vấn đề là khối lượng công việc. Có (N) phòng và mỗi phòng có thể yêu cầu kiểm tra (N) người tham gia, đưa ra (N^2) so sánh. Tại (N=10^5), tức là so sánh (10^{10}) trong trường hợp xấu nhất, quá chậm. 

Quan sát làm thay đổi vấn đề là danh tính của người tham gia không còn quan trọng khi chúng tôi biết liệu số hộ chiếu có xuất hiện hay không. Với mỗi số phòng (r) từ (1) đến (N), câu trả lời sẽ đạt chính xác khi (r) xuất hiện ít nhất một lần trong số các số hộ chiếu. Nhiều lần xuất hiện giống nhau (r) không giúp ích gì vì phòng (r) chỉ có thể chứa một người tham gia. 

Chúng tôi có thể đại diện cho tất cả các số hộ chiếu đã xuất hiện cùng với một bộ. Trong khi đọc đầu vào, chúng tôi chỉ chèn các giá trị thỏa mãn (1 \le a_i \le N). Cuối cùng, kích thước của tập hợp chính xác là số phòng mà người tham gia phù hợp tồn tại. 

Hai cách tiếp cận có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(N)) dự kiến ​​| (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N) và tạo một tập hợp trống có tên`seen`. Bộ này sẽ chứa từng số hộ chiếu có thể tương ứng với một phòng, với các số trùng lặp sẽ tự động bị xóa. 
2. Đọc từng (N) số hộ chiếu. Nếu một giá trị (a_i) thỏa mãn (1 \le a_i \le N), hãy chèn nó vào`seen`. Các giá trị lớn hơn (N) không bao giờ có thể khớp với bất kỳ phòng nào, vì vậy việc lưu trữ chúng là không cần thiết. 
3. Sau khi tất cả người tham gia đã được xử lý, xuất ra`len(seen)`. Mỗi giá trị trong bộ tương ứng với một số phòng riêng biệt có thể được so khớp bằng cách chỉ định người tham gia có số hộ chiếu đó vào phòng tương ứng. 

Tại sao tất cả những trận đấu này có thể đạt được cùng một lúc? Giả sử tập hợp chứa (k) số hộ chiếu hợp lệ riêng biệt. Mỗi người tương ứng với một số phòng khác nhau, vì vậy chúng tôi có thể chọn một người tham gia cho mỗi giá trị hộ chiếu riêng biệt và đưa người tham gia đó vào phòng phù hợp. Những người tham gia này khác nhau vì một người tham gia chỉ có một số hộ chiếu và các giá trị hộ chiếu khác nhau không thể thuộc về cùng một người tham gia. Những người tham gia còn lại có thể được phân công tùy ý vào các phòng còn lại. 

### Tại sao nó hoạt động 

Bất biến sau khi xử lý bất kỳ tiền tố nào của người tham gia là`seen`chứa chính xác số hộ chiếu riêng biệt từ tiền tố đó nằm trong phạm vi phòng (1) đến (N). Cuối cùng, với mỗi phòng (r), tập hợp chứa (r) chính xác khi có ít nhất một người tham gia có hộ chiếu (r). Nếu có (r), một người tham gia như vậy có thể được chỉ định vào phòng (r), tạo ra một trận đấu. Nếu (r) vắng mặt, không có bài tập nào có thể làm cho phòng (r) khớp. Vì các giá trị khác nhau trong`seen`tương ứng với các phòng khác nhau, tất cả các trận đấu có thể có này có thể đạt được cùng một lúc. Do đó, kích thước của tập hợp chính xác là số lượng kết quả phù hợp tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    seen = set()

    for _ in range(n):
        passport = int(input())
        if passport <= n:
            seen.add(passport)

    print(len(seen))

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi số phòng và số người tham gia. Bộ này ban đầu trống, khớp với trạng thái trước khi bất kỳ số hộ chiếu nào được xử lý. 

Đối với mọi người tham gia, mã sẽ kiểm tra`passport <= n`. Số hộ chiếu được đảm bảo ít nhất là (1), do đó không cần phải kiểm tra giới hạn dưới một cách rõ ràng. Hộ chiếu bằng (n) phải được chấp nhận, đó là lý do tại sao việc so sánh được thực hiện`<=`còn hơn là`<`. 

Đang gọi`seen.add(passport)`tự động xử lý các bản sao. Nếu năm người tham gia có hộ chiếu (5), bộ vẫn chỉ chứa một bản sao của (5), khớp hoàn toàn với thực tế là chỉ một người trong số họ có thể chiếm phòng (5). 

Không có vấn đề tràn số nguyên trong Python và giá trị hộ chiếu lớn nhất, (10^9), được xử lý trực tiếp bởi loại số nguyên. Quan trọng hơn, chúng tôi không bao giờ cố gắng phân bổ một mảng có kích thước (10^9), vì vậy phạm vi hộ chiếu lớn không có tác động tiêu cực đến việc sử dụng bộ nhớ. 

trận chung kết`len(seen)`là câu trả lời vì mỗi giá trị được lưu trữ tương ứng với một phòng phù hợp riêng biệt có thể đạt được. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là```
5
1
3
5
7
5
```Trạng thái thay đổi như sau. 

| Người tham gia | Hộ chiếu | Số phòng hợp lệ? |`seen`sau khi xử lý | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Có |`{1}`| 1 | 
| 2 | 3 | Có |`{1, 3}`| 2 | 
| 3 | 5 | Có |`{1, 3, 5}`| 3 | 
| 4 | 7 | Không |`{1, 3, 5}`| 3 | 
| 5 | 5 | Có, đã có mặt |`{1, 3, 5}`| 3 | 

Tập cuối cùng chứa (1), (3) và (5). Chúng tương ứng với các phòng (1), (3) và (5), do đó có thể tạo ba kết quả khớp. Lần xuất hiện thứ hai của hộ chiếu (5) không làm tăng câu trả lời vì phòng (5) đã được người tham gia đầu tiên mang hộ chiếu đó chiếm giữ. 

Đối với Mẫu 2, đầu vào là```
4
1000000000
1000000000
1000000000
1000000000
```Dấu vết là: 

| Người tham gia | Hộ chiếu | Số phòng hợp lệ? |`seen`sau khi xử lý | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1000000000 | Không |`{}`| 0 | 
| 2 | 1000000000 | Không |`{}`| 0 | 
| 3 | 1000000000 | Không |`{}`| 0 | 
| 4 | 1000000000 | Không |`{}`| 0 | 

Tất cả số hộ chiếu đều vượt quá (N=4), vì vậy không có số nào có thể khớp với một phòng. Tập cuối cùng trống và câu trả lời là (0). Dấu vết này cũng chứng minh tại sao thuật toán không cần quan tâm đến số lần giá trị hộ chiếu không thể xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) dự kiến ​​| Mỗi hộ chiếu được đọc một lần và chèn vào bộ băm trong thời gian không đổi dự kiến. | 
| Không gian | (O(N)) | Có thể lưu trữ tối đa (N) số hộ chiếu riêng biệt. | 

Với (N \le 10^5), đường chuyền tuyến tính chỉ thực hiện một số thao tác nhỏ cho mỗi người tham gia. Bộ này chứa tối đa (10^5) số nguyên, do đó mức tiêu thụ bộ nhớ cũng nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io
from contextlib import redirect_stdout

def solve():
    n = int(input())
    seen = set()

    for _ in range(n):
        passport = int(input())
        if passport <= n:
            seen.add(passport)

    print(len(seen))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

    return output.getvalue().strip()

# Provided samples
assert run("5\n1\n3\n5\n7\n5\n") == "3", "sample 1"
assert run("4\n1000000000\n1000000000\n1000000000\n1000000000\n") == "0", "sample 2"

# Minimum-size inputs
assert run("1\n1\n") == "1", "minimum size, valid passport"
assert run("1\n2\n") == "0", "minimum size, passport outside room range"

# All values are equal, so duplicates must count only once
assert run("6\n4\n4\n4\n4\n4\n4\n") == "1", "duplicate passports"

# Boundary condition: both 1 and N are valid
assert run("3\n1\n2\n3\n") == "3", "upper boundary N must be included"

# Mixed valid and invalid values
assert run("6\n1\n6\n7\n2\n2\n1000000000\n") == "3", "valid range and duplicates"

# Maximum-size input
maximum_case = "100000\n" + "1\n" * 100000
assert run(maximum_case) == "1", "maximum N with all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`1`| Kích thước đầu vào tối thiểu chỉ có hộ chiếu phù hợp với phòng 1 | 
|`1\n2\n`|`0`| Kích thước đầu vào tối thiểu với hộ chiếu nằm ngoài phạm vi phòng | 
|`6\n4\n4\n4\n4\n4\n4\n`|`1`| Số hộ chiếu trùng lặp phải được tính một lần | 
|`3\n1\n2\n3\n`|`3`| Cả hai điểm cuối, đặc biệt là hộ chiếu (N), đều hợp lệ | 
|`6\n1\n6\n7\n2\n2\n1000000000\n`|`3`| Hỗn hợp các giá trị hợp lệ, trùng lặp và giá trị lớn hơn (N) | 
|`100000`bản sao hộ chiếu`1`|`1`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là số hộ chiếu trùng lặp. Coi như```
5
1
3
5
7
5
```Thuật toán chèn (1), (3) và (5) vào tập hợp. Hộ chiếu (7) bị loại bỏ vì không còn chỗ (7), còn hộ chiếu thứ hai (5) giữ nguyên bộ vì (5) đã có sẵn. Kết quả là (3). Thuộc tính quan trọng là tập hợp này đại diện cho các phòng có thể được ghép, thay vì những người tham gia có thể được ghép. 

Trường hợp cạnh thứ 2 là số hộ chiếu nằm ngoài phạm vi phòng. Với```
4
1000000000
1000000000
1000000000
1000000000
```mọi giá trị đều thất bại`passport <= n`kiểm tra. Tập hợp vẫn trống nên kết quả là (0). Điều này tránh được cả kết quả khớp không chính xác và việc lưu trữ các giá trị không liên quan không cần thiết. 

Trường hợp cạnh thứ ba là ranh giới trên. Vì```
3
1
2
3
```thuật toán chấp nhận cả ba giá trị vì (1 \le a_i \le 3). Tập hợp trở thành`{1, 2, 3}`, đưa ra câu trả lời (3). Điều kiện phải bao gồm sự bằng nhau với (N), nếu không phòng (N) sẽ bị loại trừ không chính xác. 

Trường hợp tối thiểu có kết quả khớp hợp lệ là```
1
1
```Ở đây (N=1) và hộ chiếu (1) đã vượt qua kiểm tra phạm vi. Tập hợp trở thành`{1}`, vậy đáp án là (1). Đối với đầu vào có liên quan chặt chẽ```
1
2
```hộ chiếu (2) lớn hơn số phòng duy nhất nên tập hợp vẫn trống và câu trả lời là (0). 

Hộp có kích thước tối đa có thể chứa (100000) số hộ chiếu giống hệt nhau. Bộ vẫn chỉ lưu trữ một giá trị, trong khi thuật toán thực hiện chính xác một lần chuyển qua đầu vào. Điều này chứng tỏ tại sao giải pháp vẫn tuyến tính ngay cả khi số lượng người tham gia ở mức tối đa.
