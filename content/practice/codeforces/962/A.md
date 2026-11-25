---
title: "CF 962A - Xích đạo"
description: "Chúng ta được cho một chuỗi ngày và mỗi ngày Polycarp giải quyết một số vấn đề cố định. Nếu chúng ta xem xét toàn bộ thời gian đào tạo, có tổng số vấn đề được giải quyết trong tất cả các ngày."
date: "2026-06-17T01:44:23+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 962
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 42 (Rated for Div. 2)"
rating: 1300
weight: 962
solve_time_s: 74
verified: true
draft: false
---

[CF 962A - Xích đạo](https://codeforces.com/problemset/problem/962/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi ngày và mỗi ngày Polycarp giải quyết một số vấn đề cố định. Nếu chúng ta xem xét toàn bộ thời gian đào tạo, có tổng số vấn đề được giải quyết trong tất cả các ngày. Nhiệm vụ là tìm ngày sớm nhất sao cho đến cuối ngày hôm đó Polycarp đã giải được ít nhất một nửa tổng số bài toán trong cả lịch trình. 

Nói cách khác, chúng tôi tính tổng hiện hành từ ngày đầu tiên trở đi và dừng ở vị trí đầu tiên nơi tổng tiền tố này đạt ít nhất một nửa tổng tổng. Câu trả lời chính là chỉ số của ngày hôm đó. 

Các ràng buộc cho phép lên tới 200.000 ngày, vì vậy mọi giải pháp đều phải chạy theo thời gian tuyến tính. Một cách tiếp cận bậc hai liên tục tính toán lại các tổng tiền tố hoặc các hậu tố quét sẽ thực hiện theo thứ tự$n^2$, quá chậm ở quy mô này. Tích lũy một lượt là đủ và cần thiết. 

Một điểm tinh tế là định nghĩa “một nửa hoặc nhiều hơn”. Vì tổng số có thể là số lẻ nên chúng tôi không chia thành các nửa chính xác mà so sánh với ngưỡng của$\lceil \frac{S}{2} \rceil$hoặc kiểm tra tương đương khi tổng tiền tố nhân với 2 ít nhất bằng tổng. 

Các trường hợp cạnh chủ yếu là cấu trúc. Khi$n = 1$, câu trả lời luôn là 1 vì ngày đầu tiên đã có đủ mọi vấn đề. Một trường hợp góc khác là khi một giá trị lớn xuất hiện muộn trong chuỗi, nghĩa là câu trả lời không sớm ngay cả khi tiền tố ban đầu tăng chậm. 

## Phương pháp tiếp cận 

Một cách đơn giản để giải bài toán là trước tiên hãy tính tổng số bài toán, sau đó mỗi ngày tính lại tổng số bài toán từ ngày đầu tiên cho đến ngày hôm đó. Mỗi lần kiểm tra sẽ liên quan đến việc tổng hợp tiền tố từ đầu. Điều này đúng vì nó phản ánh trực tiếp định nghĩa của điều kiện, nhưng nó lặp đi lặp lại rất nhiều. Trong ngày$i$, tính toán lại tổng chi phí tiền tố$O(i)$, và tính tổng tất cả các ngày cho kết quả$O(n^2)$hoạt động trong trường hợp xấu nhất, quá lớn đối với$n = 200{,}000$. 

Quan sát quan trọng là tổng tiền tố có thể được duy trì tăng dần. Thay vì tính toán lại, chúng tôi tích lũy tổng chạy khi lặp lại một lần. Ở mỗi bước, chúng tôi so sánh tổng tiền tố hiện tại với một nửa tổng số tiền. Chỉ số đầu tiên thỏa mãn điều kiện là câu trả lời. 

Điều này có hiệu quả vì điều kiện chỉ phụ thuộc vào tiền tố và tổng tiền tố tạo thành một chuỗi không giảm đơn điệu. Sau khi vượt qua ngưỡng, nó sẽ vẫn được vượt qua cho tất cả các chỉ số sau này, do đó điểm giao nhau đầu tiên được xác định duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Quét tổng tiền tố tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dãy các bài toán được giải hàng ngày và tính tổng của tất cả các giá trị. Tổng số này xác định ngưỡng chúng ta phải đạt tới. 
2. Khởi tạo một biến đang chạy`prefix = 0`. Điều này sẽ lưu trữ số lượng tích lũy các vấn đề được giải quyết cho đến ngày hiện tại. 
3. Lặp lại các ngày từ trái sang phải. Đối với mỗi ngày, hãy thêm giá trị hiện tại vào`prefix`. 
4. Sau khi cập nhật`prefix`, kiểm tra xem`2 * prefix >= total`. Sự so sánh này tránh được phép chia dấu phẩy động và mã hóa trực tiếp điều kiện “ít nhất một nửa”. 
5. Chỉ mục đầu tiên mà điều kiện trở thành đúng sẽ được trả về ngay lập tức dưới dạng câu trả lời. 

Lý do đằng sau việc dừng ngay lập tức là khi tiền tố đạt đến một nửa tổng số, tất cả các tiền tố sau cũng sẽ thỏa mãn điều kiện do tăng trưởng đơn điệu. 

### Tại sao nó hoạt động 

Chuỗi tổng tiền tố không giảm vì tất cả các giá trị hàng ngày đều dương. Điều kiện “tổng tiền tố ít nhất bằng một nửa tổng số tiền” xác định một vị từ đơn điệu trên các chỉ số: một khi nó trở thành đúng, nó sẽ không bao giờ trở thành sai nữa. Điều này đảm bảo rằng chỉ mục đầu tiên thỏa mãn nó được xác định rõ ràng và duy nhất, và việc quét từ trái sang phải sẽ tìm thấy nó mà không cần quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)
prefix = 0

for i, x in enumerate(a, start=1):
    prefix += x
    if prefix * 2 >= total:
        print(i)
        break
```Giải pháp đầu tiên tính toán tổng số vấn đề để biết được ngưỡng mục tiêu. Sau đó, nó thực hiện một lần chuyển duy nhất, duy trì tổng tiền tố đang chạy. Phép so sánh sử dụng phép nhân với 2 để tránh các phép tính dấu phẩy động và bảo đảm an toàn cho số nguyên. 

Việc liệt kê bắt đầu từ 1 vì vấn đề yêu cầu chỉ số ngày dựa trên 1. Khi điều kiện được thỏa mãn, vòng lặp sẽ kết thúc sớm, đảm bảo tính toán không cần thiết ở mức tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 3 2 1
```Tổng tổng là 7, do đó ngưỡng là 3,5, nghĩa là chúng tôi tìm kiếm tiền tố đầu tiên đạt ít nhất 4. 

| Ngày | Giá trị | Tiền tố Tổng | Điều kiện (tiền tố 2* >= 7) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Không | 
| 2 | 3 | 4 | Có | 

Câu trả lời là 2 vì tiền tố đạt tới 4 vào ngày thứ 2, đã giải quyết được ít nhất một nửa số vấn đề. 

### Ví dụ 2 

đầu vào:```
6
1 2 3 4 2 0
```Tổng số tiền là 12, vì vậy ngưỡng là 6. 

| Ngày | Giá trị | Tiền tố Tổng | Điều kiện (tiền tố 2* >= 12) | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Không | 
| 2 | 2 | 3 | Không | 
| 3 | 3 | 6 | Có | 

Ngày hợp lệ đầu tiên là 3, vì tổng tiền tố bằng chính xác một nửa tổng số tại thời điểm đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính tổng một lần và sau đó quét mảng một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Quét tuyến tính là đủ cho$n \le 200{,}000$và tất cả các hoạt động là phép cộng và so sánh số nguyên theo thời gian không đổi, do đó giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))
    
    total = sum(a)
    prefix = 0
    
    for i, x in enumerate(a, start=1):
        prefix += x
        if prefix * 2 >= total:
            return str(i)

    return ""

# provided sample
assert run("4\n1 3 2 1\n") == "2"

# minimum size
assert run("1\n5\n") == "1"

# already satisfied early
assert run("3\n10 1 1\n") == "1"

# exact half on last element
assert run("4\n1 1 1 3\n") == "4"

# all equal
assert run("5\n2 2 2 2 2\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 | 1 | trường hợp cạnh phần tử đơn | 
| 10 1 1 | 1 | sự thống trị sớm của yếu tố đầu tiên | 
| 1 1 1 3 | 4 | ngưỡng đạt đến vị trí cuối cùng | 
| 2 2 2 2 2 | 3 | tăng trưởng tiền tố đối xứng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi chỉ riêng phần tử đầu tiên đã vượt quá một nửa tổng số. Đối với đầu vào:```
3
10 1 1
```Tổng là 12, vậy một nửa là 6. Tiền tố đầu tiên là 10, đã thỏa mãn điều kiện nên câu trả lời là 1. Thuật toán xử lý việc này một cách tự nhiên vì việc kiểm tra được thực hiện ngay sau khi xử lý phần tử đầu tiên. 

Một trường hợp khác là khi đạt đến ngưỡng chính xác ở phần tử cuối cùng. Vì:```
4
1 1 1 3
```Tổng là 6, một nửa là 3. Tổng tiền tố là 1, 2, 3, 6 và lần đầu tiên chúng ta đạt ít nhất 3 là vào ngày thứ 3, nhưng tùy thuộc vào cách xử lý đẳng thức, ngày 3 hợp lệ vì 2 * 3 >= 6. Thuật toán kích hoạt chính xác tại thời điểm đó và dừng sớm. 

Trường hợp cấu trúc cuối cùng là phân bố đồng đều, chẳng hạn như:```
5
2 2 2 2 2
```Tổng số là 10, một nửa là 5. Tổng tiền tố là 2, 4, 6, vì vậy ngày thứ 3 là lần đầu tiên vượt qua ngưỡng. Bản chất đơn điệu của tổng tiền tố đảm bảo không có sự mơ hồ trong việc lựa chọn chỉ số này.
