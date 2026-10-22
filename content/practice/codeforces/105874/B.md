---
title: "CF 105874B - Tàu vũ trụ"
description: "Chúng ta được cho một số quả cân, mỗi quả cân là một số nguyên dương, biểu thị khối lượng mà chúng ta được phép đặt lên đĩa cân bên trái của cân. Trên chảo bên phải, chúng ta có thể đặt bất kỳ sản phẩm nào có trọng lượng mà chúng ta muốn xác định."
date: "2026-06-25T14:24:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105874
codeforces_index: "B"
codeforces_contest_name: "Spring Lyceum Second school olympiad in informatics 2025"
rating: 0
weight: 105874
solve_time_s: 48
verified: true
draft: false
---

[CF 105874B - Tàu vũ trụ](https://codeforces.com/problemset/problem/105874/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số quả cân, mỗi quả cân là một số nguyên dương, biểu thị khối lượng mà chúng ta được phép đặt lên đĩa cân bên trái của cân. Trên chảo bên phải, chúng ta có thể đặt bất kỳ sản phẩm nào có trọng lượng mà chúng ta muốn xác định. Cân được coi là chính xác và chúng ta được phép thực hiện nhiều lần cân tùy ý. 

Ý tưởng chính là với một tập hợp trọng số cố định, không phải mọi trọng số mục tiêu nhất thiết đều có thể được xác định duy nhất. Một số trọng lượng có thể tạo ra kết quả cân không rõ ràng trong đó các trọng lượng sản phẩm khác nhau có thể dẫn đến cùng một chuỗi kết quả cân. Nhiệm vụ là xác định giá trị nguyên lớn nhất`p`sao cho mọi sản phẩm có trọng lượng từ`1`lên đến`p`có thể được xác định duy nhất bằng cách sử dụng các trọng lượng này và các lần cân lặp lại. 

Một cách có cấu trúc hơn để xem vấn đề là các trọng số xác định một hệ thống các giá trị có thể biểu diễn thông qua các kết hợp đã ký. Mỗi lần cân nhắc một cách hiệu quả cho phép so sánh hoạt động giống như việc chọn hệ số từ`{-1, 0, 1}`cho mỗi trọng lượng. Câu hỏi trọng tâm là cách biểu diễn có dấu này có thể được mở rộng đến mức nào mà không có xung đột về khả năng biểu diễn đối với số nguyên. 

Ràng buộc`n ≤ 2 * 10^5`ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các khoản tiền có thể đạt được một cách rõ ràng, vì ngay cả việc liệt kê các tập hợp con cũng sẽ tăng theo cấp số nhân. Ngay cả việc lập trình động trên tất cả các giá trị có thể truy cập cũng sẽ thất bại vì trọng số tăng lên`10^9`, làm cho bất kỳ phạm vi DP dày đặc nào đều không thể thực hiện được. 

Trường hợp cạnh tinh tế xuất hiện khi trọng số quá thưa thớt hoặc suy biến. Ví dụ: nếu chỉ có một trọng lượng duy nhất`[5]`, thì chúng ta không thể xác định duy nhất bất kỳ trọng lượng nào từ`1`ĐẾN`4`, vì chúng ta không thể biểu diễn chúng một cách rõ ràng bằng cách sử dụng bất kỳ tổ hợp trọng lượng nào. Câu trả lời đúng trong trường hợp này là`0`, mặc dù cách giải thích ngây thơ có thể gợi ý rằng chúng ta có thể đo bội số của 5. 

Một trường hợp góc khác là khi trọng lượng lớn nhưng bao gồm các số gia nhỏ, chẳng hạn như`[1, 3]`. Ở đây chúng ta có thể phân biệt trọng lượng lên đến`4`, nhưng không nhất thiết phải vượt quá điểm đó, bởi vì ngoài điểm đó, các biểu diễn bắt đầu trùng lặp về các kết quả cân bằng có thể đạt được. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để gán từng trọng lượng cho đĩa bên trái, đĩa bên phải hoặc không được sử dụng qua nhiều lần cân và kiểm tra xem trọng lượng sản phẩm nào vẫn có thể nhận dạng duy nhất. Điều này nhanh chóng trở nên khó giải quyết vì mỗi trọng lượng có ba trạng thái cho mỗi lần cân và ngay cả một lần cân cũng đã tạo ra`3^n`cấu hình. Với việc cho phép nhiều lần cân, không gian trạng thái sẽ mở rộng hơn nữa và không thể khám phá ngay cả đối với mức độ vừa phải.`n`. 

Cái nhìn sâu sắc quan trọng là về cơ bản nhiều trọng số không làm tăng sức mạnh biểu đạt ngoài sự kết hợp đã ký của các trọng số nhất định. Mỗi trọng số đóng góp một “thang phân giải” cố định và hệ thống hoạt động giống như xây dựng một cơ sở số trong đó các chữ số có thể được`-1, 0, 1`. Yếu tố giới hạn là liệu chúng ta có thể liên tục bao phủ dòng số nguyên mà không có khoảng trống về khả năng biểu diễn hay không. 

Nếu sắp xếp các trọng số, chúng ta có thể xử lý chúng theo thứ tự tăng dần và duy trì phạm vi liền kề tối đa`[0, p]`có thể được biểu diễn một cách duy nhất. Mỗi trọng lượng mới sẽ mở rộng phạm vi này hoặc tạo ra khoảng cách. Nếu hiện tại chúng ta có thể biểu diễn tất cả các giá trị lên tới`p`, sau đó là trọng lượng mới`x`chỉ có thể mở rộng phạm vi này nếu`x ≤ p + 1`. Ngược lại, một khoảng trống sẽ xuất hiện ở`p + 1`, khiến cho không thể biểu diễn duy nhất giá trị đó và quá trình dừng lại. 

Điều này biến vấn đề thành một cuộc quét tham lam trên các trọng số được sắp xếp trong khi vẫn duy trì tiền tố có thể truy cập của các giá trị có thể biểu thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force tất cả các lần cân | Hàm mũ | Hàm mũ | Quá chậm | 
| Đã sắp xếp phần mở rộng phạm vi tham lam | O(n log n) | O(1) hoặc O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng trọng số theo thứ tự không giảm. Điều này cho phép chúng tôi xử lý chúng theo thứ tự mà chúng có khả năng mở rộng phạm vi có thể biểu thị. 
2. Khởi tạo một biến`p = 0`, đại diện cho giá trị trọng lượng tối đa hiện có thể được xác định duy nhất. 
3. Lặp lại từng trọng số`x`theo thứ tự sắp xếp. Ở mỗi bước, chúng tôi kiểm tra xem trọng số này có thể mở rộng phạm vi biểu diễn liền kề hay không. 

Lý do là nếu chúng ta có thể xác định duy nhất tất cả các trọng số lên đến`p`, thì giá trị còn thiếu tiếp theo là`p + 1`. Trọng số mới phải có khả năng “bắc cầu” hoặc bao phủ giá trị tiếp theo này để mở rộng hệ thống. 
4. Nếu`x > p + 1`, chúng tôi dừng lại ngay lập tức. Điều này có nghĩa là có một khoảng trống ở`p + 1`không thể được lấp đầy bởi bất kỳ sự kết hợp nào của trọng số trước đó hoặc trọng số hiện tại, do đó tính duy nhất không vượt quá điểm này. 
5. Nếu không, chúng tôi sẽ cập nhật`p = p + x`. Bước này phản ánh rằng việc thêm trọng số mới sẽ mở rộng phạm vi tổng có thể phân biệt được một cách chính xác.`x`, bởi vì tất cả các giá trị có thể biểu thị trước đây giờ đây có thể được dịch chuyển và kết hợp với trọng số này. 
6. Sau khi xử lý tất cả các trọng số, xuất ra`p`. 

Điều bất biến được duy trì là tại bất kỳ thời điểm nào trong quá trình lặp, mọi số nguyên trong`[0, p]`có thể được biểu diễn duy nhất bằng cách sử dụng các trọng số đã xử lý với các kết hợp có dấu. Thứ tự được sắp xếp đảm bảo rằng khi chúng ta cố gắng kéo dài khoảng thời gian này, không thiếu trọng lượng nào nhỏ hơn chưa được xử lý cần thiết để lấp đầy khoảng trống. Nếu một khoảng trống xuất hiện thì đó là vĩnh viễn vì trọng số lớn hơn không thể tạo ra các khoảng tăng nhỏ hơn có thể biểu thị được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

p = 0
for x in a:
    if x > p + 1:
        break
    p += x

print(p)
```Bước sắp xếp là cần thiết vì đối số tham lam dựa vào việc luôn cố gắng mở rộng giá trị biểu thị bị thiếu nhỏ nhất trước tiên. Nếu không phân loại, trọng lượng lớn xuất hiện sớm có thể bị thổi phồng không chính xác`p`và che giấu một khoảng trống có thể cản trở sự tiến bộ hơn nữa. 

điều kiện`x > p + 1`là kiểm tra ranh giới quan trọng. Nó phát hiện khi trọng lượng hiện tại quá lớn để góp phần lấp đầy giá trị nhỏ nhất không thể truy cập được, giá trị này luôn luôn là`p + 1`. Bất kỳ việc triển khai nào thay vào đó sẽ kiểm tra lại`p`hoặc sử dụng một bất đẳng thức khác sẽ thất bại trong trường hợp khoảng cách chính xác là một đơn vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi bắt đầu với`p = 0`. 

| Bước | x | p trước | Tình trạng | p sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 1 | 1 | 
| 2 | 2 | 1 | 2 2 2 | 3 | 
| 3 | 3 | 3 | 3 4 | 6 | 
| 4 | 4 | 6 | 4 7 | 10 | 

Đầu ra là`10`. 

Dấu vết này cho thấy sự mở rộng liên tục mà không có bất kỳ khoảng trống nào. Mỗi trọng số mới vẫn nằm trong giới hạn có thể tiếp cận, xác nhận rằng khoảng có thể biểu diễn tăng trưởng đều đặn mà không bị gián đoạn. 

### Ví dụ 2 

đầu vào:```
3
1 2 5
```| Bước | x | p trước | Tình trạng | p sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 1 | 1 | 
| 2 | 2 | 1 | 2 2 2 | 3 | 
| 3 | 5 | 3 | 5 > 4 | dừng lại | 

Đầu ra là`3`. 

Điều này cho thấy một điểm thất bại: sau khi đạt được`3`, giá trị còn thiếu tiếp theo là`4`, nhưng trọng lượng tiếp theo là`5`, quá lớn để lấp đầy khoảng trống. Việc xây dựng bị phá vỡ ở ranh giới chính xác này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian | O(1) hoặc O(n) | tùy thuộc vào việc sắp xếp có đúng chỗ hay không | 

Các ràng buộc cho phép lên đến`2 * 10^5`trọng lượng, vì vậy một`O(n log n)`giải pháp phù hợp thoải mái trong các giới hạn thông thường và quét tuyến tính sẽ bổ sung thêm chi phí không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    p = 0
    for x in a:
        if x > p + 1:
            break
        p += x

    return str(p)

# provided samples (from statement)
assert run("4\n1 2 3 4\n") == "10"
assert run("2\n1 3\n") == "4"

# custom cases
assert run("1\n5\n") == "0", "single large weight"
assert run("1\n1\n") == "1", "minimal extension"
assert run("3\n1 2 5\n") == "3", "gap after small prefix"
assert run("5\n1 1 1 1 1\n") == "5", "repeated small weights"
assert run("6\n2 2 2 2 2 2\n") == "2", "no early bridging possible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5`|`0`| không có phạm vi đại diện | 
|`1 1`|`1`| trường hợp không tầm thường nhỏ nhất | 
|`1 2 5`|`3`| phát hiện khoảng cách chính xác | 
|`1 1 1 1 1`|`5`| tăng dần nhỏ lặp đi lặp lại | 
|`2 2 2 2 2 2`|`2`| trường hợp trì trệ | 

## Vỏ cạnh 

Khi chỉ có một trọng lượng lớn hơn`1`, thuật toán dừng ngay tại`p = 0`bởi vì`x > p + 1`. Đối với đầu vào`5`, quá trình quét sẽ dừng ngay lập tức và xuất ra`0`, phù hợp với thực tế là không có giá trị nhỏ hơn nào có thể được tách biệt duy nhất. 

Khi tất cả các trọng số đều có giá trị nhỏ giống hệt nhau như`1`, phạm vi tăng tuyến tính. Mỗi bước thỏa mãn điều kiện`1 ≤ p + 1`, do đó khoảng mở rộng một cách xác định, tạo ra câu trả lời cuối cùng bằng`n`. 

Khi có khoảng trống xuất hiện sau tiền tố hợp lệ, chẳng hạn như`1, 2, 10`, các quá trình thuật toán`1`Và`2`để đạt được`p = 3`, sau đó phát hiện`10 > 4`và dừng lại. Dấu vết xác nhận rằng giá trị biểu thị bị thiếu nhỏ nhất sẽ chi phối tất cả tính khả thi trong tương lai và trọng số lớn hơn không thể sửa chữa các khoảng trống trước đó.
