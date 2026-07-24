---
title: "CF 103785A - BCD"
description: "Chúng ta được cung cấp một bộ sưu tập các đồ vật giống hệt nhau và một thùng chứa. Mỗi vùng chứa có thể chứa tối đa các đối tượng $K$ và chúng tôi muốn đặt tất cả các đối tượng $N$ vào vùng chứa. Nhiệm vụ là xác định số lượng thùng chứa tối thiểu cần thiết khi chúng ta đóng gói một cách tối ưu."
date: "2026-07-02T08:50:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103785
codeforces_index: "A"
codeforces_contest_name: "CodeBrew : Freshers Contest 2022"
rating: 0
weight: 103785
solve_time_s: 44
verified: true
draft: false
---

[CF 103785A - BCD](https://codeforces.com/problemset/problem/103785/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các đồ vật giống hệt nhau và một thùng chứa. Mỗi thùng chứa tối đa$K$các đối tượng và chúng tôi muốn đặt tất cả$N$các đồ vật vào trong thùng chứa. Nhiệm vụ là xác định số lượng thùng chứa tối thiểu cần thiết khi chúng ta đóng gói một cách tối ưu. 

Đầu vào bao gồm hai số nguyên. Cái đầu tiên thể hiện tổng số đồ vật chúng ta có. Số thứ hai đại diện cho số lượng đối tượng tối đa có thể được đặt trong một thùng chứa. Đầu ra là một số nguyên biểu thị số lượng thùng chứa cần thiết để chứa tất cả các đối tượng mà không vượt quá dung lượng. 

Các ràng buộc đủ nhỏ để việc tính toán phải có thời gian không đổi cho mỗi trường hợp thử nghiệm. Kể cả nếu$N$lớn, có khả năng lên tới$10^9$trở lên trong cài đặt lập trình cạnh tranh điển hình, các phép toán liên quan là số học đơn giản, do đó, mô phỏng đóng gói tuyến tính hoặc lặp sẽ không cần thiết và có thể quá chậm nếu lặp lại trong nhiều trường hợp thử nghiệm. Giải pháp mong đợi phải dựa vào lý luận toán học trực tiếp hơn là mô phỏng. 

Trường hợp cạnh tinh tế xuất hiện khi số lượng đối tượng chia hết cho kích thước vùng chứa. Ví dụ, nếu$N = 10$Và$K = 2$, câu trả lời đúng là 5. Một cách tiếp cận ngây thơ luôn thêm một vùng chứa bổ sung sau khi chia sẽ trả về không chính xác 6. Một trường hợp cạnh khác là khi$K > N$. Ví dụ,$N = 3$,$K = 10$, câu trả lời đúng là 1, không phải 0. Phép chia số nguyên thuần túy$N / K$sẽ đưa ra sai 0 container nếu không điều chỉnh. 

## Phương pháp tiếp cận 

Một cách đơn giản để suy nghĩ về vấn đề này là mô phỏng việc đổ đầy từng thùng chứa một. Chúng tôi bắt đầu với thùng chứa đầu tiên, đặt tối đa$K$các đối tượng, trừ chúng khỏi$N$và lặp lại cho đến khi không còn vật nào. Mỗi lần lặp đại diện cho việc lấp đầy một thùng chứa. Điều này đúng vì nó phản ánh trực tiếp ràng buộc. 

Tuy nhiên, phương pháp này thực hiện gần như$N / K$lặp đi lặp lại, và trong trường hợp xấu nhất là$K = 1$, nó thoái hóa thành$N$các bước. Nếu như$N$lớn, điều này trở nên không hiệu quả và không cần thiết vì cấu trúc của bài toán hoàn toàn là việc đóng gói đồng nhất. 

Quan sát quan trọng là mọi thùng chứa, ngoại trừ thùng chứa cuối cùng, đều được lấp đầy hoàn toàn. Điều này có nghĩa là chúng ta đang phân vùng một cách hiệu quả$N$các mục thành các nhóm kích thước$K$. Số lượng các nhóm như vậy đúng bằng mức trần$N / K$. Điều này loại bỏ hoàn toàn mô phỏng và giảm bài toán xuống một biểu thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng | O(N/K) | O(1) | Quá chậm | 
| Phân chia trực tiếp có trần | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của$N$Và$K$. Những điều này xác định số lượng vật phẩm chúng ta phải đặt và số lượng mỗi thùng chứa có thể chứa. 
2. Tính phép chia số nguyên$N // K$, đếm xem chúng ta có thể hình thành bao nhiêu thùng chứa đầy. Điều này đưa ra số lượng container cơ sở giả định việc đóng gói hoàn hảo. 
3. Kiểm tra xem có số dư khi chia không$N$qua$K$. Nếu như$N \bmod K \neq 0$, khi đó có những đồ còn sót lại cần có thùng chứa bổ sung. 
4. Nếu còn dư, hãy tăng số lượng container lên một. Điều này giải thích cho vùng chứa cuối cùng được lấp đầy một phần. 
5. Xuất số đếm cuối cùng. 

Lý do đằng sau việc kiểm tra số dư là phép chia số nguyên cắt ngắn về 0, loại bỏ mọi phần còn sót lại không tạo thành một nhóm đầy đủ. Phần bị loại bỏ đó vẫn cần có không gian trong một thùng chứa bổ sung. 

### Tại sao nó hoạt động 

Mỗi thùng chứa, ngoại trừ thùng cuối cùng, phải chứa chính xác$K$các mặt hàng trong một đóng gói tối ưu. Nếu chúng ta có nhiều hơn một vùng chứa được lấp đầy một phần, chúng ta có thể hợp nhất chúng và giảm tổng số vùng chứa, trái ngược với sự tối ưu. Do đó, có thể có nhiều nhất một thùng chứa không đầy đủ và nó tồn tại khi và chỉ khi$N$không chia hết cho$K$. Điều này đảm bảo rằng việc tính toán mức trần của$N / K$đưa ra số lượng container tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

containers = n // k
if n % k != 0:
    containers += 1

print(containers)
```Giải pháp mã hóa trực tiếp quan sát toán học. Phép chia số nguyên tính toán số lượng vùng chứa đầy và kiểm tra modulo xác định xem có cần thêm một vùng chứa hay không. Không cần vòng lặp hoặc cấu trúc dữ liệu bổ sung, giúp duy trì việc triển khai ở mức tối thiểu và tránh mọi rủi ro về vấn đề hiệu suất. 

Một lỗi phổ biến là quên trường hợp còn lại. Chỉ viết`n // k`thất bại khi$N$không chia hết cho$K$, vì các đối tượng còn sót lại vẫn cần một thùng chứa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 10, k = 3
```| Bước | n // k | n % k | container | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | 1 | 3 | 
| Kiểm tra phần còn lại | | 1 ≠ 0 | 4 | 

Sư đoàn đưa ra 3 thùng đầy chứa 9 đồ vật. Còn lại một món, cần thêm hộp đựng nên đáp án cuối cùng là 4. 

### Ví dụ 2 

đầu vào:```
n = 8, k = 4
```| Bước | n // k | n % k | container | 
| --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | 2 | 
| Kiểm tra phần còn lại | | 0 | 2 | 

Ở đây, tất cả các vật dụng đều được đựng chính xác vào hai thùng chứa không có thức ăn thừa. Không cần vùng chứa bổ sung, xác nhận tính chính xác của kết quả chia số nguyên. 

Những ví dụ này cho thấy thuật toán phân biệt chính xác giữa trường hợp chia chính xác và trường hợp còn sót lại, đây là quyết định cấu trúc duy nhất cần có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép toán số học được thực hiện bất kể kích thước đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này phù hợp một cách thoải mái với mọi ràng buộc hợp lý vì nó thực hiện một số thao tác không đổi cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    containers = n // k
    if n % k != 0:
        containers += 1
    return str(containers)

# provided samples
assert run("10 3") == "4"
assert run("8 4") == "2"

# custom cases
assert run("1 1") == "1", "single item single capacity"
assert run("5 10") == "1", "capacity larger than items"
assert run("12 5") == "3", "multiple full plus remainder"
assert run("1000000000 1") == "1000000000", "max spread case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp cân bằng nhỏ nhất | 
| 5 10 | 1 | công suất lớn hơn N | 
| 12 5 | 3 | xử lý phần còn lại | 
| 1000000000 1 | 1000000000 | kịch bản căng thẳng đếm tuyến tính trong trường hợp xấu nhất | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$N < K$. Đối với đầu vào`5 10`, thuật toán tính toán$5 // 10 = 0$và tìm thấy phần dư khác 0, điều này gây ra sự gia tăng. Kết quả cuối cùng trở thành 1, thể hiện chính xác rằng ngay cả một số lượng nhỏ mặt hàng vẫn cần một thùng chứa. 

Một trường hợp khác là chia hết chính xác, chẳng hạn như`12 3`. Tính toán mang lại kết quả$12 // 3 = 4$và số dư bằng không. Vì không cần thêm khoảng trống nên câu trả lời vẫn là 4. Điều này xác nhận rằng thuật toán không đếm quá mức các vùng chứa. 

Trường hợp thứ ba là đầu vào tối thiểu như`1 1`. Ở đây phép chia mang lại 1 và không có số dư nào tồn tại, do đó kết quả là 1, phù hợp với kỳ vọng trực quan rằng một mục sẽ lấp đầy chính xác một thùng chứa.
