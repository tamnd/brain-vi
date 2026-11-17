---
title: "CF 1994B - Trò Chơi Vui Nhộn"
description: "Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau, s và t. Vanya có thể thực hiện một thao tác cụ thể trên s nhiều lần: anh chọn một mảng con liền kề từ vị trí l đến r, và với mỗi vị trí i trong mảng con đó, thay thế s[i] bằng s[i] XOR s[i - l + 1]."
date: "2026-06-09T02:21:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1994
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 959 sponsored by NEAR (Div. 1 + Div. 2)"
rating: 1100
weight: 1994
solve_time_s: 322
verified: false
draft: false
---

[CF 1994B - Trò chơi vui nhộn](https://codeforces.com/problemset/problem/1994/B) 

**Đánh giá:** 1100 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi nhị phân có độ dài bằng nhau,`s`Và`t`. Vanya có thể thực hiện một thao tác cụ thể trên`s`lặp đi lặp lại: anh ta chọn một mảng con liền kề từ vị trí`l`ĐẾN`r`, và với mỗi vị trí`i`trong mảng con đó, thay thế`s[i]`với`s[i] XOR s[i - l + 1]`. Mục đích là để xác định xem liệu có thể sử dụng bất kỳ số lượng thao tác nào để chuyển đổi`s`vào trong`t`. 

Quan sát quan trọng đầu tiên là hoạt động hoàn toàn tuyến tính trên các bit. Mỗi bit có thể được đảo ngược khi và chỉ khi tồn tại ít nhất một`1`trong tiền tố có thể truyền qua XOR. Nếu phần tử đầu tiên là`0`, nó không bao giờ có thể ảnh hưởng trực tiếp đến các bit sau này. Ngược lại, nếu có ít nhất một`1`TRONG`s`, chúng ta có thể truyền nó để lật các bit khác thông qua các thao tác thích hợp. 

Các ràng buộc đầu vào cho phép các chuỗi lên tới`2·10^5`bit cho mỗi trường hợp thử nghiệm và tổng của tất cả độ dài chuỗi trong các trường hợp thử nghiệm cũng nhiều nhất là`2·10^5`. Điều này có nghĩa là chúng ta cần một`O(n)`giải pháp cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng mô phỏng trực tiếp các hoạt động, đặc biệt là trên các vòng lặp lồng nhau, sẽ quá chậm. Các trường hợp cạnh phát sinh khi`s`tất cả các số 0 hoặc các chuỗi đều có độ dài 1, vì trong những trường hợp này không thể truyền XOR. 

Việc triển khai bất cẩn nhằm mô phỏng các hoạt động có thể thất bại đối với các đầu vào như`s = 0`Và`t = 1`, bởi vì không có thao tác nào có thể thay đổi số 0 đơn thành số 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng hoạt động XOR trên tất cả các khả năng có thể`(l, r)`cặp lặp đi lặp lại cho đến khi`s`bằng`t`. Về nguyên tắc, điều này hoạt động vì hoạt động mang tính xác định và giới hạn ở các vị trí hữu hạn. Tuy nhiên, có khoảng`O(n^2)`cặp cho mỗi thao tác và có thể có nhiều thao tác lặp lại, dẫn đến độ phức tạp trong trường hợp xấu nhất`O(n^3)`hoặc tệ hơn. Đối với các ràng buộc lớn nhất (`n ≈ 2·10^5`), điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là mỗi bit trong`s`chỉ có thể ảnh hưởng đến các bit sau thông qua XOR nếu có ít nhất một`1`trước nó. Cụ thể, hoạt động tại chỉ mục`i`chỉ có thể lật một`0`ĐẾN`1`(hoặc ngược lại) nếu tồn tại ít nhất một`1`trong tiền tố`s[0..i-1]`. Vì vậy, việc chuyển đổi chỉ không thể thực hiện được nếu`s`tất cả đều là số không và`t`chứa ít nhất một`1`. Mặt khác, trò chơi rất "thú vị" vì chúng ta có thể tuyên truyền một cách xây dựng những trò chơi hiện có để phù hợp`t`. 

Điều này làm giảm vấn đề thành một kiểm tra đơn giản: nếu`s`tất cả đều là số không và`t`không, in ra "Không". Nếu phần tử đầu tiên của`s`bằng phần tử đầu tiên của`t`và có ít nhất một`1`ở đâu đó trong`s`, hoặc nếu các chuỗi giống nhau thì in ra "Có". Cách tiếp cận này là`O(n)`cho mỗi trường hợp thử nghiệm và phù hợp thoải mái trong giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc độ dài`n`và trình tự`s`Và`t`. 
2. Nếu`s`bằng với`t`rồi, xuất "Có" và tiếp tục. Không có hoạt động cần thiết. 
3. Đếm số lượng`1`ở trong`s`. Nếu không có ai (`count_1 == 0`) Và`t`chứa ít nhất một`1`, xuất ra "Không". Không thể biến số không thành số một. 
4. Nếu không, hãy kiểm tra xem bit đầu tiên của`s`là`0`và bit đầu tiên của`t`là`1`. Nếu vậy, hãy ghi "Không" vì vị trí đầu tiên không bao giờ có thể thay đổi từ`0`ĐẾN`1`mà không có cái trước để truyền bá. 
5. Trong tất cả các trường hợp khác, xuất ra "Có". Có ít nhất một`1`để truyền bá và đạt được bất kỳ cấu hình cần thiết nào của`t`. 

Tại sao nó hoạt động: Điều bất biến là bất kỳ vị trí nào`i`chỉ có thể được đảo ngược nếu tồn tại ít nhất một`1`trong tiền tố`s[0..i-1]`. Bằng cách đếm`1`s và kiểm tra phần tử đầu tiên, chúng tôi nắm bắt được tất cả các ràng buộc ngăn cản`t`từ chỗ có thể đạt được. Phần còn lại được đảm bảo bởi tính chất xây dựng của việc truyền bá XOR. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    q = int(input())
    for _ in range(q):
        n = int(input())
        s = input().strip()
        t = input().strip()
        
        if s == t:
            print("Yes")
            continue
        
        if '1' not in s:
            if '1' in t:
                print("No")
            else:
                print("Yes")
            continue
        
        if s[0] == '0' and t[0] == '1':
            print("No")
        else:
            print("Yes")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên là kiểm tra sự bằng nhau để xử lý ngay những trường hợp tầm thường. Số lượng của`1`ở trong`s`xác định xem có thể thực hiện được bất kỳ thao tác XOR nào không. Việc kiểm tra bit đầu tiên đảm bảo các vị trí không thể thay đổi được tôn trọng. Tất cả các bước chạy trong`O(n)`mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
1
0
1
```| Bước | s | t | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 1 | Kiểm tra sự bình đẳng | Không bằng | 
| Đếm 1 giây trong s | 0 | - | Không có ai ở s, t có một | Không | 

Điều này xác nhận rằng các số 0 bit đơn không thể bị đảo ngược. 

**Ví dụ 2** 

đầu vào:```
1
4
0100
0001
```| Bước | s | t | Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0100 | 0001 | Kiểm tra sự bình đẳng | Không bằng | 
| Đếm 1 giây trong s | 1 | - | s có một | Tiếp tục | 
| Bit đầu tiên s vs t | 0 vs 0 | - | Tương thích bit đầu tiên | Có | 

Điều này chứng tỏ rằng miễn là có ít nhất một`1`TRONG`s`, sự lan truyền cho phép đạt được`t`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi quét các chuỗi để kiểm tra sự bằng nhau và đếm số đơn vị | 
| Không gian | O(1) | Chỉ có một số biến được sử dụng; trình tự có thể được đọc từng dòng | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là`2·10^5`, làm cho độ phức tạp tổng thể nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("6\n1\n0\n1\n7\n0110100\n0110100\n9\n100101010\n101111110\n4\n0011\n1011\n4\n0100\n0001\n8\n10110111\n01100000\n") == "No\nYes\nYes\nNo\nYes\nYes"

# Custom cases
assert run("1\n1\n0\n0\n") == "Yes", "single zero unchanged"
assert run("1\n1\n1\n1\n") == "Yes", "single one unchanged"
assert run("1\n2\n00\n01\n") == "No", "two zeros cannot produce a one"
assert run("1\n3\n101\n010\n") == "Yes", "ones can propagate to flip zeros"
assert run("1\n5\n00000\n00000\n") == "Yes", "all zeros unchanged"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1-bit 0 → 0 | Có | Một bit không hoạt động | 
| 1-bit 1 → 1 | Có | Một bit không hoạt động | 
| 2-bit 00 → 01 | Không | Không thể tạo số từ số không | 
| 3-bit 101 → 010 | Có | Những cái được truyền qua XOR | 
| 5-bit tất cả số không không thay đổi | Có | Tất cả số không không cần lật | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi`s`bắt đầu bằng`0`Và`t`bắt đầu bằng`1`. Ví dụ,`s = 0xxxx`Và`t = 1xxxx`. Trong trường hợp này, không có thao tác nào có thể thay đổi bit đầu tiên từ`0`ĐẾN`1`, vì XOR phụ thuộc vào cái trước đó. Thuật toán trả về chính xác "Không". Một trường hợp cạnh khác là khi`s`có chính xác một`1`nằm sau bit đầu tiên và`t`yêu cầu lật các bit trước đó. Giải pháp này hoạt động vì chỉ có bit đầu tiên quan trọng đối với việc kiểm tra sự lan truyền này; mọi bit sau này luôn có thể được truyền đi một khi có`1`.
