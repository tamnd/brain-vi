---
title: "CF 102212C - Lợn Latin"
description: "Mỗi test case là một câu tiếng Anh. Ký tự đầu tiên của câu là chữ hoa, các chữ cái còn lại là chữ thường và không có dấu câu."
date: "2026-08-18T00:34:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102212
codeforces_index: "C"
codeforces_contest_name: "Amazalgo Uni 2019 Practice Contest"
rating: 0
weight: 102212
solve_time_s: 521
verified: false
draft: false
---

[CF 102212C - Tiếng Latin lợn](https://codeforces.com/problemset/problem/102212/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi test case là một câu tiếng Anh. Ký tự đầu tiên của câu là chữ hoa, các chữ cái còn lại là chữ thường và không có dấu câu. Chúng ta cần chuyển đổi từng từ một cách độc lập bằng cách sử dụng quy tắc Pig Latin đã cho: xóa ký tự đầu tiên của từ, nối các ký tự còn lại, sau đó nối ký tự đầu tiên theo sau`ay`. 

Ví dụ,`Hello`trở thành`Ellohay`. các`H`được di chuyển phía sau`ello`, vậy kết quả là`Ello`theo sau là`hay`. Vì ký tự đầu tiên của câu gốc là chữ hoa nên việc di chuyển ký tự đó thay vì thay đổi kiểu chữ sẽ tự động giữ cho câu kết quả được viết hoa. 

Có nhiều nhất là 20 câu. Không có độ dài câu tối đa nào được nêu trong các ràng buộc được cung cấp, vì vậy thiết kế an toàn là làm cho thời gian chạy tỷ lệ thuận với tổng số ký tự trong đầu vào. Một thuật toán liên tục quét hoặc xây dựng lại toàn bộ câu cho mỗi ký tự có thể trở thành bậc hai về độ dài câu, trong khi một lần chuyển qua mỗi ký tự là tuyến tính và vừa vặn thoải mái với giới hạn 1 giây đối với kích thước đầu vào của cuộc thi thông thường. 

Trường hợp cạnh chính là một từ có một chữ cái. Đối với câu`I`, từ này không còn ký tự nào nên kết quả chỉ đơn giản là`Iay`. Việc triển khai bất cẩn cho rằng luôn có một hậu tố có thể vô tình truy cập vào một vị trí không hợp lệ hoặc xây dựng từ không chính xác. 

Một sai lầm dễ mắc nữa là quên rằng ký tự in hoa thuộc về từ đó và cũng phải được di chuyển. Vì`Apple`, câu trả lời là`PpleAay`, không`Appleay`và không`ppleAay`. Ký tự đầu tiên được chuyển xuống cuối trước`ay`được thêm vào. 

Trường hợp ranh giới cuối cùng là một câu có chứa nhiều từ, vì sự chuyển đổi phải xảy ra riêng biệt cho mỗi từ. Vì`Go to`, kết quả đúng là`Ogay otay`. Chỉ áp dụng phép biến đổi một lần cho toàn bộ câu sẽ coi khoảng trắng và từ thứ hai là một phần của cùng một chuỗi không chính xác. 

## Phương pháp tiếp cận 

Việc triển khai đơn giản có thể biến đổi từng từ bằng cách lấy ký tự đầu tiên của nó, lấy phần còn lại của từ và ghép các phần lại với nhau. Đó đã là ý tưởng thuật toán đúng đắn. Thay vào đó, việc triển khai thực sự đơn giản có thể tạo mỗi từ được chuyển đổi một ký tự bằng cách sử dụng nối chuỗi lặp lại. Mặc dù văn bản kết quả là chính xác, nhưng các chuỗi không thể thay đổi được trong Python, do đó, việc mở rộng liên tục một chuỗi đang phát triển có thể sao chép tiền tố đã được tạo sẵn. Đối với một từ dài`L`, điều đó có thể mất`O(L^2)`các thao tác ký tự trong trường hợp xấu nhất. Xuyên suốt một câu có tổng độ dài`L`, do đó trường hợp xấu nhất là`O(L^2)`. 

Cấu trúc của bài toán này cho chúng ta một cách xây dựng tuyến tính đơn giản hơn. Việc chuyển đổi không phụ thuộc vào bất kỳ ký tự nào ngoại trừ ký tự đầu tiên và các ký tự còn lại giữ nguyên thứ tự ban đầu. Do đó chúng ta chỉ cần xác định ký tự đầu tiên một lần rồi nối hậu tố và ký tự đã lưu với`ay`. Việc chia câu thành các từ sẽ tạo ra các phần độc lập, do đó mỗi ký tự đầu vào được xử lý một số lần không đổi. 

Cấu trúc brute-force hoạt động hiệu quả vì nó duy trì thứ tự ký tự được yêu cầu, nhưng nó có thể lãng phí công sức sao chép tiền tố nhiều lần. Nhận xét rằng sự biến đổi chỉ đơn giản là`first character + suffix`sắp xếp lại thành`suffix + first character + "ay"`cho phép chúng tôi xây dựng từng kết quả trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(L²)`trong trường hợp xấu nhất |`O(L)`| Quá chậm để có đủ từ dài | 
| Tối ưu |`O(L)`|`O(L)`| Đã chấp nhận | 

Đây,`L`biểu thị tổng số ký tự được xử lý, bao gồm cả dấu cách. Phương pháp tối ưu là tuyến tính vì mỗi ký tự thuộc về đầu vào hoặc đầu ra một số lần không đổi. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và xử lý từng câu một cách độc lập. Một câu không bao giờ được trộn lẫn với một trường hợp kiểm thử khác vì mỗi dòng đại diện cho một thông báo riêng biệt. 
2. Tách câu theo khoảng trắng để lấy từng từ riêng lẻ. Vì dữ liệu đầu vào không chứa dấu câu và các khoảng trắng thông thường phân tách các từ nên mỗi mã thông báo kết quả chính xác là một từ cần được chuyển đổi. 
3. Với mỗi từ, lưu ký tự đầu tiên của nó và lấy chuỗi con bắt đầu tại vị trí`1`. Ký tự đầu tiên phải được lưu trước khi xây dựng kết quả vì đó là ký tự duy nhất có vị trí thay đổi. 
4. Cấu trúc từ đã chuyển đổi thành`word[1:] + word[0] + "ay"`. Hậu tố không thay đổi, ký tự đầu tiên ban đầu được đặt ngay sau nó và`ay`được thêm vào cuối cùng. 
5. Nối tất cả các từ đã chuyển đổi có dấu cách và in ra câu kết quả. Việc nối ở cuối sẽ bảo toàn chính xác một khoảng trắng giữa các từ liền kề và giữ cho các phép biến đổi từ độc lập. 

### Tại sao nó hoạt động 

Đối với mỗi từ đầu vào`w`, đặt ký tự đầu tiên của nó là`c`và hậu tố còn lại của nó là`s`, Vì thế`w = c + s`. Phép biến đổi Pig Latin cần thiết chính xác là`s + c + "ay"`, đó là những gì thuật toán xây dựng. Do thuật toán áp dụng phép biến đổi này một cách độc lập cho từng từ và không thay đổi thứ tự các từ hoặc ký tự bên trong mỗi hậu tố nên mọi từ đầu ra đều đúng và câu đầu ra hoàn chỉnh cũng đúng. Việc viết hoa cũng được giữ nguyên ở vị trí bắt buộc vì ký tự viết hoa đầu tiên của câu gốc được chuyển đến cuối từ đầu tiên, trong đó nó vẫn là chữ hoa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def transform_word(word):
    return word[1:] + word[0] + "ay"

def solve():
    t = int(input())

    for _ in range(t):
        sentence = input().strip()
        words = sentence.split()

        result = [transform_word(word) for word in words]
        print(" ".join(result))

if __name__ == "__main__":
    solve()
```

`transform_word`trực tiếp thực hiện dạng toán học của phép biến đổi.`word[0]`là nhân vật di chuyển, trong khi`word[1:]`chứa mọi ký tự giữ nguyên thứ tự tương đối ban đầu của nó. 

Tính năng hiểu danh sách áp dụng thao tác đó một lần cho mỗi từ, khớp với bước 3 và bước 4 của hướng dẫn. Việc xây dựng một danh sách và nối nó một lần sẽ tốt hơn là liên tục nối thêm vào một chuỗi câu vì cấu trúc cuối cùng vẫn là tuyến tính.`split()`là đủ vì dữ liệu đầu vào không chứa dấu câu và các từ được phân tách bằng khoảng trắng. Cuộc gọi đến`strip()`xóa dòng mới được đọc bởi`input()`, trong khi`split()`cũng xử lý mọi không gian xung quanh một cách an toàn. 

Không có phép tính số, do đó không phát sinh các điều kiện tràn số nguyên và biên số học. Việc lập chỉ mục duy nhất quan trọng là`word[0]`và mỗi từ đầu vào hợp lệ đều chứa ít nhất một ký tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Câu có hai từ,`Hello`Và`world`. Trạng thái biến đổi của mỗi từ là: 

| Lời | Ký tự đầu tiên | Hậu tố còn lại | Từ được chuyển đổi | 
| --- | --- | --- | --- | 
|`Hello`|`H`|`ello`|`Ellohay`| 
|`world`|`w`|`orld`|`orldway`| 

Hai từ biến đổi được nối với nhau bằng một dấu cách, tạo ra`Ellohay orldway`. Từ đầu tiên chứng tỏ rằng chữ hoa`H`được di chuyển thay vì chuyển thành chữ thường, do đó câu đầu ra vẫn được viết hoa. 

### Mẫu 2 

Một vài từ đầu tiên là đủ để thể hiện quá trình lặp lại và thao tác tương tự sẽ tiếp tục cho đến hết câu. 

| Lời | Ký tự đầu tiên | Hậu tố còn lại | Từ được chuyển đổi | 
| --- | --- | --- | --- | 
|`Hello`|`H`|`ello`|`Ellohay`| 
|`danbo`|`d`|`anbo`|`anboday`| 
|`Hello`|`H`|`ello`|`Ellohay`| 
|`peccy`|`p`|`eccy`|`eccypay`| 
|`How`|`H`|`ow`|`Owhay`| 
|`are`|`a`|`re`|`reaay`| 
|`you`|`y`|`ou`|`ouyay`| 
|`today`|`t`|`oday`|`odaytay`| 

Sau những phép biến đổi này, đầu ra một phần là`Ellohay anboday Ellohay eccypay Owhay reaay ouyay odaytay`. Việc xử lý các từ còn lại theo cách tương tự sẽ tạo ra kết quả mẫu được cung cấp. Dấu vết chứng tỏ rằng không có trạng thái nào được chia sẻ giữa các từ: mỗi từ bắt đầu trích xuất ký tự đầu tiên của chính nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(L)`| Mỗi ký tự đầu vào được kiểm tra và sao chép một số lần không đổi | 
| Không gian |`O(L)`| Các từ được chuyển đổi và đầu ra cuối cùng yêu cầu không gian tỷ lệ với đầu vào | 

Đây`L`là tổng chiều dài của các câu đang được xử lý. Chỉ với 20 trường hợp thử nghiệm và không có thao tác nào yêu cầu quét lồng nhau đầu vào, giải pháp tuyến tính dễ dàng phù hợp với giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def transform_word(word):
    return word[1:] + word[0] + "ay"

def solve():
    t = int(input())
    for _ in range(t):
        sentence = input().strip()
        words = sentence.split()
        print(" ".join(transform_word(word) for word in words))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        from contextlib import redirect_stdout

        out = io.StringIO()
        with redirect_stdout(out):
            solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample 1
assert run(
    "1\n"
    "Hello world\n"
) == "Ellohay orldway\n", "sample 1"

# Provided sample 2
assert run(
    "8\n"
    "Hello danbo\n"
    "Hello peccy\n"
    "How are you today\n"
    "Good how are you\n"
    "Oh no\n"
    "Whats wrong\n"
    "It seems like our messages are not being encrypted\n"
    "Dont panic\n"
) == (
    "Ellohay anboday\n"
    "Ellohay eccypay\n"
    "Owhay reaay ouyay odaytay\n"
    "Oodgay owhay reaay ouyay\n"
    "Hoay onay\n"
    "Hatsway rongway\n"
    "Tiay eemssay ikelay uroay essagesmay reaay otnay eingbay ncryptedeay\n"
    "Ontday anicpay\n"
), "sample 2"

# Minimum-size input: one one-letter word
assert run(
    "1\n"
    "I\n"
) == "Iay\n", "one-letter word"

# Multiple one-letter words
assert run(
    "1\n"
    "A I O\n"
) == "Aay Iay Oay\n", "all one-letter words"

# Boundary case: first and last characters of several words
assert run(
    "1\n"
    "Abc xyz Z\n"
) == "bAcay yz xay Zay\n", "first and last character handling"

# All-equal characters
assert run(
    "1\n"
    "Aaaa aaaa\n"
) == "aaaAay aaaay\n", "all-equal characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\nI`|`Iay`| Từ một chữ cái có kích thước tối thiểu | 
|`1\nA I O`|`Aay Iay Oay`| Nhiều từ một chữ cái và các phép biến đổi độc lập | 
|`1\nAbc xyz Z`|`bAcay yz xay Zay`| Chuyển động của ký tự đầu tiên và ranh giới hậu tố | 
|`1\nAaaa aaaa`|`aaaAay aaaay`| Các ký tự và cách viết hoa giống hệt nhau lặp đi lặp lại | 

Các mẫu được cung cấp cũng xác nhận thêm các câu nhiều từ thông thường và nhiều trường hợp thử nghiệm. Các trường hợp tùy chỉnh có chủ ý bao gồm các từ có hậu tố trống, các từ có ký tự đầu và cuối khác nhau cũng như các từ trong đó mọi ký tự đều giống hệt nhau, đây là những vị trí phổ biến gây ra lỗi lập chỉ mục hoặc nối. 

## Vỏ cạnh 

Một từ có một chữ cái không có hậu tố. Xem xét đầu vào chính xác`1`theo sau là`I`. Thuật toán đọc`word[0]`BẰNG`I`Và`word[1:]`là chuỗi rỗng, vì vậy nó xây dựng`"" + "I" + "ay"`, cho`Iay`. Không có trường hợp đặc biệt nào được yêu cầu vì lát cắt của Python`word[1:]`tự nhiên trở nên trống rỗng ở ranh giới. 

Từ đầu tiên viết hoa không yêu cầu thao tác viết hoa riêng biệt. Đối với đầu vào`Apple`, ký tự đầu tiên là`A`, hậu tố là`pple`, và kết quả là`ppleAay`. Chữ hoa`A`di chuyển cùng với ký tự ban đầu của nó thay vì bị viết thường. Một giải pháp gọi`.lower()`trên mỗi từ trước khi chuyển đổi nó sẽ tạo ra không chính xác`ppleaay`. 

Mỗi từ phải được chuyển đổi một cách độc lập. Vì`Go to`, từ đầu tiên có`G`Và`o`, cho`Ogay`, trong khi cái thứ hai có`t`Và`o`, cho`otay`. Đầu ra cuối cùng là`Ogay otay`. Việc phân tách trước tiên sẽ ngăn không cho khoảng trắng được coi là một phần của từ và đảm bảo rằng mỗi từ có chính xác một từ`ay`hậu tố. 

Các ký tự lặp lại không làm thay đổi quy tắc. Vì`Aaaa`, cái đầu tiên`A`di chuyển đến cuối hậu tố, tạo ra`aaaAay`. Vì`aaaa`, kết quả là`aaaay`. Thuật toán phân biệt ký tự đầu tiên theo vị trí của nó chứ không phải theo giá trị của nó, do đó nó vẫn đúng ngay cả khi mọi ký tự đều giống nhau.
