---
title: "CF 1475A - Ước số lẻ"
description: "Chúng ta được yêu cầu xác định xem một số nguyên $n$ có ước số lẻ lớn hơn một hay không. Nói cách khác, chúng ta muốn biết liệu có tồn tại một số lẻ $x 1$ chia $n$ đều hay không. Nếu $n$ chia hết cho một số như vậy thì câu trả lời là \"CÓ\"; nếu không thì là \"KHÔNG\"."
date: "2026-06-11T00:06:48+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1475
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 697 (Div. 3)"
rating: 900
weight: 1475
solve_time_s: 113
verified: true
draft: false
---

[CF 1475A - Ước số lẻ](https://codeforces.com/problemset/problem/1475/A) 

**Xếp hạng:** 900 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một số nguyên đã cho có$n$có ước số lẻ lớn hơn một. Nói cách khác, chúng ta muốn biết liệu có tồn tại số lẻ nào đó hay không$x > 1$đó là sự chia rẽ$n$đồng đều. Nếu như$n$chia hết cho số đó thì câu trả lời là "CÓ"; nếu không thì là "KHÔNG". 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một số nguyên duy nhất$n$. Lớn nhất có thể$n$có thể đạt được$10^{14}$, vượt xa giới hạn của một cách tiếp cận ngây thơ là thử mọi số lên đến$n$như một ước số tiềm năng. Cho rằng có thể có tới$10^4$trường hợp thử nghiệm, bất kỳ cách tiếp cận tuyến tính nào trong$n$sẽ rất chậm. Chúng tôi cần thứ gì đó hoạt động theo thời gian logarit hoặc không đổi cho mỗi truy vấn. 

Các trường hợp cạnh bao gồm số lượng nhỏ và lũy thừa của hai. Ví dụ, nếu$n = 2$, không có ước số lẻ lớn hơn một nên câu trả lời phải là "KHÔNG". Nếu như$n = 3$, bản thân nó là một số lẻ lớn hơn một, nên câu trả lời là "CÓ". Đối với sức mạnh của hai, chẳng hạn như$n = 16$, mọi ước số cũng là lũy thừa của hai và do đó là số chẵn; những thứ này sẽ trả về "KHÔNG". Việc xác định sai lũy thừa của hai là có ước số lẻ là một lỗi phổ biến trong cách triển khai đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là kiểm tra từng số nguyên từ 3 đến$n$(bỏ qua số chẵn) để xem nó có chia không$n$. Điều này hoạt động vì nếu bất kỳ số nào chia hết$n$, đó là số chia lẻ. Tuy nhiên, trường hợp xấu nhất xảy ra đối với$n = 10^{14}$, yêu cầu đại khái$5 \cdot 10^{13}$kiểm tra phân chia, điều này là hoàn toàn không khả thi. Điều này rõ ràng không vượt quá giới hạn thời gian. 

Quan sát quan trọng là một số nguyên$n$không có ước số lẻ nào lớn hơn 1 khi và chỉ nếu nó là lũy thừa của 2. Quyền hạn của hai là số có dạng$2^k$, và tất cả các ước của chúng đều là lũy thừa của hai, nghĩa là chúng đều chẵn ngoại trừ 1. Bất kỳ số nào khác$n$hoặc có thừa số lẻ hoặc có thể được phân tích thành số lẻ nhân với lũy thừa hai. Cái nhìn sâu sắc này làm giảm vấn đề để kiểm tra xem liệu$n$là sức mạnh của hai. 

Việc kiểm tra xem một số có phải là lũy thừa của hai hay không có thể được thực hiện một cách hiệu quả bằng thủ thuật bitwise. Đối với số nguyên dương$n$, nếu như$n \& (n-1) == 0$, sau đó$n$là sức mạnh của hai. Điều này hoạt động vì lũy thừa của hai có chính xác một bit được đặt ở dạng nhị phân. Nếu không, số này có hệ số lẻ lớn hơn một và chúng tôi trả về "CÓ". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Kiểm tra sức mạnh của hai bit | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Điều này xác định số lượng chúng ta cần xử lý tuần tự. 
2. Với mỗi test, hãy đọc số nguyên$n$. Chúng tôi sẽ kiểm tra xem liệu$n$có ước số lẻ lớn hơn một. 
3. Áp dụng phép thử lũy thừa hai bit: tính toán$n \& (n-1)$. Nếu giá trị này bằng 0,$n$là lũy thừa của hai và chúng ta xuất ra "NO" vì nó không có ước số lẻ nào lớn hơn một. 
4. Nếu$n \& (n-1)$khác 0, xuất ra "CÓ". Điều này chỉ ra rằng$n$không phải là lũy thừa thuần túy của hai, nghĩa là nó có ước số lẻ lớn hơn một. 

Tại sao nó hoạt động: lũy thừa của hai là số nguyên duy nhất trong đó tất cả các ước số không tầm thường đều là số chẵn. Bằng cách kiểm tra biểu diễn nhị phân, thủ thuật bitwise xác định trực tiếp thuộc tính này. Bất kỳ số nào không vượt qua được kiểm tra đều phải chứa một thừa số lẻ, đảm bảo ước số lẻ lớn hơn một. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n & (n - 1) == 0:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    main()
```Mã bắt đầu bằng cách đọc tất cả các đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi lặp qua từng trường hợp thử nghiệm và áp dụng kiểm tra theo bit. sử dụng`n & (n-1)`là rất quan trọng; một cách tiếp cận modulo ngây thơ như`n % 2`hoặc lặp lại các ước số sẽ không đáp ứng được các ràng buộc về thời gian. Mã xử lý các số rất lớn một cách chính xác vì số nguyên Python có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$n = 2$| n | n & (n-1) | Đầu ra | 
| --- | --- | --- | 
| 2 | 2 & 1 = 0 | KHÔNG | 

Số 2 là lũy thừa của 2 nên không có ước số lẻ nào lớn hơn 1. 

**Ví dụ 2:**$n = 3$| n | n & (n-1) | Đầu ra | 
| --- | --- | --- | 
| 3 | 3 & 2 = 2 | CÓ | 

Số 3 không phải là lũy thừa của 2 nên nó có ước số lẻ lớn hơn 1, chính nó là 3. 

**Ví dụ 3:**$n = 16$| n | n & (n-1) | Đầu ra | 
| --- | --- | --- | 
| 16 | 16 & 15 = 0 | KHÔNG | 

16 là lũy thừa của 2, tất cả các ước đều chẵn trừ 1 nên "KHÔNG" là đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi bằng cách sử dụng một thao tác bitwise duy nhất. | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng ngoài bộ nhớ đầu vào. | 

Với những hạn chế$t \le 10^4$Và$n \le 10^{14}$, giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. Mỗi thao tác đều ở mức tối thiểu nên ngay cả những thao tác đầu vào lớn nhất cũng hoàn thành ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("6\n2\n3\n4\n5\n998244353\n1099511627776\n") == "NO\nYES\nNO\nYES\nYES\nNO", "sample 1"

# Custom tests
assert run("2\n1\n8\n") == "NO\nNO", "minimum and power of two"
assert run("3\n9\n10\n15\n") == "YES\nYES\nYES", "odd numbers and non-powers-of-two"
assert run("2\n1024\n2048\n") == "NO\nNO", "large powers of two"
assert run("1\n99999999999999\n") == "YES", "large non-power-of-two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 8 | KHÔNG, KHÔNG | n nhỏ nhất và lũy thừa nhỏ của 2 | 
| 9, 10, 15 | CÓ, CÓ, CÓ | số không có lũy thừa của hai, một số lẻ | 
| 1024, 2048 | KHÔNG, KHÔNG | sức mạnh lớn của hai | 
| 99999999999999 | CÓ | một số rất lớn không phải là lũy thừa của hai | 

## Vỏ cạnh 

Thuật toán xử lý chính xác các số nhỏ nhất có thể. Vì$n = 2$, kiểm tra bitwise trả về 0, tạo ra "NO". Đối với các số lẻ như$n = 3$hoặc$n = 9$, thử nghiệm thất bại, xác định chính xác rằng tồn tại một ước số lẻ lớn hơn một. Đối với lũy thừa của hai, ngay cả những lũy ​​thừa rất lớn như$n = 2^{40}$, thang đo kiểm tra không lặp hoặc đệ quy, đảm bảo cả tốc độ và độ chính xác. Không có lỗi nào tồn tại vì thao tác theo bit phát hiện chính xác mô hình lũy thừa một bit của hai.
