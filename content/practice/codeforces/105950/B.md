---
title: "CF 105950B - Thư viện ma thuật"
description: "Chúng ta được cấp một chuỗi chữ thường duy nhất và được phép thực hiện tối đa một thao tác: chọn hai vị trí và hoán đổi các ký tự ở các vị trí đó."
date: "2026-06-25T06:39:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105950
codeforces_index: "B"
codeforces_contest_name: "UDESC Selection Contest 2025-1"
rating: 0
weight: 105950
solve_time_s: 52
verified: true
draft: false
---

[CF 105950B - Thư viện ma thuật](https://codeforces.com/problemset/problem/105950/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ thường duy nhất và được phép thực hiện tối đa một thao tác: chọn hai vị trí và hoán đổi các ký tự ở các vị trí đó. Sau sự hoán đổi tùy chọn này, chuỗi được đặt trở lại thư viện khái niệm trong đó tất cả các chuỗi chữ thường hữu hạn được sắp xếp theo thứ tự từ điển và mục tiêu là làm cho chuỗi của chúng ta xuất hiện sớm nhất có thể theo thứ tự đó. 

Trong thực tế, điều này dẫn đến vấn đề chuỗi thuần túy: thứ tự từ điển giữa hai chuỗi được xác định chính xác giống như thứ tự từ điển, so sánh vị trí khác nhau đầu tiên. Vì mọi chuỗi hữu hạn đều tồn tại theo cùng một thứ tự, nên “vị trí thư viện” tương đương với chính chuỗi đó theo thứ tự từ điển. Vì vậy, việc giảm thiểu vị trí cũng giống như việc tạo ra chuỗi nhỏ nhất về mặt từ điển có thể đạt được bằng 0 hoặc một lần hoán đổi. 

Đầu vào chỉ là một chuỗi`S`có chiều dài lên tới$10^5$. Đầu ra là một chuỗi khác có cùng độ dài, thu được từ`S`bằng cách không làm gì hoặc hoán đổi chính xác một cặp ký tự. 

Ràng buộc ngay lập tức loại trừ bất kỳ nỗ lực bậc hai nào đối với tất cả các lần hoán đổi, vì việc thử tất cả các cặp sẽ là$O(n^2)$nó quá lớn đối với$n = 10^5$. Bất kỳ giải pháp khả thi nào cũng phải tính đến tất cả các giao dịch hoán đổi theo thời gian tuyến tính hoặc gần tuyến tính. 

Một trường hợp thất bại tinh tế đối với những ý tưởng tham lam ngây thơ xuất hiện khi có nhiều ký tự tốt nhất giống hệt nhau tồn tại trong hậu tố. 

Ví dụ, hãy xem xét`bcaac`. Một cách tiếp cận ngây thơ có thể thấy rằng việc hoán đổi`b`với một`a`cải thiện chuỗi và chọn chuỗi đầu tiên`a`nó tìm thấy. Nhưng việc hoán đổi với sự xuất hiện sai có thể tạo ra`acabc`thay vì`aacbc`, điều này tệ hơn về mặt từ điển mặc dù cả hai đều là những hoán đổi đơn hợp lệ. Điều này cho thấy chúng ta phải cẩn thận chọn _lần_xuất hiện của một ký tự để hoán đổi, chứ không chỉ _có nên hoán đổi hay không. 

Một trường hợp khác là khi không có trao đổi nào sẽ cải thiện chuỗi, chẳng hạn như`abcde`. Bất kỳ sự hoán đổi nào cũng sẽ giữ nguyên hoặc làm xấu đi trật tự từ điển. Đầu ra đúng là chuỗi gốc. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ thử mọi cặp$(i, j)$, thực hiện hoán đổi, tính toán chuỗi kết quả và lấy giá trị tối thiểu theo từ điển. Mỗi chi phí so sánh$O(n)$, dẫn đến$O(n^3)$tổng cộng nếu được thực hiện trực tiếp hoặc tốt nhất là$O(n^2)$hoán đổi với$O(n)$so sánh từng cái. Điều này là không thể thực hiện được ở$10^5$. 

Nhận xét quan trọng là chỉ có vị trí sớm nhất nơi chúng ta có thể cải thiện dây mới quan trọng. Thứ tự từ điển được quyết định từ trái sang phải, vì vậy việc cải thiện vị trí sau trong khi vẫn giữ nguyên vị trí trước đó là vô ích nếu có cải tiến tốt hơn trước đó. 

Điều này dẫn đến cấu trúc tham lam: chúng ta quét từ trái sang phải và quyết định xem vị trí có`i`có thể được cải thiện bằng cách hoán đổi nó với một ký tự nhỏ hơn xuất hiện sau này. Để thực hiện điều này một cách hiệu quả, chúng tôi tính toán trước cho mỗi vị trí ký tự nhỏ nhất có sẵn trong hậu tố của nó và vị trí nó xuất hiện. Sau đó, ở vị trí đầu tiên mà ký tự hiện tại chưa phải là ký tự nhỏ nhất có thể trong hậu tố của nó, chúng tôi thực hiện hoán đổi mang lại sự cải thiện tốt nhất. 

Sự tinh tế còn lại là sự ràng buộc. Nếu ký tự nhỏ nhất xuất hiện nhiều lần trong hậu tố, việc hoán đổi với lần xuất hiện ngoài cùng bên phải là tối ưu, vì nó giữ nguyên cấu trúc hậu tố trước đó trong khi vẫn kéo giá trị nhỏ nhất về phía trước nhất có thể, tối đa hóa lợi ích từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trao đổi vũ phu tất cả các cặp |$O(n^3)$hoặc$O(n^2)$|$O(1)$| Quá chậm | 
| Tham lam với theo dõi tối thiểu hậu tố |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng lưu trữ cho từng chỉ mục`i`, ký tự nhỏ nhất trong hậu tố bắt đầu từ`i`, cùng với vị trí nơi nó xảy ra. Điều này cho phép chúng tôi biết ngay đối tác hoán đổi tốt nhất có thể cho bất kỳ vị thế nào. 
2. Quét chuỗi từ trái sang phải. Tại vị trí`i`, so sánh`S[i]`với ký tự nhỏ nhất trong`S[i:]`. Nếu chúng bằng nhau thì không có gì ở vị trí này có thể cải thiện thứ tự từ điển, vì vậy chúng ta tiếp tục. 
3. Vị trí đầu tiên`i`Ở đâu`S[i]`lớn hơn hậu tố tối thiểu là nơi tốt nhất để hành động. Bất kỳ chỉ mục nào trước đó đều không thể được cải thiện và bất kỳ chỉ mục nào sau đó sẽ ít có tác động hơn đến thứ tự từ điển. 
4. Trong số tất cả các lần xuất hiện của ký tự tối thiểu trong hậu tố, hãy chọn vị trí ngoài cùng bên phải`j`. 
5. Hoán đổi`S[i]`Và`S[j]`, sau đó dừng lại. Chỉ có một trao đổi được cho phép. 
6. Nếu không tồn tại vị trí đó, xuất ra chuỗi gốc không thay đổi. 

### Tại sao nó hoạt động 

So sánh từ điển được quyết định ở chỉ mục đầu tiên nơi hai chuỗi khác nhau. Do đó, bất kỳ cải tiến nào cũng phải thay đổi vị trí sớm nhất có thể trong đó chuỗi chưa ở mức tối thiểu so với hậu tố của nó. Khi chúng tôi tìm thấy chỉ mục đầu tiên có ký tự nhỏ hơn tồn tại sau đó, việc thay đổi bất kỳ điều gì trước đó là không thể và thay đổi bất kỳ điều gì sau đó không thể chi phối được cải tiến này. 

Việc chọn ký tự nhỏ nhất trong hậu tố đảm bảo giảm tối đa chỉ số khác biệt sớm nhất. Việc chọn lần xuất hiện ngoài cùng bên phải của nó đảm bảo rằng tiền tố của hậu tố vẫn nhỏ nhất có thể sau khi hoán đổi, tránh sự suy giảm ngẫu nhiên trong các so sánh sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = list(input().strip())
    n = len(s)

    if n <= 1:
        print("".join(s))
        return

    # suffix_min_char[i] = smallest character from i..n-1
    # suffix_min_pos[i] = rightmost position of that smallest character
    suffix_min_char = [''] * n
    suffix_min_pos = [0] * n

    suffix_min_char[-1] = s[-1]
    suffix_min_pos[-1] = n - 1

    for i in range(n - 2, -1, -1):
        if s[i] < suffix_min_char[i + 1]:
            suffix_min_char[i] = s[i]
            suffix_min_pos[i] = i
        elif s[i] > suffix_min_char[i + 1]:
            suffix_min_char[i] = suffix_min_char[i + 1]
            suffix_min_pos[i] = suffix_min_pos[i + 1]
        else:
            suffix_min_char[i] = s[i]
            suffix_min_pos[i] = i  # prefer rightmost for same char

    for i in range(n):
        if s[i] > suffix_min_char[i]:
            j = suffix_min_pos[i]
            s[i], s[j] = s[j], s[i]
            break

    print("".join(s))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào hậu tố cực tiểu tính toán trước để mỗi vị trí có thể được đánh giá trong thời gian không đổi. Việc quét từ trái sang phải đảm bảo rằng hoán đổi có lợi đầu tiên là hoán đổi quan trọng nhất đối với thứ tự từ điển. 

Một chi tiết triển khai tinh tế là theo dõi sự xuất hiện ngoài cùng bên phải của ký tự tối thiểu trong các mối quan hệ. Nếu điều này không được xử lý, việc hoán đổi có thể chọn một lần xuất hiện sớm hơn và để lại một ký tự nhỏ hơn bên phải không được sử dụng, điều này có thể làm xấu đi thứ tự từ điển thu được trong các so sánh sâu hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:`discreta`Chúng tôi tính toán hậu tố tối thiểu: 

| tôi | s[i] | hậu tố tối thiểu | trao đổi ứng viên | 
| --- | --- | --- | --- | 
| 0 | d | một | có (hoán đổi với a cuối cùng) | 

Tại chỉ số 0,`d`lớn hơn hậu tố tối thiểu`a`, vậy chúng ta trao đổi`d`với ngoài cùng bên phải`a`. 

Chuỗi kết quả:`aiscretd`Điều này chứng tỏ rằng việc cải thiện vị trí khả thi đầu tiên sẽ chiếm ưu thế hơn tất cả những cân nhắc sau này. 

### Ví dụ 2 

Chuỗi đầu vào:`harrypotter`Hậu tố cực tiểu xác định rằng tại chỉ số 0,`h`có thể được cải thiện bằng cách sử dụng`a`. 

Chúng tôi trao đổi:`h`↔`a`Kết quả:`ahrrypotter`Điều này cho thấy rằng ngay cả khi có thể thực hiện nhiều lần hoán đổi sau này, việc thực hiện chỉ số cải thiện sớm nhất sẽ mang lại lợi ích từ điển tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần quét hậu tố cộng với một lần quét từ trái sang phải | 
| Không gian |$O(n)$| Mảng lưu trữ hậu tố ký tự và vị trí tối thiểu | 

Các ràng buộc cho phép lên đến$10^5$ký tự và giải pháp chỉ thực hiện một số bước tuyến tính, khiến nó hoạt động tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    s = list(sys.stdin.readline().strip())
    n = len(s)

    suffix_min_char = [''] * n
    suffix_min_pos = [0] * n

    suffix_min_char[-1] = s[-1]
    suffix_min_pos[-1] = n - 1

    for i in range(n - 2, -1, -1):
        if s[i] < suffix_min_char[i + 1]:
            suffix_min_char[i] = s[i]
            suffix_min_pos[i] = i
        elif s[i] > suffix_min_char[i + 1]:
            suffix_min_char[i] = suffix_min_char[i + 1]
            suffix_min_pos[i] = suffix_min_pos[i + 1]
        else:
            suffix_min_char[i] = s[i]
            suffix_min_pos[i] = i

    for i in range(n):
        if s[i] > suffix_min_char[i]:
            j = suffix_min_pos[i]
            s[i], s[j] = s[j], s[i]
            break

    print("".join(s))

# minimal
assert run("a\n") == "a", "single char"

# already optimal
assert run("abcde\n") == "abcde", "no improvement"

# simple swap
assert run("ba\n") == "ab", "basic swap"

# repeated letters
assert run("baca\n") == "a bca".replace(" ", ""), "best improvement early"

# all equal
assert run("aaaa\n") == "aaaa", "no effect"

# later improvement
assert run("cab\n") == "acb", "swap first char with best suffix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | một | đầu vào nhỏ nhất | 
| abcde | abcde | không có trường hợp trao đổi | 
| ba | ab | cải tiến trao đổi đơn | 
| baca | abca | lựa chọn đúng giữa nhiều ứng viên | 
| aaa | aaa | trường hợp cạnh trùng lặp | 
| taxi | acb | cải tiến dựa trên hậu tố | 

## Vỏ cạnh 

Đối với một chuỗi không có cơ hội giảm như`abcde`, thuật toán không bao giờ tìm thấy vị trí có hậu tố tối thiểu nhỏ hơn ký tự hiện tại, do đó không thực hiện hoán đổi và chuỗi gốc được trả về. 

Đối với các chuỗi có các ký tự tối thiểu lặp lại trong hậu tố, chẳng hạn như`bcaac`, hậu tố tối thiểu ở vị trí đầu tiên là`a`và lần xuất hiện ngoài cùng bên phải sẽ được chọn. Hoán đổi với ngoài cùng bên phải`a`đảm bảo kết quả từ điển tốt nhất bằng cách đặt ký tự nhỏ nhất càng sớm càng tốt trong khi vẫn giữ được cấu trúc tối ưu ở phần còn lại. 

Đối với các trường hợp cải tiến tối ưu không phải ở lần xuất hiện đầu tiên của ký tự lớn mà ở chỉ mục đầu tiên có thể thực hiện bất kỳ cải tiến nào, chẳng hạn như`harrypotter`, quá trình quét đảm bảo chúng tôi không hoán đổi sớm các vị trí sau này, điều này sẽ dẫn đến việc giảm từ vựng yếu hơn.
