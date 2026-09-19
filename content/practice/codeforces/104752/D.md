---
title: "CF 104752D - Xác định thông báo Palindrome"
description: "Chúng ta được cung cấp một chuỗi duy nhất bao gồm các ký tự Latinh viết thường, nhưng nó cũng có thể chứa các chữ số hoặc ký tự khác trong các ví dụ về định dạng đầu vào, vì vậy chúng ta coi nó như một chuỗi các ký hiệu."
date: "2026-06-29T01:24:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104752
codeforces_index: "D"
codeforces_contest_name: "Concurso de programaci\u00f3n ANIEI 2023"
rating: 0
weight: 104752
solve_time_s: 63
verified: true
draft: false
---

[CF 104752D - Xác định thông báo Palindrome](https://codeforces.com/problemset/problem/104752/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất bao gồm các ký tự Latinh viết thường, nhưng nó cũng có thể chứa các chữ số hoặc ký tự khác trong các ví dụ về định dạng đầu vào, vì vậy chúng ta coi nó như một chuỗi các ký hiệu. Từ chuỗi này, chúng ta được phép chọn một số bộ ký tự và sắp xếp lại chúng theo bất kỳ thứ tự nào. Mục đích là để quyết định xem có tồn tại một lựa chọn không trống của các ký tự này có thể được hoán vị thành một bảng màu hay không. 

Một palindrome đòi hỏi sự đối xứng xung quanh tâm của nó. Sự đối xứng đó đặt ra một điều kiện nghiêm ngặt về số lần mỗi ký tự có thể xuất hiện. Các nhân vật phải ghép đôi ở các phía đối diện nhau và tối đa một nhân vật được phép giữ nguyên không ghép đôi ở vị trí trung tâm. 

Độ dài chuỗi lên tới 100000, điều này ngay lập tức gợi ý rằng mọi giải pháp đều phải chạy theo thời gian tuyến tính hoặc gần với thời gian đó. Một cách tiếp cận bậc hai hoặc hàm mũ sẽ là không thể bởi vì ngay cả một phương pháp đơn giản$O(n^2)$kiểm tra đã có sẵn$10^{10}$hoạt động trong trường hợp xấu nhất. 

Một trường hợp khó nhận thấy là chúng ta không bắt buộc phải sử dụng tất cả các ký tự. Điều này quan trọng vì nó loại bỏ ràng buộc “chẵn lẻ tần số” thông thường đối với toàn bộ chuỗi. Ví dụ: nếu chúng ta có chính xác một ký tự xuất hiện một lần, chúng ta có thể chỉ cần bỏ qua nó và tạo thành một bảng màu từ phần còn lại, bảng màu này luôn hợp lệ nếu có ít nhất một ký tự tồn tại. Một trường hợp cạnh khác là một chuỗi có tất cả các ký tự riêng biệt. Thậm chí ở đó, chúng ta có thể chọn bất kỳ ký tự đơn lẻ nào và tạo thành một bảng màu dài 1. 

Một sai lầm ngây thơ là cho rằng chúng ta phải sử dụng tất cả các ký tự. Ví dụ, đầu vào`abc`sẽ bị từ chối không chính xác nếu người ta cho rằng tất cả các chữ cái phải được sử dụng, nhưng trên thực tế chúng ta có thể chọn`a`một mình và tạo thành một palindrome. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng kiểm tra tất cả các tập hợp con của các ký tự và đối với mỗi tập hợp con, hãy kiểm tra xem nó có thể được hoán vị thành một bảng màu hay không. Ngay cả khi chúng ta bỏ qua thứ tự và chỉ xem xét số lượng ký tự thì số lượng tập hợp con vẫn là$2^n$, điều này hoàn toàn không thể thực hiện được đối với$n = 10^5$. Ngay cả việc giảm xuống các vectơ tần số cũng không giúp ích gì vì không gian trạng thái vẫn theo cấp số nhân. 

Quan sát quan trọng là vì chúng ta có thể loại bỏ các ký tự một cách tự do nên yêu cầu duy nhất để xây dựng một bảng màu là chúng ta có thể chọn ít nhất một ký tự. Bất kỳ ký tự đơn nào tạo thành một bảng màu hợp lệ có độ dài bằng một. Điều này ngay lập tức giải quyết vấn đề: miễn là chuỗi không trống, câu trả lời luôn là CÓ. 

Quan điểm sâu hơn dự định là tính khả thi của palindrom phụ thuộc vào các ràng buộc chẵn lẻ, nhưng những ràng buộc đó luôn có thể được thỏa mãn bằng cách chọn một lần xuất hiện ký tự đơn lẻ. Không có hạn chế nào buộc chúng tôi phải sử dụng nhiều ký tự riêng biệt. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem chuỗi đầu vào có chứa ít nhất một ký tự hay không, điều này luôn đúng với các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con | O(2^n) | O(n) | Quá chậm | 
| Quan sát tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng tối ưu 

1. Đọc chuỗi đầu vào$s$. Chúng ta chỉ cần biết nó có trống hay không. 
2. Nếu$s$có độ dài ít nhất là 1, ngay lập tức kết luận rằng chúng ta có thể tạo thành một bảng màu. 
3. In CÓ. 

Lý do đằng sau bước 2 là bất kỳ ký tự đơn lẻ nào cũng đã là một bảng màu. Vì chúng ta được phép chọn một tập hợp con các chữ cái nên chúng ta luôn có thể chọn một lần xuất hiện và tạo thành một bảng màu hợp lệ có độ dài bằng một. 

## Tại sao nó hoạt động 

Điều kiện palindrome yêu cầu tất cả các ký tự ngoại trừ một ký tự có thể có tần số chẵn trong nhiều tập hợp đã chọn. Nếu chúng ta chọn chính xác một ký tự thì tần số của nó là 1 và nó thỏa mãn điều kiện một cách tầm thường vì nó là ký tự ở giữa. Do đó, mọi tập hợp có kích thước 1 không trống đều hợp lệ, có nghĩa là mọi chuỗi đầu vào không trống sẽ tự động chứa lựa chọn hợp lệ. 

Điều bất biến là chúng ta không bắt buộc phải sử dụng tất cả các ký tự mà chỉ thể hiện một tập hợp con có thể được sắp xếp lại thành một bảng màu. Tập con đơn luôn thỏa mãn bất biến này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().rstrip("\n")
    if len(s) > 0:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc chuỗi đầu vào và kiểm tra độ dài của nó. Không cần đếm tần số vì khả năng loại bỏ các ký tự làm tầm thường hóa các ràng buộc chẵn lẻ. Điểm tinh tế duy nhất là loại bỏ dòng mới một cách chính xác; nếu không, việc kiểm tra độ dài vẫn hoạt động nhưng có thể bao gồm ký tự dòng mới tùy thuộc vào môi trường. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`3aab`Chúng tôi chỉ theo dõi xem liệu chúng tôi có thể tạo thành một tập hợp con palindrome hay không. 

| Bước | Chuỗi hiện tại | Hành động | Lý do | 
| --- | --- | --- | --- | 
| 1 | 3aab | đọc đầu vào | đầu vào tồn tại | 
| 2 | 3aab | kiểm tra độ dài | không trống | 
| 3 | CÓ | đầu ra | tồn tại tập hợp ký tự đơn | 

Điều này cho thấy rằng mặc dù chuỗi chứa nhiều ký hiệu riêng biệt nhưng chúng ta có thể chọn một ký tự đơn lẻ như`a`. 

### Ví dụ 2:`3abc`| Bước | Chuỗi hiện tại | Hành động | Lý do | 
| --- | --- | --- | --- | 
| 1 | 3abc | đọc đầu vào | đầu vào tồn tại | 
| 2 | 3abc | kiểm tra độ dài | không trống | 
| 3 | CÓ | đầu ra | chọn một ký tự | 

Điều này xác nhận rằng việc thiếu các ký tự lặp lại không thành vấn đề vì việc lựa chọn tập hợp con cho phép chúng ta bỏ qua các ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét một lần để đọc chuỗi | 
| Không gian | O(1) | không có cấu trúc dữ liệu phụ trợ | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện một lượng công việc không đổi sau khi đọc dữ liệu đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    s = sys.stdin.readline().rstrip("\n")
    print("YES" if len(s) > 0 else "NO")

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("3aab\n") == "YES"
assert run("3abc\n") == "YES"
assert run("6aaaaaa\n") == "YES"

# custom cases
assert run("a\n") == "YES", "single character"
assert run("z\n") == "YES", "any single char works"
assert run("1234567890\n") == "YES", "mixed digits still valid"
assert run("\n") == "NO", "empty string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`| CÓ | trường hợp không trống tối thiểu | 
|`"z"`| CÓ | bất kỳ biểu tượng nào cũng có tác dụng | 
|`"1234567890"`| CÓ | ký tự không phải chữ cái không liên quan | 
|`""`| KHÔNG | trường hợp cạnh đầu vào trống | 

## Vỏ cạnh 

Trường hợp chuỗi trống là điều kiện biên có ý nghĩa duy nhất. Nếu dòng đầu vào trống, thuật toán sẽ xuất ra NO một cách chính xác vì không có ký tự nào có sẵn để tạo thành một bảng màu có độ dài 1. 

Đối với đầu vào`"a"`, thuật toán đọc một chuỗi không trống, kiểm tra độ dài và ngay lập tức trả về CÓ. Tập con được chọn là`{a}`, tạo thành palindrome`"a"`. 

Đối với đầu vào`"abcde"`, thuật toán lại trả về CÓ vì nó không cố gắng thực thi tính đối xứng trên toàn bộ. Thay vào đó, nó ngầm chọn một ký tự đơn lẻ như`"c"`và tạo thành một palindrome hợp lệ. 

Tất cả các trường hợp khác đều tuân theo cùng một mẫu và quyết định luôn được xác định duy nhất bằng việc có tồn tại ít nhất một ký tự hay không.
