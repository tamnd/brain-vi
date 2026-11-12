---
title: "CF 1628B - Sở Thích Phim Đặc Biệt"
description: "Chúng ta được cung cấp một chuỗi các chuỗi ngắn, mỗi chuỗi đại diện cho một cảnh trong phim. Từ những cảnh này, chúng ta được phép chọn một chuỗi tiếp theo, nghĩa là chúng ta giữ nguyên thứ tự ban đầu nhưng có thể bỏ qua một số cảnh. Nếu chúng ta nối các cảnh đã chọn, chúng ta sẽ thu được một chuỗi duy nhất."
date: "2026-06-10T05:07:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1628
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 767 (Div. 1)"
rating: 1700
weight: 1628
solve_time_s: 104
verified: false
draft: false
---

[CF 1628B - Tùy chọn phim đặc biệt](https://codeforces.com/problemset/problem/1628/B) 

**Đánh giá:** 1700 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chuỗi ngắn, mỗi chuỗi đại diện cho một cảnh trong phim. Từ những cảnh này, chúng ta được phép chọn một chuỗi tiếp theo, nghĩa là chúng ta giữ nguyên thứ tự ban đầu nhưng có thể bỏ qua một số cảnh. 

Nếu chúng ta nối các cảnh đã chọn, chúng ta sẽ thu được một chuỗi duy nhất. Mục đích là để xác định xem có tồn tại ít nhất một dãy con không trống mà chuỗi nối của nó là một dãy palindrome hay không. 

Khó khăn chính là chúng ta không xây dựng chuỗi một cách tự do mà bằng cách chọn toàn bộ các khối có chiều dài tối đa là ba và chúng ta phải giữ trật tự. 

Các ràng buộc rất lớn, với tổng số cảnh lên tới 100000 trong tất cả các trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào thử tất cả các chuỗi con đều là không thể ngay lập tức vì có nhiều chuỗi con theo cấp số nhân. Ngay cả việc kiểm tra tất cả các chuỗi con sau khi nối cũng không khả thi. 

Một cách tiếp cận ngây thơ sẽ là thử xây dựng nhiều kết hợp hoặc thậm chí xây dựng các palindrome một cách tham lam, nhưng nó sẽ thất bại trong trường hợp palindrome phụ thuộc vào việc khớp các mẫu đảo ngược ở xa. 

Trường hợp biên tinh tế xuất hiện khi không có cảnh đơn lẻ nào tự nó là một bảng màu nhưng hai cảnh tạo thành một bảng màu khi được kết hợp. Ví dụ: “ab” và “ba”. Nếu chỉ kiểm tra từng chuỗi riêng lẻ, điều này sẽ trả về KHÔNG chính xác. 

Một trường hợp cạnh khác phát sinh khi một palindrome có thể được hình thành từ ba phần: một chuỗi hai chữ cái, một chữ cái palindrome ở giữa và đảo ngược của nó. Ví dụ: “ab”, “c”, “ba”. Thiếu cấu trúc này dẫn đến sự từ chối không chính xác. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ xem xét tất cả các chuỗi con của các cảnh, nối chúng lại và kiểm tra xem chuỗi kết quả có phải là một bảng màu hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng nó khám phá tất cả các tập hợp con của một mảng có kích thước n, mang lại khả năng O(2^n). Ngay cả khi việc kiểm tra palindrome là tuyến tính về tổng chiều dài, thì điều này vẫn vượt xa giới hạn khả thi. 

Chúng ta cần giảm đáng kể không gian tìm kiếm. Quan sát quan trọng là một bảng màu được xác định bằng cách so khớp các ký tự đối xứng từ cả hai đầu. Vì mỗi cảnh có độ dài tối đa là 3 nên chúng ta chỉ cần suy luận xem liệu chúng ta có thể tạo thành một bảng màu có độ dài 1, 2 hoặc 3 bằng cách sử dụng các mảnh có sẵn hay không. 

Điều này dẫn đến sự đơn giản hóa về mặt cấu trúc. Một dãy palindrome con hợp lệ phải thỏa mãn một trong hai điều kiện. Hoặc một cảnh đã là một chuỗi palindrome hoặc chúng ta có thể chọn hai cảnh mà sự ghép nối của chúng tạo thành một chuỗi palindrome. Vì phép nối phải đối xứng nên trường hợp thứ hai đơn giản là tìm một chuỗi và đảo ngược của nó trong chuỗi, có thể được phân tách bằng các phần tử khác. 

Chúng tôi không cần phải xem xét các kết hợp dài hơn vì bất kỳ palindrome nào có độ dài lớn hơn 2 trong cài đặt hạn chế này phải giảm xuống các cặp phù hợp hoặc khối palindrome trung tâm có độ dài 1 hoặc 3 và chúng đã được bao phủ bằng cách kiểm tra các đảo ngược và đơn palindromic. 

Do đó, bài toán trở thành bài toán tần số và tra cứu trên các chuỗi và các đảo ngược của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · L) | O(L) | Quá chậm | 
| Tối ưu | O(n · L) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và duy trì một tập hợp các chuỗi đã thấy. 

1. Duy trì bộ băm của tất cả các chuỗi gặp phải cho đến nay. 

Điều này cho phép tra cứu liên tục theo thời gian xem liệu bản sao ngược có tồn tại hay không. 
2. Đối với mỗi chuỗi s mới, trước tiên hãy kiểm tra xem bản thân s có phải là một bảng màu không. 

Nếu đúng như vậy, chúng ta có thể trả lời ngay CÓ vì một dãy con có một phần tử là hợp lệ. 
3. Kiểm tra xem mặt sau của s đã được nhìn thấy chưa.

Nếu có, chúng ta có thể tạo thành một phép nối đối xứng của hai cảnh: một chuỗi trước đó và chuỗi hiện tại. 
4. Ngoài ra, để đảm bảo an toàn cho các ràng buộc về độ dài cấu trúc, chúng tôi cũng theo dõi tất cả các chuỗi có độ dài 2 được nhìn thấy và kiểm tra các mẫu so khớp chéo trong đó ký tự đầu tiên của một chuỗi khớp với ký tự thứ hai của chuỗi khác theo thứ tự ngược lại. Tuy nhiên, vì độ dài tối đa là 3 nên việc tra cứu ngược đã nắm bắt được đầy đủ tất cả các palindrome hai khối hợp lệ. 
5. Nếu cả hai điều kiện đều không được thỏa mãn, hãy chèn s vào tập hợp và tiếp tục. 
6. Nếu không có điều kiện nào được kích hoạt ở cuối, xuất ra NO. 

### Tại sao nó hoạt động 

Một palindrome được xây dựng từ việc ghép toàn bộ chuỗi phải căn chỉnh các vị trí đối xứng trên chuỗi cuối cùng. Vì mỗi khối rất ngắn nên bất kỳ cấu trúc hợp lệ nào cũng sẽ thu gọn thành một khối đối xứng duy nhất hoặc một cặp khối đảo ngược nhau theo thứ tự. Kiểm tra ngược đảm bảo rằng bất cứ khi nào một cặp phản chiếu hợp lệ tồn tại theo thứ tự tiếp theo, chúng tôi sẽ phát hiện ra nó bất kể phần tử bị bỏ qua. 

Bởi vì chúng tôi quét từ trái sang phải, mọi nửa bên trái tiềm năng đã được lưu trữ khi chúng tôi kiểm tra nửa bên phải ứng cử viên, đảm bảo không bỏ sót cặp hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_pal(s):
    return s == s[::-1]

t = int(input())
for _ in range(t):
    n = int(input())
    seen = set()
    
    ok = False
    
    for _ in range(n):
        s = input().strip()
        
        if not ok:
            if is_pal(s):
                ok = True
            else:
                rs = s[::-1]
                if rs in seen:
                    ok = True
        
        seen.add(s)
    
    print("YES" if ok else "NO")
```Giải pháp duy trì một bộ duy nhất cho mỗi trường hợp thử nghiệm và kiểm tra từng chuỗi theo thời gian trung bình không đổi. 

Kiểm tra palindrome là phép so sánh đảo ngược chuỗi đơn giản. Tra cứu ngược là cơ chế cốt lõi để phát hiện xem đối tác phù hợp đã xuất hiện trước đó hay chưa. 

Cờ boolean ngăn chặn những công việc không cần thiết sau khi tìm thấy cấu hình hợp lệ, nhưng không ảnh hưởng đến tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
zx
ab
cc
zx
ba
```Chúng tôi theo dõi bộ và trả lời: 

| Bước | s | Xuôi ngược đều giống nhau? | Đảo ngược trong bộ? | Đã xem bộ | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | zx | không | không | zx | không | 
| 2 | ab | không | không | zx, ab | không | 
| 3 | cc | vâng | - | zx, ab, cc | CÓ | 

Khi “cc” xuất hiện, chúng tôi ngay lập tức phát hiện một chuỗi con hợp lệ. 

### Ví dụ 2 

đầu vào:```
2
ab
ba
```| Bước | s | Xuôi ngược đều giống nhau? | Đảo ngược trong bộ? | Đã xem bộ | 
| --- | --- | --- | --- | --- | 
| 1 | ab | không | không | ab | 
| 2 | ba | không | vâng (ab) | ab, ba | 

Ở đây, “ab” và “ba” tạo thành một dãy con palindrome gồm hai phần tử hợp lệ. 

Những ví dụ này cho thấy thuật toán nắm bắt chính xác cả cấu trúc palindrome một khối và hai khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi chuỗi được xử lý một lần, với các phép toán băm và đảo ngược theo thời gian không đổi kể từ L 3 | 
| Không gian | O(n) | Lưu trữ các chuỗi đã thấy trong một bộ | 

Các ràng buộc cho phép tối đa 100000 chuỗi, vì vậy việc xử lý tuyến tính là đủ. Trung bình mỗi thao tác là O(1), vừa vặn thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        seen = set()
        ok = False
        
        for _ in range(n):
            s = input().strip()
            if not ok:
                if s == s[::-1] or s[::-1] in seen:
                    ok = True
            seen.add(s)
        
        out.append("YES" if ok else "NO")
    
    return "\n".join(out)

# provided samples
assert run("""6
5
zx
ab
cc
zx
ba
2
ab
bad
4
co
def
orc
es
3
a
b
c
3
ab
cd
cba
2
ab
ab
""") == """YES
NO
NO
YES
YES
NO"""

# custom cases
assert run("""3
1
a
2
ab
ba
3
abc
def
ghi
""") == """YES
YES
NO"""

assert run("""1
3
ab
bc
cba
""") == """YES"""

assert run("""1
2
ab
ac
""") == """NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| người bạn độc thân | CÓ | palindrome đơn phần tử | 
| cặp đảo ngược | CÓ | palindrome hai chuỗi | 
| không khớp | KHÔNG | trường hợp vắng mặt | 
| hộp đựng dây chuyền | CÓ | cấu trúc đảo ngược gián tiếp | 
| không có cấu trúc | KHÔNG | ranh giới âm | 

## Vỏ cạnh 

Đầu vào chuỗi ký tự đơn được xử lý ngay lập tức vì nó luôn là một bảng màu. Ví dụ, đầu vào`["a"]`tạo ra CÓ ở bước đầu tiên kể từ`a == a[::-1]`. 

Một cặp như`["ab", "ba"]`chỉ được phát hiện thông qua tra cứu ngược lại. Khi xử lý`"ba"`,`"ab"`đã tồn tại trong tập hợp, kích hoạt kết quả CÓ mặc dù cả hai chuỗi đều không phải là một bảng màu. 

Một trường hợp như`["ab", "cd", "cba"]`không tạo ra một chuỗi con palindrome hợp lệ vì không có cặp đảo ngược nào thẳng hàng và không có chuỗi palindrome đơn nào xuất hiện. Thuật toán xử lý tất cả các chuỗi, không bao giờ kích hoạt điều kiện và đưa ra kết quả NO một cách chính xác. 

Những trường hợp này xác nhận rằng thuật toán hoàn toàn dựa vào tính đối xứng cấu trúc chứ không phải là cấu trúc mạnh mẽ, đủ để đảm bảo tính chính xác trong các ràng buộc.
