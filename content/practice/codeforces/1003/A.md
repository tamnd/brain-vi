---
title: "CF 1003A - Túi Polycarp"
description: "Chúng ta được cấp nhiều tập hợp giá trị đồng xu và chúng ta cần đặt mọi đồng xu vào một bộ sưu tập “túi” theo một hạn chế đơn giản: bên trong bất kỳ túi riêng lẻ nào, không có giá trị nào được phép lặp lại."
date: "2026-06-16T23:30:13+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1003
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 494 (Div. 3)"
rating: 800
weight: 1003
solve_time_s: 75
verified: true
draft: false
---

[CF 1003A - Túi của Polycarp](https://codeforces.com/problemset/problem/1003/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp nhiều tập hợp giá trị đồng xu và chúng ta cần đặt mọi đồng xu vào một bộ sưu tập “túi” theo một hạn chế đơn giản: bên trong bất kỳ túi riêng lẻ nào, không có giá trị nào được phép lặp lại. Các đồng xu không thể phân biệt được ngoại trừ giá trị của chúng, vì vậy nhiệm vụ hoàn toàn là về số lượng bản sao của mỗi giá trị tồn tại và cách chúng có thể được phân tách giữa các nhóm. 

Mỗi túi hoạt động giống như một tập hợp các giá trị, không phải là nhiều tập hợp. Nếu một giá trị xuất hiện nhiều lần trong đầu vào thì những bản sao đó phải được phân phối trên các túi khác nhau. Mục tiêu là giảm thiểu số lượng túi như vậy cần thiết để chứa tất cả các đồng tiền. 

Phạm vi ràng buộc rất nhỏ, tối đa 100 xu và giá trị cũng bị giới hạn bởi 100. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào từ thời gian bậc hai trở xuống đều đủ dễ dàng và ngay cả các phương pháp đếm trực tiếp cũng an toàn mà không cần quan tâm đến hiệu suất. 

Hành vi của cạnh chính xuất phát từ các bản sao. Nếu tất cả các giá trị là khác nhau thì một túi là đủ. Nếu tất cả các giá trị đều giống nhau thì mỗi đồng xu sẽ tạo thành một túi riêng biệt vì không có túi nào có thể chứa hai giá trị giống hệt nhau. Một trường hợp tinh tế là khi một số giá trị lặp lại với tần số khác nhau, bởi vì hệ số giới hạn không phải là tổng số đồng xu mà là giá trị đơn lẻ thường xuyên nhất. 

Ví dụ: nếu chúng ta có đầu vào như`1 1 2 2 3`, giá trị`1`xuất hiện hai lần và`2`cũng xuất hiện hai lần, buộc phải có ít nhất hai túi, mặc dù tổng kích thước lớn hơn hai. Bất kỳ nỗ lực ngây thơ nào chỉ xem xét tổng số hoặc số lượng duy nhất sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng mô phỏng thực sự quá trình đóng gói. Người ta có thể liên tục tạo các túi bằng cách đặt các đồng xu một cách tham lam trong khi đảm bảo không có bản sao trong túi, loại bỏ các đồng xu đã sử dụng và lặp lại cho đến khi tất cả các đồng xu được đặt. Điều này đúng vì mỗi bước tạo ra một nhóm hợp lệ và cuối cùng tất cả các đồng tiền đều được chỉ định. 

Vấn đề là hiệu quả. Mỗi lần chúng tôi xây dựng một túi, chúng tôi sẽ quét các đồng xu còn lại và cố gắng ghép càng nhiều giá trị riêng biệt càng tốt. Trong trường hợp xấu nhất, chẳng hạn như khi tất cả các đồng tiền giống hệt nhau hoặc khi tồn tại nhiều bản sao, chúng tôi liên tục xử lý gần như toàn bộ danh sách. Điều này dẫn đến hành vi gần như O(n²) hoặc tệ hơn tùy thuộc vào chi tiết triển khai và mặc dù n ở đây nhỏ nhưng cách tiếp cận này về mặt khái niệm là không cần thiết. 

Quan sát quan trọng là mỗi giá trị riêng biệt có tần số f đóng góp chính xác f “yêu cầu” phải được đặt vào các túi khác nhau. Mỗi túi có thể chứa tối đa một lần xuất hiện của mỗi giá trị, vì vậy nếu một giá trị xuất hiện f lần, chúng ta cần ít nhất f túi khác nhau để đặt tất cả các bản sao của nó. Điều này có nghĩa là câu trả lời ít nhất phải có tần số tối đa của bất kỳ giá trị nào. 

Giới hạn này cũng đủ. Nếu chúng ta tạo nhiều túi với tần suất tối đa, chúng ta có thể phân phối số lần xuất hiện của từng giá trị trên các túi khác nhau theo kiểu vòng tròn đơn giản. Vì không có giá trị nào xuất hiện nhiều lần hơn số lượng túi nên mọi lần xuất hiện đều có thể được đặt mà không có xung đột. 

Điều này làm giảm vấn đề thành nhiệm vụ đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đóng gói vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Đếm tần số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi giá trị đồng xu xuất hiện bao nhiêu lần trong mảng. Điều này nắm bắt tất cả các ràng buộc ở dạng nhỏ gọn vì chỉ có sự trùng lặp mới gây ra xung đột. 
2. Theo dõi tần số lớn nhất trong số tất cả các giá trị. Giá trị này thể hiện số lượng túi tối thiểu cần thiết để ngay cả giá trị thường xuyên nhất cũng có thể được phân tách trên các túi khác nhau. 
3. Xuất tần số tối đa này làm câu trả lời. Mọi giá trị khác sẽ tự động nằm gọn trong các túi này vì không có giá trị nào trong số chúng xuất hiện thường xuyên hơn dung lượng đã chọn. 

### Tại sao nó hoạt động 

Mỗi túi có thể chứa tối đa một đồng xu có giá trị nhất định. Do đó, nếu một giá trị nào đó xuất hiện f lần, chúng ta cần ít nhất f túi riêng biệt để đặt tất cả các lần xuất hiện mà không lặp lại. Ngược lại, nếu chúng ta có k túi trong đó k bằng tần số tối đa, chúng ta có thể gán lần xuất hiện thứ i của mọi giá trị cho túi i. Điều này đảm bảo không có hai giá trị giống hệt nhau nào va chạm vào cùng một túi và mỗi đồng xu được chỉ định chính xác một lần. 

Thuật toán này đúng vì nó biến đổi ràng buộc đóng gói thành ràng buộc dung lượng trên mỗi giá trị và sử dụng giới hạn chặt chẽ nhất có thể trên tất cả các giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

freq = [0] * 101

for x in a:
    freq[x] += 1

ans = max(freq)
print(ans)
```Giải pháp dựa hoàn toàn vào việc đếm tần số. Mảng`freq`có kích thước là 101 vì các giá trị được đảm bảo nằm trong khoảng từ 1 đến 100. Mỗi đồng xu tăng nhóm tương ứng của nó và câu trả lời cuối cùng chỉ đơn giản là giá trị nhóm tối đa. 

Không cần phải sắp xếp hoặc mô phỏng. Logic xoay quanh thực tế là các ràng buộc độc lập giữa các giá trị ngoại trừ việc chia sẻ các nhóm và hạn chế chung duy nhất là các giá trị giống hệt nhau phải được tách biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 2 4 3 3 2
```Tần số là: 

| Giá trị | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 

Chúng tôi chỉ theo dõi số lượng tối đa. 

| Bước | Giá trị được xử lý | trạng thái tần số (một phần) | tần số tối đa | 
| --- | --- | --- | --- | 
| 1 | 1 | 1:1 | 1 | 
| 2 | 2 | 2:1 | 1 | 
| 3 | 4 | 4:1 | 1 | 
| 4 | 3 | 3:1 | 1 | 
| 5 | 3 | 3:2 | 2 | 
| 6 | 2 | 2:2 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy rằng các bản sao xác định giới hạn chứ không phải tổng kích thước. 

### Ví dụ 2 

đầu vào:```
5
5 5 5 5 1
```| Giá trị | Đếm | 
| --- | --- | 
| 5 | 4 | 
| 1 | 1 | 

| Bước | Giá trị được xử lý | trạng thái tần số | tần số tối đa | 
| --- | --- | --- | --- | 
| 1 | 5 | 5:1 | 1 | 
| 2 | 5 | 5:2 | 2 | 
| 3 | 5 | 5:3 | 3 | 
| 4 | 5 | 5:4 | 4 | 
| 5 | 1 | 1:1 | 4 | 

Câu trả lời cuối cùng là 4. 

Điều này thể hiện hành vi trong trường hợp xấu nhất trong đó một giá trị duy nhất quyết định số lượng túi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đồng xu được xử lý một lần để cập nhật tần số đếm | 
| Không gian | O(1) | Mảng tần số có kích thước cố định (100 giá trị có thể có) | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì n tối đa là 100 và các thao tác hoàn toàn là gia số theo thời gian không đổi và lần quét cuối cùng trên một mảng cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    freq = [0] * 101
    for x in a:
        freq[x] += 1
    return str(max(freq))

# provided sample
assert run("6\n1 2 4 3 3 2\n") == "2"

# all distinct
assert run("4\n1 2 3 4\n") == "1"

# all same
assert run("5\n7 7 7 7 7\n") == "5"

# mixed frequencies
assert run("7\n1 1 2 2 2 3 4\n") == "3"

# single element
assert run("1\n10\n") == "1"

# two high-frequency values
assert run("6\n5 5 6 6 6 5\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | 1 | tất cả các giá trị riêng biệt | 
| 7 7 7 7 7 | 5 | trường hợp lặp lại tối đa | 
| 1 1 2 2 2 3 4 | 3 | cấu trúc tần số hỗn hợp | 
| 10 | 1 | đầu vào kích thước tối thiểu | 

## Vỏ cạnh 

Khi tất cả các đồng xu có giá trị duy nhất, mỗi tần số là 1, do đó tối đa là 1 và chỉ cần một túi. Thuật toán khởi tạo tất cả các tần số về 0 và tăng từng tần số một lần, để lại mức tối đa là 1, tạo ra một túi duy nhất một cách chính xác. 

Khi tất cả các đồng xu đều giống hệt nhau, mỗi lần tăng đều chạm vào cùng một nhóm. Đối với một đầu vào như`7 7 7 7`, tần số của 7 trở thành 4 và thuật toán xuất ra 4. Điều này phù hợp với thực tế là mỗi đồng xu giống hệt nhau phải chiếm một túi riêng do hạn chế. 

Khi nhiều giá trị có cùng tần số tối đa, chẳng hạn như`1 1 2 2`, cả hai giá trị đều đạt tần số 2. Giá trị tối đa vẫn là 2 và hai túi là đủ. Mỗi lần xuất hiện của 1 và 2 có thể được chia thành hai túi mà không có xung đột, xác nhận rằng các mối quan hệ không thay đổi kết quả vượt quá giá trị tối đa.
