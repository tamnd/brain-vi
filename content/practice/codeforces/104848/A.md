---
title: "CF 104848A - Một sửa đổi không phải Palindromic"
description: "Chúng tôi được cung cấp một mảng số nguyên. Chúng ta phải tăng chính xác một phần tử lên 1 và sau đó kiểm tra xem mảng kết quả có phải là một bảng màu hay không. Nhiệm vụ là quyết định xem có tồn tại sự lựa chọn vị trí như vậy hay không. Một mảng có tính chất palindromic khi mọi phần tử khớp với phần tử được phản chiếu của nó."
date: "2026-06-28T11:18:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104848
codeforces_index: "A"
codeforces_contest_name: "2021-2022 ICPC, Moscow Subregional"
rating: 0
weight: 104848
solve_time_s: 50
verified: true
draft: false
---

[CF 104848A - Sửa đổi không phải Palindromic](https://codeforces.com/problemset/problem/104848/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng số nguyên. Chúng ta phải tăng chính xác một phần tử lên 1 rồi kiểm tra xem mảng kết quả có **không** là một bảng màu hay không. Nhiệm vụ là quyết định xem có tồn tại sự lựa chọn vị trí như vậy hay không. 

Một mảng có tính chất palindromic khi mọi phần tử khớp với phần tử được phản chiếu của nó. Việc tăng một giá trị chỉ thay đổi một vị trí, do đó chỉ đẳng thức liên quan đến vị trí đó và gương của nó mới có thể thay đổi. 

Độ dài mảng tối đa là 1000. Ngay cả một thuật toán thử mọi vị trí và kiểm tra toàn bộ mảng mỗi lần cũng thực hiện tối đa khoảng một triệu so sánh phần tử, dễ dàng nằm trong giới hạn. Không cần cấu trúc dữ liệu phức tạp hoặc tiền xử lý. 

Một số trường hợp đáng chú ý. 

Hãy xem xét một mảng không phải là một bảng màu.```
3
1
2
3
```Đầu ra đúng là:```
1
```Bất kể phần tử nào được tăng lên, mảng không thể đột nhiên trở thành palindromic. Giải pháp bất cẩn chỉ kiểm tra mảng palindromic sẽ trả lời sai`0`. 

Mảng nhỏ nhất có thể cũng cần xử lý đặc biệt.```
1
7
```Đầu ra đúng là:```
0
```Sau khi tăng phần tử duy nhất, mảng vẫn chứa một phần tử và mỗi độ dài một mảng là một bảng màu. 

Các palindrome có độ dài lẻ cần được chăm sóc cẩn thận vì phần tử ở giữa không có đối tác phản chiếu riêng biệt.```
3
1
2
1
```Đầu ra đúng là:```
1
```Tăng phần tử ở giữa tạo ra`[1, 3, 1]`, vẫn là một palindrome. Bước đi đúng là tăng một trong hai đầu, tạo ra`[2, 2, 1]`hoặc`[1, 2, 2]`, cả hai đều không phải là palindromic. Giả sử mọi vị trí đều hoạt động sẽ không chính xác. 

Một mảng có các phần tử giống hệt nhau là một ví dụ hữu ích khác.```
4
5
5
5
5
```Đầu ra đúng là:```
1
```Việc tăng bất kỳ vị trí nào sẽ ngay lập tức phá vỡ sự bình đẳng với phần tử được phản chiếu của nó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi tư thế có thể. Đối với mỗi vị trí, hãy tạm thời tăng phần tử đó lên một, kiểm tra xem mảng kết quả có phải là một bảng màu hay không, sau đó khôi phục giá trị ban đầu. Nếu bất kỳ nỗ lực nào tạo ra một mảng không phải palindromic, câu trả lời là`1`. Nếu không thì câu trả lời là`0`. 

Phương pháp này đúng vì nó kiểm tra rõ ràng mọi sửa đổi pháp lý. Thời gian chạy của nó là`O(n^2)`, vì có`n`các vị trí ứng cử viên và mỗi lần kiểm tra palindrome sẽ quét mảng một lần. Với`n ≤ 1000`, nhiều nhất là khoảng một triệu so sánh, đủ nhanh. 

Nhìn kỹ hơn vào tác động của việc tăng một yếu tố sẽ thấy một quan sát thậm chí còn đơn giản hơn. 

Nếu mảng ban đầu không phải là một bảng màu, việc tăng thêm một phần tử không thể sửa chữa được mọi cặp không khớp. Vị trí đã thay đổi chỉ tham gia vào một so sánh được phản ánh, vì vậy mọi sự không khớp khác vẫn còn. Câu trả lời là ngay lập tức`1`. 

Nếu mảng ban đầu là một palindrome thì chỉ có hai trường hợp có thể xảy ra. 

Nếu như`n = 1`, phần tử đơn cũng là phần tử ở giữa, do đó việc thay đổi nó sẽ để lại mảng đối xứng. 

Nếu như`n > 1`, việc chọn bất kỳ vị trí nào không phải là phần tử ở giữa duy nhất sẽ thay đổi một mặt của cặp được phản chiếu trong khi vẫn giữ nguyên mặt kia. Vì các giá trị chỉ tăng lên nên đẳng thức bị phá vỡ ngay lập tức, tạo ra một mảng không palindromic. 

Điều này làm giảm vấn đề kiểm tra xem mảng ban đầu có phải là palindromic hay không và độ dài của nó có bằng một hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Kiểm tra xem mảng có phải là một bảng màu hay không bằng cách so sánh mọi phần tử với phần tử đối xứng của nó. 
3. Nếu mảng chưa phải là một bảng màu, hãy in`1`. Bất kỳ sự không khớp hiện có nào không liên quan đến vị trí đã sửa đổi vẫn không thay đổi, do đó mảng không thể trở thành đối xứng. 
4. Nếu mảng là một palindrome và độ dài của nó là`1`, in`0`. Chỉ có một phần tử nên mọi sửa đổi có thể vẫn tạo ra một bảng màu. 
5. Nếu không, hãy in`1`. Vì độ dài lớn hơn một nên hãy chọn bất kỳ vị trí nào không phải là tâm của mảng có độ dài lẻ. Việc tăng giá trị đó sẽ phá vỡ sự bình đẳng với gương của nó, làm cho mảng không có giá trị palindromic. 

### Tại sao nó hoạt động 

Thuộc tính chính là việc tăng một phần tử chỉ ảnh hưởng đến một cặp được nhân đôi. Nếu mảng ban đầu đã chứa một phần không khớp, thì ít nhất một phần không khớp vẫn tồn tại sau mọi sửa đổi có thể, do đó mảng vẫn không phải là palindromic. Nếu mảng ban đầu là một palindrome và có nhiều hơn một phần tử thì luôn tồn tại một vị trí có gương riêng biệt. Việc tăng chính xác một cạnh của cặp đó sẽ làm cho hai giá trị khác nhau, phá hủy bảng màu. Trường hợp không thể duy nhất là một mảng có độ dài bằng một, phần tử duy nhất phản chiếu chính nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = [int(input()) for _ in range(n)]

pal = True
for i in range(n // 2):
    if a[i] != a[n - 1 - i]:
        pal = False
        break

if not pal:
    print(1)
elif n == 1:
    print(0)
else:
    print(1)
```Đầu tiên, chương trình đọc mảng và thực hiện kiểm tra bảng màu tiêu chuẩn bằng cách so sánh các vị trí đối xứng. Vòng lặp chỉ cần kiểm tra nửa đầu vì mọi so sánh đều bao gồm một cặp được phản chiếu. 

Khi đã biết trạng thái palindrome, logic còn lại sẽ được rút ra trực tiếp từ chứng minh. Một mảng không palindromic ngay lập tức mang lại`1`. Độ dài một palindrome là trường hợp duy nhất không thể xảy ra và mang lại`0`. Mọi trường hợp khác là một palindrome có ít nhất một cặp vị trí riêng biệt được phản chiếu, do đó việc tăng một điểm cuối sẽ phá vỡ sự bình đẳng đó. 

Việc thực hiện tránh được một lỗi bằng cách sử dụng`n - 1 - i`như chỉ số được nhân đôi. Đối với độ dài lẻ, phần tử ở giữa không bao giờ được so sánh với chính nó vì vòng lặp dừng sau`n // 2`lần lặp lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1
2
3
```| Bước | tôi | Giá trị so sánh | Palindrome cho đến nay | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | Đúng | 
| So sánh | 0 | 1 đấu 3 | Sai | 

Mảng này đã không phải là palindromic nên thuật toán sẽ ngay lập tức đưa ra`1`. Ví dụ này minh họa rằng không cần phải suy luận thêm về vị trí cần sửa đổi. 

### Ví dụ 2 

đầu vào:```
3
1
2
1
```| Bước | tôi | Giá trị so sánh | Palindrome cho đến nay | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | Đúng | 
| So sánh | 0 | 1 đấu 1 | Đúng | 
| Quyết định | - | Chiều dài > 1 | Đầu ra 1 | 

Mảng bắt đầu như một palindrome. Vì chiều dài của nó vượt quá một, nên việc tăng một trong hai đầu sẽ phá vỡ một cặp được nhân đôi, tạo ra một mảng không palindromic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét so sánh các cặp được nhân đôi. | 
| Không gian | O(1) | Chỉ có một vài biến bổ sung được sử dụng ngoài mảng đầu vào. | 

Thuật toán thực hiện một lần truyền tuyến tính duy nhất qua mảng, thấp hơn nhiều so với giới hạn cho`n = 1000`. Việc sử dụng bộ nhớ bổ sung liên tục của nó cũng đáp ứng thoải mái giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    a = [int(input()) for _ in range(n)]

    pal = True
    for i in range(n // 2):
        if a[i] != a[n - 1 - i]:
            pal = False
            break

    if not pal:
        print(1)
    elif n == 1:
        print(0)
    else:
        print(1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# custom cases
assert run("1\n5\n") == "0\n", "single element"
assert run("2\n1\n1\n") == "1\n", "two equal elements"
assert run("3\n1\n2\n3\n") == "1\n", "already non-palindromic"
assert run("4\n7\n7\n7\n7\n") == "1\n", "all equal"
assert run("5\n1\n2\n3\n2\n1\n") == "1\n", "odd palindrome"
assert run("1000\n" + "1\n" * 1000) == "1\n", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1, [5]`|`0`| Kích thước tối thiểu, trường hợp không thể | 
|`2, [1,1]`|`1`| Palindrome không tầm thường nhỏ nhất | 
|`3, [1,2,3]`|`1`| Đã có mảng không palindromic | 
|`4, [7,7,7,7]`|`1`| Tất cả các giá trị bằng nhau | 
|`5, [1,2,3,2,1]`|`1`| Bảng màu có độ dài lẻ với phần tử ở giữa | 
|`1000`giá trị giống nhau |`1`| Đầu vào được phép lớn nhất | 

## Vỏ cạnh 

Trường hợp phần tử đơn là đầu vào duy nhất có câu trả lời`0`.```
1
7
```Việc kiểm tra palindrome thành công vì không có cặp đối xứng nào để so sánh. Thuật toán sau đó phát hiện ra rằng`n == 1`và in`0`. Việc thay đổi phần tử duy nhất không thể tạo ra một mảng không phải palindrome vì mỗi độ dài của một mảng đều là một palindrome. 

Hãy xem xét một mảng đã không phải là palindromic.```
3
1
2
3
```Sự so sánh đầu tiên cho thấy`1 != 3`, do đó cờ palindrome trở thành sai. Thuật toán in ngay lập tức`1`. Những điểm không khớp hiện tại không thể biến mất hoàn toàn sau khi chỉ tăng một phần tử. 

Bây giờ hãy xem xét một palindrome có độ dài lẻ.```
3
1
2
1
```Việc kiểm tra palindrome thành công. Vì độ dài lớn hơn một nên thuật toán in`1`. Mặc dù việc sửa đổi tâm giữ cho mảng có màu nhạt, nhưng việc sửa đổi một trong hai đầu sẽ phá vỡ cặp phản chiếu duy nhất, do đó, một nước đi hợp lệ luôn tồn tại. 

Cuối cùng, hãy xem xét một bảng màu có độ dài chẵn với các giá trị giống nhau.```
4
5
5
5
5
```Kiểm tra palindrome thành công và độ dài vượt quá một. Thuật toán in`1`. Việc tăng bất kỳ vị trí nào sẽ thay đổi một mặt của cặp được phản chiếu trong khi mặt đối diện vẫn giữ nguyên`5`, ngay lập tức phá hủy palindrome.
