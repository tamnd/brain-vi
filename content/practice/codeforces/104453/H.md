---
title: "CF 104453H - \u0421\u043a\u0430\u0437\u043e\u0447\u043d\u044b\u0435 \u0441\u043d\u044b"
description: "Chúng ta có một tập hợp các đường thẳng trong mặt phẳng, mỗi đường được mô tả bằng một phương trình có dạng $y = A x + B$. Không có hai đường thẳng nào giống hệt nhau nên mỗi cặp đều song song hoặc cắt nhau đúng một lần."
date: "2026-06-30T14:34:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104453
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Qualyfing Round, 2021"
rating: 0
weight: 104453
solve_time_s: 62
verified: false
draft: false
---

[CF 104453H - \u0421\u043a\u0430\u0437\u043e\u0447\u043d\u044b\u0435 \u0441\u043d\u044b](https://codeforces.com/problemset/problem/104453/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đường thẳng trong mặt phẳng, mỗi đường được mô tả bằng một phương trình có dạng$y = A x + B$. Không có hai đường thẳng nào giống hệt nhau nên mỗi cặp đều song song hoặc cắt nhau đúng một lần. 

Nhiệm vụ là chọn đường giao với ít đường khác nhất và xuất ra số giao điểm tối thiểu đó. 

Được định hình lại một cách cụ thể hơn, hãy coi mỗi đường thẳng như một đường thẳng vô tận không phân đoạn. Khi hai đường ray không song song thì chúng cắt nhau tại đúng một điểm. Đối với mỗi bản nhạc, chúng tôi đếm xem nó đi qua bao nhiêu bản nhạc khác và chúng tôi muốn số lượng bản nhạc đó nhỏ nhất trên tất cả các bản nhạc. 

Kích thước đầu vào tăng lên$N = 10^5$. Việc so sánh trực tiếp từng cặp trên tất cả các cặp sẽ yêu cầu khoảng$10^{10}$kiểm tra, vượt xa những gì khả thi trong thời gian vài giây. Điều này ngay lập tức loại trừ các phương pháp bậc hai. 

Một quan sát cấu trúc quan trọng là nút giao chỉ phụ thuộc vào độ dốc. Hai dòng$A_1 x + B_1$Và$A_2 x + B_2$cắt nhau khi và chỉ khi$A_1 \ne A_2$. Nếu hệ số góc bằng nhau thì các đường thẳng song song và không bao giờ cắt nhau. 

Vì vậy, đối với bất kỳ đường thẳng nào có độ dốc$A$, số giao điểm của nó chính xác là$N - \text{count of lines with slope } A$. 

Các trường hợp cạnh xuất hiện khi nhiều đường có cùng độ dốc. Ví dụ: nếu tất cả các dòng có cùng$A$, thì không có giao điểm nào xảy ra và câu trả lời là 0. Nếu các hệ số góc đều khác nhau thì mỗi đường thẳng sẽ cắt tất cả các đường khác và câu trả lời là$N - 1$. Một cách tiếp cận ngây thơ mà bỏ qua các bội số sẽ cho rằng mọi cặp đều giao nhau một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra từng cặp đường và tăng bộ đếm bất cứ khi nào độ dốc khác nhau. Điều này đúng vì nó mô phỏng trực tiếp định nghĩa về giao lộ. Tuy nhiên, nó thực hiện$N(N-1)/2$so sánh, sẽ trở thành khoảng năm tỷ hoạt động khi$N = 10^5$, điều đó không khả thi. 

Sự đơn giản hóa chính xuất phát từ việc tách hình học khỏi việc đếm. Giao lộ hoàn toàn không phụ thuộc vào điểm giao cắt, chỉ phụ thuộc vào việc độ dốc có phù hợp hay không. Thay vì kiểm tra các cặp, chúng ta chỉ cần biết mỗi độ dốc xuất hiện bao nhiêu lần. 

Sau khi nhóm các đường theo độ dốc, chúng ta có thể tính số lượng giao điểm cho mỗi nhóm trong thời gian không đổi: tất cả các đường bên ngoài nhóm giao nhau với mọi đường bên trong nhóm đó. Vì vậy, đối với giá trị độ dốc$A$, số giao lộ là$N - f(A)$, Ở đâu$f(A)$là tần số của nó 

Điều này biến vấn đề thành một nhiệm vụ đếm tần số trên các số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$|$O(1)$| Quá chậm | 
| Tối ưu (đếm tần số) |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Chiến lược tối ưu 

1. Đọc tất cả các dòng và trích xuất độ dốc của chúng$A$. Sự đánh chặn$B$không liên quan vì nó không ảnh hưởng đến việc hai đường thẳng có cắt nhau hay không. 
2. Đếm số lần mỗi độ dốc xuất hiện bằng bản đồ băm hoặc từ điển. Bước này nén hình học thành thông tin tần số. 
3. Với mỗi nhóm độ dốc, hãy tính số giao điểm của các đường trong nhóm đó như sau:$N - f(A)$. Điều này xuất phát từ thực tế là chỉ những đường có cùng độ dốc mới không giao nhau. 
4. Theo dõi mức tối thiểu của các giá trị này trên tất cả các nhóm độ dốc. 
5. Xuất ra giá trị tối thiểu này. 

### Tại sao nó hoạt động 

Mỗi đường chỉ không giao nhau với các đường có cùng độ dốc. Vì tất cả các độ dốc khác đảm bảo chính xác một giao lộ, nên mỗi đường trong một lớp độ dốc đều có số lượng giao lộ giống hệt nhau. Do đó, việc giảm thiểu trên các đường tương đương với việc giảm thiểu các nhóm độ dốc và biểu thức$N - f(A)$nắm bắt chính xác tất cả các tương tác không song song. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    freq = {}

    for _ in range(n):
        a, b = map(int, input().split())
        freq[a] = freq.get(a, 0) + 1

    ans = n
    for cnt in freq.values():
        ans = min(ans, n - cnt)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng dòng và ngay lập tức loại bỏ phần chặn vì nó không có vai trò gì trong các giao lộ. Từ điển`freq`lưu trữ bao nhiêu dòng chia sẻ mỗi độ dốc. 

Sau khi xử lý đầu vào, chúng tôi lặp lại các nhóm độ dốc. Đối với mỗi nhóm kích thước`cnt`, chúng tôi tính toán`n - cnt`, biểu thị có bao nhiêu đường có hệ số góc khác nhau và do đó giao nhau với nhóm. Mức tối thiểu trên tất cả các nhóm sẽ đưa ra câu trả lời mong muốn. 

Một điểm tinh tế là khởi tạo`ans`BẰNG`n`. Điều này là an toàn vì số giao điểm tối đa có thể có của bất kỳ đường nào là$n - 1$và việc khởi tạo cao hơn hoặc bằng không ảnh hưởng đến tính chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
1 3
2 3
```Chúng tôi tính toán tần số độ dốc: 

| Bước | Độ dốc đọc | Bản đồ tần số | 
| --- | --- | --- | 
| 1 | 1 | {1: 1} | 
| 2 | 1 | {1: 2} | 
| 3 | 2 | {1: 2, 2: 1} | 

Bây giờ hãy tính số lượng giao lộ: 

| Độ dốc | Tần số | Nút giao | 
| --- | --- | --- | 
| 1 | 2 | 3 - 2 = 1 | 
| 2 | 1 | 3 - 1 = 2 | 

Tối thiểu là 1. 

Điều này cho thấy rằng việc nhóm các sườn dốc giống nhau sẽ làm giảm số lượng giao điểm của chúng vì các đường song song không giao nhau. 

### Mẫu 2 

đầu vào:```
5
1 1
1 2
1 3
2 2
2 3
```Tần số độ dốc: 

| Bước | Độ dốc | Bản đồ tần số | 
| --- | --- | --- | 
| 1 | 1 | {1: 1} | 
| 2 | 1 | {1: 2} | 
| 3 | 1 | {1: 3} | 
| 4 | 2 | {1: 3, 2: 1} | 
| 5 | 2 | {1: 3, 2: 2} | 

Số lượng giao lộ: 

| Độ dốc | Tần số | Nút giao | 
| --- | --- | --- | 
| 1 | 3 | 5 - 3 = 2 | 
| 2 | 2 | 5 - 2 = 3 | 

Câu trả lời là 2. 

Điều này xác nhận rằng đường tối ưu là đường nằm trong nhóm độ dốc thường xuyên nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Một đường chuyền để đếm độ dốc và một đường chuyền vượt qua các sườn dốc độc đáo | 
| Không gian |$O(N)$| Trong trường hợp xấu nhất tất cả các độ dốc đều khác biệt | 

Thuật toán dễ dàng phù hợp với các ràng buộc cho$N \le 10^5$. Cả bộ nhớ và thời gian chạy đều tuyến tính, điều này là tối ưu vì mỗi dòng đầu vào phải được đọc ít nhất một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else capture(inp)

def capture(inp):
    old_in = sys.stdin
    old_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_in
    sys.stdout = old_out
    return out.strip()

# provided samples
assert capture("3\n1 2\n1 3\n2 3\n") == "1"
assert capture("5\n1 1\n1 2\n1 3\n2 2\n2 3\n") == "2"

# all equal slopes
assert capture("4\n1 0\n1 1\n1 2\n1 3\n") == "0"

# all distinct slopes
assert capture("4\n1 0\n2 0\n3 0\n4 0\n") == "3"

# mixed distribution
assert capture("6\n1 0\n1 1\n2 0\n2 1\n2 2\n3 0\n") == "3"

# minimum case
assert capture("1\n5 7\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các độ dốc bằng nhau | 0 | không có nút giao nào tồn tại | 
| tất cả các sườn dốc khác nhau | n-1 | trường hợp giao nhau tối đa | 
| phân phối hỗn hợp | tính toán tối thiểu | tính đúng đắn dưới sự mất cân bằng | 
| dòng đơn | 0 | điều kiện biên | 

## Vỏ cạnh 

Khi tất cả các hệ số góc giống hệt nhau, bản đồ tần số chứa một khóa duy nhất có giá trị$N$. Đối với đầu vào như:```
3
1 5
1 2
1 9
```thuật toán tính toán`n - cnt = 3 - 3 = 0`, phản ánh chính xác rằng không có giao lộ nào xảy ra. 

Khi tất cả các hệ số góc là khác nhau thì mỗi tần số là 1. Đối với:```
3
1 0
2 0
3 0
```mỗi nhóm mang lại`3 - 1 = 2`, vì vậy câu trả lời là 2. Điều này phù hợp với thực tế là mọi đường thẳng đều cắt nhau. 

Ví dụ: khi có nhóm độ dốc chiếm ưu thế:```
6
1 0
1 1
1 2
2 0
3 0
4 0
```nhóm lớn nhất có cỡ 3, cho`6 - 3 = 3`. Thuật toán xác định chính xác các đường trong nhóm độ dốc dày đặc nhất sẽ giảm thiểu các điểm giao nhau vì chúng tránh được các xung đột song song nhất.
