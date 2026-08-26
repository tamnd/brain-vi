---
title: "CF 104339E - So sánh"
description: "Chúng ta có hai cách biểu diễn bằng văn bản của số thực ở dạng thập phân và chúng ta cần quyết định xem số nào lớn hơn hoặc chúng có bằng nhau hay không. Điều khó khăn là định dạng rất lỏng lẻo."
date: "2026-07-01T18:38:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "E"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 69
verified: true
draft: false
---

[CF 104339E - So sánh](https://codeforces.com/problemset/problem/104339/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cách biểu diễn bằng văn bản của số thực ở dạng thập phân và chúng ta cần quyết định xem số nào lớn hơn hoặc chúng có bằng nhau hay không. Điều khó khăn là định dạng rất lỏng lẻo. Một số có thể có phần nguyên tùy chọn, phần phân số tùy chọn hoặc cả hai, nhưng không bao giờ thiếu cả hai. Dấu thập phân có thể không có nếu không có phần phân số và cả hai bên của dấu chấm cũng có thể bị thiếu, điều này thực tế hoạt động giống như số 0. Các số 0 ở đầu và các số 0 ở cuối không liên quan và có thể xuất hiện với số lượng tùy ý. 

Về mặt khái niệm, mỗi chuỗi đầu vào mã hóa một số thực không âm ở cơ số 10, nhưng không ở dạng chuẩn. Nhiệm vụ hoàn toàn là so sánh giá trị số chứ không phải thứ tự chuỗi. 

Các ràng buộc bị chi phối bởi độ dài của mỗi chuỗi đầu vào, có thể lên tới 100.000 ký tự. Điều đó ngay lập tức loại trừ bất kỳ chiến lược bình thường hóa số nào bằng cách chuyển đổi chúng thành các loại dấu phẩy động hoặc số thập phân Python, vì cả độ chính xác và hiệu suất đều sẽ thất bại. Ngay cả số học số nguyên lớn cũng phải được xử lý cẩn thận vì phần phân số có thể lớn bằng phần nguyên. 

Khó khăn chính là việc biểu diễn được chia thành hai thành phần độc lập, số nguyên và phân số, và cả hai phải được so sánh theo từ điển sau khi chuẩn hóa mà không xây dựng các số trung gian khổng lồ. 

Một số trường hợp đặc biệt khiến các giải pháp ngây thơ thất bại trong âm thầm. 

Ví dụ đầu tiên là phân biệt các số 0 đứng đầu trong các phần nguyên. Coi như`00012.3`so với`12.3`. So sánh chuỗi sẽ coi chuỗi đầu tiên không chính xác là nhỏ hơn do thứ tự từ điển, nhưng về mặt số lượng, chúng có độ lớn bằng số nguyên. 

Ví dụ thứ hai thiếu phần nguyên. Đầu vào thích`.15`đại diện cho`0.15`và phải so sánh chính xác với thứ gì đó như`0.149999`. 

Ví dụ thứ ba là so sánh phân số trong đó một số có phần phân số dài hơn. Ví dụ,`1.2300`Và`1.23`bằng nhau, mặc dù các chuỗi thô khác nhau. Một so sánh ngây thơ so sánh trực tiếp các chuỗi phân số sẽ nói không chính xác chuỗi đầu tiên lớn hơn do có thêm ký tự. 

Những vấn đề này cho thấy chúng ta phải bình thường hóa cấu trúc chứ không phải tính toán các giá trị số. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là phân tích từng chuỗi thành một đối tượng thập phân có độ chính xác cao hoặc mô phỏng số học chính xác tùy ý bằng cách chuyển đổi toàn bộ số thành một số nguyên duy nhất được chia tỷ lệ bằng 10 thành lũy thừa của độ dài phân số tối đa. Đối với mỗi số, chúng ta cần xác định độ dài phân số, sau đó đệm phần nguyên tương ứng, nối và so sánh dưới dạng số nguyên lớn. 

Về nguyên tắc, điều này hoạt động chính xác vì cả hai số đều được chuyển đổi thành các biểu diễn số nguyên có thể so sánh được. Tuy nhiên, chi phí trở thành vấn đề. Nếu cả hai số có tối đa 100.000 chữ số và chúng ta ghép chúng thành một chuỗi số nguyên lớn, thì bản thân phép so sánh là O(n) và việc chuyển đổi cũng như chuẩn hóa cũng yêu cầu O(n). Trong thực tế, điều này vẫn nằm ở ranh giới nhưng chỉ được chấp nhận trong Python nếu được triển khai cẩn thận. Tuy nhiên, việc phân bổ và đệm lặp đi lặp lại làm cho nó dễ vỡ. 

Một quan sát tốt hơn là chúng ta không bao giờ cần xây dựng một số tổng hợp. Trước tiên chúng ta chỉ cần so sánh các phần nguyên và chỉ khi chúng bằng nhau thì chúng ta mới so sánh các phần phân số. Trong mỗi phần, việc chuẩn hóa hoàn toàn là việc bỏ qua các số 0 ở đầu hoặc cuối. 

Do đó, vấn đề giảm xuống còn so sánh chuỗi theo các quy tắc chuẩn hóa được kiểm soát: loại bỏ các số 0 đứng đầu trong phần nguyên, loại bỏ các số 0 ở cuối trong phần phân số, sau đó so sánh độ dài và thứ tự từ điển. 

Điều này làm giảm vấn đề từ việc xây dựng số đến việc so sánh hai cặp chuỗi đã được làm sạch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chuẩn hóa số nguyên lớn) | O(n) trên mỗi số, hằng số nặng | O(n) | Rủi ro | 
| Tối ưu (tách và chuẩn hóa các phần) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mỗi chuỗi đầu vào thành các phần số nguyên và phân số xung quanh dấu chấm. Nếu không tồn tại dấu chấm, hãy coi phần phân số là trống. Nếu chuỗi bắt đầu bằng dấu chấm, hãy coi phần nguyên là trống. 
2. Chuẩn hóa phần nguyên bằng cách loại bỏ các số 0 đứng đầu. Nếu kết quả trống, hãy thay thế bằng`"0"`. Điều này đảm bảo rằng các giá trị như`"000"`Và`""`cả hai đều đại diện cho số không. 
3. Chuẩn hóa phần phân số bằng cách loại bỏ các số 0 ở cuối. Nếu nó trở nên trống rỗng, hãy coi nó như`""`. Chúng tôi không ép buộc nó`"0"`bởi vì đẳng thức phân số phụ thuộc vào độ dài và nội dung. 
4. So sánh các phần nguyên trước. Nếu một chữ số có nhiều chữ số hơn chữ số kia thì chữ số nào dài hơn sẽ đại diện cho số lớn hơn. Nếu chúng khác nhau về thứ tự từ điển ở cùng độ dài, điều đó sẽ quyết định kết quả. 
5. Nếu các phần nguyên bằng nhau thì so sánh các phần phân số. Đầu tiên so sánh theo độ dài của chuỗi phân số. Phần phân số dài hơn có nghĩa là độ chính xác cao hơn mức bình đẳng, vì vậy`1.2300`trở thành`1.23`sau khi cắt tỉa và chúng trở nên bằng nhau. 
6. Nếu độ dài bằng nhau, hãy so sánh từng chữ số theo từ điển. 
7. Nếu cả hai phần trùng khớp hoàn toàn thì các số bằng nhau. 

### Tại sao nó hoạt động 

Mỗi số thực trong bài toán này được xác định duy nhất bởi độ lớn nguyên và phần mở rộng phân số sau khi loại bỏ các số 0 dư thừa. Việc chuẩn hóa đảm bảo rằng mỗi giá trị ánh xạ tới một dạng chuẩn trong đó so sánh số nguyên phản ánh độ lớn và so sánh phân số chỉ phản ánh thứ tự chi tiết khi các phần nguyên khớp nhau. Vì cả hai phần được so sánh theo thứ tự có ý nghĩa giảm dần nên ưu thế số nguyên luôn quyết định trước và so sánh phân số chỉ giải quyết các mối quan hệ. Cấu trúc này bảo toàn sự tương đương về thứ tự với số thực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def split_num(s: str):
    if '.' in s:
        a, b = s.split('.')
    else:
        a, b = s, ""

    a = a.lstrip('0')
    if a == "":
        a = "0"

    b = b.rstrip('0')
    return a, b

def cmp(a1, b1, a2, b2):
    if len(a1) != len(a2):
        return 1 if len(a1) > len(a2) else -1
    if a1 != a2:
        return 1 if a1 > a2 else -1

    if b1 == b2:
        return 0

    if len(b1) != len(b2):
        return 1 if len(b1) > len(b2) else -1

    if b1 > b2:
        return 1
    return -1

s1 = input().strip()
s2 = input().strip()

a1, b1 = split_num(s1)
a2, b2 = split_num(s2)

print(cmp(a1, b1, a2, b2))
```Hàm phân tách có nhiệm vụ chuyển đổi từng số thành biểu diễn chính tắc. Phần nguyên được loại bỏ các số 0 đứng đầu, đảm bảo so sánh độ lớn chính xác. Phần phân số bị loại bỏ các số 0 ở cuối, đảm bảo rằng các khai triển thập phân tương đương không phân kỳ. 

Hàm so sánh cẩn thận tránh chuyển đổi chuỗi thành kiểu số. Các phần nguyên được so sánh đầu tiên bằng cách sử dụng độ dài, đây là yếu tố quyết định quan trọng nhất. Chỉ khi độ dài phù hợp thì chúng ta mới quay lại so sánh từ điển. 

So sánh phân số chỉ được sử dụng như một yếu tố quyết định. So sánh độ dài được sử dụng đầu tiên vì`"123"`vs`"1230"`mặt khác sẽ so sánh không chính xác nếu được xử lý theo từ điển mà không chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
211.000000000000000001
211
```| Bước | Số nguyên phần 1 | Phân số 1 | Số nguyên phần 2 | Phân số 2 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Chia | 211 | 000...001 | 211 | "" | số nguyên bằng nhau | 
| Bình thường hóa | 211 | 1 | 211 | "" | số nguyên bằng | 
| So sánh phân số | 1 | | "" | | 1 > trống | 

Các phần nguyên khớp chính xác, do đó việc so sánh giảm xuống các phần phân số. Sau khi loại bỏ các số 0 ở cuối, số đầu tiên có thành phần phân số không trống trong khi số thứ hai không có thành phần nào, vì vậy số đầu tiên lớn hơn. 

### Ví dụ 2 

đầu vào:```
15
00000000015.00000000
```| Bước | Số nguyên phần 1 | Phân số 1 | Số nguyên phần 2 | Phân số 2 | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Chia | 15 | "" | 00000000015 | 00000000 | cùng giá trị | 
| Bình thường hóa | 15 | "" | 15 | "" | bằng | 

Sau khi chuẩn hóa cả hai phần nguyên giảm xuống`15`và các phần phân số trở nên trống rỗng. Điều này khẳng định sự bình đẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được quét một số lần không đổi để tách và cắt | 
| Không gian | O(n) | Chuỗi chuẩn hóa được lưu trữ trong trường hợp xấu nhất | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì mỗi ký tự được xử lý với số lần không đổi và không có đối tượng số trung gian lớn nào được xây dựng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def split_num(s: str):
        if '.' in s:
            a, b = s.split('.')
        else:
            a, b = s, ""
        a = a.lstrip('0')
        if a == "":
            a = "0"
        b = b.rstrip('0')
        return a, b

    def cmp(a1, b1, a2, b2):
        if len(a1) != len(a2):
            return 1 if len(a1) > len(a2) else -1
        if a1 != a2:
            return 1 if a1 > a2 else -1
        if b1 == b2:
            return 0
        if len(b1) != len(b2):
            return 1 if len(b1) > len(b2) else -1
        return 1 if b1 > b2 else -1

    s1 = input().strip()
    s2 = input().strip()
    a1, b1 = split_num(s1)
    a2, b2 = split_num(s2)
    return str(cmp(a1, b1, a2, b2))

# provided samples
assert run("211.000000000000000001\n211\n") == "1", "sample 1"
assert run("15\n00000000015.00000000\n") == "0", "sample 2"
assert run(".15\n00000000015.00000000\n") == "-1", "sample 3"

# custom cases
assert run("0.0\n0") == "0", "both zero forms"
assert run("000.0001\n0.0001000") == "0", "fraction normalization"
assert run("1.1\n1.10") == "0", "trailing zero fraction equality"
assert run("2\n10") == "-1", "integer length dominance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0.0 vs 0`| 0 | chuẩn hóa hoàn toàn bằng 0 | 
|`000.0001 vs 0.0001000`| 0 | tính chính xác của việc cắt tỉa phân đoạn | 
|`1.1 vs 1.10`| 0 | bỏ qua các số 0 phân số ở cuối | 
|`2 vs 10`| -1 | độ chính xác so sánh độ dài số nguyên | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi cả hai phần nguyên thu gọn lại thành trống sau khi loại bỏ các số 0. Ví dụ,`0000.5`Và`.5`cả hai đều đại diện cho cùng một giá trị số nguyên`0`. Bước chuẩn hóa buộc các chuỗi số nguyên trống vào`"0"`, vì vậy cả hai đều trở nên giống hệt nhau trước khi so sánh phân số. 

Một trường hợp khác là khi các phần phân đoạn trở nên trống rỗng sau khi cắt bớt. Ví dụ,`1.2300`trở thành số nguyên`1`và phân số`23`, trong khi`1.23`trở nên giống nhau. Vì so sánh phân số coi các chuỗi trống chỉ bằng nhau khi cả hai đều trống hoặc cả hai đều khớp nhau, nên sự bình đẳng được giữ nguyên. 

Trường hợp cạnh cuối cùng là các phần nguyên lớn có độ dài bằng nhau nhưng có giá trị từ điển khác nhau, chẳng hạn như`999`Và`100`. Mặc dù về mặt từ điển`"999" > "100"`, so sánh độ dài giống hệt nhau, do đó so sánh từ điển giải quyết chính xác thứ tự mà không cần chuyển đổi số.
