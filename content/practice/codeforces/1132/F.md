---
title: "CF 1132F - Xóa chuỗi"
description: "Chúng ta có một chuỗi các chữ cái viết thường. Trong một thao tác, chúng ta có thể chọn bất kỳ khối liền kề nào có các ký tự giống nhau và loại bỏ nó. Sau khi gỡ bỏ, các phần còn lại của chuỗi nối lại với nhau."
date: "2026-06-12T04:10:22+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 2000
weight: 1132
solve_time_s: 102
verified: true
draft: false
---

[CF 1132F - Xóa chuỗi](https://codeforces.com/problemset/problem/1132/F) 

**Xếp hạng:** 2000 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các chữ cái viết thường. Trong một thao tác, chúng ta có thể chọn bất kỳ khối liền kề nào có các ký tự giống nhau và loại bỏ nó. Sau khi gỡ bỏ, các phần còn lại của chuỗi nối lại với nhau. 

Mục tiêu là xóa toàn bộ chuỗi bằng cách sử dụng càng ít thao tác càng tốt. 

Khó khăn chính là việc xóa sẽ làm thay đổi cấu trúc của chuỗi. Các ký tự ban đầu được phân tách có thể trở thành liền kề sau khi loại bỏ các ký tự trung gian. Điều này có nghĩa là một quyết định có vẻ chưa tối ưu tại địa phương có thể tạo ra cơ hội tốt hơn sau này. 

Ví dụ, hãy xem xét:```
abaca
```Nếu chúng ta xóa từng ký tự một cách độc lập, chúng ta cần năm thao tác. Một chiến lược tốt hơn là:```
abaca
delete b  -> aaca
delete both a blocks together after merging -> c
delete c
```Việc này chỉ mất ba thao tác. 

Độ dài của chuỗi tối đa là 500. Điều đó ngay lập tức loại trừ mọi cách tiếp cận khám phá rõ ràng tất cả các chuỗi xóa. Số lượng lệnh xóa có thể tăng theo cấp số nhân. Với$n=500$, thậm chí$O(n^4)$đã có khoảng 62,5 tỷ hoạt động và quá chậm. Chúng ta cần một giải pháp lập trình động xung quanh$O(n^3)$. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3
aaa
```Câu trả lời là:```
1
```Tất cả các ký tự có thể bị xóa cùng một lúc. Một DP ngây thơ luôn chia các khoảng thời gian có thể tạo ra 3 không chính xác. 

Hãy xem xét:```
3
aba
```Câu trả lời là:```
2
```Xóa phần giữa`b`, thì cả hai`a`các ký tự trở nên liền kề và có thể được loại bỏ cùng nhau. Bất kỳ cách tiếp cận nào chỉ nhìn vào các ký tự bằng nhau hiện liền kề đều bỏ lỡ khả năng này. 

Coi như:```
4
abab
```Câu trả lời là:```
3
```Các chữ cái giống nhau không thể hợp nhất thành một lần xóa duy nhất. Chúng ta phải tính toán cẩn thận những ký tự nào có thể được kết hợp sau khi xóa trung gian. 

Những ví dụ này cho thấy vấn đề cơ bản là về các khoảng thời gian và sự hợp nhất trong tương lai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử đệ quy mọi thao tác xóa hợp lệ. Đối với mỗi trạng thái, chúng tôi chọn một trong các chuỗi con thống nhất hiện có thể tháo rời, xóa nó và tiếp tục. 

Điều này đúng vì nó khám phá mọi chuỗi hoạt động có thể có. Thật không may, số lượng chuỗi kết quả tăng lên một cách bùng nổ. Ngay cả với độ dài vừa phải, không gian trạng thái vẫn trở nên khổng lồ. Trong trường hợp xấu nhất, thời gian chạy là theo cấp số nhân. 

Cấu trúc của vấn đề gợi ý điều gì đó khác biệt. Hiệu quả của việc xóa được giới hạn trong khoảng thời gian. Bất cứ khi nào chúng ta tập trung vào một chuỗi con$s[l..r]$, cách tối ưu để xóa nó chỉ phụ thuộc vào khoảng thời gian đó. 

Điều này tự nhiên dẫn đến khoảng DP. 

Cho phép$dp[l][r]$biểu thị số thao tác tối thiểu cần thiết để xóa chuỗi con$s[l..r]$. 

Giả sử chúng ta muốn loại bỏ ký tự$s[l]$. Một tùy chọn là xóa nó một cách riêng biệt và sau đó xóa phần còn lại:$$dp[l][r] = 1 + dp[l+1][r]$$Quan sát thú vị xuất hiện khi một sự xuất hiện khác có cùng ký tự tồn tại trong khoảng thời gian. 

Nếu như$s[l] = s[k]$, thì sau khi xóa mọi thứ giữa chúng, hai ký tự bằng nhau này có thể tham gia vào cùng một thao tác xóa. Chúng ta không cần phải trả thêm một thao tác nào cho nhân vật ở vị trí$l$. 

Điều này mang lại sự chuyển đổi:$$dp[l][r] = \min(dp[l][r], dp[l+1][k-1] + dp[k][r])$$Thuật ngữ đầu tiên loại bỏ mọi thứ giữa các ký tự phù hợp. Sau đó, vị trí$l$có thể hợp nhất với việc xóa liên quan đến vị trí$k$. 

Quan sát này chuyển đổi tìm kiếm theo cấp số nhân thành khoảng khối DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Khoảng thời gian tối ưu DP | O(n³) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[l][r]`là số thao tác tối thiểu cần thiết để xóa chuỗi con khỏi chỉ mục`l`lập chỉ mục`r`. 
2. Khởi tạo tất cả các khoảng ký tự đơn. 

Một ký tự đơn luôn có thể bị xóa trong một thao tác, vì vậy:```
dp[i][i] = 1
```3. Khoảng thời gian xử lý theo thứ tự độ dài tăng dần. 

Các khoảng nhỏ hơn phải được giải trước các khoảng lớn hơn vì các trạng thái lớn hơn phụ thuộc vào chúng. 
4. Đối với mỗi khoảng thời gian`[l, r]`, hãy bắt đầu với chiến lược xóa`s[l]`riêng.```
dp[l][r] = 1 + dp[l+1][r]
```Điều này luôn luôn hợp lệ. 
5. Tìm kiếm mọi vị trí`k`với`l < k ≤ r`như vậy`s[l] == s[k]`. 

Các ký tự bằng nhau có thể được loại bỏ cùng nhau sau khi phần giữa biến mất. 
6. Đối với mỗi trường hợp như vậy`k`, hãy thử hợp nhất việc xóa`s[l]`với việc xóa có chứa`s[k]`.```
cost = dp[l+1][k-1] + dp[k][r]
```Thuật ngữ đầu tiên loại bỏ mọi thứ giữa hai ký tự bằng nhau. Thuật ngữ thứ hai xóa hậu tố trong khi cho phép ký tự tại`l`để tham gia thao tác loại bỏ ký tự tại`k`. 
7. Giữ giá trị tối thiểu trong số tất cả các khả năng. 
8. Câu trả lời cuối cùng là`dp[0][n-1]`. 

### Tại sao nó hoạt động 

DP xem xét mọi số phận có thể xảy ra của nhân vật ngoài cùng bên trái trong một khoảng thời gian. 

Hoặc ký tự đó sẽ bị xóa một mình, được biểu thị bằng`1 + dp[l+1][r]`, hoặc cuối cùng nó sẽ bị xóa cùng với một ký tự phù hợp ở đâu đó bên phải. Đối với mọi vị trí phù hợp`k`, khoảng cách giữa chúng phải được loại bỏ hoàn toàn trước tiên, điều này gây tốn kém`dp[l+1][k-1]`. Sau đó, nhân vật tại`l`có thể được đưa vào thao tác xóa tương tự như ký tự tại`k`, vì vậy không cần thực hiện thêm thao tác nào. 

Mọi chiến lược tối ưu hợp lệ đều rơi vào đúng một trong những trường hợp này. Vì sự tái phát chiếm mức tối thiểu trên tất cả chúng,`dp[l][r]`bằng mức tối ưu thực sự cho mọi khoảng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    dp = [[0] * n for _ in range(n)]

    for i in range(n):
        dp[i][i] = 1

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1

            dp[l][r] = 1 + dp[l + 1][r]

            for k in range(l + 1, r + 1):
                if s[l] == s[k]:
                    middle = dp[l + 1][k - 1] if l + 1 <= k - 1 else 0
                    dp[l][r] = min(
                        dp[l][r],
                        middle + dp[k][r]
                    )

    print(dp[0][n - 1])

if __name__ == "__main__":
    solve()
```Bảng DP lưu trữ câu trả lời cho mỗi khoảng thời gian. Các khoảng ký tự đơn được khởi tạo thành 1 vì một thao tác sẽ loại bỏ một ký tự. 

Khoảng thời gian được xử lý bằng cách tăng độ dài. Điều này đảm bảo rằng bất cứ khi nào chúng ta tính toán`dp[l][r]`, tất cả các khoảng thời gian nhỏ hơn xuất hiện trong lần lặp lại đều có sẵn. 

Phần tinh tế là quá trình chuyển đổi hợp nhất. Khi`s[l] == s[k]`, chúng tôi không thêm một thao tác bổ sung nào cho vị trí`l`. Nhân vật ở`l`tham gia xóa mà cuối cùng loại bỏ vị trí`k`. Đó chính xác là lý do tại sao phép truy hồi sử dụng:```
middle + dp[k][r]
```thay vì:```
middle + 1 + dp[k][r]
```Một sai lầm dễ mắc phải khác là xử lý khoảng trống giữa`l+1`Và`k-1`. Khi`k = l + 1`, khoảng đó có chi phí bằng không. Việc triển khai kiểm tra rõ ràng trường hợp này. 

Câu trả lời được lưu trữ trong`dp[0][n-1]`, đại diện cho toàn bộ chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
abaca
```Các trạng thái DP có liên quan: 

| Khoảng thời gian | Chuỗi con | Giá trị DP | 
| --- | --- | --- | 
| [0,0] | một | 1 | 
| [1,1] | b | 1 | 
| [2,2] | một | 1 | 
| [3,3] | c | 1 | 
| [4,4] | một | 1 | 
| [0,2] | aba | 2 | 
| [2,4] | aca | 2 | 
| [0,4] | bàn tính | 3 | 

Trong khoảng thời gian đầy đủ`[0,4]`, ký tự đầu tiên và cuối cùng đều là`a`. Sau khi loại bỏ các phần ở giữa một cách thích hợp, những phần này`a`các nhân vật có thể tham gia vào việc xóa tương tự. Điều này làm giảm câu trả lời từ giá trị ban đầu là 5 xuống còn 3. 

### Ví dụ 2 

đầu vào:```
3
aaa
```| Khoảng thời gian | Chuỗi con | Giá trị DP | 
| --- | --- | --- | 
| [0,0] | một | 1 | 
| [1,1] | một | 1 | 
| [2,2] | một | 1 | 
| [0,1] | aa | 1 | 
| [1,2] | aa | 1 | 
| [0,2] | aaa | 1 | 

Mọi ký tự đều có thể hợp nhất thành cùng một thao tác xóa. Việc lặp lại liên tục tìm thấy các ký tự trùng khớp và không bao giờ cần các thao tác bổ sung ngoài lần đầu tiên. 

Ví dụ này xác nhận rằng DP xử lý chính xác các chữ cái dài bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n³) | Khoảng O(n²), mỗi lần quét O(n) vị trí khớp | 
| Không gian | O(n²) | Bảng DP cho tất cả các khoảng | 

Với$n \le 500$, bảng DP chứa 250.000 trạng thái. Số lần chuyển đổi khối là khoảng 125 triệu thao tác đơn giản trong trường hợp xấu nhất, có thể chấp nhận được trong các giải pháp Codeforces được tối ưu hóa cho vấn đề này và vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    n = int(input())
    s = input().strip()

    dp = [[0] * n for _ in range(n)]

    for i in range(n):
        dp[i][i] = 1

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1

            dp[l][r] = 1 + dp[l + 1][r]

            for k in range(l + 1, r + 1):
                if s[l] == s[k]:
                    middle = dp[l + 1][k - 1] if l + 1 <= k - 1 else 0
                    dp[l][r] = min(dp[l][r], middle + dp[k][r])

    return str(dp[0][n - 1])

def run(inp: str) -> str:
    return solve_io(inp)

# provided sample
assert run("5\nabaca\n") == "3", "sample 1"

# custom cases
assert run("1\na\n") == "1", "single character"
assert run("3\naaa\n") == "1", "all equal"
assert run("3\naba\n") == "2", "merge after middle deletion"
assert run("4\nabab\n") == "3", "alternating pattern"
assert run("5\nabcde\n") == "5", "all distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`1`| Đầu vào kích thước tối thiểu | 
|`3 / aaa`|`1`| Hoàn thành việc hợp nhất các chữ cái bằng nhau | 
|`3 / aba`|`2`| Các ký tự trở nên liền kề sau khi xóa | 
|`4 / abab`|`3`| Nhiều lựa chọn hợp nhất cạnh tranh | 
|`5 / abcde`|`5`| Không có sự hợp nhất nào | 

## Vỏ cạnh 

### Tất cả các ký tự đều giống nhau 

đầu vào:```
3
aaa
```Đầu tiên DP đặt tất cả các khoảng ký tự đơn thành 1. Khi tính toán`[0,1]`, nó thông báo cả hai đầu đều`a`, cho phép hợp nhất và giảm câu trả lời xuống 1. Điều tương tự cũng xảy ra với`[0,2]`. 

Câu trả lời cuối cùng:```
1
```Trường hợp này xác minh rằng các chữ cái giống nhau lặp đi lặp lại được hợp nhất thành một thao tác thay vì được tính riêng. 

### Các ký tự bằng nhau được phân tách bằng ký tự khác 

đầu vào:```
3
aba
```Khoảng thời gian`[0,2]`bắt đầu và kết thúc bằng`a`. 

Sự tái phát đánh giá:```
dp[1][1] + dp[2][2]
= 1 + 1
= 2
```Giữa`b`được xóa đầu tiên. Sau đó hai người`a`các ký tự trở nên liền kề và biến mất cùng nhau. 

Câu trả lời cuối cùng:```
2
```Đây chính xác là tình huống đánh bại các cách tiếp cận tham lam chỉ dựa trên sự liền kề hiện tại. 

### Không có ký tự bằng nhau 

đầu vào:```
5
abcde
```Không có quá trình chuyển đổi hợp nhất nào có thể áp dụng được vì mỗi ký tự đều khác nhau. 

DP liên tục sử dụng:```
dp[l][r] = 1 + dp[l+1][r]
```mang lại:```
5
```Mỗi ký tự phải được xóa riêng lẻ. 

### Kiểu xen kẽ 

đầu vào:```
4
abab
```Có những cặp khớp nhau nhưng không thể hợp nhất tất cả chúng thành một thao tác duy nhất. DP khám phá mọi vị trí phù hợp và phát hiện ra rằng giá trị tốt nhất có thể đạt được là:```
3
```Trường hợp này xác nhận rằng việc lặp lại không đánh giá quá cao lợi ích của việc khớp các ký tự và chỉ thực hiện các phép hợp nhất thực sự có thể đạt được sau khi xóa trung gian.
