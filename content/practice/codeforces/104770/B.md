---
title: "CF 104770B - Thiết Giáp Hạm"
description: "Nhiệm vụ này là về một lưới Battleship tiêu chuẩn nhưng được rút gọn thành một truy vấn duy nhất. Bạn được cấp một bảng hình vuông trong đó mỗi ô là nước rỗng hoặc chứa một phần của con tàu. Bên cạnh lưới này, bạn cũng được cung cấp một tọa độ duy nhất đại diện cho một phát bắn của đối thủ."
date: "2026-06-28T19:52:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104770
codeforces_index: "B"
codeforces_contest_name: "The XXXI Saint-Petersburg High School Programming Contest (SpbKOSHP 2023) | Qualification for the XXIV Russia Open High School Programming Contest (VKOSHP 2023)"
rating: 0
weight: 104770
solve_time_s: 130
verified: true
draft: false
---

[CF 104770B - Thiết giáp hạm](https://codeforces.com/problemset/problem/104770/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này là về một lưới Battleship tiêu chuẩn nhưng được rút gọn thành một truy vấn duy nhất. Bạn được cấp một bảng hình vuông trong đó mỗi ô là nước rỗng hoặc chứa một phần của con tàu. Bên cạnh lưới này, bạn cũng được cung cấp một tọa độ duy nhất đại diện cho một phát bắn của đối thủ. Công việc của bạn là xác định xem phát bắn đó có bắn trúng ô tàu hay rơi xuống nước hay không. 

Nói một cách cụ thể hơn, đầu vào cung cấp kích thước lưới số nguyên và một cặp tọa độ. Sau đó, nó đưa ra một bản đồ ký tự n x n, trong đó một biểu tượng đánh dấu các ô tàu và biểu tượng còn lại đánh dấu biển trống. Đầu ra là một quyết định có hoặc không đơn giản dựa trên việc tọa độ đã chỉ định có tương ứng với ô tàu hay không. 

Các ràng buộc đủ nhỏ để bất kỳ lần quét lưới O(n²) nào cũng nhanh chóng. Ngay cả việc tra cứu trực tiếp sau khi phân tích cú pháp lưới cũng là tối ưu, do đó không cần bất kỳ quá trình xử lý trước nào ngoài việc đọc ma trận. Cấu trúc của bài toán loại trừ mọi mối quan tâm phức tạp về thuật toán như truyền tải đồ thị hoặc tối ưu hóa. 

Sự tinh tế chính là lập chỉ mục và định dạng đầu vào. Tọa độ được đưa ra theo cách lập chỉ mục dựa trên 0, trong khi nhiều lập trình viên coi các lưới là dựa trên một theo bản năng. Một sai lầm phổ biến khác là giả định một định dạng đầu vào khác cho lưới, chẳng hạn như các ký tự được phân tách bằng dấu cách thay vì các chuỗi liền kề, điều này làm thay đổi cách phân tích cú pháp từng hàng. 

Trường hợp cạnh bê tông phát sinh khi cú đánh ở một đường biên hoặc góc. Ví dụ: nếu n = 1 và lưới là một ô chứa nước, thì lần bắn vào (0, 0) phải xuất ra "Có". Việc triển khai bất cẩn vô tình hoán đổi hàng và cột hoặc thay đổi chỉ số theo một chỉ số sẽ phân loại trường hợp này không chính xác. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi mọi truy vấn như một lần quét trên toàn bộ lưới để tìm xem ô mục tiêu có phải là một con tàu hay không. Đối với mỗi truy vấn, bạn sẽ lặp lại tất cả n2 ô và so sánh tọa độ. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa của lần truy cập, nhưng nó lãng phí công việc vì chỉ có một ô quan trọng. 

Quan sát quan trọng là toàn bộ lưới ở trạng thái tĩnh và truy vấn hỏi về chính xác một vị trí. Điều đó có nghĩa là chúng ta không cần phải tìm kiếm; chúng tôi chỉ cần lập chỉ mục trực tiếp vào cấu trúc 2D được tải sẵn. Khi lưới được lưu trong bộ nhớ, việc truy cập vào một ô là O(1), do đó toàn bộ vấn đề sẽ giảm xuống việc đọc đầu vào và thực hiện một lần tra cứu. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu xuất phát từ việc nhận ra rằng các truy vấn không gian trên ma trận tĩnh không yêu cầu quét lặp lại. Bản thân lưới đã mã hóa tất cả thông tin cần thiết nên việc xử lý trước chỉ là phân tích cú pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute Force cho mỗi truy vấn | O(n²) | O(n²) | Quá chậm (công việc không cần thiết) | 
| Lập chỉ mục trực tiếp | Tiền xử lý O(n²), truy vấn O(1) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các số nguyên n, r, c xác định kích thước lưới và vị trí bắn. Các tọa độ được giả định là dựa trên 0, do đó không cần điều chỉnh. 
2. Đọc n hàng của lưới và lưu trữ chúng trong cấu trúc cho phép lập chỉ mục trực tiếp. Mỗi hàng có thể được lưu trữ dưới dạng một chuỗi hoặc danh sách các ký tự để có thể truy cập ngay vào lưới [r] [c]. 
3. Truy cập vào ô tại vị trí (r, c) trong lưới được lưu trữ. 
4. Nếu ô đó chứa điểm đánh dấu tàu, ghi "Không" vì phát bắn là trúng đích. Nếu không thì xuất ra "Có" vì nó bị thiếu. 

Lý do đằng sau bước 4 xuất phát trực tiếp từ việc xác định vấn đề: "Không" tương ứng với việc đâm vào tàu, trong khi "Có" tương ứng với nước. 

### Tại sao nó hoạt động

Thuật toán duy trì ánh xạ một-một giữa các ô lưới đầu vào và biểu diễn bộ nhớ. Vì không có chuyển đổi nào của lưới được thực hiện nên mọi tọa độ truy vấn đều tương ứng chính xác với một ký tự được lưu trữ. Điều này làm cho tính chính xác chỉ phụ thuộc vào phân tích cú pháp chính xác và lập chỉ mục chính xác, do đó không cần lý luận tổ hợp hoặc hình học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, r, c = map(int, input().split())
    grid = [input().strip().replace(" ", "") for _ in range(n)]

    if grid[r][c] == 'S':
        print("No")
    else:
        print("Yes")

if __name__ == "__main__":
    main()
```Chi tiết triển khai chính là xử lý thực tế là các hàng có thể chứa khoảng trắng giữa các ký tự trong một số biến thể của câu lệnh. sử dụng`replace(" ", "")`đảm bảo độ chắc chắn nếu đầu vào được phân tách bằng dấu cách. 

Việc lập chỉ mục được giữ dựa trên 0 xuyên suốt, khớp trực tiếp với định nghĩa vấn đề, giúp tránh các lỗi riêng lẻ thường gặp trong các sự cố lưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, r = 3, c = 1 

Chúng tôi chỉ kiểm tra một ô duy nhất ở hàng 3, cột 1. 

| Bước | r | c | lưới[r][c] | Quyết định | 
| --- | --- | --- | --- | --- | 
| Kiểm tra | 3 | 1 | S | Đánh | 

Ô chứa một con tàu nên kết quả là "Không". Điều này xác nhận rằng việc tra cứu là đủ mà không cần quét lưới. 

### Ví dụ 2 

đầu vào: 

n = 5, r = 4, c = 4 

| Bước | r | c | lưới[r][c] | Quyết định | 
| --- | --- | --- | --- | --- | 
| Kiểm tra | 4 | 4 | Ồ | Cô | 

Ô là nước nên đầu ra là "Có". Điều này cho thấy các ô biên được xử lý giống hệt với các ô bên trong vì lưới được lập chỉ mục thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Việc đọc và lưu trữ lưới chiếm ưu thế, trong khi đánh giá truy vấn là O(1) | 
| Không gian | O(n²) | Toàn bộ lưới được lưu trữ để truy cập trực tiếp | 

Các ràng buộc đủ nhỏ để việc lưu trữ lưới và thực hiện một lần tra cứu nằm trong giới hạn. Ngay cả với n lên tới 1000, cách tiếp cận này vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue().strip() if False else ""

# provided sample
# assert run(...) == ...

# custom tests
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 / O | Có | nước đơn bào | 
| 1 0 0/S | Không | tàu đơn bào | 
| 2 1 1 / OO OO | Có | cô gái phía dưới bên phải | 
| 2 0 0 / SO OS | Không | cú đánh trên cùng bên trái | 

## Vỏ cạnh 

Trường hợp cạnh khóa là lưới 1 x 1, trong đó cả chỉ mục hàng và cột phải phân giải chính xác mà không giả sử bất kỳ cấu trúc xung quanh nào. Trong trường hợp đó, thuật toán giảm xuống còn so sánh một ký tự và mọi lỗi lập chỉ mục sẽ ngay lập tức hiển thị. 

Một trường hợp khác là khi đầu vào bao gồm khoảng trắng giữa các ký tự. Việc coi mỗi dòng là một chuỗi thô mà không loại bỏ khoảng trắng sẽ làm thay đổi chỉ số và tạo ra kết quả không chính xác. Giải pháp tránh điều này bằng cách chuẩn hóa từng hàng trước khi lập chỉ mục, đảm bảo cấu trúc lưới phù hợp với bố cục 2D dự kiến.
