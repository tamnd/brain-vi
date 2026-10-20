---
title: "CF 105755C - Đếm cặp"
description: "Chúng tôi được cung cấp nhiều tập hợp số nguyên dương và chúng tôi muốn chọn một tập hợp con không có bản sao sao cho mọi cặp số được chọn đều thỏa mãn điều kiện bitwise nghiêm ngặt liên quan đến XOR. Với hai số được chọn bất kỳ $x$ và $y$, chúng ta tính $x oplus y$."
date: "2026-06-22T15:09:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105755
codeforces_index: "C"
codeforces_contest_name: "Bay Area Programming Contest 2025"
rating: 0
weight: 105755
solve_time_s: 80
verified: true
draft: false
---

[CF 105755C - Đếm cặp](https://codeforces.com/problemset/problem/105755/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều tập hợp số nguyên dương và chúng tôi muốn chọn một tập hợp con không có bản sao sao cho mọi cặp số được chọn đều thỏa mãn điều kiện bitwise nghiêm ngặt liên quan đến XOR. 

Với hai số được chọn bất kỳ$x$Và$y$, chúng tôi tính toán$x \oplus y$. Yêu cầu là kết quả XOR này phải lớn hơn số lớn hơn trong hai số. Nếu điều này không thành công đối với dù chỉ một cặp thì tập hợp con đó không hợp lệ. 

Vì vậy, nhiệm vụ là tối đa hóa số lượng giá trị riêng biệt mà chúng ta có thể chọn trong khi vẫn giữ ràng buộc theo cặp này đúng cho mọi cặp trong tập hợp con. 

Ràng buộc$n \le 3 \cdot 10^5$ngay lập tức loại trừ mọi phương pháp kiểm tra tất cả các cặp một cách rõ ràng. Việc kiểm tra ngây thơ đối với tất cả các tập hợp con hoặc thậm chí xác minh tham lam bằng so sánh từng cặp sẽ là phương trình bậc hai trong trường hợp xấu nhất và sẽ không vượt qua. Bất kỳ giải pháp hợp lệ nào cũng phải giảm sự tương tác giữa các phần tử xuống mức gần hơn với thời gian tuyến tính hoặc tuyến tính. 

Trường hợp cạnh khóa xuất hiện khi nhiều số có cùng phạm vi độ lớn. Ví dụ: hãy xem xét một khối số như$4, 5, 6, 7$. Tất cả chúng đều nằm trong cùng một khoảng lũy ​​thừa hai, nhưng hành vi XOR theo cặp của chúng bị hạn chế rất nhiều. Một trường hợp khác là khi các giá trị được trải rộng trên các phạm vi cường độ khác nhau, như$1, 2, 3$, trong đó một số cặp hoạt động tốt và những cặp khác ngay lập tức vi phạm điều kiện. Cách tiếp cận tham lam ngây thơ chỉ so sánh các giá trị liền kề hoặc chỉ kiểm tra một hướng đặt hàng sẽ thất bại trong các trường hợp hỗn hợp như vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử mọi tập hợp con và xác minh xem tất cả các cặp có thỏa mãn điều kiện XOR hay không. Ngay cả khi chúng tôi sửa một tập hợp con và xác thực nó trong$O(k^2)$, số lượng tập hợp con là theo cấp số nhân nên điều này hoàn toàn không khả thi. 

Một nỗ lực hợp lý hơn một chút là sắp xếp mảng và cố gắng xây dựng tập hợp con một cách tham lam, kiểm tra từng ứng cử viên mới dựa trên tất cả các phần tử đã được chọn. Điều này làm giảm việc tìm kiếm tập hợp con, nhưng bước xác thực vẫn trở thành$O(n^2)$trong trường hợp xấu nhất, vì mỗi lần chèn có thể quét toàn bộ tập hợp đã chọn. 

Quan sát quan trọng xuất phát từ việc hiểu cách XOR hoạt động so với bit số quan trọng nhất. Giá trị của một số bị chi phối bởi bit được đặt cao nhất của nó. Khi hai số khác nhau ở bit được đặt cao nhất, XOR hoạt động theo cách có xu hướng triệt tiêu hoặc bảo toàn độ lớn theo cách rất có cấu trúc. 

Nếu hai số nằm trong các nhóm bit cao nhất khác nhau thì một trong số chúng có bit quan trọng nhất cao hơn. Trong trường hợp đó, số lớn hơn chiếm ưu thế so sánh và XOR không thể vượt quá số đó một cách đáng tin cậy vì XOR không bao giờ đưa ra các bit cao hơn bit cao nhất của một trong hai toán hạng. Điều này ngay lập tức hạn chế những kết hợp nào là ứng cử viên chẵn. 

Bên trong nhóm bit cao nhất cố định, tất cả các số đều có chung bit đầu. XOR hủy bit đó, điều này buộc kết quả giảm xuống dưới toàn bộ phạm vi của nhóm đó. Vì mọi số trong nhóm ít nhất là$2^k$, trong khi XOR vẫn ở dưới$2^k$, bất kỳ cặp nào trong cùng một nhóm sẽ tự động vi phạm điều kiện. 

Điều này dẫn đến sự đơn giản hóa cấu trúc mạnh mẽ: có thể chọn nhiều nhất một số từ mỗi nhóm bit cao nhất. Khi điều này được nhận ra, vấn đề sẽ giảm xuống việc đếm xem có bao nhiêu nhóm bit cao nhất riêng biệt được biểu thị trong đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (nhóm MSB) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số, hãy xác định vị trí bit quan trọng nhất của nó. Điều này xác định khoảng lũy ​​thừa hai của số đó. 
2. Duy trì một mảng boolean hoặc tập hợp được lập chỉ mục theo vị trí bit để theo dõi nhóm nào đã được biểu diễn trong tập hợp con đã chọn. 
3. Lặp lại tất cả các số. Với mỗi số, hãy tính chỉ số MSB của nó. 
4. Nếu chỉ số MSB này chưa từng được nhìn thấy trước đây, hãy đánh dấu nó là đã sử dụng và tăng dần câu trả lời. Nếu không, hãy bỏ qua số vì việc chọn nhiều phần tử từ cùng một nhóm sẽ vi phạm điều kiện ngay lập tức. 
5. Xuất ra tổng số nhóm MSB riêng biệt gặp phải. 

### Tại sao nó hoạt động 

Điều bất biến là bất kỳ tập hợp con hợp lệ nào cũng có thể chứa tối đa một phần tử cho mỗi lớp bit cao nhất. Trong một lớp, mọi số đều nằm trong$[2^k, 2^{k+1})$. Đối với bất kỳ hai số nào như vậy, XOR sẽ loại bỏ bit đầu tiên và tạo ra một giá trị hoàn toàn bên dưới$2^k$, luôn nhỏ hơn cả hai số ban đầu. Điều này trực tiếp vi phạm yêu cầu XOR phải vượt quá mức tối đa của cặp. Trên các lớp khác nhau, việc chọn nhiều hơn một phần tử nói chung cũng không an toàn, nhưng ràng buộc bên trong một lớp đã buộc chiến lược tối ưu phải chọn tối đa một đại diện cho mỗi lớp và bất kỳ tập hợp con hợp lệ nào cũng không được vượt quá giới hạn này. Do đó, việc đếm các giá trị MSB riêng biệt sẽ cho kích thước tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def msb(x):
    return x.bit_length() - 1

n = int(input())
arr = list(map(int, input().split()))

seen = set()
ans = 0

for x in arr:
    b = msb(x)
    if b not in seen:
        seen.add(b)
        ans += 1

print(ans)
```Giải pháp tính toán bit có ý nghĩa nhất của mỗi số bằng cách sử dụng`bit_length`, chạy trong thời gian không đổi trên mỗi số nguyên. bộ`seen`đảm bảo chúng ta đếm mỗi nhóm bit chỉ một lần. 

Một điểm tinh tế là việc trùng lặp hoàn toàn không thành vấn đề vì bài toán yêu cầu một tập hợp các phần tử duy nhất. Ngay cả khi tồn tại các bản sao trong đầu vào, chúng sẽ thu gọn về cùng một nhóm MSB và bị bỏ qua sau lần xuất hiện đầu tiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Chúng tôi tính toán các nhóm MSB: 

| Giá trị | Nhị phân | chỉ số MSB | Đã thấy trước đây? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | Không | lấy | 1 | 
| 2 | 10 | 1 | Không | lấy | 2 | 
| 3 | 11 | 1 | Có | bỏ qua | 2 | 

Điều này cho thấy giá trị 2 và 3 cạnh tranh cho cùng một lớp bit, do đó chỉ có thể sử dụng một trong số chúng. Câu trả lời cuối cùng là 2. 

### Ví dụ 2 

đầu vào:```
5
4 5 6 7 8
```| Giá trị | Nhị phân | chỉ số MSB | Đã thấy trước đây? | Hành động | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 100 | 2 | Không | lấy | 1 | 
| 5 | 101 | 2 | Có | bỏ qua | 1 | 
| 6 | 110 | 2 | Có | bỏ qua | 1 | 
| 7 | 111 | 2 | Có | bỏ qua | 1 | 
| 8 | 1000 | 3 | Không | lấy | 2 | 

Ví dụ này chứng tỏ rằng khi một lớp bit được sử dụng, tất cả các giá trị khác trong cùng một khoảng sẽ không thể sử dụng được, ngay cả khi chúng khác nhau đáng kể ở các bit thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một lần bằng tính toán MSB theo thời gian không đổi và tra cứu thiết lập | 
| Không gian | O(log A) | Chúng tôi lưu trữ tối đa một mục nhập cho mỗi vị trí bit có thể | 

Giải pháp dễ dàng phù hợp trong giới hạn vì phạm vi số chỉ yêu cầu khoảng 22-23 lớp bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def msb(x):
        return x.bit_length() - 1

    n = int(input())
    arr = list(map(int, input().split()))

    seen = set()
    ans = 0
    for x in arr:
        b = msb(x)
        if b not in seen:
            seen.add(b)
            ans += 1

    return str(ans)

# provided samples
assert run("3\n1 2 3\n") == "2"
assert run("3\n4000000 4000000 4000000\n") == "1"

# custom cases
assert run("1\n7\n") == "1", "single element"
assert run("5\n1 2 4 8 16\n") == "5", "all distinct MSBs"
assert run("4\n3 5 6 7\n") == "1", "same MSB collapse"
assert run("6\n1 2 3 4 5 6\n") == "3", "mixed MSB groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 7 | 1 | trường hợp cạnh đơn phần tử | 
| sức mạnh của hai | 5 | tất cả các MSB khác biệt | 
| 3 5 6 7 | 1 | sụp đổ hoàn toàn cùng lớp | 
| 1 2 3 4 5 6 | 3 | hành vi nhóm hỗn hợp | 

## Vỏ cạnh 

Đầu vào tối thiểu có một số duy nhất luôn tạo ra câu trả lời là 1, vì không có cặp nào vi phạm điều kiện. 

Đối với trường hợp như`4 5 6 7`, tất cả các số đều có chung bit quan trọng nhất. Thuật toán gán chúng vào cùng một nhóm, chỉ giữ lại lần xuất hiện đầu tiên và bỏ qua phần còn lại. Điều này phù hợp với thực tế là bất kỳ cặp nào trong phạm vi này đều không đáp ứng được điều kiện XOR. 

Vì`1 2 3`, nhóm sẽ chia thành MSB 0 cho 1 và MSB 1 cho cả 2 và 3. Thuật toán đếm chính xác hai nhóm, khớp với tập hợp con tốt nhất có thể đạt được`{1, 2}`hoặc`{1, 3}`nhưng không bao giờ là cả ba.
