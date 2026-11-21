---
title: "CF 234C - Thời tiết"
description: "Chúng tôi được cung cấp một chuỗi nhiệt độ hàng ngày. Chúng tôi có thể thay đổi bất kỳ nhiệt độ nào thành bất kỳ giá trị nào khác và mỗi vị trí được sửa đổi sẽ tốn một lần thay đổi. Mục tiêu là làm cho trình tự tuân theo một khuôn mẫu rất cụ thể."
date: "2026-06-04T10:04:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 1300
weight: 234
solve_time_s: 91
verified: true
draft: false
---

[CF 234C - Thời tiết](https://codeforces.com/problemset/problem/234/C) 

**Đánh giá:** 1300 
**Tags:** dp, triển khai 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhiệt độ hàng ngày. Chúng tôi có thể thay đổi bất kỳ nhiệt độ nào thành bất kỳ giá trị nào khác và mỗi vị trí được sửa đổi sẽ tốn một lần thay đổi. 

Mục tiêu là làm cho trình tự tuân theo một khuôn mẫu rất cụ thể. Phải có một vị trí phân chia sao cho mỗi ngày trước khi phân chia có nhiệt độ hoàn toàn âm và mỗi ngày sau khi phân chia có nhiệt độ hoàn toàn dương. Cả hai phần phải không trống. Nhiệt độ bằng 0 không bao giờ được phép ở trình tự cuối cùng. 

Chúng tôi cần số lượng vị trí tối thiểu phải được sửa đổi. 

Số ngày có thể lớn như$10^5$. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào kiểm tra mọi phân chia có thể và sau đó quét lại toàn bộ mảng, bởi vì điều đó sẽ yêu cầu$O(n^2)$làm việc, đại khái$10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính. 

Phần khó khăn là xử lý số không một cách chính xác. Nhiệt độ bằng 0 không hợp lệ ở cả hai phía của đường phân chia. Nếu một ngày thuộc phân đoạn âm thì số 0 phải được thay đổi. Nếu nó thuộc đoạn dương thì số 0 cũng phải được thay đổi. 

Một sai lầm dễ mắc phải khác là quên rằng cả hai phân đoạn phải chứa ít nhất một phần tử. Ví dụ:```
2
-5 -3
```Câu trả lời là`1`, không`0`. Chúng ta phải tạo một hậu tố dương nên ít nhất một giá trị phải thay đổi. 

Tương tự:```
2
4 7
```Câu trả lời là`1`, bởi vì chúng ta cần một tiền tố phủ định không trống. 

Coi như:```
3
0 0 0
```Mọi vị trí đều phải được thay đổi. Một kết quả hợp lệ là`-1 -1 1`, vậy câu trả lời là`3`. Bất kỳ cách tiếp cận nào coi số 0 là đã được chấp nhận sẽ thất bại ở đây. 

Một ví dụ tinh tế khác là:```
4
-1 1 -2 1
```Sự phân chia tốt nhất là sau phần tử thứ ba. Chỉ một`-2`cần phải trở nên tích cực, vì vậy câu trả lời là`1`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi vị trí phân chia có thể. 

Giả sử sự phân chia là sau vị trí$k$. Sau đó vị trí$1 \ldots k$phải âm và vị trí$k+1 \ldots n$phải tích cực. Chúng tôi có thể đếm có bao nhiêu phần tử vi phạm các yêu cầu đó và lấy mức tối thiểu trên tất cả các phần tách. 

Điều này đúng vì nó đánh giá rõ ràng mọi cấu trúc cuối cùng hợp lệ. Vấn đề là tốc độ. có$n-1$các phần tách có thể xảy ra và việc kiểm tra một phần tách một cách đơn giản đòi hỏi phải quét toàn bộ mảng. Sự phức tạp trở nên$O(n^2)$, quá chậm đối với$n=10^5$. 

Nhận xét quan trọng là chi phí của việc chia tách chỉ phụ thuộc vào hai loại vi phạm. 

Đối với tiền tố phủ định, một phần tử đã đúng nếu nó hoàn toàn âm. Nếu không nó phải được thay đổi. 

Đối với hậu tố dương, một phần tử đã đúng nếu nó hoàn toàn dương. Nếu không nó phải được thay đổi. 

Điều này gợi ý tiền xử lý thông tin tiền tố và hậu tố. 

Cho phép:`prefix[i]`= số phần tử trong số phần tử đầu tiên`i`những vị trí không âm. 

Đây chính xác là những vị trí cần sửa đổi nếu lần đầu tiên`i`các phần tử hình thành nên đoạn âm. 

Tương tự, hãy:`suffix[i]`= số phần tử từ vị trí`i`ĐẾN`n`điều đó không tích cực. 

Đây chính xác là những vị trí cần sửa đổi nếu vị trí`i...n`hình thành phần tích cực. 

Sau đó để phân chia sau vị trí`k`, chi phí trở thành:```
prefix[k] + suffix[k+1]
```Giờ đây, mọi phần phân chia có thể được đánh giá theo thời gian không đổi sau quá trình tiền xử lý tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng nhiệt độ. 
2. Xây dựng một mảng tiền tố trong đó`prefix[i]`lưu trữ bao nhiêu phần tử giữa các vị trí`1...i`không hoàn toàn tiêu cực. 

Một phần tử đóng góp vào số lượng này nếu nó bằng 0 hoặc dương, vì một trong hai giá trị sẽ cần sửa đổi bên trong phân đoạn âm. 
3. Xây dựng một mảng hậu tố trong đó`suffix[i]`lưu trữ bao nhiêu phần tử giữa các vị trí`i...n`không hoàn toàn tích cực. 

Một phần tử đóng góp vào số đếm này nếu nó bằng 0 hoặc âm, vì một trong hai giá trị sẽ cần sửa đổi bên trong phân đoạn dương. 
4. Lặp lại mọi vị trí phân chia hợp lệ`k`từ`1`ĐẾN`n-1`. 
5. Tính toán:```
cost = prefix[k] + suffix[k+1]
```Thuật ngữ đầu tiên tính những sửa đổi cần thiết trong tiền tố phủ định. Thuật ngữ thứ hai tính các sửa đổi cần thiết trong hậu tố tích cực. 
6. Giữ chi phí tối thiểu trong tất cả các lần chia tách. 
7. Xuất ra giá trị nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Đối với vị trí phân chia cố định, mọi phần tử đều thuộc về một phía. 

Nếu một phần tử nằm ở tiền tố phủ định, thì nó cần được sửa đổi một cách chính xác khi nó không hoàn toàn phủ định. Mảng tiền tố đếm chính xác những vi phạm này. 

Nếu một phần tử ở hậu tố dương, nó cần được sửa đổi một cách chính xác khi nó không hoàn toàn dương. Mảng hậu tố đếm chính xác những vi phạm này. 

Hai phân đoạn này rời rạc nên số lượng sửa đổi của chúng được cộng trực tiếp. Vì mọi sự sắp xếp cuối cùng hợp lệ đều tương ứng với chính xác một vị trí phân chia, nên việc đánh giá tất cả các phân chia và lấy giá trị tối thiểu mang lại câu trả lời tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    t = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + (1 if t[i - 1] >= 0 else 0)

    suffix = [0] * (n + 2)
    for i in range(n, 0, -1):
        suffix[i] = suffix[i + 1] + (1 if t[i - 1] <= 0 else 0)

    ans = n
    for k in range(1, n):
        ans = min(ans, prefix[k] + suffix[k + 1])

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng tiền tố tuân theo định nghĩa trực tiếp. Mọi giá trị lớn hơn hoặc bằng 0 đều vi phạm phân đoạn âm, vì vậy chúng tôi thêm một giá trị khi`t[i-1] >= 0`. 

Cấu trúc hậu tố có tính đối xứng. Mọi giá trị nhỏ hơn hoặc bằng 0 đều vi phạm phân đoạn dương, vì vậy chúng tôi thêm một giá trị khi`t[i-1] <= 0`. 

Lỗi triển khai phổ biến nhất là xử lý số không không chính xác. Số 0 không hợp lệ ở cả hai bên nên nó góp phần vào số lượng vi phạm của cả hai tùy thuộc vào vị trí của nó. 

Một lỗi dễ xảy ra khác xuất hiện trong vòng lặp phân tách. Phần tách phải để trống cả hai phần, vì vậy các giá trị hợp lệ là`1`bởi vì`n-1`. Cho phép`0`hoặc`n`sẽ tạo ra một phân đoạn trống và tạo ra câu trả lời sai. 

Không tồn tại vấn đề tràn số nguyên vì tất cả số đếm đều nhiều nhất`n`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
-1 1 -2 1
```Vi phạm tiền tố: 

| tôi | giá trị | tiền tố[i] | 
| --- | --- | --- | 
| 1 | -1 | 0 | 
| 2 | 1 | 1 | 
| 3 | -2 | 1 | 
| 4 | 1 | 2 | 

Vi phạm hậu tố: 

| tôi | hậu tố[i] | 
| --- | --- | 
| 4 | 0 | 
| 3 | 1 | 
| 2 | 1 | 
| 1 | 2 | 

Đánh giá phân chia: 

| k | tiền tố[k] | hậu tố[k+1] | chi phí | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 1 | 0 | 1 | 

Chi phí tối thiểu là`1`. 

Ví dụ này cho thấy sự phân chia tối ưu không nhất thiết phải là duy nhất. Việc phân chia sau vị trí 1 và 3 đều yêu cầu một sửa đổi. 

### Ví dụ 2 

đầu vào:```
3
0 0 0
```Vi phạm tiền tố: 

| tôi | giá trị | tiền tố[i] | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 0 | 2 | 
| 3 | 0 | 3 | 

Vi phạm hậu tố: 

| tôi | hậu tố[i] | 
| --- | --- | 
| 3 | 1 | 
| 2 | 2 | 
| 1 | 3 | 

Đánh giá phân chia: 

| k | tiền tố[k] | hậu tố[k+1] | chi phí | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 
| 2 | 2 | 1 | 3 | 

Trả lời:`3`. 

Điều này chứng tỏ tại sao số 0 phải được coi là vi phạm của cả hai bên. Mọi vị trí đều yêu cầu sửa đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt cho tiền tố, một lượt cho hậu tố, một lượt chuyển qua phần tách | 
| Không gian | O(n) | Mảng tiền tố và hậu tố | 

Với$n \le 10^5$, một thuật toán tuyến tính chỉ thực hiện vài trăm nghìn phép tính, thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ, chỉ cần một vài mảng số nguyên có độ dài$n$. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    t = list(map(int, input().split()))

    prefix = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix[i] = prefix[i - 1] + (t[i - 1] >= 0)

    suffix = [0] * (n + 2)
    for i in range(n, 0, -1):
        suffix[i] = suffix[i + 1] + (t[i - 1] <= 0)

    ans = n
    for k in range(1, n):
        ans = min(ans, prefix[k] + suffix[k + 1])

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("4\n-1 1 -2 1\n") == "1", "sample 1"

# minimum size, already valid
assert run("2\n-1 1\n") == "0"

# minimum size, all negative
assert run("2\n-5 -3\n") == "1"

# minimum size, all positive
assert run("2\n4 7\n") == "1"

# all zeros
assert run("3\n0 0 0\n") == "3"

# split near the end
assert run("5\n-1 -2 -3 -4 5\n") == "0"

# catches zero handling
assert run("4\n-1 0 0 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / -1 1`|`0`| Đã đáp ứng yêu cầu | 
|`2 / -5 -3`|`1`| Hậu tố dương phải không trống | 
|`2 / 4 7`|`1`| Tiền tố phủ định phải không trống | 
|`3 / 0 0 0`|`3`| Số không không hợp lệ ở mọi nơi | 
|`5 / -1 -2 -3 -4 5`|`0`| Chia gần ranh giới | 
|`4 / -1 0 0 1`|`2`| Xử lý đúng số 0 | 

## Vỏ cạnh 

Hãy xem xét:```
2
-5 -3
```Mảng tiền tố trở thành:```
0 0
```Số lượng vi phạm hậu tố cho vị trí cuối cùng là:```
1
```Chỉ có một phần chia hợp lệ, sau phần tử đầu tiên. Chi phí là:```
0 + 1 = 1
```Một nhiệt độ phải trở nên tích cực. Thuật toán xuất ra chính xác`1`. 

Bây giờ hãy xem xét:```
2
4 7
```Sự phân chia hợp lệ duy nhất lại là sau phần tử đầu tiên. 

Phần tử đầu tiên thuộc đoạn âm và dương nên góp phần vi phạm. Yếu tố thứ hai đã tích cực. 

Chi phí tính toán là`1`, đó là tối ưu. 

Cuối cùng hãy xem xét:```
3
0 0 0
```Mọi số 0 đều đóng góp như một sự vi phạm bất kể nó thuộc về bên nào. Thuật toán đếm một sửa đổi cho mỗi vị trí và trả về`3`. 

Điều này hoàn toàn chính xác bởi vì mọi nhiệt độ đều phải trở nên âm hoặc dương hoàn toàn, và không nhiệt độ nào có thể bằng không.
