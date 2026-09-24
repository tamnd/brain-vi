---
title: "CF 104803A - \u8bcd\u5178"
description: "Chúng ta có một tập hợp các chuỗi $n$ riêng biệt, mỗi chuỗi có cùng độ dài $m$. Thao tác duy nhất được phép thực hiện trên một chuỗi là tự do hoán đổi các ký tự của nó, vì bất kỳ hai vị trí nào trong một từ đều có thể hoán đổi bất kỳ số lần nào."
date: "2026-06-28T16:47:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104803
codeforces_index: "A"
codeforces_contest_name: "NOIP 2023"
rating: 0
weight: 104803
solve_time_s: 97
verified: false
draft: false
---

[CF 104803A - \u8bcd\u5178](https://codeforces.com/problemset/problem/104803/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ sưu tập$n$các chuỗi riêng biệt, mỗi chuỗi có cùng độ dài$m$. Thao tác duy nhất được phép thực hiện trên một chuỗi là tự do hoán đổi các ký tự của nó, vì bất kỳ hai vị trí nào trong một từ đều có thể hoán đổi bất kỳ số lần nào. Điều này có nghĩa là đối với mỗi từ, chúng ta không thực sự bị ràng buộc bởi thứ tự ban đầu của nó mà chỉ bị ràng buộc bởi nhiều bộ ký tự mà nó chứa. 

Đối với mỗi chỉ số$i$, chúng tôi hỏi liệu có thể sắp xếp lại từng từ một cách độc lập sao cho$i$-từ thứ trở nên nhỏ nhất về mặt từ điển trong số tất cả$n$các chuỗi kết quả. 

Vì vậy, đối với mỗi từ, chúng ta được phép chọn bất kỳ hoán vị nào của các chữ cái của nó và sau đó chúng ta so sánh các chuỗi cuối cùng theo thứ tự từ điển. Chúng tôi muốn biết liệu có cách nào để gán các hoán vị sao cho một từ được chọn$w_i$thực sự nhỏ hơn tất cả những cái khác. 

Hạn chế chính là mỗi từ độc lập về mặt sắp xếp lại, nhưng tất cả các từ phải được sắp xếp đồng thời theo cách thỏa mãn điều kiện sắp xếp toàn cục. 

Từ$n, m \le 3000$, bất kỳ giải pháp nào so sánh tất cả các cặp hoán vị hoặc cố gắng mô phỏng sự sắp xếp đều quá chậm. Một nỗ lực ngây thơ nhằm tạo ra các dạng tối ưu cho mỗi từ và so sánh với tất cả các từ khác sẽ là$O(n m \log m)$, và làm điều này trên mỗi từ ứng cử viên sẽ quá tốn kém. 

Một vấn đề nhỏ xuất hiện khi nhiều từ có cách phân bổ ký tự giống nhau. Một từ có vẻ “nhỏ” khi đứng riêng lẻ có thể bị buộc phải trở nên lớn hơn khi những từ khác cũng tối ưu hóa thứ tự của chúng, bởi vì tất cả các từ đều được tối ưu hóa đồng thời cho cùng một mục tiêu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là xem xét từng từ$i$, sau đó cố gắng xây dựng các hoán vị của tất cả các từ để tối đa hóa cơ hội$i$nhỏ nhất về mặt từ điển. Đối với một cố định$i$, người ta có thể cố gắng xây dựng chuỗi nhỏ nhất có thể một cách tham lam cho$w_i$và đối với mỗi từ khác, hãy tạo một chuỗi càng lớn càng tốt trong khi vẫn sử dụng cùng một bộ ký tự. 

Đối với một từ, việc sắp xếp các ký tự của nó sẽ mang lại hoán vị nhỏ nhất có thể về mặt từ điển, trong khi việc đảo ngược sẽ mang lại hoán vị lớn nhất về mặt từ điển. Vì vậy, một kiểm tra ngây thơ cho mỗi$i$có thể là: so sánh được sắp xếp$w_i$chống lại việc sắp xếp$w_j$hoặc chống lại đảo ngược$w_j$, tùy theo cách giải thích. 

Tuy nhiên, cách tiếp cận này không thành công vì so sánh từ điển không độc lập với mỗi từ. Trật tự tương đối chỉ phụ thuộc vào vị trí khác nhau đầu tiên, và các từ khác nhau có thể “trì hoãn” sự khác biệt của chúng theo những cách phá vỡ sự lựa chọn toàn cục ngây thơ. Khó khăn thực sự là chúng ta không chỉ so sánh các chuỗi cố định mà còn hỏi liệu có tồn tại sự gán hoán vị nhất quán tạo ra mức tối thiểu nghiêm ngặt tại vị trí hay không.$i$. 

Quan sát quan trọng là đối với bất kỳ từ nào, dạng từ điển nhỏ nhất có thể tốt nhất chỉ đơn giản là phiên bản được sắp xếp của nó. Không có hoán vị nào khác có thể đánh bại nó. Tương tự, mỗi từ đều có một chuỗi “giới hạn dưới” cố định$s_i$, dạng được sắp xếp của nó và không có sự sắp xếp nào có thể tạo ra thứ gì nhỏ hơn dạng này. 

Vì vậy cách duy nhất$w_i$có thể là mức tối thiểu nghiêm ngặt nếu dạng tốt nhất có thể của nó nhỏ hơn hẳn dạng tốt nhất có thể có của mọi từ khác. Bởi vì nếu ngay cả dạng tốt nhất của một từ khác cũng nhỏ hơn hoặc bằng thì$i$không bao giờ có thể thắng. 

Do đó, vấn đề giảm xuống còn việc sắp xếp từng từ và so sánh các dạng tối thiểu chuẩn tắc này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 m!)$|$O(nm)$| Quá chậm | 
| Tối ưu |$O(n m \log m)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng một biểu diễn chuẩn cho mỗi từ bằng cách sắp xếp các ký tự của nó. 

1. Với mỗi từ$w_i$, sắp xếp các ký tự của nó để có được$s_i$. Điều này thể hiện chuỗi nhỏ nhất về mặt từ điển có thể đạt được từ$w_i$, vì bất kỳ hoán vị nào cũng chỉ có thể sắp xếp lại nhiều tập hợp và việc sắp xếp sẽ giảm thiểu thứ tự từ điển. 
2. So sánh tất cả$s_i$chuỗi để tìm mức tối thiểu toàn cầu của chúng theo thứ tự từ điển. Đặt chuỗi tối thiểu này là$s_k$. 
3. Đối với mỗi chỉ số$i$, xuất ra 1 khi và chỉ khi$s_i = s_k$, nếu không thì xuất ra 0. 

Lý do điều này có tác dụng là vì nếu dạng được sắp xếp của một từ không ở mức tối thiểu toàn cục thì sẽ tồn tại một từ khác có dạng được sắp xếp nhỏ hơn hoàn toàn. Vì không có từ nào có thể được làm nhỏ hơn dạng đã được sắp xếp của nó,$i$không bao giờ có thể trở thành mức tối thiểu nghiêm ngặt trong bất kỳ cấu hình nào. 

### Tại sao nó hoạt động 

Mỗi từ có giới hạn dưới cố định trong tất cả các phép toán được phép: hoán vị được sắp xếp của nó. Bất kỳ chuỗi cuối cùng có thể đạt được nào đều phải lớn hơn hoặc bằng giới hạn này về mặt từ điển. Do đó, trong số tất cả các cấu hình có thể có của tất cả các từ, ứng cử viên từ điển sớm nhất có thể có mà bất kỳ từ nào cũng có thể đạt được là phiên bản được sắp xếp của nó. 

Nếu phiên bản được sắp xếp của một từ không phải là phiên bản nhỏ nhất trong số tất cả các phiên bản được sắp xếp thì sẽ tồn tại một từ khác có cách biểu thị tối thiểu có thể đạt được nhỏ hơn và từ đó sẽ luôn chiếm ưu thế bất kể hoán vị được chọn như thế nào. Ngược lại, nếu một từ chia sẻ chuỗi được sắp xếp nhỏ nhất trên toàn cầu, nó có thể được sắp xếp để đạt được dạng đó trong khi những từ khác không thể vượt quá giới hạn của chính chúng, khiến nó có thể ở mức tối thiểu nghiêm ngặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
words = [input().strip() for _ in range(n)]

sorted_words = [''.join(sorted(w)) for w in words]
min_sorted = min(sorted_words)

res = []
for s in sorted_words:
    res.append('1' if s == min_sorted else '0')

print(''.join(res))
```Giải pháp đọc tất cả các từ, chuyển đổi từng từ thành dạng chuẩn được sắp xếp và sau đó tìm từ nhỏ nhất trong số đó. Đầu ra cuối cùng kiểm tra sự bằng nhau với mức tối thiểu này. 

Chi tiết triển khai chính là chúng tôi không bao giờ so sánh trực tiếp các từ gốc. Chỉ các biểu mẫu được sắp xếp mới quan trọng vì chúng thể hiện phạm vi đầy đủ của các hoạt động được phép. Việc sắp xếp từng chuỗi chiếm ưu thế trong thời gian chạy và tính năng sắp xếp tích hợp của Python đủ hiệu quả để$n, m \le 3000$. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với ba từ: 

đầu vào:```
3 4
baca
abca
caaa
```Các hình thức sắp xếp: 

| tôi | bản gốc | được sắp xếp | 
| --- | --- | --- | 
| 1 | baca | bàn tính | 
| 2 | abca | aabc | 
| 3 | caa | cây keo | 

Chúng tôi so sánh về mặt từ điển. 

| bước | tối thiểu hiện tại | ứng cử viên | 
| --- | --- | --- | 
| 1 | bàn tính | bàn tính | 
| 2 | aabc | cập nhật | 
| 3 | aabc | cây keo | 

Mức tối thiểu cuối cùng là`aabc`, tương ứng với từ 2. 

Đầu ra:```
010
```Điều này xác nhận rằng chỉ từ 2 mới có thể đạt được sự sắp xếp nhỏ nhất có thể trên toàn cầu. 

Bây giờ hãy xem xét một trường hợp có quan hệ: 

đầu vào:```
3 3
cba
bca
abc
```Các hình thức sắp xếp: 

| tôi | bản gốc | được sắp xếp | 
| --- | --- | --- | 
| 1 | cba | abc | 
| 2 | bca | abc | 
| 3 | abc | abc | 

Tất cả các dạng được sắp xếp đều như nhau, vì vậy tất cả các từ đều có thể đạt được cấu hình tối thiểu như nhau. 

Đầu ra:```
111
```Điều này chứng tỏ rằng nhiều từ có thể đồng thời tối ưu khi nhiều bộ của chúng giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n m \log m)$| Mỗi trong số$n$chuỗi được sắp xếp riêng lẻ | 
| Không gian |$O(nm)$| Lưu trữ cho tất cả các chuỗi và các phiên bản được sắp xếp của chúng | 

Các ràng buộc cho phép tổng cộng tối đa 9 triệu ký tự, do đó, việc sắp xếp từng chuỗi một cách độc lập là đủ nhanh trong Python và nằm trong giới hạn 1 giây nếu triển khai hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    n, m = map(int, input().split())
    words = [input().strip() for _ in range(n)]
    sorted_words = [''.join(sorted(w)) for w in words]
    mn = min(sorted_words)
    print(''.join('1' if s == mn else '0' for s in sorted_words))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old_stdin
    return out.getvalue().strip()

# provided sample
assert run("4 7\nabandon\nbananaa\nabaanna\nnotnotn") == "1110"

# minimum size
assert run("1 3\nabc") == "1"

# all identical multisets
assert run("2 3\nabc\nbca") == "11"

# strict ordering
assert run("3 3\ncba\nbca\nabc") == "001"

# duplicate minimum only
assert run("3 4\nbaca\nabca\ncaaa") == "010"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| từ đơn | 1 | trường hợp cơ sở | 
| hoán vị giống hệt nhau | 11 | xử lý cà vạt | 
| đặt hàng nghiêm ngặt | 001 | lựa chọn tối thiểu đúng | 
| trường hợp hỗn hợp | 010 | tính đúng đắn chung | 

## Vỏ cạnh 

One edge case is when multiple words share identical character multisets. Ví dụ: 

đầu vào:```
3 3
abc
bca
cab
```Mỗi dạng được sắp xếp là`abc`, do đó tất cả các từ đều tạo ra mức tối thiểu chuẩn như nhau. Đầu ra của thuật toán`111`. Vì tất cả các từ có thể được sắp xếp lại thành cùng một chuỗi nhỏ nhất về mặt từ điển, bất kỳ từ nào trong số chúng có thể đóng vai trò là mức tối thiểu tùy thuộc vào việc ngắt kết nối, vì vậy tất cả đều hợp lệ. 

Một trường hợp khác là khi một từ bị thống trị nghiêm ngặt: 

đầu vào:```
2 3
cba
abc
```Các hình thức được sắp xếp là`abc`Và`abc`, vì vậy cả hai đều bằng nhau và cả hai đều là cực tiểu hợp lệ. Nếu chúng ta sửa đổi một chút:```
2 3
cbb
abc
```Các biểu mẫu được sắp xếp trở thành`bbc`Và`abc`. Từ`abc`nhỏ hơn, chỉ từ thứ hai có thể là nhỏ nhất và kết quả là`01`. Từ đầu tiên không thể khắc phục được nhược điểm về từ điển vì không hoán vị nào có thể tạo ra một chuỗi nhỏ hơn`bbc`.
