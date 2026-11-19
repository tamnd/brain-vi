---
title: "CF 2070A - FizzBuzz Remixed"
description: "Bài toán yêu cầu chúng ta đếm các số nguyên từ 0 đến n, trong đó số nguyên đó có cùng số dư khi chia cho 3 và cho 5. Thay vì tạo từng số và kiểm tra điều kiện, chúng ta chỉ cần tính xem tồn tại bao nhiêu số như vậy."
date: "2026-06-08T06:55:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2070
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 175 (Rated for Div. 2)"
rating: 800
weight: 2070
solve_time_s: 101
verified: true
draft: false
---

[CF 2070A - FizzBuzz phối lại](https://codeforces.com/problemset/problem/2070/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta đếm các số nguyên từ`0`ĐẾN`n`bao gồm trong đó số nguyên để lại cùng số dư khi chia cho`3`và bởi`5`. Thay vì tạo ra từng số và kiểm tra điều kiện, chúng ta chỉ cần tính xem tồn tại bao nhiêu số như vậy. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một số nguyên duy nhất`n`và đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng thỏa mãn điều kiện. 

Giới hạn của`n`đi lên`10^9`, và có thể có tới`10^4`trường hợp thử nghiệm. Một cách tiếp cận ngây thơ lặp đi lặp lại trên mọi số lên đến`n`sẽ yêu cầu lên tới`10^9`hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. Điều này buộc chúng ta phải tìm ra một công thức hoặc mẫu toán học thay vì mô phỏng mọi con số. 

Trường hợp cạnh không rõ ràng xảy ra khi`n`nhỏ hơn`3`hoặc`5`. Ví dụ, với`n = 0`, chỉ một`0`thỏa mãn điều kiện thì tính được`1`. Một trường hợp cạnh khác xảy ra khi`n`nằm ngay dưới bội số của`15`bởi vì mẫu số hợp lệ lặp lại mỗi`15`những con số. Việc triển khai bất cẩn có thể không bao gồm khối cuối cùng một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu lặp đi lặp lại từ`0`ĐẾN`n`và kiểm tra từng số nguyên với`i % 3 == i % 5`. Điều này đúng nhưng quá chậm đối với`n`lên đến`10^9`. Số lượng hoạt động có thể đạt tới`10^9 * 10^4`trong trường hợp xấu nhất là không thể thực hiện được. 

Nhận xét quan trọng là điều kiện`i % 3 == i % 5`xảy ra định kỳ. bội số chung nhỏ nhất của`3`Và`5`là`15`, do đó dãy lặp lại sau mỗi 15 số. Trong mỗi khối 15 số, chính xác là các số`0, 1, 2, 15, ...`thỏa mãn`i % 3 == i % 5`. Bằng cách kiểm tra khối đầu tiên, chúng ta thấy rằng`0, 1, 2`thỏa mãn nó. Vì vậy, đối với bất kỳ`n`, chúng ta có thể tính toán có bao nhiêu khối hoàn chỉnh gồm 15 khối phù hợp`[0, n]`và còn lại bao nhiêu số thừa rồi đếm những số thỏa mãn điều kiện còn lại trong số đó. 

Điều này làm giảm vấn đề thành số học số nguyên đơn giản: chia, modulo và nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Đếm khối | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`. 
2. Tính xem có bao nhiêu khối đầy đủ 15 số`[0, n]`sử dụng phép chia số nguyên:`full_blocks = n // 15`. 
3. Tính các số còn lại ngoài khối đầy đủ:`remainder = n % 15`. 
4. Mỗi khối đầy đủ đóng góp`3`số hợp lệ (`0, 1, 2`trong khối). nhân`full_blocks * 3`. 
5. Khối còn lại đóng góp`min(remainder + 1, 3)`số hợp lệ, vì chỉ có ba số đầu tiên trong bất kỳ khối nào thỏa mãn điều kiện. 
6. Tính tổng các phần đóng góp từ các khối đầy đủ và phần dư để có đáp án. 
7. In kết quả cho test case. 

**Tại sao nó hoạt động:** Điều kiện`i % 3 == i % 5`lặp lại mỗi 15 số. Bằng cách đếm đầy đủ các khối 15 số và xử lý phần còn lại một cách cẩn thận, chúng tôi đảm bảo sẽ đếm tất cả các số thỏa mãn điều kiện chính xác một lần. Điều này tránh việc lặp lại tất cả các số trong khi vẫn cung cấp số đếm chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        full_blocks = n // 15
        remainder = n % 15
        count = full_blocks * 3 + min(remainder + 1, 3)
        print(count)

if __name__ == "__main__":
    solve()
```Mã này đọc tất cả các trường hợp kiểm thử một cách hiệu quả bằng cách sử dụng I/O nhanh. Việc tính toán sử dụng phép chia số nguyên và modulo để tính các khối 15 số hoàn chỉnh và các số còn lại.`min(remainder + 1, 3)`đảm bảo chúng tôi chỉ đếm tối đa ba số trong khối còn lại. Điều này tránh được những sai lầm ở ranh giới, chẳng hạn như`n`ngay dưới bội số của 15. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 5`| full_blocks | phần còn lại | đóng góp khối | đóng góp còn lại | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | phút(5+1, 3) = 3 | 3 | 

Những con số thỏa mãn`i % 3 == i % 5`là`0, 1, 2`. 

**Ví dụ 2:**`n = 42`| full_blocks | phần còn lại | đóng góp khối | đóng góp còn lại | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 | 12 | 2*3 = 6 | phút(12+1, 3) = 3 | 9 | 

Số hợp lệ:`0,1,2`từ khối đầu tiên,`15,16,17`từ khối thứ hai,`30,31,32`từ khối thứ ba (số dư 12 cho ba số). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm chỉ yêu cầu số học số nguyên: chia, modulo, nhân và tối thiểu. | 
| Không gian | O(1) | Không cần thêm mảng hoặc cấu trúc. | 

Giải pháp dễ dàng xử lý tối đa các ràng buộc của`t = 10^4`Và`n = 10^9`bởi vì mỗi trường hợp thử nghiệm được tính toán trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("7\n0\n5\n15\n42\n1337\n17101997\n998244353\n") == "1\n3\n4\n9\n270\n3420402\n199648872", "sample 1"

# custom cases
assert run("3\n2\n14\n15\n") == "3\n3\n4", "small edge cases"
assert run("2\n0\n1\n") == "1\n2", "n=0 and n=1 edge cases"
assert run("1\n1000000000\n") == str((1000000000//15)*3 + min(1000000000%15 + 1,3)), "large n"
assert run("1\n14\n") == "3", "n just below a multiple of 15"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2, 14, 15 | 3, 3, 4 | Xử lý đúng khối dư và khối 15 số đầy đủ | 
| 0, 1 | 1, 2 | Đầu vào tối thiểu bao gồm n=0 | 
| 1000000000 | tính toán | Hiệu suất tối đa n | 

## Vỏ cạnh 

cho`n = 0`, thuật toán trả về đúng`1`bởi vì chỉ`0 % 3 == 0 % 5`. Vì`n = 14`, khối còn lại gồm 14 số chỉ đếm đúng ba số đầu tiên. Vì`n`bằng bội số của 15, ví dụ: 15, công thức cộng thêm một số từ phần còn lại, cho ra chính xác bốn số, phù hợp với việc đếm thủ công. Tính bất biến mà mẫu lặp lại sau mỗi 15 số đảm bảo tính chính xác trong tất cả các trường hợp cạnh.
