---
title: "CF 104069G - Đại Hội"
description: "Chúng ta được cung cấp một tuyến tàu điện ngầm tuyến tính với các ga được sắp xếp theo thứ tự cố định từ tây sang đông. Mỗi trạm được kết nối với trạm tiếp theo và việc di chuyển giữa các trạm liền kề luôn mất đúng một phút."
date: "2026-07-02T03:00:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "G"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 43
verified: true
draft: false
---

[CF 104069G - Cuộc họp lớn](https://codeforces.com/problemset/problem/104069/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến tàu điện ngầm tuyến tính với các ga được sắp xếp theo thứ tự cố định từ tây sang đông. Mỗi trạm được kết nối với trạm tiếp theo và việc di chuyển giữa các trạm liền kề luôn mất đúng một phút. Hai người xuất phát cùng lúc ở hai ga khác nhau và cùng di chuyển dọc theo đường này. Nhiệm vụ là xác định thời gian tối thiểu cho đến khi họ có thể ở cùng một trạm vào cùng một thời điểm. 

Đầu vào đưa ra số lượng trạm, theo sau là danh sách tên các trạm dọc theo tuyến. Sau đó, chúng ta được đặt hai tên trạm chỉ vị trí xuất phát của hai người. Đầu ra là một số nguyên duy nhất: thời điểm sớm nhất mà chúng có thể chiếm giữ cùng một trạm nếu cả hai bắt đầu di chuyển ngay lập tức và di chuyển dọc theo đường một cách tối ưu. 

Cấu trúc chính là biểu đồ không phải là biểu đồ chung mà là một đường dẫn đơn giản. Mỗi trạm có tối đa hai trạm lân cận và chuyển động có tính xác định về khoảng cách: thời gian giữa hai trạm bất kỳ chỉ là chênh lệch chỉ số của chúng trong danh sách. 

Các ràng buộc là nhỏ, với tối đa 100 trạm và tên trạm có độ dài lên tới 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả một cách tiếp cận bậc hai hoặc đơn giản đối với tất cả các cặp trạm cũng sẽ không đáng kể để chạy trong giới hạn. Thử thách thực sự không phải là hiệu suất mà là chuyển chính xác điều kiện đáp ứng thành biểu thức trong thời gian ngắn nhất. 

Một điểm tinh tế là hai người không cần phải gặp nhau bằng cách bước về phía nhau một cách rõ ràng. Họ có thể di chuyển tùy ý dọc theo tuyến và thời gian họp là tối thiểu so với tất cả các trạm họp có thể. Một cách giải thích ngây thơ có thể cho rằng họ phải gặp nhau giữa chừng hoặc chỉ xem xét một hướng, điều này sẽ bỏ lỡ điểm gặp nhau tối ưu. 

Các trường hợp đặc biệt cần lưu ý bao gồm khi cả hai đều xuất phát ở cùng một trạm, trong đó câu trả lời là 0 và khi điểm gặp mặt tối ưu là một trong những vị trí xuất phát, nghĩa là một người đợi tại chỗ trong khi người kia đến. 

## Phương pháp tiếp cận 

Cách mạnh mẽ nhất để suy nghĩ về vấn đề này là coi mỗi trạm là một điểm gặp gỡ tiềm năng. Đối với mỗi trạm, hãy tính thời gian để cả hai người đến được trạm đó, đây chính là sự khác biệt tuyệt đối giữa các chỉ số trong danh sách trạm. Thời gian gặp nhau của trạm đó là thời gian tối đa của hai lần đến vì cả hai phải có mặt ở đó đồng thời. Câu trả lời là giá trị tối thiểu này trên tất cả các trạm. 

Điều này có hiệu quả vì mọi cuộc họp hợp lệ đều phải diễn ra tại một trạm nào đó và thời gian được xác định hoàn toàn bởi khoảng cách trên một tuyến. Vấn đề là nếu chúng tôi triển khai điều này mà không có cấu trúc, chúng tôi sẽ liên tục quét hoặc tính toán lại khoảng cách một cách không cần thiết, nhưng ngay cả điều đó cũng ổn vì n chỉ là 100. 

Sự đơn giản hóa chính là nhận ra rằng tên trạm có thể được ánh xạ trực tiếp tới các chỉ mục. Khi chúng ta biết chỉ số của hai trạm bắt đầu, bài toán sẽ giảm xuống mức tối thiểu hóa max(|i - k|, |j - k|) trên tất cả k. Trên một dòng, biểu thức này được giảm thiểu khi k nằm giữa i và j và giá trị tối ưu sẽ bằng chính xác một nửa khoảng cách giữa chúng, được làm tròn lên. 

Điều này đưa ra một công thức trực tiếp: câu trả lời là (|i - j| + 1) // 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các trạm họp | O(n) | O(1) | Đã chấp nhận | 
| Ánh xạ chỉ mục + công thức trực tiếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số lượng trạm và lưu danh sách theo thứ tự. Thứ tự rất quan trọng vì nó xác định cấu trúc số liệu của biểu đồ. 
2. Xây dựng ánh xạ từ tên trạm đến chỉ mục của trạm đó trong danh sách. Điều này cho phép tra cứu các vị trí theo thời gian liên tục. 
3. Truy xuất chỉ số của hai trạm xuất phát. Đặt chúng là i và j. 
4. Tính khoảng cách tuyệt đối giữa chúng trên đường thẳng. Khoảng cách này thể hiện thời gian ngắn nhất để một người đến được người kia nếu họ di chuyển trực tiếp. 
5. Làm tròn lại một nửa khoảng cách này, vì cả hai có thể di chuyển về phía nhau cùng một lúc và mỗi phút sẽ giảm khoảng cách của họ tối đa hai bước. 

Tại sao nó hoạt động: trên một đường đi, bất kỳ điểm gặp k nào đều tạo ra thời gian đến |i - k| và |j - k|. Thời gian gặp nhau là lớn nhất của hai giá trị này. Hàm này được giảm thiểu khi k nằm giữa i và j, vì việc di chuyển k về phía khoảng cách sẽ làm giảm khoảng cách lớn hơn trong hai khoảng cách. Khi k nằm trong đoạn thẳng, sự cân bằng tốt nhất đạt được khi cả hai bên co lại đối xứng, dẫn đến chia đôi tổng khoảng cách giữa i và j. Vì chuyển động rời rạc tính theo số phút nên kết quả là giới hạn của một nửa khoảng cách. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())
stations = []
pos = {}

for i in range(n):
    name = input().strip()
    stations.append(name)
    pos[name] = i

c, m = input().split()
i = pos[c]
j = pos[m]

dist = abs(i - j)
print((dist + 1) // 2)
```Việc triển khai hoàn toàn dựa vào việc chuyển đổi tên trạm thành các chỉ mục, giúp tránh việc quét lặp lại. Từ điển đảm bảo quyền truy cập O(1) vào các vị trí. Biểu thức cuối cùng`(dist + 1) // 2`là một cách tính toán nhỏ gọn khoảng cách trần bằng một nửa, phù hợp với thời gian họp tối ưu trên một đường dây. 

Một lỗi phổ biến là cố gắng mô phỏng chuyển động từng bước, nhưng điều đó là không cần thiết vì cấu trúc đảm bảo một giải pháp dạng đóng. Một vấn đề tiềm ẩn khác là quên rằng cả hai đều chuyển động đồng thời, đó là lý do tại sao khoảng cách giảm xuống còn hai mỗi phút chứ không phải một. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
butanta
pinheiros
faria
fradique
oscar
paulista
luz
pinheiros oscar
```Các chỉ số là: 

pinheiros = 1, oscar = 4 

| Bước | tôi | j | quận | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 4 | 3 | - | 
| tính toán | 1 | 4 | 3 | 2 | 

Thời gian gặp nhau là 2 vì chúng có thể di chuyển về phía nhau, giảm khoảng cách từ 3 xuống 0 theo hai bước đồng bộ. 

### Ví dụ 2 

đầu vào:```
5
a
b
c
d
e
a e
```Chỉ số: 

a = 0, e = 4 

| Bước | tôi | j | quận | trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 4 | 4 | - | 
| tính toán | 0 | 4 | 4 | 2 | 

Điều này cho thấy một trường hợp đối xứng trong đó cả hai điểm cuối đều di chuyển vào trong và gặp nhau ở giữa sau 2 phút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | xây dựng bản đồ vị trí yêu cầu quét tất cả các trạm một lần | 
| Không gian | O(n) | cửa hàng từ điển một mục mỗi trạm | 

Các ràng buộc cho phép tối đa 100 trạm, vì vậy ngay cả khi chúng tôi sử dụng cách tiếp cận ít trực tiếp hơn, hiệu suất sẽ không thành vấn đề. Giải pháp chạy thoải mái trong giới hạn do tiền xử lý tuyến tính và truy vấn thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input().strip())
    stations = []
    pos = {}

    for i in range(n):
        name = input().strip()
        stations.append(name)
        pos[name] = i

    c, m = input().split()
    i = pos[c]
    j = pos[m]

    dist = abs(i - j)
    return str((dist + 1) // 2)

# provided sample
assert run("""7
butanta
pinheiros
faria
fradique
oscar
paulista
luz
pinheiros oscar
""") == "2"

# same start
assert run("""3
a
b
c
a a
""") == "0"

# adjacent
assert run("""3
a
b
c
a b
""") == "1"

# symmetric far ends
assert run("""5
a
b
c
d
e
a e
""") == "2"

# middle meeting
assert run("""5
a
b
c
d
e
b d
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | 0 | trường hợp bắt đầu giống hệt nhau | 
| a b | 1 | trạm lân cận | 
| một e | 2 | điểm cuối đối xứng | 
| bd | 1 | họp bên trong phân khúc | 

## Vỏ cạnh 

Khi cả hai người xuất phát ở cùng một trạm, chênh lệch chỉ số bằng 0, do đó khoảng cách được tính toán bằng 0 và công thức trả về 0. Thuật toán xử lý chính xác điều này mà không cần sử dụng cách viết hoa đặc biệt. 

Đối với các trạm liền kề, chẳng hạn như chỉ số 2 và 3, khoảng cách là một, do đó`(1 + 1) // 2 = 1`. Điều này phù hợp với thực tế là chúng sẽ gặp nhau sau một phút nếu chúng di chuyển về phía nhau. 

Đối với các điểm cuối cực đoan, chẳng hạn như chỉ số 0 và n-1, cuộc họp diễn ra ở khu vực trung tâm. Công thức tự động nắm bắt điều này mà không cần phải tìm kiếm điểm giữa một cách rõ ràng, vì nó bắt nguồn từ việc giảm thiểu tối đa hai khoảng cách tuyến tính trên một đường dẫn.
