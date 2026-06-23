---
title: "CF 105321B - Tìm kiếm kinh nguyệt"
description: "Chúng ta được cung cấp một chuỗi ẩn có độ dài $N$, nhưng chúng ta không bao giờ có thể nhìn thấy nó một cách trực tiếp. Thay vào đó, chúng ta có thể đặt các truy vấn về chuỗi con và mỗi truy vấn sẽ hỏi liệu chuỗi con đã chọn $t = s[L..R]$ có phải là khoảng thời gian hợp lệ của toàn bộ chuỗi hay không."
date: "2026-06-22T10:52:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105321
codeforces_index: "B"
codeforces_contest_name: "2024 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 105321
solve_time_s: 60
verified: true
draft: false
---

[CF 105321B - Tìm kiếm kinh nguyệt](https://codeforces.com/problemset/problem/105321/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài ẩn$N$, nhưng chúng ta không bao giờ có thể nhìn thấy nó một cách trực tiếp. Thay vào đó, chúng ta có thể hỏi các truy vấn về chuỗi con và mỗi truy vấn sẽ hỏi liệu chuỗi con được chọn có$t = s[L..R]$là khoảng thời gian hợp lệ của toàn bộ chuỗi. 

Một chuỗi con được coi là một dấu chấm nếu toàn bộ chuỗi có thể được tạo thành bằng cách lặp lại chuỗi con đó một số nguyên lần, với ít nhất hai lần lặp lại. Nói cách khác, nếu chúng ta lấy$t$và nối nó nhiều lần, chúng ta phải xây dựng lại chính xác chuỗi đầy đủ$s$, không có sự không khớp và không có ký tự còn sót lại. 

Nhiệm vụ của chúng ta không phải là khôi phục chuỗi. Chúng ta chỉ cần quyết định có bao nhiêu truy vấn cần thiết trong trường hợp xấu nhất để xác định xem chuỗi chưa biết có phải là chuỗi định kỳ hay không và cũng đưa ra một tập hợp truy vấn cụ thể đạt được mức tối thiểu này. 

Mỗi truy vấn kiểm tra một cách hiệu quả xem toàn bộ chuỗi có được xây dựng từ việc lặp lại một khối ứng cử viên hay không. Khó khăn chính là chúng tôi phải chọn trước tất cả các truy vấn mà không thấy bất kỳ câu trả lời nào và vẫn đảm bảo rằng sau khi nhận được phản hồi, chúng tôi luôn có thể xác định xem khoảng thời gian hợp lệ có tồn tại hay không. 

Từ quan điểm phức tạp,$N$có thể lớn như$10^6$. Điều này ngay lập tức loại trừ bất kỳ điều gì phụ thuộc vào việc khám phá tất cả các chuỗi con hoặc mô phỏng các tương tác. Lời giải phải phụ thuộc hoàn toàn vào tính chất cấu trúc của chiều dài$N$, vì chúng tôi đang thiết kế một bộ truy vấn cố định độc lập với chuỗi thực tế. 

Một trường hợp khó nhận thấy là khi$N$là nguyên tố. Trong trường hợp đó, độ dài khoảng thời gian duy nhất có thể là$1$và toàn bộ quyết định giảm xuống liệu tất cả các ký tự có giống nhau hay không. Một trường hợp cạnh khác là khi$N$có tính tổng hợp cao, bởi vì có nhiều khoảng thời gian ứng cử viên tồn tại, nhưng hầu hết chúng đều dư thừa về mặt thông tin. 

## Phương pháp tiếp cận 

Điểm khởi đầu tự nhiên là suy nghĩ về ý nghĩa của một chuỗi có tính tuần hoàn. Nếu một chuỗi bao gồm các khối lặp lại thì tồn tại một số khối có độ dài nhỏ nhất$p$sao cho chuỗi lặp lại chính xác tiền tố có độ dài của nó$p$. Bất kỳ thời hạn hợp lệ phải chia$N$, bởi vì sự lặp lại phải xếp chuỗi đều nhau. 

Điều này làm giảm cấu trúc ẩn thành ước số của$N$. Mỗi số chia$p < N$đại diện cho một khoảng thời gian tiềm năng. Nếu chúng ta biết ước số nào là các khoảng thời gian hợp lệ, chúng ta có thể trả lời ngay lập tức liệu chuỗi đó có phải là tuần hoàn hay không bằng cách kiểm tra xem có bất kỳ ước số nào trong số chúng hoạt động hay không. 

Chiến lược brute-force là truy vấn mọi chuỗi con có thể đại diện cho một dấu chấm. Vì bất kỳ khoảng thời gian hợp lệ nào cũng phải là sự lặp lại đầy đủ nên việc kiểm tra các chuỗi con có dạng là đủ$[1, p]$cho mọi$p$đó là sự chia rẽ$N$. Mỗi truy vấn cho chúng ta biết chuỗi có chính xác bao gồm các lần lặp lại của tiền tố đó hay không. Điều này đúng, nhưng số ước như vậy có thể lớn, lên tới khoảng$10^6$trong các trường hợp bệnh lý, khiến nó có khả năng tốn kém về ngân sách truy vấn. 

Quan sát quan trọng là việc kiểm tra tất cả các ước số không chỉ là đủ mà còn cần thiết trong trường hợp xấu nhất. Nếu chúng ta bỏ qua bất kỳ số chia nào$p$, chúng ta có thể xây dựng một chuỗi có khoảng thời gian hợp lệ duy nhất chính xác là$p$và tất cả các độ dài được thử nghiệm khác đều không thành công. Điều này buộc bất kỳ chiến lược đúng đắn nào phải bao gồm mọi ước số ứng cử viên dưới dạng một truy vấn riêng biệt, bởi vì mỗi ước số có thể phân biệt duy nhất một cấu hình ẩn khác nhau. 

Ngoài ra còn có sự dư thừa theo nghĩa là nếu một chuỗi có dấu chấm$p$, thì nó cũng có chu kỳ$k \cdot p$với mọi số nguyên$k$, nhưng điều này không giúp giảm tập truy vấn vì chúng ta không biết liệu các ước số nhỏ hơn có giữ nguyên hay không. Do đó, việc bỏ qua bất kỳ số chia nào đều có nguy cơ làm mất tính đầy đủ. 

Do đó, chiến lược tối ưu là truy vấn chính xác một tiền tố cho mỗi ước số thích hợp của$N$và sử dụng các câu trả lời để xác định xem ít nhất một trong số chúng có phải là khoảng thời gian hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các chuỗi con | Hàm mũ | O(1) | Không thể | 
| Hãy thử tất cả các ước số dưới dạng truy vấn | O(√N) để liệt kê các ước số | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải quyết vấn đề bằng cách liệt kê tất cả các độ dài khoảng thời gian ứng cử viên hợp lệ, chính xác là các ước số của$N$nhỏ hơn$N$. 

## Hướng dẫn thuật toán 

1. Lặp lại tất cả các số nguyên$d$từ 1 đến$N$, nhưng chỉ xét những cái chia$N$. Mỗi cái như vậy$d$đại diện cho độ dài khoảng thời gian ứng cử viên vì cấu trúc tuần hoàn hợp lệ phải xếp chuỗi đều nhau. 
2. Với mỗi ước số$d < N$, xây dựng một truy vấn trên chuỗi con$[1, d]$. Điều này kiểm tra xem toàn bộ chuỗi có bao gồm các lần lặp lại của chuỗi đầu tiên hay không$d$nhân vật. Chúng tôi hạn chế các chuỗi con tiền tố vì mọi khoảng thời gian hợp lệ đều buộc khối đầu tiên phải khớp chính xác với mẫu. 
3. Thu thập tất cả các truy vấn như vậy. Số lượng truy vấn chính xác là số ước số thích hợp của$N$. 
4. Xuất tất cả các truy vấn. Thứ tự không quan trọng vì mỗi truy vấn đều độc lập và chúng tôi không điều chỉnh dựa trên câu trả lời. 

Điểm cấu trúc quan trọng là chúng tôi không cố gắng xây dựng lại chuỗi mà chỉ để kiểm tra tính nhất quán với từng độ dài lát gạch có thể có. Mỗi ước số đóng vai trò như một giả thuyết độc lập về cấu trúc của chuỗi ẩn. 

### Tại sao nó hoạt động 

Bất kỳ khoảng thời gian hợp lệ nào của chuỗi đều phải có độ dài$p$đó là sự chia rẽ$N$và phải khớp với tiền tố của chuỗi. Do đó, mọi ứng cử viên hợp lệ đều được đưa vào bộ truy vấn của chúng tôi. 

Ngược lại, nếu chuỗi không tuần hoàn thì không có ước số nào vượt qua bài kiểm tra. Nếu là chu kỳ thì ít nhất một ước số tương ứng với chu kỳ thực sẽ vượt qua. Vì chúng tôi kiểm tra toàn diện tất cả các khả năng trên tập hợp ước số nên chúng tôi không thể bỏ lỡ cấu trúc chính xác và không có cấu trúc sai nào có thể giả mạo tính nhất quán trên tất cả các ước số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    
    divisors = []
    i = 1
    while i * i <= n:
        if n % i == 0:
            if i < n:
                divisors.append(i)
            j = n // i
            if j != i and j < n:
                divisors.append(j)
        i += 1

    divisors.sort()
    
    k = len(divisors)
    print(k)
    for d in divisors:
        print(1, d)

if __name__ == "__main__":
    solve()
```Việc thực hiện tập trung hoàn toàn vào việc liệt kê các ước số một cách hiệu quả. Vòng lặp lên đến$\sqrt{N}$đảm bảo chúng tôi tìm thấy tất cả các cặp yếu tố mà không cần quét toàn bộ phạm vi. Chúng tôi loại trừ rõ ràng$N$chính nó bởi vì một tiết phải bao gồm ít nhất hai lần lặp lại, làm cho$N$không hợp lệ như một độ dài ứng cử viên. 

Mỗi ước số được chuyển đổi thành một truy vấn có dạng$(1, d)$, đại diện cho chuỗi con tiền tố có độ dài$d$. 

## Ví dụ đã hoạt động 

Hãy xem xét$N = 4$. 

Các ước của 4 là$1, 2, 4$. Chúng tôi loại bỏ 4, để lại 1 và 2. 

| Bước | Số chia | Hành động | 
| --- | --- | --- | 
| 1 | 1 | truy vấn (1,1) | 
| 2 | 2 | truy vấn (1,2) | 

Vì thế$K = 2$và chúng tôi xuất các truy vấn có độ dài 1 và 2. 

Nếu chuỗi ẩn là "aaaa", cả hai truy vấn đều trả về có. Nếu là "abca", cả hai đều trả về số không. Nếu đó là "abab", chỉ truy vấn có độ dài 2 trả về có. 

Bây giờ hãy xem xét$N = 6$. 

Số chia là$1,2,3,6$, vì vậy chúng tôi kiểm tra$1,2,3$. 

| Bước | Số chia | Hành động | 
| --- | --- | --- | 
| 1 | 1 | truy vấn (1,1) | 
| 2 | 2 | truy vấn (1,2) | 
| 3 | 3 | truy vấn (1,3) | 

Nếu chuỗi là "abcabc", chỉ$p=3$thành công. Nếu là "aaaaaa", tất cả đều thành công. Nếu nó không định kỳ, không có thành công. 

Những ví dụ này cho thấy rằng mọi cấu trúc tuần hoàn có thể được nắm bắt bởi ít nhất một ước số được kiểm tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{N})$| Chúng tôi liệt kê các cặp số chia lên tới$\sqrt{N}$| 
| Không gian |$O(1)$| Chỉ lưu trữ ước số và đầu ra | 

Việc tính toán hoàn toàn là số học$N$, đủ nhỏ để$10^6$. Việc tạo truy vấn hiệu quả và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    output = []
    
    def fake_print(*args):
        output.append(" ".join(map(str, args)))
    
    global print
    old_print = print
    print = fake_print
    try:
        solve()
    finally:
        print = old_print
    
    return "\n".join(output)

# sample-like cases
assert run("2\n") == "1\n1 1"
assert run("4\n") == "2\n1 1\n1 2"

# custom cases
assert run("3\n") == "1\n1 1"
assert run("6\n") == "3\n1 1\n1 2\n1 3"
assert run("1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 truy vấn (1) | cạnh tổng hợp nhỏ nhất | 
| 3 | 1 truy vấn (1) | hành vi độ dài nguyên tố | 
| 6 | 3 truy vấn | nhiều ước số | 
| 1 | 0 | xử lý trường hợp thoái hóa | 

## Vỏ cạnh 

Đối với một số nguyên tố$N$, chẳng hạn như$N = 7$, thuật toán chỉ xuất ra số chia$1$. Điều này phản ánh chính xác rằng khoảng thời gian duy nhất có thể có là độ dài 1, do đó, phép kiểm tra có ý nghĩa duy nhất là liệu tất cả các ký tự có giống nhau hay không. Việc liệt kê số chia tự nhiên sẽ thu gọn vấn đề thành một giả thuyết duy nhất. 

Đối với một số tổng hợp cao như$N = 12$, thuật toán tạo ra tất cả các ước số$1,2,3,4,6$. Mặc dù một số trong số này là dư thừa về mặt cấu trúc tuần hoàn, nhưng chúng vẫn cần thiết để phân biệt trong trường hợp xấu nhất. Nếu loại bỏ bất kỳ giá trị nào trong số chúng, chúng ta có thể tạo một chuỗi có khoảng thời gian hợp lệ duy nhất khớp với ước số bị thiếu, khiến chiến lược không hoàn chỉnh. 

Vì$N = 1$, không có truy vấn nào được tạo ra vì không có khoảng thời gian hợp lệ yêu cầu ít nhất hai lần lặp lại. Điều này phù hợp với định nghĩa, vì một chuỗi ký tự đơn không thể được phân tách thành các khối lặp lại có độ dài nhỏ hơn. 

Mỗi trường hợp này đều tuân theo trực tiếp từ việc xây dựng dựa trên số chia, đảm bảo tính nhất quán mà không cần logic trường hợp đặc biệt.
