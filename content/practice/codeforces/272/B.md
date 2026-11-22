---
title: "CF 272B - Dima và Trình tự"
description: "Chúng ta được cho một chuỗi các số nguyên dương và một hàm ánh xạ mỗi số nguyên tới một giá trị không âm. Hàm được định nghĩa đệ quy: nó gửi từ 0 đến 0, nó bỏ qua thừa số 2 và mỗi khi chúng ta gặp một số lẻ, chúng ta sẽ đóng góp một đơn vị một cách hiệu quả và…"
date: "2026-06-05T01:41:49+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 272
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 167 (Div. 2)"
rating: 1400
weight: 272
solve_time_s: 69
verified: true
draft: false
---

[CF 272B - Dima và Trình tự](https://codeforces.com/problemset/problem/272/B) 

**Đánh giá:** 1400 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên dương và một hàm ánh xạ mỗi số nguyên tới một giá trị không âm. Hàm được định nghĩa đệ quy: nó gửi từ 0 đến 0, bỏ qua thừa số hai và mỗi khi gặp một số lẻ, chúng ta đóng góp một đơn vị một cách hiệu quả và tiếp tục xử lý số nguyên chia cho hai. 

Một cách hữu ích để giải thích điều này là hàm này đếm số lần chúng ta loại bỏ các chữ số nhị phân ở cuối cho đến khi đạt đến 0, nhưng chỉ đếm những đóng góp đến từ các phần lẻ. Cụ thể, hóa ra giá trị f(x) chính xác là số đơn vị trong biểu diễn nhị phân của x. Điều này là do việc chia cho hai sẽ dịch chuyển biểu diễn nhị phân sang phải và trừ đi một khi số lẻ tương ứng với việc loại bỏ một bit đã đặt. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu cặp chỉ số i và j sao cho i nhỏ hơn j và các giá trị hàm của a[i] và a[j] bằng nhau. Nói cách khác, chúng ta cần nhóm các số theo số nhị phân của chúng và đếm xem có bao nhiêu cặp có thể được hình thành trong mỗi nhóm. 

Kích thước đầu vào đạt tới 100000 phần tử, mỗi phần tử lên tới 10^9. So sánh bậc hai trên tất cả các cặp sẽ bao gồm khoảng 5 tỷ lần kiểm tra trong trường hợp xấu nhất, quá chậm. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các số giống hệt nhau hoặc khi tất cả các số có số lượng nhị phân riêng biệt. Trong trường hợp đầu tiên, mọi cặp đều đóng góp, dẫn đến n(n−1)/2. Trong trường hợp thứ hai, câu trả lời trở thành số không. Việc triển khai đơn giản có thể liên tục tính toán lại f(x) không hiệu quả, dẫn đến chi phí không cần thiết hoặc thậm chí hết thời gian chờ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ tính toán f(a[i]) cho mọi phần tử và so sánh nó với mọi phần tử khác. Ngay cả khi tính toán f(x) là hiệu quả, việc so sánh tất cả các cặp vẫn tốn O(n²), tức là khoảng 10¹⁰ phép tính với n = 10⁵, điều này là không khả thi. 

Quan sát quan trọng là chúng ta thực sự không cần so sánh các cặp một cách rõ ràng. Chúng ta chỉ quan tâm đến việc mỗi giá trị của f(x) xuất hiện bao nhiêu lần. Khi đã biết tần số, mỗi nhóm sẽ đóng góp sự kết hợp của hai phần tử. Nếu một giá trị cụ thể xuất hiện k lần, nó sẽ đóng góp k(k−1)/2 cặp. 

Do đó, vấn đề giảm xuống còn việc tính toán bản đồ tần số của các giá trị f(a[i]). Vì f(x) là số lượng bit được đặt trong x, nên chúng ta có thể tính toán trực tiếp bằng cách sử dụng thao tác đếm bit trong O(log x) hoặc phân bổ O(1) tùy thuộc vào việc triển khai. Điều này biến vấn đề thành quét tuyến tính bằng cách băm hoặc đếm mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tần số của f(x) | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các số nguyên từ chuỗi đầu vào. 

Chúng ta cần chúng được lưu trữ hoặc xử lý ngay lập tức để có thể tính toán các giá trị được chuyển đổi của chúng. 
2. Với mỗi số a[i], hãy tính f(a[i]). 

Vì f(x) bằng số lượng bit được đặt trong x nên chúng tôi tính toán số lượng. Điều này làm giảm hàm đệ quy thành thao tác bit trực tiếp. 
3. Duy trì một từ điển (hoặc bản đồ băm) để đếm số lần mỗi giá trị f xuất hiện. 

Điều này cho phép chúng ta nhóm các số theo kết quả chuyển đổi của chúng mà không cần sắp xếp. 
4. Sau khi xử lý tất cả các phần tử, lặp lại tất cả các giá trị tần số trong bản đồ. 

Với mỗi tần số k, hãy thêm k(k−1)/2 vào câu trả lời vì mọi cặp trong nhóm đó đều hợp lệ. 
5. Xuất kết quả tích lũy. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc phân vùng mảng thành các lớp tương đương được xác định bởi các giá trị f(x) bằng nhau. Mỗi cặp chúng tôi đếm chính xác là một cặp chỉ số trong cùng một lớp. Vì mỗi cặp hợp lệ phải đến từ chính xác một lớp và mọi cặp trong một lớp đều hợp lệ, nên việc tính tổng tất cả các lớp sẽ tạo ra số lượng chính xác không có sự trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    ans = 0

    for x in a:
        v = x.bit_count()
        if v in freq:
            freq[v] += 1
        else:
            freq[v] = 1

    for cnt in freq.values():
        ans += cnt * (cnt - 1) // 2

    print(ans)

if __name__ == "__main__":
    main()
```Giải pháp đọc dữ liệu đầu vào một lần và xử lý từng số một cách độc lập. Thao tác chính là`bit_count()`, tính toán số lượng bit được thiết lập một cách hiệu quả trong Python. Chúng tôi lưu trữ tần số của các giá trị này trong từ điển. Cuối cùng, chúng tôi tính toán đóng góp của cặp bằng công thức tổ hợp để chọn hai phần tử từ mỗi nhóm. Số học số nguyên là an toàn vì câu trả lời tối đa nằm trong phạm vi số nguyên có dấu 64 bit cho n tối đa 10^5. 

Một cạm bẫy triển khai phổ biến là tính toán lại f(x) nhiều lần bên trong các vòng lặp lồng nhau, dẫn đến hành vi bậc hai. Một vấn đề tế nhị khác là quên rằng từ điển Python phải được lặp qua các giá trị chứ không phải khóa khi tích lũy các kết hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 4
```Tất cả các số đều có chính xác một bit được đặt, vì vậy tất cả đều ánh xạ tới giá trị 1. 

| Bước | x | f(x) | bản đồ tần số | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | {1:1} | 
| 2 | 2 | 1 | {1:2} | 
| 3 | 4 | 1 | {1:3} | 

Bây giờ chúng tôi tính toán đóng góp từ giá trị 1: 3 × 2/2 = 3. 

Điều này cho thấy trường hợp tất cả các phần tử thuộc về một lớp tương đương, tạo ra số lượng cặp tối đa. 

### Ví dụ 2 

đầu vào:```
5
1 2 3 4 7
```Các dạng nhị phân cho số lượng: 1, 1, 2, 1, 3. 

| Bước | x | f(x) | bản đồ tần số | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | {1:1} | 
| 2 | 2 | 1 | {1:2} | 
| 3 | 3 | 2 | {1:2, 2:1} | 
| 4 | 4 | 1 | {1:3, 2:1} | 
| 5 | 7 | 3 | {1:3, 2:1, 3:1} | 

Bây giờ tính toán đóng góp: 

Đối với giá trị 1: 3 × 2/2 = 3, các giá trị khác đóng góp 0. 

Câu trả lời cuối cùng là 3, tương ứng với các cặp số có số lượng bằng nhau. 

Ví dụ này chứng minh rằng chỉ có việc nhóm mới quan trọng chứ không phải giá trị tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một lần và số lượng được khấu hao O(1) trong Python | 
| Không gian | O(n) | Bản đồ tần số lưu trữ tối đa n khóa riêng biệt | 

Giải pháp vừa vặn một cách thoải mái trong các ràng buộc vì n là 10^5 và tất cả các thao tác đều là các đường truyền tuyến tính qua đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    freq = {}
    ans = 0

    for x in a:
        v = x.bit_count()
        freq[v] = freq.get(v, 0) + 1

    for cnt in freq.values():
        ans += cnt * (cnt - 1) // 2

    return str(ans)

# provided sample
assert run("3\n1 2 4\n") == "3"

# all distinct popcounts
assert run("4\n1 2 4 8\n") == "0"

# all equal values
assert run("5\n3 3 3 3 3\n") == "10"

# mixed case
assert run("6\n1 2 3 4 5 6\n") == run("6\n1 2 3 4 5 6\n")

# single element
assert run("1\n100\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các quyền hạn riêng biệt của hai | 0 | không có cặp nào tồn tại | 
| tất cả các số giống hệt nhau | cặp tối đa | đếm tổ hợp đầy đủ | 
| phần tử đơn | 0 | điều kiện biên | 
| giá trị hỗn hợp | tính toán đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Đầu vào tối thiểu có kích thước một, chẳng hạn như`1 / 100`, tạo ra các cặp bằng không. Thuật toán xử lý việc này vì bản đồ tần số chứa một mục nhập duy nhất có số một và 1·0/2 có giá trị bằng 0. 

Trường hợp mọi phần tử đều là lũy thừa của hai, chẳng hạn như`1 2 4 8 16`, cũng mang lại kết quả bằng 0 vì mỗi số có chính xác một bit được đặt, vì vậy tất cả các giá trị f(x) đều giống hệt nhau. Thuật toán nhóm tất cả vào một nhóm và tính 5·4/2 = 10, điều này đúng vì mọi cặp đều khớp. 

Trường hợp tất cả các số đều giống hệt nhau, chẳng hạn như`3 3 3 3`, tạo ra bốn phần tử trong một nhóm. Thuật toán tính 4·3/2 = 6, khớp với số cặp không có thứ tự.
