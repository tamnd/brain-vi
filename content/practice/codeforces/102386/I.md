---
title: "CF 102386I - \u041f\u0435\u0440\u0441\u0435\u0430\u043d\u0442\u043e\u0432\u043a\u0430"
description: "Chúng ta được đưa cho một câu mà các từ có thể đã được sắp xếp lại các chữ cái bên trong. Đối với mỗi từ, các chữ cái đầu tiên và cuối cùng được giữ cố định, đồng thời cho phép mọi hoán vị các chữ cái giữa chúng."
date: "2026-08-15T08:10:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102386
codeforces_index: "I"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u0423\u0440\u0430\u043b\u044c\u0441\u043a\u043e\u0433\u043e \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b\u0430 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u043c\u0438\u0440\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2019"
rating: 0
weight: 102386
solve_time_s: 1746
verified: false
draft: false
---

[CF 102386I - \u041f\u0435\u0440\u0441\u0435\u0430\u043d\u0442\u043e\u0432\u043a\u0430](https://codeforces.com/problemset/problem/102386/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 29m 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa cho một câu mà các từ có thể đã được sắp xếp lại các chữ cái bên trong. Đối với mỗi từ, các chữ cái đầu tiên và cuối cùng được giữ cố định, đồng thời cho phép mọi hoán vị các chữ cái giữa chúng. Bên cạnh câu bị hỏng, chúng tôi nhận được một từ điển chứa mọi từ có thể xuất hiện trong câu gốc. 

Đối với mỗi từ trong câu bị hỏng, chúng ta cần tìm bất kỳ từ nào trong từ điển có thể biến thành từ đó bằng cách chỉ hoán vị các chữ cái bên trong của nó. Một từ trong từ điển tương thích chính xác khi nó có cùng chữ cái đầu tiên, cùng chữ cái cuối cùng và cùng số lần xuất hiện của mỗi chữ cái. Nếu có ít nhất một từ trong từ điển tương thích tồn tại cho mỗi từ trong câu, chúng ta có thể đưa ra bất kỳ sự tái cấu trúc nào như vậy. Nếu ngay cả một từ trong câu không có từ trong từ điển tương thích thì câu trả lời là`No solution`. 

Tổng số ký tự trong câu nhiều nhất là (5\cdot10^5), và tổng số ký tự trong từ điển cũng nhiều nhất là (5\cdot10^5). Số lượng mục từ điển có thể đạt tới (5\cdot10^5). Các giới hạn này loại trừ việc quét toàn bộ từ điển để tìm từng từ trong câu. Một câu có thể chứa hàng trăm nghìn từ, do đó, một tìm kiếm (O(Wn)), trong đó (W) là số từ trong câu, có thể đạt tới khoảng (1,25\cdot10^{11}) ứng viên kiểm tra theo giới hạn riêng lẻ (W\le250000) và (n\le500000). Thay vào đó, chúng ta cần xử lý tỷ lệ thuận với tổng lượng đầu vào. 

Có một số trường hợp ranh giới mà việc triển khai bất cẩn có thể xử lý sai. Một từ có một chữ cái không có các chữ cái bên trong, vì vậy nó chỉ có thể khớp với một từ trong từ điển có cùng một ký tự. Ví dụ, với đầu vào```
a.
1
a
```câu trả lời là`a.`. Việc coi ký tự đầu tiên và ký tự cuối cùng là hai vị trí riêng biệt mà không xử lý trường hợp một ký tự có thể vô tình lập chỉ mục không chính xác cho phần giữa trống. 

Các từ trong từ điển khác nhau có thể có cùng một chữ ký. Ví dụ,```
tihs.
2
this
hits
```chỉ có`this`như một câu trả lời hợp lệ, bởi vì`hits`có chữ cái đầu và chữ cái cuối khác nhau. Mặt khác,```
scret.
2
secret
serect
```sẽ cho phép từ tương thích nếu từ được quan sát có cùng độ dài và số lượng chữ cái. Bài toán cho phép một cách rõ ràng bất kỳ câu trả lời hợp lệ nào, do đó chỉ cần lưu trữ một từ cho mỗi chữ ký là đủ. 

Một từ cũng có thể có độ dài chính xác nhưng vẫn không thể xây dựng lại được vì một chữ cái bên trong có số bội sai. Ví dụ,```
wlrd.
1
world
```không có giải pháp. Từ được quan sát chỉ chứa bốn chữ cái, trong khi từ trong từ điển chứa năm chữ cái, vì vậy chỉ so sánh chữ cái đầu tiên và chữ cái cuối cùng sẽ chấp nhận nó một cách sai lầm. 

Cuối cùng, dấu chấm thuộc về cú pháp câu chứ không thuộc về bất kỳ từ nào. Vì```
hello wolrd.
2
hello
world
```từ thứ hai là`wolrd`, không`wolrd.`. Trình phân tích cú pháp bao gồm dấu chấm trong chữ ký từ sẽ không tìm thấy`world`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lấy từng từ trong câu bị hỏng và so sánh nó với mọi từ trong từ điển. Đối với một cặp ứng cử viên, chúng ta có thể kiểm tra ký tự đầu tiên và ký tự cuối cùng, sau đó so sánh tần suất ký tự trong hai từ. Phương pháp này đúng vì những điều kiện này chính xác là định nghĩa có thể đạt được bằng cách hoán vị các chữ cái bên trong. 

Vấn đề là việc quét từ điển lặp đi lặp lại. Nếu câu có (W) từ và từ điển có (n) mục thì có thể có (Wn) kiểm tra ứng viên. Từ giới hạn đầu vào, (W) có thể là khoảng (250000) và (n) có thể là (500000), đưa ra giới hạn trên là (125000000000) các cuộc kiểm tra ứng viên. Ngay cả việc bị từ chối liên tục đối với hầu hết các ứng viên cũng vượt xa mức hợp lý. 

Quan sát quan trọng là thứ tự của các chữ cái bên trong hoàn toàn không quan trọng. Mỗi từ có thể được biểu diễn bằng một chữ ký chuẩn bao gồm chữ cái đầu tiên, chữ cái cuối cùng và tần số của mỗi chữ cái trong số 26 chữ cái viết thường. Hai từ tương thích khi và chỉ khi chữ ký của chúng bằng nhau. 

Chúng ta có thể tính toán chữ ký này một lần cho mỗi từ trong từ điển và đưa nó vào bảng băm. Sau đó, mỗi từ trong câu sẽ được chuyển thành cùng một chữ ký và tra cứu trực tiếp. Việc tìm kiếm lặp đi lặp lại tốn kém sẽ biến mất, chỉ còn lại một lượt duyệt qua từ điển và một lượt duyệt qua câu. 

Phương pháp brute-force hoạt động vì nó kiểm tra chính xác điều kiện phù hợp, nhưng nó liên tục phát hiện lại cùng một thông tin. Nhận xét rằng tính tương thích được xác định hoàn toàn bằng một chữ ký chuẩn nhỏ cho phép chúng ta thay thế các phép so sánh lặp đi lặp lại bằng tra cứu bảng băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Wn)) kiểm tra ứng viên, với chi phí so sánh từ bổ sung | (O(n)) | Quá chậm | 
| Tối ưu | (O(S+D+26n+26W)), hiệu quả (O(S+D)) | (O(D+26n)) | Đã chấp nhận | 

Ở đây (S) là tổng độ dài của các từ trong câu và (D) là tổng độ dài của các từ trong từ điển. Vì cả hai đều nhiều nhất là (5\cdot10^5), nên các số hạng tuyến tính chiếm ưu thế. 

## Hướng dẫn thuật toán 

1. Đọc câu bị lỗi và bỏ dấu chấm cuối cùng. Việc tách chuỗi còn lại bằng dấu cách sẽ cho ra chính xác chuỗi các từ bị hỏng, vì đầu vào đảm bảo một khoảng trắng duy nhất giữa các từ lân cận. 
2. Đối với mỗi từ trong từ điển, hãy xây dựng một chữ ký chứa ký tự đầu tiên, ký tự cuối cùng và vectơ tần số 26 phần tử. Vectơ tần số đếm tất cả các chữ cái trong từ, bao gồm cả điểm cuối. Việc bao gồm các điểm cuối trong số đếm là an toàn vì các ký tự đó cũng được cố định. 
3. Lưu trữ một từ điển cho mỗi chữ ký trong bảng băm. Một số từ trong từ điển khác nhau có thể có chung một chữ ký, nhưng điều đó không gây ra vấn đề gì vì câu lệnh chấp nhận bất kỳ sự tái tạo hợp lệ nào. 
4. Với mỗi từ bị lỗi trong câu, hãy tính chữ ký của nó và tra cứu trong bảng. Nếu thiếu chữ ký, không từ nào trong từ điển có thể tạo ra từ được quan sát này, do đó toàn bộ câu không có sự tái tạo hợp lệ. 
5. Nếu có chữ ký, hãy thêm từ trong từ điển đã lưu vào câu được xây dựng lại. Lặp lại điều này một cách độc lập cho mỗi từ. 
6. Nối các từ đã được xây dựng lại bằng dấu cách đơn và nối thêm dấu chấm cuối cùng. Chuỗi kết quả có ranh giới từ và dấu câu giống hệt như câu đầu vào. 

### Tại sao nó hoạt động

Điều bất biến là mỗi chữ ký được lưu trữ thể hiện chính xác tập hợp các từ trong từ điển mà các chữ cái của chúng có thể được sắp xếp lại thành từ được quan sát tương ứng mà không thay đổi chữ cái đầu tiên hoặc cuối cùng của nó. Nếu một từ được quan sát có cùng chữ ký với một từ trong từ điển, thì hai từ đó chứa chính xác nhiều chữ cái giống nhau và có điểm cuối giống hệt nhau, do đó các chữ cái bên trong có thể được hoán vị để biến đổi từ này sang chữ kia. Nếu chữ ký của chúng khác nhau thì ít nhất một điểm cuối hoặc tần số chữ cái sẽ khác nhau, khiến cho việc hoán vị như vậy là không thể. Do đó, mọi từ trong từ điển được chọn đều hợp lệ và việc không tìm thấy chữ ký chứng tỏ rằng không có sự tái tạo hợp lệ nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def signature(word):
    cnt = [0] * 26
    for ch in word:
        cnt[ord(ch) - 97] += 1
    return (word[0], word[-1], tuple(cnt))

def solve():
    text = input().rstrip('\n')
    n = int(input())

    dictionary = {}

    for _ in range(n):
        word = input().strip()
        key = signature(word)
        if key not in dictionary:
            dictionary[key] = word

    words = text[:-1].split(' ')
    answer = []

    for word in words:
        key = signature(word)
        original = dictionary.get(key)

        if original is None:
            print("No solution")
            return

        answer.append(original)

    print(' '.join(answer) + '.')

if __name__ == "__main__":
    solve()
```các`signature`hàm xây dựng một vectơ tần số có kích thước cố định. Thời gian chạy của nó là tuyến tính theo độ dài từ vì mỗi ký tự được xử lý chính xác một lần. Bộ dữ liệu làm cho vectơ trở nên bất biến, do đó, nó có thể được sử dụng một cách an toàn như một phần của khóa từ điển. 

Từ điển được điền trước khi xử lý câu. Khi hai từ trong từ điển có cùng chữ ký, từ đầu tiên được giữ lại. Điều này là có chủ ý vì đầu ra chỉ yêu cầu một văn bản gốc hợp lệ. 

Câu được phân tích cú pháp bằng`text[:-1]`, loại bỏ chính xác khoảng thời gian cuối cùng. Sự đảm bảo về đầu vào có nghĩa là không có dấu câu nào khác cần xử lý đặc biệt. Tách bằng một dấu cách sẽ cho ra chuỗi từ gốc. 

Việc tra cứu sử dụng`dictionary.get(key)`thay vì lập chỉ mục trực tiếp. Một chữ ký bị thiếu tạo ra`None`, cho phép chương trình báo cáo ngay lập tức`No solution`. Mỗi từ trong từ điển hợp lệ là một chuỗi chữ thường không trống, vì vậy`None`không thể nhầm lẫn với giá trị được lưu trữ hợp pháp. 

Dấu chấm cuối cùng chỉ được thêm vào sau khi tất cả các từ đã được xây dựng lại. Điều này tránh việc vô tình coi nó như một phần của chữ ký từ. 

Số nguyên Python không tràn ở đây. Tần số lớn nhất tối đa là (5\cdot10^5), thấp hơn nhiều so với bất kỳ giới hạn số nguyên thực tế nào. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, từ điển chứa`hello`Và`world`. Các từ được quan sát là`hello`Và`wolrd`. 

| Lời | Đầu tiên | Cuối cùng | Đếm chữ cái | Kết quả tra cứu | 
| --- | --- | --- | --- | --- | 
|`hello`|`h`|`o`|`e:1, h:1, l:2, o:1`|`hello`| 
|`wolrd`|`w`|`d`|`d:1, l:1, o:1, r:1, w:1`|`world`| 

Từ đầu tiên đã có sẵn ở dạng từ điển nên chữ ký của nó sẽ được tìm thấy`hello`. Từ thứ hai có cùng nhiều ký tự và điểm cuối như`world`, vì vậy chữ ký của nó tìm thấy`world`. Kết quả được xây dựng lại là`hello world.`. 

Đối với Mẫu 2, từ trong từ điển duy nhất có khả năng khớp với từ của câu thứ hai là`world`, nhưng từ được quan sát là`wlrd`. 

| Lời | Đầu tiên | Cuối cùng | Chiều dài | Kết quả tra cứu | 
| --- | --- | --- | --- | --- | 
|`hello`|`h`|`o`| 5 |`hello`| 
|`wlrd`|`w`|`d`| 4 | vắng mặt | 

Chữ ký thứ hai không thể bằng chữ ký của`world`, bởi vì lá thư`o`bị thiếu và tổng số ký tự khác nhau. Tra cứu thất bại nên thuật toán in ngay`No solution`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(S+D+26(W+n))), hiệu quả (O(S+D)) | Mỗi ký tự đầu vào được tính một lần và mỗi chữ ký có chính xác 26 bộ đếm | 
| Không gian | (O(D+26(W+n))), hiệu quả (O(D+W+n)) | Bảng băm lưu trữ một từ đại diện và một chữ ký có kích thước cố định cho mỗi chữ ký từ điển | 

Tổng độ dài câu và tổng độ dài từ điển đều được giới hạn bởi (5\cdot10^5). Bảng chữ cái chỉ có 26 chữ cái nên phần cố định 26 phần tử của mỗi chữ ký là nhỏ. Do đó, thuật toán thực hiện một lượng xử lý ký tự tuyến tính cộng với các phép toán bảng băm, phù hợp với các ràng buộc đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

def signature(word):
    cnt = [0] * 26
    for ch in word:
        cnt[ord(ch) - 97] += 1
    return (word[0], word[-1], tuple(cnt))

def solve():
    input = sys.stdin.readline

    text = input().rstrip('\n')
    n = int(input())

    dictionary = {}

    for _ in range(n):
        word = input().strip()
        key = signature(word)
        if key not in dictionary:
            dictionary[key] = word

    words = text[:-1].split(' ')
    answer = []

    for word in words:
        original = dictionary.get(signature(word))
        if original is None:
            print("No solution")
            return
        answer.append(original)

    print(' '.join(answer) + '.')

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

# Provided samples
assert run("""hello wolrd.
2
hello
world
""") == "hello world.", "sample 1"

assert run("""hello wlrd.
2
hello
world
""") == "No solution", "sample 2"

assert run("""tihs is vrey sceret txet.
7
text
secret
serect
scret
is
very
this
""") == "this is very secret text.", "sample 3"

# Minimum-size input
assert run("""a.
1
a
""") == "a.", "single-letter word"

# One-letter observed word must not match another letter
assert run("""b.
1
a
""") == "No solution", "single-letter mismatch"

# Same endpoints and letter multiset, but dictionary has several valid choices
result = run("""scret.
2
secret
serect
""")
assert result in {"secret.", "serect."}, "multiple valid dictionary words"

# Boundary case where the last letter matters
assert run("""abcda.
1
abcdb
""") == "No solution", "different last letter"

# Large input close to the sentence-size limit
long_word = "a" * 499999
large_input = long_word + ".\n1\n" + long_word + "\n"
assert run(large_input) == long_word + ".", "large word"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a.`với từ điển`a`|`a.`| Từ nhỏ nhất có thể và phần bên trong trống | 
|`b.`với từ điển`a`|`No solution`| Điểm cuối không khớp | 
|`scret.`với`secret`,`serect`| Từ điển tương thích | Nhiều từ điển có cùng chữ ký | 
|`abcda.`với`abcdb`|`No solution`| Ranh giới ký tự cuối cùng | 
| Một từ có 499999 ký tự có trong từ điển | Cùng một từ lớn | Kích thước đầu vào gần tối đa và xử lý tuyến tính | 

## Vỏ cạnh 

Từ một chữ cái được xử lý mà không cần phân nhánh đặc biệt vì chữ ký tính ký tự duy nhất và ghi nó làm cả ký tự đầu tiên và ký tự cuối cùng. Vì```
a.
1
a
```chữ ký của từ trong câu chính xác là chữ ký được lưu trữ cho`a`, vì vậy đầu ra là`a.`. Vì```
b.
1
a
```vectơ tần số và điểm cuối khác nhau, do đó việc tra cứu không thành công và đầu ra bị lỗi`No solution`. 

Nhiều từ trong từ điển có thể đại diện cho cùng một từ bị hỏng. Coi như```
scret.
2
secret
serect
```Cả hai từ trong từ điển đều có chữ cái đầu tiên`s`, lá thư cuối cùng`t`, và tần số chữ cái giống nhau. Bảng băm giữ một đại diện. Cái nào được giữ lại thì có thể tái tạo lại`scret`, do đó kết quả đầu ra hợp lệ bất kể mục từ điển nào được lưu trữ. 

Số lượng chữ cái nội bộ sai không thể được sửa chữa bằng cách sắp xếp lại. Coi như```
wlrd.
1
world
```Từ được quan sát có một`w`, một`l`, một`r`, và một`d`, trong khi`world`Ngoài ra còn chứa`o`và có chiều dài bằng năm. Chữ ký của họ khác nhau nên việc tra cứu từ điển không thành công. Thuật toán in`No solution`thay vì chấp nhận một từ chỉ vì điểm cuối của nó đồng ý. 

Giai đoạn cuối cùng không được tham gia ký tên. Vì```
hello wolrd.
2
hello
world
```trình phân tích cú pháp sẽ loại bỏ dấu chấm cuối cùng trước khi tách câu. Nó xử lý`hello`Và`wolrd`, tìm thấy`hello`Và`world`, sau đó khôi phục lại dấu chấm sau khi nối các từ được xây dựng lại. Điều này giữ cho dấu chấm câu tách biệt khỏi điều kiện tần số chữ cái.
