---
title: "CF 104745A - Cứu nguy rạp chiếu phim"
description: "Nhiệm vụ là xây dựng một bộ phân loại nhỏ có thể đọc một tên duy nhất và trả về một câu cố định tùy thuộc vào vũ trụ hư cấu mà tên đó thuộc về. Đầu vào luôn chính xác là một trong ba tên ký tự có thể có."
date: "2026-06-28T23:01:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "A"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 45
verified: true
draft: false
---

[CF 104745A - Giải cứu rạp chiếu phim](https://codeforces.com/problemset/problem/104745/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là xây dựng một bộ phân loại nhỏ có thể đọc một tên duy nhất và trả về một câu cố định tùy thuộc vào vũ trụ hư cấu mà tên đó thuộc về. Đầu vào luôn chính xác là một trong ba tên ký tự có thể có. Mỗi tên ánh xạ tới một vũ trụ cụ thể: một tên tương ứng với Star Wars, tên khác tương ứng với Star Trek và tên cuối cùng không thuộc về cả hai. 

Từ góc độ thuật toán, không gian đầu vào có kích thước không đổi. Không có sự mơ hồ về phân tích cú pháp, không có cấu trúc ẩn và không có tính toán nào ngoài việc kiểm tra tính bằng nhau đối với các chuỗi đã biết. Điều này đặt vấn đề một cách chắc chắn vào loại trong đó bất kỳ giải pháp tối ưu tiệm cận nào đều tầm thường và các ràng buộc về hiệu suất không ảnh hưởng đến các quyết định thiết kế. 

Mặc dù logic đơn giản nhưng vẫn có một số cạm bẫy khi triển khai. Một nỗ lực ngây thơ có thể liên quan đến việc khớp một phần thay vì so sánh chuỗi chính xác. Ví dụ: kiểm tra xem liệu đầu vào có chứa chuỗi con “o” có khớp không chính xác với cả ba đầu vào hay không, dẫn đến phân loại không rõ ràng hoặc không chính xác. 

Một chế độ lỗi tinh vi khác xuất hiện khi xử lý các ký tự khoảng trắng hoặc dòng mới. Nếu đầu vào không được loại bỏ đúng cách, việc so sánh đẳng thức trực tiếp với “Yoda”, “Spock” ​​hoặc “Frodo” sẽ không thành công ngay cả khi giá trị logic khớp. Ví dụ: đọc đầu vào dưới dạng`"Yoda\n"`và so sánh trực tiếp với`"Yoda"`sẽ tạo ra sự không khớp trừ khi đầu vào được chuẩn hóa. 

Trường hợp cạnh thứ ba là xử lý viết hoa không chính xác. Sự cố chỉ định các chuỗi chính xác, do đó, việc xử lý đầu vào theo cách không phân biệt chữ hoa chữ thường sẽ chấp nhận không chính xác các biến thể như “yoda” hoặc “YODA”, những biến thể này không được coi là hợp lệ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trong bối cảnh này vẫn sẽ là so sánh trực tiếp với từng chuỗi hợp lệ có thể có bằng cách sử dụng một chuỗi kiểm tra có điều kiện. Ngay cả khi được triển khai không hiệu quả, chẳng hạn như quét qua danh sách ứng cử viên và so sánh từng ký tự, tổng công việc vẫn không đổi vì số lượng chuỗi hợp lệ được cố định ở mức ba và độ dài của chúng bị giới hạn. 

Một cách tiếp cận có cấu trúc hơn là ánh xạ rõ ràng từng chuỗi đầu vào tới câu đầu ra tương ứng bằng cách sử dụng từ điển hoặc các điều kiện theo chuỗi. Điều này loại bỏ mọi sự mơ hồ và thực hiện phép biến đổi trực tiếp: mỗi khóa tương ứng với chính xác một chuỗi đầu ra. 

Quan sát quan trọng là miền này có hạn và được biết trước. Điều này làm giảm vấn đề từ nhiệm vụ phân loại chuỗi chung sang vấn đề bảng tra cứu với độ phân giải thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (so sánh thủ công) | O(1) | O(1) | Đã chấp nhận | 
| Ánh xạ trực tiếp (từ điển / if-else) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào từ đầu vào tiêu chuẩn và xóa khoảng trắng ở cuối để các thành phần định dạng không ảnh hưởng đến so sánh. Điều này đảm bảo chuỗi ở dạng chuẩn hóa phù hợp để khớp chính xác. 
2. So sánh chuỗi đã chuẩn hóa với ba trường hợp hợp lệ đã biết. Mỗi so sánh là một phép kiểm tra tính bằng nhau của chuỗi đầy đủ, điều này là đủ vì tập hợp các đầu vào hợp lệ là cố định và nhỏ. 
3. Nếu chuỗi khớp với “Yoda”, hãy xuất ra câu biểu thị tư cách thành viên trong Star Wars. 
4. Nếu chuỗi khớp với “Spock”, hãy xuất ra câu biểu thị tư cách thành viên trong Star Trek. 
5. Nếu chuỗi khớp với “Frodo”, hãy xuất ra câu cho biết nó không thuộc về vũ trụ nào. 

Mỗi nhánh loại trừ lẫn nhau, do đó chỉ có một đầu ra được tạo ra. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là miền đầu vào chứa chính xác ba giá trị có thể và mỗi giá trị được liên kết với chính xác một đầu ra. Bởi vì so sánh đẳng thức phân chia không gian đầu vào thành các trường hợp riêng biệt bao gồm tất cả các khả năng, nên mọi đầu vào hợp lệ đều được xử lý duy nhất. Không có khả năng xảy ra trường hợp trùng lặp hoặc thiếu vì việc ánh xạ đã đầy đủ trên tập hợp được phép. 

## Giải pháp Python```python
import sys

def solve():
    s = sys.stdin.readline().strip()

    if s == "Yoda":
        print("Pertenece a Star Wars.")
    elif s == "Spock":
        print("Pertenece a Star Trek.")
    else:
        print("No pertenece ni a Star Wars ni a Star Trek.")

if __name__ == "__main__":
    solve()
```Giải pháp đọc một dòng duy nhất, bình thường hóa nó bằng cách sử dụng`strip`, sau đó thực hiện chuỗi so sánh trực tiếp. các`strip`cuộc gọi là cần thiết vì đầu vào từ đầu vào tiêu chuẩn thường bao gồm một dòng mới ở cuối, nếu không sẽ phá vỡ các kiểm tra tính bằng nhau chính xác. 

Cấu trúc có điều kiện được sắp xếp có chủ ý sao cho hai thành viên vũ trụ đã biết được kiểm tra trước tiên. trận chung kết`else`nhánh xử lý an toàn đầu vào hợp lệ duy nhất còn lại, “Frodo”, vì sự cố đảm bảo rằng đầu vào luôn là một trong ba chuỗi được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào là “Spock”. 

| Bước | Chuỗi hiện tại | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Spock | dải áp dụng | Spock | 
| 2 | Spock | bằng "Yoda" | sai | 
| 3 | Spock | bằng "Spock" | đúng | 
| 4 | Spock | đầu ra được in | câu Star Trek | 

Dấu vết này cho thấy lần so sánh thứ hai thành công ngay lập tức và không cần phân nhánh thêm. 

### Ví dụ 2 

Đầu vào là “Frodo”. 

| Bước | Chuỗi hiện tại | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Frodo | dải áp dụng | Frodo | 
| 2 | Frodo | bằng "Yoda" | sai | 
| 3 | Frodo | bằng "Spock" | sai | 
| 4 | Frodo | rơi vào chỗ khác | đúng | 
| 5 | Frodo | đầu ra được in | câu không vũ trụ | 

Điều này xác nhận rằng nhánh dự phòng nắm bắt chính xác trường hợp hợp lệ duy nhất còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một số lượng so sánh chuỗi không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài một chuỗi đầu vào | 

Hành vi thời gian không đổi là đủ cho bất kỳ kích thước đầu vào hợp lý nào và dung lượng bộ nhớ không tăng theo đầu vào vì sự cố xử lý chính xác một chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("Spock") == "Pertenece a Star Trek."
assert run("Yoda") == "Pertenece a Star Wars."
assert run("Frodo") == "No pertenece ni a Star Wars ni a Star Trek."

# custom cases
assert run("Yoda\n") == "Pertenece a Star Wars.", "newline handling"
assert run("Spock ") == "Pertenece a Star Trek.", "trailing space handling"
assert run("Frodo") == "No pertenece ni a Star Wars ni a Star Trek.", "neutral case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"Yoda\n"`| Câu chiến tranh giữa các vì sao | chuẩn hóa dòng mới | 
|`"Spock "`| câu Star Trek | xử lý khoảng trắng ở cuối | 
|`"Frodo"`| không câu | tính chính xác dự phòng | 

## Vỏ cạnh 

Trường hợp Edge phù hợp nhất là sự hiện diện của các ký tự khoảng trắng ẩn trong đầu vào. Nếu đầu vào có dạng “Yoda\n”, việc so sánh trực tiếp mà không chuẩn hóa sẽ không thành công. Trong quá trình triển khai,`strip()`cuộc gọi đảm bảo rằng giá trị bên trong trở thành chính xác là Yoda, sau đó điều kiện đầu tiên được đánh giá là đúng và câu Star Wars chính xác được tạo ra. 

Một trường hợp khác là dấu cách ở cuối, chẳng hạn như “Spock”. Sau khi loại bỏ, chuỗi trở thành “Spock”, do đó điều kiện thứ hai khớp và câu Star Trek được in ra. Điều này chứng tỏ rằng quá trình chuẩn hóa diễn ra trước khi phân loại, đảm bảo tính mạnh mẽ chống lại nhiễu định dạng. 

Trường hợp cuối cùng là đầu vào dự phòng được đảm bảo “Frodo”. Vì nó không khớp với một trong hai điều kiện đầu tiên nên việc thực thi tự nhiên rơi vào nhánh cuối cùng và tạo ra phản hồi “không” chính xác mà không yêu cầu kiểm tra tính bằng nhau rõ ràng.
