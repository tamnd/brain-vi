---
title: "CF 102392I - Trò chơi tuyệt đối"
description: "Alice và Bob mỗi người bắt đầu với một mảng (n) số nguyên. Ở mỗi lượt, người chơi sẽ xóa một giá trị khỏi mảng của chính họ, Alice sẽ di chuyển trước. Việc xóa tiếp tục cho đến khi mỗi mảng chứa chính xác một giá trị."
date: "2026-08-10T21:13:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 76
verified: true
draft: false
---

[CF 102392I - Trò chơi tuyệt đối](https://codeforces.com/problemset/problem/102392/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Alice và Bob mỗi người bắt đầu với một mảng (n) số nguyên. Ở mỗi lượt, người chơi sẽ xóa một giá trị khỏi mảng của chính họ, Alice sẽ di chuyển trước. Việc xóa tiếp tục cho đến khi mỗi mảng chứa chính xác một giá trị. Nếu những giá trị còn sót lại đó là (x) và (y), Alice muốn làm cho (|x-y|) càng lớn càng tốt, trong khi Bob muốn làm cho nó càng nhỏ càng tốt. 

Đầu vào cho ra (n), mảng của Alice (a) và mảng của Bob (b). Chúng ta cần giá trị của (|x-y|) khi chơi tối ưu. 

Khó khăn chính là người chơi không xóa khỏi cùng một mảng. Alice không bao giờ có thể xóa một giá trị khỏi (b) và Bob không bao giờ có thể xóa một giá trị khỏi (a). Sự độc lập đó trông có vẻ đơn giản, nhưng trật tự xen kẽ tạo ra sự tương tác về mặt lý thuyết trò chơi: Bob sẽ phản ứng với mọi hành động xóa của Alice. 

Ở đây (n\le 1000), do đó, giải pháp (O(n^2)) đã thực hiện tối đa khoảng một triệu thao tác theo cặp và dễ thực hiện. Việc mô phỏng trò chơi theo cấp số nhân hoặc giai thừa là hoàn toàn không thể thực hiện được. Các giá trị có thể đạt tới (10^9), do đó việc triển khai cũng phải xử lý các khác biệt một cách an toàn, mặc dù số nguyên Python không có vấn đề tràn. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Nếu (n=1) thì không có nước đi nào cả. Ví dụ,```
1
14
42
```có câu trả lời (28). Giải pháp giả định ít nhất một lần xóa sẽ xử lý sai trường hợp này. 

Vấn đề thứ hai xảy ra khi mọi giá trị của (b) nằm ở một phía của giá trị từ (a). Ví dụ,```
2
10 20
1 2
```Đối với (10), giá trị gần nhất trong (b) là (2), cho khoảng cách (8). Với (20), lại là (2), cho khoảng cách (18), nên đáp án là (18). Việc triển khai tìm kiếm nhị phân chỉ kiểm tra phần tử ở vị trí chèn có thể không thành công khi vị trí đó ở cuối. 

Ranh giới đối diện có cùng một vấn đề. Vì```
2
1 20
5 15
```khoảng cách gần nhất là (4) và (5), nên đáp án là (5). Khi tìm kiếm (1), vị trí chèn là phần đầu của mảng đã sắp xếp nên phần trước không tồn tại. 

Các giá trị trùng lặp là một cách kiểm tra hữu ích khác. Với```
4
7 7 7 7
7 7 7 7
```câu trả lời là (0). Việc xử lý các giá trị bằng nhau dưới dạng các lựa chọn trò chơi riêng biệt sẽ không làm thay đổi giá trị và mọi giải pháp dựa trên vị trí đã sắp xếp vẫn phải xử lý các giá trị trùng lặp một cách chính xác. 

## Phương pháp tiếp cận 

Một minimax bạo lực theo nghĩa đen tuân theo mọi trình tự xóa có thể xảy ra. Ở trạng thái có (k) giá trị còn lại trong mảng của người chơi, người chơi đó có (k) khả năng bị xóa. Thứ tự xóa hoàn toàn một mảng là hoán vị của (n) phần tử của nó, bởi vì việc chọn (n-1) phần tử bị xóa đầu tiên sẽ xác định duy nhất phần tử sống sót cuối cùng. Do đó, có thể có (n!) lịch sử xóa đối với Alice và (n!) đối với Bob, đưa ra ((n!)^2) lịch sử thiết bị đầu cuối được ghép nối trong cây trò chơi theo nghĩa đen. Thậm chí (n=10) đã cung cấp lịch sử thiết bị đầu cuối (10!^2\approx1.3\times10^{13}), do đó, minimax trực tiếp không thể sử dụng được từ lâu trước mức tối đa (n=1000). 

Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi cách có thể mà người chơi có thể tác động đến những người sống sót cuối cùng, nhưng nó lãng phí gần như toàn bộ công việc của mình theo thứ tự xóa các giá trị. Lợi ích thực tế chỉ phụ thuộc vào hai giá trị cuối cùng. Quan sát trọng tâm là đối với mỗi giá trị Alice (a_i), chúng ta chỉ cần biết giá trị gần nhất mà Bob có thể để lại so với nó. 

Xác định 

[ 
d_i=\min_j |a_i-b_j|. 
] 

Giả sử Alice muốn (a_i) là giá trị cuối cùng. Cô ấy có thể chỉ cần giữ (a_i) và xóa mọi giá trị khác khỏi mảng của mình. Sau đó, Bob muốn để lại giá trị (b) càng gần (a_i) càng tốt. Khoảng cách liên quan chính xác là (d_i). 

Câu hỏi còn lại là liệu lệnh xóa xen kẽ có cho Bob đủ quyền kiểm soát để đạt được mức tối thiểu đó hay không. Nó có. Sửa, với mỗi (a_i), một giá trị Bob (b_{f(i)}) đạt được (d_i). Sau khi Alice xóa một giá trị, giả sử (k) giá trị Alice vẫn còn. Bob có giá trị (k+1) tại thời điểm đó. Hiện tại cần nhiều nhất (k) giá trị của Bob làm nhân chứng (b_{f(i)}) cho các giá trị Alice còn lại. Do đó, bất kỳ giá trị Alice còn lại nào cũng không cần ít nhất một giá trị Bob và Bob có thể xóa giá trị đó một cách an toàn. 

Việc lặp lại chiến lược này cho phép Bob bảo toàn được nhân chứng phù hợp cho bất kỳ giá trị nào của Alice còn tồn tại. Do đó, Bob luôn có thể đảm bảo khoảng cách cuối cùng tối đa là (d_i) cho người sống sót (a_i). Mặt khác, Alice có thể chọn (a_i) có (d_i) lớn nhất và giữ cho nó tồn tại. Do đó, giá trị của trò chơi là 

[ 
\boxed{\max_i \min_j |a_i-b_j|}. 
] 

Điều này làm cho trò chơi trở thành vấn đề với người hàng xóm gần nhất. Chúng ta có thể tính toán nó trực tiếp trong (O(n^2)), như vậy là đủ cho (n\le1000). Chúng ta cũng có thể sắp xếp (b), sau đó tìm giá trị gần nhất với mọi (a_i) bằng tìm kiếm nhị phân, giảm độ phức tạp xuống (O(n\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n!)^2)) lịch sử thiết bị đầu cuối | Trạng thái đệ quy theo cấp số nhân | Quá chậm | 
| Giảm theo cặp | (O(n^2)) | (O(n)) | Đã chấp nhận | 
| Sắp xếp hàng xóm gần nhất | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

Việc triển khai bên dưới sử dụng phiên bản được sắp xếp (O(n\log n)). Việc giảm thiểu lý thuyết trò chơi tương tự cũng có thể được thực hiện bằng cách quét theo cặp (O(n^2)) đơn giản hơn. 

## Hướng dẫn thuật toán 

1. Đọc mảng của Alice (a) và mảng của Bob (b), sau đó sắp xếp (b) theo thứ tự tăng dần. Việc sắp xếp cho phép chúng ta tìm giá trị Bob gần nhất với bất kỳ giá trị Alice nào mà không cần kiểm tra từng cặp. 
2. Với mỗi giá trị Alice (x), hãy sử dụng tìm kiếm nhị phân để tìm giá trị đầu tiên trong (b) ít nhất là (x). Gọi vị trí của nó`pos`. 
3. Nếu`pos`nằm trong mảng, so sánh (x) với`b[pos]`. Đây là giá trị Bob nhỏ nhất không nằm dưới (x), vì vậy trong số tất cả các giá trị ở phía đó, nó là ứng cử viên gần nhất có thể có. 
4. Nếu`pos > 0`, đồng thời so sánh (x) với`b[pos - 1]`. Đây là giá trị Bob lớn nhất bên dưới (x), vì vậy nó là ứng cử viên gần nhất có thể có ở phía bên kia. 
5. Lấy khoảng cách nhỏ hơn trong số những khoảng cách có sẵn này. Giá trị đó là (\min_j |x-b_j|), khoảng cách tốt nhất mà Bob có thể tạo ra nếu Alice rời đi (x). 
6. Duy trì mức tối đa của các khoảng cách gần nhất này trên tất cả (x\in a). Alice có thể chọn (x) tương ứng làm người sống sót của mình, vì vậy giá trị tối đa này là giá trị trò chơi. 
7. In khoảng cách tối đa. 

### Tại sao nó hoạt động 

hãy để 

[ 
T=\max_{x\in A}\min_{y\in B}|x-y|. 
] 

Alice có thể chọn một (x) đạt mức tối đa này và không bao giờ xóa nó. Giá trị cuối cùng của cô ấy khi đó là (x), bất kể Bob có xóa đi hay không. 

Đối với Bob, chọn cho mỗi giá trị Alice (x) một giá trị Bob (f(x)) thỏa mãn (|x-f(x)|\le T). Bất cứ khi nào Alice vừa xóa và các giá trị (k) vẫn còn trong mảng của cô ấy thì Bob còn lại các giá trị (k+1). Nhân chứng (f(x)) cho (k) giá trị Alice còn lại đó sử dụng tối đa (k) giá trị Bob riêng biệt, do đó ít nhất một giá trị Bob không phải là nhân chứng cho bất kỳ giá trị Alice còn lại nào. Bob xóa một giá trị không cần thiết như vậy. Điều này bảo tồn một nhân chứng hợp lệ cho mọi người sống sót có thể có của Alice trong tương lai. 

Cuối cùng, nếu Alice bỏ đi (x) thì Bob vẫn còn một nhân chứng (f(x)) với khoảng cách nhiều nhất là (T). Do đó Bob đảm bảo kết quả không lớn hơn (T), trong khi Alice đảm bảo kết quả không nhỏ hơn (T). Giá trị trò chơi chính xác là (T). 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    b.sort()

    answer = 0

    for x in a:
        pos = bisect_left(b, x)
        best = 10**30

        if pos < n:
            best = min(best, abs(x - b[pos]))

        if pos > 0:
            best = min(best, abs(x - b[pos - 1]))

        answer = max(answer, best)

    print(answer)

if __name__ == "__main__":
    solve()
```Dữ liệu đầu vào được đọc chính xác một lần cho mỗi mảng, sau đó mảng của Bob được sắp xếp. các`bisect_left`cuộc gọi tìm vị trí đầu tiên có giá trị ít nhất là giá trị Alice hiện tại. 

Chỉ có hai ứng cử viên cần được kiểm tra. Nếu như`pos < n`,`b[pos]`là ứng cử viên gần nhất bên phải. Nếu như`pos > 0`,`b[pos - 1]`là ứng cử viên gần nhất ở bên trái. Không có phần tử nào khác có thể gần hơn vì mảng đã được sắp xếp. 

Hai kiểm tra ranh giới là độc lập có chủ ý. Khi`pos == 0`, không có tiền thân. Khi`pos == n`, không có phần tử nào ở bên phải. Xử lý cả hai trường hợp một cách rõ ràng sẽ tránh lập chỉ mục bên ngoài mảng.`best`bắt đầu ở một giá trị lớn hơn nhiều so với bất kỳ câu trả lời nào có thể có. Vì tất cả các giá trị đầu vào nhiều nhất là (10^9), nên mọi chênh lệch tuyệt đối nhiều nhất là (10^9-1). Số nguyên Python cũng làm cho lỗi tràn không liên quan. 

Cuối cùng,`answer`lấy khoảng cách lân cận gần nhất tối đa, khớp với biểu thức (\max_i\min_j) đã được chứng minh ở trên. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
4
2 14 7 14
5 10 9 22
```Mảng của Bob trở thành`[5, 9, 10, 22]`. 

| Giá trị Alice (x) |`pos`| Ứng viên phù hợp | Ứng cử viên trái | Khoảng cách gần nhất |`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 2 | 0 | 5 | không | 3 | 3 | 
| 14 | 3 | 22 | 10 | 4 | 4 | 
| 7 | 1 | 9 | 5 | 2 | 4 | 
| 14 | 3 | 22 | 10 | 4 | 4 | 

Khoảng cách gần nhất tối đa là (4). Alice có thể giữ (14), trong khi Bob có thể bảo toàn (10), đưa ra hiệu số cuối cùng (4). Các giá trị Alice khác cho phép Bob đến gần hơn, vì vậy Alice thích (14). 

Đối với mẫu thứ hai,```
1
14
42
```không có thao tác xóa nào vì cả hai mảng đều đã chứa một giá trị. 

| Giá trị Alice (x) |`pos`| Ứng viên phù hợp | Ứng cử viên trái | Khoảng cách gần nhất |`answer`| 
| --- | --- | --- | --- | --- | --- | 
| 14 | 0 | 42 | không | 28 | 28 | 

Câu trả lời là (28), khớp trực tiếp với cặp cuối cùng duy nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Việc sắp xếp (b) chi phí (O(n\log n)), sau đó (n) tìm kiếm nhị phân có chi phí khác (O(n\log n)) | 
| Không gian | (O(n)) | Các mảng và bản sao được sắp xếp của (b) yêu cầu không gian tuyến tính | 

Với (n\le1000), ngay cả việc triển khai (O(n^2)) đơn giản hơn cũng sẽ chỉ thực hiện kiểm tra khoảng cách khoảng (10^6). Giải pháp (O(n\log n)) được trình bày thoải mái trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    a = [next(it) for _ in range(n)]
    b = [next(it) for _ in range(n)]

    b.sort()

    answer = 0

    for x in a:
        pos = bisect_left(b, x)
        best = 10**30

        if pos < n:
            best = min(best, abs(x - b[pos]))

        if pos > 0:
            best = min(best, abs(x - b[pos - 1]))

        answer = max(answer, best)

    return str(answer)

def run(inp: str) -> str:
    return solve_data(inp)

# Provided sample 1
assert run("""4
2 14 7 14
5 10 9 22
""") == "4", "sample 1"

# Provided sample 2
assert run("""1
14
42
""") == "28", "sample 2"

# Minimum size, no moves
assert run("""1
5
5
""") == "0", "minimum size"

# All values equal
assert run("""4
7 7 7 7
7 7 7 7
""") == "0", "all equal values"

# Both lower and upper binary-search boundaries
assert run("""2
1 20
5 15
""") == "5", "boundary positions"

# All Bob values below Alice values
assert run("""2
10 20
1 2
""") == "18", "lower boundary"

# Maximum n and maximum value difference
n = 1000
a = " ".join(["1000000000"] * n)
b = " ".join(["1"] * n)
max_case = f"{n}\n{a}\n{b}\n"
assert run(max_case) == "999999999", "maximum size and values"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 / 5`|`0`| Kích thước tối thiểu và chấm dứt ngay lập tức | 
|`4 / 7 7 7 7 / 7 7 7 7`|`0`| Giá trị trùng lặp và bằng nhau | 
|`2 / 1 20 / 5 15`|`5`|`bisect_left`ở đầu và cuối | 
|`2 / 10 20 / 1 2`|`18`| Không có ứng cử viên tìm kiếm nhị phân bên phải | 
| (n=1000), tất cả (a_i=10^9), tất cả (b_i=1) |`999999999`| Phạm vi giá trị và kích thước đầu vào tối đa | 

## Vỏ cạnh 

Trường hợp (n=1) không yêu cầu mô phỏng trò chơi. Vì```
1
14
42
```các giá trị duy nhất còn sót lại đã là (14) và (42).`bisect_left([42], 14)`trả lại`0`, vì vậy chỉ có ứng viên phù hợp mới được chọn và khoảng cách gần nhất là (28). Giá trị tối đa trên một giá trị Alice cũng là (28). 

Khi vị trí chèn ở đầu thì vị trí trước đó không tồn tại. Vì```
2
1 20
5 15
```việc tìm kiếm (1) trả về vị trí`0`, cho khoảng cách (4) với`5`. Việc tìm kiếm (20) trả về vị trí`2`, vì vậy không có ứng cử viên phù hợp và chỉ`15`được kiểm tra, cho khoảng cách (5). Câu trả lời cuối cùng là (5). Hai giá trị này thực hiện cả hai đầu của mảng đã sắp xếp. 

Khi mọi giá trị Bob nhỏ hơn giá trị Alice thì vị trí chèn luôn là`n`. Vì```
2
10 20
1 2
```giá trị Bob gần nhất với (10) là (2), với khoảng cách (8), trong khi giá trị gần nhất với (20) cũng là (2), với khoảng cách (18). Câu trả lời là (18). các`pos < n`kiểm tra ngăn chặn truy cập không hợp lệ vào`b[n]`. 

Các giá trị trùng lặp không tạo thêm sức mạnh chiến lược. Với```
4
7 7 7 7
7 7 7 7
```mọi giá trị Alice đều có giá trị Bob ở khoảng cách (0). Khoảng cách gần nhất được tính là (0) cho mỗi lần lặp, vì vậy câu trả lời là (0). Điều này cũng chứng tỏ tại sao đối số lại là về các giá trị và khoảng cách gần nhất của chúng chứ không phải về các chỉ số duy nhất. 

Trường hợp giá trị tối đa cũng an toàn. Nếu Alice chỉ chứa (10^9) và Bob chỉ chứa (1), thì chênh lệch thu được là (999999999). Thuật toán tính toán trực tiếp mà không bị tràn và kết quả vẫn nằm trong phạm vi số nguyên của Python.
