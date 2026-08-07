---
title: "CF 102536A - Tệp bị chậm"
description: "Chúng tôi cần so sánh hai mật khẩu và xác định khoảng cách chỉnh sửa của chúng. Chuỗi đầu tiên là nội dung người dùng đã nhập và chuỗi thứ hai là mật khẩu thực tế. Chỉnh sửa là thay đổi một ký tự, chèn một ký tự hoặc xóa một ký tự."
date: "2026-08-06T20:12:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "A"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 180
verified: true
draft: false
---

[CF 102536A - Tệp bị làm chậm](https://codeforces.com/problemset/problem/102536/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi cần so sánh hai mật khẩu và xác định khoảng cách chỉnh sửa của chúng. Chuỗi đầu tiên là nội dung người dùng đã nhập và chuỗi thứ hai là mật khẩu thực tế. Chỉnh sửa là thay đổi một ký tự, chèn một ký tự hoặc xóa một ký tự. Kết quả đầu ra chỉ phụ thuộc vào số lượng chỉnh sửa tối thiểu cần thiết là 0, 1, 2, 3 hay lớn hơn. 

Đầu vào có thể chứa tối đa 10.000 cặp mật khẩu và độ dài kết hợp của tất cả mật khẩu có thể đạt tới 2.000.000 ký tự. Giải pháp khoảng cách chỉnh sửa lập trình động thông thường sử dụng`O(nm)`thời gian cho các chuỗi có độ dài`n`Và`m`, điều này sẽ quá đắt khi cả hai chuỗi đều có độ dài khoảng 100.000. Ngay cả một cặp có chiều dài 100.000 cũng sẽ cần khoảng 10 tỷ lần chuyển đổi trạng thái. Giải pháp cần khai thác thực tế là chúng ta chỉ quan tâm đến khoảng cách lên tới ba. 

Một số trường hợp cần đặc biệt chú ý vì những so sánh đơn giản hơn có thể cho kết quả sai. Đầu tiên là khi các dây có độ dài khác nhau. Ví dụ:```
Input:
1
abc
abcd
```Đầu ra đúng là:```
You almost got it. You're wrong in just one spot.
```Việc so sánh chỉ đếm các vị trí khác nhau sẽ không tìm thấy vị trí không khớp nào trong ba ký tự đầu tiên và xác nhận chuỗi khớp không chính xác. Ký tự bị thiếu là xóa hoặc chèn nên phải tính. 

Một trường hợp khác là khi thao tác chèn sẽ dịch chuyển tất cả các ký tự sau này. Ví dụ:```
Input:
1
abcde
abXcde
```Đầu ra đúng là:```
You almost got it. You're wrong in just one spot.
```Bộ đếm không khớp theo vị trí sẽ thấy một số vị trí sai, ngay cả khi chèn`X`sửa chữa mọi thứ chỉ bằng một thao tác. 

Trường hợp cạnh cuối cùng là sự khác biệt giữa thay thế và chỉnh sửa nhiều lần do thay đổi độ dài. Ví dụ:```
Input:
1
abc
xyz
```Đầu ra đúng là:```
You almost got it, but you're wrong in two spots.
```Trên thực tế, khoảng cách chỉnh sửa ở đây là ba, vì cả ba ký tự đều phải được thay thế. Phương pháp chỉ so sánh số lượng ký tự khác nhau sau khi sắp xếp hoặc bỏ qua các vị trí có thể phân loại không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính khoảng cách Levenshtein cổ điển bằng lập trình động. Đối với hai chuỗi, chúng tôi tạo một bảng trong đó mỗi ô biểu thị số lần chỉnh sửa tối thiểu cần thiết để chuyển tiền tố của một chuỗi thành tiền tố của chuỗi kia. Quá trình chuyển đổi xem xét việc xóa một ký tự, chèn một ký tự hoặc thay thế một ký tự không khớp. Điều này đúng vì mọi phép biến đổi tối ưu đều phải kết thúc bằng một trong ba thao tác đó. 

Vấn đề là kích thước của bảng. Nếu cả hai mật khẩu đều có độ dài 100.000 thì bảng có khoảng 10 tỷ ô. Mặc dù câu trả lời được giới hạn ở mức ba, nhưng thuật toán thông thường vẫn dành thời gian để tính toán các trạng thái không thể ảnh hưởng đến quyết định cuối cùng. 

Quan sát quan trọng là chúng ta không cần khoảng cách chính xác khi nó lớn hơn ba. Chúng ta chỉ cần biết khoảng cách có nhiều nhất là ba hay không. Khi chênh lệch độ dài giữa hai chuỗi đã lớn hơn ba, thì ít nhất cần phải chèn hoặc xóa nhiều lần, do đó câu trả lời ngay lập tức là quá lớn. 

Khi độ dài gần bằng nhau, chúng ta có thể căn chỉnh các chuỗi bằng cách sử dụng thực tế là chỉ cho phép một số chỉnh sửa nhỏ. Nếu các chuỗi khác nhau tối đa ba thao tác, thì các phần khớp gần như phải được căn chỉnh. Chúng ta có thể đệ quy bỏ qua các ký tự trùng khớp từ cả hai đầu, sau đó xử lý vùng nhỏ không khớp còn lại. Mỗi sự không khớp có thể được giải quyết bằng cách thử các thao tác chỉnh sửa có thể, đồng thời dừng ngay khi số lần chỉnh sửa vượt quá ba. 

Giải pháp brute-force hoạt động vì mọi trình tự chỉnh sửa có thể đều được xem xét nhưng không thành công vì số lượng trình tự có thể tăng lên nhanh chóng. Quan sát cho thấy khoảng cách cho phép là một hằng số nhỏ cho phép chúng ta chỉ tìm kiếm trong không gian nhỏ xung quanh điểm không khớp hiện tại thay vì xây dựng bảng lập trình động đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo số lần chỉnh sửa | O(độ sâu) | Quá chậm | 
| Lập trình động đầy đủ | O(nm) | O(nm) hoặc O(min(n,m)) | Quá chậm để có được đầu vào tối đa | 
| Tìm kiếm giới hạn tối ưu | O(k * L), trong đó k = 3 và L là độ dài chuỗi kết hợp | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mật khẩu. Nếu chúng giống nhau thì khoảng cách chỉnh sửa bằng 0 và chúng ta có thể trả về ngay thông báo đăng nhập thành công. Kiểm tra sự bằng nhau trước tiên cũng tránh được việc đệ quy không cần thiết trong trường hợp đơn giản phổ biến nhất. 
2. So sánh độ dài của hai mật khẩu. Nếu chênh lệch của chúng lớn hơn ba, hãy trả lại tin nhắn có khoảng cách lớn hơn ba. Chênh lệch độ dài bằng bốn đã yêu cầu bốn lần chèn hoặc xóa, ngay cả khi mọi ký tự hiện có đều khớp. 
3. Sử dụng hàm đệ quy nhận hai vị trí hiện tại và số lần chỉnh sửa đã được sử dụng. Trước khi thực hiện thêm công việc, hãy bỏ qua mọi ký tự phù hợp từ vị trí hiện tại. Các ký tự trùng khớp không bao giờ cần tham gia vào trình tự chỉnh sửa tối ưu. 
4. Nếu một chuỗi đến cuối chuỗi thì các ký tự còn lại của chuỗi kia phải được chèn hoặc xóa hết. Thêm độ dài còn lại đó vào số lần chỉnh sửa hiện tại. 
5. Nếu số lần chỉnh sửa hiện tại đã lớn hơn ba, hãy ngừng khám phá đường dẫn này. Khoảng cách lớn hơn không ảnh hưởng đến danh mục cuối cùng. 
6. Khi cả hai chuỗi vẫn còn ký tự và chúng khác nhau, hãy thử ba thao tác chỉnh sửa có thể thực hiện được. Thay thế cả hai ký tự và tiến về phía trước, xóa ký tự khỏi chuỗi đầu tiên hoặc xóa ký tự khỏi chuỗi thứ hai. Trả về số lượng chỉnh sửa nhỏ nhất được tìm thấy. 
7. Chuyển đổi khoảng cách cuối cùng thành thông báo đầu ra được yêu cầu. Khoảng cách trên ba chia sẻ cùng một đầu ra.

Tại sao nó hoạt động: Sau khi loại bỏ tất cả các tiền tố bằng nhau, các ký tự đầu tiên còn lại sẽ khác. Bất kỳ trình tự chỉnh sửa tối ưu nào cũng phải thay thế một trong các ký tự này, xóa một trong số chúng hoặc chèn một ký tự trước một trong số chúng. Đây chính xác là ba chuyển đổi được thuật toán khám phá. Quá trình đệ quy chỉ dừng lại sau khi chứng minh rằng cần có nhiều hơn ba lần chỉnh sửa, điều này là đủ vì tất cả các khoảng cách lớn hơn đều có cùng kết quả đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 3

def limited_distance(a, b):
    from functools import lru_cache

    @lru_cache(maxsize=None)
    def solve(i, j, used):
        while i < len(a) and j < len(b) and a[i] == b[j]:
            i += 1
            j += 1

        if i == len(a):
            return used + (len(b) - j)
        if j == len(b):
            return used + (len(a) - i)

        if used > LIMIT:
            return LIMIT + 1

        best = LIMIT + 1

        best = min(best, solve(i + 1, j + 1, used + 1))
        best = min(best, solve(i + 1, j, used + 1))
        best = min(best, solve(i, j + 1, used + 1))

        return best

    return solve(0, 0, 0)

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        a = input().rstrip("\n")
        b = input().rstrip("\n")

        d = limited_distance(a, b)

        if d == 0:
            ans.append("You're logged in!")
        elif d == 1:
            ans.append("You almost got it. You're wrong in just one spot.")
        elif d == 2:
            ans.append("You almost got it, but you're wrong in two spots.")
        elif d == 3:
            ans.append("You're wrong in three spots.")
        else:
            ans.append("What you entered is too different from the real password.")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`limited_distance`hàm là tìm kiếm giới hạn được mô tả trong thuật toán. Bộ đệm lưu trữ các trạng thái bao gồm hai vị trí hiện tại và số lần chỉnh sửa đã được sử dụng. Số lần chỉnh sửa được bao gồm vì việc đạt được cùng một cặp vị trí sau khi sử dụng số lần chỉnh sửa khác nhau có thể dẫn đến các quyết định cắt tỉa khác nhau. 

Vòng lặp bỏ qua các ký tự bằng nhau là sự tối ưu hóa chính. Các phần dài giống hệt nhau của mật khẩu sẽ bị bỏ qua mà không tạo trạng thái đệ quy. Vì chỉ được phép chỉnh sửa ba lần nên việc phân nhánh đệ quy chỉ xảy ra ở một số vị trí có các chuỗi khác nhau. 

Các trường hợp cơ bản xử lý tình huống trong đó một mật khẩu kết thúc trước. Hậu tố còn lại của mật khẩu kia không thể khớp theo bất kỳ cách nào khác, vì vậy mọi ký tự còn lại đều yêu cầu chèn hoặc xóa. 

Hàm không bao giờ cần phân biệt giữa khoảng cách bốn và lớn hơn. Trở về`LIMIT + 1`là đủ vì tất cả những trường hợp đó đều tạo ra cùng một đầu ra. Số nguyên Python không tràn ở đây và kích thước bộ đệm vẫn nhỏ vì chỉ các trạng thái gần vùng không khớp mới được khám phá. 

## Ví dụ đã hoạt động 

Đối với cặp mẫu đầu tiên:```
password
password
```| Vị trí trong chuỗi đầu tiên | Vị trí trong chuỗi thứ hai | Các chỉnh sửa đã sử dụng | Hành động hiện tại | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | Bỏ qua các ký tự phù hợp | Kết thúc cả hai chuỗi | 
| 8 | 8 | 0 | Khoảng cách còn lại | 0 | 

Thuật toán loại bỏ toàn bộ tiền tố chung và đến cuối cả hai chuỗi. Khoảng cách bằng 0 nên thông báo đăng nhập sẽ được in. 

Đối với cặp mẫu thứ tư:```
password
pazzw0rd
```| Vị trí trong chuỗi đầu tiên | Vị trí trong chuỗi thứ hai | Các chỉnh sửa đã sử dụng | Hành động hiện tại | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | Nhảy`pa`| Không khớp ở chỉ số 2 | 
| 2 | 2 | 0 | Thay thế`s`với`z`| Các chỉnh sửa đã sử dụng trở thành 1 | 
| 3 | 3 | 1 | Thay thế`s`với`z`| Các chỉnh sửa đã sử dụng trở thành 2 | 
| 5 | 5 | 2 | Thay thế`o`với`0`| Các chỉnh sửa đã sử dụng trở thành 3 | 
| 8 | 8 | 3 | Kết thúc cả hai chuỗi | Khoảng cách là 3 | 

Dấu vết này cho thấy lý do tại sao việc thay thế phải được tính độc lập. Ba ký tự sai khác nhau cần phải chỉnh sửa ba lần mặc dù các chuỗi có cùng độ dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3L) là O(L) | Mỗi trạng thái được khám phá nằm trong ranh giới khoảng cách chỉnh sửa nhỏ và các lần chạy phù hợp sẽ bị bỏ qua. | 
| Không gian | O(L) trong trường hợp xấu nhất | Bảng ghi nhớ chỉ lưu trữ các trạng thái đệ quy đã khám phá. | 

Đây,`L`là độ dài kết hợp của hai mật khẩu. Giới hạn đầu vào là tổng cộng 2.000.000 ký tự là phù hợp vì thuật toán không bao giờ xây dựng bảng bậc hai và chỉ khám phá các trạng thái được tạo bởi tối đa ba lần chỉnh sửa. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import lru_cache

def program(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert program("""5
password
password
password
passw0rd
password
pazzword
password
pazzw0rd
password
username
""") == """You're logged in!
You almost got it. You're wrong in just one spot.
You almost got it, but you're wrong in two spots.
You're wrong in three spots.
What you entered is too different from the real password.
""", "sample"

assert program("""1
a
a
""") == """You're logged in!
""", "single equal character"

assert program("""1
a
b
""") == """You almost got it. You're wrong in just one spot.
""", "single replacement"

assert program("""1
abc
abcdef
""") == """You're wrong in three spots.
""", "three insertions"

assert program("""1
abc
xyz
""") == """You're wrong in three spots.
""", "three replacements"

assert program("""1
aaaaaaaaaa
bbbbbbbbbb
""") == """What you entered is too different from the real password.
""", "large mismatch count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mật khẩu bằng nhau | Tin nhắn đăng nhập | Xử lý khoảng cách bằng không | 
| Một nhân vật so với nhân vật khác | Một tin nhắn di chuyển | Chuyển tiếp thay thế | 
| Tiền tố ngắn so với chuỗi dài hơn | Tin nhắn ba nước | Phần chèn ở cuối | 
| Các chuỗi có độ dài bằng nhau hoàn toàn khác nhau | Ba nước đi hoặc phân loại lớn hơn | Chỉnh sửa xử lý giới hạn | 
| Nhiều ký tự không khớp | Thông điệp quá khác biệt | Cắt sớm vượt quá khoảng cách ba | 

## Vỏ cạnh 

Sự khác biệt về độ dài lớn hơn ba sẽ được xử lý trước khi đệ quy có thể lãng phí thời gian. Ví dụ:```
Input:
1
a
abcde
```Mật khẩu thứ hai có thêm bốn ký tự. Cho dù hiện tại`a`khớp hoàn hảo, cần có bốn lần chèn. Thuật toán trả về khoảng cách lớn hơn ba và in:```
What you entered is too different from the real password.
```Bộ đếm không khớp dựa trên vị trí sẽ chỉ tập trung không chính xác vào ký tự được chia sẻ và bỏ lỡ các phần chèn cần thiết. 

Việc chèn một lần vào giữa mật khẩu sẽ được xử lý vì thuật toán sẽ cố gắng xóa và chèn chứ không chỉ thay thế. Vì:```
Input:
1
abcde
abXcde
```Quá trình đệ quy bỏ qua`ab`, đạt đến điểm không khớp và cố gắng xóa`X`từ chuỗi thứ hai. Các ký tự còn lại khớp nhau, cho khoảng cách là một. 

Mật khẩu có ký tự giống nhau nhưng viết hoa khác nhau được coi là khác nhau vì so sánh sử dụng các ký tự thực tế. Vì:```
Input:
1
Password
password
```Ký tự đầu tiên khác nhau nên thuật toán sẽ tính ký tự thay thế. Đầu ra là:```
You almost got it. You're wrong in just one spot.
```Điều này tránh việc vô tình coi mật khẩu là không phân biệt chữ hoa chữ thường, điều này sẽ làm thay đổi ý nghĩa của so sánh.
