---
title: "CF 118A - Tác vụ chuỗi"
description: "Chúng tôi nhận được một chuỗi chứa các chữ cái tiếng Anh viết hoa và viết thường. Nhiệm vụ là chuyển đổi chuỗi này theo ba quy tắc. Đầu tiên, mọi nguyên âm phải được loại bỏ. Các nguyên âm trong bài toán này là A, O, Y, E, U, I ở cả dạng viết hoa và viết thường."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 118
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 89 (Div. 2)"
rating: 1000
weight: 118
solve_time_s: 98
verified: true
draft: false
---

[CF 118A - Nhiệm vụ chuỗi](https://codeforces.com/problemset/problem/118/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một chuỗi chứa các chữ cái tiếng Anh viết hoa và viết thường. Nhiệm vụ là chuyển đổi chuỗi này theo ba quy tắc. 

Đầu tiên, mọi nguyên âm phải được loại bỏ. Các nguyên âm trong bài toán này là`A, O, Y, E, U, I`ở cả dạng chữ hoa và chữ thường. 

Thứ hai, mọi ký tự còn lại đều là phụ âm và chúng ta phải chuyển nó thành chữ thường. 

Thứ ba, trước mỗi phụ âm ta chèn dấu chấm`.`. 

Ví dụ, chuỗi`Codeforces`trở thành`.c.d.f.r.c.s`bởi vì các nguyên âm`o`,`e`, Và`o`bị loại bỏ, trong khi các phụ âm còn lại được viết thường và có dấu chấm ở đầu. 

Độ dài đầu vào tối đa là 100 ký tự. Đó là con số cực kỳ nhỏ nên hiệu suất không phải là vấn đề đáng lo ngại ở đây. Ngay cả một giải pháp không hiệu quả cũng sẽ chạy ngay lập tức. Tuy nhiên, cách tiếp cận rõ ràng nhất là quét tuyến tính một chuỗi trên chuỗi. 

Phần khó khăn không phải là sự phức tạp mà là tính chính xác. Một số chi tiết nhỏ có thể dễ dàng đưa ra đáp án sai nếu xử lý bất cẩn. 

Một lỗi phổ biến là quên nguyên âm viết hoa. Coi như:```
Input:
aBAcAba
```Đầu ra đúng là:```
.b.c.b
```Nếu chúng ta chỉ kiểm tra nguyên âm chữ thường, chữ hoa`A`các ký tự sẽ vẫn còn trong câu trả lời một cách không chính xác. 

Một lỗi dễ mắc phải khác là thêm dấu chấm trước mỗi ký tự trước khi kiểm tra xem đó có phải là nguyên âm hay không. Ví dụ:```
Input:
tour
```Đầu ra đúng là:```
.t.r
```Việc thực hiện bất cẩn có thể tạo ra:```
.t.o.u.r
```vì nó chèn dấu chấm trước nguyên âm thay vì bỏ qua chúng hoàn toàn. 

Trường hợp tinh vi thứ ba là xử lý chính xác các phụ âm viết hoa. Ví dụ:```
Input:
QWERTY
```Đầu ra đúng là:```
.q.w.r.t
```các chữ cái`E`Và`Y`là các nguyên âm trong bài toán này và phải biến mất, trong khi các phụ âm còn lại phải chuyển thành chữ thường. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng ký tự chuỗi. Đối với mỗi ký tự, chúng tôi kiểm tra xem đó có phải là nguyên âm hay không. Nếu không, chúng tôi sẽ thêm dấu chấm và chữ thường của ký tự vào câu trả lời. 

Điều này đã chạy theo thời gian tuyến tính vì mỗi ký tự được kiểm tra một lần. 

Một phiên bản đơn giản hơn có thể liên tục xây dựng lại các chuỗi bằng cách sử dụng phép nối như:```
ans = ans + "." + ch.lower()
```Chuỗi Python là bất biến, vì vậy mỗi phép nối sẽ tạo ra một chuỗi mới. Trong trường hợp xấu nhất, điều này dẫn đến hành vi bậc hai. Chỉ với 100 ký tự, thậm chí điều đó vẫn có thể vượt qua một cách thoải mái, nhưng đó không phải là cách thực hành lý tưởng. 

Cách tiếp cận tốt hơn là xây dựng một danh sách các phần và nối chúng lại một lần ở cuối. Việc thêm vào danh sách là hiệu quả và cuối cùng`"".join(...)`tạo ra câu trả lời trong thời gian tuyến tính. 

Quan sát quan trọng là mọi ký tự được xử lý độc lập. Việc một chữ cái có tồn tại hay không chỉ phụ thuộc vào việc chữ cái cụ thể đó có phải là nguyên âm hay không. Không có sự tương tác giữa các vị trí nên chỉ cần một đường chuyền là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tạo một bộ chứa tất cả các nguyên âm ở dạng chữ thường:`{'a', 'o', 'y', 'e', 'u', 'i'}`. 

Việc sử dụng nguyên âm viết thường cho phép chúng ta chuẩn hóa các ký tự trước khi kiểm tra chúng. 
3. Tạo một danh sách trống để lưu trữ các phần của câu trả lời cuối cùng. 
4. Lặp lại từng ký tự trong chuỗi. 
5. Chuyển ký tự hiện tại thành chữ thường. 

Điều này xử lý chữ hoa và chữ thường một cách thống nhất. 
6. Kiểm tra xem ký tự viết thường có phải là nguyên âm hay không. 

Nếu là nguyên âm thì bỏ qua hoàn toàn. 
7. Nếu là phụ âm thì nối thêm`"." + character`vào danh sách câu trả lời. 
8. Sau khi xử lý tất cả các ký tự, nối danh sách thành một chuỗi và in nó. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng danh sách câu trả lời chứa phiên bản được chuyển đổi chính xác của tất cả các ký tự được xử lý. 

Khi chúng ta gặp một nguyên âm, bài toán yêu cầu nó phải biến mất, vì vậy việc bỏ qua nó sẽ duy trì tính chính xác. 

Khi gặp một phụ âm, bài toán yêu cầu hai thao tác: chuyển nó thành chữ thường và đặt dấu chấm trước nó. Đang bổ sung`"." + lowercase_character`thực hiện chính xác các thao tác đó. 

Vì mỗi ký tự được xử lý một lần và theo các quy tắc của bài toán, nên chuỗi nối cuối cùng chính xác là kết quả đầu ra được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

vowels = set("aoyeui")
result = []

for ch in s:
    ch = ch.lower()

    if ch not in vowels:
        result.append("." + ch)

print("".join(result))
```Chương trình bắt đầu bằng cách đọc chuỗi đầu vào và xóa dòng mới ở cuối bằng`strip()`. 

Bộ nguyên âm chỉ chứa các chữ cái viết thường. Thay vì kiểm tra riêng cả hai biến thể chữ hoa và chữ thường, mã sẽ chuyển đổi mọi ký tự thành chữ thường trước khi so sánh. Điều này giữ cho logic đơn giản và tránh trùng lặp. 

các`result`danh sách lưu trữ các phụ âm được chuyển đổi. Sử dụng danh sách được ưu tiên hơn là nối chuỗi lặp lại vì chuỗi Python là bất biến. Việc thêm vào danh sách là hiệu quả và việc tham gia một lần vào cuối sẽ tránh được việc sao chép không cần thiết. 

Bên trong vòng lặp, mỗi ký tự được chuẩn hóa bằng`lower()`. Nếu ký tự không phải là nguyên âm, mã sẽ thêm một dấu chấm theo sau là phụ âm viết thường. 

The final output is produced with`"".join(result)`. 

Một chi tiết tinh tế là thứ tự thực hiện các thao tác. Chúng ta phải viết thường ký tự này trước khi kiểm tra tư cách thành viên trong bộ nguyên âm. Nếu không thì nguyên âm viết hoa như`E`hoặc`Y`sẽ sống sót một cách sai lầm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
tour
```| Nhân vật hiện tại | Chữ thường | Nguyên âm? | Danh sách kết quả | 
| --- | --- | --- | --- | 
| t | t | Không | [".t"] | 
| o | o | Có | [".t"] | 
| bạn | bạn | Có | [".t"] | 
| r | r | Không | [".t", ".r"] | 

Đầu ra cuối cùng:```
.t.r
```Ví dụ này cho thấy các nguyên âm bị loại bỏ hoàn toàn chứ không phải được biến đổi. 

### Ví dụ 2 

đầu vào:```
Codeforces
```| Nhân vật hiện tại | Chữ thường | Nguyên âm? | Danh sách kết quả | 
| --- | --- | --- | --- | 
| C | c | Không | [".c"] | 
| o | o | Có | [".c"] | 
| d | d | Không | [".c", ".d"] | 
| e | e | Có | [".c", ".d"] | 
| f | f | Không | [".c", ".d", ".f"] | 
| o | o | Có | [".c", ".d", ".f"] | 
| r | r | Không | [".c", ".d", ".f", ".r"] | 
| c | c | Không | [".c", ".d", ".f", ".r", ".c"] | 
| e | e | Có | [".c", ".d", ".f", ".r", ".c"] | 
| s | s | Không | [".c", ".d", ".f", ".r", ".c", ".s"] | 

Đầu ra cuối cùng:```
.c.d.f.r.c.s
```Dấu vết này chứng tỏ rằng các phụ âm viết hoa được chuyển đổi chính xác và các nguyên âm biến mất bất kể chữ hoa chữ thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần | 
| Không gian | O(n) | Danh sách kết quả lưu trữ đầu ra được chuyển đổi | 

Kích thước đầu vào tối đa chỉ là 100 ký tự nên thuật toán dễ dàng nằm gọn trong giới hạn. Ngay cả những cách tiếp cận chậm hơn cũng có thể vượt qua, nhưng giải pháp tuyến tính vừa rõ ràng vừa hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    vowels = set("aoyeui")
    result = []

    for ch in s:
        ch = ch.lower()

        if ch not in vowels:
            result.append("." + ch)

    print("".join(result))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("tour\n") == ".t.r", "sample 1"

# custom cases
assert run("a\n") == "", "single vowel should disappear"
assert run("b\n") == ".b", "single consonant"
assert run("Codeforces\n") == ".c.d.f.r.c.s", "mixed uppercase and lowercase"
assert run("QWERTY\n") == ".q.w.r.t", "uppercase vowels removed correctly"
assert run("abcdefghijklmnopqrstuvwxyz\n") == ".b.c.d.f.g.h.j.k.l.m.n.p.q.r.s.t.v.w.x.z", "full alphabet case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| chuỗi trống | Loại bỏ nguyên âm đơn | 
|`b`|`.b`| Định dạng phụ âm đơn | 
|`Codeforces`|`.c.d.f.r.c.s`| Vỏ hỗn hợp và nhiều nguyên âm | 
|`QWERTY`|`.q.w.r.t`| Xử lý nguyên âm viết hoa | 
|`abcdefghijklmnopqrstuvwxyz`|`.b.c.d.f.g.h.j.k.l.m.n.p.q.r.s.t.v.w.x.z`| Truyền tải đầy đủ và loại bỏ tất cả nguyên âm | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
A
```Thuật toán chuyển đổi`A`sang chữ thường`a`. Từ`a`là một nguyên âm, nó được bỏ qua. Danh sách kết quả vẫn trống, do đó đầu ra là một chuỗi trống. Điều này xác nhận các nguyên âm viết hoa được xử lý chính xác. 

Hãy xem xét đầu vào:```
BCDF
```Các bước xử lý là:`B -> b -> append ".b"`

`C -> c -> append ".c"`

`D -> d -> append ".d"`

`F -> f -> append ".f"`Đầu ra cuối cùng trở thành:```
.b.c.d.f
```Điều này xác nhận rằng mỗi phụ âm nhận được chính xác một dấu chấm ở đầu. 

Hãy xem xét đầu vào:```
Yy
```Cả hai ký tự đều trở thành chữ thường`y`. Từ`y`được coi là nguyên âm trong bài toán này, cả hai đều bị bỏ qua. Đầu ra trống. 

Một sai lầm phổ biến là quên điều đó`Y`ở đây được tính là một nguyên âm mặc dù có nhiều vấn đề khác phân loại nó theo cách khác. 

Cuối cùng, hãy xem xét:```
Input:
aBAcAba
```Trình tự xử lý là:`a -> vowel -> skip`

`B -> b -> append ".b"`

`A -> a -> skip`

`c -> c -> append ".c"`

`A -> a -> skip`

`b -> b -> append ".b"`

`a -> a -> skip`Đầu ra cuối cùng là:```
.b.c.b
```Ví dụ này xác nhận rằng các nguyên âm viết hoa sẽ bị loại bỏ trước khi định dạng phụ âm diễn ra.
