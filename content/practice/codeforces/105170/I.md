---
title: "CF 105170I - Bài toán dễ nhất"
description: "Nhiệm vụ đưa ra một câu cố định duy nhất làm đầu vào: “Quét mã QR để đăng nhập ngay bây giờ.”. Mục đích không phải là biến đổi hay diễn giải nó mà là tính toán một thuộc tính đơn giản của văn bản chính xác này, cụ thể là có bao nhiêu ký tự trong đó là các chữ cái tiếng Anh viết thường từ a đến z."
date: "2026-06-27T08:33:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105170
codeforces_index: "I"
codeforces_contest_name: "The 2024 CCPC National Invitational Contest (Changchun) , The 17th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 105170
solve_time_s: 125
verified: true
draft: false
---

[CF 105170I - Vấn đề dễ nhất](https://codeforces.com/problemset/problem/105170/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ đưa ra một câu cố định duy nhất làm đầu vào: “Quét mã QR để đăng nhập ngay bây giờ.”. Mục đích không phải là biến đổi hay diễn giải nó mà là tính toán một thuộc tính đơn giản của văn bản chính xác này, cụ thể là có bao nhiêu ký tự trong đó là các chữ cái tiếng Anh viết thường từ a đến z. 

Đầu vào luôn chỉ là một dòng, do đó không có sự phức tạp về phân tích cú pháp ngoài việc đọc một chuỗi. Đầu ra là một số nguyên duy nhất biểu thị số ký tự là chữ thường. Các chữ cái viết hoa, dấu cách, dấu chấm câu và bất kỳ ký hiệu nào khác đều bị bỏ qua. 

Vì độ dài đầu vào không đổi và nhỏ nên không có ràng buộc về thuật toán theo nghĩa lập trình cạnh tranh thông thường. Bất kỳ cách tiếp cận nào từ quét trực tiếp đến thậm chí nhiều lần truyền qua chuỗi đều đủ nhanh. Rủi ro có ý nghĩa duy nhất là các ký tự bị phân loại sai hoặc phân loại sai, ví dụ như vô tình đếm các chữ cái viết hoa hoặc bao gồm cả khoảng trắng. 

Một lỗi phổ biến là coi tất cả các ký tự chữ cái là hợp lệ mà không phân biệt chữ hoa chữ thường. Ví dụ: trong chuỗi con “QR”, cả hai ký tự đều là chữ hoa và không đóng góp vào câu trả lời, mặc dù chúng là các chữ cái. Một sai lầm nữa là quên bỏ qua tiết cuối cùng. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là lặp qua từng ký tự trong chuỗi và kiểm tra xem nó có thuộc phạm vi 'a' đến 'z' hay không. Nếu có, chúng tôi tăng một bộ đếm. Điều này hiệu quả vì mỗi ký tự có thể được phân loại độc lập mà không cần bất kỳ bối cảnh nào từ các lân cận hoặc cấu trúc toàn cầu. 

Chi phí của phương pháp này là tuyến tính theo độ dài của câu. Vì câu cố định và ngắn gọn nên điều này đã tối ưu trong thực tế. Ngay cả khi đầu vào lớn hơn nhiều, một lần quét vẫn đủ vì thao tác trên mỗi ký tự là thời gian không đổi. 

Quan sát quan trọng là vấn đề giảm xuống việc phân loại ký tự. Không cần phân tích từ, phân tách mã thông báo hoặc xử lý ngữ pháp. Cấu trúc câu không liên quan ngoại trừ việc nó là nguồn của các ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Đếm/bộ lọc Python trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Chiến lược tối ưu 

1. Đọc dòng đầu vào dưới dạng chuỗi. Điều này bảo toàn các khoảng trắng và dấu câu chính xác như đã cho, điều này cần thiết vì mọi vị trí ký tự đều quan trọng đối với việc phân loại. 
2. Khởi tạo bộ đếm về 0. Biến này theo dõi số lượng chữ cái viết thường hợp lệ đã được nhìn thấy cho đến nay. 
3. Lặp lại từng ký tự trong chuỗi. Mỗi ký tự được kiểm tra độc lập vì tư cách thành viên trong tập hợp lệ không phụ thuộc vào vị trí. 
4. Đối với mỗi ký tự, hãy kiểm tra xem nó có nằm giữa ‘a’ và ‘z’ hay không. Nếu có, hãy tăng bộ đếm. Điều kiện này tự nhiên loại trừ các chữ cái viết hoa, dấu cách và dấu câu mà không cần có trường hợp đặc biệt cho từng loại. 
5. Sau khi quét xong, xuất bộ đếm. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là điều kiện phân loại sẽ phân chia bộ ký tự thành hai nhóm riêng biệt: các chữ cái viết thường hợp lệ và mọi thứ khác. Mỗi ký tự được xử lý chính xác một lần và đóng góp của nó cho câu trả lời cuối cùng là độc lập với tất cả các ký tự khác. Điều này đảm bảo rằng việc tổng hợp các quyết định cục bộ sẽ tạo ra số lượng toàn cầu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().rstrip("\n")

ans = 0
for ch in s:
    if 'a' <= ch <= 'z':
        ans += 1

print(ans)
```Giải pháp đọc toàn bộ dòng bao gồm cả dấu cách, sau đó chỉ xóa dòng mới ở cuối. Vòng lặp kiểm tra từng ký tự bằng cách sử dụng so sánh phạm vi ASCII trực tiếp, nhanh hơn và rõ ràng hơn các phương thức gọi như`islower`, cũng sẽ bao gồm các chữ cái viết thường không phải tiếng Anh trong một số môi trường. 

quầy`ans`chỉ được cập nhật khi ký tự hoàn toàn nằm trong phạm vi tiếng Anh viết thường, đảm bảo các chữ cái viết hoa như 'S', 'Q' và 'R' bị loại trừ ngay cả khi chúng là chữ cái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
Scan the QR code to sign in now.
```Chúng tôi xử lý các ký tự một cách tuần tự: 

| Nhân vật | Chữ thường a-z? | Đếm | 
| --- | --- | --- | 
| S | Không | 0 | 
| c | Có | 1 | 
| một | Có | 2 | 
| n | Có | 3 | 
| không gian | Không | 3 | 
| t | Có | 4 | 
| h | Có | 5 | 
| e | Có | 6 | 
| không gian | Không | 6 | 
| Hỏi | Không | 6 | 
| R | Không | 6 | 
| không gian | Không | 6 | 
| c | Có | 7 | 
| o | Có | 8 | 
| d | Có | 9 | 
| e | Có | 10 | 
| không gian | Không | 10 | 
| t | Có | 11 | 
| o | Có | 12 | 
| không gian | Không | 12 | 
| s | Có | 13 | 
| tôi | Có | 14 | 
| g | Có | 15 | 
| n | Có | 16 | 
| không gian | Không | 16 | 
| tôi | Có | 17 | 
| n | Có | 18 | 
| không gian | Không | 18 | 
| n | Có | 19 | 
| o | Có | 20 | 
| w | Có | 21 | 
| . | Không | 21 | 

Câu trả lời cuối cùng là 21. 

Dấu vết này xác nhận rằng chỉ các chữ cái tiếng Anh viết thường mới được tính và tất cả các ký tự cấu trúc đều bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
abcXYZ.
```| Nhân vật | Chữ thường a-z? | Đếm | 
| --- | --- | --- | 
| một | Có | 1 | 
| b | Có | 2 | 
| c | Có | 3 | 
| X | Không | 3 | 
| Y | Không | 3 | 
| Z | Không | 3 | 
| . | Không | 3 | 

Đầu ra là 3. 

Điều này cho thấy các chữ cái viết hoa không đóng góp ngay cả khi chúng nằm cạnh các chữ cái viết thường hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra một lần trong một lần quét tuyến tính | 
| Không gian | O(1) | Chỉ một bộ đếm được lưu trữ bất kể kích thước đầu vào | 

Độ dài đầu vào là cố định và nhỏ, do đó giải pháp nằm trong giới hạn tầm thường. Ngay cả đối với đầu vào lớn hơn nhiều, việc quét ký tự một lần vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    input = sys.stdin.readline

    s = input().rstrip("\n")
    ans = 0
    for ch in s:
        if 'a' <= ch <= 'z':
            ans += 1
    return str(ans)

# provided sample (as described)
assert run("Scan the QR code to sign in now.\n") == "21"

# all uppercase only
assert run("ABCDEF.\n") == "0"

# all lowercase letters
assert run("abcdef\n") == "6"

# mixed sentence
assert run("aA bB cC.\n") == "3"

# single character edge
assert run("z\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| “Quét mã QR để đăng nhập ngay bây giờ.” | 21 | câu trường hợp hỗn hợp chính | 
| “ABCDEF.” | 0 | không có chữ thường | 
| “abcdef” | 6 | tất cả các ký tự hợp lệ | 
| “aA bB cC.” | 3 | tính chính xác của việc lọc trường hợp | 
| “z” | 1 | đầu vào tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các chữ cái xuất hiện ở dạng chữ hoa. Ví dụ: trong đầu vào “QR”, cả hai ký tự đều là chữ cái nhưng phải được loại trừ. Thuật toán xử lý điều này một cách chính xác vì nó giới hạn rõ ràng phạm vi ở chữ thường 'a' đến 'z', do đó, các giá trị ASCII viết hoa nằm ngoài điều kiện. 

Một trường hợp khác là dấu câu liền kề. Trong “bây giờ”. dấu chấm ngay sau một chuỗi các chữ cái hợp lệ. Vì việc kiểm tra dựa trên ký tự nên dấu câu không bao giờ ảnh hưởng đến các ký tự lân cận và chỉ tính 'n', 'o' và 'w'. 

Cuối cùng, dấu cách là dấu phân cách thường xuyên trong câu. Thuật toán tự nhiên bỏ qua chúng vì chúng không nằm trong phạm vi chữ thường nên không cần xử lý đặc biệt.
