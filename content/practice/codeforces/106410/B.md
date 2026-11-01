---
title: "CF 106410B - Yash bị lác mắt"
description: "Chúng tôi được cung cấp một số chuỗi. Đối với mỗi chuỗi, chúng ta có thể hoán đổi nhiều lần hai ký tự lân cận. Câu hỏi đặt ra là liệu những hoán đổi này có thể biến chuỗi thành hai phần liên tiếp giống hệt nhau hay không."
date: "2026-06-25T09:54:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106410
codeforces_index: "B"
codeforces_contest_name: "HPI 2026 Novice"
rating: 0
weight: 106410
solve_time_s: 34
verified: true
draft: false
---

[CF 106410B - Yash bị lác mắt](https://codeforces.com/problemset/problem/106410/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số chuỗi. Đối với mỗi chuỗi, chúng ta có thể hoán đổi nhiều lần hai ký tự lân cận. Câu hỏi đặt ra là liệu những hoán đổi này có thể biến chuỗi thành hai phần liên tiếp giống hệt nhau hay không. Nói cách khác, sau khi sắp xếp lại các ký tự theo thứ tự bất kỳ, chuỗi cuối cùng có thể trông như thế nào`t + t`cho một số chuỗi`t`? 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là một chuỗi chữ thường và đầu ra cho chuỗi đó là`YES`khi có sự sắp xếp lại như vậy, nếu không thì`NO`. 

Ràng buộc chính là tổng chiều dài của tất cả các chuỗi, tối đa là`2 * 10^5`. Điều này có nghĩa là chúng ta có thể cung cấp giải pháp quét mọi ký tự với số lần không đổi. Bất kỳ cách tiếp cận nào cố gắng sắp xếp lại nhiều khả năng hoặc mô phỏng các giao dịch hoán đổi đều không thể thực hiện được, bởi vì ngay cả một chuỗi có độ dài`200000`có một số lượng lớn các hoán vị có thể có. 

Phần không rõ ràng là hiểu những gì các giao dịch hoán đổi liền kề cho phép. Các hoán đổi liền kề có thể di chuyển các ký tự xung quanh một cách tự do nhưng chúng không thể tạo hoặc xóa bất kỳ ký tự nào. Điều duy nhất quan trọng là số lượng từng chữ cái trong chuỗi gốc. 

Việc triển khai đơn giản chỉ có thể kiểm tra xem độ dài có chẵn hay không. Ví dụ:```
1
aabbc
```Độ dài là 5 nên ngay lập tức bị lỗi, nhưng lỗi thú vị hơn xuất hiện khi độ dài chẵn:```
1
aabbcdef
```Một giải pháp bất cẩn chỉ có thể chia chuỗi làm đôi và so sánh hai nửa sau khi sắp xếp. Tuy nhiên, các ký tự có sẵn là`a, a, b, b, c, d, e, f`. các chữ cái`c`,`d`,`e`, Và`f`xuất hiện một lần nên chúng không thể thuộc cùng một vị trí ở cả hai nửa. Đầu ra đúng là:```
NO
```Một trường hợp cạnh khác là một ký tự đơn:```
1
a
```Không có cách nào để chia một chuỗi có độ dài thành hai phần bằng nhau, vì vậy câu trả lời là:```
NO
```Trường hợp cạnh đối diện là một chuỗi trong đó mỗi ký tự đã có một bản sao phù hợp:```
1
abba
```Chúng ta có thể sắp xếp lại nó thành`ba + ba`, vì vậy kết quả đúng là:```
YES
```## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là thử tất cả các cách sắp xếp lại chuỗi có thể có và kiểm tra xem liệu bất kỳ cách nào trong số chúng có dạng`t + t`. Điều này đúng vì các hoán đổi liền kề có thể tạo ra bất kỳ hoán vị nào của các ký tự. Tuy nhiên, một chuỗi có độ dài`n`có thể có tới`n!`hoán vị khác nhau, điều này trở nên không thể thực hiện được ngay cả đối với các giá trị rất nhỏ của`n`. 

Điều quan trọng là chúng ta không thực sự cần biết sự sắp xếp cuối cùng. Nếu chuỗi cuối cùng là`t + t`, khi đó mọi nhân vật sẽ xuất hiện hai lần trong nửa đầu và hai lần trong nửa sau ở những vị trí giống hệt nhau. Điều đó có nghĩa là mọi ký tự trong toàn bộ chuỗi phải xuất hiện với số lần chẵn. 

Hướng ngược lại cũng đúng. Nếu mỗi nhân vật xuất hiện với số lần chẵn thì chúng ta có thể xếp một nửa số nhân vật vào nửa đầu và nửa còn lại vào nửa sau. Vì các giao dịch hoán đổi liền kề cho phép sắp xếp lại tùy ý nên cấu trúc này luôn có thể truy cập được. 

Vấn đề được giảm bớt từ việc tìm kiếm thông qua hoán vị đến đếm tần số. Chúng ta chỉ cần xác minh xem mọi tần số có chia hết cho hai hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số lần mỗi chữ cái viết thường xuất hiện trong chuỗi. Sự sắp xếp cuối cùng chỉ có thể sử dụng các ký tự hiện có, vì vậy số đếm này chứa tất cả thông tin cần thiết. 
2. Kiểm tra từng ký tự. Nếu bất kỳ ký tự nào xuất hiện với số lần lẻ thì câu trả lời là`NO`. Số tiền lẻ không thể chia đều cho hai bản sao`t`. 
3. Nếu tất cả số ký tự đều là số chẵn, thì xuất ra`YES`. Mỗi ký tự có thể được chia thành hai nhóm bằng nhau, một nhóm cho mỗi nửa chuỗi cuối cùng. 

Tại sao nó hoạt động: Điều bất biến là các giao dịch hoán đổi liền kề bảo toàn tần số của mọi ký tự. Một chuỗi có dạng`t + t`yêu cầu mọi ký tự phải có mặt số lần chẵn vì số lần xuất hiện của nó được chia đều cho hai nửa giống hệt nhau. Nếu tất cả các số đếm đều chẵn, chúng ta có thể xây dựng hai nửa bằng cách lấy chính xác một nửa số lần xuất hiện của mỗi ký tự cho mỗi bên. Vì bất kỳ hoán vị nào cũng có thể đạt được bằng cách sử dụng các giao dịch hoán đổi liền kề nên luôn có thể đạt được biểu mẫu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()
        freq = [0] * 26

        for ch in s:
            freq[ord(ch) - ord('a')] += 1

        ok = True
        for count in freq:
            if count % 2:
                ok = False
                break

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đọc số lượng trường hợp thử nghiệm và xử lý từng chuỗi một cách độc lập. Mảng tần số có một khe cho mỗi chữ cái viết thường, do đó việc cập nhật cần có thời gian không đổi cho mỗi ký tự. 

Vòng lặp cuối cùng kiểm tra khả năng chia hết cho hai. Không cần phải xây dựng chuỗi được sắp xếp lại vì sự tồn tại của việc sắp xếp lại như vậy chỉ phụ thuộc vào số lượng ký tự. 

Việc triển khai sẽ tránh mọi giả định về thứ tự ban đầu của các ký tự. Một lỗi phổ biến là so sánh nửa đầu và nửa sau của chuỗi đầu vào, nhưng việc hoán đổi được phép có nghĩa là thứ tự đầu vào không có ý nghĩa đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
abba
```| Bước | Chuỗi | Số lượng ký tự | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | abba | a:2, b:2 | Tiếp tục | 
| Kiểm tra | abba | 2 là chẵn | Tiếp tục | 
| Kiểm tra b | abba | 2 là chẵn | CÓ | 

Chuỗi đã có tần số cân bằng. Thuật toán không cần thực hiện hoán đổi vì nó chỉ kiểm tra xem có tồn tại sự sắp xếp hợp lệ hay không. 

### Mẫu 2 

đầu vào:```
2
a
abcdeadbceff
```Trường hợp thử nghiệm đầu tiên: 

| Bước | Chuỗi | Số lượng ký tự | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | một | một:1 | Đã tìm thấy số lẻ | 
| Cuối cùng | một | Không thể chia đều | KHÔNG | 

Trường hợp thử nghiệm thứ hai: 

| Bước | Chuỗi | Số lượng ký tự | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | abcdeadbceff | a:2,b:2,c:2,d:2,e:2,f:2 | Tiếp tục | 
| Kiểm tra tất cả các chữ cái | abcdeadbceff | Mọi số đếm đều chẵn | CÓ | 

Ví dụ thứ hai chứng minh rằng thứ tự ban đầu là không liên quan. Các ký tự ban đầu không được sắp xếp thành hai nửa bằng nhau, nhưng việc hoán đổi có thể sắp xếp lại chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được tính một lần và mỗi mục nhập bảng chữ cái được kiểm tra một lần. | 
| Không gian | O(1) | Mảng tần số luôn chỉ chứa 26 số nguyên. | 

Tổng kích thước đầu vào được giới hạn ở`2 * 10^5`, do đó quét tuyến tính dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.getvalue()

# provided samples
assert run("""1
abba
""") == "YES", "sample 1"

assert run("""2
a
abcdeadbceff
""") == "NO\nYES", "sample 2"

# minimum size
assert run("""1
a
""") == "NO", "single character cannot be duplicated"

# all equal values
assert run("""1
aaaaaa
""") == "YES", "all characters split evenly"

# odd frequency hidden among many pairs
assert run("""1
aabbcccd
""") == "NO", "odd counts must fail"

# larger boundary case
assert run("""1
""" + "ab" * 100000 + "\n") == "YES", "large valid input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`NO`| Xử lý độ dài tối thiểu | 
|`aaaaaa`|`YES`| Tất cả các nhân vật xuất hiện đồng đều | 
|`aabbcccd`|`NO`| Phát hiện một tần số lẻ duy nhất | 
|`ab`lặp đi lặp lại 100000 lần |`YES`| Hiệu suất tuyến tính trên kích thước tối đa | 

## Vỏ cạnh 

Đối với trường hợp ký tự đơn:```
1
a
```Tần số của`a`là một. Thuật toán phát hiện một nửa không thể nhận được các ký tự giống như nửa còn lại nên nó xuất ra`NO`. 

Đối với chuỗi có độ dài chẵn có ký tự tần số lẻ:```
1
aabbcdef
```Số lượng của`c`,`d`,`e`, Và`f`tất cả đều là một Thuật toán từ chối chuỗi vì những ký tự đó không thể phân chia thành hai nửa giống hệt nhau. 

Đối với một chuỗi đã thỏa mãn điều kiện:```
1
abba
```Số lượng của`a`Và`b`cả hai đều là hai. Thuật toán chấp nhận nó, mặc dù nó không bao giờ xây dựng được sự sắp xếp cuối cùng. Một sự sắp xếp cuối cùng có thể là`ba + ba`. 

Đối với đầu vào hợp lệ lớn:```
1
abababab...
```Ở đâu`ab`được lặp lại nhiều lần, thuật toán chỉ thực hiện một lần đếm ký tự và một lần kiểm tra kích thước không đổi. Nó không phụ thuộc vào số lượng giao dịch hoán đổi hoặc sắp xếp có thể có.
