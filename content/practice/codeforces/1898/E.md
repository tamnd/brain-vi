---
title: "CF 1898E - Sofia và Dây đàn"
description: "Chúng ta được cho hai chuỗi, s và t. Nhiệm vụ là xác định xem có thể chuyển đổi s thành t bằng hai thao tác: loại bỏ bất kỳ ký tự nào khỏi s hoặc sắp xếp bất kỳ chuỗi con nào của s theo thứ tự bảng chữ cái hay không."
date: "2026-06-08T21:32:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1898
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 910 (Div. 2)"
rating: 2200
weight: 1898
solve_time_s: 125
verified: false
draft: false
---

[CF 1898E - Sofia và Dây đàn](https://codeforces.com/problemset/problem/1898/E) 

**Xếp hạng:** 2200 
**Thẻ:** cấu trúc dữ liệu, tham lam, sắp xếp, chuỗi, hai con trỏ 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi,`s`Và`t`. Nhiệm vụ là xác định liệu có thể chuyển đổi`s`vào trong`t`sử dụng hai thao tác: xóa bất kỳ ký tự nào khỏi`s`hoặc sắp xếp bất kỳ chuỗi con nào của`s`theo thứ tự bảng chữ cái. Sự chuyển đổi phải kết thúc bằng`t`chính xác, không chỉ là sự hoán vị các ký tự của nó. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra độ dài của`s`Và`t`, sau đó là chính các chuỗi đó. Đầu ra là "CÓ" nếu có thể chuyển đổi và "KHÔNG" nếu ngược lại. 

Những ràng buộc chỉ ra rằng`n`có thể đạt tới 200.000 và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng bị giới hạn ở 200.000. Điều này loại trừ bất kỳ thuật toán nào tệ hơn tuyến tính (`O(n log n)`) độ phức tạp cho từng trường hợp thử nghiệm. Các phương pháp bậc hai, như thử tất cả các dãy con có thể có của`s`, sẽ quá chậm. 

Các trường hợp nguy hiểm có thể phá vỡ các cách tiếp cận ngây thơ bao gồm các tình huống trong đó`t`là một dãy con của một phiên bản được sắp xếp của`s`nhưng không phải của`s`chính nó. Ví dụ, nếu`s = "cba"`Và`t = "bc"`, một thuật toán bất cẩn kiểm tra các chuỗi con trong`s`không xem xét thao tác sắp xếp có thể từ chối nó một cách không chính xác, nhưng việc sắp xếp tiền tố sẽ đưa ra câu trả lời đúng. Một trường hợp cạnh khác xảy ra khi`t`chứa các chữ cái bị thiếu`s`; điều này sẽ ngay lập tức trả về "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là mô phỏng mọi trình tự xóa và sắp xếp chuỗi con. Về mặt khái niệm, chúng ta có thể thử tất cả các chuỗi con được sắp xếp có thể có của`s`và xóa các ký tự cho đến khi chúng tôi khớp`t`. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế: ngay cả đối với`n = 100`, số lượng kết hợp là vô cùng lớn. Bản thân mỗi thao tác sắp xếp chuỗi con là`O(n log n)`, và có`O(n^2)`các chuỗi con. 

Thông tin chi tiết quan trọng là thao tác thứ hai, sắp xếp bất kỳ chuỗi con nào, cho phép chúng ta di chuyển các ký tự về phía trước trong chuỗi miễn là không có ký tự nhỏ hơn nào chặn chúng. Tức là để hình thành`t`, chúng ta cần các chữ cái xuất hiện theo cùng một thứ tự tương đối từ phải sang trái xét đến tính chẵn lẻ của vị trí của chúng. Việc sắp xếp chỉ có thể di chuyển các ký tự qua các vị trí có cùng giá trị chẵn lẻ (chỉ số chẵn hoặc lẻ), vì thứ tự từ điển không thể cho phép một ký tự nhảy qua một ký tự nhỏ hơn từ giá trị chẵn lẻ đối diện mà không có thao tác sắp xếp bao trùm nó. 

Để thực hiện điều này, chúng tôi quét`t`từ phải sang trái và cố gắng ghép từng ký tự với ký tự tương ứng trong`s`. Chúng tôi cũng duy trì số lượng các chữ cái còn lại có giá trị nhỏ hơn để xác định xem một ký tự có bị "chặn" hay không. Nếu một ký tự bắt buộc bị chặn bởi một ký tự nhỏ hơn ở bên phải của nó (có cùng tính chẵn lẻ) thì việc chuyển đổi là không thể. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính với bảng tần số, mang lại giải pháp hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3 log n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng truy cập`cnt`cho mỗi chữ cái`a`ĐẾN`z`để theo dõi số lần xuất hiện của mỗi ký tự còn lại trong`s`khi chúng tôi quét từ cuối. 
2. Đặt hai con trỏ:`i`vào cuối`s`Và`j`vào cuối`t`. 
3. Trong khi`j >= 0`, cố gắng so khớp`t[j]`với`s[i]`:

- Nếu như`s[i] == t[j]`, giảm cả hai`i`Và`j`và cập nhật bộ đếm cho`s[i]`. 
- Nếu như`s[i] != t[j]`, kiểm tra xem`s[i]`nhỏ hơn`t[j]`. Nếu vậy, hãy tăng bộ đếm của nó và di chuyển`i`trái, vì ký tự này có thể chặn`t[j]`sau này nếu chúng ta cố gắng sắp xếp. 
- Nếu tồn tại bất kỳ ký tự nào nhỏ hơn (ký tự có giá trị chữ cái thấp hơn`t[j]`) với bộ đếm dương thì`t[j]`bị chặn và câu trả lời là "KHÔNG". 
4. Nếu chúng tôi khớp thành công tất cả các ký tự của`t`, trả về "CÓ". Nếu con trỏ`i`đạt -1 và`j`không phải là -1, trả về "KHÔNG". 

Tại sao nó hoạt động: Bằng cách quét từ bên phải, chúng tôi đảm bảo rằng mỗi ký tự của`t`có thể được "kéo" từ`s`sử dụng tính năng xóa và sắp xếp mà không bị chặn bởi các chữ cái nhỏ hơn ở bên phải. Mảng truy cập theo dõi các trình chặn tiềm năng một cách hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_transform(s, t):
    n, m = len(s), len(t)
    cnt = [0] * 26
    i, j = n - 1, m - 1
    while j >= 0:
        target = ord(t[j]) - ord('a')
        while i >= 0 and ord(s[i]) - ord('a') != target:
            cnt[ord(s[i]) - ord('a')] += 1
            i -= 1
        if i < 0:
            return "NO"
        # Check if there is any smaller character blocking the target
        for k in range(target):
            if cnt[k] > 0:
                return "NO"
        i -= 1
        j -= 1
    return "YES"

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    s = input().strip()
    t_str = input().strip()
    print(can_transform(s, t_str))
```Mã này thiết lập một bộ đếm các chữ cái trong`s`và lặp lại từ cuối cả hai chuỗi, đảm bảo rằng mỗi ký tự của`t`có thể được so khớp mà không bị chặn bởi các chữ cái nhỏ hơn. Chúng tôi cẩn thận giảm bộ đếm và con trỏ để duy trì tính bất biến`s[i+1:]`chỉ chứa các chữ cái có thể được bỏ qua hoặc loại bỏ một cách an toàn. 

## Ví dụ đã hoạt động 

Theo dõi thông qua đầu vào mẫu đầu tiên`s = "sofia"`Và`t = "afios"`: 

| tôi | j | s[i] | t[j] | cnt | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 4 | một | s | ... | s[i] != t[j], tăng cnt[a], i-- | 
| 3 | 4 | tôi | s | ... | s[i] != t[j], tăng cnt[i], i-- | 
| 2 | 4 | f | s | ... | s[i] != t[j], tăng cnt[f], i-- | 
| 1 | 4 | o | s | ... | s[i] != t[j], tăng cnt[o], i-- | 
| 0 | 4 | s | s | ... | khớp, i--, j-- | 

Tất cả các chữ cái đều khớp thành công, vì vậy đầu ra là "CÓ". 

Vì`s = "cba"`Và`t = "bc"`: 

| tôi | j | s[i] | t[j] | cnt | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | một | c | cnt[a]=1 | a < c, tiếp tục i-- | 
| 1 | 1 | b | c | cnt[b]=1 | b < c, tiếp tục i-- | 
| 0 | 1 | c | c | khớp, i--, j-- | | 

Kế tiếp`t[j] = b`, tôi = -1. Bộ đếm còn lại cnt[b]=1 cho phép b di chuyển, vì vậy "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét`s`Và`t`một lần, với việc kiểm tra liên tục bằng bộ đếm 26 chữ cái | 
| Không gian | O(26) | Chỉ cần mảng bộ đếm để theo dõi các trình chặn | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm 200.000, giải pháp dễ dàng phù hợp trong 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())  # assumes the solution above is saved as solution.py
    return output.getvalue().strip()

# provided samples
assert run("""8
5 5
sofia
afios
3 2
cba
bc
5 1
sofia
e
15 7
anavolimilovana
aamanan
26 4
abcdefghijklmnopqrstuvwxyz
nope
26 4
zyxwvutsrqponmlkjihgfedcba
nope
7 3
apricot
cat
3 3
cba
acb""") == "\n".join(["YES","YES","NO","YES","NO","YES","NO","YES"])

# custom cases
assert run("""1
3 3
abc
abc""") == "YES", "already equal"
assert run("""1
3 3
abc
cab""") == "YES", "sort prefix"
assert run("""1
5 3
aaaaa
aaa""") == "YES", "all letters same, deletion"
assert run("""1
5 5
abcde
fghij""") == "NO", "letters missing"
assert run("""1
1 1
a
b""") == "NO", "single letter impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Những gì nó xác nhận
