---
title: "CF 2031B - Que Penchick và Satay"
description: "Chúng ta có một hoán vị của các số từ $1$ đến $n$. Mục tiêu là hoán vị được sắp xếp $[1,2,dots,n]$. Hoạt động duy nhất được phép hoán đổi hai vị trí lân cận khi các giá trị khác nhau chính xác $1$. Ví dụ: $3$ và $4$ có thể được hoán đổi, nhưng $3$ và $5$ thì không."
date: "2026-06-08T11:51:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2031
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 987 (Div. 2)"
rating: 900
weight: 2031
solve_time_s: 120
verified: true
draft: false
---

[CF 2031B - Que Penchick và Satay](https://codeforces.com/problemset/problem/2031/B) 

**Xếp hạng:** 900 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có hoán vị của số$1$bởi vì$n$. Mục tiêu là hoán vị được sắp xếp$[1,2,\dots,n]$. 

Hoạt động duy nhất được phép hoán đổi hai vị trí lân cận khi các giá trị khác nhau một cách chính xác$1$. Ví dụ,$3$Và$4$có thể được hoán đổi, nhưng$3$Và$5$có thể không. 

Nhiệm vụ là xác định xem hoán vị có thể được chuyển đổi thành thứ tự được sắp xếp bằng cách sử dụng bất kỳ số lần hoán đổi nào như vậy hay không. 

Những hạn chế là lớn. Trên tất cả các trường hợp thử nghiệm, tổng chiều dài tối đa là$2 \cdot 10^5$. Bất kỳ thuật toán nào mô phỏng nhiều lần hoán đổi hoặc quét mảng liên tục sẽ trở nên quá chậm. Phương pháp bậc hai bị loại trừ vì$n^2$hoạt động sẽ đạt khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Trường hợp cạnh tinh tế là một cặp liền kề bị đảo ngược. Coi như:```
2 1 3 4
```Các giá trị$2$Và$1$khác nhau một cách chính xác$1$, vì vậy chúng có thể được hoán đổi trực tiếp. Câu trả lời là`YES`. 

Một trường hợp quan trọng khác là:```
3 1 2
```giá trị$3$phải di chuyển sang phải, nhưng nó không bao giờ có thể hoán đổi với một trong hai$1$hoặc$2$, bởi vì sự khác biệt là$2$Và$1$chỉ sau$1$Và$2$trở nên liền kề. Câu trả lời đúng là`NO`. Một cách tiếp cận ngây thơ chỉ kiểm tra số lần đảo ngược sẽ bỏ lỡ điều này. 

Một ví dụ đặc biệt tiết lộ là:```
4 2 3 1
```Cặp đôi$(2,3)$có thể trao đổi mãi mãi, nhưng giá trị$4$cũng không bao giờ có thể vượt qua$2$hoặc$1$. Hoán vị không thể sắp xếp được nên đáp án là`NO`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi lần hoán đổi hợp lệ là một cạnh trong biểu đồ trạng thái và tìm kiếm xem hoán vị được sắp xếp có thể truy cập được hay không. Điều này đúng vì mọi chuỗi hoạt động hợp pháp đều tương ứng với một đường dẫn trong biểu đồ đó. 

Thật không may, có$n!$hoán vị. Ngay cả đối với mức độ vừa phải$n$, việc khám phá các trạng thái trở nên không thể. 

Quan sát quan trọng là chỉ các giá trị liên tiếp mới có thể hoán đổi. 

Xét hai giá trị có hiệu vượt quá$1$, chẳng hạn như$2$Và$5$. Chúng không bao giờ có thể được hoán đổi trực tiếp. Vì mọi phép toán được phép chỉ trao đổi các số liên tiếp nên thứ tự tương đối của$2$Và$5$không bao giờ có thể thay đổi. 

Điều này có nghĩa là với mỗi cặp$x < y-1$, thứ tự của chúng là bất biến trong toàn bộ quá trình. 

Các cặp duy nhất có thứ tự tương đối có thể thay đổi là các giá trị liên tiếp:$$(1,2),\ (2,3),\ (3,4),\ \dots,\ (n-1,n).$$Giả sử một giá trị cách xa vị trí của nó nhiều hơn một vị trí. Việc sửa lỗi đó sẽ yêu cầu thay đổi thứ tự của nó so với một số giá trị không liên tiếp, điều này là không thể. 

Điều này ngay lập tức dẫn đến một đặc tính đơn giản. Mọi phần tử phải ở vị trí riêng hoặc liền kề với nó. Trong ký hiệu hoán vị,$$|p_i - i| \le 1$$cho mọi vị trí$i$. 

Nếu điều kiện này được giữ thì hoán vị chỉ bao gồm các phép đảo ngược liền kề rời rạc như$$[2,1,4,3,5].$$Mỗi sự đảo ngược bao gồm các giá trị liên tiếp và có thể được sửa chữa độc lập bằng một lần hoán đổi hợp pháp. 

Nếu điều kiện không thành công ở bất cứ đâu thì việc sắp xếp là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị. 
2. Quét mọi vị trí$i$. 
3. Kiểm tra xem$$|p_i - (i+1)| \le 1.$$Mảng được lập chỉ mục 0 trong mã, do đó vị trí chính xác của giá trị$i+1$được đại diện bởi$i+1$. 

1. Nếu bất kỳ vị trí nào vi phạm điều kiện này, xuất ra`NO`. 

Một giá trị cách xa đích đến của nó từ hai vị trí trở lên sẽ cần phải vượt qua một giá trị không liên tiếp, điều này không bao giờ có thể xảy ra. 

1. Nếu tất cả các vị trí đều thỏa mãn điều kiện, xuất ra`YES`. 

Hoán vị sau đó bao gồm hoàn toàn các điểm cố định và các giao dịch hoán đổi liền kề của các giá trị liên tiếp, tất cả đều có thể được sửa chữa một cách hợp pháp. 

### Tại sao nó hoạt động 

Thứ tự tương đối của hai giá trị bất kỳ có chênh lệch vượt quá$1$không bao giờ thay đổi, bởi vì chúng không bao giờ có thể tham gia vào một sự hoán đổi pháp lý. 

Giả sử một số phần tử$x$nằm cách vị trí cuối cùng của nó ít nhất hai vị trí. Để đến đích,$x$phải vượt qua một số giá trị$y$với$|x-y|>1$. Điều đó đòi hỏi phải thay đổi thứ tự tương đối của chúng, điều này là không thể. 

Do đó mọi hoán vị có thể sắp xếp phải thỏa mãn$|p_i-i|\le1$trong ký hiệu 1 chỉ mục. 

Ngược lại, nếu mỗi phần tử cách vị trí chính xác của nó nhiều nhất một vị trí thì hoán vị là một tập hợp các đảo ngược liền kề rời rạc$(i+1,i)$. Mỗi sự đảo ngược như vậy bao gồm các giá trị liên tiếp và có thể được sửa bằng một lần hoán đổi pháp lý duy nhất. Thực hiện các giao dịch hoán đổi này sẽ sắp xếp hoán vị. 

Vì vậy điều kiện vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        ok = True
        for i, x in enumerate(p, start=1):
            if abs(x - i) > 1:
                ok = False
                break

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp kiểm tra đặc tính đã được chứng minh ở trên. 

Vòng lặp sử dụng`enumerate(..., start=1)`sao cho chỉ số khớp với các vị trí dựa trên 1 tự nhiên của hoán vị. Điều này tránh lặp lại`+1`điều chỉnh và loại bỏ một lỗi thường gặp. 

Quá trình quét dừng ngay sau khi phát hiện vi phạm. Không cần thêm thông tin khi một phần tử cách mục tiêu của nó nhiều hơn một vị trí. 

Không có mô phỏng nào được thực hiện. Chứng minh cho thấy điều kiện dịch chuyển hoàn toàn quyết định đáp án. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 3 4
```| Vị trí | Giá trị |$|p_i-i|$| 

|---|---:|---:| 

| 1 | 2 | 1 | 

| 2 | 1 | 1 | 

| 3 | 3 | 0 | 

| 4 | 4 | 0 | 

Tất cả các giá trị đều thỏa mãn điều kiện. 

Đầu ra:```
YES
```Sự đảo ngược$(2,1)$liên quan đến các giá trị liên tiếp, vì vậy một hoán đổi hợp pháp sẽ sắp xếp mảng. 

### Ví dụ 2 

đầu vào:```
4 2 3 1
```| Vị trí | Giá trị |$|p_i-i|$| 

|---|---:|---:| 

| 1 | 4 | 3 | 

| 2 | 2 | 0 | 

| 3 | 3 | 0 | 

| 4 | 1 | 3 | 

Vị trí đầu tiên đã vi phạm điều kiện. 

Đầu ra:```
NO
```giá trị$4$cách nơi nó thuộc về ba vị trí. Việc khắc phục điều đó sẽ yêu cầu vượt qua các giá trị có sự khác biệt với$4$vượt quá$1$, điều đó là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần đi qua hoán vị | 
| Không gian |$O(1)$| Chỉ có một vài biến ngoài mảng đầu vào | 

Tổng của tất cả$n$giá trị nhiều nhất là$2 \cdot 10^5$, do đó việc quét tuyến tính qua từng trường hợp kiểm thử dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
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

        ok = True
        for i, x in enumerate(p, start=1):
            if abs(x - i) > 1:
                ok = False
                break

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided sample
assert run("""2
4
2 1 3 4
4
4 2 3 1
""") == """YES
NO"""

# minimum size
assert run("""1
1
1
""") == "YES"

# single adjacent swap
assert run("""1
2
2 1
""") == "YES"

# displacement greater than one
assert run("""1
3
3 1 2
""") == "NO"

# multiple disjoint adjacent inversions
assert run("""1
6
2 1 4 3 6 5
""") == "YES"

# reversed permutation
assert run("""1
5
5 4 3 2 1
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`|`YES`| Kích thước tối thiểu | 
|`[2,1]`|`YES`| Hoán đổi pháp lý duy nhất | 
|`[3,1,2]`|`NO`| Phần tử bị dịch chuyển bởi nhiều vị trí | 
|`[2,1,4,3,6,5]`|`YES`| Nhiều nghịch đảo liền kề độc lập | 
|`[5,4,3,2,1]`|`NO`| Chuyển vị lớn khiến việc phân loại không thể | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
2 1
```Các chuyển vị là$1$Và$1$. Thuật toán chấp nhận. Một trao đổi trao đổi hợp pháp duy nhất$2$Và$1$, tạo ra hoán vị được sắp xếp. 

Coi như:```
1
3
3 1 2
```Phần tử đầu tiên có độ dịch chuyển$2$. Thuật toán ngay lập tức từ chối. giá trị$3$sẽ cần phải di chuyển qua$1$và các giá trị này khác nhau bởi$2$, nên thứ tự tương đối của chúng không bao giờ thay đổi. 

Coi như:```
1
6
2 1 4 3 6 5
```Mọi chuyển vị đều bằng hoặc$0$hoặc$1$. Thuật toán chấp nhận. Mỗi cặp đảo ngược bao gồm các giá trị liên tiếp và có thể được cố định độc lập. 

Coi như:```
1
5
5 4 3 2 1
```Phần tử đầu tiên có độ dịch chuyển$4$. Thuật toán từ chối ngay lập tức. Việc sắp xếp sẽ yêu cầu thay đổi thứ tự tương đối của nhiều cặp giá trị không liên tiếp, điều mà không một thao tác hợp pháp nào có thể thực hiện được.
