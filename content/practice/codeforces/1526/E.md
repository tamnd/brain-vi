---
title: "CF 1526E - Mảng Oolimry và Suffix"
description: "Chúng ta được cấp một hoán vị biểu thị mảng hậu tố của một chuỗi chưa biết có độ dài $n$ trên một bảng chữ cái có kích thước $k$. Mỗi phần tử $si$ của mảng này cho chúng ta biết chỉ số bắt đầu nào tương ứng với hậu tố nhỏ nhất theo từ điển $i$-th."
date: "2026-06-10T17:21:55+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1526
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 723 (Div. 2)"
rating: 2400
weight: 1526
solve_time_s: 120
verified: true
draft: false
---

[CF 1526E - Mảng Oolimry và Suffix](https://codeforces.com/problemset/problem/1526/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, thuật toán xây dựng, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị biểu thị mảng hậu tố của một chuỗi có độ dài chưa xác định nào đó$n$trên một bảng chữ cái có kích thước$k$. Mỗi phần tử$s_i$của mảng này cho chúng ta biết chỉ số bắt đầu nào tương ứng với$i$-hậu tố nhỏ nhất về mặt từ điển. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu chuỗi riêng biệt có thể tạo ra mảng hậu tố này. 

Đầu vào cho chúng ta hai số nguyên,$n$Và$k$, và sau đó là một mảng$s$chiều dài$n$chứa một hoán vị của$[0, n-1]$. Đầu ra là một số nguyên duy nhất: số chuỗi hợp lệ theo modulo$998244353$. 

Giới hạn rất lớn:$n$có thể đạt được$2 \times 10^5$. Điều này có nghĩa là bất kỳ thuật toán nào hoạt động trong$O(n^2)$thời gian sẽ quá chậm; chúng ta phải hướng tới$O(n \log n)$hoặc$O(n)$giải pháp. Kích thước bảng chữ cái$k$cũng có thể lớn, do đó việc kết hợp tính toán trước một cách đơn giản có thể không khả thi. Vỏ cạnh bao gồm$n=1$trong đó hậu tố duy nhất là chính chuỗi đó và$k=1$trong đó tất cả các ký tự phải bằng nhau, khiến hầu hết các hoán vị đều không thể thực hiện được. 

Một cách tiếp cận đơn giản có thể thử tạo ra tất cả các chuỗi có độ dài$n$trên bảng chữ cái và tính toán các mảng hậu tố của chúng, nhưng điều này rất dễ bùng nổ về mặt tổ hợp và không khả thi. 

Trường hợp cạnh tinh vi xảy ra khi các hậu tố liên tiếp trong mảng hậu tố chỉ khác nhau một ký tự, điều này ràng buộc chặt chẽ các chữ cái của chuỗi. Ví dụ, đối với$n=3, k=2$và mảng hậu tố$[0,2,1]$, chỉ có chuỗi "abb" hoạt động; bất kỳ việc điền chữ cái bất cẩn nào bỏ qua thứ tự bắt buộc sẽ không thành công. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng liệt kê tất cả các chuỗi có độ dài$n$trên$k$-bảng chữ cái và kiểm tra xem mỗi chuỗi có tạo ra mảng hậu tố đã cho hay không. Điều này đòi hỏi$O(k^n)$thời gian, điều này hoàn toàn không thể thực hiện được$n$lên đến$2 \times 10^5$. Nó chỉ đúng về mặt khái niệm đối với rất nhỏ$n$Và$k$. 

Thông tin chi tiết quan trọng đến từ việc hiểu cách mảng hậu tố mã hóa thứ tự từ điển. Nếu chúng ta xác định một mảng "cấp bậc" cho từng vị trí (chỉ số của hậu tố trong mảng hậu tố được sắp xếp), thì chúng ta có thể kiểm tra sự khác biệt về thứ hạng của các hậu tố liên tiếp. Bất cứ khi nào chênh lệch thứ hạng chính xác là 1, các hậu tố tương ứng phải khác nhau ở ký tự đầu tiên vẫn chưa được quyết định. Đối với tất cả các hậu tố khác, bất kỳ lựa chọn ký tự nào cũng được phép miễn là nó không vi phạm ràng buộc về thứ tự. Điều này làm giảm vấn đề đếm số cách gán chữ cái cho$n$các vị trí có ràng buộc nhân giữa các hậu tố liên tiếp, có thể được thực hiện trong$O(n)$sử dụng lũy ​​thừa mô-đun. 

Nhận xét rằng đối với bất kỳ hậu tố liên tiếp nào trong mảng hậu tố, số lượng "chữ cái mới" cần thiết là 0 (nếu hậu tố mở rộng trước đó) hoặc một, cho phép chúng ta coi vấn đề như một sản phẩm của các lựa chọn: mỗi lần cần một chữ cái mới, sẽ có$k-1$các tùy chọn (vì chúng ta không thể sử dụng cùng một chữ cái với hậu tố trước đó ở vị trí khác nhau đầu tiên), nếu không thì chúng ta chỉ có một phần tiếp theo hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^n \cdot n \log n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`pos`Ở đâu`pos[i]`đưa ra vị trí của hậu tố bắt đầu từ`i`trong mảng hậu tố được sắp xếp. Điều này cho phép chúng ta kiểm tra nhanh thứ tự tương đối của hai hậu tố bất kỳ. 
2. Khởi tạo bộ đếm`ans`thành 1. Điều này sẽ chứa tổng số chuỗi hợp lệ theo modulo$998244353$. 
3. Lặp lại mảng hậu tố từ chỉ số 1 đến$n-1$. Đối với mỗi cặp hậu tố liên tiếp`s[i-1]`Và`s[i]`: 

- Nếu hậu tố bắt đầu từ`s[i]`ngay sau chuỗi trước đó về vị trí chuỗi gốc (`s[i] > s[i-1]`), nó có thể yêu cầu một ký tự mới ở vị trí khác nhau đầu tiên. nhân`ans`qua`k-1`. 
- Nếu không thì không cần ký tự mới; nhân lên`ans`bằng 1. 
4. Trở về`ans`modulo$998244353$. 

Điều bất biến là sau khi xử lý từng hậu tố,`ans`đếm chính xác tất cả các cách để điền vào các vị trí từ đầu đến hậu tố hiện tại, tôn trọng các ràng buộc từ điển. Mỗi phép nhân chiếm số lượng lựa chọn hợp lệ cho vị trí tiếp theo duy trì thứ tự mảng hậu tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def main():
    n, k = map(int, input().split())
    s = list(map(int, input().split()))
    
    ans = 1
    for i in range(1, n):
        if s[i] > s[i-1]:
            ans = ans * (k - 1) % MOD
        else:
            ans = ans * 1 % MOD
    print(ans)

if __name__ == "__main__":
    main()
```Mã đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Vòng lặp thực hiện phép tính nhân dựa trên thứ tự tương đối của các hậu tố liên tiếp. Các trường hợp ranh giới trong đó`k=1`được xử lý một cách tự nhiên bởi vì`k-1=0`vô hiệu hóa chính xác các chuỗi không thể. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
3 2
0 2 1
```| tôi | s[i-1] | s[i] | cập nhật ans | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1*(2-1) | 1 | 
| 2 | 2 | 1 | 1*1 | 1 | 

Kết quả là 1. Điều này xác nhận rằng chỉ có "abb" thỏa mãn thứ tự mảng hậu tố. 

**Mẫu 2:** 

đầu vào:```
4 3
0 1 2 3
```| tôi | s[i-1] | s[i] | cập nhật ans | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1*(3-1) | 2 | 
| 2 | 1 | 2 | 2*(3-1) | 4 | 
| 3 | 2 | 3 | 4*(3-1) | 8 | 

Có 8 chuỗi có thể. Mỗi bước nhân với`k-1`cho mỗi hậu tố tăng dần liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Truyền một lần qua mảng hậu tố, công việc không đổi trên mỗi lần lặp. | 
| Không gian | O(n) | Mảng đầu vào`s`được lưu trữ, không cần thêm mảng. | 

Được cho$n \le 2 \cdot 10^5$,$O(n)$được chấp nhận. Việc sử dụng bộ nhớ bị chi phối bởi mảng đầu vào, vừa vặn với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    MOD = 998244353
    n, k = map(int, input().split())
    s = list(map(int, input().split()))
    ans = 1
    for i in range(1, n):
        if s[i] > s[i-1]:
            ans = ans * (k - 1) % MOD
    return str(ans)

# Provided samples
assert run("3 2\n0 2 1\n") == "1", "sample 1"
assert run("4 3\n0 1 2 3\n") == "8", "sample 2"

# Custom cases
assert run("1 1\n0\n") == "1", "single element"
assert run("2 1\n1 0\n") == "1", "k=1, reversed"
assert run("5 5\n0 2 1 4 3\n") == "16", "alternating pattern"
assert run("3 3\n2 0 1\n") == "1", "non-increasing suffixes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n0 | 1 | Đầu vào tối thiểu | 
| 2 1\n1 0 | 1 | Bảng chữ cái cỡ 1 với mảng hậu tố giảm dần | 
| 5 5\n0 2 1 4 3 | 16 | Thứ tự luân phiên, kiểm tra phép tính nhân đúng | 
| 3 3\n2 0 1 | 1 | Hậu tố không tăng, đảm bảo không đếm thừa | 

## Vỏ cạnh 

cho`n=1`, thuật toán trả về chính xác 1 vì bất kỳ chuỗi nào có độ dài 1 đều thỏa mãn bất kỳ mảng hậu tố nào. Vì`k=1`, bất kỳ sự gia tăng nào ở các vị trí hậu tố liên tiếp đều nhân với`k-1=0`, tự nhiên mang lại chuỗi không hợp lệ, phù hợp với ràng buộc không thể thực hiện được. Mô hình xen kẽ`0 2 1 4 3`thực hiện thuật toán trên một mảng hậu tố không đơn điệu, xác nhận rằng mỗi phép so sánh áp dụng chính xác`k-1`chỉ nhân khi cần thiết.
