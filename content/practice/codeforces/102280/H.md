---
title: "CF 102280H - \u0417\u0430\u0434\u0430\u0447\u0430 \u0428\u0443\u043c\u0430\u0445\u0435\u0440\u0430"
description: "Chúng ta có hai số nguyên tố, (p) và (q), và cần quyết định xem [ (p+1)^q ] có phải là số chính phương hay không. Các số có thể chứa tới (1000) chữ số thập phân, vì vậy chúng vượt xa các loại số nguyên máy thông thường."
date: "2026-08-13T09:51:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102280
codeforces_index: "H"
codeforces_contest_name: "2010, \u0422\u0440\u0435\u043d\u0438\u0440\u043e\u0432\u043a\u0430 \u0421\u0413\u0410\u0423 aka \u041a\u043e\u043d\u0442\u0435\u0441\u0442 \u043f\u0440\u043e \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u043a\u0438"
rating: 0
weight: 102280
solve_time_s: 169
verified: true
draft: false
---

[CF 102280H - \u0417\u0430\u0434\u0430\u0447\u0430 \u0428\u0443\u043c\u0430\u0445\u0435\u0440\u0430](https://codeforces.com/problemset/problem/102280/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên tố (p) và (q) và cần phải quyết định xem 

[ 
(p+1)^q 
] 

là một hình vuông hoàn hảo Các số có thể chứa tới (1000) chữ số thập phân, vì vậy chúng vượt xa các loại số nguyên máy thông thường. Đầu ra cần thiết là`YES`khi biểu thức là bình phương của một số tự nhiên và`NO`nếu không thì. 

Thuộc tính quyết định là số mũ (q). Vì (q) là số nguyên tố nên nó là (2) hoặc số nguyên tố lẻ. Khi (q=2), biểu thức đơn giản là ((p+1)^2), luôn là một hình vuông hoàn hảo. Khi (q) là số lẻ, việc nâng một số lên lũy thừa thứ (q) không thay đổi cho dù mọi số mũ nguyên tố trong hệ số của nó đều là số chẵn. Do đó ((p+1)^q) là một hình vuông khi bản thân (p+1) là một hình vuông. 

Câu hỏi còn lại đơn giản hơn nhiều vì (p) cũng là số nguyên tố. Giả sử (p+1=k^2). Sau đó 

[ 
p=k^2-1=(k-1)(k+1). 
] 

Vì (p) là số nguyên tố nên tích này chỉ có thể có một thừa số bằng (1). Do đó (k-1=1), cho (k=2) và (p=3). Vì vậy, với mọi số nguyên tố lẻ (q), câu trả lời là`YES`chính xác khi nào (p=3). 

Giới hạn trên rất lớn của (10^{1000}) thay đổi hoàn toàn chiến lược triển khai. Chúng tôi không thể chuyển đổi đầu vào thành số nguyên 64 bit và việc xây dựng ((p+1)^q) là vô vọng vì số chữ số của nó sẽ rất lớn. Ngay cả một phép thử bình phương hoàn hảo chung về giá trị đó cũng sẽ yêu cầu thao tác một số nguyên lớn về mặt thiên văn. Việc rút gọn toán học cho phép chúng ta tránh được mọi phép tính số học có hàm lũy thừa rất lớn. Chúng ta chỉ cần so sánh các chuỗi đầu vào thập phân với`2`Và`3`, mất thời gian tỷ lệ thuận với độ dài đầu vào. 

Có một số trường hợp ranh giới có thể đánh lừa việc triển khai chỉ dựa trên đại số của câu lệnh. Ví dụ, với```
3
2
```câu trả lời là`YES`, bởi vì ((3+1)^2=16). Một giải pháp kiểm tra xem (p+1) có phải là số bình phương hay không trước tiên sẽ bác bỏ trường hợp này một cách không chính xác, vì số mũ đặc biệt (q=2) làm cho mọi cơ số đều đúng. 

Vì```
2
3
```câu trả lời là`NO`. Ở đây (q) là số lẻ, vì vậy (p+1=3) bản thân nó phải là một hình vuông, nhưng thực tế không phải vậy. Ở đây, một giải pháp coi mọi số mũ lẻ là tạo ra một hình vuông sẽ thất bại. 

Vì```
3
3
```câu trả lời là`YES`, bởi vì (p+1=4) là một hình vuông và lũy thừa lẻ của hình vuông vẫn là hình vuông. Trường hợp này xác minh nhánh thứ hai của đặc tính. 

Câu lệnh nói rằng các đầu vào là số nguyên tố, vì vậy các giá trị như`0`Và`1`nằm ngoài miền toán học hợp lệ mặc dù xuất hiện trong giới hạn số. Giải pháp dựa trên điều kiện nguyên tố và không cần xử lý các giá trị đó như các ca kiểm thử hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ xây dựng ((p+1)^q) và kiểm tra xem kết quả có phải là hình vuông hay không. Điều này đúng về mặt toán học nhưng nó hoàn toàn bỏ qua cấu trúc của bài toán. Với đầu vào có tối đa (1000) chữ số, (q) có thể theo thứ tự (10^{1000}). Ngay cả việc thực hiện phép nhân (q) về mặt khái niệm cũng là khoảng (10^{1000}) phép toán và số kết quả có số chữ số vô cùng lớn. Cách tiếp cận như vậy không chỉ đơn thuần là quá chậm so với giới hạn (0,5) giây mà còn không khả thi. 

Một phương pháp ít trực tiếp hơn một chút có thể cố gắng xác định xem (p+1) có phải là bình phương hay không bằng cách liệt kê các nghiệm có thể. Trong trường hợp xấu nhất (p) nằm ở khoảng (10^{1000}), do đó nghiệm nằm ở khoảng (10^{500}), cho ra khoảng (10^{500}) ứng cử viên. Điều đó cũng không thể được. 

Quan sát hữu ích là số mũ là số nguyên tố. Mọi số nguyên tố đều là (2) hoặc lẻ. Trường hợp (q=2) ngay lập tức cho một hình vuông. Đối với số lẻ (q), hãy xem xét việc phân tích thành thừa số nguyên tố của (p+1). Nếu thừa số nguyên tố xảy ra với số mũ (e), thì nó xảy ra ở ((p+1)^q) với số mũ (eq). Vì (q) là số lẻ nên (eq) là số chẵn khi (e) là số chẵn. Do đó, lũy thừa là bình phương khi (p+1) đã là bình phương. 

Bây giờ hãy sử dụng tính chất (p) là số nguyên tố. Nếu (p+1=k^2), thì (p=(k-1)(k+1)). Một số nguyên tố không thể được biểu diễn dưới dạng tích của hai số nguyên lớn hơn (1), do đó (k-1=1), điều này buộc (p=3). 

Do đó, toàn bộ vấn đề đã được giải quyết thành hai so sánh chuỗi. Nếu (q=2), in`YES`. Ngược lại, vì (q) là số nguyên tố lẻ nên in`YES`chính xác khi nào (p=3). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(q)) phép nhân, với kích thước kết quả rất lớn | Thiên văn | Quá chậm | 
| Tối ưu | (O( | p | + | q | )) | (O( | p | + | q | )) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (p) và (q) dưới dạng chuỗi thay vì chuyển đổi chúng thành số nguyên thông thường. Giá trị của chúng có thể chứa (1000) chữ số, trong khi thuật toán chỉ cần so sánh chúng với các hằng số nhỏ (2) và (3). 
2. Kiểm tra xem (q=2). Nếu có thì xuất ra`YES`ngay lập tức bởi vì 
[ 
(p+1)^2 
] 
là bình phương với mọi khả năng (p). 
3. Nếu (q\neq2) thì (q) là số nguyên tố lẻ. Đối với số mũ lẻ, ((p+1)^q) là số bình phương khi (p+1) là số bình phương. 
4. Giả sử (p+1=k^2). Vì (p) là số nguyên tố nên 
[ 
p=k^2-1=(k-1)(k+1). 
] 
Giá trị nguyên tố duy nhất có thể xảy ra tại (k=2), cho ra (p=3). 
5. Kiểm tra xem (p=3). Nếu vậy, xuất`YES`; nếu không thì xuất ra`NO`. 

Bất biến chính là sau nhánh đầu tiên, (q) được biết là số lẻ, do đó trạng thái bình phương của ((p+1)^q) chính xác là trạng thái bình phương của (p+1). Tính nguyên tố của (p) sau đó rút gọn điều kiện đó thành khả năng duy nhất (p=3). Mọi đầu vào hợp lệ có thể thuộc về một trong hai nhánh này, vì vậy không trường hợp nào khác có thể tạo ra`YES`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p = input().strip()
    q = input().strip()

    if q == "2" or p == "3":
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Đầu vào được giữ dưới dạng chuỗi vì việc chuyển đổi giá trị hàng nghìn chữ số thành số nguyên máy là không cần thiết đối với giải pháp này. Python cũng có thể xử lý các số nguyên lớn tùy ý, nhưng việc sử dụng chuỗi làm cho độ phức tạp mong muốn trở nên rõ ràng và tránh mọi phép nhân hoặc lũy thừa. 

Phép so sánh đầu tiên xử lý số mũ nguyên tố đặc biệt (2). Không cần điều kiện nào cho (p) vì toàn bộ biểu thức có dạng (x^2). 

Nếu phép so sánh đó không thành công thì (q) là số nguyên tố lẻ theo sự đảm bảo của bài toán. Trong trường hợp đó, giá trị duy nhất có thể có của (p) dẫn đến hình vuông là (3), do đó phép so sánh thứ hai là đủ. 

Không có lo ngại về tràn vì chương trình không bao giờ tính toán (p+1), không bao giờ tính lũy thừa và không bao giờ xây dựng căn bậc hai. Hoạt động lớn nhất là so sánh hai chuỗi có độ dài tối đa (1000). 

## Ví dụ đã hoạt động 

Phần mẫu được trích xuất của câu lệnh không bảo toàn các giá trị đầu vào thực tế, nhưng hai trường hợp cơ bản dự định có thể được trình bày bằng các ví dụ hợp lệ sau. 

### Ví dụ 1 

Hãy xem xét```
3
2
```Việc thực hiện là: 

| Bước | (p) | (q) | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | (q=2) |`YES`| 

Thuật toán dừng ở nhánh đầu tiên. Thật vậy, biểu thức là (4^2=16), nên kết quả là một hình vuông hoàn hảo. Ví dụ này chứng minh tại sao việc chỉ kiểm tra xem (p+1) có phải là bình phương hay không là không đủ như một chiến lược chung. 

### Ví dụ 2 

Hãy xem xét```
2
3
```Việc thực hiện là: 

| Bước | (p) | (q) | Quyết định | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | (q\neq2) | tiếp tục | 
| 2 | 2 | 3 | (p\neq3) |`NO`| 

Vì (q=3) là số lẻ nên cơ số (p+1=3) sẽ phải là hình vuông. Không phải vậy nên câu trả lời là`NO`. 

Đối với một dấu vết hữu ích khác, hãy xem xét```
3
3
```Ở đây phép so sánh đầu tiên thất bại vì (q\neq2), nhưng phép so sánh thứ hai thành công vì (p=3). Biểu thức cơ bản là (4^3=64=8^2), xác nhận rằng nhánh số mũ lẻ cũng đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | p | + | q | )) | Việc đọc và so sánh hai chuỗi thập phân chiếm ưu thế trong công việc. | 
| Không gian | (O( | p | + | q | )) | Các chuỗi đầu vào chiếm không gian tỷ lệ thuận với độ dài của chúng. | 

Cả hai đầu vào đều có tối đa (1000) chữ số, do đó thuật toán chỉ thực hiện một lượng công việc rất nhỏ so với giới hạn. Đặc biệt, nó không bao giờ xây dựng giá trị tiềm năng rất lớn ((p+1)^q), đó là lý do giải pháp phù hợp thoải mái với giới hạn (0,5) giây và (64) MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    p = input().strip()
    q = input().strip()

    if q == "2" or p == "3":
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue() if False else ""
    finally:
        sys.stdin = old_stdin
        input = old_input

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided-sample-style cases
assert run("3\n2\n") == "YES", "q = 2 always gives a square"
assert run("2\n3\n") == "NO", "odd q requires p + 1 to be a square"

# Minimum valid prime values
assert run("2\n2\n") == "YES", "both primes are 2"

# The unique p producing a square for odd q
assert run("3\n3\n") == "YES", "p = 3 works for every odd prime q"

# Large decimal inputs, q is odd and p is not 3
assert run("99999999999999999999999999999999999999999999999999\n3\n") == "NO", \
    "large p must not trigger big-integer arithmetic"

# Large q with q = 2 represented exactly by its small decimal form
assert run("99999999999999999999999999999999999999999999999999\n2\n") == "YES", \
    "q = 2 works for every prime p"
```Trình trợ giúp ở trên tạm thời thay thế đầu vào và đầu ra tiêu chuẩn sao cho giống nhau`solve`chức năng có thể được kiểm tra nhiều lần. Hai khẳng định đầu tiên bao gồm hai nhánh cơ bản. Cái thứ ba kiểm tra số nguyên tố hợp lệ nhỏ nhất (p) và (q). Số thứ tư xác minh giá trị đặc biệt (p=3) cho số mũ lẻ. Hai lần kiểm tra cuối cùng chứng minh rằng thuật toán không cố gắng thực hiện số học trên các giá trị hàng nghìn chữ số. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2`|`YES`| Số mũ đặc biệt (q=2) | 
|`2 3`|`NO`| Nhỏ nhất (p) với số mũ lẻ | 
|`2 2`|`YES`| Giá trị nguyên tố hợp lệ tối thiểu | 
|`3 3`|`YES`| Trường hợp duy nhất (p=3) cho số lẻ (q) | 
| Lớn (p),`q=3`|`NO`| Đầu vào lớn không có số học số nguyên lớn | 
| Lớn (p),`q=2`|`YES`| (q=2) nhánh có (p) | 

## Vỏ cạnh 

Trường hợp không rõ ràng đầu tiên là (q=2). Đối với đầu vào```
2
2
```thuật toán ngay lập tức nhìn thấy`q == "2"`và in`YES`. Biểu thức là (3^2=9). Tổng quát hơn, nhánh này chấp nhận mọi số nguyên tố hợp lệ (p), do đó nó phải được kiểm tra trước khi áp dụng đối số số mũ lẻ. 

Trường hợp thứ hai là (p=2) với số mũ nguyên tố lẻ:```
2
3
```Ở đây (q) là số lẻ và (p+1=3). Thuật toán bỏ qua nhánh (q=2), sau đó kiểm tra`p == "3"`, sai và in`NO`. Về mặt đại số, (3^3=27), không phải là hình vuông. 

Trường hợp thứ ba là trường hợp dương duy nhất cho số lẻ (q):```
3
3
```Sự so sánh đầu tiên thất bại, nhưng`p == "3"`thành công. Thuật toán in`YES`. Thật vậy, (p+1=4=2^2) và (4^3=64=8^2). 

Trường hợp cạnh cuối cùng liên quan đến giới hạn số rất lớn. Giả sử (p) có (1000) chữ số và (q=2). Chương trình không tính toán (p+1) hoặc ((p+1)^2). Nó chỉ so sánh chuỗi`q`với`"2"`, vì vậy nó có thể quay trở lại ngay lập tức`YES`. Tương tự, đối với số nguyên tố lớn (p) và số lẻ (q), nó chỉ kiểm tra xem chuỗi đầu vào đầy đủ cho (p) có chính xác không`"3"`. Điều này tránh được tình trạng tràn, tăng bộ nhớ và thời gian chạy mà việc tính toán trực tiếp biểu thức có thể gây ra.
