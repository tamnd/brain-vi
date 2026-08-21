---
title: "CF 104069D - Nhật Ký Hạnh Phúc"
description: "Chúng ta được cấp một dãy số nguyên biểu thị cảm giác của một người trong một chuỗi ngày. Mỗi giá trị nằm trong khoảng từ -10 đến 10, do đó, chuỗi này mã hóa các điểm tâm trạng ngắn hàng ngày."
date: "2026-07-02T02:59:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "D"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 42
verified: true
draft: false
---

[CF 104069D - Nhật ký hạnh phúc](https://codeforces.com/problemset/problem/104069/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy số nguyên biểu thị cảm giác của một người trong một chuỗi ngày. Mỗi giá trị nằm trong khoảng từ -10 đến 10, do đó, chuỗi này mã hóa các điểm tâm trạng ngắn hàng ngày. Nhiệm vụ là tính trung bình số học của tất cả các giá trị này và phân loại kết quả thành một trong ba nhãn cảm xúc: trung bình dương, trung bình 0 hoặc trung bình âm. 

Đầu ra không phải là số trung bình mà chỉ là dấu của nó. Nếu tổng tất cả các giá trị chia cho n lớn hơn 0, chúng ta sẽ in ra một khuôn mặt vui vẻ. Nếu nó chính xác bằng 0, chúng ta sẽ in ra một mặt trung tính. Nếu không, chúng tôi sẽ in một khuôn mặt buồn. 

Quan sát quan trọng là việc tính toán trung bình một cách rõ ràng dưới dạng số dấu phẩy động là không cần thiết. Vì phép chia cho một số nguyên dương không làm thay đổi dấu của một số nên thay vào đó chúng ta có thể làm việc trực tiếp với tổng của mảng. 

Ràng buộc n 100000 có nghĩa là quét tuyến tính là đủ. Bất kỳ giải pháp nào cố gắng tính toán lại các phạm vi con hoặc tổng hợp lặp lại sẽ vẫn đạt, nhưng bất cứ điều gì tệ hơn O(n) là không cần thiết. Cách tiếp cận bậc hai sẽ thực hiện tối đa 10^10 thao tác trong trường hợp xấu nhất, rõ ràng là quá chậm so với giới hạn 1 giây. 

Các trường hợp cạnh chủ yếu liên quan đến việc xử lý dấu hiệu. 

Trường hợp một cạnh là khi tất cả các số đều bằng 0. Tổng bằng 0 và đầu ra phải là ":|". Việc triển khai trung bình dấu phẩy động bất cẩn có thể gây ra các vấn đề làm tròn nếu được thực hiện bằng số float, nhưng tổng số nguyên sẽ tránh hoàn toàn điều đó. 

Một trường hợp cạnh khác là khi tổng rất nhỏ nhưng khác 0, chẳng hạn như [-1, 1]. Giá trị trung bình chính xác bằng 0 và số học số nguyên bảo toàn chính xác điều đó. Điều này rất quan trọng vì phép chia dấu phẩy động có thể tạo ra một epsilon nhỏ thay vì số 0 chính xác tùy thuộc vào chi tiết triển khai. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo của vấn đề là tính tổng của tất cả các giá trị và chia cho n, sau đó kiểm tra kết quả. Đây đã là thời gian tuyến tính vì nó yêu cầu một lần tính tổng và chia theo thời gian không đổi sau đó. Không có cấu trúc bài toán con có ý nghĩa hoặc không cần tính toán tiền tố, sắp xếp hoặc cấu trúc dữ liệu. 

Một phiên bản đơn giản hơn một chút có thể tính giá trị trung bình trong một vòng lặp bằng cách cộng và chia liên tục hoặc tính lại tổng một phần nhiều lần. Ví dụ: việc tính lại tổng từ đầu cho mỗi phần tử sẽ dẫn đến thời gian O(n^2), điều này trở nên không khả thi khi n đạt tới 100000. 

Sự đơn giản hóa chính là thừa nhận rằng dấu của trung bình chỉ phụ thuộc vào dấu của tổng vì n hoàn toàn dương. Điều này loại bỏ hoàn toàn nhu cầu phân chia trong hầu hết các triển khai và tránh các vấn đề về dấu phẩy động. 

Chúng tôi giảm vấn đề thành một lần tích lũy duy nhất trên mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính lại tổng nhiều lần | O(n^2) | O(1) | Quá chậm | 
| Tổng số lần vượt qua | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n, biểu thị số ngày được ghi. Điều này xác định có bao nhiêu giá trị chúng tôi sẽ tổng hợp. 
2. Khởi tạo một biến`total_sum = 0`. Biến này sẽ tích lũy tất cả điểm hàng ngày khi chúng tôi xử lý dữ liệu đầu vào. 
3. Lặp lại từng số nguyên trong dãy và cộng từng giá trị vào`total_sum`. Điều này xây dựng tổng chính xác của tất cả tâm trạng hàng ngày trong một lần. 
4. Sau khi xử lý tất cả các giá trị, so sánh`total_sum`bằng không. Vì n dương nên việc chia cho n sẽ không đổi dấu, nên phép so sánh này xác định đầy đủ câu trả lời. 
5. Nếu`total_sum > 0`, xuất ra ":)". Nếu như`total_sum == 0`, xuất ra ":|". Nếu không thì xuất ra ":(". 

Tại sao nó hoạt động: mức trung bình được định nghĩa là`total_sum / n`. Vì n > 0 nên dấu của phân số hoàn toàn được xác định bởi`total_sum`. Phép chia không thể lật dấu nên việc kiểm tra tổng trực tiếp cũng tương đương với việc kiểm tra trung bình cộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    total = 0
    for x in arr:
        total += x
    
    if total > 0:
        print(":)")
    elif total == 0:
        print(":|")
    else:
        print(":(")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc kích thước đầu vào và danh sách các số nguyên. Sau đó, nó thực hiện một vòng lặp tích lũy duy nhất, đó là tính toán cốt lõi. Không cần cấu trúc trung gian nào ngoài bộ tích lũy số nguyên. 

Bước quyết định cuối cùng so sánh số tiền tích lũy với số 0. Điều này tránh tính toán mức trung bình thực tế và tránh hoàn toàn số học dấu phẩy động, giúp giải pháp luôn chính xác và an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`10 10 10 10 10`Chúng tôi giải thích điều này là năm giá trị tích cực. 

| Bước | Giá trị hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| 1 | 10 | 10 | 
| 2 | 10 | 20 | 
| 3 | 10 | 30 | 
| 4 | 10 | 40 | 
| 5 | 10 | 50 | 

Tổng cuối cùng là 50, là số dương nên kết quả đầu ra là ":)". 

Điều này xác nhận rằng các đầu vào dương nhất quán sẽ tạo ra sự phân loại tích cực bất kể tỷ lệ cường độ. 

### Ví dụ 2:`-1 1`| Bước | Giá trị hiện tại | Tổng Chạy | 
| --- | --- | --- | 
| 1 | -1 | -1 | 
| 2 | 1 | 0 | 

Tổng cuối cùng là 0 nên kết quả đầu ra là ":|". 

Điều này thể hiện trường hợp hủy trong đó các giá trị dương và âm cân bằng chính xác và cho thấy tại sao việc tích lũy số nguyên lại bảo toàn chính xác tính trung lập chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần trong một lượt | 
| Không gian | O(1) | Chỉ có một bộ tích lũy số nguyên duy nhất được sử dụng ngoài bộ lưu trữ đầu vào | 

Kích thước đầu vào lên tới 100000 vừa vặn thoải mái trong quá trình quét tuyến tính. Các thao tác được thực hiện là các phép cộng và so sánh số nguyên đơn giản, đủ nhanh trong các ràng buộc điển hình của Codeforces. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n10 10 10 10 10\n") == ":)", "sample 1"
assert run("2\n-1 1\n") == ":|", "sample 2"

# custom cases
assert run("1\n5\n") == ":)", "single positive"
assert run("1\n-7\n") == ":(", "single negative"
assert run("3\n0 0 0\n") == ":|", "all zeros"
assert run("4\n1 2 -4 1\n") == ":|", "balanced mix"
assert run("6\n10 10 10 -10 -10 -10\n") == ":|", "symmetric cancellation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tích cực duy nhất | :) | trường hợp tích cực tối thiểu | 
| âm đơn | :( | trường hợp tiêu cực tối thiểu | 
| tất cả số không | : | | 
| hỗn hợp cân bằng | : | | 
| hủy bỏ đối xứng | : | | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị bằng 0. Ví dụ: 

đầu vào:```
3
0 0 0
```Thuật toán khởi tạo`total = 0`và không bao giờ thay đổi nó. Sự so sánh cuối cùng`total == 0`kích hoạt và đầu ra ":|", điều này đúng. 

Một trường hợp khác là sự hủy bỏ hoàn hảo: 

đầu vào:```
2
-1 1
```Tổng chạy trở thành 0 sau khi xử lý cả hai phần tử. Mặc dù các giá trị trung gian là âm nhưng trạng thái cuối cùng mới là điều quan trọng. Thuật toán xuất ra chính xác ":|". 

Trường hợp tinh tế cuối cùng là độ lệch dương hoặc âm lớn, chẳng hạn như tất cả 10 hoặc tất cả -10. Vì bộ tích lũy sử dụng số học số nguyên và cường độ tổng tối đa được giới hạn bởi 100000 × 10 = 10^6 nên không có rủi ro tràn trong các loại số nguyên 32 bit hoặc 64 bit tiêu chuẩn. Việc so sánh dấu hiệu vẫn ổn định và mang tính quyết định.
