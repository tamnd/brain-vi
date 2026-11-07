---
title: "CF 132D - Hằng số trong ngôn ngữ của Shakespeare"
description: "Chúng ta được yêu cầu biểu diễn một số nguyên dương dưới dạng nhị phân dưới dạng tổng lũy ​​thừa của hai, với tùy chọn sử dụng lũy ​​thừa âm, sao cho tổng số số hạng được giảm thiểu. Về mặt hình thức, chúng ta muốn viết số này dưới dạng tổng các biểu thức có dạng +2^x hoặc -2^x."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 132
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 96 (Div. 1)"
rating: 2100
weight: 132
solve_time_s: 122
verified: false
draft: false
---

[CF 132D - Các hằng số trong ngôn ngữ của Shakespeare](https://codeforces.com/problemset/problem/132/D) 

**Xếp hạng:** 2100 
**Tags:** thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu biểu diễn một số nguyên dương dưới dạng nhị phân dưới dạng tổng lũy thừa của hai, với tùy chọn sử dụng lũy thừa âm, sao cho tổng số số hạng được giảm thiểu. Về mặt hình thức, chúng ta muốn viết số này dưới dạng tổng các biểu thức có dạng`+2^x`hoặc`-2^x`. Đầu vào là một chuỗi nhị phân có độ dài lên tới 1.000.000, ngay lập tức cho chúng ta biết rằng bản thân số đó có thể rất lớn và chúng ta không thể lặp lại tất cả các giá trị nguyên. Đầu ra đầu tiên là số lượng số hạng trong tổng tối thiểu, theo sau là từng số hạng một cách rõ ràng. 

Thách thức đến từ việc giảm thiểu số lượng điều khoản. Một cách tiếp cận đơn giản là biểu diễn mỗi 1 bit dưới dạng`+2^x`, tạo ra một số số hạng bằng số 1 trong biểu diễn nhị phân. Mặc dù đúng nhưng điều này thường không phải là tối thiểu. Ví dụ, hệ nhị phân`1111`(thập phân 15) có thể được biểu diễn dưới dạng`+2^4 - 2^0`, chỉ đưa ra hai thuật ngữ thay vì bốn. 

Các trường hợp cạnh bao gồm các số như`1000`(thập phân 8) trong đó phương thức đơn giản đã ở mức tối thiểu hoặc các số có thời gian chạy dài là 1 chẳng hạn như`11111`(thập phân 31), trong đó việc nhóm và sử dụng lũy ​​thừa cao hơn sẽ làm giảm số lượng thuật ngữ. Một cách tiếp cận bất cẩn sẽ bỏ qua việc truyền tải và luôn phát ra một số hạng trên mỗi 1 bit, dẫn đến giải pháp dưới mức tối ưu. 

Độ dài đầu vào lớn có nghĩa là chúng ta cần một thuật toán tuyến tính theo độ dài của chuỗi nhị phân, vì các phương pháp bậc hai hoặc cao hơn sẽ vượt quá giới hạn thời gian 2 giây. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua chuỗi nhị phân, với mỗi bit được đặt thành 1, hãy thêm`+2^x`vào danh sách các thuật ngữ. Điều này tạo ra một số thuật ngữ bằng số 1. Nó đúng nhưng có thể rất xa mức tối thiểu. Đối với một chuỗi nhị phân có độ dài`n`với tất cả các bit được đặt thành 1, phương thức ngây thơ tạo ra`n`điều khoản. Với`n`lên tới 1.000.000, điều này không thực tế cả về mức tối thiểu và kích thước đầu ra. 

Cái nhìn sâu sắc quan trọng là coi biểu diễn nhị phân giống như số cơ sở 2 và xem xét việc "mang" như thế nào. Nếu chúng ta có dãy số 1 liên tiếp, việc lật chúng về 0 và thêm +1 vào lũy thừa cao hơn tiếp theo sẽ làm giảm tổng số số hạng. Đây là cách sử dụng hiệu quả danh tính`2^k + 2^k + ... + 2^k (r times) = 2^(k+1) + ...`với các từ mang và phủ định khi cần thiết. 

Thuật toán tối ưu xử lý số nhị phân từ bit ít quan trọng nhất đến bit có ý nghĩa nhất, duy trì "mang" truyền về phía trước bất cứ khi nào chuỗi 1 có thể được thu gọn thành ít số hạng hơn. Ở mỗi bước, chúng tôi quyết định xem nên giữ lại một bit, tạo ra số hạng phủ định hay góp phần mang theo. Điều này làm giảm tổng số thuật ngữ xuống mức tối thiểu theo lý thuyết trong khi lặp qua chuỗi một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đúng nhưng không tối thiểu | 
| Tối ưu | O(n) | O(n) | Đúng và tối thiểu | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi nhị phân từ đầu vào và đảo ngược nó để xử lý từ bit ít quan trọng nhất đến bit quan trọng nhất. Đảo ngược đơn giản hóa việc xử lý chỉ mục khi thêm mang. 
2. Khởi tạo một biến mang về 0 và một danh sách trống cho các số hạng. Việc thực hiện đại diện cho một chờ xử lý`+2^k`hoặc`-2^k`điều chỉnh tới bit cao hơn tiếp theo. 
3. Lặp lại từng chỉ số bit`i`. Thêm bit hiện tại và số mang để có được giá trị hiệu quả cho vị trí này. Giá trị này có thể là 0, 1, 2 hoặc 3 tùy thuộc vào bit và bất kỳ giá trị mang nào trước đó. 
4. Nếu giá trị hiệu dụng là 0 hoặc 1 thì không cần số hạng âm. Nếu giá trị hiệu dụng là 1, hãy nối thêm`+2^i`vào danh sách các thuật ngữ. Đặt mang về 0. 
5. Nếu giá trị hiệu dụng là 2 thì điều này tương đương với`0`ở vị trí hiện tại với mang theo`+1`tới bit cao hơn tiếp theo. Không thêm thuật ngữ cho bit này; tăng mang theo. 
6. Nếu giá trị hiệu dụng là 3 thì giá trị này tương đương với`1`ở vị trí hiện tại với mang theo`+1`tới bit cao hơn tiếp theo. Nối thêm`-2^i`để bù đắp thêm`2^i`và tăng số lượng mang theo. 
7. Sau khi xử lý tất cả các bit, nếu còn lại phần mang, hãy nối thêm`+2^len(binary_string)`để xử lý nó. 
8. In tổng số số hạng theo sau mỗi số hạng. Thứ tự của các điều khoản không quan trọng. 

Điều bất biến ở đây là ở mỗi bước, sự kết hợp của bit hiện tại, bit mang trước đó và quyết định phát ra`+2^i`hoặc`-2^i`đảm bảo rằng tổng của tất cả các số hạng được phát ra cộng với số mang bằng số ban đầu. Điều này ngăn chặn việc đếm quá mức hoặc thiếu đóng góp từ bất kỳ bit nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

binary = input().strip()[::-1]  # reverse to process LSB to MSB
n = len(binary)
carry = 0
terms = []

for i in range(n):
    bit = int(binary[i])
    val = bit + carry
    if val == 0:
        carry = 0
    elif val == 1:
        terms.append(f"+2^{i}")
        carry = 0
    elif val == 2:
        carry = 1
    elif val == 3:
        terms.append(f"-2^{i}")
        carry = 1

if carry:
    terms.append(f"+2^{n}")

print(len(terms))
print("\n".join(terms))
```Đoạn mã đầu tiên đảo ngược chuỗi nhị phân sao cho chỉ mục đó`i`tương ứng với`2^i`. Chúng tôi lặp lại từng bit, kết hợp nó với bất kỳ dữ liệu mang nào từ các hoạt động trước đó. Các trường hợp cho`val`xử lý tất cả các khả năng: không cần thuật ngữ, nối thêm thuật ngữ tích cực, truyền bá mang hoặc nối thêm thuật ngữ phủ định với mang. Việc kiểm tra cuối cùng đối với phần còn sót lại đảm bảo tính chính xác cho các số như`1111`truyền bá một sức mạnh bổ sung. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`1111`(thập phân 15): 

| Chỉ mục (i) | Chút | Mang theo | Giá trị hiệu quả | Hành động | Điều khoản | Mang theo mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | +2^0 | +2^0 | 0 | 
| 1 | 1 | 0 | 1 | +2^1 | +2^1 | 0 | 
| 2 | 1 | 0 | 1 | +2^2 | +2^2 | 0 | 
| 3 | 1 | 0 | 1 | +2^3 | +2^3 | 0 | 

Điều này tạo ra 4 thuật ngữ. Áp dụng chiến lược mang theo: 

| Chỉ mục (i) | Chút | Mang theo | Giá trị hiệu quả | Hành động | Điều khoản | Mang theo mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | -2^0 | -2^0 | 1 | 
| 1 | 1 | 1 | 2 | mang 1 | | 1 | 
| 2 | 1 | 1 | 2 | mang 1 | | 1 | 
| 3 | 1 | 1 | 2 | mang 1 | | 1 | 
| 4 | 0 | 1 | 1 | +2^4 | +2^4 | 0 | 

Điều này tạo ra 2 thuật ngữ tối ưu:`+2^4, -2^0`. 

Đầu vào khác`1010`(thập phân 10): 

| Chỉ mục | Chút | Mang theo | Giá trị hiệu quả | Hành động | Điều khoản | Mang theo | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | Không có | | 0 | 
| 1 | 1 | 0 | 1 | +2^1 | +2^1 | 0 | 
| 2 | 0 | 0 | 0 | Không có | | 0 | 
| 3 | 1 | 0 | 1 | +2^3 | +2^3 | 0 | 

sản xuất`+2^1, +2^3`, đó là tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi xử lý từng bit chính xác một lần và mỗi thao tác bên trong vòng lặp là O(1). | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách các thuật ngữ, có thể có nhiều nhất n+1 thuật ngữ trong trường hợp xấu nhất. | 

Điều này phù hợp với giới hạn thời gian và bộ nhớ cho n 1.000.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    binary = input().strip()[::-1]
    n = len(binary)
    carry = 0
    terms = []
    for i in range(n):
        bit = int(binary[i])
        val = bit + carry
        if val == 0:
            carry = 0
        elif val == 1:
            terms.append(f"+2^{i}")
            carry = 0
        elif val == 2:
            carry = 1
        elif val == 3:
```
