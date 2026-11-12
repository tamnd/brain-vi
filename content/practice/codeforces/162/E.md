---
title: "CF 162E - HQ9+"
description: "Vấn đề hỏi liệu một chương trình được viết bằng ngôn ngữ đùa HQ9+ có tạo ra kết quả nào không. Chương trình là một chuỗi chứa từ 1 đến 100 ký tự ASCII có thể in được. Chỉ có bốn hướng dẫn quan trọng: \"H\", \"Q\", \"9\" và \"+\"."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 162
codeforces_index: "E"
codeforces_contest_name: "VK Cup 2012 Wild-card Round 1"
rating: 1800
weight: 162
solve_time_s: 69
verified: true
draft: false
---

[CF 162E - HQ9+](https://codeforces.com/problemset/problem/162/E) 

**Đánh giá:** 1800 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề hỏi liệu một chương trình được viết bằng ngôn ngữ đùa HQ9+ có tạo ra kết quả nào không. Chương trình là một chuỗi chứa từ 1 đến 100 ký tự ASCII có thể in được. Chỉ có bốn hướng dẫn quan trọng: "H", "Q", "9" và "+". Lệnh "+" thao tác với bộ tích lũy bên trong nhưng không tạo ra bất kỳ đầu ra nào, vì vậy nó có thể bị bỏ qua vì mục đích của vấn đề này. Tất cả các ký tự khác, bao gồm cả chữ thường và dấu chấm câu, cũng bị bỏ qua. Nhiệm vụ là phát hiện xem chương trình có chứa ít nhất một trong các chữ "H", "Q" hay "9" hay không, trong trường hợp đó việc chạy chương trình sẽ tạo ra đầu ra. 

Các ràng buộc nhỏ, với độ dài đầu vào tối đa là 100 ký tự. Điều này có nghĩa là chúng ta có thể thực hiện quét tuyến tính đơn giản thông qua chuỗi và không cần tối ưu hóa ngoài O(n). Các trường hợp cạnh không rõ ràng bao gồm các chuỗi chỉ chứa các ký tự bị bỏ qua hoặc các chuỗi có kiểu chữ hỗn hợp. Ví dụ, một chương trình`"hello"`chỉ chứa các chữ cái viết thường, bị bỏ qua, vì vậy kết quả đầu ra đúng là "KHÔNG". Ngược lại,`"H"`tạo ra đầu ra, vì vậy câu trả lời là "CÓ". Một cách tiếp cận bất cẩn có thể kiểm tra xem có bất kỳ sự xuất hiện nào của 'h' thay vì 'H' hay không, đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp qua từng ký tự của chuỗi và kiểm tra xem nó có khớp với "H", "Q" hay "9" hay không. Nếu tìm thấy bất kỳ kết quả nào phù hợp, xuất ngay "CÓ"; nếu không thì xuất "NO" ở cuối. Điều này có tác dụng vì sự cố chỉ yêu cầu phát hiện một lệnh duy nhất tạo ra đầu ra và mỗi ký tự có thể được kiểm tra độc lập. Với độ dài tối đa là 100, phương pháp này yêu cầu tối đa 100 phép so sánh, điều này không đáng kể về mặt chi phí tính toán. 

Cách tiếp cận tối ưu về mặt hiệu quả cũng giống như cách tiếp cận vũ phu trong trường hợp này. Cái nhìn sâu sắc quan trọng là nhận ra rằng chúng ta không cần phải mô phỏng chương trình hoặc xử lý bộ tích lũy. Vấn đề giảm xuống còn một bài kiểm tra tư cách thành viên trong một bộ ba ký tự. Việc sử dụng một bộ để so sánh sẽ làm cho mã ngắn gọn và rõ ràng, đồng thời việc chuyển một lần qua chuỗi sẽ đảm bảo tính chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào đại diện cho chương trình HQ9+. 
2. Khởi tạo tập hợp chứa các ký tự`'H'`,`'Q'`, Và`'9'`. Bộ này đại diện cho các hướng dẫn tạo ra đầu ra. 
3. Lặp lại từng ký tự trong chuỗi đầu vào. Đối với mỗi ký tự, hãy kiểm tra xem nó có phải là thành viên của tập lệnh tạo đầu ra hay không. 
4. Nếu tìm thấy bất kỳ ký tự nào trong tập hợp, hãy in ngay "CÓ" và kết thúc thuật toán. Điều này đảm bảo chúng ta không lãng phí thời gian quét phần còn lại của chuỗi một cách không cần thiết. 
5. Nếu vòng lặp hoàn thành mà không tìm thấy bất kỳ ký tự nào trong tập hợp, hãy in "NO". Tại thời điểm này, chúng tôi biết chương trình không tạo ra đầu ra. 

Tại sao nó hoạt động: Điều bất biến là chương trình tạo ra đầu ra khi và chỉ khi nó chứa ít nhất một trong các "H", "Q" hoặc "9". Bằng cách quét chuỗi một lần và kiểm tra tư cách thành viên trong một tập hợp, chúng tôi đảm bảo rằng mọi lệnh tạo đầu ra sẽ được phát hiện và chúng tôi không bao giờ tạo ra kết quả dương tính giả đối với các ký tự bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

program = input().strip()
output_instructions = {'H', 'Q', '9'}

for char in program:
    if char in output_instructions:
        print("YES")
        break
else:
    print("NO")
```Giải pháp đọc chương trình từ đầu vào tiêu chuẩn và loại bỏ các ký tự dòng mới ở cuối bằng`strip()`. bộ`output_instructions`cho phép kiểm tra tư cách thành viên liên tục cho mỗi ký tự. các`for-else`cấu trúc đảm bảo rằng "NO" chỉ được in nếu vòng lặp hoàn thành mà không tìm thấy kết quả khớp. Sử dụng một bộ thay vì nhiều`or`điều kiện làm cho mã có thể mở rộng và dễ đọc. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`"Hello!"`| Bước | char | char trong bộ? | Hành động | 
| --- | --- | --- | --- | 
| 1 | 'H' | Có | In CÓ, dừng | 
| 2 | 'e' | - | - | 

Thuật toán phát hiện 'H' ngay lập tức và xuất ra "CÓ". Các ký tự tiếp theo không được kiểm tra, chứng tỏ sự kết thúc sớm. 

Mẫu 2: 

đầu vào:`"hello"`| Bước | char | char trong bộ? | Hành động | 
| --- | --- | --- | --- | 
| 1 | 'h' | Không | Tiếp tục | 
| 2 | 'e' | Không | Tiếp tục | 
| 3 | 'l' | Không | Tiếp tục | 
| 4 | 'l' | Không | Tiếp tục | 
| 5 | 'o' | Không | Tiếp tục | 

Không có ký tự nào khớp với 'H', 'Q' hoặc '9', do đó vòng lặp hoàn thành và thuật toán in ra "NO". Điều này xác nhận rằng các chữ cái viết thường bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét từng ký tự một lần và thực hiện kiểm tra tư cách thành viên được thiết lập theo thời gian không đổi. | 
| Không gian | O(1) | Tập lệnh có kích thước cố định gồm ba phần tử, không phụ thuộc vào kích thước đầu vào. | 

Cho rằng n ≤ 100, giải pháp thực hiện tối đa 100 thao tác và sử dụng bộ nhớ tối thiểu, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as io_module
    out = io_module.StringIO()
    with redirect_stdout(out):
        program = input().strip()
        output_instructions = {'H', 'Q', '9'}
        for char in program:
            if char in output_instructions:
                print("YES")
                break
        else:
            print("NO")
    return out.getvalue().strip()

# provided samples
assert run("Hello!\n") == "YES", "sample 1"
assert run("hello\n") == "NO", "sample 2"

# custom cases
assert run("H\n") == "YES", "single instruction H"
assert run("+++\n") == "NO", "only non-output instructions"
assert run("9abc\n") == "YES", "9 instruction at start"
assert run("abcdQ\n") == "YES", "Q instruction at end"
assert run("!@#$\n") == "NO", "symbols only, no instructions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "H" | CÓ | Lệnh đầu ra đơn | 
| "++++" | KHÔNG | Chỉ số gia tích lũy không tạo ra đầu ra | 
| "9abc" | CÓ | Lệnh xuất ở đầu chuỗi | 
| "abcdQ" | CÓ | Lệnh xuất ở cuối chuỗi | 
| "!@#$" | KHÔNG | Các ký hiệu không hướng dẫn được bỏ qua một cách chính xác | 

## Vỏ cạnh 

Một chuỗi chỉ chứa các chữ cái viết thường hoặc ký hiệu như`"hello!"`tạo ra chính xác "KHÔNG". Thuật toán kiểm tra từng ký tự riêng lẻ và chỉ kích hoạt "CÓ" cho chữ hoa "H", "Q" hoặc chữ số "9". Một chương trình như`"H9+Q"`đưa ra "CÓ" ngay lập tức khi gặp 'H', chứng tỏ rằng việc chấm dứt sớm có hiệu quả. Chương trình một ký tự`"+"`tạo ra "KHÔNG" vì '+' không tạo ra đầu ra. Việc xử lý các trường hợp khó khăn này xác nhận giải pháp này là mạnh mẽ và phù hợp với yêu cầu của vấn đề.
