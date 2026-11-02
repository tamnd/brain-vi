---
title: "CF 1073A - Chuỗi con đa dạng"
description: "Chúng ta được cung cấp một chuỗi duy nhất bao gồm các chữ cái viết thường và chúng ta được yêu cầu tìm bất kỳ đoạn liền kề nào của chuỗi này sao cho không có ký tự đơn nào chiếm ưu thế trong đoạn đó bằng cách xuất hiện hơn một nửa chiều dài của nó. Nếu phân đoạn đó tồn tại, chúng tôi có thể xuất bất kỳ phân đoạn nào trong số đó."
date: "2026-06-15T14:11:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1073
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 53 (Rated for Div. 2)"
rating: 1000
weight: 1073
solve_time_s: 289
verified: true
draft: false
---

[CF 1073A - Chuỗi con đa dạng](https://codeforces.com/problemset/problem/1073/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 4 phút 49 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất bao gồm các chữ cái viết thường và chúng ta được yêu cầu tìm bất kỳ đoạn liền kề nào của chuỗi này sao cho không có ký tự đơn nào chiếm ưu thế trong đoạn đó bằng cách xuất hiện hơn một nửa chiều dài của nó. Nếu phân đoạn đó tồn tại, chúng tôi có thể xuất bất kỳ phân đoạn nào trong số đó. Nếu không tồn tại, chúng tôi phải báo cáo là không thể. 

Đối tượng chính không phải là chuỗi đầy đủ mà là chuỗi con của nó. Đối với mỗi chuỗi con, chúng tôi đang kiểm tra điều kiện tần số: nếu chuỗi con có độ dài$L$, thì tối đa mỗi ký tự phải xuất hiện$L/2$lần. Điều này tương đương với việc nói rằng chuỗi con không thể được tạo thành chủ yếu từ một ký tự lặp lại. 

Ràng buộc$n \le 1000$có nghĩa là một$O(n^3)$lực lượng vũ phu đối với các chuỗi con có tính năng đếm tần số sẽ là giới hạn nhưng vẫn có khả năng vượt qua trong Python được tối ưu hóa, trong khi$O(n^2)$giải pháp được an toàn thoải mái. Điều này gợi ý rõ ràng rằng chúng ta nên cố gắng suy luận về các chuỗi con có cấu trúc nhỏ hơn là các cấu trúc tổng thể phức tạp. 

Một điểm tinh tế là câu trả lời không bắt buộc phải dài hoặc tối đa. Điều này loại bỏ bất kỳ nhu cầu tối ưu hóa theo thời gian. Bất kỳ chuỗi con hợp lệ nào cũng đủ, thường cho phép đối số xây dựng thay vì tìm kiếm. 

Các trường hợp cạnh phát sinh khi dây đồng nhất. Ví dụ, nếu$s = "aaaa"$, mọi chuỗi con đều đồng nhất nên điều kiện đa số luôn bị vi phạm. Trong những trường hợp như vậy, đầu ra đúng là "KHÔNG". Một cách tiếp cận đơn giản chỉ kiểm tra tính đa dạng của chuỗi đầy đủ sẽ thất bại ở đây vì ngay cả khi chuỗi đầy đủ không thành công, các chuỗi con ngắn hơn vẫn có thể hoạt động, nhưng trong trường hợp cụ thể này thì không. 

Một trường hợp cạnh quan trọng khác là khi chuỗi có các ký tự hỗn hợp nhưng vẫn chiếm ưu thế cục bộ. Ví dụ: "aaab" có câu trả lời hợp lệ là "ab" hoặc "ba", nhưng riêng "aaa" thì không có chuỗi con hợp lệ nào cả. Điều này nhấn mạnh rằng chúng tôi không tìm kiếm sự đa dạng toàn cầu mà là sự cân bằng địa phương. 

## Phương pháp tiếp cận 

Chiến lược brute-force là liệt kê tất cả các chuỗi con và kiểm tra từng chuỗi con bằng cách đếm tần số ký tự. Đối với mỗi chuỗi con$s[l:r]$, chúng tôi tính toán 26 số đếm và xác minh xem có số lượng nào vượt quá một nửa chiều dài hay không. Điều này đúng vì nó khớp trực tiếp với định nghĩa. 

Vấn đề là sự phức tạp. có$O(n^2)$chuỗi con và mỗi chi phí kiểm tra$O(n)$nếu chúng ta tính lại số đếm từ đầu, dẫn đến$O(n^3)$. Ngay cả với tổng tiền tố giảm kiểm tra xuống$O(1)$mỗi nhân vật, chúng ta vẫn có$O(26n^2)$, đó là ranh giới nhưng có thể chấp nhận được ở$n=1000$. Tuy nhiên, điều này là không cần thiết vì cấu trúc của bài toán cho phép quan sát đơn giản hơn nhiều. 

Điểm mấu chốt là nếu tồn tại một chuỗi con hợp lệ thì chúng ta luôn có thể tìm thấy một chuỗi có độ dài tối đa là 2. Nếu chúng ta lấy bất kỳ hai ký tự riêng biệt liền kề nào thì chuỗi con có độ dài 2 đã thỏa mãn điều kiện, bởi vì mỗi ký tự xuất hiện chính xác một lần và$1 \le 2/2$chỉ sai nếu một ký tự xuất hiện nhiều lần. Do đó, bất kỳ cặp ký tự liền kề khác nhau đều hợp lệ. 

Điều này làm giảm toàn bộ vấn đề khi quét bất kỳ chỉ mục nào$i$như vậy$s[i] \ne s[i+1]$. Nếu một cặp như vậy tồn tại, chúng tôi sẽ xuất nó. Nếu không có cặp nào như vậy tồn tại thì chuỗi đó là đồng nhất và không tồn tại chuỗi con hợp lệ nào cả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$hoặc$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét chuỗi từ trái sang phải, kiểm tra từng cặp ký tự liền kề. 
2. Tại vị trí$i$, so sánh$s[i]$Và$s[i+1]$. 
3. Nếu khác nhau thì xuất ngay chuỗi con$s[i:i+2]$và chấm dứt. 
4. Nếu quá trình quét hoàn tất mà không tìm thấy bất kỳ cặp liền kề nào khác nhau, hãy kết luận rằng tất cả các ký tự đều giống hệt nhau. 
5. Trong trường hợp đó, xuất ra "NO". 

Lý do bước 3 là đủ là vì hai ký tự riêng biệt bất kỳ tạo thành một chuỗi con trong đó không ký tự nào có thể xuất hiện nhiều lần, điều này tự động thỏa mãn điều kiện phân tập. 

### Tại sao nó hoạt động 

Điều bất biến là nếu một chuỗi con hợp lệ tồn tại ở bất kỳ đâu trong chuỗi thì có thể có một chuỗi không khớp liền kề hoặc chuỗi đó hoàn toàn đồng nhất. Trong một chuỗi thống nhất, mọi chuỗi con đều có cùng một ký tự được lặp lại, do đó không có chuỗi con nào có thể thỏa mãn điều kiện. Trong một chuỗi không đồng nhất, ít nhất một cặp liền kề phải khác nhau và chính cặp đó tạo thành một giải pháp hợp lệ. Vì vậy, việc quét các cặp liền kề là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
s = input().strip()

for i in range(n - 1):
    if s[i] != s[i + 1]:
        print("YES")
        print(s[i:i+2])
        sys.exit(0)

print("NO")
```Giải pháp hoàn toàn dựa vào việc phát hiện sự chuyển đổi đầu tiên giữa các ký tự khác nhau. Việc thoát sớm đảm bảo chúng tôi không quét một cách không cần thiết sau khi tìm thấy câu trả lời hợp lệ. lát cắt`s[i:i+2]`an toàn vì Python xử lý các ranh giới một cách rõ ràng và chúng tôi chỉ lặp lại tối đa`n-2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`codeforces`| tôi | s[i] | s[i+1] | hành động | 
| --- | --- | --- | --- | 
| 0 | c | o | tìm thấy không khớp, xuất ra "co" | 

Thuật toán dừng ngay ở cặp đầu tiên. Điều này chứng tỏ rằng chuỗi con hợp lệ sớm nhất luôn được chấp nhận, bất kể vị trí. 

### Ví dụ 2 

đầu vào:`aaaaa`| tôi | s[i] | s[i+1] | hành động | 
| --- | --- | --- | --- | 
| 0 | một | một | tiếp tục | 
| 1 | một | một | tiếp tục | 
| 2 | một | một | tiếp tục | 
| 3 | một | một | tiếp tục | 

Không tìm thấy sự không khớp nào nên thuật toán đưa ra kết quả là "KHÔNG". Điều này xác nhận rằng các chuỗi thống nhất không có chuỗi con hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một lần vượt qua các cặp liền kề | 
| Không gian |$O(1)$| chỉ sử dụng các biến phụ không đổi | 

Quét tuyến tính là tối ưu vì mỗi ký tự phải được kiểm tra ít nhất một lần trong trường hợp xấu nhất để xác định xem có tồn tại bất kỳ ký tự nào không khớp liền kề hay không. Với$n \le 1000$, điều này nhanh một cách tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        n = int(input().strip())
        s = input().strip()
        for i in range(n - 1):
            if s[i] != s[i + 1]:
                print("YES")
                print(s[i:i+2])
                return out.getvalue().strip()
        print("NO")
    return out.getvalue().strip()

# provided sample
assert run("10\ncodeforces\n") == "YES\nco"

# custom cases
assert run("1\na\n") == "NO"
assert run("2\naa\n") == "NO"
assert run("2\nab\n") == "YES\nab"
assert run("5\nabcde\n") in {"YES\nab", "YES\nbc", "YES\ncd", "YES\nde"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"1 a"`| KHÔNG | trường hợp cạnh một ký tự | 
|`"aa"`| KHÔNG | chuỗi thống nhất tối thiểu | 
|`"ab"`| CÓ ab | trường hợp hợp lệ tối thiểu | 
|`"abcde"`| bất kỳ cặp liền kề nào | nhiều câu trả lời hợp lệ | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"z"`tự động không hợp lệ vì bất kỳ chuỗi con nào có độ dài 1 và ký tự duy nhất xuất hiện hơn một nửa thời gian. Thuật toán đưa ra kết quả "KHÔNG" chính xác vì không có cặp liền kề nào để kiểm tra. 

Một chuỗi hoàn toàn thống nhất như`"kkkkk"`cư xử tương tự. Quá trình quét không bao giờ tìm thấy sự không khớp và kết quả đầu ra chính xác là "KHÔNG". Mọi chuỗi con đều thừa hưởng sự mất cân bằng giống nhau. 

Một chuỗi hỗn hợp như`"ba"`chứng minh trường hợp tích cực ở độ dài tối thiểu. Phép so sánh đầu tiên đã mang lại kết quả không khớp và chuỗi con`"ba"`ngay lập tức hợp lệ vì cả hai ký tự xuất hiện một lần và không vượt quá một nửa của 2.
