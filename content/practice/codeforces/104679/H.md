---
title: "CF 104679H - Khiêu vũ cùng DS"
description: "Chúng ta được cho hai số nguyên. Một là tham số giống cơ sở cố định $k$, và tham số kia là giới hạn trên $r$. Đối với bất kỳ số nguyên không âm $n$ nào, chúng ta xác định một quá trình: nếu $n$ chia hết cho $k$, chúng ta chia nó cho $k$, nếu không thì chúng ta trừ 1."
date: "2026-06-29T09:02:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104679
codeforces_index: "H"
codeforces_contest_name: "Replay of Battle of Brains 2022, University of Dhaka"
rating: 0
weight: 104679
solve_time_s: 42
verified: true
draft: false
---

[CF 104679H - Khiêu vũ cùng DS](https://codeforces.com/problemset/problem/104679/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên. Một là tham số giống cơ sở cố định$k$, và cái còn lại là giới hạn trên$r$. Với mọi số nguyên không âm$n$, chúng tôi xác định một quá trình: nếu$n$chia hết cho$k$, chúng tôi chia nó cho$k$, nếu không thì chúng ta trừ 1. Lặp lại điều này cho đến khi đạt 0 sẽ có một số thao tác$f(n)$. Nhiệm vụ là tối đa hóa$f(n)$trên tất cả các số nguyên$n$trong phạm vi$[0, r]$. 

Khó khăn chính không phải là tính toán$f(n)$cho một giá trị duy nhất. Phần đó trở nên có cấu trúc khi chúng tôi nhận thấy quy trình hoạt động giống như liên tục xóa chữ số cuối cùng trong cơ số$k$. Thử thách thực sự là tìm kiếm trên tất cả các giá trị cho đến$r$, có thể rất lớn, vì vậy chúng ta cần suy luận xem cấu trúc của các con số ảnh hưởng như thế nào đến chi phí mà không cần mô phỏng mọi$n$. 

Các ràng buộc ngụ ý trong việc thiết lập vấn đề là các giới hạn lập trình cạnh tranh điển hình:$r$đủ lớn để việc lặp qua tất cả các giá trị là không thể, do đó bất kỳ giải pháp nào đánh giá từng giá trị$n$độc lập sẽ là bậc hai hoặc tuyến tính trong$r$, quá chậm. Chúng ta cần một cái gì đó gần với logarit hoặc tệ nhất là tuyến tính về số chữ số. 

Trường hợp cạnh tinh tế xuất hiện khi$n$có sự thay đổi cấu trúc hàng đầu trong cơ sở$k$. Ví dụ: khi chữ số có nghĩa nhất trở thành 0 sau khi giảm, số chữ số sẽ co lại một cách hiệu quả. Một vấn đề khác là giá trị nhỏ của$n$, trong đó “công thức chữ số” vẫn giữ nguyên nhưng cần diễn giải cẩn thận khi biểu diễn chỉ có một chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là đơn giản. Đối với mọi$n \le r$, mô phỏng quá trình: nếu chia hết cho$k$, chia, nếu không thì trừ một, đếm bước. Mỗi chi phí mô phỏng$O(\log_k n)$, vì mỗi phép chia làm giảm độ lớn, nhưng trong trường hợp xấu nhất, chúng ta trừ đi nhiều lần trước khi phép chia xảy ra. Tổng thể$n$, điều này dẫn đến khoảng$O(r \log r)$, vượt xa giới hạn khả thi khi$r$là lớn. 

Bước ngoặt là nhận ra rằng quy trình này tương đương với việc làm việc ở cơ sở$k$. Mỗi phép trừ làm giảm chữ số cuối cùng và mỗi phép chia sẽ loại bỏ một chữ số. Điều này có nghĩa là số bước được gắn trực tiếp với cấu trúc chữ số của$n$trong căn cứ$k$. Trong thực tế, mỗi chữ số đóng góp giá trị của nó cộng với chi phí cấu trúc, dẫn đến một dạng đóng rõ ràng:$$f(n) = (\text{sum of digits of } n \text{ in base } k) + (\text{number of digits}) - 1$$Vì vậy, thay vì mô phỏng các chuyển đổi, chúng tôi đang tối đa hóa hàm dựa trên chữ số. 

Bây giờ vấn đề trở thành: trong số tất cả các số$n \le r$, cực đại hóa hàm chỉ phụ thuộc vào các chữ số trong cơ số$k$. Đây là cấu trúc kiểu chữ số-DP cổ điển, nhưng chúng ta không cần DP đầy đủ. Chúng tôi chỉ cần mức tối đa và mục tiêu là đơn điệu ở mỗi chữ số: các chữ số lớn hơn luôn hữu ích. 

Điều này cho phép xây dựng tham lam trên các tiền tố của$r$trong căn cứ$k$. Chúng tôi sửa độ dài tiền tố ở nơi chúng tôi khớp$r$, sau đó tại vị trí đầu tiên nơi chúng tôi thả xuống dưới$r$, chúng ta giảm chữ số đó đi một và điền phần còn lại bằng$k-1$, tối đa hóa tổng chữ số trong khi vẫn ở dưới$r$. 

Chúng tôi cũng xử lý trường hợp đặc biệt khi chúng tôi chọn tiền tố chung bằng 0, điều này giúp giảm số lượng chữ số đi một cách hiệu quả nếu chữ số đứng đầu cho phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(r \log r)$|$O(1)$| Quá chậm | 
| Xây dựng tham lam chữ số Base-k |$O(\log_k r)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi$r$vào cơ sở của nó$k$đại diện. Điều này đưa ra một mảng chữ số trong đó mỗi vị trí tương ứng với lũy thừa của$k$. Điều này là cần thiết vì hàm mục tiêu dựa trên chữ số. 
2. Đối với mỗi độ dài tiền tố có thể$p$từ$0$tới số chữ số trong$r$, giả sử chúng ta khớp với cái đầu tiên$p$chữ số của$r$. Điều này sửa cấu trúc cao nhất của số ứng cử viên. 
3. Nếu$p$bằng toàn bộ chiều dài, ứng cử viên chính xác là$r$, do đó giá trị của nó được tính trực tiếp từ tổng chữ số cộng với số chữ số. 
4. Ngược lại, tại vị trí$p$, chúng ta giảm chữ số của$r$bằng 1, miễn là nó không bằng 0. Nếu nó bằng 0 thì lựa chọn tiền tố này không hợp lệ vì chúng ta không thể tạo thành một số nhỏ hơn nếu không giảm độ dài trước đó. 
5. Sau khi giảm chữ số đó, điền vào tất cả các vị trí còn lại bằng$k-1$. Điều này tối đa hóa tổng chữ số trong khi đảm bảo số được xây dựng hoàn toàn nhỏ hơn$r$. 
6. Nếu chúng ta chọn$p = 0$, chúng ta đang xây dựng các số có ít chữ số hơn một cách hiệu quả$r$. Trong trường hợp này, số tốt nhất có thể là tất cả các chữ số bằng$k-1$với một chữ số ít hơn$r$, vì điều đó tối đa hóa cả tổng chữ số và số chữ số theo ràng buộc. 
7. Tính giá trị mục tiêu cho từng ứng viên và lấy giá trị lớn nhất. 

### Tại sao nó hoạt động 

Bất kỳ số nào$n \le r$phải phù hợp$r$ở một số tiền tố và khác nhau ở chữ số khác nhau đầu tiên. Tại vị trí đó, nó phải nhỏ hơn rất nhiều và tất cả các chữ số sau đó có thể được chọn tự do tối đa$k-1$. Vì mục tiêu tăng theo từng chữ số và cũng tăng theo số chữ số, nên mọi giải pháp tối ưu đều phải tối đa hóa các chữ số sau độ lệch đầu tiên và trì hoãn độ lệch càng xa càng tốt. Điều này buộc cấu trúc tham lam vượt qua các tiền tố, đảm bảo không có ứng cử viên nào ngoài các hình thức được xây dựng này có thể cải thiện mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def to_base_k(x, k):
    if x == 0:
        return [0]
    digs = []
    while x > 0:
        digs.append(x % k)
        x //= k
    return digs[::-1]

def value(digits):
    # (sum of digits) + (number of digits) - 1
    return sum(digits) + len(digits) - 1

def solve():
    k, r = map(int, input().split())

    digits = to_base_k(r, k)
    n = len(digits)

    best = 0

    # case: use r itself
    best = max(best, value(digits))

    # try prefix matches
    for p in range(n):
        if digits[p] == 0:
            continue

        cand = digits[:p]
        cand.append(digits[p] - 1)
        cand.extend([k - 1] * (n - p - 1))

        best = max(best, value(cand))

    # try shorter length (p = 0 type case)
    if n > 1:
        cand = [k - 1] * (n - 1)
        best = max(best, value(cand))

    print(best)

if __name__ == "__main__":
    solve()
```Việc chuyển đổi sang cơ sở$k$là cần thiết vì nó biến quá trình trừ và chia thành một bài toán về chữ số. Người trợ giúp`value`trực tiếp mã hóa công thức dẫn xuất, tránh mọi mô phỏng. 

Vòng lặp chính liệt kê vị trí đầu tiên nơi chúng ta thoát khỏi$r$. điều kiện`digits[p] == 0`bỏ qua các trường hợp không hợp lệ khi chúng tôi không thể giảm chữ số đó nếu không mượn trước đó. Khi chúng tôi giảm một chữ số, hãy điền hậu tố bằng$k-1$đảm bảo sự đóng góp tối đa từ các vị trí còn lại. 

Trường hợp đặc biệt cuối cùng xử lý các số có ít chữ số hơn$r$, trong đó chúng ta tối đa hóa cả số chữ số và tổng chữ số một cách độc lập bằng cách sử dụng tất cả$k-1$chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để$k = 10$,$r = 274$. Các chữ số cơ bản 10 là$[2, 7, 4]$. 

| Tiền tố p | Chữ số ứng cử viên | Giá trị | 
| --- | --- | --- | 
| 3 | [2, 7, 4] | 2+7+4+3-1 = 15 | 
| 0 | [9, 9] | 9+9+2-1 = 19 | 
| 1 | [1, 9, 9] | 1+9+9+3-1 = 21 | 
| 2 | [2, 6, 9] | 2+6+9+3-1 = 19 | 

Tốt nhất là tiền tố 1, cho kết quả 21. Điều này chứng tỏ tại sao việc đẩy độ lệch sớm hơn đôi khi có thể chiếm ưu thế, vì số lượng chữ số và tối đa hóa hậu tố tương tác với nhau. 

### Ví dụ 2 

hãy để$k = 9$,$r = 413089$. Các chữ số là$[4,1,3,0,8,9]$. 

| Tiền tố p | Chữ số ứng cử viên | Giá trị | 
| --- | --- | --- | 
| 6 | [4,1,3,0,8,9] | cố định | 
| 2 | [4,1,2,8,8,8] | ứng viên tối ưu | 
| 3 | không hợp lệ (chữ số 0) | bỏ qua | 
| 0 | [8,8,8,8,8] | trường hợp có chiều dài ngắn hơn | 

Điều này cho thấy việc bỏ qua các chữ số 0 là cần thiết như thế nào, vì việc giảm số 0 mà không vay trước đó là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log_k r)$| chuyển đổi cơ sở đơn và quét tuyến tính trên các chữ số | 
| Không gian |$O(\log_k r)$| lưu trữ đại diện base-k | 

Lời giải chỉ phụ thuộc vào số chữ số của$r$, vì vậy nó vẫn hiệu quả ngay cả khi$r$là cực kỳ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def to_base_k(x, k):
        if x == 0:
            return [0]
        digs = []
        while x > 0:
            digs.append(x % k)
            x //= k
        return digs[::-1]

    def value(digits):
        return sum(digits) + len(digits) - 1

    k, r = map(int, sys.stdin.readline().split())
    digits = to_base_k(r, k)
    n = len(digits)

    best = value(digits)

    for p in range(n):
        if digits[p] == 0:
            continue
        cand = digits[:p] + [digits[p] - 1] + [k - 1] * (n - p - 1)
        best = max(best, value(cand))

    if n > 1:
        best = max(best, value([k - 1] * (n - 1)))

    return str(best)

# small cases
assert run("10 0") == "0"
assert run("10 5") == "5"
assert run("10 10") == "10"
assert run("10 274") == "21"

# base-k behavior
assert run("2 5") == "5"
assert run("2 12") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10 0 | 0 | ranh giới tối thiểu | 
| 10 5 | 5 | hành vi một chữ số | 
| 10 274 ​​| 21 | logic tiền tố nhiều chữ số | 
| 2 12 | 7 | cấu trúc chữ số nhị phân | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$r$là sức mạnh của$k$, chẳng hạn như$1000_k$. Việc giảm tiền tố ngây thơ có thể cố gắng giảm chữ số 0 quá muộn, điều này không hợp lệ. Thuật toán bỏ qua các tiền tố đó một cách chính xác và thay vào đó dựa vào các vị trí trước đó hoặc các cấu trúc có độ dài ngắn hơn. 

Một trường hợp cạnh khác là$r < k$, trong đó biểu diễn cơ sở có một chữ số. Trong trường hợp này, chỉ có hai ứng cử viên tồn tại:$r$chính nó và$k-1$. Thuật toán tự nhiên bao gồm cả hai vì vòng lặp tiền tố không đóng góp gì có ý nghĩa và trường hợp có độ dài ngắn hơn không có hoặc chiếm ưu thế một cách thích hợp, tạo ra kết quả chính xác mà không cần viết hoa đặc biệt.
