---
title: "CF 1145A - Loại Thanos"
description: "Chúng ta bắt đầu với một mảng có độ dài là lũy thừa của hai. Hoạt động được phép rất bất thường: bất cứ khi nào mảng hiện tại không được sắp xếp theo thứ tự không giảm, chúng ta có thể xóa nửa bên trái hoặc nửa bên phải của nó. Sau đó, chúng tôi lặp lại quá trình tương tự trên nửa còn lại."
date: "2026-06-12T03:25:49+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1145
codeforces_index: "A"
codeforces_contest_name: "April Fools Day Contest 2019"
rating: 0
weight: 1145
solve_time_s: 83
verified: true
draft: false
---

[CF 1145A - Phân loại Thanos](https://codeforces.com/problemset/problem/1145/A) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng có độ dài là lũy thừa của hai. Hoạt động được phép rất bất thường: bất cứ khi nào mảng hiện tại không được sắp xếp theo thứ tự không giảm, chúng ta có thể xóa nửa bên trái hoặc nửa bên phải của nó. Sau đó, chúng tôi lặp lại quá trình tương tự trên nửa còn lại. 

Bởi vì mọi thao tác sẽ loại bỏ chính xác một nửa phân đoạn hiện tại, nên mảng duy nhất mà chúng ta có thể thu được là các phân đoạn liền kề có độ dài là lũy thừa của hai và ranh giới của chúng phát sinh từ việc liên tục chọn nửa bên trái hoặc bên phải trong quá trình phân chia đệ quy. 

Nhiệm vụ là tìm kích thước lớn nhất có thể của một phân đoạn có thể trở thành kết quả cuối cùng và đã được sắp xếp theo thứ tự không giảm. 

Những hạn chế là rất nhỏ. Độ dài mảng tối đa là 16, do đó, ngay cả các phương pháp tiếp cận theo cấp số nhân cũng sẽ phù hợp một cách thoải mái. Điều này có nghĩa là thách thức không phải là hiệu suất mà là việc nhận ra cấu trúc được tạo ra bởi hoạt động giảm một nửa lặp đi lặp lại. 

Một lỗi phổ biến là tìm mảng con được sắp xếp dài nhất. Sắp xếp Thanos không thể giữ một mảng con tùy ý. Nó chỉ có thể giữ các phân đoạn được tạo ra bằng cách giảm một nửa đệ quy. 

Coi như:```
8
1 2 3 4 100 0 1 2
```Mảng con`[1,2,3,4]`có độ dài 4 và đã được sắp xếp, có thể lấy được vì nó chính xác là nửa bên trái. 

Bây giờ hãy xem xét:```
8
100 1 2 3 4 5 6 7
```Mảng con`[1,2,3,4,5,6,7]`có độ dài 7 và đã được sắp xếp, nhưng không thể lấy được vì không có chuỗi xóa một nửa nào có thể để lại đoạn có độ dài 7. Câu trả lời đúng là 4. 

Một trường hợp cạnh khác là mảng giảm hoàn toàn:```
4
4 3 2 1
```Không có đoạn nào có độ dài 2 được sắp xếp. Cuối cùng, chúng ta chỉ có thể giữ lại một phần tử duy nhất và bất kỳ phần tử đơn lẻ nào cũng được sắp xếp một cách tầm thường. Câu trả lời là 1. 

Trường hợp tinh vi thứ ba là khi toàn bộ mảng đã được sắp xếp:```
4
1 2 2 4
```Câu trả lời là 4. Việc triển khai đệ quy bất cẩn luôn tiếp tục phân tách sẽ trả về sai 2 hoặc 1. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng mọi chuỗi quyết định có thể xảy ra của Thanos. Tại mỗi phân đoạn, chúng ta có thể dừng nếu nó được sắp xếp, nếu không chúng ta sẽ khám phá cả hai nửa một cách đệ quy. Vì mỗi phép chia chia đôi độ dài đoạn nên phép đệ quy tạo thành một cây nhị phân hoàn chỉnh. 

Lực lượng vũ phu này thực sự khả thi ở đây. Đối với mảng có kích thước 16, có rất ít trạng thái đệ quy. Mỗi phân đoạn có thể truy cập tương ứng với một nút trong cây giảm một nửa. 

Quan sát quan trọng là loại Thanos chỉ có thể tạo ra các phân đoạn phát sinh từ việc giảm một nửa đệ quy. Chúng ta không bao giờ cần phải xem xét các mảng con tùy ý. 

Giả sử chúng ta kiểm tra một phân khúc. Nếu nó đã được sắp xếp thì bản thân phân đoạn này là câu trả lời cuối cùng hợp lệ và độ dài của nó là một ứng cử viên. Nếu nó không được sắp xếp, bước đi hợp pháp duy nhất tiếp theo là giữ lại nửa bên trái hoặc nửa bên phải. Điều đó tự nhiên dẫn đến một định nghĩa đệ quy: 

Đối với một phân đoạn: 

Nếu nó được sắp xếp, hãy trả về độ dài của nó. 

Ngược lại, giải đệ quy cả hai nửa và lấy kết quả tối đa. 

Sự đệ quy phản ánh trực tiếp các quy tắc của quá trình. Mỗi phân khúc có thể tiếp cận được khám phá chính xác một lần. 

Mặc dù các ràng buộc rất nhỏ nhưng công thức đệ quy này cũng là giải pháp rõ ràng và tự nhiên nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các phân khúc có thể tiếp cận | O(n log n) | O(log n) | Đã chấp nhận | 
| Phân chia và chinh phục đệ quy | O(n log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm đệ quy hoạt động trên một đoạn của mảng. 
2. Kiểm tra xem phân đoạn hiện tại có được sắp xếp theo thứ tự không giảm hay không. 
3. Nếu nó được sắp xếp, hãy trả về độ dài đoạn ngay lập tức. 

Điều này đúng vì quá trình sắp xếp của Thanos có thể dừng ngay khi mảng hiện tại được sắp xếp. 
4. Nếu không, hãy chia đoạn thành nửa bên trái và nửa bên phải. 
5. Tính toán đệ quy câu trả lời tốt nhất có được từ nửa bên trái. 
6. Tính toán đệ quy câu trả lời tốt nhất có được từ nửa bên phải. 
7. Trả về kết quả lớn hơn trong hai kết quả. 

Khi phân đoạn hiện tại không được sắp xếp, các quy tắc cho phép chúng tôi giữ lại một nửa. Câu trả lời tốt nhất có thể đạt được phải đến từ một trong hai lựa chọn đó. 

### Tại sao nó hoạt động 

Mỗi mảng mà Thanos sắp xếp có thể đạt được tương ứng với một phân đoạn được tạo bằng cách liên tục lấy nửa bên trái hoặc bên phải của phân đoạn hiện tại. Đệ quy truy cập chính xác các phân đoạn đó. 

Đối với bất kỳ phân khúc nào, chỉ có hai khả năng. Hoặc là nó đã được sắp xếp, trong trường hợp đó có thể đạt được toàn bộ chiều dài của nó và không có câu trả lời nào lớn hơn có thể đến từ bên trong cùng phân khúc đó hoặc nó không được sắp xếp, trong trường hợp đó, nước đi hợp pháp đầu tiên phải loại bỏ một nửa và tiếp tục ở nửa kia. Do đó, câu trả lời tối ưu là câu trả lời tối đa có thể đạt được từ hai nửa. 

Bằng cách quy nạp theo độ dài đoạn, hàm đệ quy trả về đoạn được sắp xếp lớn nhất có thể đạt được cho mỗi đoạn mà nó xử lý. Áp dụng nó cho toàn bộ mảng sẽ mang lại câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_segment(arr):
    m = len(arr)

    sorted_segment = True
    for i in range(m - 1):
        if arr[i] > arr[i + 1]:
            sorted_segment = False
            break

    if sorted_segment:
        return m

    half = m // 2
    return max(
        solve_segment(arr[:half]),
        solve_segment(arr[half:])
    )

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    print(solve_segment(a))

if __name__ == "__main__":
    solve()
```Hàm đệ quy biểu thị mảng hiện tại còn lại sau một số lần búng tay. 

Vòng lặp đầu tiên kiểm tra xem phân đoạn đã được sắp xếp chưa. Nếu mọi cặp liền kề đều thỏa mãn`a[i] <= a[i+1]`, phân đoạn là trạng thái cuối cùng hợp lệ và chúng tôi trả về độ dài của nó. 

Khi phân đoạn không được sắp xếp, hành động pháp lý duy nhất là giữ nửa bên trái hoặc giữ nửa bên phải. Các cuộc gọi đệ quy tương ứng chính xác với hai lựa chọn đó. 

Trường hợp cơ bản không cần phải viết rõ ràng. Một đoạn có độ dài 1 luôn được sắp xếp, do đó việc kiểm tra đã sắp xếp thành công và trả về 1. 

Một chi tiết triển khai dễ bị bỏ qua là việc sử dụng thứ tự không giảm. Cho phép các giá trị liền kề bằng nhau, do đó việc so sánh phải được thực hiện`>`còn hơn là`>=`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 2 4
```| Phân đoạn | Chiều dài | Sắp xếp? | Trở về | 
| --- | --- | --- | --- | 
| [1,2,2,4] | 4 | Có | 4 | 

Toàn bộ mảng đã được sắp xếp nên quá trình đệ quy sẽ dừng ngay lập tức. Điều này chứng tỏ rằng chúng ta không nên tiếp tục phân tách khi đã tìm thấy một phân đoạn đã được sắp xếp. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```| Phân đoạn | Chiều dài | Sắp xếp? | Hành động | 
| --- | --- | --- | --- | 
| [4,3,2,1] | 4 | Không | Chia | 
| [4,3] | 2 | Không | Chia | 
| [4] | 1 | Có | Trở lại 1 | 
| [3] | 1 | Có | Trở lại 1 | 
| [2,1] | 2 | Không | Chia | 
| [2] | 1 | Có | Trở lại 1 | 
| [1] | 1 | Có | Trở lại 1 | 

Tổng hợp kết quả: 

| Phân đoạn | Kết quả | 
| --- | --- | 
| [4,3] | 1 | 
| [2,1] | 1 | 
| [4,3,2,1] | 1 | 

Câu trả lời là 1. Ví dụ này cho thấy rằng phép đệ quy cuối cùng sẽ đạt đến các phần tử đơn lẻ, luôn là các mảng được sắp xếp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi cấp độ đệ quy kiểm tra các phân đoạn có tổng chiều dài là O(n) và có các cấp độ O(log n) | 
| Không gian | O(log n) | Độ sâu đệ quy tối đa bằng số lần chia đôi | 

Với`n ≤ 16`, những chi phí này rất nhỏ. Chương trình về cơ bản chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    def dfs(arr):
        ok = True
        for i in range(len(arr) - 1):
            if arr[i] > arr[i + 1]:
                ok = False
                break

        if ok:
            return len(arr)

        m = len(arr) // 2
        return max(dfs(arr[:m]), dfs(arr[m:]))

    print(dfs(a))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_input = globals().get("input")

    sys.stdin = io.StringIO(inp)
    globals()["input"] = sys.stdin.readline

    out = io.StringIO()
    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    globals()["input"] = backup_input

    return out.getvalue().strip()

# provided sample
assert run("4\n1 2 2 4\n") == "4", "sample"

# minimum size
assert run("1\n42\n") == "1", "single element"

# completely decreasing
assert run("4\n4 3 2 1\n") == "1", "must end at length 1"

# sorted left half only
assert run("8\n1 2 3 4 8 7 6 5\n") == "4", "left half survives"

# all equal values
assert run("8\n5 5 5 5 5 5 5 5\n") == "8", "non-decreasing includes equality"

# sorted right half only
assert run("8\n8 7 6 5 1 2 3 4\n") == "4", "right half survives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`| 1 | Kích thước mảng tối thiểu | 
|`4 / 4 3 2 1`| 1 | Cuối cùng đạt đến các yếu tố đơn lẻ | 
|`8 / 1 2 3 4 8 7 6 5`| 4 | Câu trả lời hay nhất đến từ nửa bên trái | 
|`8 / 5 5 5 5 5 5 5 5`| 8 | Cho phép bình đẳng theo thứ tự sắp xếp | 
|`8 / 8 7 6 5 1 2 3 4`| 4 | Câu trả lời hay nhất đến từ nửa bên phải | 

## Vỏ cạnh 

### Toàn bộ mảng đã được sắp xếp 

đầu vào:```
4
1 2 2 4
```Kiểm tra được sắp xếp đầu tiên thành công. Thuật toán trả về 4 ngay lập tức và không bao giờ khám phá các phân đoạn nhỏ hơn. 

Đầu ra:```
4
```Điều này ngăn cản việc mất câu trả lời tối ưu bằng cách tiếp tục phân chia một cách không cần thiết. 

### Không thể lấy được mảng con được sắp xếp dài nhất 

đầu vào:```
8
100 1 2 3 4 5 6 7
```Tồn tại một mảng con được sắp xếp có độ dài 7, nhưng cách sắp xếp của Thanos không thể giữ các phạm vi tùy ý. 

Đệ quy chỉ kiểm tra các phân đoạn có thể truy cập được thông qua việc giảm một nửa lặp đi lặp lại:```
[100 1 2 3 4 5 6 7]
    -> [100 1 2 3]
    -> [4 5 6 7]
```Nửa bên phải được sắp xếp và có độ dài 4. 

Đầu ra:```
4
```Điều này xác nhận rằng thuật toán tôn trọng cấu trúc của thao tác thay vì tìm kiếm các mảng con được sắp xếp tùy ý. 

### Mảng giảm hoàn toàn 

đầu vào:```
4
4 3 2 1
```Toàn bộ mảng không được sắp xếp. Không một nửa nào được sắp xếp. Việc phân chia tiếp tục cho đến khi đạt được các đoạn có độ dài 1. 

Mỗi đoạn có độ dài 1 đều được sắp xếp, vì vậy câu trả lời tốt nhất có thể đạt được là 1. 

Đầu ra:```
1
```Điều này chứng tỏ rằng phép đệ quy luôn kết thúc với một câu trả lời hợp lệ, ngay cả khi không tồn tại phân đoạn được sắp xếp lớn hơn.
