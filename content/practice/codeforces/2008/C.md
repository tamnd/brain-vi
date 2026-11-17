---
title: "CF 2008C - Mảng tốt dài nhất"
description: "Chúng ta được yêu cầu xây dựng một mảng các số nguyên nằm giữa hai giới hạn cho trước, l và r, sao cho mảng đó tăng lên một cách nghiêm ngặt và sự khác biệt giữa các phần tử liên tiếp cũng tăng lên một cách nghiêm ngặt. Mục tiêu của chúng tôi là tìm độ dài tối đa có thể có của một mảng như vậy."
date: "2026-06-08T13:22:51+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 800
weight: 2008
solve_time_s: 84
verified: true
draft: false
---

[CF 2008C - Mảng tốt dài nhất](https://codeforces.com/problemset/problem/2008/C) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, toán học 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng các số nguyên giữa hai giới hạn cho trước,`l`Và`r`, sao cho mảng đang tăng lên một cách nghiêm ngặt và sự khác biệt giữa các phần tử liên tiếp cũng tăng lên một cách nghiêm ngặt. Mục tiêu của chúng tôi là tìm độ dài tối đa có thể có của một mảng như vậy. Mỗi trường hợp thử nghiệm cho chúng ta một cặp`(l, r)`và chúng ta phải xuất ra một số nguyên duy nhất cho trường hợp đó: độ dài của "mảng tốt" dài nhất vừa với giới hạn. 

Những ràng buộc của vấn đề cho phép`l`Và`r`lớn như$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ giải pháp nào xây dựng lặp lại tất cả các mảng có thể hoặc kiểm tra tất cả các kết hợp giữa`l`Và`r`. Thậm chí$O((r-l)^2)$cách tiếp cận sẽ quá chậm, bởi vì`r-l`có thể lên đến$10^9$. Chúng ta cần một phương pháp tính toán độ dài tối đa bằng cách sử dụng các thuộc tính số học, không phải liệt kê. 

Một trường hợp cạnh tinh tế phát sinh khi`l`bằng`r`. Trong tình huống đó, mảng duy nhất có thể là`[l]`, vậy câu trả lời là`1`. Một tình huống phức tạp khác là khi`r - l`rất nhỏ, chỉ cho phép các mảng có độ dài`2`nhiều nhất. Các thuật toán bất cẩn cho rằng mảng luôn có thể phát triển vượt quá hai phần tử sẽ thất bại ở đây. Ví dụ,`(l=1, r=2)`chỉ cho phép`[1,2]`mặc dù mẫu có thể gợi ý mảng dài hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ bắt đầu từ`l`, thử tất cả các khác biệt tăng dần và tiếp tục thêm các phần tử cho đến khi đạt được`r`. Điều này hiệu quả vì chúng ta có thể mô phỏng trực tiếp các quy tắc "mảng tốt", nhưng nó trở nên không thực tế khi`r-l`là lớn, vì số lượng các chuỗi tiềm năng tăng lên một cách tổ hợp. Trong trường hợp xấu nhất, việc lặp qua tất cả các khả năng sẽ mất thời gian tỷ lệ thuận với số tập con của`[l..r]`, đó là thiên văn. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu đến từ việc quan sát bản chất của sự khác biệt. Giả sử phần tử đầu tiên là`x_1 = l`. Để tối đa hóa độ dài mảng, sự khác biệt sẽ tăng tối thiểu ở mỗi bước: bắt đầu bằng`1`về sự khác biệt đầu tiên, sau đó`2`cho lần tiếp theo, v.v. Điều này đưa ra một chuỗi có dạng`l, l+1, l+1+2, l+1+2+3, ...`hoặc nói chung`l + 1 + 2 + ... + k`. Tổng của số đầu tiên`k`số tự nhiên là`k*(k+1)/2`. Như vậy, tối đa`k`chúng ta có thể phù hợp với`[l, r]`thỏa mãn`l + k*(k+1)/2 - 1 ≤ r`. 

Điều này làm giảm vấn đề giải quyết bất đẳng thức bậc hai cho từng trường hợp thử nghiệm. Cách tiếp cận nhanh vì tính toán lớn nhất`k`thỏa mãn bất đẳng thức có thể được thực hiện trong thời gian không đổi bằng cách sử dụng công thức tính số tam giác hoặc thông qua tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r-l)^2) | O(r-l) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi test case, hãy đọc giới hạn`l`Và`r`. Phần tử đầu tiên của mảng của chúng tôi sẽ là`l`. 
2. Khởi tạo một biến`length`ĐẾN`1`để tính đến phần tử đầu tiên. 
3. Tính phạm vi còn lại có sẵn:`remaining = r - l`. 
4. Tổng các chênh lệch để mở rộng mảng bằng`k`các yếu tố bổ sung là`1 + 2 + ... + k = k*(k+1)/2`. Chúng ta cần số nguyên lớn nhất`k`như vậy`k*(k+1)/2 ≤ remaining`. 
5. Giải quyết`k`sử dụng số học số nguyên. Một cách là tìm kiếm nhị phân giữa`0`Và`√(2*remaining)`. Một cái khác là công thức bậc hai`k = floor((-1 + sqrt(1 + 8*remaining))/2)`. 
6. Thêm`k`ĐẾN`length`. đầu ra`length`. 

Tại sao nó hoạt động: ở mỗi bước, việc chọn sai biệt tiếp theo nhỏ nhất có thể đảm bảo chúng ta có thể mở rộng mảng xa nhất có thể trước khi vượt quá`r`. Tăng chênh lệch đầu tiên hoặc bỏ qua số sẽ làm giảm độ dài tối đa. Công thức`k*(k+1)/2 ≤ remaining`nắm bắt chính xác có bao nhiêu bước phù hợp trong phạm vi cho phép. 

## Giải pháp Python```python
import sys, math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    l, r = map(int, input().split())
    remaining = r - l
    # Solve k*(k+1)/2 <= remaining
    k = int((-1 + math.isqrt(1 + 8*remaining)) // 2)
    print(k + 1)
```Giải pháp đọc nhiều trường hợp thử nghiệm, tính toán phạm vi còn lại sau khi đặt phần tử đầu tiên và giải bất đẳng thức số tam giác.`math.isqrt`đảm bảo căn bậc hai số nguyên chính xác không có lỗi làm tròn dấu phẩy động. Thêm`1`chiếm phần tử ban đầu`l`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`l=1, r=5`| Bước | Còn lại | k*(k+1)/2 ≤ Còn lại | k | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 1+2=3 4 4 | 2 | 3 | 

Sự liên tiếp:`[1, 2, 4]`hoặc`[1,2,5]`. Độ dài tối đa là`3`. 

### Ví dụ 2 

đầu vào:`l=10, r=20`| Bước | Còn lại | k*(k+1)/2 ≤ Còn lại | k | Chiều dài | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 1+2+3+4=10 10 | 4 | 5 | 

Sự liên tiếp:`[10,11,13,16,20]`. Độ dài tối đa là`5`. 

Những dấu vết này xác nhận rằng công thức nắm bắt được số bước tối đa mà không vi phạm giới hạn trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Tính căn bậc hai số nguyên và số học đơn giản cần có thời gian không đổi. | 
| Không gian | O(1) | Không cần mảng hoặc bộ nhớ bổ sung; chỉ một vài số nguyên cho mỗi trường hợp thử nghiệm. | 

Ngay cả đối với`t = 10^4`Và`r-l = 10^9`, thuật toán chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        l, r = map(int, input().split())
        remaining = r - l
        k = int((-1 + math.isqrt(1 + 8*remaining)) // 2)
        output.append(str(k+1))
    return "\n".join(output)

# Provided samples
assert run("5\n1 2\n1 5\n2 2\n10 20\n1 1000000000\n") == "2\n3\n1\n5\n44721", "samples"

# Custom test cases
assert run("1\n1 1\n") == "1", "single element array"
assert run("1\n100 101\n") == "2", "minimum extension"
assert run("1\n1 3\n") == "2", "small range"
assert run("1\n1 10\n") == "4", "medium range"
assert run("1\n1 15\n") == "5", "exact triangular number"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | tay cầm`l=r`| 
| 100 101 | 2 | gia hạn tối thiểu có thể | 
| 1 3 | 2 | phạm vi nhỏ với chiều dài tối đa 2 | 
| 1 10 | 4 | phạm vi trung bình, đảm bảo công thức tính k chính xác | 
| 1 15 | 5 | số tam giác phù hợp chính xác | 

## Vỏ cạnh 

Khi nào`l=r`, bộ thuật toán`remaining = 0`và tính toán`k=0`, thu được chiều dài`1`. Vì`r-l=1`, công thức cho`k=1`, dẫn đến độ dài`2`. Căn bậc hai số nguyên đảm bảo chúng ta không bao giờ vượt quá`k*(k+1)/2 > remaining`. Trong mọi trường hợp, thuật toán tạo ra độ dài tối đa chính xác, được xác nhận bằng cách truy tìm cả phạm vi tối thiểu và tối đa.
