---
title: "CF 1554A - Anh đào"
description: "Chúng ta được cho một mảng các số nguyên dương. Đối với mỗi mảng con chứa ít nhất hai phần tử, chúng ta tính tích của phần tử lớn nhất và phần tử nhỏ nhất của nó. Nhiệm vụ là tìm tích lớn nhất như vậy trong số tất cả các mảng con có thể có."
date: "2026-06-10T12:52:55+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1554
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 735 (Div. 2)"
rating: 800
weight: 1554
solve_time_s: 102
verified: true
draft: false
---

[CF 1554A - Anh đào](https://codeforces.com/problemset/problem/1554/A) 

**Đánh giá:** 800 
**Thẻ:** tham lam 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Đối với mỗi mảng con chứa ít nhất hai phần tử, chúng ta tính tích của phần tử lớn nhất và phần tử nhỏ nhất của nó. Nhiệm vụ là tìm tích lớn nhất như vậy trong số tất cả các mảng con có thể có. 

Việc đọc trực tiếp định nghĩa gợi ý xem xét từng cặp điểm cuối và tính toán mức tối thiểu và tối đa trong phạm vi đó. Thách thức là một trường hợp thử nghiệm có thể chứa tới$10^5$phần tử và tổng số phần tử trong tất cả các trường hợp thử nghiệm đạt$3 \cdot 10^5$. Bất kỳ thuật toán nào kiểm tra tất cả$O(n^2)$mảng con ngay lập tức quá chậm, vì điều đó sẽ đòi hỏi khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Bản thân các giá trị này lớn bằng$10^6$. Vì câu trả lời là tích của hai giá trị mảng nên nó có thể đạt tới$10^{12}$, vì vậy số học số nguyên 64 bit là bắt buộc. Python tự động xử lý việc này. 

Một điểm tinh tế là mảng con tốt nhất không nhất thiết phải dài. Coi như:```
3
2 4 3
```Mảng con$[4,3]$cho$4 \cdot 3 = 12$, trong khi mảng con lớn hơn$[2,4,3]$chỉ cho$4 \cdot 2 = 8$. Việc mở rộng phạm vi có thể làm giảm mức tối thiểu và làm cho sản phẩm trở nên tồi tệ hơn. 

Một sai lầm dễ mắc phải khác là cho rằng câu trả lời phải liên quan đến phần tử cực đại toàn cục. Ví dụ:```
4
100 1 50 50
```Mảng con$[50,50]$cho$2500$, trong khi mọi mảng con chứa 100 cũng chứa 1, tạo ra nhiều nhất$100$. Chỉ tập trung vào giá trị lớn nhất sẽ bỏ lỡ câu trả lời đúng. 

Cạm bẫy thứ ba là tràn số nguyên trong các ngôn ngữ như C++ nếu sử dụng số nguyên 32 bit:```
2
1000000 1000000
```Câu trả lời đúng là$10^{12}$, không vừa với số nguyên có dấu 32 bit. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp điểm cuối$(l,r)$, tính giá trị tối thiểu và tối đa trong mảng con đó và cập nhật câu trả lời. Điều này đúng vì nó đánh giá rõ ràng mọi phạm vi ứng cử viên. 

Vấn đề là thời gian chạy. có$O(n^2)$các mảng con và tính toán lại giá trị tối thiểu và tối đa cho mỗi mảng con$O(n)$. Điều đó dẫn đến$O(n^3)$thời gian. Ngay cả khi chúng tôi duy trì mức tối thiểu và tối đa tăng dần và giảm độ phức tạp xuống$O(n^2)$, nó vẫn còn quá chậm đối với$n=10^5$. 

Quan sát quan trọng xuất phát từ việc hiểu điều gì sẽ xảy ra khi một mảng con chứa nhiều hơn hai phần tử. 

Giả sử một mảng con có giá trị nhỏ nhất$m$và giá trị tối đa$M$. Cả hai giá trị này đều xuất hiện ở đâu đó bên trong mảng con. Xét đoạn giữa các vị trí của$m$Và$M$. Đoạn đó cũng là một mảng con và giá trị tối thiểu của nó vẫn là$m$trong khi mức tối đa của nó vẫn là$M$. Sản phẩm vẫn còn$m \cdot M$. 

Bây giờ hãy nhìn vào hai vị trí nơi$m$Và$M$xảy ra. Di chuyển từ vị trí này sang vị trí khác phải có ít nhất một cặp phần tử liền kề. Trong số các cặp liền kề đó, có một cặp có ít nhất tích$m \cdot M$. Tại sao? Mọi giá trị trên đường dẫn đó nằm giữa$m$Và$M$, do đó, bất kỳ cặp liền kề nào cũng bao gồm các giá trị trong khoảng đó. Câu trả lời tối đa có thể đạt được luôn được một số cặp liền kề nhận ra. 

Một cách thậm chí còn đơn giản hơn để thấy điều đó là đối với một mảng con có giá trị tối thiểu là$m$và tối đa là$M$, câu trả lời được đóng góp bởi mảng con đó chính xác là$mM$. Cặp đôi$(m,M)$phải xuất hiện ở đâu đó trong mảng với chỉ các giá trị giữa chúng ngăn cách chúng. Kiểm tra các cặp liền kề là đủ và đây là quan sát dự định cho vấn đề này. 

Vì vậy, thay vì kiểm tra tất cả các mảng con, chúng ta chỉ cần kiểm tra từng cặp liền kề và tính toán:$$a_i \cdot a_{i+1}$$Tối đa các sản phẩm này là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng. 
3. Khởi tạo đáp án là 0. 
4. Lặp lại tất cả các cặp liền kề$(a_i, a_{i+1})$. 
5. Tính tích của họ và cập nhật câu trả lời nếu tích này lớn hơn. 
6. Sau khi xử lý tất cả các cặp liền kề, hãy in câu trả lời. 

Lý do điều này có hiệu quả là vì mọi câu trả lời ứng viên có thể đạt được từ một mảng con dài hơn cũng có thể đạt được bằng cách xem xét các giá trị tối thiểu và tối đa bên trong phạm vi đó và kết quả tối đa có thể cuối cùng sẽ xuất hiện giữa các phần tử liền kề. Vì mọi cặp liền kề đều được kiểm tra nên không thể bỏ qua giá trị tối ưu. 

### Tại sao nó hoạt động 

Cho một mảng con tối ưu có giá trị nhỏ nhất$m$và giá trị tối đa$M$. Giá trị được đóng góp bởi mảng con đó là$mM$. 

Tập trung vào những vị trí$m$Và$M$xảy ra. Mọi phần tử giữa các vị trí đó đều nằm giữa$m$Và$M$bao gồm. Dọc theo đường đi này có một cặp liền kề có tích ít nhất là$mM$. Vì mảng con tối ưu đã tạo ra$mM$, mức tối ưu toàn cục phải bằng tích cực đại của một số cặp liền kề. 

Thuật toán kiểm tra từng cặp liền kề chính xác một lần, do đó nó nhất thiết phải tìm ra tích tối đa đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    for i in range(n - 1):
        ans = max(ans, a[i] * a[i + 1])

    print(ans)
```Giải pháp tuân theo quan sát trực tiếp. 

Vòng lặp chạy từ chỉ mục 0 đến chỉ mục$n-2$, điều này đảm bảo rằng mọi cặp liền kề được xử lý chính xác một lần. Đối với mỗi cặp, sản phẩm của họ được tính toán và so sánh với câu trả lời tốt nhất hiện tại. 

Biến trả lời bắt đầu từ 0 vì tất cả các giá trị mảng đều dương, do đó mọi sản phẩm hợp lệ cũng dương. 

Số nguyên Python tự động hỗ trợ các giá trị lớn hơn$2^{31}-1$, vì vậy sản phẩm lên tới$10^{12}$được xử lý an toàn mà không cần bất kỳ sự chăm sóc đặc biệt nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 4 3
```| tôi | Cặp | Sản phẩm | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 0 | (2, 4) | 8 | 8 | 
| 1 | (4, 3) | 12 | 12 | 

Câu trả lời cuối cùng:```
12
```Cặp liền kề tốt nhất là$(4,3)$. Điều này phù hợp với mảng con tối ưu$[4,3]$. 

### Ví dụ 2 

đầu vào:```
4
3 2 3 1
```| tôi | Cặp | Sản phẩm | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 0 | (3, 2) | 6 | 6 | 
| 1 | (2, 3) | 6 | 6 | 
| 2 | (3, 1) | 3 | 6 | 

Câu trả lời cuối cùng:```
6
```Ví dụ này cho thấy một số mảng con khác nhau có thể đạt được mức tối ưu như nhau. Thuật toán chỉ cần giá trị tối đa chứ không cần phạm vi tạo ra nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua các cặp liền kề | 
| Không gian |$O(1)$| Chỉ có một vài biến được sử dụng | 

Tổng số phần tử được xử lý trên tất cả các trường hợp thử nghiệm nhiều nhất là$3 \cdot 10^5$. Quá trình quét tuyến tính cho mỗi trường hợp thử nghiệm có thể dễ dàng đủ nhanh trong giới hạn 1 giây và mức sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = 0
        for i in range(n - 1):
            ans = max(ans, a[i] * a[i + 1])

        out.append(str(ans))

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run(
"""4
3
2 4 3
4
3 2 3 1
2
69 69
6
719313 273225 402638 473783 804745 323328
"""
) == """12
6
4761
381274500335
"""

# minimum size
assert run(
"""1
2
5 7
"""
) == """35
"""

# all equal values
assert run(
"""1
5
4 4 4 4 4
"""
) == """16
"""

# off-by-one check, best pair at the end
assert run(
"""1
5
1 2 3 4 100
"""
) == """400
"""

# large values
assert run(
"""1
2
1000000 1000000
"""
) == """1000000000000
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 5 7`|`35`| Mảng nhỏ nhất có thể | 
|`4 4 4 4 4`|`16`| Tất cả các giá trị bằng nhau | 
|`1 2 3 4 100`|`400`| Cặp liền kề tốt nhất ở vị trí cuối cùng | 
|`1000000 1000000`|`1000000000000`| Sản phẩm lớn và an toàn số nguyên | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
1
2
5 7
```Chỉ có một cặp liền kề. Thuật toán tính toán$5 \cdot 7 = 35$và trả về 35. Vì mọi mảng con hợp lệ phải chứa cả hai phần tử nên điều này nhất thiết phải đúng. 

Hãy xem xét một mảng trong đó một mảng con dài hơn có vẻ hấp dẫn:```
1
3
2 4 3
```Thuật toán kiểm tra sản phẩm 8 và 12, trả về 12. Mảng con lớn hơn$[2,4,3]$có tối thiểu 2 và tối đa 4, chỉ cho 8. Quá trình quét xác định chính xác rằng việc mở rộng phạm vi có thể làm giảm sản phẩm. 

Xét các giá trị rất lớn:```
1
2
1000000 1000000
```Sản phẩm liền kề là:$$1000000 \times 1000000 = 1000000000000$$Thuật toán lưu trữ và in trực tiếp giá trị này. Số nguyên có độ chính xác tùy ý của Python tránh được sự cố tràn. 

Hãy xem xét trường hợp phần tử tối đa toàn cầu không phải là một phần của câu trả lời:```
1
4
100 1 50 50
```Các sản phẩm liền kề là: 

| Cặp | Sản phẩm | 
| --- | --- | 
| (100, 1) | 100 | 
| (1, 50) | 50 | 
| (50, 50) | 2500 | 

Thuật toán trả về 2500. Bất kỳ cách tiếp cận nào cố gắng ép giá trị tối đa toàn cầu 100 vào câu trả lời sẽ thất bại trong ví dụ này.
