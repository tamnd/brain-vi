---
title: "CF 1144A - Dây đa dạng"
description: "Chúng ta được cung cấp một số chuỗi ngắn và với mỗi chuỗi chúng ta cần quyết định xem liệu nó có tạo thành một khối bảng chữ cái liên tục duy nhất mà không có bất kỳ khoảng trống hoặc sự lặp lại nào hay không. Hãy coi bảng chữ cái viết thường như một dòng từ ‘a’ đến ‘z’."
date: "2026-06-12T03:30:53+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1144
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 550 (Div. 3)"
rating: 800
weight: 1144
solve_time_s: 78
verified: true
draft: false
---

[CF 1144A - Dây đa dạng](https://codeforces.com/problemset/problem/1144/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi ngắn và với mỗi chuỗi chúng ta cần quyết định xem liệu nó có tạo thành một khối bảng chữ cái liên tục duy nhất mà không có bất kỳ khoảng trống hoặc sự lặp lại nào hay không. Hãy coi bảng chữ cái viết thường như một dòng từ ‘a’ đến ‘z’. Một chuỗi hợp lệ nếu khi bạn nhìn vào các chữ cái trong đó, chúng tương ứng với một số đoạn liền kề của dòng này và mỗi chữ cái xuất hiện đúng một lần. 

Vì vậy, một chuỗi như “dabcef” hoạt động vì các chữ cái của nó là {a, b, c, d, e, f}, tạo thành một khoảng liên tục trong bảng chữ cái. Thứ tự bên trong chuỗi không quan trọng đối với định nghĩa, chỉ có tập hợp các ký tự mới quan trọng. Mặt khác, “az” không thành công vì nó nhảy qua tất cả các chữ cái giữa ‘a’ và ‘z’, còn “babc” không thành công vì nó lặp lại ‘b’. 

Mỗi trường hợp thử nghiệm là độc lập và chúng tôi chỉ cần xuất ra “Có” nếu chuỗi thỏa mãn các điều kiện này và “Không” nếu ngược lại. 

Các ràng buộc rất nhỏ: nhiều nhất là 100 chuỗi, mỗi chuỗi có độ dài tối đa là 100. Điều này ngay lập tức loại trừ mọi thứ ngoài công việc tuyến tính trên mỗi chuỗi. Ngay cả cách tiếp cận O(n^2) trên mỗi chuỗi vẫn có thể được chấp nhận, nhưng bất cứ điều gì phức tạp hơn việc kiểm tra tần số và quét bảng chữ cái đều là chi phí không cần thiết. 

Các trường hợp thất bại chính thường đến từ việc quên một trong hai điều kiện. Một là trùng lặp, ví dụ “aa” nên bị từ chối mặc dù nó là loại ký tự đơn. Thứ hai là các khoảng trống trong bảng chữ cái, ví dụ “ac” phải bị từ chối mặc dù cả hai chữ cái đều khác nhau vì thiếu chữ ‘b’ giữa chúng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là thử mọi chữ cái bắt đầu có thể có của một đoạn bảng chữ cái liền kề và kiểm tra xem chuỗi có thể được sắp xếp lại vào đoạn đó hay không. Đối với mỗi phân đoạn ứng cử viên, chúng tôi sẽ so sánh các ký tự được sắp xếp hoặc xây dựng mảng tần số và xác minh sự bằng nhau. Điều này hoạt động, nhưng nó là quá mức cần thiết. 

Điều quan trọng cần lưu ý là một chuỗi hợp lệ phải hoạt động giống như một “đường dẫn” trên dòng bảng chữ cái. Nếu chúng ta sắp xếp hoặc suy luận về tập hợp các ký tự thì các chữ cái tối thiểu và tối đa sẽ xác định phân đoạn duy nhất có thể. Nếu chuỗi hợp lệ thì mọi ký tự giữa các cực này phải xuất hiện chính xác một lần và không được phép trùng lặp. 

Điều này làm giảm vấn đề thành một kiểm tra đơn giản: đảm bảo tính duy nhất, tính toán các ký tự tối thiểu và tối đa và xác minh rằng số lượng ký tự riêng biệt bằng độ dài khoảng cách giữa chúng. 

Điều này hiệu quả vì tất cả các phép toán đều tuyến tính theo độ dài chuỗi và kích thước bảng chữ cái không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 · n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) mỗi chuỗi | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi chuỗi, chúng tôi xử lý nó một cách độc lập. 

1. Chúng tôi quét chuỗi và ghi lại xem chúng tôi có thấy bất kỳ ký tự lặp lại nào không. Nếu một ký tự lặp lại, chuỗi ngay lập tức không hợp lệ. Điều này là bắt buộc vì các bản sao vi phạm quy tắc “mỗi chữ cái chính xác một lần”. 
2. Trong khi quét, chúng tôi cũng theo dõi ký tự tối thiểu và tối đa gặp phải theo thứ tự bảng chữ cái. Điều này cung cấp cho chúng tôi các điểm cuối phân khúc tiềm năng. 
3. Sau khi quét, chúng tôi tính toán xem chúng tôi đã nhìn thấy bao nhiêu ký tự riêng biệt. Nếu phát hiện thấy sự trùng lặp, chúng tôi đã biết câu trả lời là “Không”. 
4. Bây giờ chúng ta so sánh kích thước của khoảng được xác định bởi các ký tự tối thiểu và tối đa. Độ dài khoảng là`(max_char - min_char + 1)`. 
5. Nếu số lượng ký tự riêng biệt bằng độ dài khoảng này thì mỗi ký tự trong phạm vi phải xuất hiện chính xác một lần, vì vậy chúng tôi trả về “Có”. Nếu không, chúng tôi trả về “Không”. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là một chuỗi hợp lệ phải biểu thị một khoảng hoàn chỉnh trên bảng chữ cái mà không thiếu phần tử. Nếu cấm trùng lặp thì mỗi ký tự đóng góp chính xác một vị trí trong khoảng đó. Nếu tập hợp các ký tự được quan sát có kích thước k, nhưng kéo dài một khoảng lớn hơn k thì ít nhất một ký tự ở giữa bị thiếu, phá vỡ sự liền kề. Ngược lại, nếu kích thước khớp với độ dài khoảng và không có phần trùng lặp thì tập hợp phải chính xác là khoảng, vì không có chỗ cho khoảng trống hoặc phần bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_diverse(s: str) -> bool:
    seen = [False] * 26
    mn = 26
    mx = -1

    for ch in s:
        idx = ord(ch) - ord('a')
        if seen[idx]:
            return False
        seen[idx] = True
        mn = min(mn, idx)
        mx = max(mx, idx)

    # number of distinct characters
    cnt = sum(seen)

    return (mx - mn + 1) == cnt

def main():
    n = int(input())
    for _ in range(n):
        s = input().strip()
        print("Yes" if is_diverse(s) else "No")

if __name__ == "__main__":
    main()
```Việc thực hiện trực tiếp theo thuật toán. Mảng boolean`seen`đảm bảo phát hiện các bản sao liên tục. Theo dõi`mn`Và`mx`tránh việc sắp xếp hoặc xây dựng lại phạm vi dựa trên tập hợp. So sánh cuối cùng kiểm tra xem các ký tự có lấp đầy toàn bộ khoảng trống mà không có khoảng trống hay không. 

Một điểm tinh tế là chúng ta chỉ dựa vào tập hợp các ký tự chứ không dựa vào thứ tự của chúng trong chuỗi. Điều này là an toàn vì điều kiện chỉ phụ thuộc vào độ kề trong bảng chữ cái chứ không phụ thuộc vào độ kề trong chính chuỗi. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào: một hợp lệ và một không hợp lệ. 

### Ví dụ 1:`"dabcef"`| Bước | Nhân vật | Đã thấy kích thước đặt | Chỉ số tối thiểu | Chỉ số tối đa | Nhân bản? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | d | 1 | 3 | 3 | Không | 
| 2 | một | 2 | 0 | 3 | Không | 
| 3 | b | 3 | 0 | 3 | Không | 
| 4 | c | 4 | 0 | 3 | Không | 
| 5 | e | 5 | 0 | 4 | Không | 
| 6 | f | 6 | 0 | 5 | Không | 

Sau khi xử lý,`cnt = 6`,`mn = 0`,`mx = 5`. Kích thước khoảng là`5 - 0 + 1 = 6`, khớp với số ký tự riêng biệt nên kết quả là “Có”. 

Điều này xác nhận rằng các đầu vào không có thứ tự vẫn tạo thành một khối bảng chữ cái liền kề. 

### Ví dụ 2:`"babc"`| Bước | Nhân vật | Đã thấy kích thước đặt | Chỉ số tối thiểu | Chỉ số tối đa | Nhân bản? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | b | 1 | 1 | 1 | Không | 
| 2 | một | 2 | 0 | 1 | Không | 
| 3 | b | - | - | - | Có | 

Ở ký tự thứ ba, chúng tôi phát hiện ra rằng 'b' đã được nhìn thấy nên chúng tôi ngay lập tức trả về "Không". 

Điều này chứng tỏ rằng chỉ sự trùng lặp thôi cũng đủ để vô hiệu hóa một chuỗi, ngay cả khi điều kiện khoảng cách bảng chữ cái có thể được thỏa mãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi chuỗi được quét một lần và tất cả các thao tác là O(1) cho mỗi ký tự | 
| Không gian | O(1) | Chỉ sử dụng một mảng có độ dài 26 cố định | 

Cho rằng tổng kích thước đầu vào tối đa là 10.000 ký tự, điều này chạy thoải mái trong giới hạn. Các phép toán hệ số không đổi là tối thiểu và bị chi phối bởi việc lập chỉ mục mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    def is_diverse(s: str) -> bool:
        seen = [False] * 26
        mn = 26
        mx = -1

        for ch in s:
            idx = ord(ch) - ord('a')
            if seen[idx]:
                return False
            seen[idx] = True
            mn = min(mn, idx)
            mx = max(mx, idx)

        cnt = sum(seen)
        return (mx - mn + 1) == cnt

    n = int(input())
    for _ in range(n):
        s = input().strip()
        print("Yes" if is_diverse(s) else "No")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""8
fced
xyz
r
dabcef
az
aa
bad
babc
""") == """Yes
Yes
Yes
Yes
No
No
No
No"""

# single letter
assert run("""1
a
""") == "Yes"

# simple invalid gap
assert run("""1
ac
""") == "No"

# full alphabet segment shuffled
assert run("""1
cba
""") == "Yes"

# duplicate inside otherwise valid range
assert run("""1
abca
""") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn`a`| Có | trường hợp có chiều dài tối thiểu | 
|`ac`| Không | thiếu chữ giữa | 
|`cba`| Có | phân đoạn hợp lệ được xáo trộn | 
|`abca`| Không | phát hiện trùng lặp | 

## Vỏ cạnh 

Một chuỗi có độ dài bằng 1 luôn hợp lệ vì nó tạo thành một đoạn liền kề không có khoảng trống và không trùng lặp. Thuật toán xử lý việc này vì`mn == mx`Và`cnt == 1`, vì vậy việc kiểm tra khoảng thời gian đã trôi qua. 

Một trường hợp như “az” chứng minh tại sao việc kiểm tra phạm vi là cần thiết. Quá trình quét tìm thấy`mn = 0`,`mx = 25`, Và`cnt = 2`. Kích thước khoảng là 26, không khớp, do đó, nó trả về chính xác “Không” ngay cả khi không có bản sao. 

Trường hợp như “aa” thể hiện quy tắc trùng lặp. Ký tự thứ hai kích hoạt sự từ chối ngay lập tức trước khi logic phạm vi quan trọng, đảm bảo tính chính xác mà không cần dựa vào kiểm tra khoảng thời gian.
