---
title: "CF 103486B - Bài tập số học"
description: "Chúng ta có ba số nguyên $A$, $B$ và $K$. Nhiệm vụ là tính giá trị của phân số $A / B$ dưới dạng số thập phân và xuất ra nó với chính xác các chữ số $K$ sau dấu thập phân."
date: "2026-07-03T06:20:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "B"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 46
verified: true
draft: false
---

[CF 103486B - Bài tập số học](https://codeforces.com/problemset/problem/103486/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên$A$,$B$, Và$K$. Nhiệm vụ là tính giá trị của phân số$A / B$dưới dạng số thập phân và xuất ra chính xác$K$chữ số sau dấu thập phân. Kết quả phải được làm tròn bằng quy tắc làm tròn lên một nửa, nghĩa là chúng ta kiểm tra chữ số ngay sau số$K$-chữ số thập phân thứ: nếu lớn hơn 5 thì ta tăng$K$-chữ số thứ 1, nếu không chúng ta giữ nguyên. 

Kích thước đầu vào nhỏ:$A, B \le 1000$Và$K \le 1000$. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không cần bất kỳ phương pháp số phức tạp tiệm cận nào. Một mô phỏng đơn giản của phép chia dài là đủ vì tạo ra tối đa$K+1$chữ số chỉ yêu cầu$O(K)$các bước tính toán. 

Sự tinh tế chính là tính chính xác xung quanh việc làm tròn và truyền bá. Việc triển khai đơn giản thường thất bại khi làm tròn gây ra hiện tượng nhớ theo tầng, chẳng hạn như khi phần phân số kết thúc bằng 9999 và việc làm tròn đẩy nó vào phần nguyên. 

Ví dụ, hãy xem xét: 

đầu vào:$1\ 3\ 1$Giá trị chính xác là$0.3333...$Nếu chúng ta tính hai chữ số:$0.33$, việc làm tròn không làm gì cả và kết quả đầu ra là chính xác. 

Nhưng đối với: 

đầu vào:$2\ 3\ 1$Giá trị chính xác là$0.6666...$Chúng tôi tính toán$0.66$, rồi xem chữ số 6 tiếp theo, vì vậy chúng ta làm tròn đến$0.7$. Điều này chứng tỏ rằng việc làm tròn phải được áp dụng sau khi tạo chữ số bổ sung, không phải trong quá trình tạo chữ số. 

Một trường hợp cạnh khác phát sinh khi làm tròn ảnh hưởng đến phần nguyên: 

đầu vào:$999\ 1000\ 3$chúng tôi nhận được$0.999...$. Với việc làm tròn, điều này trở thành$1.000$. Cách tiếp cận dựa trên chuỗi đơn giản chỉ thao tác các chữ số phân số có thể không truyền được phần mang vào phần nguyên. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tính giá trị hữu tỉ chính xác dưới dạng số dấu phẩy động và định dạng nó bằng$K$số thập phân bằng cách sử dụng làm tròn tích hợp. Mặc dù đơn giản về mặt khái niệm, nhưng việc biểu diễn dấu phẩy động không đáng tin cậy với độ chính xác lên tới 1000 chữ số. Nhân đôi tiêu chuẩn chỉ đảm bảo độ chính xác khoảng 15-17 chữ số thập phân, do đó, đối với số lớn hơn$K$, cách tiếp cận này trở nên không chính xác. 

Một cách tiếp cận đơn giản khác là tính từng chữ số thập phân bằng cách sử dụng phép nhân lặp lại cho 10 và phép chia số nguyên, về cơ bản là phép chia dài. Điều này đúng nhưng phải được mở rộng cẩn thận để tạo thêm một chữ số làm tròn. 

Quan sát quan trọng là phép chia số nguyên ở dạng thập phân có thể được mô phỏng hoàn toàn bằng số học số nguyên. Ở mỗi bước, chúng ta giữ lại số dư, nhân với 10 và trích chữ số tiếp theo bằng phép chia. Điều này tránh hoàn toàn lỗi dấu phẩy động. Để hỗ trợ làm tròn, chúng tôi tạo ra$K+1$chữ số thay vì$K$, sau đó áp dụng quy tắc làm tròn một lần vào cuối. Bởi vì$K \le 1000$, điều này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Dấu phẩy động + định dạng | O(1) | O(1) | Không đúng với K lớn | 
| Mô phỏng phép chia dài | O(K) | O(K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng phép chia dài thủ công$A / B$ở cơ sở 10, nhưng chỉ tối đa$K+1$chữ số thập phân. 

1. Tính phần nguyên là$A // B$và số dư ban đầu là$A \% B$. Phần nguyên được cố định và không phụ thuộc vào độ chính xác. 
2. Tạo nhiều chữ số thập phân. Đối với mỗi bước, nhân số dư với 10 và tính chữ số tiếp theo là$(\text{remainder} \times 10) // B$. Cập nhật phần còn lại cho phù hợp. Điều này phản ánh cách mở rộng thập phân hoạt động trong phép chia thủ công. 
3. Bảo quản chính xác$K+1$chữ số của phần phân số. Chữ số phụ chỉ tồn tại cho các quyết định làm tròn. 
4. Áp dụng làm tròn. Nếu$(K+1)$-chữ số thứ ít nhất là 5, tăng dần$K$-chữ số thứ. Điều này có thể kích hoạt việc truyền tải ngược thông qua các chữ số phân số. 
5. Nếu phần truyền mang vượt quá chữ số phân số đầu tiên, hãy truyền vào phần nguyên. 
6. Xây dựng chuỗi cuối cùng một cách chính xác$K$các chữ số phân số, đệm bằng số 0 nếu cần. 

Tại sao nó hoạt động: bất biến của phép chia dài là sau mỗi bước, phần còn lại biểu thị phần phân số thực được chia tỷ lệ theo lũy thừa 10. Mỗi chữ số được trích xuất chính xác cho vị trí đó trong khai triển cơ số 10. Vì chúng tôi tạo thêm một chữ số trước khi làm tròn nên quyết định làm tròn số$K$-chữ số thứ sử dụng thông tin chính xác về việc liệu giá trị thực có vượt quá ranh giới cắt bớt hay không, đảm bảo tính chính xác khi làm tròn một nửa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B, K = map(int, input().split())

    int_part = A // B
    rem = A % B

    digits = []
    for _ in range(K + 1):
        rem *= 10
        digits.append(rem // B)
        rem %= B

    # rounding from K+1 digit
    if digits[-1] >= 5:
        i = K - 1
        carry = 1
        while i >= 0 and carry:
            digits[i] += carry
            if digits[i] == 10:
                digits[i] = 0
                carry = 1
            else:
                carry = 0
            i -= 1

        if carry:
            int_part += 1

    frac = digits[:K]

    print(f"{int_part}." + "".join(map(str, frac)))

if __name__ == "__main__":
    solve()
```Phần nguyên được trích xuất trực tiếp vì nó độc lập với độ chính xác thập phân. Vòng lặp tạo ra$K+1$các chữ số sử dụng phép chia dài cổ điển với tính năng theo dõi số dư. Khối làm tròn truyền ngược một cách rõ ràng, điều này là cần thiết vì việc làm tròn có thể xếp tầng trên nhiều số 9 ở cuối. 

Bước định dạng cuối cùng đảm bảo chính xác$K$chữ số bằng cách cắt mảng. Phần đệm bằng 0 được giữ nguyên một cách tự nhiên vì chúng tôi lưu trữ các chữ số một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 2`Chúng tôi tính toán$1 / 2 = 0.5$. 

| Bước | Phần còn lại | Chữ số | 
| --- | --- | --- | 
| 1 | 1 → 10 | 5 | 
| 2 | 0 → 0 | 0 (chữ số phụ để làm tròn) | 

Chúng tôi có chữ số`[5, 0]`. Vì chữ số phụ là 0 nên không làm tròn số. Đầu ra trở thành`0.50`. 

Điều này xác nhận rằng việc bảo toàn số 0 ở cuối hoạt động chính xác. 

### Ví dụ 2 

đầu vào:`10 99 5`Chúng tôi mô phỏng phép chia dài: 

| Bước | Phần còn lại | Chữ số | 
| --- | --- | --- | 
| 1 | 10 → 100 | 1 | 
| 2 | 1 → 10 | 0 | 
| 3 | 10 → 100 | 1 | 
| 4 | 1 → 10 | 0 | 
| 5 | 10 → 100 | 1 | 
| 6 (thêm) | 1 → 10 | 0 | 

chữ số:`[1, 0, 1, 0, 1, 0]`Không làm tròn vì chữ số cuối cùng là 0. Kết quả cuối cùng là`0.10101`. 

Điều này chứng tỏ rằng thuật toán bảo toàn cấu trúc thập phân xen kẽ mà không bị nhiễu bởi logic làm tròn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K) | Mỗi chữ số thập phân yêu cầu số học theo thời gian không đổi và chúng tôi tính K+1 chữ số | 
| Không gian | O(K) | Chúng tôi lưu trữ các chữ số được tạo một cách rõ ràng để làm tròn | 

Các ràng buộc cho phép tối đa 1000 chữ số, do đó, quét tuyến tính và mô phỏng số học có thể dễ dàng thực hiện nhanh chóng trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("1 2 2\n") == "0.50"
assert run("10 99 5\n") == "0.10101"

# custom cases
assert run("1 3 1\n") == "0.3", "simple rounding down"
assert run("2 3 1\n") == "0.7", "rounding up with carry"
assert run("999 1000 3\n") == "1.000", "carry into integer part"
assert run("1 8 5\n") == "0.12500", "trailing zeros preserved"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 3 1 | 0,3 | cắt ngắn đơn giản | 
| 2 3 1 | 0,7 | làm tròn | 
| 999 1000 3 | 1.000 | chuyển thành phần nguyên | 
| 1 | | |
