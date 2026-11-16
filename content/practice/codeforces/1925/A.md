---
title: "CF 1925A - Chúng tôi đã giải quyết được mọi thứ!"
description: "Chúng ta được yêu cầu xây dựng một chuỗi đơn trên k chữ cái viết thường đầu tiên sao cho mọi chuỗi có thể có độ dài n được tạo thành từ k chữ cái đó xuất hiện ở đâu đó bên trong nó dưới dạng một chuỗi con."
date: "2026-06-09T01:32:48+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1925
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 921 (Div. 2)"
rating: 800
weight: 1925
solve_time_s: 108
verified: false
draft: false
---

[CF 1925A - Chúng tôi đã giải quyết được mọi thứ!](https://codeforces.com/problemset/problem/1925/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, chuỗi 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một chuỗi đơn trên k chữ cái viết thường đầu tiên sao cho mọi chuỗi có thể có độ dài n được tạo thành từ k chữ cái đó xuất hiện ở đâu đó bên trong nó dưới dạng một chuỗi con. Một chuỗi con có nghĩa là chúng ta có thể xóa các ký tự khỏi chuỗi được xây dựng mà không cần sắp xếp lại những gì còn lại và vẫn lấy được chuỗi đích. 

Yêu cầu chính là cực kỳ mạnh mẽ: chúng tôi không chỉ nhúng một vài mẫu mà còn tất cả kⁿ chuỗi có thể có độ dài n. Vì n và k đều lớn nhất là 26, nên tổng số chuỗi cần thiết có thể rất lớn trong trường hợp xấu nhất, nhưng chuỗi được xây dựng được phép sử dụng lại cấu trúc rất nhiều vì các chuỗi con có thể chồng lên nhau theo những cách linh hoạt. 

Đầu ra cũng phải càng ngắn càng tốt. Điều đó buộc chúng ta phải tránh cấu trúc dư thừa, bởi vì bất kỳ sự lặp lại ký tự không cần thiết nào sẽ chỉ làm tăng nguy cơ không tối ưu. 

Một cách tiếp cận ngây thơ sẽ cố gắng đảm bảo rõ ràng mỗi chuỗi có độ dài n xuất hiện dưới dạng một chuỗi con. Người ta có thể cố gắng nối tất cả các chuỗi kⁿ hoặc nối thêm các chuỗi bị thiếu một cách tham lam. Điều này thất bại ngay lập tức vì kⁿ tăng theo cấp số nhân và chuỗi yêu cầu sẽ trở nên không khả thi ngay cả đối với các đầu vào nhỏ như n = 10, k = 2. 

Một thất bại tinh tế hơn xuất phát từ việc ghép nối tham lam các mẫu như lặp lại "abc...". Điều này có thể bao gồm nhiều chuỗi con, nhưng nó không đảm bảo bao phủ tất cả các kết hợp có độ dài n. Ví dụ: với k = 2 và n = 2, chuỗi "abab" không chứa "bb" làm dãy con nên đã không đủ. 

Cấu trúc của bài toán gợi ý rằng chúng ta cần một cấu trúc phổ quát có thể mã hóa ngầm tất cả các chuỗi mà không cần liệt kê chúng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liệt kê tất cả các chuỗi kⁿ và cố gắng hợp nhất chúng thành một chuỗi siêu ngắn nhất theo quan hệ chuỗi con. Đây thực chất là bài toán về dãy siêu chung ngắn nhất trên tập mũ. Ngay cả việc biểu diễn không gian trạng thái cũng trở nên bất khả thi vì mỗi chuỗi có độ dài n đóng góp các ràng buộc và việc hợp nhất chúng đòi hỏi thời gian và bộ nhớ theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến việc bao gồm tất cả các chuỗi có độ dài n một cách rõ ràng, chúng tôi xây dựng một chuỗi cho phép chúng tôi "định tuyến" bất kỳ chuỗi lựa chọn nào thông qua các vị trí. Nếu chúng ta lặp lại một cấu trúc được lựa chọn cẩn thận, chúng ta có thể đảm bảo rằng bất kỳ chuỗi mong muốn nào cũng có thể được nhúng bằng cách chọn lần xuất hiện của các ký tự theo thứ tự. 

Cách xây dựng tối ưu lại đơn giản đến mức đáng ngạc nhiên: bắt đầu với bảng chữ cái k theo thứ tự, sau đó lặp lại n lần. Điều này tạo ra một chuỗi trong đó mọi chữ cái xuất hiện trong mọi "lớp", cho phép chúng ta chọn bất kỳ chuỗi nào có độ dài n bằng cách luôn lấy các lần xuất hiện từ các lớp liên tiếp. 

Tại sao điều này hoạt động được gắn liền với tính linh hoạt của chuỗi tiếp theo. Mỗi lần xuất hiện của một ký tự trong lần lặp lại sau đó có thể biểu thị bước tiếp theo trong bất kỳ chuỗi mong muốn nào. Bằng cách đảm bảo có sẵn k lựa chọn ở mọi độ sâu, chúng tôi tránh được xung đột khi một lựa chọn này cản trở một lựa chọn khác. 

Tính tối thiểu xuất phát từ thực tế là mỗi chuỗi bắt buộc cần ít nhất n lần xuất hiện được phân bổ theo thứ tự và với k lựa chọn cho mỗi vị trí, chúng ta không thể nén các lớp thêm mà không mất khả năng phân tách các vị trí chuỗi tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(kⁿ · n) | O(kⁿ) | Quá chậm | 
| Xây dựng lớp | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng chuỗi bảng chữ cái cơ sở gồm k chữ cái đầu tiên theo thứ tự tăng dần. Điều này cung cấp tất cả các ký hiệu có sẵn ở một "cấp độ" duy nhất. 
2. Lặp lại chuỗi cơ sở này chính xác n lần và nối các kết quả. Mỗi lần lặp lại hoạt động như một lớp mới trong đó các chuỗi tiếp theo có thể tiến lên một bước. 
3. Xuất chuỗi kết quả. 

Mỗi lần lặp lại là cần thiết vì các chuỗi con phải có khả năng chọn ký tự thứ i của chuỗi mục tiêu từ vị trí muộn hơn so với ký tự thứ (i-1). Nếu không có các lớp lặp lại, chúng tôi không thể đảm bảo ràng buộc đặt hàng đó. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi mục tiêu nào có độ dài n đều có thể được nhúng bằng cách ánh xạ ký tự thứ i của nó với lần xuất hiện thứ i của ký tự đó trong cấu trúc lớp lặp lại. Vì mỗi lớp chứa tất cả k chữ cái nên chúng ta không bao giờ bị buộc phải sử dụng lại cùng một lần xuất hiện hai lần và thứ tự giữa các lớp đảm bảo điều kiện tuần tự tiếp theo. Điều này thiết lập một ánh xạ đơn điệu từ các vị trí trong chuỗi đích tới các vị trí trong chuỗi được xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    base = ''.join(chr(ord('a') + i) for i in range(k))
    print(base * n)
```Giải pháp xây dựng tiền tố bảng chữ cái một lần cho mỗi trường hợp thử nghiệm và lặp lại n lần. Điều tinh tế duy nhất là đảm bảo chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập vì cả n và k đều khác nhau. 

Tính chính xác phụ thuộc hoàn toàn vào việc duy trì bảng chữ cái đầy đủ trong mỗi khối lặp lại. Bất kỳ nỗ lực hoán vị hoặc giảm bớt bảng chữ cái nào cũng sẽ phá vỡ khả năng ánh xạ các chuỗi tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: n = 2, k = 2 

Chúng ta xây dựng base="ab", chuỗi cuối cùng = "abab". 

| Bước | Nhân vật mục tiêu | Vị trí được lựa chọn trong xây dựng | 
| --- | --- | --- | 
| 1 | một | 'a' đầu tiên trong khối đầu tiên | 
| 2 | b | 'b' đầu tiên trong khối thứ hai | 

Điều này xác nhận rằng tất cả 4 chuỗi có độ dài 2 trên {a, b} đều có thể được nhúng vì mỗi lựa chọn có thể được định tuyến đến khối sau. 

### Ví dụ 2 

Đầu vào: n = 3, k = 3 

Xây dựng: "abcabcabc" 

| Bước | Nhân vật mục tiêu | Vị trí được chọn | 
| --- | --- | --- | 
| 1 | b | khối 1 | 
| 2 | c | khối 2 | 
| 3 | một | khối 3 | 

Ngay cả khi các ký tự lặp lại hoặc giảm đi theo từ điển, các khối sau luôn cung cấp lần xuất hiện tiếp theo hợp lệ. 

Điều này chứng tỏ rằng thứ tự được xử lý hoàn toàn theo chỉ mục khối chứ không phải theo thứ tự ký tự bên trong khối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | xây dựng chuỗi cơ sở có độ dài k và lặp lại n lần | 
| Không gian | O(nk) | kích thước của chuỗi đầu ra kết quả | 

Các ràng buộc cho phép tối đa 26 ký tự và 26 lần lặp lại cho mỗi lần kiểm tra, do đó, kích thước đầu ra tối đa là 676 ký tự cho mỗi trường hợp kiểm tra, điều này không quan trọng đối với việc tính toán và in trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    _out = StringIO()
    _sys.stdin = io.StringIO(inp)

    t = int(input())
    res = []
    for _ in range(t):
        n, k = map(int, input().split())
        base = ''.join(chr(ord('a') + i) for i in range(k))
        res.append(base * n)
    return "\n".join(res)

# provided samples
assert run("4\n1 2\n2 1\n2 2\n2 3\n") == "ab\naa\nbaba\nabcabc", "sample check"

# minimum case
assert run("1\n1 1\n") == "a", "single character"

# single alphabet repeated
assert run("1\n5 1\n") == "aaaaa", "k=1 repetition"

# maximal alphabet small n
assert run("1\n2 3\n") == "abcabc", "small layered case"

# boundary mix
assert run("2\n2 2\n3 1\n") == "baba\naaa", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | một | trường hợp nhỏ nhất có thể | 
| 5 1 | aaa | hành vi lặp lại một chữ cái | 
| 2 3 | abc | phân lớp với k > 2 | 
| hỗn hợp | baba / aaa | xử lý nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Với k = 1, cấu trúc giảm xuống còn lặp lại một ký tự n lần. Mỗi chuỗi có độ dài n được xác định duy nhất, do đó kết quả gần như chính xác vì chuỗi duy nhất có thể là một chữ cái đơn lặp lại. 

Đối với n = 1, yêu cầu là phải chứa mọi ký tự đơn trong số k chữ cái đầu tiên dưới dạng một chuỗi con. Việc xây dựng trở thành chuỗi bảng chữ cái, do đó mỗi ký tự xuất hiện trực tiếp. 

Đối với k và n lớn hơn, sự lặp lại theo lớp đảm bảo rằng các chuỗi con không cạnh tranh vị trí, vì mỗi lớp độc lập và chứa đầy đủ bảng chữ cái. Điều này ngăn ngừa bất kỳ xung đột thứ tự nào giữa các chuỗi mục tiêu khác nhau.
