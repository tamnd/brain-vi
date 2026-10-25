---
title: "CF 106049F - Trò chơi RBS"
description: "Trò chơi này xây dựng một chuỗi khung theo khối. Alice kiểm soát mọi lượt lẻ, kể cả lượt cuối cùng và mỗi lượt của cô ấy thêm chính xác một dấu ngoặc. Bob điều khiển từng lượt chẵn và thêm chính xác b dấu ngoặc."
date: "2026-06-25T12:34:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106049
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #44 (DIV3.5-Forces)"
rating: 0
weight: 106049
solve_time_s: 44
verified: true
draft: false
---

[CF 106049F - Trò chơi RBS](https://codeforces.com/problemset/problem/106049/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trò chơi này xây dựng một chuỗi khung theo khối. Alice kiểm soát mọi lượt lẻ, kể cả lượt cuối cùng và mỗi lượt của cô ấy cộng chính xác`a`dấu ngoặc. Bob điều khiển từng lượt chẵn và cộng chính xác`b`dấu ngoặc. Sau tất cả các lượt, Alice chỉ thắng nếu toàn bộ chuỗi là một chuỗi dấu ngoặc thông thường, nghĩa là mỗi tiền tố có ít nhất số dấu ngoặc mở bằng số dấu ngoặc đóng và số dư cuối cùng bằng 0. 

Phiên bản bị hack chỉ yêu cầu chúng tôi quyết định xem Alice nên chọn người chơi nào để chơi. Chúng ta cần xác định xem Alice có chiến lược đảm bảo chuỗi ngoặc thông thường hay Bob luôn có thể ngăn chặn điều đó. Các hạn chế là nhỏ nhưng giải pháp dự định không phải là mô phỏng. Một chiến lược phải hoạt động chống lại mọi động thái có thể xảy ra từ đối thủ, vì vậy việc kiểm tra các chuỗi dấu ngoặc có thể xảy ra sẽ theo cấp số nhân. Số lượng chuỗi có thể có trong một lần di chuyển đã lớn vì một khối dấu ngoặc có thể có nhiều cách sắp xếp khác nhau. 

Khó khăn chính là chuỗi dấu ngoặc thông thường có hai yêu cầu cùng một lúc. Số cuối cùng của dấu ngoặc mở và đóng phải khớp nhau và mọi tiền tố phải hợp lệ. Cấu trúc chỉ sửa số dư cuối cùng vẫn có thể thất bại nếu tiền tố sớm trở thành số âm. 

Có hai trường hợp quan trọng phá vỡ các giải pháp ngây thơ. Coi như:```
1
3 2 4
```Nếu Alice cố gắng chơi, cô ấy có thể bắt đầu với tối đa hai dấu ngoặc mở nhiều hơn dấu ngoặc đóng. Bob có thể trả lời bằng bốn dấu ngoặc đóng. Ngay cả khi Alice bắt đầu với`((', Bob can append `))))`, làm cho tiền tố không hợp lệ. Đầu ra là:```
0
```Cách tiếp cận bất cẩn chỉ kiểm tra tổng số dấu ngoặc có thể bỏ sót điều này vì Alice vẫn có nước đi cuối cùng. 

Một trường hợp khác là:```
1
3 4 2
```Ở đây Alice có thể bắt đầu với bốn dấu ngoặc mở, Bob không thể loại bỏ tất cả giới hạn an toàn đó trong một nước đi và Alice có thể sửa lại sự cân bằng ở lượt cuối cùng của mình. Đầu ra là:```
1
```Một cách tiếp cận bỏ qua thứ tự các lượt và chỉ so sánh tổng số lượng có thể đưa ra quyết định sai lầm. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất là tưởng tượng mọi lựa chọn có thể có của người chơi và mọi khối khung có thể có mà họ có thể đặt. Đối với mỗi trò chơi hoàn chỉnh, chúng ta có thể kiểm tra xem chuỗi kết quả có đều đặn hay không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của trò chơi, nhưng nó hoàn toàn không thực tế. Một nước đi có độ dài 50 có số lượng chuỗi ngoặc có thể rất lớn và trò chơi có tới 49 lượt, do đó số lượng trò chơi có thể tăng lên theo cấp số nhân. 

Quan sát hữu ích đến từ việc xem xét sự cân bằng thay vì trình tự chính xác. Số dư là số dấu ngoặc mở trừ đi dấu ngoặc đóng. Một chuỗi thông thường không bao giờ được để giá trị này trở thành âm. 

Nếu Alice chọn chơi, cô ấy luôn có thể bắt đầu chỉ với việc mở dấu ngoặc. Sau đó, cô ấy muốn giữ đủ số dư dương để Bob không thể làm cho chuỗi không hợp lệ. Vì Alice di chuyển sau mỗi bước di chuyển của Bob và cũng thực hiện bước đi cuối cùng, cô ấy có thể khôi phục lại sự cân bằng sau cuộc tấn công của Bob bất cứ khi nào kích thước khối của cô ấy ít nhất bằng kích thước khối của Bob. 

Giả định`a >= b`. Sau mỗi lần Bob di chuyển, Bob có thể giảm số dư hiện tại tối đa`b`. Alice sau đó có thể thêm`a`mở ngoặc hoặc đóng ngoặc theo cách khôi phục trạng thái an toàn. Cô ấy có thể duy trì bất biến rằng sau mỗi lần Alice di chuyển, số dư sẽ chính xác bằng`a`. Nước đi cuối cùng của Alice có thể đóng mọi thứ và kết thúc với số dư bằng 0. 

Nếu như`a < b`, Alice không thể đảm bảo an toàn. Bất kể cô ấy thực hiện bước đi đầu tiên như thế nào, Bob có thể chọn một khối chỉ có dấu ngoặc đóng. Vì khối của Bob lớn hơn nên anh ấy có thể giảm số dư xuống dưới 0 ngay lập tức. Khi tiền tố trở nên không hợp lệ, chuỗi cuối cùng không thể đều đặn. 

Toàn bộ vấn đề giảm xuống khi so sánh hai kích thước khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`a`, Và`b`cho từng trường hợp thử nghiệm. Giá trị của`n`không ảnh hưởng đến quyết định vì sự so sánh giống nhau giữa sức mạnh của Alice và Bob đối với mọi số vòng lẻ. 
2. So sánh`a`Và`b`. Nếu kích thước khối của Alice ít nhất bằng kích thước khối của Bob thì Alice luôn có thể duy trì số dư dương an toàn, do đó đầu ra`1`. 
3. Nếu kích thước khối của Bob lớn hơn, hãy xuất ra`0`bởi vì Bob có thể buộc số dư tiền tố trở nên âm. 

Tại sao nó hoạt động: 

Điều bất biến đằng sau chiến lược của Alice là sau mỗi lượt Alice, chuỗi có thể được giữ ở mức cân bằng dương an toàn. Cuộc tấn công mạnh nhất có thể có của Bob là biến mọi khung trong khối của anh ấy thành khung đóng, điều này làm giảm số dư một cách chính xác.`b`. Khối tiếp theo của Alice chỉ có thể bù đắp cho sự mất mát này nếu nó có kích thước ít nhất`b`. Khi`a >= b`, Alice luôn có thể hồi phục. Khi`a < b`, Bob có thể làm cho tiền tố đầu tiên sau nước đi của anh ta trở thành không hợp lệ, vì vậy Alice không có chiến lược thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        if a >= b:
            ans.append("1")
        else:
            ans.append("0")
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp thử nghiệm và chỉ sử dụng mối quan hệ giữa độ dài hai khối. Số vòng là không cần thiết vì`n`luôn là số lẻ, nghĩa là Alice luôn có nước đi cuối cùng. 

Việc so sánh sử dụng`>=`còn hơn là`>`. Khi`a`Và`b`bằng nhau, Alice có thể khôi phục chính xác sự cân bằng sau nước đi của Bob, vì vậy đẳng thức là trường hợp thắng. 

Không có chỉ số mảng hoặc mô phỏng nào liên quan, điều này tránh được mọi vấn đề riêng lẻ. Các giá trị số học nhỏ nhưng logic tương tự cũng áp dụng được cho các số nguyên lớn hơn. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên: 

đầu vào:```
1
3 2 4
```Dấu vết là: 

| Bước | một | b | Quyết định | 
| --- | --- | --- | --- | 
| Đọc giá trị | 2 | 4 | Nước đi của Alice nhỏ hơn | 
| So sánh | 2 | 4 | Bob có thể phá vỡ tiền tố | 
| Đầu ra | | | 0 | 

Điều này chứng tỏ trường hợp thua cuộc. Khối của Bob lớn hơn nên anh ấy luôn có thể tạo tiền tố âm. 

Đối với ví dụ thứ hai: 

đầu vào:```
1
3 4 2
```Dấu vết là: 

| Bước | một | b | Quyết định | 
| --- | --- | --- | --- | 
| Đọc giá trị | 4 | 2 | Bước đi của Alice lớn hơn | 
| So sánh | 4 | 2 | Alice có thể khôi phục lại sự cân bằng | 
| Đầu ra | | | 1 | 

Điều này chứng tỏ trường hợp chiến thắng. Alice có thể duy trì đủ thăng bằng sau mỗi lượt và về đích với trình tự khung hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một so sánh. | 
| Không gian | O(1) | Chỉ các giá trị hiện tại được lưu trữ. | 

Giải pháp này dễ dàng phù hợp với các giới hạn vì nó tránh tạo ra bất kỳ chuỗi khung nào và thực hiện công việc liên tục cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, a, b = map(int, input().split())
        out.append("1" if a >= b else "0")
    return "\n".join(out)

# samples
assert solution("""2
3 2 2
3 4 2
""") == """1
1""", "sample"

# minimum size
assert solution("""1
3 2 4
""") == "0", "minimum losing case"

# equal moves
assert solution("""1
49 50 50
""") == "1", "equal block sizes"

# large values
assert solution("""1
49 50 2
""") == "1", "alice much stronger"

# boundary where bob is slightly stronger
assert solution("""1
5 48 50
""") == "0", "bob stronger by two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 4`|`0`| Bob có thể phá hủy số dư tiền tố | 
|`49 50 50`|`1`| Bình đẳng vẫn thắng | 
|`49 50 2`|`1`| Alice có thể dễ dàng hồi phục | 
|`5 48 50`|`0`| Sự khác biệt nhỏ vẫn còn quan trọng | 

## Vỏ cạnh 

Đối với trường hợp thua cạnh:```
1
3 2 4
```Nước đi đầu tiên của Alice có thể tạo ra sự cân bằng tối đa`2`bằng cách chỉ đặt dấu ngoặc mở. Bob có thể trả lời bằng bốn dấu ngoặc đóng, giảm số dư đi bốn. Tiền tố trở nên không hợp lệ ngay lập tức. Vì chuỗi ngoặc thông thường không thể chứa số dư tiền tố âm nên Alice không thể phục hồi. 

Đối với trường hợp có kích thước bằng nhau:```
1
3 4 4
```Alice bắt đầu với bốn dấu ngoặc mở. Bob có thể giảm số dư đi bốn, nhưng nước đi cuối cùng của Alice có cùng kích thước và có thể khôi phục lại trình tự một cách chính xác. Bất biến vẫn đúng vì Alice không bao giờ có sức mạnh kém hơn Bob. 

Đối với trường hợp Alice mạnh nhất:```
1
49 50 2
```Mỗi nước đi của Bob chỉ có thể loại bỏ hai đơn vị cân bằng trong khi Alice có thể khôi phục tối đa năm mươi dấu ngoặc. Alice có đủ quyền kiểm soát để giữ cho chuỗi có hiệu lực trong toàn bộ trò chơi.
