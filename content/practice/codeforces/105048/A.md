---
title: "CF 105048A – Tồn tại hay không tồn tại?"
description: "Chúng ta được cung cấp một dòng văn bản có thể chứa các chữ cái tiếng Anh viết hoa và viết thường trộn lẫn với dấu cách. Nhiệm vụ là xác định xem chuỗi ký tự liền kề “be” có xuất hiện ở bất kỳ đâu trong văn bản này hay không khi chúng ta bỏ qua cách viết hoa."
date: "2026-06-28T05:41:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105048
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 03-22-24 Div. 2 (Beginner)"
rating: 0
weight: 105048
solve_time_s: 72
verified: true
draft: false
---

[CF 105048A - Tồn tại hay không tồn tại?](https://codeforces.com/problemset/problem/105048/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng văn bản có thể chứa các chữ cái tiếng Anh viết hoa và viết thường trộn lẫn với dấu cách. Nhiệm vụ là xác định xem chuỗi ký tự liền kề “be” có xuất hiện ở bất kỳ đâu trong văn bản này hay không khi chúng ta bỏ qua cách viết hoa. 

Sự chuyển đổi quan trọng là trường hợp đó không quan trọng, vì vậy mọi ký tự phải được xử lý như thể nó là chữ thường trước khi chúng ta tìm kiếm. Dấu cách chỉ là các ký tự bình thường trong dữ liệu đầu vào nhưng chúng không thể là một phần của chuỗi con đích vì chúng tôi đang tìm kiếm cụ thể hai chữ cái liên tiếp tạo thành “be”. 

Kích thước đầu vào tối đa là 1000 ký tự, cực kỳ nhỏ theo tiêu chuẩn lập trình cạnh tranh. Điều này ngay lập tức loại trừ mọi lo ngại về hiệu quả ngoài chức năng quét tuyến tính. Ngay cả một cách tiếp cận ngây thơ để kiểm tra mọi chuỗi con có thể cũng đã đủ trong giới hạn. 

Sự tinh tế chính là xử lý việc chuẩn hóa chữ hoa chữ thường và đảm bảo chúng ta chỉ xem xét các chữ cái liên tiếp. Một lỗi phổ biến là tìm kiếm “be” trong chuỗi gốc mà không chuyển đổi chữ hoa chữ thường, điều này không thành công với các đầu vào như “BE” hoặc “Be”. Một sai lầm khác là bỏ qua các khoảng trắng một cách không chính xác theo cách phá vỡ logic kề cận. Ví dụ: “b e” không được khớp vì các ký tự không liên tiếp trong chuỗi gốc. 

Các trường hợp cạnh bao gồm các chuỗi không có chữ cái nào tạo thành mẫu mặc dù các chữ cái xuất hiện gần nhau, chẳng hạn như “b e” và các chuỗi trong đó kết quả khớp kéo dài các trường hợp khác nhau như “bE” hoặc “BE”. Chuỗi ký tự đơn như “b” hoặc “e” cũng phải trả về chính xác “no”. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là chuẩn hóa chuỗi thành một trường hợp thống nhất, sau đó quét nó từ trái sang phải để kiểm tra từng cặp ký tự liền kề. Nếu bất kỳ cặp nào bằng “be”, chúng ta ngay lập tức kết luận câu trả lời là khẳng định. 

Một biến thể brute-force sẽ kiểm tra rõ ràng mọi chuỗi con có độ dài 2 và so sánh nó với “be” sau khi chuyển đổi sang chữ thường. Đối với một chuỗi có độ dài n, thao tác này kiểm tra n - 1 chuỗi con, mỗi chuỗi con có thời gian không đổi, cho ra độ phức tạp tuyến tính. Ngay cả những phương pháp đơn giản hơn như tạo ra tất cả các chuỗi con cũng sẽ không cần thiết nhưng vẫn vượt qua được ràng buộc. 

Quan sát chính là vấn đề giảm xuống còn tìm kiếm mẫu đơn giản có độ dài cố định 2. Không cần thuật toán chuỗi nâng cao như KMP vì mẫu có kích thước không đổi. Điều này thu gọn tác vụ thành một lần chuyển qua chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các chuỗi con) | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (quét một lần) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào dưới dạng một dòng. Chuỗi này có thể bao gồm các khoảng trắng mà chúng tôi sẽ không xóa, chỉ bỏ qua cho mục đích khớp một cách gián tiếp thông qua so sánh ký tự. 
2. Chuyển toàn bộ chuỗi thành chữ thường. Điều này đảm bảo rằng các so sánh là thống nhất và loại bỏ việc phân nhánh dựa trên trường hợp sau này. 
3. Lặp lại chuỗi từ chỉ số 0 đến n - 2. Chúng ta dừng ở n - 2 vì chúng ta luôn kiểm tra một cặp ký tự liên tiếp. 
4. Với mỗi vị trí i, kiểm tra xem s[i] có phải là 'b' và s[i + 1] có phải là 'e' hay không. Điều này trực tiếp kiểm tra xem chuỗi con bắt đầu từ i có khớp với mẫu đích hay không. 
5. Nếu tìm thấy bất kỳ cặp nào như vậy, ngay lập tức xuất ra “có” và kết thúc chương trình. 
6. Nếu vòng lặp kết thúc mà không tìm được kết quả phù hợp, xuất ra “no”. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính bất biến là mỗi lần xuất hiện của chuỗi con “be” trong chuỗi gốc phải xuất hiện dưới dạng hai ký tự liên tiếp trong một cặp chỉ số nào đó (i, i + 1). Viết thường duy trì mối quan hệ bình đẳng giữa các chữ cái, do đó, bất kỳ kết quả khớp hợp lệ nào trong chuỗi gốc đều tương ứng chính xác với kết quả khớp trong chuỗi được chuyển đổi. Vì chúng ta kiểm tra tất cả các cặp liền kề nên không có sự xuất hiện hợp lệ nào có thể bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().rstrip("\n")
s = s.lower()

found = False
for i in range(len(s) - 1):
    if s[i] == 'b' and s[i + 1] == 'e':
        found = True
        break

print("YES" if found else "NO")
```Giải pháp đọc đầu vào một cách hiệu quả và ngay lập tức chuẩn hóa nó bằng cách sử dụng`.lower()`. Vòng lặp được giới hạn cẩn thận để tránh lập chỉ mục ngoài phạm vi khi truy cập`i + 1`. Việc thoát sớm đảm bảo chúng tôi dừng lại ngay khi tìm thấy mẫu, mặc dù ngay cả khi không thoát sớm thì độ phức tạp vẫn là tuyến tính. 

Một lỗi triển khai phổ biến là quên rằng các khoảng trắng vẫn còn trong chuỗi. Mã này xử lý điều đó một cách chính xác vì dấu cách sẽ không bao giờ khớp với 'b' hoặc 'e', ​​vì vậy chúng đương nhiên không so sánh được mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
To Be or Not to BE that is the QUESTION
```Sau khi viết thường:```
to be or not to be that is the question
```| tôi | s[i] | s[i+1] | Trận đấu | 
| --- | --- | --- | --- | 
| 0 | t | o | không | 
| 1 | o | | không | 
| 2 | | b | không | 
| 3 | b | e | vâng | 

Tại chỉ số 3, ta gặp cặp “be” nên thuật toán dừng ngay lập tức và cho ra CÓ. Điều này xác nhận rằng việc chuẩn hóa trường hợp cho phép phát hiện cả “Be” và “BE” một cách thống nhất. 

### Ví dụ 2 

đầu vào:```
To b or not to b that is the question
```Sau khi viết thường:```
to b or not to b that is the question
```| tôi | s[i] | s[i+1] | Trận đấu | 
| --- | --- | --- | --- | 
| 0 | t | o | không | 
| 1 | o | | không | 
| 2 | | b | không | 
| 3 | b | | không | 
| 4 | | o | không | 
| ... | ... | ... | ... | 

Không có cặp liền kề nào tạo thành “be”, do đó vòng lặp hoàn thành mà không cần đặt cờ. Kết quả đầu ra là KHÔNG, xác nhận rằng các chữ cái cách biệt không kết hợp sai giữa các khoảng trắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng cặp ký tự liền kề một lần | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài chuỗi đầu vào | 

Độ dài đầu vào tối đa là 1000, do đó, ngay cả quét tuyến tính cũng có hiệu quả theo thời gian không đổi trong thực tế. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = sys.stdin.readline().rstrip("\n").lower()
    for i in range(len(s) - 1):
        if s[i] == 'b' and s[i + 1] == 'e':
            return "YES"
    return "NO"

# provided samples
assert run("To Be or Not to BE that is the QUESTION\n") == "YES"
assert run("To b or not to b that is the question\n") == "NO"

# custom cases
assert run("be\n") == "YES"
assert run("Be\n") == "YES"
assert run("b e\n") == "NO"
assert run("bbbbbeeeee\n") == "YES"
assert run("xyz\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "được" | CÓ | trận đấu tối thiểu | 
| "b e" | KHÔNG | không gian phá vỡ liền kề | 
| "BBBB" | KHÔNG | không có kết quả dương tính giả trong trường hợp chuẩn hóa | 
| "xyz" | KHÔNG | chữ cái không khớp | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chuỗi con xuất hiện với cách viết hoa hỗn hợp. Đối với đầu vào như “bE”, trước tiên thuật toán chuyển đổi nó thành “be”, sau đó kiểm tra liền kề sẽ phát hiện nó ở chỉ số 0 và trả về CÓ ngay lập tức. Điều này xác nhận rằng việc chuẩn hóa sẽ thống nhất chính xác tất cả các biến thể của trường hợp. 

Một trường hợp cạnh khác là sự hiện diện của khoảng trắng giữa các chữ cái mà nếu không sẽ tạo thành mẫu. Ví dụ: “b e” trở thành “b e” sau khi chuẩn hóa. Vòng lặp chỉ kiểm tra các chỉ số liền kề, do đó, nó không bao giờ kết hợp các ký tự trong khoảng trắng và trả về NO một cách chính xác. 

Trường hợp cạnh cuối cùng là các chuỗi rất ngắn chẳng hạn như “b” hoặc “e”. Sau khi viết thường, phạm vi vòng lặp sẽ trống vì độ dài trừ một bằng 0 hoặc âm, do đó không có phép lặp nào xảy ra và hàm trả về NO một cách chính xác.
