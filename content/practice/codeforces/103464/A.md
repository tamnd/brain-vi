---
title: "CF 103464A - Stegosaurus"
description: "Chúng ta được cung cấp một bộ sưu tập stegosauruses, mỗi con được liên kết với một giá trị số nguyên duy nhất biểu thị số lượng gai mà nó có."
date: "2026-07-03T06:52:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103464
codeforces_index: "A"
codeforces_contest_name: "The second stage of the Republican Olympiad in Informatics. Mogilev region, 2021."
rating: 0
weight: 103464
solve_time_s: 45
verified: true
draft: false
---

[CF 103464A - Khủng long Stegosaurus](https://codeforces.com/problemset/problem/103464/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập stegosauruses, mỗi con được liên kết với một giá trị số nguyên duy nhất biểu thị số lượng gai mà nó có. Từ bộ sưu tập này, chúng ta phải chọn bất kỳ hai loài khủng long stegosaurus riêng biệt nào và đo lường mức độ giống nhau về ngoại hình của chúng, trong đó độ giống nhau được xác định là sự khác biệt tuyệt đối giữa số lượng gai của chúng. 

Nhiệm vụ giảm xuống còn việc tìm cặp giá trị có chênh lệch số càng nhỏ càng tốt và đưa ra chênh lệch tối thiểu đó. 

Được diễn đạt lại theo thuật ngữ thuật toán, chúng ta được cung cấp một mảng và phải tính toán chênh lệch tuyệt đối nhỏ nhất giữa hai phần tử riêng biệt bất kỳ. 

Quan sát quan trọng từ các ràng buộc là$n \le 10^5$, điều này ngay lập tức loại trừ bất kỳ phương pháp so sánh cặp bậc hai nào. Một vòng lặp đôi ngây thơ sẽ kiểm tra đại khái$\frac{n(n-1)}{2}$cặp, trong trường hợp xấu nhất là về$5 \times 10^9$hoạt động. Điều này vượt xa giới hạn một hoặc hai giây thông thường trong chương trình thi đấu. 

Một số trường hợp đặc biệt quan trọng trong thực tế. Nếu nhiều giá trị giống hệt nhau, chẳng hạn như đầu vào như`1 1 1 1`, câu trả lời đúng là 0 vì việc chọn bất kỳ cặp bằng nhau nào cũng mang lại hiệu số bằng 0. Một cách tiếp cận bất cẩn chỉ kiểm tra các vị trí liền kề trong một mảng chưa được sắp xếp sẽ thất bại ở đây nếu nó không đảm bảo các giá trị bằng nhau trở nên liền kề sau khi xử lý trước. 

Một trường hợp tinh tế khác xuất hiện khi các giá trị bị phân tán rộng rãi, chẳng hạn`0 100 50 49 51`. Câu trả lời đúng là 1, đến từ cặp (50, 49) hoặc (50, 51). Bất kỳ giải pháp nào không có lý do chung về mức độ gần nhau sau khi đặt hàng đều có thể bỏ lỡ các cặp như vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng cặp stegosaurus, tính toán sự khác biệt tuyệt đối về số lượng tăng đột biến của chúng và giữ mức tối thiểu. Điều này hiệu quả vì nó trực tiếp đánh giá định nghĩa của câu trả lời mà không có bất kỳ sự chuyển đổi nào. Tuy nhiên, nó trở nên quá chậm vì phải kiểm tra tất cả các cặp, dẫn đến$O(n^2)$so sánh. 

Thông tin chi tiết quan trọng đến từ việc nhận ra rằng sự khác biệt tuyệt đối hoạt động tốt khi sắp xếp. Nếu chúng ta sắp xếp mảng thì cặp giá trị gần nhất phải xuất hiện cạnh nhau theo thứ tự được sắp xếp. Điều này là do bất kỳ cặp không liền kề nào cũng có ít nhất một phần tử ở giữa và phần tử trung gian đó chỉ có thể giảm khoảng cách giữa các cặp lân cận, không bao giờ tăng khoảng cách vượt quá ứng cử viên liền kề tốt hơn. 

Như vậy thay vì so sánh tất cả các cặp, chúng ta chỉ cần so sánh các phần tử liên tiếp sau khi sắp xếp. Điều này làm giảm vấn đề quét mảng đã sắp xếp một lần và theo dõi chênh lệch liền kề tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + Quét |$O(n \log n)$|$O(1)$hoặc$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$n$và mảng số lượng tăng đột biến. Điều này tạo thành tập dữ liệu chúng ta phải phân tích. 
2. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp rất quan trọng vì nó sắp xếp lại các giá trị sao cho các số gần đó trong không gian giá trị trở thành số lân cận trong mảng. 
3. Khởi tạo một biến`ans`với số lượng rất lớn. Biến này theo dõi sự khác biệt nhỏ nhất được tìm thấy cho đến nay. 
4. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến phần tử cuối cùng, kiểm tra từng cặp liền kề. 
5. Đối với mỗi chỉ số$i$, tính sự khác biệt giữa`a[i+1]`Và`a[i]`. Vì mảng đã được sắp xếp nên sự khác biệt này được đảm bảo không âm và thể hiện sự so sánh gần nhất có thể liên quan đến`a[i]`ở phía bên phải của nó. 
6. Cập nhật`ans`nếu chênh lệch này nhỏ hơn giá trị được lưu trữ hiện tại. Điều này đảm bảo chúng tôi luôn giữ được ứng viên tốt nhất được thấy cho đến nay. 
7. Sau khi xử lý tất cả các cặp liền kề, xuất ra`ans`. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là trong một mảng được sắp xếp, bất kỳ cặp nào$(i, j)$với$j > i + 1$không thể tạo ra sự khác biệt nhỏ hơn một số cặp liền kề giữa chúng. Về mặt hình thức, đối với bất kỳ$i < k < j$, chúng ta có:$$a[j] - a[i] = (a[j] - a[k]) + (a[k] - a[i])$$Cả hai số hạng đều không âm sau khi sắp xếp, do đó tổng chênh lệch ít nhất bằng một trong các khoảng trống liền kề bên trong khoảng. Điều này đảm bảo rằng mức tối thiểu toàn cục phải xảy ra giữa các phần tử liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

ans = float('inf')

for i in range(n - 1):
    diff = a[i + 1] - a[i]
    if diff < ans:
        ans = diff

print(ans)
```Mã theo thuật toán trực tiếp. Bước sắp xếp đảm bảo chúng ta chỉ cần so sánh cục bộ. Vòng lặp trên các cặp liền kề là an toàn vì mọi cặp ứng cử viên có liên quan đều được biểu diễn ở đó. sử dụng`float('inf')`là một cách thuận tiện để khởi tạo trình theo dõi tối thiểu mà không phải lo lắng về giới hạn của các giá trị đầu vào. 

Một lỗi triển khai phổ biến là quên sắp xếp mảng, dẫn đến thiếu hoàn toàn cặp tối ưu. Một vấn đề tinh tế khác là sử dụng sự khác biệt tuyệt đối trên dữ liệu chưa được sắp xếp nhưng chỉ kiểm tra các dữ liệu lân cận, điều này không chính xác vì mức độ gần gũi không mang tính cục bộ trong một sắp xếp chưa được sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 6 0 19 10
```Mảng được sắp xếp trở thành`[0, 2, 6, 10, 19]`. 
| tôi | một [tôi] | a[i+1] | sự khác biệt | và | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 2 | 2 | 
| 1 | 2 | 6 | 4 | 2 | 
| 2 | 6 | 10 | 4 | 2 | 
| 3 | 10 | 19 | 9 | 2 | 
Câu trả lời cuối cùng là`2`. 

Dấu vết này cho thấy khoảng cách nhỏ nhất xuất hiện như thế nào ngay sau khi sắp xếp và không có sự so sánh nào sau đó sẽ cải thiện nó. 

### Ví dụ 2 

đầu vào:```
7
1 2 1 2 4 2 3
```Mảng được sắp xếp trở thành`[1, 1, 2, 2, 2, 3, 4]`. 

| tôi | một [tôi] | a[i+1] | khác biệt | trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 0 | 
| 1 | 1 | 2 | 1 | 0 | 
| 2 | 2 | 2 | 0 | 0 | 
| 3 | 2 | 2 | 0 | 0 | 
| 4 | 2 | 3 | 1 | 0 | 
| 5 | 3 | 4 | 1 | 0 | 

Câu trả lời cuối cùng là`0`. 

Điều này xác nhận rằng các bản sao được xử lý một cách tự nhiên vì việc sắp xếp các nhóm giá trị giống hệt nhau, ngay lập tức tạo ra sự khác biệt bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau | 
| Không gian |$O(1)$hoặc$O(n)$| Phụ thuộc vào việc thực hiện sắp xếp | 

Được cho$n \le 10^5$, việc sắp xếp vừa vặn thoải mái trong giới hạn thời gian và đường chuyền tuyến tính là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()
    ans = float('inf')
    for i in range(n - 1):
        ans = min(ans, a[i + 1] - a[i])
    return str(ans)

# provided samples
assert run("5\n2 6 0 19 10\n") == "2"
assert run("7\n1 2 1 2 4 2 3\n") == "0"
assert run("2\n10 15\n") == "5"

# custom cases
assert run("2\n0 0\n") == "0", "all equal"
assert run("3\n1000000000 0 500000000\n") == "500000000", "large spread"
assert run("4\n1 100 101 102\n") == "1", "clustered values"
assert run("5\n5 4 3 2 1\n") == "1", "reverse order input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`trường hợp | 0 | xử lý trùng lặp | 
| lan rộng lớn | 500000000 | giá trị lớn đúng đắn | 
| giá trị nhóm | 1 | phát hiện tối thiểu liền kề | 
| thứ tự ngược lại | 1 | phân loại cần thiết | 

## Vỏ cạnh 

Đối với các đầu vào nặng trùng lặp như`4 4 4 4`, thuật toán sắp xếp thành một mảng không đổi và mọi hiệu liền kề đều bằng 0, do đó kết quả chính xác là 0 mà không cần xử lý đặc biệt. 

Đối với các đầu vào được sắp xếp ngược như`5 4 3 2 1`, sắp xếp biến nó thành`1 2 3 4 5`và chênh lệch liền kề tối thiểu trở thành 1. Một cách tiếp cận ngây thơ bỏ qua việc sắp xếp và chỉ kiểm tra các hàng xóm ban đầu sẽ kết luận sai các khoảng trống lớn hơn chẳng hạn như 4 hoặc 3. 

Đối với các trường hợp phân tách giá trị lớn như`0 1000000000 500`, sắp xếp đảm bảo cặp gần nhất được tiết lộ là`(500, 0)`hoặc`(1000000000, 500)`, với mức tối thiểu chính xác được trích xuất từ ​​​​các so sánh liền kề sau khi đặt hàng.
