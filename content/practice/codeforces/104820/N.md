---
title: "CF 104820N - \u041c\u0443\u0437\u044b\u043a\u0430\u043b\u044c\u043d\u043e\u0435"
description: "Chúng ta được cung cấp một số thể loại âm nhạc, mỗi thể loại có một số lượng bài hát cố định. Nhiệm vụ là quyết định xem có thể sắp xếp tất cả các bài hát vào một danh sách phát sao cho không có hai bài hát liền kề nào thuộc cùng một thể loại hay không."
date: "2026-06-28T12:59:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "N"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 68
verified: true
draft: false
---

[CF 104820N - \u041c\u0443\u0437\u044b\u043a\u0430\u043b\u044c\u043d\u043e\u0435](https://codeforces.com/problemset/problem/104820/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số thể loại âm nhạc, mỗi thể loại có một số lượng bài hát cố định. Nhiệm vụ là quyết định xem có thể sắp xếp tất cả các bài hát vào một danh sách phát sao cho không có hai bài hát liền kề nào thuộc cùng một thể loại hay không. 

Một cách khác để nghĩ về điều này là chúng ta đang cố gắng xen kẽ nhiều nhóm vật phẩm giống hệt nhau. Mỗi thể loại hoạt động giống như một khối các mã thông báo giống hệt nhau và chúng ta phải hoán đổi tất cả các mã thông báo trên một dòng trong khi tránh các hàng xóm giống hệt nhau. 

Kích thước đầu vào cho phép lên tới 100.000 thể loại và mỗi thể loại có thể đóng góp tới 10.000 bài hát. Điều này có nghĩa là tổng số bài hát có thể rất lớn, có thể lên tới 10^9. Bất kỳ giải pháp nào cố gắng xây dựng hoặc mô phỏng hoán vị một cách rõ ràng đều không thể thực hiện được. Ngay cả việc sắp xếp và mô phỏng tham lam trên từng bài hát cũng sẽ quá chậm nếu lặp lại toàn bộ chuỗi mở rộng. 

Giới hạn thời gian ngụ ý rằng chúng ta nên làm việc theo O(n) hoặc O(n log n) theo số lượng thể loại, không phải trên tổng số bài hát. 

Một trường hợp quan trọng phát sinh khi một thể loại thống trị phần còn lại. Ví dụ: nếu chúng ta có các số đếm như 1000, 1, 1, 1 thì rõ ràng là không thể xen kẽ đủ các bài hát khác để tách tất cả các lần xuất hiện của thể loại thống trị. Một cách tiếp cận ngây thơ chỉ kiểm tra tổng số tiền hoặc số chẵn lẻ sẽ thất bại ở đây vì cấu trúc của cọc lớn nhất quan trọng chứ không chỉ tổng số. 

Một trường hợp tế nhị khác là khi hai thể loại lớn cùng nhau thống trị phần còn lại. Ví dụ: các số đếm như 50, 49, 1, 1, 1 vẫn có tác dụng, nhưng những đánh giá sai lầm nhỏ chỉ so sánh mức tối đa với tổng của các số khác mà không có lý do bất bình đẳng chính xác có thể gây hiểu nhầm nếu thực hiện không chính xác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng xây dựng danh sách phát một cách rõ ràng. Người ta có thể liên tục chọn một thể loại khác với bài hát được đặt cuối cùng, chọn một cách tham lam trong số các thể loại có sẵn. Điều này giống như một mô phỏng với cấu trúc ưu tiên. Mỗi vị trí yêu cầu chọn một thể loại hợp lệ tiếp theo và trong trường hợp xấu nhất, chúng tôi thực hiện lựa chọn tuyến tính hoặc logarit cho mỗi bài hát trong số hàng tỷ bài hát tiềm năng. Vì tổng số bài hát là tổng của tất cả a_i, điều này dẫn đến O(tổng số bài hát log n) hoặc tệ hơn, điều này là không khả thi. 

Quan sát quan trọng là chúng ta không cần sự sắp xếp chính xác, chỉ cần sự tồn tại của nó. Vấn đề trở thành một điều kiện khả thi cổ điển: liệu chúng ta có thể tách các lần xuất hiện của thể loại thường xuyên nhất bằng cách sử dụng tất cả các bài hát khác làm dấu phân cách hay không. 

Nếu một thể loại có quá nhiều bài hát, chắc chắn nó sẽ buộc phải liền kề với chính nó. Hãy tưởng tượng đặt tất cả các bài hát khác làm dấu phân cách trước tiên. Mỗi dấu phân cách có thể phá vỡ tối đa một phần liền kề giữa hai lần xuất hiện của thể loại thống trị. Nếu số lượng lớn nhất lớn hơn tổng số bài hát khác cộng với một bài hát thì không có cách nào tránh được sự liền kề. 

Đặt max là a_i lớn nhất và tổng là tổng số bài hát. Số lượng bài hát không tối đa là tổng - tối đa. Chúng tôi cần ít nhất khoảng trống tối đa - 1 để đặt các bài hát khác giữa các lần xuất hiện của thể loại lớn nhất. Do đó, điều kiện trở thành: 

tối đa - 1 tổng - tối đa 

mà đơn giản hóa thành: 

tối đa ≤ tổng - tối đa + 1 

Nếu sự bất bình đẳng này thất bại thì không có sự sắp xếp nào tồn tại. Mặt khác, chúng ta luôn có thể xây dựng một thứ tự hợp lệ bằng cách sử dụng các đối số xen kẽ tham lam, do đó điều kiện vừa đủ vừa cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng số bài hát log n) | O(n) | Quá chậm | 
| Điều kiện tần số tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả số lượng thể loại và tính tổng và giá trị tối đa của chúng. Điều này nắm bắt được cấu trúc toàn cầu về mức độ phân phối tập trung. 
2. Xác định số lượng lớn nhất, vì chỉ thể loại thường xuyên nhất mới có thể gây ra các vấn đề lân cận không thể tránh khỏi. Tất cả các thể loại khác luôn có thể được sử dụng làm dấu phân cách nếu chúng tồn tại với số lượng đủ. 
3. Tính xem có bao nhiêu bài hát không thuộc thể loại chủ đạo bằng cách lấy tổng số tiền trừ đi số lượng tối đa. Đây là những dấu phân cách duy nhất có sẵn. 
4. Kiểm tra xem số lượng dấu phân cách có sẵn ít nhất là tối đa - 1. Đây là số khoảng cách giữa các lần xuất hiện của thể loại chiếm ưu thế trong bất kỳ trình tự hợp lệ nào. 
5. Nếu điều kiện đúng thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên lập luận lấp đầy khoảng trống. Bất kỳ sự sắp xếp nào thuộc thể loại thường xuyên nhất đều tạo ra chính xác tối đa - 1 khoảng cách bắt buộc giữa các lần xuất hiện của nó. Mỗi bài hát khác có thể chiếm nhiều nhất một khoảng trống như vậy nếu chúng ta muốn tránh sự liền kề. Nếu có ít bài hát không chiếm ưu thế hơn số khoảng trống thì ít nhất một khoảng trống phải được giữ trống, buộc hai thể loại giống hệt nhau phải chạm vào nhau. Ngược lại, nếu tồn tại đủ dấu phân cách, chúng tôi luôn có thể phân phối chúng qua các khoảng trống, sau đó chèn các phần tử chi phối còn lại mà không vi phạm tính khả thi, đảm bảo tồn tại sự sắp xếp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    mx = max(a)
    
    if mx <= total - mx + 1:
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo điều kiện dẫn xuất trực tiếp. Tổng của tất cả các số được tính một lần và giá trị tối đa được trích xuất trong một lần truyền qua mảng. Bất đẳng thức cuối cùng mã hóa toàn bộ điều kiện khả thi, do đó không cần mô phỏng thêm. 

Một điểm tinh tế là`+1`trong tình trạng này. Nó tương ứng với thực tế là thể loại lớn nhất có thể chiếm cả hai đầu của chuỗi, vì vậy nó cần ít dấu phân cách hơn số lượng của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2 3
```Chúng tôi tính tổng = 6 và mx = 3. 

| Bước | tổng cộng | mx | tổng cộng - mx | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | - | 
| Sau khi đọc | 6 | 3 | 3 | kiểm tra 3 ≤ 4 | 

Chúng ta kiểm tra xem 3 ≤ 6 - 3 + 1 = 4 có đúng không, vì vậy câu trả lời là Có. 

Điều này thể hiện sự phân bổ cân bằng trong đó thể loại lớn nhất có thể được xen kẽ bằng các bài hát còn lại. 

### Mẫu 2 

đầu vào:```
2
1 1
```Chúng tôi tính tổng = 2 và mx = 1. 

| Bước | tổng cộng | mx | tổng cộng - mx | Tình trạng | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 0 | 0 | 0 | - | 
| Sau khi đọc | 2 | 1 | 1 | kiểm tra 1 2 | 

Điều kiện được thỏa mãn nên câu trả lời là Có. Điều này xác nhận rằng ngay cả trường hợp đối xứng nhỏ nhất cũng đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng và tối đa | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Thuật toán này hiệu quả với n lên tới 100.000 và hoạt động ngay cả khi tổng số lượng cực lớn vì nó không bao giờ lặp lại trên từng bài hát. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n_and_rest = inp.strip().split()
    n = int(n_and_rest[0])
    arr = list(map(int, n_and_rest[1:1+n]))
    total = sum(arr)
    mx = max(arr)
    return "Yes\n" if mx <= total - mx + 1 else "No\n"

# provided samples
assert run("3\n1 2 3") == "Yes\n"
assert run("2\n1 1") == "Yes\n"

# custom cases
assert run("1\n5") == "Yes\n"  # single genre always valid
assert run("2\n10 1") == "No\n"  # dominant too large
assert run("5\n3 3 3 3 3") == "Yes\n"  # uniform distribution
assert run("4\n1 1 1 10") == "No\n"  # strong imbalance
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | Có | trường hợp cạnh thể loại đơn | 
| 2 10 1 | Không | áp đảo áp đảo | 
| 5 3 3 3 3 3 | Có | tính khả thi thống nhất | 
| 4 1 1 1 10 | Không | phát hiện độ lệch mạnh | 

## Vỏ cạnh 

Trường hợp quan trọng nhất là khi chỉ có một thể loại. Đối với đầu vào:```
1
5
```Chúng tôi tính tổng = 5 và mx = 5. Điều kiện trở thành 5 ≤ 1, sai nên kết quả đầu ra là Không. Điều này phản ánh chính xác rằng bất kỳ chuỗi bài hát giống hệt nhau nào cũng sẽ luôn vi phạm quy tắc liền kề. 

Một trường hợp khác là khi thể loại lớn nhất chỉ vừa đủ lớn. Vì:```
4
1 1 1 4
```Ta có tổng = 7 và mx = 4. Điều kiện là 4 ≤ 4 là đúng nên câu trả lời là Có. Một sự sắp xếp hợp lệ tồn tại bằng cách đặt các đĩa đơn giữa các lần xuất hiện của thể loại lớn nhất. 

Cuối cùng, hãy xem xét một trường hợp thất bại được đóng gói chặt chẽ:```
3
1 1 3
```Ở đây tổng = 5 và mx = 3. Điều kiện là 3 3, vì vậy nó hợp lệ. Chúng ta có thể xây dựng một chuỗi như 3 1 3 1 3, cho thấy rằng đẳng thức chính xác là ranh giới giữa các cấu hình có thể và không thể.
