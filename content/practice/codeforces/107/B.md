---
title: "CF 107B - Đội bóng rổ"
description: "Chúng tôi có một trường đại học với nhiều khoa. Mỗi bộ phận đóng góp một số cầu thủ bóng rổ. Herr Wafa thuộc bộ phận h, và anh ấy đã được đảm bảo một suất trong đội cuối cùng. Đội phải có chính xác n người chơi bao gồm cả Wafa."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 107
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 83 (Div. 1 Only)"
rating: 1600
weight: 107
solve_time_s: 142
verified: true
draft: false
---

[CF 107B - Đội bóng rổ](https://codeforces.com/problemset/problem/107/B) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một trường đại học với nhiều khoa. Mỗi bộ phận đóng góp một số cầu thủ bóng rổ. Herr Wafa thuộc bộ phận`h`, và anh ấy đã được đảm bảo một suất trong đội cuối cùng. 

Nhóm phải chứa chính xác`n`những người chơi bao gồm cả chính Wafa. Mọi đội hợp lệ có chứa Wafa đều có khả năng như nhau. Chúng ta cần xác suất để ít nhất một đồng đội của Wafa cũng xuất thân từ bộ phận của anh ấy. 

Giả sử bộ phận`h`chứa`s[h]`tổng số người chơi, bao gồm cả Wafa. Sau đó có`s[h] - 1`những người chơi khác trong bộ phận của anh ấy có thể trở thành đồng đội của anh ấy. 

Tổng số người chơi hiện có là tổng của tất cả`s[i]`. Nếu tổng này nhỏ hơn`n`, việc thành lập một đội hoàn chỉnh là không thể, vì vậy câu trả lời là`-1`. 

Các ràng buộc đủ nhỏ để có thể thực hiện được các phép tính tổ hợp. Số lượng phòng ban có thể lên tới 1000 và quy mô mỗi phòng ban có thể đạt tới 100, vì vậy tổng số người chơi tối đa là`100000`. Chúng tôi không thể liệt kê tất cả các đội có thể vì số lượng kết hợp tăng theo cấp số nhân. Ngay cả việc chọn 50 người chơi từ 1000 người cũng đã tạo ra rất nhiều khả năng. 

Quan sát quan trọng là chúng ta chỉ cần một xác suất chứ không cần các đội thực tế. Điều này gợi ý mạnh mẽ việc đếm các kết hợp về mặt toán học. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ các triển khai ngây thơ. 

Hãy xem xét đầu vào này:```
5 2 1
2 2
```Trường đại học chỉ có 4 người chơi, nhưng quy mô đội là 5. Kết quả đúng là:```
-1
```Việc triển khai bất cẩn vẫn có thể cố gắng tính toán các kết hợp có tham số không hợp lệ. 

Một trường hợp khó khăn khác xuất hiện khi Wafa là người chơi duy nhất trong bộ phận của anh ta:```
3 3 2
5 1 5
```Có đủ tổng số người chơi, nhưng không có đồng đội nào có thể có từ bộ phận 2. Xác suất đúng là:```
0
```Một số cách triển khai chia không chính xác cho 0 hoặc cho rằng mỗi bộ phận đều đóng góp ít nhất hai người chơi. 

Trường hợp tinh vi thứ ba xảy ra khi phải chọn toàn bộ trường đại học:```
4 2 1
2 2
```Tất cả bốn người chơi phải được chọn. Vì bộ phận 1 có hai người chơi trong đó có Wafa nên câu trả lời là:```
1
```Phép tính gần đúng dấu phẩy động dựa trên xác suất lặp có thể tích lũy lỗi ở đây nếu được viết bất cẩn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Vì Wafa luôn được chọn nên chúng ta chỉ cần chọn phần còn lại`n - 1`người chơi từ những người khác. Chúng ta có thể liệt kê mọi tập hợp con có thể có kích thước`n - 1`, kiểm tra xem có ít nhất một người chơi được chọn đến từ bộ phận của Wafa hay không và đếm xem có bao nhiêu tập hợp con thỏa mãn điều kiện. 

Điều này hoạt động về mặt khái niệm vì mọi đội hợp lệ có Wafa đều có khả năng như nhau. Xác suất chỉ đơn giản là:$$\frac{\text{favorable teams}}{\text{all teams}}$$Vấn đề là số lượng tập hợp con. Nếu có khoảng 100000 người chơi và chúng ta phải chọn thậm chí 50 người trong số họ thì số lượng kết hợp là rất lớn. Việc liệt kê đầy đủ là hoàn toàn không thể. 

Điều quan trọng cần lưu ý là chúng ta không cần phải tạo ra các nhóm một cách rõ ràng. Chúng ta chỉ cần đếm chúng. 

Sau khi sửa Wafa trong đội, có:$$\text{total players} - 1$$các ứng cử viên còn lại, và chúng tôi chọn:$$n - 1$$của họ. 

Thay vì tính trực tiếp các đội thuận lợi, việc tính sự kiện bổ sung sẽ dễ dàng hơn. 

Điều tồi tệ là không có đồng đội nào của Wafa đến từ bộ phận của anh ấy. Kể từ khi bộ phận`h`có`s[h] - 1`những người chơi khác ngoài Wafa, chúng ta phải tránh tất cả bọn họ. 

Điều đó để lại:$$(\text{total players} - 1) - (s[h] - 1)$$những người chơi đủ điều kiện bên ngoài bộ phận của mình. 

Xác suất của sự kiện xấu trở thành:$$\frac{\binom{\text{outside}}{n-1}} {\binom{\text{total}-1}{n-1}}$$Khi đó câu trả lời bắt buộc là:$$1 - \text{bad probability}$$Điều này biến đổi vấn đề từ phép liệt kê hàm mũ thành một vài phép tính tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | Tổ hợp O(n) hoặc O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, Và`h`. 
2. Đọc mảng kích thước phòng ban`s`. 
3. Tính tổng số người chơi:$$total = \sum s[i]$$4. Nếu`total < n`, in`-1`. 

Việc thành lập một đội hợp lệ là không thể vì trường đại học không có đủ người chơi. 
5. Hãy:$$own = s[h-1]$$Đây là số lượng người chơi trong bộ phận của Wafa, bao gồm cả chính Wafa. 
6. Tính tổng số cách chọn đồng đội của Wafa:$$\binom{total-1}{n-1}$$Chúng tôi trừ một vì Wafa đã được cố định trong đội. 
7. Tính số đội kém mà không có đồng đội nào đến từ bộ phận của Wafa. 

Có:$$total - own$$người chơi bên ngoài bộ phận của mình, vì vậy số lượng là:$$\binom{total-own}{n-1}$$8. Xác suất xấu là:$$\frac{\binom{total-own}{n-1}} {\binom{total-1}{n-1}}$$9. In:$$1 - \text{bad probability}$$### Tại sao nó hoạt động 

Mọi đội có chứa Wafa đều có khả năng như nhau, do đó xác suất giảm xuống khi đếm các kết hợp. 

Mẫu số tính tất cả các lựa chọn đồng đội có thể có. Tử số của sự kiện bù sẽ tính chính xác những lựa chọn mà mọi đồng đội đều đến từ bộ phận khác. Vì hai bộ này phân chia tất cả các khả năng thành "tốt" và "xấu", nên việc trừ xác suất xấu từ 1 sẽ đưa ra câu trả lời cần thiết. 

Thuật toán không thể đếm quá hoặc bỏ lỡ các trường hợp vì mỗi nhóm tương ứng với chính xác một tập hợp con của`n - 1`đồng đội. 

## Giải pháp Python```python
import sys
from math import comb

input = sys.stdin.readline

def solve():
    n, m, h = map(int, input().split())
    s = list(map(int, input().split()))

    total = sum(s)

    if total < n:
        print(-1)
        return

    own = s[h - 1]

    total_ways = comb(total - 1, n - 1)
    bad_ways = comb(total - own, n - 1)

    ans = 1.0 - (bad_ways / total_ways)

    print(ans)

solve()
```Phần đầu tiên đọc đầu vào và tính tổng số người chơi. 

Trường hợp không thể xảy ra phải được xử lý trước khi thực hiện bất kỳ phép tính tổ hợp nào. Nếu như`total < n`, kết hợp như`C(3, 5)`sẽ không hợp lệ. 

biểu thức`comb(total - 1, n - 1)`đếm tất cả các set đồng đội có thể có vì bản thân Wafa đã được cố định trong đội. 

biểu thức`comb(total - own, n - 1)`là tinh tế. Chúng tôi xóa tất cả người chơi khỏi bộ phận của Wafa, bao gồm cả các thành viên khác của bộ phận đó. Vì Wafa đã được sửa lỗi riêng nên chỉ những người chơi ngoài bộ phận mới đủ điều kiện tham gia sự kiện xấu. 

Câu trả lời cuối cùng sử dụng phép chia dấu phẩy động. Số nguyên Python xử lý các giá trị kết hợp lớn tùy ý một cách an toàn, do đó không có rủi ro tràn. 

Việc lập chỉ mục`s[h - 1]`rất dễ mắc sai lầm vì các phòng ban được đánh số từ 1 trong khi danh sách Python được lập chỉ mục bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 1
2 1
```Có tổng cộng 3 người chơi. Bộ phận của Wafa có 2 người chơi. 

| Biến | Giá trị | 
| --- | --- | 
| tổng cộng | 3 | 
| sở hữu | 2 | 
| tổng cộng - 1 | 2 | 
| n - 1 | 2 | 
| tổng số cách | C(2,2) = 1 | 
| cách xấu | C(1,2) = 0 | 
| trả lời | 1 - 0/1 = 1 | 

Tất cả người chơi phải được chọn, vì vậy người chơi thứ hai từ bộ phận của Wafa đảm bảo sẽ xuất hiện trong đội. 

### Ví dụ 2 

đầu vào:```
2 3 1
2 1 1
```Wafa cần một đồng đội. 

| Biến | Giá trị | 
| --- | --- | 
| tổng cộng | 4 | 
| sở hữu | 2 | 
| tổng cộng - 1 | 3 | 
| n - 1 | 1 | 
| tổng số cách | C(3,1) = 3 | 
| cách xấu | C(2,1) = 2 | 
| trả lời | 1 - 2/3 = 1/3 | 

Có ba đồng đội có thể. Chỉ có một người thuộc bộ phận của Wafa nên xác suất là`1/3`. 

Dấu vết này xác nhận rằng việc đếm sự kiện phần bù phù hợp với lý luận trực tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) hiệu quả | Chỉ một vài phép tính tổ hợp | 
| Không gian | O(1) | Sử dụng số lượng biến không đổi | 

Giải pháp dễ dàng phù hợp trong giới hạn. Tích hợp sẵn của Python`math.comb`xử lý các kết hợp lớn một cách hiệu quả và thuật toán chỉ thực hiện một số lượng hoạt động không đổi bất kể số lượng phòng ban. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import comb

def solve():
    input = sys.stdin.readline

    n, m, h = map(int, input().split())
    s = list(map(int, input().split()))

    total = sum(s)

    if total < n:
        print(-1)
        return

    own = s[h - 1]

    total_ways = comb(total - 1, n - 1)
    bad_ways = comb(total - own, n - 1)

    ans = 1.0 - (bad_ways / total_ways)

    print(ans)

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
assert run("3 2 1\n2 1\n") == "1.0", "sample 1"

# impossible case
assert run("5 2 1\n2 2\n") == "-1", "not enough players"

# Wafa alone in department
assert run("3 3 2\n5 1 5\n") == "0.0", "no teammate possible"

# all players selected
assert run("4 2 1\n2 2\n") == "1.0", "forced inclusion"

# symmetric departments
res = float(run("3 3 1\n2 2 2\n"))
assert abs(res - 0.4) < 1e-9, "probability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 1 / 2 2`|`-1`| Đội hình bất khả thi | 
|`3 3 2 / 5 1 5`|`0.0`| Wafa không có đồng đội cùng bộ phận | 
|`4 2 1 / 2 2`|`1.0`| Toàn bộ trường đại học phải được chọn | 
|`3 3 1 / 2 2 2`|`0.4`| Xác suất tổ hợp chung | 

## Vỏ cạnh 

Hãy xem xét tình huống không thể xảy ra:```
5 2 1
2 2
```Tổng số người chơi chỉ có 4, nhưng quy mô đội là 5. Thuật toán kiểm tra`total < n`ngay lập tức và in`-1`. Không có công thức tổ hợp nào được đánh giá, vì vậy các trạng thái không hợp lệ sẽ được tránh một cách an toàn. 

Bây giờ hãy xem xét trường hợp Wafa làm việc một mình trong bộ phận của mình:```
3 3 2
5 1 5
```Đây`own = 1`. Điều đó có nghĩa là không có thêm người chơi nào từ bộ phận của anh ấy. 

Thuật toán tính toán:$$\binom{10-1}{2} = \binom{9}{2}$$cho tất cả các lựa chọn đồng đội có thể có và:$$\binom{10-1}{2} = \binom{9}{2}$$một lần nữa cho những lựa chọn tồi, bởi vì mọi đồng đội có thể có đều ở ngoài bộ phận của anh ta. Xác suất kết quả trở thành:$$1 - 1 = 0$$cái nào đúng. 

Cuối cùng, hãy xem xét trường hợp lựa chọn bắt buộc:```
4 2 1
2 2
```Quy mô đội bằng tổng số người chơi. Mọi người chơi đều phải được chọn. 

Thuật toán tính toán:$$\binom{3}{3} = 1$$tổng số đội, và:$$\binom{2}{3} = 0$$những đội xấu. Vì không có đội xấu nào tồn tại nên câu trả lời trở nên chính xác`1`.
