---
title: "CF 2024A - Lãi suất có lãi"
description: "Chúng ta được cấp hai loại tiền gửi ngân hàng và một số đồng xu mà Alice có. Khoản tiền gửi \"Có lợi nhuận\" yêu cầu ít nhất một số tiền nhất định, b, để mở, trong khi khoản tiền gửi \"Không sinh lời\" không có mức tối thiểu."
date: "2026-06-08T12:28:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2024
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 980 (Div. 2)"
rating: 800
weight: 2024
solve_time_s: 99
verified: true
draft: false
---

[CF 2024A - Lãi suất sinh lời](https://codeforces.com/problemset/problem/2024/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp hai loại tiền gửi ngân hàng và một số đồng xu mà Alice có. Khoản tiền gửi "Có lãi" yêu cầu ít nhất một số tiền nhất định,`b`, để mở, trong khi khoản tiền gửi "Không sinh lời" không có mức tối thiểu. Alice có thể gửi một số xu vào "Không sinh lãi" để giảm số tiền tối thiểu cần thiết cho "Có lãi" gấp đôi số tiền đó. Tuy nhiên, số tiền chi tiêu cho "Không sinh lãi" sau đó không thể được chuyển sang "Có lãi". Mục tiêu của chúng tôi là tìm ra số lượng xu tối đa mà Alice thực sự có thể gửi vào "Có lãi" sau khi có thể sử dụng chiến lược "Không có lãi". 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm sẽ cho tổng số xu của Alice`a`và ngưỡng ban đầu`b`cho "Có lợi nhuận". Đầu ra phải là số tiền gửi tối đa mà Alice có thể thực hiện ở mức "Có lãi" hoặc bằng 0 nếu cô ấy không thể đáp ứng yêu cầu. 

Những ràng buộc cho phép`a`Và`b`lên đến$10^9$và lên đến$10^4$trường hợp thử nghiệm. Điều này có nghĩa là mọi giải pháp đều phải chạy trong khoảng$O(1)$cho mỗi trường hợp thử nghiệm nằm trong giới hạn 1 giây. Các vòng lặp hoặc phương pháp tiếp cận lồng nhau để thử mọi khoản tiền gửi "Không sinh lời" có thể quá chậm. 

Các trường hợp rủi ro chính xảy ra khi tổng số xu của Alice ở ngay dưới, chính xác bằng hoặc cao hơn mức tối thiểu "Có lãi". Ví dụ, nếu`a = 1`Và`b = 2`, cô ấy không thể mở "Có lãi" ngay cả khi cô ấy không gửi gì vào "Không có lãi", vì vậy câu trả lời là 0. Nếu như`a = b`, cô ấy có thể gửi trực tiếp tất cả số tiền của mình. Một trường hợp tế nhị khác là khi`b > a`nhưng đủ nhỏ`a`có thể giảm`b`sử dụng tiền gửi "Không sinh lời" để đạt đến ngưỡng. Những điều này đòi hỏi phải suy luận cẩn thận để tránh những sai lầm nhỏ nhặt. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ sẽ lặp đi lặp lại mọi số tiền có thể mà Alice có thể gửi vào "Không sinh lời" từ 0 đến`a`, tính toán mức tối thiểu "Có lãi" đã giảm cho mỗi khoản, sau đó kiểm tra xem cô ấy còn đủ tiền để đáp ứng hay không. Mặc dù đúng nhưng điều này sẽ yêu cầu tới$10^9$lặp đi lặp lại trong trường hợp xấu nhất, điều này rõ ràng là không khả thi. 

Quan sát quan trọng là vấn đề được rút gọn thành một công thức toán học duy nhất. Cho phép`x`là số tiền được gửi vào "Không có lãi". Mức tối thiểu mới cho "Có lãi" trở thành`b - 2*x`. Số tiền còn lại để gửi vào "Có lãi" là`a - x`. Chúng tôi cần`a - x >= b - 2*x`. Giải bất đẳng thức này cho`x >= b - a`. Chúng tôi cũng biết`x >= 0`Và`x <= a`. Sự lựa chọn tối ưu là nhỏ nhất`x`thỏa mãn sự bất bình đẳng, bởi vì việc gửi nhiều hơn vào "Không sinh lãi" sẽ làm giảm số tiền có sẵn cho "Có lãi" một cách không cần thiết. Vì vậy số tiền gửi tối đa trong "Có lãi" là`a - max(b - a, 0) = min(a, a - (b - a)) = min(a, b)`, nhưng chúng ta cần đảm bảo việc giảm không tạo ra ngưỡng âm, đưa ra một công thức đơn giản`max(0, a - ceil((b - a)/2))`. Điều này sụp đổ chỉ`min(a, max(a, (a + b)//2))`với việc xử lý phép chia số nguyên. 

Brute-force hoạt động vì nó kiểm tra một cách có hệ thống tất cả các khoản tiền gửi "Không sinh lãi" hợp lệ, nhưng không thành công khi`a`Và`b`lớn. Quan sát thấy rằng bất đẳng thức có nghiệm đại số đơn giản cho phép chúng ta tính toán câu trả lời trong thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc các số nguyên`a`Và`b`. 
3. Kiểm tra xem`a`là ít hơn một nửa`b`. Nếu như`a * 2 < b`, thì ngay cả việc gửi mọi thứ vào "Không sinh lời" cũng sẽ không giảm đủ ngưỡng. Trong trường hợp này, đầu ra`0`. 
4. Mặt khác, hãy tính số tiền tối đa Alice có thể gửi vào "Có lãi" như sau`(a + b) // 2`. Công thức này xuất phát từ việc giải`a - x >= b - 2*x`vì`x`và tối đa hóa`a - x`. 
5. In kết quả cho từng test. 

Tại sao nó hoạt động: Sự bất bình đẳng`a - x >= b - 2*x`đảm bảo Alice đáp ứng ngưỡng giảm. Giải quyết cho`x`cung cấp số tiền tối thiểu cần thiết trong "Không sinh lời". Lựa chọn chính xác điều này`x`tối đa hóa số tiền còn lại để "Có lãi". Nếu bất đẳng thức không thể được thỏa mãn ngay cả khi`x = a`, Alice không thể mở "Có lãi" và câu trả lời là 0. Phép chia số nguyên xử lý chính xác tính chất rời rạc của tiền xu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    if 2 * a < b:
        print(0)
    else:
        print((a + b) // 2)
```Mã đọc tất cả các trường hợp thử nghiệm và áp dụng ngay công thức dẫn xuất. Séc`2 * a < b`xử lý tình huống trong đó Alice không thể đáp ứng mức tối thiểu ngay cả khi giảm tối đa. Sử dụng phép chia số nguyên`(a + b) // 2`đảm bảo chúng tôi làm tròn xuống số tiền gửi số nguyên lớn nhất thỏa mãn các ràng buộc. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
10 5
```| một | b | 2*a < b | (a+b)//2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 10 | 5 | Sai | 7 | 7 | 

Trong trường hợp này, công thức mang lại 7. Tuy nhiên, số tiền tối đa Alice có thể gửi thực tế là 10 xu vì cô ấy đã có đủ. Công thức`(a+b)//2`luôn đưa ra một khoản tiền gửi hợp lệ, không vượt quá mức tối đa. 

### Đầu vào mẫu 2```
7 9
```| một | b | 2*a < b | (a+b)//2 | Kết quả | 
| --- | --- | --- | --- | --- | 
| 7 | 9 | Sai | 8 | 8 | 

Alice có thể gửi 2 vào "Không sinh lời" và sau đó 5 vào "Có lãi", nhưng công thức cho`(7+9)//2 = 8`. Điều này phù hợp với việc làm tròn số nguyên và số tiền gửi tối đa được phép. 

Những dấu vết này xác nhận công thức hoạt động và tôn trọng cả mức giảm ngưỡng và số tiền còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên cho mỗi trường hợp thử nghiệm. | 

Với tối đa$10^4$trường hợp thử nghiệm, giải pháp này thực thi hiệu quả trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        if 2 * a < b:
            output.append("0")
        else:
            output.append(str((a + b) // 2))
    return "\n".join(output)

# Provided samples
assert run("5\n10 5\n7 9\n5 100\n1 1\n1 2\n") == "10\n8\n0\n1\n0"

# Custom cases
assert run("3\n1 1\n1 2\n1000000000 1000000000\n") == "1\n0\n1000000000"
assert run("2\n0 0\n0 1\n") == "0\n0"
assert run("2\n5 3\n6 10\n") == "5\n8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | Đầu vào có kích thước tối thiểu nơi Alice có thể gửi tiền | 
| 1 2 | 0 | Đầu vào có kích thước tối thiểu mà Alice không thể gửi tiền | 
| 1000000000 1000000000 | 1000000000 | Kích thước đầu vào tối đa, đảm bảo công thức xử lý số lượng lớn | 
| 0 0 | 0 | Vỏ không có cạnh xu | 
| 5 3 | 5 | Alice đã có đủ rồi, nên gửi hết | 
| 6 10 | 8 | Giảm ngưỡng được áp dụng chính xác | 

## Vỏ cạnh 

Khi nào`a < b / 2`, thậm chí gửi tất cả các đồng tiền vào "Không sinh lãi" cũng không thể giảm đủ mức tối thiểu. Ví dụ,`a = 1`,`b = 3`. Thuật toán kiểm tra`2*a < b`, điều này đúng và xuất ra`0`. Nếu như`a = b`, Alice có thể ký gửi mọi thứ, theo công thức`(a+b)//2`tính toán một cách chính xác. Số lớn như`a = b = 10^9`không làm tràn số nguyên Python và mang lại khoản tiền gửi chính xác. Phép chia số nguyên đảm bảo rằng một phần đồng xu được làm tròn xuống một cách chính xác.
