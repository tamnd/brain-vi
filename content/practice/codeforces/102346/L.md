---
title: "CF 102346L - Ít tung xu hơn"
description: "Chúng ta có tất cả các chuỗi nhị phân có độ dài (N), biểu thị kết quả có thể có của việc tung (N) cùng một đồng xu có thể bị sai lệch. Carla và Daniel phải chỉ định mọi chuỗi đã chọn cho tối đa một chuỗi trong số chúng, trong khi một số chuỗi có thể vẫn chưa được sử dụng."
date: "2026-08-14T02:08:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102346
codeforces_index: "L"
codeforces_contest_name: "2019-2020 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102346
solve_time_s: 90
verified: true
draft: false
---

[CF 102346L - Ít tung xu hơn](https://codeforces.com/problemset/problem/102346/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tất cả các chuỗi nhị phân có độ dài (N), biểu thị kết quả có thể có của việc tung (N) cùng một đồng xu có thể bị sai lệch. Carla và Daniel phải chỉ định mọi chuỗi đã chọn cho tối đa một chuỗi trong số chúng, trong khi một số chuỗi có thể vẫn chưa được sử dụng. Phép gán là công bằng nếu, với mọi độ lệch có thể có của đồng xu, tổng xác suất để có được chuỗi Carla bằng tổng xác suất để có được chuỗi Daniel. 

Nhiệm vụ là giảm thiểu số lượng chuỗi không sử dụng. Vấn đề chính thức có (2 \le N \le 10^{18}), với giới hạn thời gian một giây, do đó, một thuật toán phụ thuộc vào (2^N) chuỗi có thể là không thể. Ngay cả thuật toán (O(N)) cũng quá chậm trong trường hợp xấu nhất vì (N) có thể là (10^{18}). Chúng ta cần một giải pháp có thời gian chạy chỉ phụ thuộc vào số chữ số nhị phân của (N). 

Khó khăn chính là đồng xu không nhất thiết phải công bằng. Một chuỗi có (k) số đơn vị không có cùng xác suất với một chuỗi có số lượng đơn vị khác nhau. Tuy nhiên, mọi chuỗi có chính xác (k) đơn vị đều có xác suất như nhau. Có chính xác (\binom Nk) những chuỗi như vậy. 

Giả sử đồng xu tạo ra mặt sấp với xác suất (p). Một chuỗi chứa (k) số 1 và (N-k) số 0 có xác suất 

[ 
p^k(1-p)^{N-k}. 
] 

Gọi (c_k) là số chuỗi có (k) số chuỗi được gán cho Carla và (d_k) số tương ứng được gán cho Daniel. Công bằng có nghĩa là 

[ 
\sum_{k=0}^{N}(c_k-d_k)p^k(1-p)^{N-k}=0 
] 

cho mọi khả năng có thể (p). 

Các hàm (p^k(1-p)^{N-k}) tạo thành cơ sở bậc (N) Bernstein, do đó chúng độc lập tuyến tính. Do đó, 

[ 
c_k=d_k 
] 

với mọi (k). Đây là sự rút gọn trung tâm: các chuỗi có cùng số lượng đơn vị chỉ có thể được cân bằng với các chuỗi khác có cùng số lượng đơn vị. 

Đối với một (k) cố định, có sẵn các chuỗi (\binom Nk). Nếu số này là số chẵn thì chúng ta có thể đưa một nửa cho Carla và một nửa cho Daniel. Nếu nó là số lẻ thì không thể đếm số nguyên bằng nhau, vì vậy ít nhất một chuỗi phải không được sử dụng. Điều này mang lại 

[ 
\text{answer}=\sum_{k=0}^{N}\binom Nk\bmod 2. 
] 

Bài toán còn lại thuần tuý là tổ hợp: đếm số phần tử lẻ ở hàng (N) của tam giác Pascal. 

Việc triển khai bất cẩn có thể cố gắng tạo ra tất cả các chuỗi. Đối với (N=3), chỉ có tám chuỗi, vì vậy điều này có vẻ vô hại và đưa ra câu trả lời đúng (4). Nhưng đối với (N=60), đã có sẵn (2^{60}) chuỗi và đối với (N=10^{18}), con số này vượt xa mọi tính toán thực tế. 

Một trường hợp cạnh khác là (N=2). Các hệ số nhị thức là (1,2,1). Hai lớp bên ngoài, mỗi lớp chứa một chuỗi và không được sử dụng chuỗi đó, trong khi lớp giữa có thể được chia đều. Do đó, đầu ra đúng là (2). Một giải pháp chỉ đơn giản là chia mọi hệ số nhị thức cho hai sẽ thu được số 0 một cách không chính xác. 

Trường hợp ranh giới hữu ích là (N=8). Hàng của Pascal bắt đầu 

[ 
1,8,28,56,70,56,28,8,1. 
] 

Chỉ có hai hệ số bên ngoài là số lẻ, do đó chính xác hai chuỗi phải không được sử dụng. Đầu ra đúng là (2). Trường hợp này cũng đưa ra lời giải đếm sai hệ số lẻ khi (N) là lũy thừa của hai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực diễn ra trực tiếp từ việc giảm thiểu. Chúng ta có thể liệt kê tất cả (2^N) chuỗi nhị phân, đếm số chuỗi đơn vị của chúng và kiểm tra tính chẵn lẻ của tần số kết quả cho mỗi (k). Nếu số lượng đơn vị trong mỗi chuỗi được tính bằng cách kiểm tra các vị trí (N) của nó, thì việc kiểm tra bit cơ bản (\Theta(N2^N)) sẽ diễn ra. Ngay cả với cách liệt kê thông minh hơn và duy trì số lượng tăng dần, chỉ riêng trạng thái (2^N) đã khiến phương pháp này không khả thi. 

Lực lượng vũ phu hoạt động vì vấn đề đã được giảm xuống để đếm xem có bao nhiêu chuỗi tồn tại trong mỗi lớp trọng lượng Hamming. Nó thất bại vì số lượng chuỗi tăng theo cấp số nhân với (N).

Quan sát giúp mở ra giải pháp nhanh hơn là chúng ta thực sự không cần đến các hệ số nhị thức. Chúng ta chỉ cần biết mỗi hệ số có lẻ hay không. 

Hãy xem xét môđun tam giác Pascal (2). Số hệ số lẻ ở hàng (N) có dạng rất đơn giản: 

[ 
2^{\operatorname{popcount}(N)}, 
] 

trong đó (\operatorname{popcount}(N)) là số bit được đặt trong biểu diễn nhị phân của (N). 

Một cách để rút ra điều này là tính modulo (2). Viết 

[ 
N=2^{b_1}+2^{b_2}+\cdots+2^{b_r}, 
] 

trong đó (r=\operatorname{popcount}(N)). Trên modulo (2), 

[ 
(1+x)^{2^b}=1+x^{2^b}. 
] 

Như vậy 

\prod_{i=1}^{r}(1+x^{2^{b_i}}). 
] 

Khi sản phẩm được mở rộng, mọi yếu tố đều đóng góp (1) hoặc lũy thừa (x). Vì lũy thừa (2^{b_i}) là khác nhau nên mỗi tập hợp con đều tạo ra một số mũ riêng biệt. Có chính xác (2^r) tập hợp con, vì vậy chính xác (2^r) hệ số là số lẻ. 

Vì mỗi hệ số nhị thức lẻ buộc phải có một chuỗi không được sử dụng và mọi hệ số chẵn có thể được phân chia một cách hoàn hảo, nên số lượng chuỗi tối thiểu không được sử dụng chính xác là 

[ 
\boxed{2^{\operatorname{popcount}(N)}}. 
] 

Do đó, mối liên hệ giữa phương pháp tiếp cận bạo lực và tối ưu là khá trực tiếp. Brute Force hỏi xem mục nào trong hàng Pascal là số lẻ bằng cách xây dựng hàng ngầm thông qua tất cả các chuỗi. Giải pháp tối ưu bỏ qua toàn bộ hàng và đọc câu trả lời trực tiếp từ biểu diễn nhị phân của (N). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N2^N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(\log N)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N). Chúng ta chỉ cần biểu diễn nhị phân của nó, vì số lượng hệ số nhị thức lẻ phụ thuộc vào số lượng bit được đặt trong (N). 
2. Đếm số bit đã đặt của (N). Điều này mang lại (\operatorname{popcount}(N)). Trong Python,`int.bit_count()`thực hiện chính xác thao tác này. 
3. Tính toán (2^{\operatorname{popcount}(N)}). Mỗi bit tập hợp của (N) đóng góp một lựa chọn nhị phân độc lập trong việc khai triển modulo-(2) của ((1+x)^N), do đó (r) bit tập hợp tạo ra (2^r) hệ số lẻ. 
4. In kết quả. Số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn ngay cả khi (N=10^{18}). 

### Tại sao nó hoạt động 

Với mọi số có thể có (k) số đơn vị, tính công bằng đòi hỏi Carla và Daniel phải nhận được chính xác số chuỗi giống nhau từ lớp chứa (k) số đơn vị. Nếu (\binom Nk) chẵn thì lớp đó có thể được phân chia một cách hoàn hảo. Nếu là số lẻ thì một chuỗi nhất thiết không được sử dụng và một chuỗi không sử dụng là đủ. 

Do đó, giá trị tối ưu bằng số hệ số lẻ ở hàng (N) của tam giác Pascal. Danh tính modulo-(2) 

[ 
(1+x)^N=\prod_{i:\text{bit__i(N)=1}(1+x^{2^i}) 
] 

chứa chính xác hai lựa chọn cho mỗi tập bit của (N), tạo ra chính xác (2^{\operatorname{popcount}(N)}) số hạng riêng biệt. Do đó, chính xác là nhiều hệ số nhị thức là số lẻ và chính xác là nhiều chuỗi phải không được gán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(1 << n.bit_count())

if __name__ == "__main__":
    solve()
```Đầu vào là một số nguyên duy nhất, vì vậy`solve()`đọc nó trực tiếp và tính toán câu trả lời từ số bit thiết lập của nó. 

biểu thức`n.bit_count()`cho (\operatorname{popcount}(N)). biểu thức`1 << k`chính xác là (2^k), do đó nó tránh được phép tính lũy thừa dấu phẩy động và giữ cho phép tính hoàn toàn tích phân. 

Không có vòng lặp (N), không có cấu trúc tam giác Pascal và không có phép liệt kê các chuỗi nhị phân. Vì (N\le10^{18}), biểu diễn nhị phân của nó có tối đa 60 bit, do đó, ngay cả việc đếm rõ ràng các bit cũng chỉ mất vài chục lần lặp. 

Giới hạn trên cũng không gây tràn trong Python. Câu trả lời nhiều nhất là (2^{60}), vì một số nguyên lên tới (10^{18}) có tối đa 60 bit được đặt. Các số nguyên có độ chính xác tùy ý của Python xử lý trực tiếp việc này. 

## Ví dụ đã hoạt động 

Các mẫu chính thức lần lượt là (N=3), (N=5) và (N=8), với kết quả đầu ra (4), (4) và (2). 

Đối với mẫu đầu tiên, (N=3) có biểu diễn nhị phân (11_2), do đó nó chứa hai bit được đặt. 

| (N) | Nhị phân (N) | Số bit đặt | Trả lời | 
| --- | --- | --- | --- | 
| 3 | 11 | 2 | (2^2=4) | 

Hàng Pascal tương ứng là (1,3,3,1) và cả bốn hệ số đều là số lẻ. Do đó, mỗi lớp yêu cầu một chuỗi không sử dụng, tổng cộng có bốn chuỗi không sử dụng. 

Đối với mẫu thứ hai, (N=5) là (101_2), cũng có hai bit được đặt. 

| (N) | Nhị phân (N) | Số bit đặt | Trả lời | 
| --- | --- | --- | --- | 
| 5 | 101 | 2 | (2^2=4) | 

Hàng của Pascal là 

[ 
1,5,10,10,5,1. 
] 

Các hệ số lẻ là các mục đầu tiên, thứ hai, thứ năm và cuối cùng, vì vậy có bốn trong số chúng. Thuật toán thu được cùng một giá trị mà không cần xây dựng hàng. 

Đối với mẫu thứ ba, (N=8) là (1000_2), chỉ chứa một bit được đặt. 

| (N) | Nhị phân (N) | Số bit đặt | Trả lời | 
| --- | --- | --- | --- | 
| 8 | 1000 | 1 | (2^1=2) | 

Chỉ có hai hệ số nhị thức là số lẻ ở hàng thứ tám, đó là hai hệ số bên ngoài. Mọi lớp khác đều chứa số chuỗi chẵn và có thể được chia đều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log N)) | Việc đếm các bit đã đặt sẽ kiểm tra tối đa các chữ số nhị phân của (N). | 
| Không gian | (O(1)) | Chỉ số nguyên đầu vào và số nguyên kết quả được lưu trữ. | 

Đối với (N\le10^{18}), có tối đa 60 chữ số nhị phân. Do đó, thuật toán chỉ thực hiện một lượng công việc có kích thước không đổi trong phạm vi ràng buộc nhất định, thay vì chạm vào bất kỳ chuỗi nào trong số (2^N) có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_value(n: int) -> str:
    return str(1 << n.bit_count())

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        n = int(sys.stdin.readline())
        return solve_value(n) + "\n"
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("3\n") == "4\n", "sample 1"
assert run("5\n") == "4\n", "sample 2"
assert run("8\n") == "2\n", "sample 3"

# Minimum-size input
assert run("2\n") == "2\n", "minimum N"

# Maximum-size input
assert run("1000000000000000000\n") == "16777216\n", "maximum N"

# Power of two, only one set bit
assert run("16\n") == "2\n", "power of two"

# Three set bits
assert run("7\n") == "8\n", "three set bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`2`| Tối thiểu cho phép (N) và hàng Pascal không tầm thường nhỏ nhất | 
|`1000000000000000000`|`16777216`| Đầu vào có kích thước tối đa và xử lý số nguyên lớn | 
|`16`|`2`| lũy thừa của hai, trong đó chính xác một hệ số nhị thức ở mỗi đầu là số lẻ | 
|`7`|`8`| Ba bit được đặt và quy tắc (2^{\operatorname{popcount}(N)}) | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu (N=2), biểu diễn nhị phân là (10_2), do đó thuật toán đếm một bit đã đặt và trả về (2^1=2). Trực tiếp, hàng của Pascal là (1,2,1), cho hai hệ số lẻ. Hai lớp đơn tương ứng không thể được phân chia giữa những người chơi, vì vậy hai chuỗi phải không được sử dụng. 

Đối với (N=3), biểu diễn nhị phân là (11_2), cho hai bit được đặt và đầu ra (4). Các hệ số nhị thức là (1,3,3,1), đều là số lẻ. Mỗi lớp trọng số Hamming có số chuỗi lẻ, vì vậy mỗi lớp đóng góp chính xác một chuỗi không sử dụng. Điều này mang lại bốn chuỗi không sử dụng, khớp với mẫu. 

Đối với lũy thừa của hai chẳng hạn như (N=8), biểu diễn nhị phân chứa chính xác một bit được đặt. Câu trả lời là (2). Đây là câu trả lời nhỏ nhất có thể có cho bất kỳ (N) hợp lệ nào, bởi vì mỗi lớp (k=0) và (k=N) chứa chính xác một chuỗi và những chuỗi đó không bao giờ có thể được gán đồng thời cho cả hai người chơi. 

Đối với đầu vào tối đa (N=10^{18}), thuật toán không bao giờ cố gắng xây dựng (10^{18}) chuỗi hoặc lặp lại (10^{18}) lần. Nó chỉ kiểm tra biểu diễn nhị phân. Số (10^{18}) có 24 bit được đặt, do đó kết quả là 

[ 
2^{24}=16,777,216. 
] 

Kết quả phù hợp thoải mái với kiểu số nguyên của Python và quá trình tính toán kết thúc sau khi chỉ xử lý khoảng 60 chữ số nhị phân của (N).
