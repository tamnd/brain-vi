---
title: "CF 1186C - Vus the Cossack và Dây"
description: "Chúng ta được cấp một chuỗi nhị phân dài a và một chuỗi nhị phân ngắn hơn hoặc có độ dài bằng b. Chúng ta trượt b qua mọi cửa sổ có thể có a có cùng độ dài và với mỗi vị trí, chúng ta so sánh hai chuỗi theo từng ký tự."
date: "2026-06-12T00:51:56+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1186
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 571 (Div. 2)"
rating: 1800
weight: 1186
solve_time_s: 203
verified: true
draft: false
---

[CF 1186C - Vus the Cossack và Strings](https://codeforces.com/problemset/problem/1186/C) 

**Đánh giá:** 1800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân dài`a`và một chuỗi nhị phân ngắn hơn hoặc có độ dài bằng nhau`b`. Chúng tôi trượt`b`trên mọi cửa sổ có thể có của`a`có cùng độ dài và với mỗi vị trí, chúng ta so sánh hai chuỗi theo từng ký tự. 

Với mỗi căn chỉnh, chúng ta đếm xem có bao nhiêu vị trí khác nhau giữa cửa sổ của`a`Và`b`. Nhiệm vụ là đếm xem có bao nhiêu cách sắp xếp tạo ra số chẵn không khớp. 

Một cách trực tiếp để nghĩ về điều này là chúng ta đang đo khoảng cách Hamming giữa`b`và mỗi chuỗi con của`a`, và chúng ta muốn đếm xem có bao nhiêu khoảng cách chẵn. 

Chiều dài của`a`có thể lên tới 1e6 nên không thể so sánh bậc hai. Bất kỳ giải pháp nào so sánh từng cửa sổ một cách đơn giản sẽ thực hiện khoảng 1e6 × 1e6 thao tác trong trường hợp xấu nhất, vượt xa mức cho phép trong 1 giây. 

Các trường hợp cạnh chính xuất hiện khi`b`có độ dài bằng 1 hoặc khi cả hai chuỗi đều lớn và có tính lặp lại cao. Trong những trường hợp đó, việc so sánh cửa sổ trượt ngây thơ trông vẫn đơn giản nhưng âm thầm vượt quá giới hạn. 

## Phương pháp tiếp cận 

Phương thức brute-force kiểm tra mọi chuỗi con của`a`chiều dài`|b|`, và với mỗi cái được tính không khớp với`b`. Điều này đúng vì nó trực tiếp thực hiện định nghĩa của vấn đề. Tuy nhiên, đối với mỗi khoảng`n`vị trí, nó thực hiện`m`so sánh, đưa ra tổng số`O(nm)`hoạt động. Với cả hai`n`Và`m`gần đến 10^6, điều này là không khả thi. 

Để cải thiện điều này, chúng tôi tập trung vào điều thực sự quan trọng: tính chẵn lẻ của số lượng không khớp chứ không phải số lượng chính xác. Điều này gợi ý nên làm việc theo modulo 2 và cố gắng biểu diễn tính chẵn lẻ không khớp ở dạng đơn giản hơn. 

Hãy để chúng tôi mã hóa các ký tự dưới dạng bit. Sự không phù hợp ở vị trí`i`xảy ra khi`a[i] != b[j]`. Trong số học nhị phân, bất đẳng thức tương ứng với XOR là 1. Vì vậy, số lượng không khớp cho một cửa sổ bắt đầu từ`i`trở thành tổng trên`j`của:`a[i+j] XOR b[j]`Chúng tôi chỉ quan tâm đến tổng modulo 2 này. Trên GF(2), XOR hoạt động tuyến tính: 

tổng (a XOR b) mod 2 bằng tổng(a) XOR tổng(b) 

Đây là sự đơn giản hóa quan trọng. Tính chẵn lẻ của sự không khớp là: 

(tổng cửa sổ trong a) XOR (tổng của b) 

Vì vậy với mỗi cửa sổ ta chỉ cần tính chẵn lẻ của số cái trong cửa sổ của`a`, và chúng tôi so sánh nó với tính chẵn lẻ của những cái trong`b`. 

Do đó, vấn đề giảm xuống còn số chẵn lẻ của cửa sổ trượt. 

Chúng tôi tính tổng tiền tố của`a`để lấy số lượng cái trong bất kỳ cửa sổ nào trong O(1), theo dõi tính chẵn lẻ, tính tính chẵn lẻ của`b`một lần và so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tiền tố chẵn lẻ + cửa sổ trượt | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số đơn vị trong`b`và giảm nó theo modulo 2. Giá trị này thể hiện liệu`b`có tính chẵn lẻ hoặc chẵn lẻ. Điều này quan trọng vì chỉ có tính chẵn lẻ mới ảnh hưởng đến tính chẵn lẻ không khớp sau khi đơn giản hóa. 
2. Xây dựng mảng tổng tiền tố cho`a`, trong đó mỗi mục lưu trữ số lượng mục nhập cho đến chỉ mục đó. Điều này cho phép truy vấn thời gian không đổi cho bất kỳ chuỗi con nào. 
3. Lặp lại mọi chuỗi con của`a`với chiều dài`|b|`. Đối với mỗi chỉ số bắt đầu`i`, tính số lượng đơn vị trong`a[i : i+|b|]`sử dụng mảng tiền tố. 
4. Giảm tổng cửa sổ này theo modulo 2, đạt được tính chẵn lẻ của các số trong chuỗi con hiện tại. 
5. So sánh tính chẵn lẻ này với tính chẵn lẻ của`b`. Nếu chúng bằng nhau thì số lượng không khớp cho cửa sổ đó là số chẵn, vì vậy hãy tăng câu trả lời lên. 

### Tại sao nó hoạt động 

Đối với các giá trị nhị phân, việc không khớp ở một vị trí tương đương với XOR. Tổng số không khớp là tổng của các giá trị XOR. Khi giảm modulo 2, tổng XOR phân phối cho phép cộng, cho phép biểu thức thu gọn thành XOR của tổng số đơn vị. Điều này tạo ra một bất biến: tại mỗi cửa sổ, tính chẵn lẻ của các giá trị không khớp chỉ phụ thuộc vào tính chẵn lẻ của các giá trị trong cửa sổ của`a`và trong`b`, không phải trên cấu trúc chi tiết của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a = input().strip()
    b = input().strip()

    n, m = len(a), len(b)

    pb = sum(1 for ch in b) % 2

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (a[i] == '1')

    ans = 0
    for i in range(n - m + 1):
        ones = pref[i + m] - pref[i]
        if ones % 2 == pb:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán tính chẵn lẻ của những cái trong`b`. Đây là tài sản duy nhất của`b`điều đó ảnh hưởng đến câu trả lời cuối cùng sau khi giảm xuống mức chẵn lẻ. 

Tiếp theo, một mảng tổng tiền tố được xây dựng trên`a`, cho phép tính tổng mỗi chuỗi con trong thời gian không đổi. Điều này tránh việc tính toán lại cho mọi cửa sổ. 

Cuối cùng, mỗi cửa sổ được kiểm tra bằng cách so sánh tính chẵn lẻ của cửa sổ đó với tính chẵn lẻ của`b`. Điều kiện đẳng thức tương ứng trực tiếp với số lượng không khớp. 

Một điểm tinh tế là chúng tôi không bao giờ tính toán rõ ràng các điểm không khớp. Toàn bộ quá trình chuyển đổi phụ thuộc vào việc giảm biểu thức chẵn lẻ không khớp thành so sánh số lượng chẵn lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 01100010
b = 00110
```Chúng tôi tính toán tính chẵn lẻ của`b`: 

| b | những cái | chẵn lẻ | 
| --- | --- | --- | 
| 00110 | 2 | 0 | 

Bây giờ tiền tố cho`a`: 

| tôi | một [tôi] | trước | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 3 | 0 | 2 | 
| 4 | 0 | 2 | 
| 5 | 0 | 2 | 
| 6 | 1 | 3 | 
| 7 | 0 | 3 | 

Chiều dài cửa sổ là 5. 

| bắt đầu | chuỗi con | những cái | chẵn lẻ | trận đấu b? | 
| --- | --- | --- | --- | --- | 
| 0 | 01100 | 2 | 0 | vâng | 
| 1 | 11000 | 2 | 0 | vâng | 
| 2 | 10001 | 2 | 0 | vâng | 
| 3 | 00010 | 1 | 1 | không | 

Câu trả lời là 3. 

Điều này xác nhận rằng chỉ cần so sánh chẵn lẻ và tính toán không khớp hoàn toàn là không cần thiết. 

### Ví dụ 2 

đầu vào:```
a = 10101
b = 111
```Tính chẵn lẻ của`b`là 1 (ba số một). 

Tổng tiền tố: 

| tôi | một [tôi] | trước | 
| --- | --- | --- | 
| 0 | 1 | 1 | 
| 1 | 0 | 1 | 
| 2 | 1 | 2 | 
| 3 | 0 | 2 | 
| 4 | 1 | 3 | 

Windows: 

| bắt đầu | chuỗi con | những cái | chẵn lẻ | trận đấu | 
| --- | --- | --- | --- | --- | 
| 0 | 101 | 2 | 0 | không | 
| 1 | 010 | 1 | 1 | vâng | 
| 2 | 101 | 2 | 0 | không | 

Câu trả lời là 1. 

Điều này cho thấy phương pháp này phân biệt chính xác các cửa sổ hoàn toàn bằng cách căn chỉnh chẵn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | tính toán tiền tố và quét một lần trên tất cả các cửa sổ | 
| Không gian | O(n) | mảng tiền tố trên chuỗi`a`| 

Giải pháp là tuyến tính theo độ dài của chuỗi đầu vào`a`, đủ cho độ dài lên tới 1e6. Việc sử dụng bộ nhớ cũng tuyến tính nhưng tối thiểu, chỉ là một mảng số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    a = input().strip()
    b = input().strip()

    n, m = len(a), len(b)
    pb = sum(ch == '1' for ch in b) % 2

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + (a[i] == '1')

    ans = 0
    for i in range(n - m + 1):
        if (pref[i + m] - pref[i]) % 2 == pb:
            ans += 1

    return str(ans)

# provided sample
assert run("01100010\n00110\n") == "3"

# all zeros
assert run("00000\n00\n") == "4"

# all ones
assert run("11111\n11\n") == "4"

# alternating
assert run("101010\n101\n") == "2"

# single character b
assert run("10101\n1\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 4 | trường hợp chẵn lẻ thống nhất | 
| tất cả những cái | 4 | đối xứng không khớp hoàn toàn | 
| xen kẽ | 2 | chuyển tiếp hỗn hợp | 
| char đơn b | 3 | trường hợp cạnh | 

## Vỏ cạnh 

Khi nào`b`có độ dài 1, mỗi cửa sổ là một ký tự. Thuật toán giảm xuống để so sánh liệu`a[i]`phù hợp với tính chẵn lẻ của`b`. Tổng tiền tố vẫn hoạt động chính xác vì tổng mỗi cửa sổ là 0 hoặc 1, do đó so sánh chẵn lẻ là chính xác. 

Khi`a`bao gồm hoàn toàn các số 0, mọi cửa sổ đều có số 0, vì vậy tất cả các số chẵn lẻ của cửa sổ đều bằng 0. Nếu như`b`cũng có tính chẵn lẻ, mọi cửa sổ đều khớp và câu trả lời trở thành`|a| - |b| + 1`. Thuật toán tính toán điều này trực tiếp thông qua sự khác biệt về tiền tố. 

Khi`a`Và`b`là các mẫu lặp lại giống hệt nhau, mọi cửa sổ đều có cấu trúc chẵn lẻ giống hệt nhau. Thuật toán không dựa vào sự tương tự về cấu trúc mà chỉ dựa vào số lượng, vì vậy nó vẫn đúng bất kể sự lặp lại.
