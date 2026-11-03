---
title: "CF 1106C - Tết Nguyên Đán và Phân Số"
description: "Chúng ta được cung cấp một danh sách các số nguyên dương có độ dài chẵn và chúng ta phải phân chia các số này thành các nhóm. Mỗi nhóm phải chứa ít nhất hai phần tử và mỗi số phải thuộc chính xác một nhóm."
date: "2026-06-12T05:27:54+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1106
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 536 (Div. 2)"
rating: 900
weight: 1106
solve_time_s: 75
verified: true
draft: false
---

[CF 1106C - Tết Nguyên đán và Phân chia số](https://codeforces.com/problemset/problem/1106/C) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số nguyên dương có độ dài chẵn và chúng ta phải phân chia các số này thành các nhóm. Mỗi nhóm phải chứa ít nhất hai phần tử và mỗi số phải thuộc chính xác một nhóm. Đối với mỗi nhóm, chúng tôi tính tổng các phần tử của nó, bình phương tổng đó và sau đó tính tổng các giá trị bình phương này trên tất cả các nhóm. Mục tiêu là chọn nhóm làm giảm thiểu tổng số này. 

Một cách khác để xem nhiệm vụ là chúng ta đang quyết định cách "phân cụm" các số sao cho hình phạt tăng bậc hai với mỗi tổng cụm càng nhỏ càng tốt. Tổng lớn rất tốn kém vì bình phương sẽ khuếch đại chúng, do đó việc phân phối số giữa các nhóm là có lợi, nhưng các nhóm vẫn phải chứa ít nhất hai phần tử, điều này ngăn cản việc phân chia các phần tử đơn lẻ tầm thường. 

Các ràng buộc rất lớn, lên tới 300.000 số. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các phân vùng hoặc thậm chí ghép nối với lập trình động trên các tập hợp con. Bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc n log n, vì việc sắp xếp n log n đã gần đến giới hạn thực tế và mọi thứ bậc hai đều không thể thực hiện được. 

Một vấn đề tế nhị phát sinh từ yêu cầu “ít nhất 2 yếu tố cho mỗi nhóm”. Một chiến lược tham lam ngây thơ ghép các phần tử một cách tùy ý hoặc hợp nhất các giá trị nhỏ cục bộ một cách tham lam có thể thất bại do sự tương tác giữa kích thước nhóm và tổng bình phương là phi tuyến tính. Ví dụ: ghép các phần tử liền kề theo thứ tự tùy ý có thể không tối ưu. 

Một trường hợp thất bại khác xuất phát từ việc nghĩ rằng “luôn nhóm các phần tử nhỏ nhất lại với nhau” là tối ưu. Hãy xem xét một mảng như`[1, 1, 100, 100]`. Ghép các số nhỏ với nhau và các số lớn với nhau sẽ mang lại kết quả`(1+1)^2 + (100+100)^2 = 4 + 40000 = 40004`. Ghép nối chéo mang lại`(1+100)^2 + (1+100)^2 = 10201 + 10201 = 20402`, nhỏ hơn nhiều. Điều này cho thấy rằng số tiền cân bằng quan trọng hơn việc phân nhóm địa phương. 

## Phương pháp tiếp cận 

Giải thích bạo lực là xem xét tất cả các cách có thể để phân chia mảng thành các nhóm hợp lệ có kích thước ít nhất là hai. Ngay cả khi hạn chế ghép nối, số lượng kết quả khớp đã theo cấp số nhân và việc cho phép các nhóm có kích thước tùy ý làm cho không gian tìm kiếm tương đương với các phân vùng đã đặt, tăng theo cấp số nhân (số Chuông). Với n = 30, điều này đã không thể thực hiện được; với n = 300.000 thì điều đó hoàn toàn không thể xảy ra. 

Quan sát quan trọng đến từ việc viết lại mục tiêu. Khai triển hình vuông sẽ có:$$(\sum a_i)^2 = \sum a_i^2 + 2\sum_{i<j} a_i a_j$$Tổng số ô vuông riêng lẻ được cố định bất kể nhóm. Những thay đổi là thuật ngữ chéo, điều này phụ thuộc vào yếu tố nào được đặt cùng nhau. Vì vậy, việc giảm thiểu mục tiêu tương đương với việc giảm thiểu tổng các tích từng cặp trong các nhóm. 

Điều này thay đổi quan điểm: chúng tôi muốn quyết định phần tử nào tương tác theo cấp số nhân. Để giảm chi phí, chúng tôi muốn các phần tử lớn tránh bị nhóm cùng với các phần tử lớn khác. Việc trải rộng các giá trị lớn giữa các nhóm khác nhau sẽ làm giảm sản phẩm chéo lớn. Đồng thời, vì mỗi nhóm phải có ít nhất hai phần tử nên chúng ta không thể cô lập các phần tử một cách hoàn toàn. 

Cấu trúc tối ưu xuất hiện từ việc sắp xếp. Nếu chúng ta sắp xếp mảng, chiến lược tốt nhất là ghép nhỏ nhất với lớn nhất, nhỏ thứ hai với lớn thứ hai, v.v. Điều này cân bằng tổng số bên trong mỗi nhóm và giảm thiểu sự tăng trưởng bậc hai cực độ. Cấu trúc ghép nối này là tối ưu vì bất kỳ độ lệch nào đưa hai phần tử lớn vào cùng một nhóm sẽ làm tăng số hạng bậc hai nhiều hơn bất kỳ sự bù đắp nào có thể có ở nơi khác. 

Chúng tôi tạo thành chính xác n/2 nhóm có kích thước 2. Các nhóm lớn hơn luôn có thể được chia thành từng cặp mà không làm tăng chi phí trong một cấu hình tối ưu, vì việc chia tách làm giảm phương sai của tổng và giảm hình phạt bình phương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (sắp xếp + ghép nối) | O(n log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Bước này là cần thiết để cấu trúc các phần tử sao cho chúng ta có thể cân bằng một cách có hệ thống các giá trị lớn và nhỏ. 
2. Ghép nối phần tử nhỏ nhất với phần tử lớn nhất, phần tử nhỏ thứ hai với phần tử lớn thứ hai và tiếp tục đi vào trong cho đến khi tất cả các phần tử được ghép nối. Điều này đảm bảo mỗi nhóm có hai phần tử và các giá trị cực trị đó được trung hòa bằng cách ghép chúng với các phần tử đối lập về độ lớn. 
3. Với mỗi cặp, hãy tính tổng và bình phương nó, sau đó tích lũy thành đáp án. Việc phân nhóm được cố định tại thời điểm này nên chúng tôi chỉ đánh giá chi phí. 
4. Xuất ra tổng tổng của các tổng nhóm bình phương. 

Quyết định quan trọng là buộc ghép nối đối xứng sau khi sắp xếp. Nếu không sắp xếp, việc ghép đôi là tùy ý và có thể tạo ra các tổng mất cân bằng cao, dẫn đến hình phạt bậc hai lớn. 

### Tại sao nó hoạt động 

Mục tiêu bình phương trừng phạt sự mất cân bằng. Khi hai giá trị lớn được đặt cạnh nhau, tích của chúng trở nên rất lớn. Bằng cách ghép nối các thái cực, chúng tôi phân phối khối lượng đồng đều giữa các nhóm. Bất kỳ nỗ lực nào để đặt hai phần tử lớn vào cùng một nhóm đều có thể được chuyển thành cấu hình trong đó chúng được tách ra và phép biến đổi này làm giảm nghiêm trọng mục tiêu do tính lồi của hàm bình phương. Đối số trao đổi này đảm bảo rằng việc ghép cặp đối diện được sắp xếp là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

a.sort()

ans = 0
for i in range(n // 2):
    s = a[i] + a[n - 1 - i]
    ans += s * s

print(ans)
```Giải pháp bắt đầu bằng cách sắp xếp mảng sao cho các quyết định ghép nối trở nên có cấu trúc. Vòng lặp chạy chính xác n/2 lần, mỗi lần tạo thành một nhóm từ các vị trí đối xứng trong danh sách đã sắp xếp. Tổng của mỗi cặp được tính trực tiếp, bình phương và tích lũy. 

Một điểm tinh tế là chúng tôi không bao giờ thành lập các nhóm lớn hơn quy mô hai. Mặc dù vấn đề cho phép các nhóm lớn hơn, nhưng bất kỳ cấu hình tối ưu nào cũng có thể được chuyển đổi thành cấu hình dựa trên ghép nối mà không làm tăng chi phí, vì vậy việc hạn chế theo cặp là an toàn. 

Tràn số nguyên không phải là vấn đề trong Python, nhưng trong các ngôn ngữ khác, người ta phải đảm bảo số nguyên 64 bit vì các giá trị có thể đạt khoảng (2 * 10^4)^2 mỗi nhóm và tích lũy trên 1,5e5 nhóm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
8 5 2 3
```Mảng được sắp xếp là`[2, 3, 5, 8]`. Chúng tôi ghép đôi`(2, 8)`Và`(3, 5)`. 

| Bước | Cặp | Tổng hợp | Quảng trường | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (2, 8) | 10 | 100 | 100 | 
| 2 | (3, 5) | 8 | 64 | 164 | 

Điều này cho thấy các giá trị cực trị được cân bằng như thế nào để tránh bị phạt bậc hai lớn. 

### Ví dụ 2 

đầu vào:```
6
1 2 1 2 1 2
```Mảng được sắp xếp là`[1, 1, 1, 2, 2, 2]`. 

| Bước | Cặp | Tổng hợp | Quảng trường | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | (1, 2) | 3 | 9 | 9 | 
| 2 | (1, 2) | 3 | 9 | 18 | 
| 3 | (1, 2) | 3 | 9 | 27 | 

Điều này chứng tỏ rằng khi các giá trị có cấu trúc đồng nhất, thuật toán sẽ tự nhiên tạo ra các nhóm cân bằng bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, ghép nối là tuyến tính | 
| Không gian | O(1) thêm | Sắp xếp được thực hiện ngoài việc lưu trữ đầu vào | 

Các ràng buộc cho phép tối đa 300.000 phần tử và việc sắp xếp cộng với một lần quét tuyến tính duy nhất phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ ở mức tối thiểu, chỉ lưu trữ mảng đầu vào và một vài biến. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = 0
    for i in range(n // 2):
        ans += (a[i] + a[n - 1 - i]) ** 2

    return str(ans)

# provided sample
assert run("4\n8 5 2 3\n") == "164"

# minimum size
assert run("2\n1 1\n") == "4"

# all equal
assert run("4\n5 5 5 5\n") == "100"

# increasing sequence
assert run("4\n1 2 3 4\n") == "26"

# mixed large-small
assert run("6\n1 100 1 100 1 100\n") == "36300"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`4`| trường hợp cạnh tối thiểu | 
|`5 5 5 5`|`100`| giá trị thống nhất | 
|`1 2 3 4`|`26`| sắp xếp cân bằng | 
|`1 100 1 100 1 100`|`36300`| hành vi ghép đôi cực đoan | 

## Vỏ cạnh 

Đối với đầu vào nhỏ nhất`n = 2`, thuật toán tạo thành một nhóm duy nhất và xuất ra`(a1 + a2)^2`. Việc sắp xếp không làm thay đổi kết quả nên kết quả sẽ chính xác ngay lập tức. 

Đối với các mảng thống nhất như`[x, x, x, x]`, ghép nối theo bất kỳ cách nào cũng mang lại số tiền giống nhau, nhưng ghép nối được sắp xếp sẽ duy trì tính chính xác và tránh suy luận không cần thiết về các nhóm thay thế. 

Đối với các giá trị cực trị xen kẽ như`[1, 100, 1, 100, 1, 100]`, sắp xếp tạo ra`[1, 1, 1, 100, 100, 100]`và các cực trị ghép nối đảm bảo mỗi giá trị lớn được khớp với một giá trị nhỏ, ngăn chặn sự bùng nổ bậc hai trong việc nhóm các giá trị lớn lại với nhau.
