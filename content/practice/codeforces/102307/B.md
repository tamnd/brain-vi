---
title: "CF 102307B - Nhàm chán không phải bảng màu"
description: "Chúng tôi có một chuỗi không trống s. Thao tác duy nhất được phép là thêm các ký tự vào đầu bên phải của nó. Mục tiêu là nối thêm ít ký tự nhất có thể để toàn bộ chuỗi kết quả trở thành một bảng màu."
date: "2026-08-13T07:12:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "B"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 364
verified: true
draft: false
---

[CF 102307B - Nhàm chán không phải Palindrome](https://codeforces.com/problemset/problem/102307/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 4 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi không trống`s`. Thao tác duy nhất được phép là thêm các ký tự vào đầu bên phải của nó. Mục tiêu là nối thêm ít ký tự nhất có thể để toàn bộ chuỗi kết quả trở thành một bảng màu. 

Hạn chế chính là chúng tôi không thể sửa đổi hoặc xóa bất kỳ ký tự nào đã có. Giả sử một số hậu tố của`s`đã là một palindrome. Nếu chúng ta giữ hậu tố đó làm phần trung tâm của bảng màu cuối cùng, thì mọi thứ trước nó có thể được phản chiếu bằng cách thêm phần đảo ngược của nó. Ví dụ, đối với`helloworld`, hậu tố palindromic dài nhất chỉ là`d`. Tiền tố trước nó là`helloworl`, đảo ngược của nó là`lrowolleh`, vì vậy câu trả lời trở thành`helloworldlrowolleh`. 

Độ dài đầu vào tối đa là 5000. Độ dài đó đủ nhỏ để`O(n^2)`thuật toán có khả năng thực tế, nhưng nó cũng làm cho giải pháp tuyến tính trở nên đơn giản và loại bỏ nhu cầu dựa vào các hệ số hằng số tương đối rộng rãi của các phép toán chuỗi Python. MỘT`O(n^3)`Cách tiếp cận rõ ràng sẽ không phù hợp vì 5000 ký tự sẽ bao hàm hàng tỷ thao tác. Giải pháp mục tiêu sẽ sử dụng`O(n)`thời gian và`O(n)`ký ức. 

Có một số trường hợp ranh giới có thể khiến việc triển khai hợp lý trở nên sai lầm. Đối với đầu vào một ký tự, chẳng hạn như`a`, chuỗi này đã là một chuỗi palindrome nên kết quả đầu ra đúng là`a`. Việc triển khai luôn gắn thêm ít nhất một ký tự sẽ tạo ra một chuỗi dài hơn một cách không chính xác. 

Đối với một chuỗi đã có giá trị palindromic như`anitalavalatina`, không nên thêm gì vào. Đầu ra chính xác là chính xác`anitalavalatina`. Việc triển khai chỉ tìm kiếm các hậu tố palindromic thích hợp có thể vô tình thêm các ký tự không cần thiết. 

Hậu tố không nhất thiết phải chỉ có một ký tự. Vì`aace`, hậu tố palindromic dài nhất là`e`, vậy câu trả lời là`aacecaa`. Một cách tiếp cận bất cẩn chỉ kiểm tra xem toàn bộ chuỗi có phải là một bảng màu hay không và nối thêm toàn bộ chuỗi ngược lại, sẽ tạo ra`aaceecaa`, hợp lệ nhưng không tối thiểu. 

Hậu tố palindromic dài nhất cũng có thể khá dài mà không phải là toàn bộ chuỗi. Vì`abac`, hậu tố`c`là hậu tố palindromic duy nhất dài hơn một ký tự, vì vậy câu trả lời là`abacaba`. Thuật toán phải tìm hậu tố dài nhất như vậy thay vì chỉ dừng lại ở bảng màu thuận tiện đầu tiên trừ khi các ứng cử viên được kiểm tra theo đúng thứ tự. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi hậu tố, bắt đầu bằng toàn bộ chuỗi và kiểm tra xem hậu tố đó có phải là một bảng màu hay không. Hậu tố palindromic đầu tiên được tìm thấy là hậu tố dài nhất. Nếu chiều dài của nó là`k`, tiền tố của độ dài`n-k`không phải là một phần của hậu tố đó nên chúng tôi thêm phần đảo ngược của nó. Điều này đúng vì hậu tố hiện tại đã đối xứng, trong khi mọi ký tự trong tiền tố đều cần một ký tự phù hợp được thêm vào bên phải. 

Kiểm tra palindrome cho hậu tố có độ dài`k`mất nhiều nhất`floor(k/2)`so sánh nhân vật. Nếu mọi ứng cử viên đều phải được kiểm tra và mọi lần kiểm tra đều đạt đến điểm giữa của nó thì tổng số lần so sánh là`sum(floor(k/2))`vì`k = 1 ... n`. 

Vì`n = 5000`, đây chính xác là`6,250,000`so sánh. Việc tạo và đảo ngược các chuỗi con ứng viên cũng giới thiệu thêm`O(n^2)`sao chép ký tự. Do đó, cách tiếp cận này là phương trình bậc hai trong trường hợp xấu nhất. Với giới hạn cụ thể này, nó có thể được chuyển sang nhiều ngôn ngữ, nhưng nó không khai thác được cấu trúc của vấn đề. 

Quan sát nhanh hơn là việc tìm hậu tố palindromic dài nhất có thể được chuyển đổi thành bài toán so khớp tiền tố tiêu chuẩn. Đảo ngược chuỗi và gọi nó`r`. Một hậu tố của`s`là một palindrome chính xác khi tiền tố tương ứng của`r`là cùng một chuỗi. 

Ví dụ, hãy xem xét`s = abac`. Ngược lại của nó là`r = caba`. Hậu tố`c`của`s`tương ứng với tiền tố`c`của`r`. Nếu một hậu tố dài hơn của`s`là palindromic, tiền tố dài hơn của`r`sẽ phù hợp với hậu tố của`s`chính nó. 

Chúng ta có thể mã hóa sự so sánh này bằng hàm tiền tố KMP. xây dựng`r + '#' + s`. 

Dấu phân cách là một ký tự không xuất hiện trong đầu vào, do đó tiền tố không thể vô tình vượt qua từ`r`vào trong`s`. Giá trị cuối cùng của hàm tiền tố KMP cho biết độ dài của tiền tố dài nhất của`r`đó cũng là hậu tố của`s`. Bởi vì tiền tố của`r`là đảo ngược của hậu tố của`s`, sự bình đẳng giữa chúng có nghĩa là hậu tố này đọc giống hệt nhau theo cả hai hướng. Nó chính xác là hậu tố palindromic dài nhất mà chúng ta cần. 

Khi đã biết được độ dài đó, phần còn lại là ngay lập tức. Nếu hậu tố palindromic dài nhất có độ dài`k`, nối thêm`reverse(s[:n-k])`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận với n 5000, nhưng kém hiệu quả hơn | 
| Hàm tiền tố KMP | O(n) | O(n) | Được chấp nhận và tối ưu | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`và xây dựng điều ngược lại của nó`r`. Chúng tôi quan tâm đến hậu tố palindromic của`s`và chúng trở thành tiền tố khi nhìn từ hướng ngược lại. 
2. Xây dựng chuỗi kết hợp`t = r + '#' + s`. Dấu phân cách không được xảy ra trong`s`, vì hàm tiền tố KMP không bao giờ được coi các ký tự ở hai phía đối diện của dấu phân cách là một phần của một kết quả khớp ứng cử viên. 
3. Tính hàm tiền tố KMP`pi`vì`t`. Đối với mỗi vị trí,`pi[i]`lưu trữ độ dài của tiền tố thích hợp dài nhất của`t`đó cũng là một hậu tố kết thúc ở vị trí`i`. 
4. Lấy`k = pi[-1]`. Đây là tiền tố dài nhất của`r`đó cũng là hậu tố của`s`. Vì tiền tố của`r`là đảo ngược của hậu tố tương ứng của`s`, đẳng thức có nghĩa là hậu tố này là một palindrome. 
5. Giữ cái đầu tiên`n-k`nhân vật của`s`là phần vẫn cần một hình ảnh phản chiếu. Nối ngược lại của họ,`s[:n-k][::-1]`. Chuỗi kết quả là một palindrome. 
6. In chuỗi kết quả. Nếu như`s`thì đã là một palindrome rồi`k = n`, Vì thế`s[:n-k]`trống và không có gì được thêm vào. 

### Tại sao nó hoạt động 

hãy để`P`là hậu tố palindromic dài nhất của`s`, với chiều dài`k`. Bởi vì`P`là một palindrome, đảo ngược của nó vẫn là`P`. Đảo ngược toàn bộ chuỗi biến hậu tố này thành tiền tố của`r`, Vì thế`P`đồng thời là tiền tố của`r`và một hậu tố của`s`. Hàm tiền tố KMP tìm ra sự trùng lặp dài nhất như vậy, đưa ra chính xác`k`. 

Tiền tố còn lại`s[:n-k]`được đặt ở đầu chuỗi cuối cùng. Việc thêm phần đảo ngược của nó sẽ cho các ký tự phù hợp ở phía đối diện, trong khi phần ở giữa`P`đã là một palindrome. Do đó, chuỗi được xây dựng là một chuỗi palindrome. 

Sự tối thiểu xuất phát từ việc lựa chọn hậu tố palindromic dài nhất. Bất kỳ bảng màu cuối cùng hợp lệ nào cũng phải để lại một số hậu tố của chuỗi gốc làm phần bảng màu trung tâm của nó, bởi vì chỉ các ký tự mới mới có thể được thêm vào bên phải. Việc sử dụng hậu tố palindromic ngắn hơn sẽ để lại nhiều ký tự gốc hơn được phản ánh và sẽ yêu cầu chèn nhiều hơn. Do đó, hậu tố palindromic dài nhất cung cấp số lượng ký tự được nối thêm tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_string(s):
    r = s[::-1]
    t = r + '#' + s

    pi = [0] * len(t)

    for i in range(1, len(t)):
        j = pi[i - 1]

        while j > 0 and t[i] != t[j]:
            j = pi[j - 1]

        if t[i] == t[j]:
            j += 1

        pi[i] = j

    k = pi[-1]
    return s + s[:len(s) - k][::-1]

def main():
    s = input().strip()
    print(solve_string(s))

if __name__ == "__main__":
    main()
```các`solve_string`chức năng đầu tiên tạo ra`r`, đảo ngược của đầu vào. Chuỗi kết hợp`t`chứa thông tin mà KMP cần để so sánh các tiền tố của`r`với hậu tố của`s`. 

Mảng tiền tố`pi`được tính theo cách chuẩn. Biến`j`đại diện cho độ dài của trận đấu ứng cử viên hiện tại. Khi`t[i]`không khớp`t[j]`, tiếp theo`pi[j - 1]`nhảy trực tiếp đến đường viền có thể tiếp theo thay vì bắt đầu lại quá trình so sánh từ 0. Đó là điều làm cho việc tính toán tiền tố trở nên tuyến tính. 

Giá trị cuối cùng`pi[-1]`là độ dài của hậu tố palindromic dài nhất. Nếu như`k`với độ dài đó thì số ký tự phải được thêm vào là`len(s) - k`. Cắt Python tạo tiền tố bắt buộc`s[:len(s) - k]`, Và`[::-1]`cung cấp chính xác các ký tự cần thiết ở phía bên phải. 

Dải phân cách`#`là an toàn vì đầu vào của vấn đề là một chuỗi không có dấu cách, nhưng câu lệnh không hạn chế rõ ràng bảng chữ cái ở các chữ cái viết thường. Việc triển khai phòng thủ hơn có thể chọn một dấu phân cách không có trong đầu vào. Đoạn mã trên giả sử bộ ký tự thông thường cho vấn đề này. Để làm cho việc triển khai hoàn toàn độc lập với bảng chữ cái, thay vào đó, dấu phân cách có thể được chọn bằng một vòng lặp ngắn. 

Không có vấn đề tràn số nguyên vì số nguyên Python có độ chính xác tùy ý và các chỉ số duy nhất liên quan được giới hạn bởi độ dài chuỗi. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`s = helloworld`. Ngược lại của nó là`dlrowolleh`. Tính toán KMP chỉ tìm thấy một ký tự chồng chéo, tương ứng với hậu tố palindromic`d`. 

| Biến | Tiểu bang | 
| --- | --- | 
|`s`|`helloworld`| 
|`r`|`dlrowolleh`| 
| Hậu tố palindromic dài nhất |`d`| 
|`k`|`1`| 
| Tiền tố để phản ánh |`helloworl`| 
| Văn bản được thêm vào |`lrowolleh`| 
| Kết quả |`helloworldlrowolleh`| 

Hậu tố`d`đã đối xứng nên chín ký tự đầu tiên phải được phản ánh. Văn bản được thêm vào là đảo ngược của`helloworl`, tạo ra bảng màu cần thiết. 

Đối với mẫu 2,`s = anitalavalatina`. Toàn bộ chuỗi đã là một chuỗi palindrome, do đó chuỗi hoàn chỉnh khớp với chuỗi đảo ngược của nó. 

| Biến | Tiểu bang | 
| --- | --- | 
|`s`|`anitalavalatina`| 
|`r`|`anitalavalatina`| 
| Hậu tố palindromic dài nhất |`anitalavalatina`| 
|`k`|`16`| 
| Tiền tố để phản ánh | trống | 
| Văn bản được thêm vào | trống | 
| Kết quả |`anitalavalatina`| 

Ở đây sự chồng chéo KMP đạt đến toàn bộ chiều dài của chuỗi. Do đó, không còn ký tự nào để phản chiếu, đây chính xác là hành vi mong muốn đối với đầu vào đã có màu nhạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuỗi kết hợp có độ dài`2n + 1`và KMP xử lý mọi vị trí với công việc được khấu hao không đổi. | 
| Không gian | O(n) | Chuỗi đảo ngược, chuỗi kết hợp và mảng hàm tiền tố đều yêu cầu không gian tuyến tính. | 

Với`n ≤ 5000`, chuỗi kết hợp chứa tối đa 10001 ký tự. Do đó, việc tính toán hàm tiền tố chỉ thực hiện một số lượng thao tác tuyến tính nhỏ, thoải mái trong giới hạn 2 giây và thấp hơn nhiều so với giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_string(s):
    r = s[::-1]
    t = r + '#' + s

    pi = [0] * len(t)

    for i in range(1, len(t)):
        j = pi[i - 1]

        while j > 0 and t[i] != t[j]:
            j = pi[j - 1]

        if t[i] == t[j]:
            j += 1

        pi[i] = j

    k = pi[-1]
    return s + s[:len(s) - k][::-1]

def run(inp: str) -> str:
    return solve_string(inp.strip())

assert run("helloworld") == "helloworldlrowolleh", "sample 1"
assert run("anitalavalatina") == "anitalavalatina", "sample 2"

assert run("a") == "a", "minimum-size input"
assert run("aaaaaa") == "aaaaaa", "all-equal characters"
assert run("aace") == "aacecaa", "single-character palindromic suffix"
assert run("abac") == "abacaba", "nontrivial boundary overlap"

max_input = "a" * 4999 + "b"
max_expected = max_input + "a" * 4999
assert run(max_input) == max_expected, "maximum-size input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Độ dài tối thiểu và đầu vào đã có palindromic | 
|`aaaaaa`|`aaaaaa`| Toàn bộ chuỗi là một palindrome và có khoảng chồng chéo dài nhất`n`| 
|`aace`|`aacecaa`| Chỉ có thể giữ lại hậu tố một ký tự, loại bỏ những phần chèn thêm không cần thiết | 
|`abac`|`abacaba`| Phát hiện hậu tố palindromic không cần thiết và đảo ngược tiền tố | 
|`aaaa...aaab`có chiều dài 5000 |`aaaa...aaabaaaa...aaa`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào một ký tự`a`, chuỗi đảo ngược cũng là`a`. Trận đấu tiền tố-hậu tố dài nhất có độ dài`1`, Vì thế`k = n`. Tiền tố`s[:0]`trống và thuật toán trả về`a`mà không thêm bất cứ điều gì. 

Đối với một đầu vào đã có palindromic như`anitalavalatina`, chuỗi đảo ngược giống hệt chuỗi gốc. KMP phát hiện ra sự chồng chéo của toàn bộ chiều dài, vì vậy`k = n`. Điều này làm cho tiền tố được nối thêm trống và ngăn ngừa lỗi phổ biến khi thêm một bản sao không cần thiết của chuỗi. 

Vì`aace`, hậu tố`e`là palindromic, trong khi các hậu tố dài hơn thì không. KMP tìm thấy`k = 1`. Tiền tố`aac`được đảo ngược thành`caa`, cho`aacecaa`. Nhân vật trung tâm`e`được so khớp với chính nó, trong khi tiền tố ban đầu được phản ánh xung quanh nó. 

Vì`abac`, hậu tố palindromic dài nhất là`c`, Vì thế`k = 1`. Thuật toán lấy tiền tố`aba`, đảo ngược nó thành`aba`, và tạo ra`abacaba`. Trường hợp này rất hữu ích vì bản thân tiền tố là một bảng màu nhưng toàn bộ dữ liệu đầu vào thì không. Việc triển khai chỉ đảo ngược toàn bộ dữ liệu đầu vào sẽ thêm quá nhiều ký tự. 

Đối với đầu vào có độ dài tối đa bao gồm 4999 bản sao của`a`theo sau là`b`, trận chung kết`b`là hậu tố palindromic dài nhất. KMP vẫn chỉ xử lý`2n + 1`các ký tự trong chuỗi kết hợp. Thuật toán nối thêm 4999 bản sao của`a`, tạo ra một palindrome có độ dài 9999. Điều này thực hiện đầu vào lớn nhất được phép đồng thời kiểm tra xem việc triển khai có xử lý một hậu tố palindromic rất nhỏ mà không cần quét lặp đi lặp lại các hậu tố ứng cử viên hay không.
