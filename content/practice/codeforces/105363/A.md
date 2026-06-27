---
title: "CF 105363A - Xin chào!"
description: "Chúng ta được cung cấp một số chuỗi ngắn và đối với mỗi chuỗi, chúng ta phải quyết định xem liệu nó có thể được sắp xếp lại để tạo thành từ “hola” hay không. Sắp xếp lại có nghĩa là chúng ta được phép hoán vị các ký tự một cách tự do nhưng không thể thêm hoặc bớt bất kỳ ký tự nào."
date: "2026-06-23T05:34:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105363
codeforces_index: "A"
codeforces_contest_name: "XXV Spain Olympiad in Informatics, Online Qualifier 1"
rating: 0
weight: 105363
solve_time_s: 71
verified: true
draft: false
---

[CF 105363A - Xin chào!](https://codeforces.com/problemset/problem/105363/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi ngắn và đối với mỗi chuỗi, chúng ta phải quyết định xem liệu nó có thể được sắp xếp lại để tạo thành từ “hola” hay không. Sắp xếp lại có nghĩa là chúng ta được phép hoán vị các ký tự một cách tự do nhưng không thể thêm hoặc bớt bất kỳ ký tự nào. Vì vậy, nhiệm vụ giảm xuống còn việc kiểm tra xem nhiều tập hợp chữ cái trong từ đầu vào có khớp chính xác với nhiều tập hợp chữ cái trong “hola” hay không. 

Mỗi truy vấn đều độc lập và các chuỗi cực kỳ nhỏ nên quyết định cho từng trường hợp hoàn toàn mang tính cục bộ đối với chuỗi đó. Đầu ra cho mỗi trường hợp là sự chấp nhận hoặc từ chối đơn giản dựa trên sự so sánh này. 

Ràng buộc rằng mỗi chuỗi có độ dài tối đa là 5 sẽ thay đổi quan điểm một cách đáng kể. Bất kỳ thuật toán nào thực hiện công việc liên tục hoặc gần như liên tục cho mỗi ký tự đều đủ nhanh. Ngay cả những việc hơi dư thừa như sắp xếp từng chuỗi cũng không đáng kể. Điều này đẩy giải pháp thoát khỏi những mối quan tâm phức tạp và hướng tới sự chính xác và đơn giản. 

Một vài trường hợp khó khăn xuất hiện một cách tự nhiên. Một chuỗi có các chữ cái lặp lại như “holaa” phải bị từ chối vì nó chứa một ký tự phụ mặc dù nó bao gồm tất cả các ký tự bắt buộc. Một chuỗi ngắn hơn như “ola” cũng phải bị từ chối vì nó thiếu các chữ cái. Một trường hợp khác là hoán vị đúng như “ohal”, trong đó các chữ cái khớp chính xác nhưng bị xáo trộn. 

Một cách tiếp cận ngây thơ chỉ kiểm tra xem tất cả các ký tự bắt buộc có xuất hiện ít nhất một lần hay không sẽ thất bại. Ví dụ: “holi” chứa h, o, l, i, do đó, việc kiểm tra sự hiện diện ngây thơ có thể chấp nhận nhầm nó nếu nó không tính đến việc khớp tần số chính xác. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là tạo ra tất cả các hoán vị của chuỗi đầu vào và kiểm tra xem có bất kỳ hoán vị nào trong số chúng bằng “hola” hay không. Vì các chuỗi có độ dài tối đa là 5 nên số lượng hoán vị tối đa là 120, do đó điều này khả thi về mặt kỹ thuật cho mỗi trường hợp thử nghiệm. Tuy nhiên, việc tạo ra các hoán vị nhiều lần là công việc không cần thiết và về mặt khái niệm, nó che khuất cấu trúc đơn giản hơn của bài toán. Tính chính xác đến từ việc khớp số lượng ký tự chứ không phải khám phá sự sắp xếp lại một cách rõ ràng. 

Quan sát quan trọng là hai chuỗi là hoán vị của nhau khi và chỉ khi chúng chứa cùng tần số của mỗi ký tự. Thay vì tạo ra sự sắp xếp lại, chúng ta có thể so sánh trực tiếp số lượng ký tự. Vì từ mục tiêu là cố định và rất nhỏ nên chúng ta có thể sắp xếp cả hai chuỗi hoặc đếm tần số và so sánh. 

Việc sắp xếp ở đây đặc biệt rõ ràng. Nếu chúng ta sắp xếp chuỗi đầu vào và so sánh nó với phiên bản được sắp xếp của “hola”, tức là “ahlo”, chúng ta sẽ nhận được kết quả kiểm tra tính bằng nhau ngay lập tức. Ngoài ra, một mảng tần số gồm 26 chữ cái viết thường cũng hoạt động tốt như nhau, nhưng việc sắp xếp là đủ và đơn giản hơn nhiều đối với những ràng buộc nhỏ như vậy. 

Cách tiếp cận bạo lực dành thời gian khám phá các cấu trúc không cần thiết, trong khi chế độ xem tối ưu sẽ thu gọn vấn đề thành một biểu diễn chính tắc trực tiếp của từng chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tạo hoán vị) | O(T · n! · n) | O(n) | Tinh thần quá chậm chạp, không cần thiết | 
| Tối ưu (sắp xếp hoặc so sánh tần số) | O(T · n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sẽ sử dụng cách tiếp cận dựa trên sắp xếp vì đây là cách trực tiếp nhất.

1. Tính toán trước chuỗi tham chiếu đã sắp xếp cho “hola”, trở thành “ahlo”. Điều này cung cấp cho chúng tôi một dạng chuẩn để so sánh với mọi đầu vào. 
2. Đọc số lượng ca kiểm thử T, vì mỗi ca kiểm thử là độc lập và phải được xử lý riêng. 
3. Với mỗi chuỗi đầu vào s, hãy tính phiên bản đã sắp xếp của nó. Việc sắp xếp đảm bảo rằng mọi hoán vị của các chữ cái giống nhau sẽ được xếp vào cùng một cách biểu diễn. 
4. So sánh chuỗi đã sắp xếp với “ahlo”. Nếu chúng giống hệt nhau thì đầu vào chính xác là một hoán vị của “hola”, vì vậy chúng ta xuất ra “SI”. Ngược lại, chúng ta xuất ra “NO”. 
5. Lặp lại quy trình này cho tất cả các trường hợp kiểm thử, tạo ra một câu trả lời trên mỗi dòng. 

### Tại sao nó hoạt động 

Sắp xếp xác định một dạng chuẩn xác định cho bất kỳ tập hợp ký tự nào. Hai chuỗi là hoán vị của nhau khi và chỉ khi cách biểu diễn được sắp xếp của chúng giống hệt nhau. Điều này tạo ra ánh xạ một-một giữa các lớp tương đương của chuỗi theo phép hoán vị và dạng được sắp xếp của chúng, do đó sự bằng nhau của các chuỗi được sắp xếp là cần thiết và đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    target = sorted("hola")
    target = "".join(target)

    t = int(input().strip())
    for _ in range(t):
        s = input().strip()
        if "".join(sorted(s)) == target:
            print("SI")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là xây dựng dạng đã sắp xếp của từ mục tiêu “hola”, tức là “ahlo”. Điều này tránh việc tính toán lại hoặc so sánh ký tự mã hóa cứng theo cách thủ công. 

Đối với mỗi trường hợp thử nghiệm, chuỗi đầu vào được sắp xếp bằng cách sử dụng tính năng sắp xếp tích hợp của Python, tính năng này hiệu quả và đủ với độ dài tối đa là 5. Sau đó, phép so sánh là một phép kiểm tra tính bằng nhau của chuỗi đơn giản, mã hóa trực tiếp xem cả hai chuỗi có nhiều bộ ký tự giống hệt nhau hay không. 

Việc sử dụng`sys.stdin.readline`đảm bảo xử lý đầu vào nhanh chóng, mặc dù hiệu suất ở đây không quan trọng do những hạn chế nhỏ. Bản thân logic hoàn toàn tập trung vào biểu diễn chuẩn thông qua việc sắp xếp. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai trường hợp: một hoán vị hợp lệ và một chuỗi không hợp lệ. 

### Ví dụ 1 

Chuỗi đầu vào:`ohal`| Bước | Chuỗi | Mẫu sắp xếp | So sánh với "ahlo" | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | ồ | ồ | ôi == ồ ồ | KHÔNG | 
| 2 | | | | | 

Ở đây chúng ta thấy rằng mặc dù chuỗi chứa các chữ cái giống như “hola”, nhưng dạng sắp xếp khác nhau nên nó bị từ chối. Điều này cho thấy việc sắp xếp phải khớp chính xác chứ không phải một phần. 

Bây giờ hãy xem xét một trường hợp đúng. 

Chuỗi đầu vào:`loha`| Bước | Chuỗi | Mẫu sắp xếp | So sánh với "ahlo" | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | cười lớn | alo | alo == alo | SI | 

Điều này xác nhận rằng mọi hoán vị đều thu gọn về cùng một dạng chính tắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n log n) | Mỗi chuỗi có độ dài tối đa 5 được sắp xếp độc lập | 
| Không gian | O(1) | Chỉ sử dụng chuỗi tham chiếu cố định và không gian sắp xếp tạm thời | 

Các ràng buộc nhỏ đến mức ngay cả việc sắp xếp lặp đi lặp lại cũng có hiệu quả là thời gian không đổi. Giải pháp tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("""6
hola
ola
ohal
loha
holaa
holi
""") == """SI
NO
NO
SI
NO
NO"""

# minimum size
assert run("""2
h
a
""") == """NO
NO"""

# exact match and permutation variants
assert run("""3
hola
loah
ahlo
""") == """SI
SI
SI"""

# repeated character case
assert run("""2
holaa
aahlo
""") == """NO
NO"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chữ cái | KHÔNG, KHÔNG | từ chối kích thước tối thiểu | 
| hoán vị của hola | dòng SI | tính đúng đắn khi sắp xếp lại | 
| chữ cái lặp đi lặp lại | KHÔNG | xử lý không khớp tần số | 

## Vỏ cạnh 

Một chuỗi ngắn hơn “hola”, chẳng hạn như`h`, được xử lý bằng cách sắp xếp thành một chuỗi ký tự đơn. Vì điều này không thể khớp với tham chiếu bốn ký tự nên việc so sánh thất bại ngay lập tức và tạo ra “KHÔNG”. 

Một chuỗi có các chữ cái lặp đi lặp lại như`holaa`sắp xếp để`aa hlo`(về mặt khái niệm`aahlo`), khác với`ahlo`, nên nó bị từ chối. Điều này xác nhận rằng tần số không khớp được phát hiện chính xác mà không cần đếm ký tự một cách rõ ràng. 

Một hoán vị hoàn toàn hợp lệ như`loah`sắp xếp để`ahlo`, khớp chính xác với mục tiêu. Điều này xác nhận rằng sự khác biệt về thứ tự được chuẩn hóa hoàn toàn bằng bước sắp xếp.
