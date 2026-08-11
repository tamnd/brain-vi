---
title: "CF 102409B - Tổng Xor"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta được cấp một số nguyên (N) và chúng ta cần tính XOR theo bit của mọi số nguyên từ (1) đến (N): [ 1 oplus 2 oplus 3 oplus cdots oplus N. ] Thứ tự không quan trọng vì XOR có tính kết hợp và giao hoán."
date: "2026-08-11T06:34:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "B"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 636
verified: true
draft: false
---

[CF 102409B - Tổng Xor](https://codeforces.com/problemset/problem/102409/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số nguyên (N) và chúng tôi cần tính toán XOR theo bit của mọi số nguyên từ (1) đến (N): 

[ 
1 \oplus 2 \oplus 3 \oplus \cdots \oplus N. 
] 

Thứ tự không quan trọng vì XOR có tính kết hợp và giao hoán. Nhiệm vụ đơn giản là xác định giá trị của tiền tố XOR này cho tối đa (10^5) giá trị khác nhau của (N). 

Giới hạn trên (N \le 10^{18}) ngay lập tức loại trừ mọi thứ truy cập vào mọi số nguyên lên đến (N). Ngay cả đối với một trường hợp kiểm thử có kích thước tối đa, một vòng lặp trực tiếp sẽ thực hiện (10^{18}) thao tác XOR. Với (10^5) trường hợp kiểm thử, trường hợp xấu nhất về mặt lý thuyết sẽ đạt tới (10^{23}) phép tính, vượt xa giới hạn một giây có thể hỗ trợ. Chúng ta cần tính toán theo thời gian không đổi cho từng trường hợp thử nghiệm. 

Giá trị lớn của (N) cũng có nghĩa là tràn số nguyên có chiều rộng cố định sẽ là mối lo ngại trong các ngôn ngữ như C++, Java hoặc JavaScript nếu chọn loại không phù hợp. Số nguyên Python tự động tăng lên, do đó bản thân số học là an toàn. Câu trả lời nhiều nhất là lớn hơn (N) một chút, vì vậy nó vẫn nằm trong phạm vi được biểu thị bằng kiểu số nguyên của Python. 

Các giá trị nhỏ nhất bộc lộ một số lỗi phổ biến. Với (N=1), đáp án là (1), vì chỉ có một số:```
Input:
1
```

```
Output:
1
```Một công thức vô tình bắt đầu từ (0) hoặc sử dụng sai lớp cặn có thể bị lỗi ở đây. 

Với (N=2), câu trả lời là (1 \oplus 2=3):```
Input:
2
```

```
Output:
3
```Điều này nắm bắt các công thức coi trường hợp (N \equiv 2 \pmod 4) là 0. 

Với (N=3), ta có (1\oplus2\oplus3=0):```
Input:
3
```

```
Output:
0
```Đây là trường hợp ranh giới hữu ích vì tiền tố XOR đầu tiên trở thành số 0 ở đây. 

Việc chuyển đổi ở bội số của bốn cũng dễ bị xử lý sai. Với (N=4), 

[ 
1\oplus2\oplus3\oplus4=4, 
] 

vậy đáp án là (4) chứ không phải (0). Lý do đằng sau mô hình này là quan sát chính được sử dụng bởi giải pháp tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp rất đơn giản. Bắt đầu với bộ tích lũy bằng 0 và XOR mọi số nguyên từ (1) đến (N) vào nó. Sau khi xử lý (N), bộ tích lũy chính xác là giá trị được yêu cầu vì nó chứa mọi số trong phạm vi được yêu cầu một lần. 

Cách tiếp cận này đúng, nhưng thời gian chạy của nó là (O(N)) cho một trường hợp thử nghiệm. Khi (N=10^{18}), điều đó có nghĩa là lặp lại chính xác (10^{18}) vòng lặp cho một đầu vào. Với (10^5) trường hợp thử nghiệm đều có (N=10^{18}), tổng số lần lặp sẽ là (10^{23}). Giới hạn thời gian một giây khiến cách tiếp cận như vậy là không thể. 

Cấu trúc cứu chúng ta là hành vi của XOR trên các số nguyên liên tiếp. Xét XOR của các số nguyên từ (1) đến (N). Một số giá trị đầu tiên là 

[ 
1,\ 3,\ 0,\ 4,\ 1,\ 7,\ 0,\ 8,\ldots 
] 

Một mẫu xuất hiện ngay lập tức khi chúng ta nhóm (N) theo modulo còn lại (4): 

[ 
\operatorname{xor}(1\ldots N)= 
\bắt đầu{trường hợp} 
N & N\bmod4=0,\ 
1 & N\bmod4=1,\ 
N+1 & N\bmod4=2,\ 
0 & N\bmod4=3. 
\end{trường hợp} 
] 

Lý do điều này lặp lại cứ sau bốn vị trí đều xuất phát từ danh tính 

[ 
x\oplus(x+1)\oplus(x+2)\oplus(x+3)=0 
] 

khi (x) chia hết cho (4). Ví dụ, 

[ 
4\oplus5\oplus6\oplus7=0. 
] 

Do đó, các khối hoàn chỉnh gồm bốn số nguyên liên tiếp sẽ bị hủy trong XOR. Khi các khối hoàn chỉnh đó bị loại bỏ, chỉ còn lại 0, một, hai hoặc ba số và sự đóng góp của chúng chỉ phụ thuộc vào (N\bmod4). 

Phương pháp brute-force hoạt động vì nó xây dựng rõ ràng tiền tố XOR mỗi lần một số, nhưng không thành công vì (N) có thể lớn về mặt thiên văn. Quan sát thấy rằng mọi khối hoàn chỉnh gồm bốn số đều có XOR 0 cho phép chúng tôi loại bỏ gần như toàn bộ phạm vi và tính phần đóng góp còn lại từ (N\bmod4). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N)) cho mỗi trường hợp thử nghiệm | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (N) cho ca kiểm thử hiện tại và tính toán (N\bmod4). Chỉ phần còn lại này là cần thiết để xác định phần nào của mẫu XOR lặp lại mà chúng ta đang ở. 
2. Nếu (N\bmod4=0), trả về (N). Tiền tố có thể được chia thành các nhóm hoàn chỉnh gồm bốn và mỗi nhóm hoàn chỉnh hủy về 0 ngoại trừ phần đóng góp mẫu cuối cùng để lại (N). 
3. Nếu (N\bmod4=1), trả về (1). Sau khi hủy bỏ tất cả các nhóm bốn thành viên, phần đóng góp duy nhất còn lại có XOR bằng (1). 
4. Nếu (N\bmod4=2), trả về (N+1). Tiền tố còn lại kết thúc bằng hai số có XOR tạo ra giá trị này. 
5. Nếu (N\bmod4=3), trả về (0). Tiền tố ba số còn lại bị hủy hoàn toàn. 
6. Xuất ngay giá trị đã chọn và tiếp tục với ca kiểm thử tiếp theo. Mỗi trường hợp thử nghiệm đều độc lập nên không có trạng thái nào được duy trì giữa chúng. 

Bất biến cơ bản là sau khi loại bỏ mọi khối hoàn chỉnh gồm bốn số nguyên liên tiếp, đóng góp XOR của các khối bị loại bỏ đó bằng 0. Thông tin duy nhất còn lại là số phần tử trong khối cuối cùng chưa hoàn thiện, chính xác là (N\bmod4). Bốn trường hợp trên cung cấp XOR của tiền tố còn lại đó, do đó thuật toán luôn tạo ra cùng một giá trị như XOR rõ ràng mọi số nguyên từ (1) đến (N). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def xor_upto(n):
    r = n % 4

    if r == 0:
        return n
    if r == 1:
        return 1
    if r == 2:
        return n + 1
    return 0

def solve():
    t = int(input())

    answers = []
    for _ in range(t):
        n = int(input())
        answers.append(str(xor_upto(n)))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```các`xor_upto`hàm thực hiện trực tiếp bốn trường hợp từ thuật toán. Máy tính`n % 4`là đủ để chọn đáp án nên không có vòng lặp phụ thuộc vào độ lớn của (N). 

các`r == 0`trường hợp trả về (N) chính nó. Ranh giới này dễ bị nhầm lẫn với`r == 3`trường hợp, trong đó câu trả lời là số không. Ví dụ: (N=3) cho kết quả bằng 0, trong khi tăng (N) lên (4) cho kết quả là bốn. 

các`r == 2`trường hợp trả lại`n + 1`, vì vậy điều quan trọng là không được vô tình quay lại (N). Đối với (N=2), XOR được yêu cầu là (1\oplus2=3=N+1). 

Các số nguyên có độ chính xác tùy ý của Python tạo nên`n + 1`an toàn ngay cả ở (N=10^{18}). Không có phép nhân hoặc số học không cần thiết khác, do đó việc triển khai vẫn đơn giản và nhanh chóng. 

Giải pháp thu thập các câu trả lời và viết chúng một lần với`sys.stdout.write`. Với (10^5) trường hợp kiểm thử, điều này tránh được các thao tác đầu ra lặp lại không cần thiết trong khi vẫn giữ cho quá trình triển khai tương thích với kiểu nhập nhanh được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, trường hợp thử nghiệm (N=1) có phần dư (1) modulo (4). 

| (N) | (N\bmod4) | Trường hợp được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | (N\bmod4=1) | 1 | 

XOR thực tế chỉ đơn giản là (1), do đó công thức khớp với phép tính trực tiếp. 

Đối với (N=2), phép tính sẽ chuyển sang lớp dư lượng thứ hai. 

| (N) | (N\bmod4) | Trường hợp được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 2 | 2 | (N\bmod4=2) | 3 | 

Ở đây phép tính trực tiếp là (1\oplus2=3), chính xác là (N+1). 

Đối với mẫu thứ ba, (N=5). 

| (N) | (N\bmod4) | Trường hợp được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 5 | 1 | (N\bmod4=1) | 1 | 

Chúng ta cũng có thể thấy điều này trực tiếp: 

[ 
1\oplus2\oplus3\oplus4\oplus5. 
] 

Bốn số đầu tiên đóng góp số 0 dưới dạng một khối hoàn chỉnh, chỉ để lại (5) cùng với phần đóng góp mẫu của tiền tố. Tương tự, công thức đã thiết lập cho phần dư (1) cho (1). Đánh giá trực tiếp, 

[ 
1\oplus2=3,\qquad 
3\oplus3=0,\qquad 
0\oplus4=4,\qquad 
4\oplus5=1. 
] 

Vì vậy, đầu ra là (1). 

Dấu vết hữu ích thứ hai là (N=8), trong đó có hai nhóm hoàn chỉnh gồm bốn. 

| (N) | (N\bmod4) | Trường hợp được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 8 | 0 | (N\bmod4=0) | 8 | 

Trình tự có thể được nhóm lại thành ((1,2,3,4)) và ((5,6,7,8)). Mỗi nhóm bốn số tuân theo cùng một cấu trúc XOR và tiền tố hoàn chỉnh thông qua bội số của bốn có XOR bằng điểm cuối đó. Do đó, công thức trả về (8). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T)) | Mỗi trường hợp thử nghiệm yêu cầu một phép toán modulo và số lượng so sánh không đổi | 
| Không gian | (O(T)) | Việc triển khai lưu trữ các chuỗi đầu ra trước khi viết chúng | 

Bản thân phép tính sử dụng (O(1)) không gian bổ sung cho mỗi trường hợp thử nghiệm. Khoảng trống (O(T)) hiển thị ở trên chỉ đến từ việc lưu trữ tất cả các câu trả lời được định dạng. Với (T\le10^5), mức này nằm dưới giới hạn bộ nhớ 256 MB một cách thoải mái. Tổng thời gian chạy là tuyến tính theo số lượng trường hợp thử nghiệm thay vì theo giá trị của (N), do đó, ngay cả (10^5) đầu vào có (N=10^{18}) cũng có thể dễ dàng quản lý được. 

## Trường hợp thử nghiệm```python
import sys
import io

def xor_upto(n):
    r = n % 4

    if r == 0:
        return n
    if r == 1:
        return 1
    if r == 2:
        return n + 1
    return 0

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())
        answers.append(str(xor_upto(n)))

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()

        try:
            solve()
            return sys.stdout.getvalue()
        finally:
            sys.stdout = old_stdout
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample
assert run("3\n1\n2\n5\n") == "1\n3\n1", "sample 1"

# Minimum value
assert run("1\n1\n") == "1", "minimum input"

# Every residue class modulo 4
assert run("4\n2\n3\n4\n5\n") == "3\n0\n4\n1", "modulo-4 boundaries"

# Repeated equal values
assert run("5\n8\n8\n8\n8\n8\n") == "8\n8\n8\n8\n8\n", "repeated values"

# Maximum allowed value
assert run("1\n1000000000000000000\n") == "1000000000000000000", "maximum N"

# Larger boundary crossing
assert run("4\n7\n8\n9\n10\n") == "0\n8\n1\n11", "off-by-one boundaries"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n`|`1`| Đầu vào có kích thước tối thiểu và trường hợp (N\bmod4=1) | 
|`4\n2\n3\n4\n5\n`|`3\n0\n4\n1`| Tất cả bốn loại dư lượng và ranh giới liền kề | 
|`5\n8\n8\n8\n8\n8\n`|`8\n8\n8\n8\n8`| Nhiều trường hợp thử nghiệm có giá trị giống hệt nhau | 
|`1\n1000000000000000000\n`|`1000000000000000000`| Xử lý tối đa (N) và số nguyên lớn | 
|`4\n7\n8\n9\n10\n`|`0\n8\n1\n11`| Chuyển đổi qua bội số của bốn và từng lỗi một | 

## Vỏ cạnh 

Đầu vào tối thiểu là (N=1). Thuật toán tính toán (1\bmod4=1), chọn công thức thứ hai và trả về (1). Đầu vào và đầu ra chính xác là:```
Input:
1
1
```

```
Output:
1
```Điều này phát hiện các triển khai vô tình bao gồm số 0 trong phạm vi XOR hoặc sử dụng mẫu bắt đầu không chính xác. 

Kết quả 0 đầu tiên xảy ra tại (N=3). Ở đây (3\bmod4=3), do đó thuật toán trả về 0 ngay lập tức. Tính toán trực tiếp xác nhận điều đó: 

[ 
1\oplus2\oplus3=3\oplus3=0. 
] 

Như vậy:```
Input:
1
3
```

```
Output:
0
```Việc triển khai bất cẩn giả định câu trả lời phải là khẳng định hoặc chỉ kiểm tra xem (N) là chẵn hay lẻ, sẽ thất bại trong trường hợp này. 

Ranh giới tại (N=4) là một nguồn lỗi phổ biến khác. Vì (4\bmod4=0), thuật toán trả về (4). Đánh giá trực tiếp mang lại 

[ 
1\oplus2\oplus3\oplus4=0\oplus4=4. 
]

Vì thế:```
Input:
1
4
```

```
Output:
4
```Sự khác biệt giữa (N=3) và (N=4) chứng tỏ tại sao việc chỉ nhóm theo tính chẵn lẻ là không đủ. Các giá trị liên tiếp có thể có các câu trả lời hoàn toàn khác nhau mặc dù tính chẵn lẻ của chúng thay đổi có thể dự đoán được. 

Trường hợp (N=2) kiểm tra nhánh (N\bmod4=2). Thuật toán trả về (N+1=3), khớp 

[ 
1\oplus2=3. 
] 

Đầu vào chính xác là:```
Input:
1
2
```

```
Output:
3
```Điều này gây ra lỗi thường gặp khi trả về (N) cho mọi giá trị chẵn. 

Cuối cùng, giá trị tối đa (N=10^{18}) chia hết cho 4, vì (10^{18}\bmod4=0). Do đó, thuật toán trả về (10^{18}) mà không lặp qua bất kỳ số nào trong phạm vi:```
Input:
1
1000000000000000000
```

```
Output:
1000000000000000000
```Việc thực thi vẫn chỉ thực hiện một thao tác modulo và một nhánh. Đây chính xác là hành vi được yêu cầu bởi các ràng buộc, bởi vì công việc của thuật toán phụ thuộc vào số lượng trường hợp thử nghiệm chứ không phụ thuộc vào độ lớn của bản thân các con số.
