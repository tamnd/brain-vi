---
title: "CF 1686B - Mảng con lẻ"
description: "Chúng ta được hoán vị các số từ 1 đến n. Chúng ta có thể chia hoán vị này thành bất kỳ số phần liên tiếp nào. Mỗi phần là một mảng con. Đối với mỗi mảng con, chúng tôi xem xét số lần đảo ngược của nó. Một mảng con được gọi là số lẻ nếu số lần đảo ngược của nó là số lẻ."
date: "2026-06-09T23:48:13+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1686
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 794 (Div. 2)"
rating: 800
weight: 1686
solve_time_s: 102
verified: true
draft: false
---

[CF 1686B - Mảng con lẻ](https://codeforces.com/problemset/problem/1686/B) 

**Đánh giá:** 800 
**Tags:** dp, tham lam 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ`1`ĐẾN`n`. Chúng ta có thể chia hoán vị này thành bất kỳ số phần liên tiếp nào. Mỗi phần là một mảng con. 

Đối với mỗi mảng con, chúng tôi xem xét số lần đảo ngược của nó. Một mảng con được gọi là số lẻ nếu số lần đảo ngược của nó là số lẻ. Mục tiêu của chúng ta là chọn cách chia sao cho số lượng mảng con lẻ càng lớn càng tốt. 

Đầu ra của mỗi trường hợp thử nghiệm là số mảng con lẻ tối đa có thể có. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta cần một giải pháp rất nhẹ. Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 · 10^5`, có nghĩa là một`O(n)`hoặc`O(n log n)`giải pháp dễ dàng đủ nhanh. Bất kỳ cách tiếp cận nào liên tục tính toán số lượng nghịch đảo của các mảng con đều không thể thực hiện được, vì ngay cả một phép tính nghịch đảo đơn lẻ cũng có thể yêu cầu`O(n log n)`thời gian. 

Có một số tình huống dễ bỏ lỡ. 

Hãy xem xét một hoán vị đã được sắp xếp:```
1 2 3 4
```Mọi mảng con đều có số nghịch đảo bằng 0, tức là số chẵn. Câu trả lời đúng là`0`. Một chiến lược tham lam tạo ra nhiều phân đoạn một cách mù quáng sẽ thất bại vì việc tạo thêm nhiều phân đoạn không tự động tạo ra sự đảo ngược chẵn lẻ lẻ. 

Coi như:```
2 1
```Toàn bộ mảng có chính xác một đảo ngược. Câu trả lời đúng là`1`. Chia thành`[2]`Và`[1]`đưa ra hai mảng con chẵn và mất đoạn lẻ duy nhất. 

Coi như:```
4 3 2 1
```Câu trả lời là`2`, đạt được bởi`[4,3]`Và`[2,1]`. Một cách tiếp cận bất cẩn chỉ kiểm tra tính chẵn lẻ của toàn bộ hoán vị sẽ bỏ sót việc chia tách có thể làm tăng số lượng các phân đoạn lẻ. 

Khó khăn chính là chúng tôi đang tối đa hóa số lượng phân đoạn lẻ chứ không phải tổng số chẵn lẻ đảo ngược của toàn bộ mảng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cách phân chia hoán vị có thể có. có`2^(n-1)`cách đặt các vết cắt giữa các phần tử liền kề, vì vậy điều này đã vô vọng rồi. Ngay cả khi bằng cách nào đó chúng ta tạo ra tất cả các phần tách, chúng ta vẫn cần tính toán chẵn lẻ nghịch đảo cho mỗi mảng con thu được. 

Lý do vũ lực đúng về mặt khái niệm rất đơn giản: mọi phân vùng hợp lệ đều được kiểm tra, vì vậy cuối cùng câu trả lời tốt nhất sẽ được tìm thấy. Vấn đề là không gian tìm kiếm rất lớn. 

Để tìm ra giải pháp tốt hơn, chúng ta cần hiểu điều gì làm cho một mảng con trở nên kỳ lạ. 

Một quan sát rất hữu ích xuất phát từ việc xem xét các mảng con có độ dài bằng hai. 

Nếu hai phần tử liền kề tạo thành một cặp giảm dần thì```
p[i] > p[i+1]
```sau đó là mảng con`[p[i], p[i+1]]`chứa đúng một phép nghịch đảo, nên nó là số lẻ. 

Điều này ngay lập tức mang lại cho chúng ta một phân đoạn lẻ được đảm bảo. 

Bây giờ giả sử chúng ta gặp một cặp liền kề giảm dần như vậy. Có lý do nào để giữ một trong hai phần tử cùng với các vị trí lân cận không? 

Không. 

Nếu chúng ta tách hai phần tử này thành một phân đoạn, chúng ta sẽ thu được ngay một mảng con lẻ. Bất kỳ phân đoạn lớn hơn nào chứa chúng đều đóng góp tối đa một phân đoạn lẻ, bởi vì mỗi phân đoạn chỉ được tính một lần bất kể nó chứa bao nhiêu đảo ngược. 

Vì vậy, bất cứ khi nào chúng ta thấy một cặp liền kề giảm dần, chúng ta nên lấy hai phần tử đó làm phân đoạn riêng và đếm một câu trả lời. 

Sau khi sử dụng các vị trí`i`Và`i+1`, không vị trí nào có thể tham gia vào phân khúc khác. Chúng tôi bỏ qua cả hai và tiếp tục. 

Điều này trở thành một quá trình tham lam đơn giản. 

Tại sao nó tối ưu? Bởi vì mỗi đoạn lẻ phải chứa ít nhất một phép đảo ngược. Trong một hoán vị, mỗi chuỗi nghịch đảo chứa một số cặp giảm liền kề. Bất cứ khi nào chúng ta tìm thấy một cặp như vậy, việc coi nó ngay lập tức là một đoạn lẻ có độ dài bằng hai sẽ sử dụng số phần tử nhỏ nhất có thể để thu được một mảng con lẻ. Không có phân vùng nào khác có thể trích xuất nhiều hơn một phân đoạn lẻ từ hai vị trí đó. 

Toàn bộ vấn đề rút gọn lại ở việc đếm xem chúng ta có thể tham lam thực hiện bao nhiêu nghịch đảo liền kề rời rạc. 

## So sánh các phương pháp tiếp cận 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời cho`0`. 
2. Bắt đầu quét hoán vị từ trái sang phải bằng chỉ mục`i`. 
3. Nếu`p[i] > p[i+1]`, thì cặp này tạo thành một mảng con có độ dài bằng hai với đúng một phép đảo ngược. 
4. Đếm cặp này là một mảng con lẻ và tăng kết quả lên thêm`1`. 
5. Bỏ qua cả hai vị trí bằng cách di chuyển`i`chuyển tiếp bởi`2`. 

Chúng tôi làm điều này vì hai phần tử này đã được cam kết cho một phân đoạn lẻ tối ưu và việc sử dụng lại một trong hai phần tử không thể làm tăng tổng số lượng. 
6. Nếu không, hãy di chuyển`i`chuyển tiếp bởi`1`. 

Không thể thu được đoạn lẻ chỉ từ cặp liền kề này. 
7. Tiếp tục cho đến khi còn lại ít hơn hai phần tử. 
8. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Bất cứ khi nào sự đảo ngược tồn tại bên trong một phân đoạn thì phải có ít nhất một cặp giảm liền kề ở đâu đó bên trong phân đoạn đó. Do đó, một đoạn được tính là số lẻ phải chứa ít nhất một đoạn đảo ngược liền kề. 

Khi chúng ta gặp một sự đảo ngược liền kề`p[i] > p[i+1]`, đoạn hai phần tử chỉ gồm các vị trí này đã là số lẻ. Sử dụng chính xác hai phần tử để thu được một phân đoạn lẻ là tối ưu vì không phân vùng nào có thể thu được nhiều hơn một mảng con lẻ từ hai vị trí giống nhau. 

Sau khi chọn một cặp như vậy, bất kỳ giải pháp tối ưu nào cũng có thể được chuyển thành giải pháp tách biệt cặp này mà không làm giảm số lượng phân đoạn lẻ. Do đó, việc thực hiện mọi đảo ngược liền kề có sẵn một cách tham lam từ trái sang phải không bao giờ ảnh hưởng đến các lựa chọn trong tương lai. 

Các cặp được chọn là rời rạc và mỗi cặp đóng góp chính xác một mảng con lẻ. Do đó số lượng tham lam là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        res = 0
        i = 0

        while i < n - 1:
            if p[i] > p[i + 1]:
                res += 1
                i += 2
            else:
                i += 1

        ans.append(str(res))

    sys.stdout.write("\n".join(ans))

solve()
```Thuật toán duy trì một con trỏ`i`quét hoán vị một lần. 

Bất cứ khi nào`p[i] > p[i+1]`, chúng tôi đã tìm thấy một sự đảo ngược liền kề. Bản thân cặp này tạo thành một mảng con lẻ, vì vậy chúng ta đếm nó và bỏ qua cả hai vị trí. 

Bỏ qua là phần tinh tế. Nếu chúng ta chỉ di chuyển về phía trước một vị trí, các cặp chồng chéo có thể được tính nhiều lần. Một phần tử không thể thuộc về hai phân đoạn khác nhau trong một phân vùng, do đó các lựa chọn chồng chéo sẽ không hợp lệ. 

Khi cặp này tăng lên, không có đoạn lẻ có độ dài bằng hai tồn tại ở đó, vì vậy chúng ta chỉ cần tiến lên một vị trí. 

Vì mỗi chỉ mục được truy cập nhiều nhất một lần nên thời gian chạy là tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 3 2 1
```| tôi | Cặp | Đảo ngược liền kề? | Trả lời | 
| --- | --- | --- | --- | 
| 0 | (4,3) | Có | 1 | 
| 2 | (2,1) | Có | 2 | 

Câu trả lời cuối cùng:`2`. 

Ví dụ này cho thấy hành vi tham lam chính. Mỗi cặp giảm dần sẽ trở thành đoạn lẻ của riêng nó. 

### Ví dụ 2 

đầu vào:```
6
4 5 6 1 2 3
```| tôi | Cặp | Đảo ngược liền kề? | Trả lời | 
| --- | --- | --- | --- | 
| 0 | (4,5) | Không | 0 | 
| 1 | (5,6) | Không | 0 | 
| 2 | (6,1) | Có | 1 | 
| 4 | (2,3) | Không | 1 | 

Câu trả lời cuối cùng:`1`. 

Sự đảo ngược liền kề duy nhất là`(6,1)`, do đó chỉ có một phân đoạn lẻ có thể được đảm bảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét một lần từ trái sang phải | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Tổng độ dài của tất cả các hoán vị nhiều nhất là`2 · 10^5`, do đó việc quét tuyến tính trên mọi trường hợp kiểm thử sẽ dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ vẫn không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        ans = 0
        i = 0

        while i < n - 1:
            if p[i] > p[i + 1]:
                ans += 1
                i += 2
            else:
                i += 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""5
3
1 2 3
4
4 3 2 1
2
1 2
2
2 1
6
4 5 6 1 2 3
"""
) == """0
2
0
1
1"""

# minimum size
assert run(
"""1
1
1
"""
) == "0"

# single adjacent inversion
assert run(
"""1
2
2 1
"""
) == "1"

# overlapping inversions
assert run(
"""1
3
3 2 1
"""
) == "1"

# alternating pattern
assert run(
"""1
6
2 1 4 3 6 5
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Hoán vị kích thước tối thiểu | 
|`2 1`|`1`| Đoạn lẻ đơn | 
|`3 2 1`|`1`| Không thể sử dụng cả hai đảo ngược chồng chéo | 
|`2 1 4 3 6 5`|`3`| Nhiều đảo ngược liền kề rời rạc | 

## Vỏ cạnh 

Hãy xem xét hoán vị nhỏ nhất có thể:```
1
1
```Quá trình quét không bao giờ đi vào vòng lặp vì không có cặp liền kề. Câu trả lời vẫn còn`0`, điều này đúng vì mọi mảng con có độ dài một đều không có phép đảo ngược. 

Coi như:```
1
3
3 2 1
```Quá trình quét nhìn thấy`(3,2)`và đếm một đoạn lẻ. Sau đó nó bỏ qua đến cuối. Câu trả lời là`1`. 

Một lỗi phổ biến là tính cả hai`(3,2)`Và`(2,1)`, sản xuất`2`. Điều đó sẽ đòi hỏi vị trí`2`thuộc về hai phân khúc khác nhau, đó là điều không thể. 

Xét một hoán vị tăng đầy đủ:```
1
5
1 2 3 4 5
```Không tồn tại sự đảo ngược liền kề. Thuật toán không bao giờ tăng câu trả lời và trả về`0`. Mỗi mảng con đều có số lượng đảo ngược chẵn, vì vậy điều này là chính xác. 

Coi như:```
1
4
4 3 2 1
```Thuật toán tính`(4,3)`Và`(2,1)`riêng biệt và trả về`2`. Các cặp này rời rạc nên cả hai có thể đồng thời trở thành các đoạn lẻ. Điều này chứng tỏ tại sao bỏ qua hai vị trí sau khi chọn một cặp là hành vi đúng.
