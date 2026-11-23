---
title: "CF 443A - Anton và những lá thư"
description: "Chúng ta được cấp một chuỗi được định dạng duy nhất đại diện cho một tập hợp các chữ cái tiếng Anh viết thường. Bộ này được viết ở dạng văn bản rất cụ thể: nó bắt đầu bằng dấu ngoặc nhọn mở, kết thúc bằng dấu ngoặc nhọn đóng và bên trong dấu ngoặc nhọn liệt kê các chữ cái được phân tách bằng dấu phẩy và dấu cách."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 443
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 253 (Div. 2)"
rating: 800
weight: 443
solve_time_s: 79
verified: true
draft: false
---

[CF 443A - Anton và những lá thư](https://codeforces.com/problemset/problem/443/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi được định dạng duy nhất đại diện cho một tập hợp các chữ cái tiếng Anh viết thường. Bộ này được viết ở dạng văn bản rất cụ thể: nó bắt đầu bằng dấu ngoặc nhọn mở, kết thúc bằng dấu ngoặc nhọn đóng và bên trong dấu ngoặc nhọn liệt kê các chữ cái được phân tách bằng dấu phẩy và dấu cách. Cùng một chữ cái có thể xuất hiện nhiều lần vì Anton có thể đã viết nhầm các bản sao, nhưng về mặt toán học, tập hợp chỉ nên giữ các phần tử riêng biệt. 

Nhiệm vụ là xác định có bao nhiêu chữ cái duy nhất xuất hiện bên trong cách biểu diễn này. Đầu ra là một số nguyên duy nhất, kích thước của tập hợp sau khi trùng lặp sẽ bị bỏ qua. 

Độ dài đầu vào tối đa là 1000 ký tự, có nghĩa là bất kỳ giải pháp nào quét chuỗi với số lần không đổi đều đủ nhanh. Ngay cả một đường truyền tuyến tính đơn giản cũng đủ, vì 1000 phép tính là không đáng kể. Điều này ngay lập tức loại trừ không có gì đáng kể, nhưng nó cho chúng ta biết rằng chúng ta nên ưu tiên phân tích cú pháp đơn giản hơn bất kỳ thứ gì phức tạp như quay lui hoặc logic phân tích cú pháp lồng nhau. 

Sự tinh tế chính không phải là hiệu suất mà là tính chính xác của phân tích cú pháp. Đầu vào bao gồm các ký tự dấu chấm câu như dấu ngoặc nhọn, dấu phẩy và dấu cách. Một cách tiếp cận đơn giản là phân chia không chính xác hoặc quên bỏ qua các ký hiệu này có thể vô tình đếm chúng hoặc bao gồm các mã thông báo trống. 

Một trường hợp thất bại phổ biến là coi mọi ký tự ngoại trừ dấu ngoặc nhọn là một chữ cái. Ví dụ: trong chuỗi "{a, b}", nếu chúng ta lặp đi lặp lại một cách mù quáng, chúng ta có thể vô tình đưa vào dấu phẩy và dấu cách. Một trường hợp thất bại khác là chỉ phân tách bằng dấu phẩy mà quên rằng mã thông báo chứa dấu cách ở đầu, tạo ra các chuỗi như " b" thay vì "b". Nếu không được loại bỏ đúng cách, các mã thông báo như vậy sẽ được coi là các chuỗi riêng biệt và làm tăng câu trả lời. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của vấn đề sẽ là trích xuất thủ công tất cả các chuỗi con giữa các dấu phẩy, làm sạch chúng và chèn chúng vào danh sách. Đối với mỗi mã thông báo được trích xuất, chúng tôi sẽ so sánh nó với tất cả các mã thông báo đã thấy trước đó để xác định tính duy nhất. Điều này hiệu quả vì đầu vào nhỏ nhưng nó đưa ra hành vi bậc hai không cần thiết: đối với k chữ cái, mỗi lần tra cứu có thể có giá trị O(k), dẫn đến hành vi O(k^2) trong trường hợp xấu nhất. Mặc dù k được giới hạn bởi 1000, nhưng điều này vẫn là quá mức cần thiết và không cần thiết vì hàm băm tồn tại. 

Quan sát quan trọng là chúng ta không cần thứ tự hoặc số lượng tần số, chỉ cần tính duy nhất. Một cấu trúc tập hợp mô hình hóa trực tiếp yêu cầu này. Khi chúng tôi quét chuỗi, mọi chữ cái hợp lệ có thể được chèn vào bộ băm để xử lý việc loại bỏ trùng lặp trong thời gian dự kiến ​​là O(1) cho mỗi lần chèn. Các ký tự dấu chấm câu có thể được bỏ qua trong quá trình quét. 

Cấu trúc của dữ liệu đầu vào đảm bảo rằng các chữ cái hợp lệ luôn là các ký tự viết thường đơn lẻ, vì vậy chúng ta không bao giờ cần phân tích cú pháp các mã thông báo nhiều ký tự. Điều này làm giảm vấn đề xuống còn một đường truyền tuyến tính duy nhất với tính năng lọc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Danh sách Brute Force + Tìm kiếm tuyến tính | O(n^2) | O(n) | Quá chậm | 
| Thẻ đơn theo bộ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào dưới dạng một dòng. Chuỗi chứa dấu ngoặc nhọn, dấu phẩy, dấu cách và chữ thường. 
2. Khởi tạo một tập hợp trống để lưu trữ các chữ cái riêng biệt. Bộ này được sử dụng vì nó tự động bỏ qua các bản sao. 
3. Lặp lại từng ký tự trong chuỗi. 
4. Nếu ký tự nằm giữa 'a' và 'z', hãy chèn ký tự đó vào bộ. Bất kỳ ký tự nào khác đều bị bỏ qua vì đó là dấu câu cấu trúc. 
5. Sau khi xử lý tất cả các ký tự, kích thước của tập hợp là số lượng chữ cái riêng biệt. 
6. Xuất kích thước này.

Lý do chúng ta có thể bỏ qua mọi thứ một cách an toàn ngoại trừ chữ cái viết thường là định dạng đảm bảo tất cả các phần tử có ý nghĩa đều là các ký tự đơn trong phạm vi đó. Không có nhân vật nào khác đóng góp vào nội dung đã đặt. 

### Tại sao nó hoạt động 

Thuật toán duy trì một bất biến rằng tại bất kỳ thời điểm nào trong quá trình quét, tập hợp này chứa chính xác các chữ cái riêng biệt được nhìn thấy cho đến nay trong chuỗi. Mỗi lần chèn thêm một chữ cái mới hoặc không làm gì nếu nó đã có sẵn. Vì mọi chữ cái hợp lệ đều xuất hiện ở đâu đó trong chuỗi nên nó sẽ được xử lý chính xác một lần cho mỗi lần xuất hiện và tập hợp đảm bảo tính bội số không thành vấn đề. Cuối cùng, tập hợp này chứa chính xác các chữ cái duy nhất trong biểu diễn, phù hợp với định nghĩa của câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

seen = set()

for ch in s:
    if 'a' <= ch <= 'z':
        seen.add(ch)

print(len(seen))
```Giải pháp đọc toàn bộ dòng đầu vào một lần và lặp lại từng ký tự. điều kiện`'a' <= ch <= 'z'`đảm bảo chúng tôi chỉ xử lý các chữ cái thực tế và bỏ qua dấu ngoặc nhọn, dấu phẩy và dấu cách. bộ`seen`tích lũy các ký tự riêng biệt một cách hiệu quả. 

Một chi tiết triển khai tinh tế là việc sử dụng`strip()`khi đọc đầu vào. Điều này loại bỏ dòng mới ở cuối mà không ảnh hưởng đến không gian bên trong, dù sao thì điều này không liên quan. Không cần chia tách, điều này tránh hoàn toàn các cạm bẫy về mã thông báo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
{a, b, c}
```Chúng tôi quét từng ký tự và duy trì một bộ. 

| Nhân vật | Hành động | Đặt trạng thái | 
| --- | --- | --- | 
| { | bỏ qua | {} | 
| một | thêm | {a} | 
| , | bỏ qua | {a} | 
| | bỏ qua | {a} | 
| b | thêm | {a, b} | 
| , | bỏ qua | {a, b} | 
| | bỏ qua | {a,b} | 
| c | thêm | {a, b, c} | 
| } | bỏ qua | {a, b, c} | 

Kích thước tập hợp cuối cùng là 3. 

Dấu vết này cho thấy dấu câu không bao giờ ảnh hưởng đến trạng thái và chỉ các chữ cái mới góp phần tạo nên tính duy nhất. 

### Ví dụ 2 

đầu vào:```
{a, a, b, a}
```| Nhân vật | Hành động | Đặt trạng thái | 
| --- | --- | --- | 
| { | bỏ qua | {} | 
| một | thêm | {a} | 
| , | bỏ qua | {a} | 
| | bỏ qua | {a} | 
| một | đã tồn tại | {a} | 
| , | bỏ qua | {a} | 
| | bỏ qua | {a} | 
| b | thêm | {a,b} | 
| , | bỏ qua | {a,b} | 
| | bỏ qua | {a,b} | 
| một | đã tồn tại | {a,b} | 
| } | bỏ qua | {a,b} | 

Câu trả lời cuối cùng là 2. 

Điều này xác nhận rằng các bản sao không ảnh hưởng đến số lượng cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và đặt mức chèn trung bình là O(1) | 
| Không gian | O(1) | Có thể lưu trữ tối đa 26 chữ cái viết thường | 

Kích thước đầu vào tối đa là 1000, do đó, giải pháp một lần vượt qua cũng nằm trong giới hạn cho phép và chạy ngay lập tức trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    seen = set()
    for ch in s:
        if 'a' <= ch <= 'z':
            seen.add(ch)
    return str(len(seen))

# provided sample
assert run("{a, b, c}\n") == "3"

# single element
assert run("{a}\n") == "1"

# duplicates only
assert run("{a, a, a}\n") == "1"

# all letters
assert run("{a, b, c, d, e}\n") == "5"

# alternating duplicates
assert run("{a, b, a, c, b, d}\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| {a} | 1 | đầu vào hợp lệ tối thiểu | 
| {a, a, a} | 1 | xử lý trùng lặp | 
| {a,b,a,c,b,d} | 4 | trùng lặp hỗn hợp và đặt hàng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các chữ cái giống hệt nhau, chẳng hạn như "{a, a, a}". Thuật toán xử lý mọi 'a' và chèn nó vào tập hợp. Sau lần chèn đầu tiên, các lần chèn tiếp theo không làm gì cả, vì vậy tập cuối cùng vẫn là {a}, tạo ra kết quả 1. 

Một trường hợp khác là khi tập hợp chỉ chứa một phần tử "{z}". Quá trình quét bỏ qua dấu ngoặc nhọn và trực tiếp thêm 'z', tạo ra một tập hợp một phần tử. 

Trường hợp tinh tế cuối cùng là sự thay đổi khoảng cách bên trong dấu ngoặc nhọn. Mặc dù mỗi dấu phẩy được theo sau bởi một khoảng trắng, thuật toán không bao giờ dựa vào ranh giới mã thông báo, do đó các khoảng trắng thừa sẽ tự động bị bỏ qua. Chỉ các ký tự từ 'a' đến 'z' mới quan trọng, do đó các biến thể định dạng không thể ảnh hưởng đến tính chính xác.
