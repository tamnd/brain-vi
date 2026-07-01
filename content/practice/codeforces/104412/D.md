---
title: "CF 104412D - Mảng con Draconis"
description: "Chúng ta được cho hai mảng số nguyên. Mảng đầu tiên, gọi là mảng mẫu, có độ dài $M$. Mảng thứ hai, gọi là mảng nguồn, có độ dài $N$."
date: "2026-06-30T22:50:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "D"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 59
verified: true
draft: false
---

[CF 104412D - Mảng con Draconis](https://codeforces.com/problemset/problem/104412/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên. Mảng đầu tiên, gọi là mảng mẫu, có độ dài$M$. Mảng thứ hai, gọi là mảng nguồn, có độ dài$N$. Chúng tôi muốn đếm xem có bao nhiêu phân đoạn liền kề của mảng nguồn có thể được chuyển đổi thành mảng mẫu bằng cách thêm cùng một giá trị không đổi cho mọi phần tử của phân đoạn đó. 

Nói cách khác, một phân đoạn của$N$trận đấu$M$nếu hình dạng của các giá trị giống hệt nhau, có thể dịch chuyển theo chiều dọc. Các giá trị tuyệt đối không quan trọng, chỉ là giá trị thay đổi như thế nào từ vị trí này sang vị trí tiếp theo. 

Hạn chế chính là cả hai mảng có thể rất lớn, lên tới$10^6$các phần tử. Điều này loại trừ mọi so sánh bậc hai giữa mọi mảng con của$N$Và$M$, vì điều đó sẽ yêu cầu kiểm tra đại khái$N \cdot M$các yếu tố trong trường hợp xấu nhất, vượt xa$10^6$hoạt động. 

Một cách tiếp cận ngây thơ sẽ so sánh mọi độ dài-$M$mảng con của$N$trực tiếp chống lại$M$, nhưng ngay cả một chi phí so sánh duy nhất$O(M)$, dẫn đến$O(NM)$nhìn chung, điều đó tùy thuộc vào$10^{12}$hoạt động trong trường hợp xấu nhất. 

Một trường hợp cạnh tinh tế phát sinh khi tất cả các phần tử trong$M$giống hệt nhau. Trong tình huống đó, mọi kết quả khớp hợp lệ đều yêu cầu mảng con được chọn của$N$cũng bao gồm các giá trị giống hệt nhau, nhưng các phiên bản đã dịch chuyển vẫn có thể khớp. Nếu người ta so sánh không chính xác các giá trị thô thay vì những khác biệt tương đối thì các trường hợp có hình dạng giống hệt nhau sẽ bị bỏ qua. Ví dụ,$M = [1,1,1]$và một mảng con$[5,5,5]$sẽ khớp, nhưng so sánh trực tiếp sẽ thất bại. 

Một trường hợp góc khác là khi$M$có chiều dài$1$. Mỗi mảng con phần tử của$N$hợp lệ vì bất kỳ giá trị nào cũng có thể được dịch chuyển để khớp với một số. Câu trả lời đơn giản là$N$, điều này rất dễ bị bỏ qua nếu phương pháp luôn giả định tồn tại sự khác biệt. 

## Phương pháp tiếp cận 

Quan sát trọng tâm là “bằng với độ dịch chuyển không đổi” sẽ loại bỏ các giá trị tuyệt đối và chỉ để lại cấu trúc tương đối. Nếu chúng ta trừ đi các phần tử liên tiếp thì sự dịch chuyển hằng số sẽ biến mất hoàn toàn. 

Đối với bất kỳ mảng nào$A$, xác định biểu diễn sự khác biệt của nó:$$D_A[i] = A[i+1] - A[i]$$Hai mảng tương đương dịch chuyển khi và chỉ khi các mảng khác biệt của chúng giống hệt nhau. Điều này biến vấn đề thành một nhiệm vụ khớp mẫu: chúng ta không còn khớp các giá trị nữa mà khớp một chuỗi các khác biệt. 

Giải pháp brute-force sẽ lặp lại trên mọi mảng con của$N$chiều dài$M$, tính toán chuỗi sai phân của nó và so sánh nó với chuỗi sai phân của$M$. Mỗi chi phí so sánh$O(M)$, và có$O(N)$các ứng cử viên, vì vậy tổng độ phức tạp là$O(NM)$. Điều này quá chậm khi cả hai mảng đều đạt$10^6$. 

Khi vấn đề được giảm xuống còn việc khớp hai chuỗi, nó sẽ trở thành vấn đề tìm kiếm chuỗi con tiêu chuẩn: chúng ta cần đếm số lần xuất hiện của một mảng bên trong một mảng khác. Điều này có thể được giải quyết trong thời gian tuyến tính bằng cách sử dụng hàm băm hoặc hàm tiền tố (KMP). Cấu trúc rất quan trọng: chúng ta đang khớp một mẫu cố định bên trong một văn bản lớn và bảng chữ cái là các số nguyên tùy ý, vì vậy việc băm là một sự phù hợp tự nhiên. 

Sau khi chuyển đổi cả hai mảng thành dạng khác biệt của chúng, chúng tôi tìm kiếm sự xuất hiện của mảng khác biệt mẫu bên trong mảng khác biệt văn bản. Mỗi kết quả khớp tương ứng với một mảng con hợp lệ trong mảng ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NM)$|$O(1)$thêm | Quá chậm | 
| Sự khác biệt + KMP/Hash |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu$M = 1$, trở lại$N$. Bất kỳ mảng con phần tử đơn lẻ nào cũng có thể được dịch chuyển để khớp với một giá trị duy nhất, vì vậy mọi vị trí đều hợp lệ. 
2. Xây dựng mảng sai phân cho$M$, trong đó mỗi mục là$m[i+1] - m[i]$. Điều này mã hóa cấu trúc của nó một cách độc lập với các giá trị tuyệt đối. Bước này làm giảm kích thước mẫu xuống$M-1$. 
3. Xây dựng mảng sai phân cho$N$, tính toán tương tự$n[i+1] - n[i]$. Điều này chuyển vấn đề thành việc tìm kiếm sự xuất hiện của một chuỗi bên trong một chuỗi khác. 
4. Nếu$M > N$, trở lại$0$. Một mẫu dài hơn không thể khớp với một chuỗi ngắn hơn. 
5. Sử dụng thuật toán khớp mẫu theo thời gian tuyến tính, chẳng hạn như KMP để đếm số lần mảng khác biệt mẫu xuất hiện bên trong mảng khác biệt văn bản. Mỗi kết quả khớp tương ứng với một chỉ mục bắt đầu hợp lệ trong mảng ban đầu. 
6. Trả về số kết quả tìm được. 

### Tại sao nó hoạt động 

Một sự dịch chuyển liên tục ảnh hưởng đến tất cả các phần tử như nhau, do đó nó sẽ triệt tiêu khi lấy hiệu liền kề. Điều này có nghĩa là bất kỳ hai mảng nào chỉ khác nhau một hằng số đều tạo ra các mảng khác biệt giống hệt nhau. Ngược lại, nếu hai mảng có các mảng khác biệt giống hệt nhau, thì giá trị của chúng phải khác nhau một độ lệch không đổi, bởi vì việc xây dựng lại từ một giá trị bắt đầu cố định sẽ mang lại cấu trúc tương đối giống nhau. Do đó, việc so khớp các mảng khác biệt hoàn toàn tương đương với điều kiện tương tự ban đầu. Mọi kết quả khớp của mảng con hợp lệ trong không gian sai phân tương ứng một-một với một kết quả khớp dịch chuyển hợp lệ trong mảng ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_diff(arr):
    return [arr[i+1] - arr[i] for i in range(len(arr) - 1)]

def kmp_prefix(p):
    pi = [0] * len(p)
    j = 0
    for i in range(1, len(p)):
        while j > 0 and p[i] != p[j]:
            j = pi[j - 1]
        if p[i] == p[j]:
            j += 1
            pi[i] = j
    return pi

def kmp_count(text, pat):
    if not pat:
        return len(text) + 1

    pi = kmp_prefix(pat)
    j = 0
    count = 0

    for i in range(len(text)):
        while j > 0 and text[i] != pat[j]:
            j = pi[j - 1]
        if text[i] == pat[j]:
            j += 1
        if j == len(pat):
            count += 1
            j = pi[j - 1]

    return count

def solve():
    M, N = map(int, input().split())
    m = list(map(int, input().split()))
    n = list(map(int, input().split()))

    if M == 1:
        print(N)
        return

    if M > N:
        print(0)
        return

    dm = build_diff(m)
    dn = build_diff(n)

    print(kmp_count(dn, dm))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xử lý các trường hợp suy biến trong đó độ dài mẫu bằng 1 hoặc lớn hơn văn bản. Bước chuyển đổi tính toán các mảng sai phân, thu gọn bài toán so khớp thành một mảng theo chiều dài$M-1$Và$N-1$. 

Quy trình KMP được sử dụng để đếm số lần xuất hiện mẫu một cách hiệu quả trong thời gian tuyến tính. Mảng tiền tố mã hóa khoảng cách chúng ta có thể lùi lại một cách an toàn khi xảy ra sự không khớp, ngăn chặn việc kiểm tra lại cấu trúc đã được xác minh. Vòng lặp khớp sẽ tăng bộ đếm bất cứ khi nào mẫu đầy đủ được khớp, sau đó tiếp tục từ đường viền hợp lệ dài nhất. 

Một điểm tinh tế là chúng tôi hoàn toàn không so sánh các mảng thô sau khi xử lý trước. Tất cả tính chính xác phụ thuộc hoàn toàn vào thực tế là các mảng khác biệt bảo toàn tính tương đương khi dịch. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
M = 4, N = 6
M = [1,2,3,4]
N = [10,11,12,13,14,15]
```Mảng khác biệt:```
dm = [1,1,1]
dn = [1,1,1,1,1]
```| tôi | cửa sổ dn | trạng thái khớp j | hành động | 
| --- | --- | --- | --- | 
| 0 | [1,1,1] | 0 → 1 → 2 → 3 | trận đấu đầy đủ | 
| 1 | [1,1,1] | đặt lại | trận đấu | 
| 2 | [1,1,1] | đặt lại | trận đấu | 

Chúng tôi đếm 3 trận đấu bắt đầu từ các chỉ số 0, 1 và 2. 

Điều này xác nhận rằng mọi khối liên tiếp có độ dài 4 theo trình tự bước không đổi đều khớp với mẫu. 

### Ví dụ 2 

đầu vào:```
M = 3, N = 10
M = [1,1,1]
N = [2,2,2,3,3,3,4,4,4,4]
```Mảng khác biệt:```
dm = [0,0]
dn = [0,0,0,0,0,0,0,0,0]
```| tôi | cửa sổ dn | j | hành động | 
| --- | --- | --- | --- | 
| 0 | [0,0] | 0 → 1 → 2 | trận đấu | 
| 1 | [0,0] | đặt lại | trận đấu | 
| 2 | [0,0] | đặt lại | trận đấu | 
| 3 | [0,0] | đặt lại | trận đấu | 
| 4 | [0,0] | đặt lại | trận đấu | 

Chúng tôi thu được 4 kết quả phù hợp, tương ứng với mỗi đoạn không đổi có độ dài 3. 

Điều này cho thấy thuật toán xử lý chính xác trường hợp hoàn toàn bằng nhau trong đó chênh lệch giảm về số 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + M)$| xây dựng mảng khác biệt và quét KMP đều tuyến tính | 
| Không gian |$O(N + M)$| lưu trữ mảng khác biệt và bảng tiền tố | 

Các ràng buộc cho phép lên đến$10^6$các phần tử, do đó, giải pháp thời gian tuyến tính phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""4 6
1 2 3 4
10 11 12 13 14 15
""") == "3"

# sample 2
assert run("""3 10
1 1 1
2 2 2 3 3 3 4 4 4 4
""") == "4"

# sample 3
assert run("""2 6
5 8
10 12 1 4 3 9
""") == "1"

# minimum size M = 1
assert run("""1 5
7
1 2 3 4 5
""") == "5"

# all equal large block
assert run("""4 6
5 5 5 5
1 1 1 1 1 1
""") == "3"

# no match
assert run("""3 5
1 2 3
10 20 30 40 50
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| M=1 trường hợp | N | phần tử đơn luôn khớp | 
| tất cả các mảng bằng nhau | 3 | khớp mẫu không khác biệt | 
| không khớp | 0 | tính đúng đắn khi không khớp | 

## Vỏ cạnh 

Đối với trường hợp$M = 1$, thuật toán trả về trực tiếp$N$. Điều này phù hợp với thực tế là mảng khác biệt trở nên trống và mọi vị trí đều khớp với một mẫu trống. Ví dụ, đầu vào$M=[7]$,$N=[1,2,3]$tạo ra 3 kết quả khớp, vì mỗi mảng con phần tử đơn lẻ có thể được chuyển thành 7. 

Đối với các mảng thống nhất như$M=[5,5,5]$Và$N=[1,1,1,1]$, các mảng chênh lệch hoàn toàn bằng 0. Quá trình KMP khớp mẫu số 0 nhiều lần trên văn bản số 0, tạo ra$N-M+1$các trận đấu hợp lệ. Điều này xác nhận rằng thuật toán xử lý chính xác các trường hợp thông tin giá trị biến mất hoàn toàn sau khi lấy sai phân. 

Đối với các cấu trúc không khớp như$M=[1,2,3]$Và$N=[10,20,30]$, các mảng khác nhau khác nhau như$[1,1]$so với$[10,10]$, do đó không có kết quả khớp chuỗi con nào xảy ra, tạo ra số 0 như mong đợi.
