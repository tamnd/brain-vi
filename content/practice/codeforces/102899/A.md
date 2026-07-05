---
title: "CF 102899A - KK \u753b\u732a"
description: "Nhiệm vụ này được cố ý tối thiểu hóa: chương trình nhận được một mã thông báo duy nhất trên đầu vào tiêu chuẩn và phải phản hồi bằng hình vẽ con lợn ASCII cố định."
date: "2026-07-04T08:19:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "A"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 45
verified: true
draft: false
---

[CF 102899A - KK \u753b\u732a](https://codeforces.com/problemset/problem/102899/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này được cố ý tối thiểu hóa: chương trình nhận được một mã thông báo duy nhất trên đầu vào tiêu chuẩn và phải phản hồi bằng hình vẽ con lợn ASCII cố định. Đầu vào không phải là một vấn đề xử lý chuỗi chung vì không cần chuyển đổi, không cần logic phân tích cú pháp và không cần tính toán trên cấu trúc. Thay vào đó, đầu vào hoạt động như một trình kích hoạt và đầu ra luôn là tác phẩm nghệ thuật nhiều dòng giống nhau. 

Từ góc độ thuật toán, “dữ liệu” ở đây về cơ bản không liên quan gì ngoài việc hiện diện. Yêu cầu thực sự là tái tạo chính xác khối văn bản được xác định trước, bao gồm dấu cách, dấu gạch chéo ngược và dấu câu mà nếu không sẽ dễ bị hỏng trong quá trình định dạng đầu ra. 

Hồ sơ ràng buộc là tầm thường theo nghĩa lập trình cạnh tranh. Ngay cả khi chúng tôi giả sử các giới hạn tiêu chuẩn nhất là 1 giây và 256 megabyte, thì giải pháp cũng không thể tiếp cận các giới hạn đó một cách có ý nghĩa. Rủi ro thực sự duy nhất là lỗi triển khai khi xử lý chuỗi thô, đặc biệt là thoát dấu gạch chéo ngược trong Python và giữ nguyên khoảng trắng chính xác. 

Các trường hợp biên không phải về phân nhánh thuật toán mà là về độ trung thực đầu ra. Một số dạng hư hỏng cụ thể minh họa điều này. 

Nếu một giải pháp cố gắng xây dựng nghệ thuật ASCII bằng cách nối chuỗi thông thường mà không cẩn thận thoát khỏi dấu gạch chéo ngược, các dòng như`(\____/)`hoặc`/ @__@ \`có thể mất ký tự hoặc tạo ra chuỗi thoát không hợp lệ. Việc triển khai đơn giản cũng có thể vô tình loại bỏ các khoảng trắng ở cuối khi sử dụng các hàm định dạng, điều này sẽ làm hỏng bản vẽ ngay cả khi logic đúng. 

Một chế độ lỗi khác xảy ra khi các nhà phát triển cố gắng “tính toán” đầu ra dựa trên chuỗi đầu vào. Vì đầu vào luôn chỉ là`"pig"`, mọi logic có điều kiện không cần thiết đều làm tăng khả năng xảy ra hành vi không khớp mà không thêm giá trị chính xác. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của nhiệm vụ là đọc chuỗi đầu vào, xác minh nó khớp với từ kích hoạt dự kiến, sau đó in từng dòng nghệ thuật ASCII liên quan. Điều này đã đủ vì không có sự thay đổi về đầu ra. Công việc được thực hiện là tuyến tính theo kích thước của văn bản đầu ra, không đổi. 

Một nỗ lực bạo lực phức tạp hơn có thể liên quan đến việc lưu trữ nghệ thuật ASCII dưới dạng danh sách các chuỗi và lặp lại nó để in từng dòng. Điều này vẫn đúng, nhưng nó đưa ra cấu trúc không cần thiết mà không làm thay đổi độ phức tạp hoặc hành vi. Quan sát quan trọng là đầu vào không ảnh hưởng đến đầu ra ngoài việc xác nhận rằng chúng ta nên in nó. 

Giải pháp tối ưu thu gọn mọi thứ thành một quyết định duy nhất theo thời gian không đổi: bỏ qua nội dung đầu vào và trực tiếp phát ra khối được xác định trước. Cấu trúc của bài toán có thể thực hiện được điều này vì có chính xác một đầu ra hợp lệ và không có nhánh có điều kiện dựa trên biến thể đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (phân tích cú pháp + điều kiện + in từng dòng) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (chuỗi hằng in trực tiếp) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào từ đầu vào tiêu chuẩn, ngay cả khi giá trị của nó không ảnh hưởng đến kết quả. Điều này chỉ để thỏa mãn hợp đồng đầu vào của vấn đề. 
2. Chuẩn bị một chuỗi nhiều dòng cố định thể hiện nghệ thuật ASCII lợn chính xác theo yêu cầu của đặc tả đầu ra. 
3. In chuỗi thành đầu ra tiêu chuẩn mà không cần sửa đổi. 

Ý tưởng chính là không cần phân nhánh vì vấn đề xác định một trạng thái đầu ra hợp lệ duy nhất. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là bài toán xác định ánh xạ xác định từ bất kỳ đầu vào hợp lệ nào đến một đầu ra cố định duy nhất. Vì tất cả các đầu vào hợp lệ đều có hiệu lực giống hệt nhau nên hàm đầu ra không đổi. Thuật toán không cần kiểm tra nội dung đầu vào; nó chỉ cần đảm bảo tái tạo trung thực chuỗi được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    _ = input().strip()

    art = r"""(\____/)
 / @__@ \
( (oo) )
‘-.~~.-’
 /
 \
 @/
 \_
 (/ /
 \ \)
WW‘----’WW
"""
    sys.stdout.write(art)

if __name__ == "__main__":
    solve()
```Việc triển khai đọc và loại bỏ đầu vào vì nó không liên quan ngoài việc kích hoạt đầu ra. Nghệ thuật ASCII được lưu trữ dưới dạng một chuỗi nhiều dòng thô để các dấu gạch chéo ngược được giữ nguyên mà không cần thoát thủ công. Điều này tránh được những cạm bẫy Python phổ biến trong đó các chuỗi như`\_`hoặc`\ `mặt khác có thể được hiểu là chuỗi thoát. 

Đầu ra được viết trực tiếp bằng cách sử dụng`sys.stdout.write`để tránh mọi định dạng ngẫu nhiên hoặc chèn dòng mới bổ sung có thể xảy ra khi in trong một số môi trường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là một từ duy nhất kích hoạt đầu ra. 

| Bước | Đọc đầu vào | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | lợn | đọc và loại bỏ | | 
| 2 | lợn | tải nghệ thuật ASCII | | 
| 3 | lợn | nghệ thuật in ấn | lợn nghệ thuật ASCII | 

Dấu vết này cho thấy nội dung đầu vào không bao giờ tham gia vào bất kỳ quá trình ra quyết định nào. Đầu ra được xác định đầy đủ trước khi bắt đầu thực hiện. 

### Mẫu 2 

Hãy xem xét lại cùng một đầu vào vì không thể thay đổi được. 

| Bước | Đọc đầu vào | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | lợn | đọc và loại bỏ | | 
| 2 | lợn | không có logic có điều kiện | | 
| 3 | lợn | in nghệ thuật giống hệt nhau | lợn nghệ thuật ASCII | 

Điều này xác nhận rằng các lần thực thi lặp lại là giống hệt nhau và không có trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một lượng đọc và in đầu vào không đổi của một chuỗi cố định | 
| Không gian | O(1) | Nghệ thuật ASCII có kích thước không đổi độc lập với đầu vào | 

Những hạn chế của vấn đề vượt xa những gì cần thiết ở đây. Ngay cả việc thực hiện lặp đi lặp lại hoặc giới hạn hệ thống lớn cũng không liên quan vì kích thước đầu ra là cố định và nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    import sys
    input = sys.stdin.readline

    def solve():
        _ = input().strip()
        art = r"""(\____/)
 / @__@ \
( (oo) )
‘-.~~.-’
 /
 \
 @/
 \_
 (/ /
 \ \)
WW‘----’WW
"""
        sys.stdout.write(art)

    solve()
    return sys.stdout.getvalue()

# provided sample
assert run("pig\n") == r"""(\____/)
 / @__@ \
( (oo) )
‘-.~~.-’
 /
 \
 @/
 \_
 (/ /
 \ \)
WW‘----’WW
"""

# custom cases
assert run("pig\n") != "", "output must not be empty"
assert run("pig\nextra") == run("pig\n"), "extra input after token ignored in strip-based read"
assert run("pig\n") == run("pig\n"), "idempotence check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lợn | lợn ASCII | tính đúng đắn của trường hợp tiêu chuẩn | 
| lợn có thêm văn bản | lợn ASCII | đầu vào không liên quan | 
| lợn chạy lặp đi lặp lại | đầu ra giống hệt nhau | thuyết định mệnh | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là việc bảo toàn khoảng trắng ở cuối trong nghệ thuật ASCII. Nếu bất kỳ dòng nào bị mất khoảng trắng ở đầu hoặc cuối, hình dạng của con lợn sẽ trở nên không chính xác về mặt thị giác ngay cả khi chương trình “chính xác” về mặt logic. Cách tiếp cận chuỗi thô tránh được điều này hoàn toàn bằng cách giữ nguyên nguyên văn định dạng. 

Một trường hợp khác là việc giải thích ngẫu nhiên các dấu gạch chéo ngược. Ví dụ, viết`/ \`không có chuỗi thô có thể khiến Python hiểu sai`\`như một tiền tố thoát. Cách biểu diễn được chọn đảm bảo mỗi dấu gạch chéo ngược được coi là một ký tự chữ. 

Cuối cùng, một số triển khai có thể cố gắng chỉ in có điều kiện khi đầu vào bằng`"pig"`. Mặc dù điều này hoạt động theo các ràng buộc nhất định, nhưng nó gây ra sự phụ thuộc không cần thiết vào tính chính xác của đầu vào. Vì vấn đề đảm bảo một định dạng đầu vào hợp lệ duy nhất nên cách tiếp cận an toàn nhất là đầu ra vô điều kiện, loại bỏ toàn bộ các trường hợp lỗi.
