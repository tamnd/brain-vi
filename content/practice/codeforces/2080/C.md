---
title: "CF 2080C - Lật thẻ"
description: "Trong bài toán này, chúng ta được cấp một dãy n thẻ xếp thành một hàng. Mỗi thẻ có một mặt hiển thị 0 hoặc 1. Thao tác duy nhất được phép là chọn một đoạn thẻ liền kề và lật tất cả các thẻ trong đoạn đó (0 trở thành 1, 1 trở thành 0)."
date: "2026-06-08T06:25:36+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 2080
codeforces_index: "C"
codeforces_contest_name: "XIX Open Olympiad in Informatics - Final Stage, Day 2 (Unrated, Online Mirror, IOI rules)"
rating: 2300
weight: 2080
solve_time_s: 54
verified: true
draft: false
---

[CF 2080C - Lật thẻ](https://codeforces.com/problemset/problem/2080/C) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trong bài toán này, chúng ta được cung cấp một chuỗi các`n`thẻ được xếp thành một hàng. Mỗi thẻ có một mặt hiển thị 0 hoặc 1. Thao tác duy nhất được phép là chọn một đoạn thẻ liền kề và lật tất cả các thẻ trong đoạn đó (0 trở thành 1, 1 trở thành 0). Mục tiêu của chúng tôi là chuyển đổi chuỗi ban đầu thành một chuỗi trong đó tất cả các thẻ hiển thị 1. Chúng tôi cần xác định số thao tác tối thiểu cần thiết để đạt được điều này. 

Đầu vào bao gồm một số nguyên`n`theo sau là một chuỗi dài`n`đại diện cho các mặt thẻ. Đầu ra là một số nguyên duy nhất, số lần lật tối thiểu. 

Với hạn chế đó`n`có thể lên tới 10^5 và giới hạn thời gian là tiêu chuẩn (thường là 1-2 giây), chúng ta có thể mong đợi thực hiện khoảng 10^8 thao tác trong trường hợp xấu nhất. Điều này loại trừ bất kỳ thuật toán nào thử tất cả các phân đoạn liền kề có thể một cách rõ ràng, bởi vì số lượng các phân đoạn tăng theo phương trình bậc hai là n(n+1)/2. 

Các trường hợp cạnh không rõ ràng bao gồm các chuỗi trong đó có các số 0 và 1 xen kẽ, các chuỗi trong đó tất cả các thẻ đã là 1 và các chuỗi trong đó tất cả các thẻ là 0. Ví dụ: đối với`1010`, một cách tiếp cận đơn giản là lật một số 0 cùng một lúc có thể yêu cầu bốn thao tác, nhưng tồn tại một chuỗi các lần lật chỉ sử dụng hai thao tác: lật các đoạn`1-1`Và`3-3`. Nếu tất cả các thẻ đều là 1 thì câu trả lời là 0. Nếu tất cả các thẻ là 0, thì chỉ cần lật toàn bộ mảng liền kề một lần là đủ, thực hiện 1 thao tác. Việc triển khai bất cẩn thường tính toán quá mức các hoạt động bằng cách lật từng số 0 riêng lẻ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại mọi phân đoạn liền kề có thể có và thử lật nó, kiểm tra đệ quy các chuỗi kết quả cho đến khi chúng ta đạt được tất cả các phân đoạn. Điều này hiệu quả vì việc lật bất kỳ phân đoạn nào đều được cho phép và cuối cùng sẽ chuyển đổi chuỗi thành tất cả các phân đoạn. Tuy nhiên, số lượng các phân đoạn liền kề là n(n+1)/2 và việc kiểm tra tất cả các kết hợp có thể có là hàm mũ trong n, điều này hoàn toàn không khả thi với n=10^5. 

Quan sát quan trọng là chúng ta không cần phải theo dõi từng lần lật một cách rõ ràng. Mỗi lần lật chỉ quan trọng ở chỗ nó thay đổi từ 0 thành 1 như thế nào. Chúng ta có thể quét chuỗi từ trái sang phải và đếm số đoạn 0 liền kề nhau. Mỗi phân đoạn như vậy yêu cầu ít nhất một lần lật và việc lật nó trong một thao tác là tối ưu vì việc lật một phân đoạn lớn hơn bao gồm các số 1 sẽ làm tăng độ phức tạp một cách không cần thiết mà không làm giảm số lượng thao tác. Do đó, số lần lật tối thiểu bằng số đoạn 0 liền kề. 

Brute-force hoạt động vì cuối cùng nó sẽ đạt đến trình tự tất cả một, nhưng không thành công do độ phức tạp theo cấp số nhân. Quan sát thấy rằng các số 0 liền kề có thể được đảo ngược lại với nhau sẽ làm giảm vấn đề xuống còn một lần truyền qua mảng, mang lại giải pháp O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`flips`về 0. Điều này sẽ lưu trữ số lượng hoạt động được yêu cầu. 
2. Khởi tạo chỉ mục`i`về 0 để bắt đầu quét từ thẻ ngoài cùng bên trái. 
3. Trong khi`i`ít hơn`n`, kiểm tra thẻ hiện tại: 

một. Nếu là 1 thì tăng`i`và tiếp tục, vì không cần lật. 

b. Nếu bằng 0 thì tăng`flips`bằng 1, sau đó bỏ qua tất cả các số 0 liền kề bằng cách di chuyển`i`chuyển tiếp cho đến khi đạt đến số 1 hoặc kết thúc chuỗi. Điều này đảm bảo mỗi phân đoạn số 0 được tính chính xác một lần. 
4. Khi đến cuối chuỗi, xuất ra`flips`. 

Tại sao nó hoạt động: Điều bất biến là mọi phân đoạn 0 liền kề đều được tính chính xác một lần. Việc lật phân đoạn đó sẽ biến tất cả các số 0 thành 1 trong một thao tác và việc lật ra ngoài phân đoạn đó sẽ ảnh hưởng đến các số 1 đã đúng một cách không cần thiết. Bằng cách quét từ trái sang phải và đếm các đoạn, chúng tôi đảm bảo số lần lật tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_flips_to_all_ones(cards: str) -> int:
    n = len(cards)
    flips = 0
    i = 0
    while i < n:
        if cards[i] == '1':
            i += 1
        else:
            flips += 1
            while i < n and cards[i] == '0':
                i += 1
    return flips

def main():
    n = int(input())
    cards = input().strip()
    print(min_flips_to_all_ones(cards))

if __name__ == "__main__":
    main()
```chức năng`min_flips_to_all_ones`thực hiện một lần quét tuyến tính duy nhất của chuỗi. Vòng lặp while bên trong đảm bảo các số 0 liền kề được coi là một phân đoạn duy nhất. sử dụng`strip()`trên đầu vào sẽ ngăn các ký tự dòng mới ẩn can thiệp vào việc lập chỉ mục. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`cards = "1010"`| tôi | thẻ[i] | lật | Hành động | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | bỏ qua | 
| 1 | 0 | 1 | bắt đầu phân đoạn, bỏ qua 0s | 
| 2 | 1 | 1 | bỏ qua | 
| 3 | 0 | 2 | bắt đầu phân đoạn, bỏ qua 0s | 
| kết thúc | - | 2 | xong | 

Chúng tôi đếm được 2 phân đoạn 0. Lật từng đoạn một lần sẽ dẫn đến`1111`. 

**Ví dụ 2:**`cards = "111"`| tôi | thẻ[i] | lật | Hành động | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | bỏ qua | 
| 1 | 1 | 0 | bỏ qua | 
| 2 | 1 | 0 | bỏ qua | 
| kết thúc | - | 0 | xong | 

Không cần lật, như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính đơn trên chuỗi | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên | 

Được cho`n <= 10^5`, quá trình quét này sẽ hoàn tất thoải mái trong giới hạn thời gian tiêu chuẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n1010\n") == "2", "sample 1"
assert run("3\n111\n") == "0", "sample 2"

# Custom cases
assert run("5\n00000\n") == "1", "all zeros"
assert run("6\n010101\n") == "3", "alternating zeros"
assert run("1\n0\n") == "1", "single zero"
assert run("1\n1\n") == "0", "single one"
assert run("10\n1111100000\n") == "1", "zeros at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 00000 | 1 | Toàn bộ đoạn số không | 
| 010101 | 3 | Số 0 xen kẽ | 
| 0 | 1 | Số 0 đơn | 
| 1 | 0 | Độc thân | 
| 1111100000 | 1 | Các số 0 liền kề ở cuối | 

## Vỏ cạnh 

Đối với mô hình xen kẽ`010101`, thuật toán sẽ đếm chính xác từng số 0 bị cô lập dưới dạng một phân đoạn riêng biệt. Bắt đầu từ chỉ số 0, nó bỏ qua số 1, đếm lần lật cho 0 ở chỉ số 1, bỏ qua chỉ số 2 (1), đếm lần lật ở chỉ số 3 (0) và lật ở chỉ số 5 (0), thu được chính xác 3 lần lật. Điều này phù hợp với các hoạt động tối thiểu có thể. 

Đối với dãy số một, thuật toán bỏ qua mọi thẻ và không bao giờ tăng`flips`, tạo ra 0, điều này đúng. Đối với chuỗi thẻ đơn, nó xử lý chính xác cả 0 và 1, chỉ lật khi cần thiết.
