---
title: "CF 104828A - \u9b54\u6cd5\u7ec3\u4e60"
description: "Chúng tôi được cung cấp một danh sách các số nguyên và giá trị mô đun. Nhiệm vụ là tính tích của tất cả các số trong danh sách rồi xuất ra phần dư khi chia tích đó cho mô đun đã cho."
date: "2026-06-28T12:26:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104828
codeforces_index: "A"
codeforces_contest_name: "The 11-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 104828
solve_time_s: 41
verified: true
draft: false
---

[CF 104828A - \u9b54\u6cd5\u7ec3\u4e60](https://codeforces.com/problemset/problem/104828/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên và giá trị mô đun. Nhiệm vụ là tính tích của tất cả các số trong danh sách rồi xuất ra phần dư khi chia tích đó cho mô đun đã cho. 

Mặc dù câu lệnh coi đây là một bài học “huấn luyện kỳ ​​diệu” về các kiểu dữ liệu và tràn dữ liệu, nhưng việc tính toán thực tế rất đơn giản: nhân tất cả các phần tử với nhau, nhưng mỗi bước trung gian phải nằm trong giới hạn số an toàn và kết quả cuối cùng phải được thực hiện theo modulo$p$. 

Hạn chế chính đó là$n$có thể lớn như$10^5$. Điều này ngay lập tức loại trừ mọi cách tiếp cận tính toán lại sản phẩm nhiều lần hoặc sử dụng các vòng lặp lồng nhau, vì$O(n^2)$công việc sẽ rất chậm. Một đường chuyền tuyến tính duy nhất là cấu trúc hợp lý duy nhất. 

Ngoài ra còn có một hạn chế về số lượng tinh tế. Sản phẩm có tới$10^5$các con số, mỗi con số có thể gần bằng$10^9$, có thể dễ dàng tràn số nguyên 64 bit nếu chúng ta bất cẩn. Mặc dù mỗi$a_i < p$, Và$p \le 10^9$, nhân nhiều giá trị như vậy mà không giảm mô-đun sẽ vượt quá$2^{63}-1$rất nhanh chóng. Do đó, việc triển khai ngây thơ làm trì hoãn modulo cho đến khi kết thúc sẽ tạo ra kết quả không chính xác hoặc các vấn đề về thời gian chạy. 

Các trường hợp cạnh chủ yếu tập trung vào hành vi số học mô-đun: 

Nếu bất kỳ phần tử nào bằng 0 thì toàn bộ tích sẽ trở thành 0 ngay lập tức và việc tiếp tục nhân là không cần thiết. Một trường hợp góc khác là khi$p = 1$. Trong trường hợp này, mọi số modulo 1 đều bằng 0, vì vậy câu trả lời luôn bằng 0 bất kể mảng nào. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là tính tích trực tiếp và sau đó áp dụng modulo$p$. Về mặt khái niệm thì điều này đúng: phép nhân có tính kết hợp, do đó tính toán$a_1 \cdot a_2 \cdots a_n$đầu tiên và giảm ở cuối sẽ tạo ra số dư đúng. 

Điểm thất bại là sự tăng trưởng về số lượng. Ngay cả với số nguyên 64 bit, phép nhân lặp lại nhanh chóng vượt quá phạm vi có thể biểu thị. Vì$n = 10^5$, sự tăng trưởng trong trường hợp xấu nhất có cường độ theo cấp số nhân và tình trạng tràn xảy ra rất lâu trước bước cuối cùng. Điều này làm cho phương pháp brute-force không đáng tin cậy trong môi trường số nguyên có chiều rộng cố định tiêu chuẩn. 

Quan sát quan trọng là số học mô-đun cho phép rút gọn ở mỗi bước mà không làm thay đổi kết quả cuối cùng. Từ$$(x \cdot y) \bmod p = ((x \bmod p) \cdot (y \bmod p)) \bmod p,$$chúng ta có thể duy trì một sản phẩm đang chạy theo modulo$p$, đảm bảo rằng các giá trị không bao giờ vượt quá$p$. Điều này biến vấn đề thành một lần quét tuyến tính duy nhất với các cập nhật liên tục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) số học nhưng không an toàn do tràn | O(1) | Sai trong thực tế | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một biến đang chạy`ans`lưu trữ sản phẩm của tất cả các phần tử được xử lý theo modulo$p$. 

1. Khởi tạo`ans = 1`. Điều này đại diện cho tích rỗng, là phần tử trung lập cho phép nhân. Chúng ta chọn 1 vì nhân với 1 không thay đổi bất kỳ giá trị nào. 
2. Lặp qua từng phần tử`a_i`trong mảng. 
3. Trước khi nhân, rút ​​gọn phần tử theo modulo$p$nếu muốn. Trong vấn đề này nó là tùy chọn vì$a_i < p$, nhưng việc giữ thói quen sẽ đảm bảo tính đúng đắn trong bối cảnh tổng quát. 
4. Cập nhật sản phẩm đang chạy dưới dạng`ans = (ans * a_i) % p`. Bước này là cốt lõi của giải pháp. Chúng tôi giảm ngay lập tức sau khi nhân để tránh tràn và giữ giá trị giới hạn bởi$p$. 
5. Sau khi xử lý tất cả các phần tử, xuất ra`ans`. 

Logic không bao giờ yêu cầu xem lại các phần tử trước đó, vì vậy việc tính toán hoàn toàn chỉ diễn ra một lần. 

### Tại sao nó hoạt động 

Tại mỗi lần lặp,`ans`đại diện cho tích của tất cả các phần tử trước đó theo modulo$p$. Khi chúng ta nhân với phần tử tiếp theo và giảm modulo$p$, chúng tôi bảo toàn bất biến này do tính chất phân phối của số học mô-đun. Vì bất biến giữ nguyên từ phần tử đầu tiên đến phần tử cuối cùng nên giá trị cuối cùng chính xác là tích của tất cả các phần tử theo modulo$p$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, p = map(int, input().split())
    arr = list(map(int, input().split()))
    
    ans = 1 % p
    for x in arr:
        ans = (ans * (x % p)) % p
    
    print(ans)

if __name__ == "__main__":
    main()
```Mã theo sau bất biến trực tiếp. Biến`ans`luôn được giữ ở mức modulo giảm`p`, đảm bảo nó không bao giờ phát triển lớn. Mặc dù phép nhân số nguyên Python không bị tràn, cấu trúc này vẫn rất cần thiết trong các ngôn ngữ như C++ và phù hợp với các ràng buộc của bài toán dự kiến. 

Việc khởi tạo`1 % p`đảm bảo tính đúng đắn khi$p = 1$, vì câu trả lời khi đó phải bằng 0 bất kể đầu vào là gì. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2035
2023 6 3
```Chúng tôi theo dõi sản phẩm đang chạy: 

| Bước | x | trả lời trước | tính toán | trả lời sau | 
| --- | --- | --- | --- | --- | 
| 1 | 2023 | 1 | (1 × 2023) % 2035 | 2023 | 
| 2 | 6 | 2023 | (2023 × 6) % 2035 | 1819 | 
| 3 | 3 | 1819 | (1819 × 3) % 2035 | 364 | 

Câu trả lời cuối cùng là 364. 

Điều này chứng tỏ các giá trị trung gian luôn được giảm như thế nào, ngăn ngừa tràn và giữ các giá trị trong phạm vi. 

### Ví dụ 2 

đầu vào:```
3 1000000000
999999999 999999998 999999997
```| Bước | x | trả lời trước | tính toán | trả lời sau | 
| --- | --- | --- | --- | --- | 
| 1 | 999999999 | 1 | (1 × 999999999) % 1e9 | 999999999 | 
| 2 | 999999998 | 999999999 | (999999999 × 999999998) % 1e9 | 2 | 
| 3 | 999999997 | 2 | (2 × 999999997) % 1e9 | 999999994 | 

Câu trả lời cuối cùng là 999999994. 

Dấu vết này nêu bật lý do tại sao modulo phải được áp dụng sau mỗi phép nhân, không chỉ ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần với phép nhân thời gian không đổi và modulo | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được sử dụng | 

Quét tuyến tính là tối ưu cho$n \le 10^5$, thoải mái trong giới hạn một giây. Việc sử dụng bộ nhớ là không đáng kể vì không cần mảng phụ trợ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod

    n, p = map(int, inp.split()[0:2])
    arr = list(map(int, inp.split()[2:2+n]))
    
    ans = 1 % p
    for x in arr:
        ans = (ans * x) % p
    return str(ans)

# provided sample
assert run("3 2035\n2023 6 3\n") == "364", "sample 1"

# sample 2
assert run("3 1000000000\n999999999 999999998 999999997\n") == "999999994", "sample 2"

# minimum n
assert run("1 7\n5\n") == "5", "single element"

# contains zero
assert run("5 13\n3 0 7 9 11\n") == "0", "zero forces product to zero"

# p = 1
assert run("4 1\n10 20 30 40\n") == "0", "mod 1 always zero"

# all ones
assert run("5 100\n1 1 1 1 1\n") == "1", "identity multiplication"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | giá trị bản thân mod p | trường hợp cơ sở | 
| chứa số không | 0 | tài sản diệt sớm | 
| p = 1 | 0 | trường hợp cạnh mô đun | 
| tất cả những cái | 1 | nhận dạng nhân | 

## Vỏ cạnh 

Khi nào$p = 1$, mỗi bước nhân đều giảm về 0. Thuật toán khởi tạo`ans = 1 % p`, trở thành 0 ngay lập tức. Ngay khi quá trình lặp lại bắt đầu,`ans`vẫn bằng 0 bất kể đầu vào, tạo ra kết quả chính xác. 

Đối với đầu vào chứa số 0, chẳng hạn như`3 10 / 4 0 7`, phép nhân đầu tiên tạo ra số 0 sẽ thu gọn tích đang chạy về 0. Các bước tiếp theo giữ nguyên vì`0 * x % p = 0`, do đó đầu ra cuối cùng vẫn bằng 0 như mong đợi. 

Đối với mảng một phần tử, vòng lặp thực hiện một lần và kết quả chỉ đơn giản là phần tử đó theo modulo$p$. Bất biến vẫn giữ nguyên vì việc khởi tạo tương ứng với một tích trống và một phép nhân sẽ chuyển nó thành giá trị cuối cùng chính xác.
