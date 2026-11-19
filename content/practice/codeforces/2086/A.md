---
title: "CF 2086A - Mứt Dâu Tây"
description: "Chúng tôi được yêu cầu xác định cần bao nhiêu kg quả mâm xôi để sản xuất một số lọ mứt nhất định, biết rằng mứt được làm bằng cách kết hợp một lượng quả mọng và đường bằng nhau, và trong quá trình nấu, 25% tổng khối lượng bay hơi."
date: "2026-06-08T06:02:25+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2086
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 177 (Rated for Div. 2)"
rating: 800
weight: 2086
solve_time_s: 70
verified: true
draft: false
---

[CF 2086A - Mứt mâm xôi](https://codeforces.com/problemset/problem/2086/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định cần bao nhiêu kg quả mâm xôi để sản xuất một số lọ mứt nhất định, biết rằng mứt được làm bằng cách kết hợp một lượng quả mọng và đường bằng nhau, và trong quá trình nấu, 25% tổng khối lượng bay hơi. Đầu vào cung cấp số lượng lọ cho một số trường hợp thử nghiệm và đầu ra phải là số kg quả cần thiết cho mỗi trường hợp. 

Nói một cách đơn giản hơn, mỗi lọ mứt có khối lượng cuối cùng cố định là 3 kg. Để sản xuất nó, chúng tôi trộn một lượng quả mọng và đường bằng nhau, tổng cộng lên tới hơn 3 kg, vì 25% bay hơi. Về mặt đại số, nếu chúng ta sử dụng`x`kg quả mọng và`x`kg đường thì khối lượng cuối cùng là`3x/2`sau khi bốc hơi. Chúng ta cần khối lượng này bằng 3 kg mỗi bình. Giải quyết`3x/2 = 3`cho`x = 2`kg quả mỗi lọ. Do đó, bài toán rút gọn thành việc nhân số lọ với 2 để có được số quả cần thiết. 

Các ràng buộc ở mức độ nhẹ: tối đa 10^4 trường hợp thử nghiệm và tối đa 10^8 lọ cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng mọi giải pháp đều phải là O(1) cho mỗi trường hợp thử nghiệm. Các cách tiếp cận lặp đi lặp lại hoặc dựa trên mô phỏng là không cần thiết và sẽ quá chậm. Các trường hợp cạnh liên quan đến số lượng lọ nhỏ nhất (`n = 1`) và số lớn nhất có thể (`n = 10^8`), nhưng phép tính vẫn là phép nhân đơn giản, sẽ không vượt quá số nguyên 64 bit tiêu chuẩn. 

Một sai lầm ngây thơ là tính toán không chính xác phần bay hơi hoặc quên nhân với số lọ. Ví dụ, tính toán`(3 * n) * 0.75`vì việc thả nổi và làm tròn có thể dẫn đến các lỗi sai lệch cho số lượng lớn`n`. Cách tiếp cận đúng hoàn toàn là số học số nguyên:`2 * n`. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận mạnh mẽ sẽ cố gắng mô phỏng việc chế tạo từng lọ bằng cách trộn liên tục các loại quả mọng và đường, đồng thời áp dụng độ bay hơi 25%. Vì`n = 10^8`jars, điều này hoàn toàn không thực tế. Quan sát bài toán bằng toán học sẽ thấy một công thức trực tiếp: mỗi bình cần đúng 2 kg quả mọng. Khi đã xác định được điều này, giải pháp tối ưu chỉ đơn giản là nhân số lọ với 2. 

Điểm mấu chốt là vấn đề có tính chất tuyến tính: hệ số bay hơi không đổi và mỗi lọ là độc lập. Không có sự phụ thuộc giữa các lọ hoặc lợi nhuận giảm dần. Điều này làm giảm giải pháp từ một mô phỏng lặp tiềm năng thành một phép nhân duy nhất cho mỗi trường hợp thử nghiệm. Các thao tác dấu phẩy động là không cần thiết; số học số nguyên đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm đối với n lớn | 
| Phép nhân trực tiếp | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc số lọ`n`. 
3. Tính số quả cần thiết như sau:`2 * n`. 
4. In kết quả. 

Tại sao điều này có tác dụng: mỗi lọ cần 2 kg quả mọng, bắt nguồn từ công thức`final_mass = 3/4 * total_mass`, giải quyết`total_mass / 2 * 2/3 = 1`cứ một lọ mang lại 2 kg quả mọng. Vì mỗi lọ là độc lập nên tổng số là tuyến tính theo`n`. Số học số nguyên đảm bảo câu trả lời chính xác mà không có lỗi làm tròn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(2 * n)
```Giải pháp sử dụng I/O nhanh với`sys.stdin.readline`, mặc dù đối với vấn đề này tiêu chuẩn`input()`sẽ đủ. Mỗi trường hợp thử nghiệm được xử lý độc lập và nhân với 2 là đủ để tính số quả cần thiết trên mỗi lọ. 

## Ví dụ đã hoạt động 

| n | Tính toán | Kết quả | 
| --- | --- | --- | 
| 1 | 2*1 | 2 | 
| 3 | 2*3 | 6 | 

Vì`n = 1`, lọ đơn cần 2 kg quả mọng. Vì`n = 3`, ba lọ cần tổng cộng 6 kg. Dấu vết xác nhận rằng công thức`2 * n`tính toán chính xác khối lượng cần thiết trong mọi trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép nhân cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Không cần thêm bộ nhớ cho mỗi trường hợp thử nghiệm | 

Điều này là tối ưu với các ràng buộc đầu vào, vì chúng ta phải đọc`t`trường hợp thử nghiệm và đưa ra kết quả cho từng trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(2 * n)
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided samples
assert run("2\n1\n3\n") == "2\n6", "sample tests"

# Custom cases
assert run("1\n10\n") == "20", "ten jars"
assert run("1\n100000000\n") == "200000000", "max jars"
assert run("3\n1\n2\n5\n") == "2\n4\n10", "multiple small cases"
assert run("1\n0\n") == "0", "zero jars"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 2 | lọ tối thiểu khác 0 | 
| 3 | 6 | số lượng lọ nhỏ | 
| 100000000 | 200000000 | số lượng lọ tối đa, kiểm tra việc xử lý số nguyên lớn | 
| 0 | 0 | hộp đựng cạnh không có lọ | 
| nhiều nhỏ | 2, 4, 10 | nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

cho`n = 1`, phép tính`2 * 1 = 2`xử lý chính xác số lọ dương nhỏ nhất. Vì`n = 10^8`,`2 * 10^8`nằm trong giới hạn số nguyên 64 bit, do đó không xảy ra tràn. Trường hợp không bình`n = 0`sản xuất`0`, phản ánh chính xác rằng không cần quả mọng. Những điều này xác nhận công thức là mạnh mẽ trên miền vấn đề.
