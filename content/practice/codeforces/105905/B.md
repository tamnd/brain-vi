---
title: "CF 105905B - ​​\u0423\u0434\u0430\u043b\u0438 \u0441\u0438\u043c\u0432\u043e\u043b\u044b"
description: "Chúng tôi được cung cấp một chuỗi chữ thường. Trong một thao tác, chúng ta có thể loại bỏ hai ký tự là hình ảnh phản chiếu đối với phần cuối của chuỗi hiện tại. Nói cách khác, nếu một ký tự là ký tự thứ k tính từ bên trái thì ký tự kia phải là ký tự thứ k tính từ bên phải."
date: "2026-06-25T14:14:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105905
codeforces_index: "B"
codeforces_contest_name: "Ural championship 2025"
rating: 0
weight: 105905
solve_time_s: 83
verified: true
draft: false
---

[CF 105905B - \u0423\u0434\u0430\u043b\u0438 \u0441\u0438\u043c\u0432\u043e\u043b\u044b](https://codeforces.com/problemset/problem/105905/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chữ thường. Trong một thao tác, chúng ta có thể loại bỏ hai ký tự là hình ảnh phản chiếu đối với phần cuối của chuỗi hiện tại. Nói cách khác, nếu một ký tự là ký tự thứ k tính từ bên trái thì ký tự kia phải là ký tự thứ k tính từ bên phải. Hoạt động chỉ được phép khi hai ký tự đó bằng nhau. 

Sau khi tháo cặp, các phần còn lại của sợi dây được dán lại với nhau. Chúng tôi có thể thực hiện bất kỳ số lượng thao tác nào và muốn độ dài cuối cùng tối thiểu có thể. 

Độ dài chuỗi có thể lớn bằng$10^5$, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào liên tục xây dựng lại các chuỗi hoặc khám phá các lệnh hoạt động khác nhau. MỘT$O(n^2)$giải pháp đã quá chậm, vì vậy chúng ta nên hướng tới một lần quét tuyến tính duy nhất. 

Một điểm tinh tế là việc xóa một cặp được nhân đôi sẽ thay đổi chuỗi hiện tại, do đó thật hấp dẫn khi nghĩ rằng các cơ hội xóa mới có thể xuất hiện sau đó. Việc triển khai bất cẩn có thể cố gắng mô phỏng các hoạt động một cách linh hoạt và dẫn đến sự phức tạp không cần thiết. 

Hãy xem xét chuỗi:```
abba
```Các cặp phản chiếu là`(a,a)`Và`(b,b)`. Cả hai cặp đều có thể được loại bỏ, vì vậy câu trả lời là:```
0
```Bây giờ hãy xem xét:```
abca
```Cặp bên ngoài`(a,a)`có thể được gỡ bỏ, nhưng`(b,c)`không thể. Độ dài tối thiểu là:```
2
```Một ví dụ quan trọng khác là:```
abcde
```Mỗi cặp được phản chiếu đều chứa các chữ cái khác nhau nên không thể thực hiện được thao tác nào và câu trả lời là:```
5
```Quan sát quan trọng là các hoạt động không bao giờ thay đổi vị trí được nhân đôi ban đầu tương ứng với nhau. Điều đó hoàn toàn quyết định giải pháp. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liên tục tìm kiếm các cặp được nhân đôi có thể tháo rời, xóa một trong số chúng, xây dựng lại chuỗi và tiếp tục. Vì mỗi lần xóa đều thay đổi chỉ mục nên chúng ta cần duy trì chuỗi hiện tại một cách rõ ràng. Trong trường hợp xấu nhất, việc xây dựng lại chuỗi sau mỗi thao tác sẽ dẫn đến hành vi bậc hai. 

Bước đột phá đến từ việc xem dây như một tập hợp các cặp phản chiếu. 

Đối với mọi vị trí`i`ở nửa bên trái có đúng một vị trí phản chiếu`n - 1 - i`ở nửa bên phải. Chúng ta hãy gọi đây là một cặp được nhân đôi. 

Giả sử một cặp được nhân đôi chứa các ký tự bằng nhau. Sau đó, cặp đó cuối cùng có thể bị xóa. 

Giả sử một cặp được nhân đôi chứa các ký tự khác nhau. Không có hoạt động nào có thể xóa được một trong hai. Lý do là mọi thao tác đều loại bỏ một cặp được nhân đôi hoàn chỉnh. Việc loại bỏ một số cặp phản chiếu khác không làm thay đổi thực tế là hai ký tự này vẫn được ghép nối với nhau. 

Điều này có nghĩa là mỗi cặp được nhân đôi hoàn toàn độc lập: 

Nếu hai ký tự bằng nhau thì cả hai đều biến mất. 

Nếu hai nhân vật khác nhau thì cả hai sẽ tồn tại mãi mãi. 

Đối với các chuỗi có độ dài lẻ, ký tự ở giữa không có đối tác và không bao giờ có thể bị xóa. 

Vì vậy, câu trả lời chỉ đơn giản là:```
2 × (number of mismatched mirrored pairs)
+ (1 if the length is odd else 0)
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Đếm số điểm không khớp được phản ánh | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và đặt độ dài của nó là`n`. 
2. Khởi tạo bộ đếm`bad = 0`. 
3. Đối với mọi chỉ số`i`từ`0`ĐẾN`n // 2 - 1`, so sánh:```
s[i]
s[n - 1 - i]
```4. Nếu các ký tự khác nhau, hãy tăng dần`bad`. 

Mỗi cặp được nhân đôi như vậy không bao giờ có thể bị xóa, vì vậy cả hai ký tự phải còn lại trong chuỗi cuối cùng. 
5. Sau khi xử lý tất cả các cặp đối xứng, hãy tính:```
answer = 2 * bad + (n % 2)
```Thuật ngữ`2 * bad`đếm tất cả các ký tự thuộc các cặp được nhân đôi không khớp. 

Thuật ngữ`n % 2`chiếm ký tự trung tâm khi độ dài là số lẻ. 
6. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Mọi thao tác sẽ loại bỏ chính xác một cặp được nhân đôi có các ký tự bằng nhau. 

Việc xóa một cặp được phản chiếu không làm thay đổi mối quan hệ ghép đôi của các cặp được phản chiếu còn lại. Một cặp bắt đầu như`(x, y)`vẫn được ghép nối với nhau cho đến cuối cùng. 

Do đó, mỗi cặp phản chiếu có thể được phân tích độc lập. 

Các cặp nhân đôi bằng nhau có thể được loại bỏ hoàn toàn. 

Các cặp được nhân đôi không bằng nhau không bao giờ có thể bị loại bỏ. 

Ký tự ở giữa của chuỗi có độ dài lẻ không bao giờ có đối tác, vì vậy nó luôn tồn tại. 

Thuật toán đếm chính xác các ký tự còn lại sau tất cả các lần xóa có thể, đó là độ dài tối thiểu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

bad = 0

for i in range(n // 2):
    if s[i] != s[n - 1 - i]:
        bad += 1

print(2 * bad + (n % 2))
```Việc thực hiện tuân theo quan sát toán học trực tiếp. 

Vòng lặp chỉ thăm nửa bên trái của chuỗi. Đối với mỗi vị trí, nó so sánh nhân vật với đối tác được phản chiếu ở phía bên phải. 

Mỗi sự không khớp đều đóng góp chính xác hai ký tự không thể tránh khỏi cho câu trả lời. Mỗi cặp phù hợp đều đóng góp 0 vì cả hai ký tự đều có thể bị xóa. 

biểu hiện`n % 2`tự động xử lý ký tự trung tâm. Đối với độ dài chẵn, nó đóng góp bằng 0, trong khi đối với độ dài lẻ, nó đóng góp một. 

Không có vấn đề gì xảy ra vì vòng lặp chạy chính xác trên các cặp được phản chiếu và không bao giờ chạm vào vị trí chính giữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abba
```| tôi | Nhân vật trái | Đúng Nhân Vật | Số lượng không khớp | 
| --- | --- | --- | --- | 
| 0 | một | một | 0 | 
| 1 | b | b | 0 | 

Câu trả lời cuối cùng:```
2 * 0 + 0 = 0
```Đầu ra:```
0
```Ví dụ này cho thấy mọi cặp đối xứng đều khớp nhau, do đó toàn bộ chuỗi có thể biến mất. 

### Ví dụ 2 

đầu vào:```
aabaa
```| tôi | Nhân vật trái | Đúng Nhân Vật | Số lượng không khớp | 
| --- | --- | --- | --- | 
| 0 | một | một | 0 | 
| 1 | một | một | 0 | 

Độ dài là số lẻ nên nhân vật trung tâm vẫn tồn tại. 

Câu trả lời cuối cùng:```
2 * 0 + 1 = 1
```Đầu ra:```
1
```Điều này thể hiện vai trò của nhân vật trung gian. Ngay cả khi mọi cặp gương biến mất, phần trung tâm vẫn còn. 

### Ví dụ 3 

đầu vào:```
abcde
```| tôi | Nhân vật trái | Đúng Nhân Vật | Số lượng không khớp | 
| --- | --- | --- | --- | 
| 0 | một | e | 1 | 
| 1 | b | d | 2 | 

Câu trả lời cuối cùng:```
2 * 2 + 1 = 5
```Đầu ra:```
5
```Không có cặp phản chiếu nào khớp nên không thể xóa được gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua một nửa chuỗi | 
| Không gian | O(1) | Chỉ có một vài quầy được sử dụng | 

Với$n \le 10^5$, quét tuyến tính dễ dàng đủ nhanh trong giới hạn thời gian và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()
    n = len(s)

    bad = 0
    for i in range(n // 2):
        if s[i] != s[n - 1 - i]:
            bad += 1

    print(2 * bad + (n % 2))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided samples
assert run("abba\n") == "0\n", "sample 1"
assert run("abcde\n") == "5\n", "sample 2"
assert run("aabaa\n") == "1\n", "sample 3"

# custom cases
assert run("a\n") == "1\n", "single character"
assert run("aa\n") == "0\n", "smallest removable pair"
assert run("ab\n") == "2\n", "single mismatched pair"
assert run("aaaaaa\n") == "0\n", "all equal characters"
assert run("abccba\n") == "0\n", "full palindrome"
assert run("abcdef\n") == "6\n", "all mirrored pairs mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Trung tâm có chiều dài lẻ tồn tại | 
|`aa`|`0`| Cặp gương đơn có thể tháo rời | 
|`ab`|`2`| Cặp gương đơn không khớp | 
|`aaaaaa`|`0`| Tất cả các cặp có thể tháo rời | 
|`abccba`|`0`| Bảng màu chung | 
|`abcdef`|`6`| Không thể xóa | 

## Vỏ cạnh 

Hãy xem xét:```
a
```Không có cặp nào được nhân đôi cả. Thuật toán tìm`bad = 0`và vì độ dài là số lẻ:```
answer = 0 + 1 = 1
```Ký tự duy nhất còn lại, đó là chính xác. 

Coi như:```
ab
```Cặp gương duy nhất là`(a,b)`, khác hẳn. 

Thuật toán tính một điểm không khớp:```
bad = 1
answer = 2
```Không thể xóa ký tự nào, vì vậy kết quả là chính xác. 

Coi như:```
aabaa
```Cả hai cặp được nhân đôi đều khớp và có thể bị xóa. Nhân vật trung tâm không có bạn đồng hành. 

Thuật toán tính toán:```
bad = 0
answer = 1
```phù hợp với trình tự xóa tối ưu. 

Coi như:```
abcde
```Cả hai cặp được nhân đôi đều không khớp và phần trung tâm vẫn tồn tại:```
(a,e) -> mismatch
(b,d) -> mismatch
center = c
```Thuật toán trả về:```
2 * 2 + 1 = 5
```Không thể thực hiện thao tác nào nên toàn bộ chuỗi vẫn còn. Điều này xác nhận tính đúng đắn của cách giải thích cách đếm không khớp.
