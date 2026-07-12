---
title: "CF 103241F - Sách"
description: "Chúng tôi được đưa cho một danh sách các cuốn sách được đặt trên kệ. Mỗi cuốn sách có tiêu đề viết thường một từ và năm xuất bản. Nhiệm vụ là sắp xếp lại giá sách sao cho các sách chủ yếu được nhóm theo chữ cái đầu tiên của tên sách theo thứ tự bảng chữ cái."
date: "2026-07-03T15:07:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103241
codeforces_index: "F"
codeforces_contest_name: "Teamscode Summer 2021"
rating: 0
weight: 103241
solve_time_s: 42
verified: true
draft: false
---

[CF 103241F - Sách](https://codeforces.com/problemset/problem/103241/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa cho một danh sách các cuốn sách được đặt trên kệ. Mỗi cuốn sách có tiêu đề viết thường một từ và năm xuất bản. Nhiệm vụ là sắp xếp lại giá sách sao cho các sách chủ yếu được nhóm theo chữ cái đầu tiên của tên sách theo thứ tự bảng chữ cái. Trong mỗi nhóm như vậy, sách phải được sắp xếp theo năm xuất bản tăng dần, từ cũ đến mới. 

Đầu ra không phải là thứ tự dưới dạng chỉ mục hoặc cặp, mà chỉ đơn giản là chuỗi tên sách theo thứ tự được sắp xếp cuối cùng, mỗi tên một dòng. 

Kích thước đầu vào lên tới khoảng mười nghìn cuốn sách. Điều này ngay lập tức cho chúng ta biết rằng một$O(n^2)$giải pháp quét hoặc chèn nhiều lần vào danh sách sẽ quá chậm trong trường hợp xấu nhất. MỘT$O(n \log n)$Cách tiếp cận này là đủ và được mong đợi, vì việc sắp xếp chiếm ưu thế và chúng ta chỉ cần xác định khóa thứ tự chính xác. 

Điểm tinh tế chính là thứ tự từ điển theo nghĩa tùy chỉnh: nó không phải là sắp xếp chuỗi đầy đủ, chỉ có ký tự đầu tiên quan trọng đối với việc nhóm và trong nhóm đó, năm là quan trọng. 

Một sai lầm ngây thơ nảy sinh khi mọi người chỉ sắp xếp theo năm hoặc chỉ theo tên. 

Ví dụ, hãy xem xét: 

đầu vào:```
3
banana 2000
apple 1990
apricot 1980
```Đầu ra đúng:```
apricot
apple
banana
```Một cách tiếp cận ngây thơ chỉ sắp xếp theo năm sẽ tạo ra:```
apricot
apple
banana
```Điều này vô tình hoạt động ở đây nhưng không thành công trong trường hợp các chữ cái khác nhau. 

Bây giờ hãy xem xét:```
3
banana 1980
apple 2000
apricot 1990
```Đầu ra đúng:```
apple
apricot
banana
```Nếu chúng ta bỏ qua chữ cái đầu tiên, việc sắp xếp theo năm sẽ cho:```
banana
apricot
apple
```điều này không chính xác vì việc nhóm theo chữ cái đầu tiên là bắt buộc. 

Một vấn đề tế nhị khác là sự ổn định khi các năm bằng nhau. Nếu hai cuốn sách có cùng chữ cái đầu tiên và cùng năm, thì thứ tự tương đối của chúng không được xác định rõ ràng trong câu lệnh, do đó, bất kỳ sự ràng buộc nhất quán nào (ví dụ: từ điển theo tên) đều có thể chấp nhận được, nhưng chúng tôi phải đảm bảo tính tất định. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng việc sắp xếp thủ công bằng cách liên tục quét danh sách và chọn cuốn sách hợp lệ nhỏ nhất tiếp theo theo quy tắc: ưu tiên chữ cái đầu tiên, sau đó là năm. Điều này giống như sắp xếp lựa chọn trên một bộ so sánh tùy chỉnh. Điều này đúng vì ở mỗi bước chúng ta chọn rõ ràng phần tử hợp lệ tiếp theo theo thứ tự, nhưng mỗi lần lựa chọn đều phải trả phí$O(n)$, và làm điều này cho tất cả$n$vị trí dẫn đến$O(n^2)$hoạt động. Với$n = 10^4$, điều này trở nên xung quanh$10^8$so sánh, nằm ở ranh giới hoặc quá chậm trong Python dưới các ràng buộc thông thường. 

Quan sát quan trọng là mối quan hệ thứ tự là một thứ tự tổng thể nghiêm ngặt được xác định bởi một bộ nhỏ: ký tự đầu tiên của chuỗi, sau đó là năm và cuối cùng là tiêu đề đầy đủ tùy chọn cho tie-break. Khi chúng tôi nhận ra điều này, vấn đề sẽ giảm xuống thành một nhiệm vụ sắp xếp tiêu chuẩn. Các thuật toán sắp xếp xử lý nội bộ việc sắp xếp một cách hiệu quả trong$O(n \log n)$và Timsort của Python được tối ưu hóa cho các khóa có cấu trúc như vậy. 

Vì vậy, thay vì mô phỏng các quyết định, chúng tôi mã hóa quy tắc dưới dạng khóa sắp xếp và ủy quyền sắp xếp thứ tự cho thư viện tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lựa chọn vũ phu |$O(n^2)$|$O(1)$thêm | Quá chậm | 
| Sắp xếp bằng phím |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Đọc tất cả sách vào bộ nhớ 

Mỗi mục được lưu trữ dưới dạng một cặp bao gồm tiêu đề và năm xuất bản của nó. Điều này cho phép chúng tôi tính toán các khóa sắp xếp mà không cần truy cập đầu vào nhiều lần. 

### 2. Xác định khóa sắp xếp nắm bắt quy tắc sắp xếp 

Với mỗi cuốn sách, chúng ta xây dựng một bộ dữ liệu`(first_letter, year, title)`. Chữ cái đầu tiên thực thi việc phân nhóm, năm thực thi thứ tự thời gian trong mỗi nhóm và tiêu đề đóng vai trò là yếu tố quyết định. 

### 3. Sắp xếp danh sách bằng phím 

Chúng tôi áp dụng cách sắp xếp tiêu chuẩn cho tất cả các sách bằng cách sử dụng bộ dữ liệu đã xác định. Thuật toán sắp xếp sẽ so sánh các bộ dữ liệu theo từ điển, khớp chính xác với thứ tự được yêu cầu. 

### 4. Chỉ xuất ra các tiêu đề theo thứ tự sắp xếp 

Sau khi sắp xếp, chúng tôi in từng tiêu đề theo thứ tự, bỏ qua năm vì nó chỉ dùng để đặt hàng. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là thứ tự mong muốn được mô tả đầy đủ bằng cách so sánh từ điển trên`(first_letter, year, title)`. Điều này xác định một thứ tự tổng thể, nghĩa là mỗi cặp sách đều có thể so sánh được và giữ được tính nhất quán bắc cầu. Các thuật toán sắp xếp được đảm bảo tạo ra một trình tự phù hợp với bộ so sánh này, do đó danh sách kết quả chính xác là cách sắp xếp kệ cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    books = []
    
    for _ in range(n):
        name, year = input().split()
        year = int(year)
        books.append((name, year))
    
    books.sort(key=lambda x: (x[0][0], x[1], x[0]))
    
    for name, _ in books:
        print(name)

if __name__ == "__main__":
    main()
```Chi tiết triển khai quan trọng là khóa sắp xếp. Chúng tôi sử dụng rõ ràng`x[0][0]`để trích xuất ký tự đầu tiên của tiêu đề, đảm bảo nhóm theo chữ cái đầu. Thành phần thứ hai là năm, thực thi thứ tự thời gian tăng dần. Thành phần cuối cùng`x[0]`ổn định mối quan hệ khi cả hai trường trước đó đều bằng nhau. 

Một lỗi phổ biến là quên hoàn toàn việc nhóm các chữ cái đầu tiên hoặc sắp xếp theo chuỗi đầy đủ trước, điều này làm thay đổi cấu trúc dự định của thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
harry 1987
orange 1776
hotels 1973
moon 2018
```Các bước sắp xếp: 

| Sách | Bức Thư Đầu Tiên | Năm | 
| --- | --- | --- | 
| khách sạn | h | 1973 | 
| Harry | h | 1987 | 
| mặt trăng | m | 2018 | 
| cam | o | 1776 | 

Đầu ra:```
hotels
harry
moon
orange
```Điều này thể hiện sự phân nhóm: tất cả`h`sách đến trước`m`, và trong`h`, năm quyết định thứ tự. 

### Ví dụ 2 

đầu vào:```
5
alpha 2000
apple 1990
banana 1980
apex 1990
aardvark 1990
```Các bước sắp xếp: 

| Sách | Bức Thư Đầu Tiên | Năm | 
| --- | --- | --- | 
| lợn đất | một | 1990 | 
| táo | một | 1990 | 
| đỉnh | một | 1990 | 
| chuối | b | 1980 | 
| alpha | một | 2000 | 

Đầu ra:```
aardvark
apple
apex
alpha
banana
```Trường hợp này cho thấy rằng khi nhiều cuốn sách có chung cả chữ cái đầu tiên và năm, thì tiêu đề ràng buộc (tên đầy đủ) đảm bảo thứ tự xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp$n$sách sử dụng phím từ điển | 
| Không gian |$O(n)$| Lưu trữ danh sách sách và phân loại chi phí | 

Các ràng buộc cho phép lên tới 10.000 cuốn sách và$n \log n$các hoạt động diễn ra thoải mái trong giới hạn trong Python, giúp giải pháp này đủ hiệu quả để giải quyết vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    books = []
    for _ in range(n):
        name, year = input().split()
        books.append((name, int(year)))

    books.sort(key=lambda x: (x[0][0], x[1], x[0]))

    return "\n".join(name for name, _ in books)

# sample
assert run("""4
harry 1987
orange 1776
hotels 1973
moon 2018
""") == "hotels\nharry\nmoon\norange"

# minimum size
assert run("""1
alone 2020
""") == "alone"

# same first letter different years
assert run("""3
apple 2000
apex 1990
aardvark 2010
""") == "apex\napple\naardvark"

# same first letter and year
assert run("""3
bbb 2000
bba 2000
bbc 2000
""") == "bba\nbbb\nbbc"

# reverse order input
assert run("""3
c 3
b 2
a 1
""") == "a\nb\nc"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cuốn sách duy nhất | chính nó | trường hợp cạnh tối thiểu | 
| cùng một tiền tố khác nhau năm | sắp xếp theo năm | quy tắc cốt lõi | 
| tiền tố và năm giống hệt nhau | quyết định ràng buộc | ổn định | 
| thứ tự ngược lại | phân loại đầy đủ đúng đắn | trường hợp chung | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi nhiều cuốn sách có cùng chữ cái bắt đầu và năm xuất bản giống nhau. Nếu không có bộ ngắt kết nối thứ ba, các lần chạy sắp xếp khác nhau có thể tạo ra các hoán vị hợp lệ khác nhau, điều này không mong muốn trong bài toán đầu ra xác định. 

Ví dụ:```
3
booka 1999
bookb 1999
bookc 1999
```Thuật toán sử dụng tiêu đề đầy đủ làm khóa cuối cùng, do đó quá trình thực thi diễn ra theo thứ tự từ điển giữa chúng, đảm bảo đầu ra nhất quán:```
booka
bookb
bookc
```Một trường hợp khó khăn khác là khi tất cả các cuốn sách đều bắt đầu bằng các chữ cái khác nhau. Quy tắc chữ cái đầu tiên hoàn toàn xác định thứ tự và năm không có hiệu lực. Thuật toán vẫn hoạt động vì so sánh bộ dữ liệu ưu tiên ký tự đầu tiên một cách tự nhiên. 

Cuối cùng, khi tất cả các sách có cùng chữ cái đầu tiên, vấn đề sẽ giảm xuống còn việc sắp xếp theo năm và việc triển khai sẽ tự động chuyển sang hành vi đó mà không có cách viết hoa đặc biệt.
