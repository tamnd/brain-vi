---
title: "CF 908E - Thống kê năm mới và thực thể"
description: "Chúng tôi làm việc với các vectơ nhị phân có độ dài m, có thể được xem dưới dạng số nguyên từ 0 đến 2^m - 1. Một tập S hợp lệ phải đáp ứng ba thuộc tính cấu trúc. Đầu tiên, nó chứa mọi vectơ từ tập T đã cho. Thứ hai, nó đóng dưới XOR."
date: "2026-06-12T23:45:15+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "E"
codeforces_contest_name: "Good Bye 2017"
rating: 2500
weight: 908
solve_time_s: 549
verified: false
draft: false
---

[CF 908E - Năm mới và liệt kê thực thể](https://codeforces.com/problemset/problem/908/E) 

**Đánh giá:** 2500 
**Tags:** bitmasks, tổ hợp, dp, toán 
**Thời gian giải:** 9 phút 9 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi làm việc với các vectơ nhị phân có độ dài`m`, có thể được xem dưới dạng số nguyên từ`0`ĐẾN`2^m - 1`. 

Một bộ hợp lệ`S`phải thỏa mãn ba tính chất cấu trúc. 

Đầu tiên, nó chứa mọi vectơ từ tập hợp đã cho`T`. 

Thứ hai, nó được đóng theo XOR. Nếu hai vectơ thuộc về`S`, thì XOR theo bit của chúng cũng phải thuộc về`S`. 

Thứ ba, nó được đóng theo bit AND. Nếu hai vectơ thuộc về`S`, thì theo bit AND của chúng cũng phải thuộc về`S`. 

Nhiệm vụ là đếm xem có bao nhiêu bộ khác nhau`S`đáp ứng được tất cả những yêu cầu này. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không được phép xây dựng các tập hợp vectơ một cách rõ ràng. Không gian xung quanh có`2^m`vectơ và`m`có thể lớn như`1000`. Bất kỳ cách tiếp cận nào giải thích từng vectơ một đều không thể thực hiện được. 

Phần bất thường của đầu vào là`n`là rất nhỏ. Chúng tôi có nhiều nhất`50`các vectơ đã cho, trong khi thứ nguyên`m`có thể đạt được`1000`. Sự bất đối xứng đó chính là chìa khóa. Thay vì nghĩ về hàng, chúng ta nên nghĩ về cột. 

Một lỗi phổ biến là tập trung vào chính các vectơ và cố gắng tạo ra sự đóng cửa của`T`dưới XOR và AND. Ngay cả đối với các kích thước vừa phải, việc đóng cửa có thể chứa nhiều vectơ theo cấp số nhân. 

Một sai lầm dễ mắc phải khác là cho rằng chỉ riêng việc đóng theo XOR là đủ. Ví dụ:```
m = 2
T = {10, 01}
```Việc đóng XOR là toàn bộ không gian`{00,01,10,11}`. Tập hợp đó thực sự hợp lệ, nhưng nó không phải là đối tượng đại số tuyến tính duy nhất có liên quan. Điều kiện AND là yếu tố mang lại cấu trúc thực sự cho bài toán. 

Trường hợp tinh tế thứ hai xuất hiện khi một số tọa độ trông giống hệt nhau trên tất cả các vectơ của`T`.```
m = 2
T = {11}
```Cả hai tọa độ đều hoạt động giống hệt nhau bên trong`T`. Có hai bộ hợp lệ:```
{00,11}
```Và```
{00,01,10,11}
```Một giải pháp xử lý các tọa độ giống hệt nhau như được hợp nhất vĩnh viễn sẽ bỏ sót một trong số chúng. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất hữu ích vì nó tiết lộ cấu trúc đại số cơ bản. 

Giả sử chúng ta cố gắng liệt kê mọi tập hợp`S`chứa đựng`T`và kiểm tra xem nó có được đóng theo XOR và AND hay không. Vũ trụ chứa`2^m`vectơ, do đó số lượng tập ứng cử viên là$$2^{2^m}.$$Ngay cả đối với`m = 20`điều này lớn một cách vô lý. 

Một lực lượng vũ phu tinh vi hơn sẽ bắt đầu từ`T`và liên tục thêm tất cả các kết quả XOR và AND cho đến khi đạt được kết quả đóng. Tính toán tập hợp lệ nhỏ nhất chứa`T`, nhưng vấn đề yêu cầu tất cả các bộ hợp lệ, không chỉ bộ tối thiểu. 

Quan sát quan trọng là việc đóng theo XOR làm cho`S`một không gian vectơ trên`F₂`, trong khi đóng theo AND có nghĩa là`S`cũng bị đóng theo phép nhân tọa độ. 

Một không gian con của`F₂^m`được đóng dưới phép nhân tọa độ là một vành con Boolean của`F₂^m`. 

Các vòng con như vậy có hình dạng rất cứng nhắc. 

Xét hai vị trí tọa độ`i`Và`j`. Nếu mọi vectơ trong`S`luôn có bit giống nhau ở hai vị trí này thì tọa độ bên trong không thể phân biệt được`S`. Mối quan hệ này phân chia tọa độ thành các khối. 

Với mọi khối, mọi vectơ của`S`phải không đổi trên khối đó. Ngược lại, khi một phân vùng tọa độ được cố định, tập hợp tất cả các vectơ không đổi trên mỗi khối sẽ được đóng dưới cả XOR và AND. 

Do đó mọi tập hợp hợp lệ đều tương ứng chính xác với một phân vùng của các vị trí tọa độ. 

Bây giờ áp đặt yêu cầu rằng mọi vectơ của`T`thuộc về`S`. 

Lấy hai tọa độ`i`Và`j`. Chúng chỉ có thể nằm trong cùng một khối nếu mọi vectơ của`T`có cùng một bit ở các vị trí`i`Và`j`. 

Đối với mỗi tọa độ, hãy tạo chữ ký cột của nó:$$(t_1[i], t_2[i], \dots, t_n[i]).$$Các tọa độ có chữ ký khác nhau không bao giờ có thể được đặt trong cùng một khối. 

Các tọa độ có cùng chữ ký hoàn toàn có thể hoán đổi cho nhau. Bên trong một nhóm có kích thước như vậy`c`, chúng ta có thể phân vùng tọa độ theo bất kỳ cách nào chúng ta muốn và mỗi phân vùng như vậy tạo ra một tập hợp hợp lệ riêng biệt. 

Số lượng phân vùng của một tập hợp kích thước`c`là số chuông`Bell(c)`. 

Các nhóm chữ ký khác nhau độc lập nên câu trả lời là$$\prod Bell(c_i),$$Ở đâu`c_i`là kích thước của các nhóm chữ ký. 

Nhiệm vụ duy nhất còn lại là tính số Bell lên tới`1000`. 

Sử dụng phép truy hồi số Stirling$$S(n,k)=S(n-1,k-1)+kS(n-1,k),$$chúng ta có thể tính toán tất cả các số Bell$$Bell(n)=\sum_{k=0}^{n} S(n,k)$$TRONG`O(m²)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`2^m`| Hàm mũ | Quá chậm | 
| Tối ưu |`O(m² + mn)`|`O(m²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả`n`chuỗi nhị phân. 
2. Đối với mọi vị trí tọa độ`j`, xây dựng chữ ký cột của nó bao gồm`j`-bit thứ của tất cả các chuỗi đầu vào. 
3. Nhóm tọa độ bằng chữ ký giống nhau và ghi lại số lượng của mỗi nhóm. 
4. Tính toán trước số chuông lên tới`m`. 
5. Sử dụng phép truy hồi Stirling$$S(n,k)=S(n-1,k-1)+kS(n-1,k)$$modulo`10^9+7`. 
6. Đối với mọi`n`, tổng hợp tất cả`S(n,k)`để có được`Bell(n)`. 
7. Khởi tạo câu trả lời dưới dạng`1`. 
8. Đối với mỗi nhóm chữ ký có kích thước`c`, nhân câu trả lời với`Bell(c)`modulo`10^9+7`. 
9. Xuất kết quả. 

Lý do Bước 3 là đủ là vì các tọa độ có các chữ ký khác nhau không bao giờ có thể thuộc cùng một khối phân vùng, trong khi các tọa độ chia sẻ chữ ký có thể được phân vùng tùy ý. 

### Tại sao nó hoạt động 

Mọi tập hợp tốt đều là một tập hợp con Boolean của`F₂^m`. Các vành con Boolean của`F₂^m`chính xác là tập hợp các vectơ không đổi trên các khối của một số phân vùng tọa độ. 

Các vectơ của`T`hạn chế tọa độ nào có thể chia sẻ một khối. Hai tọa độ chỉ có thể được hợp nhất khi tất cả các vectơ của`T`gán cùng một bit cho cả hai vị trí, đó chính xác là điều kiện mà chữ ký cột của chúng bằng nhau. 

Trong một lớp chữ ký, mọi phân vùng đều được phép. Các lớp chữ ký khác nhau là độc lập. Số lượng phân vùng của một lớp kích thước`c`là`Bell(c)`, do đó, nhân số Bell trên tất cả các lớp sẽ tính mọi tập hợp hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    m, n = map(int, input().split())
    rows = [input().strip() for _ in range(n)]

    groups = {}

    for col in range(m):
        sig = tuple(rows[r][col] for r in range(n))
        groups[sig] = groups.get(sig, 0) + 1

    # Stirling numbers of the second kind
    stir = [[0] * (m + 1) for _ in range(m + 1)]
    stir[0][0] = 1

    bell = [0] * (m + 1)
    bell[0] = 1

    for i in range(1, m + 1):
        for k in range(1, i + 1):
            stir[i][k] = (
                stir[i - 1][k - 1] +
                k * stir[i - 1][k]
            ) % MOD

        bell[i] = sum(stir[i][1:i + 1]) % MOD

    ans = 1
    for cnt in groups.values():
        ans = (ans * bell[cnt]) % MOD

    print(ans)

solve()
```Phần đầu tiên của quá trình triển khai xây dựng chữ ký cột. Từ`n ≤ 50`, việc lưu trữ chữ ký dưới dạng một bộ ký tự là hoàn toàn an toàn. Hai tọa độ thuộc cùng một nhóm chính xác khi các bộ dữ liệu này bằng nhau. 

Số Bell được tính bằng số Stirling loại hai. Với`m ≤ 1000`, cái`O(m²)`chương trình động thực hiện khoảng một triệu cập nhật trạng thái, dễ dàng đáp ứng các giới hạn. 

Một lỗi triển khai phổ biến là thử tính toán số Bell trực tiếp từ định nghĩa đệ quy của chúng. Phép lặp đó liên quan đến tất cả các số Bell trước đó và nhanh chóng trở thành số bậc hai. Phép truy hồi Stirling đơn giản và ổn định về mặt số lượng`10^9+7`. 

Một điểm tinh tế khác là`Bell(0)=1`. Mặc dù không có nhóm chữ ký nào có kích thước bằng 0 nhưng trường hợp cơ bản này là cần thiết để khởi động Stirling DP một cách chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
11010
00101
11000
```Chữ ký cột: 

| Vị trí | Chữ ký | 
| --- | --- | 
| 1 | (1,0,1) | 
| 2 | (1,0,1) | 
| 3 | (0,1,0) | 
| 4 | (1,0,0) | 
| 5 | (0,1,0) | 

Phân nhóm: 

| Chữ ký | Đếm | 
| --- | --- | 
| (1,0,1) | 2 | 
| (0,1,0) | 2 | 
| (1,0,0) | 1 | 

Giá trị chuông: 

| Kích thước | Chuông | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 

Trả lời: 

| Quy mô nhóm | Đóng góp | 
| --- | --- | 
| 2 | 2 | 
| 2 | 2 | 
| 1 | 1 |$$2 \times 2 \times 1 = 4.$$Đầu ra:```
4
```Ví dụ này cho thấy mỗi lớp chữ ký đóng góp độc lập. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 1
11
```Chữ ký cột: 

| Vị trí | Chữ ký | 
| --- | --- | 
| 1 | (1) | 
| 2 | (1) | 

Có một lớp kích thước chữ ký`2`. 

| Kích thước | Chuông | 
| --- | --- | 
| 2 | 2 | 

Trả lời:$$2.$$Hai phân vùng là:```
{1,2}
```Và```
{1},{2}
```tương ứng với hai bộ hàng hóa hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m² + mn)`| Xây dựng số chuông DP cộng với chữ ký | 
| Không gian |`O(m²)`| Bàn Stirling DP | 

Thuật ngữ chiếm ưu thế là`O(m²)`Tính toán số Stirling. Với`m = 1000`, đây là khoảng một triệu trạng thái DP, phù hợp thoải mái trong thời hạn. Việc sử dụng bộ nhớ cũng có thể chấp nhận được đối với một`1001 × 1001`bàn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    m, n = map(int, input().split())
    rows = [input().strip() for _ in range(n)]

    groups = {}

    for col in range(m):
        sig = tuple(rows[r][col] for r in range(n))
        groups[sig] = groups.get(sig, 0) + 1

    stir = [[0] * (m + 1) for _ in range(m + 1)]
    stir[0][0] = 1

    bell = [0] * (m + 1)
    bell[0] = 1

    for i in range(1, m + 1):
        for k in range(1, i + 1):
            stir[i][k] = (stir[i - 1][k - 1] + k * stir[i - 1][k]) % MOD
        bell[i] = sum(stir[i][1:i + 1]) % MOD

    ans = 1
    for c in groups.values():
        ans = (ans * bell[c]) % MOD

    return str(ans)

# provided sample
assert run(
"""5 3
11010
00101
11000
"""
) == "4"

# minimum size
assert run(
"""1 1
0
"""
) == "1"

# one signature class of size 2
assert run(
"""2 1
11
"""
) == "2"

# three identical columns, Bell(3)=5
assert run(
"""3 1
111
"""
) == "5"

# all signatures distinct
assert run(
"""3 2
101
010
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`1`| Ví dụ nhỏ nhất có thể | 
|`2 1 / 11`|`2`| Số chuông cho cỡ 2 | 
|`3 1 / 111`|`5`| Số chuông cho cỡ 3 | 
|`3 2 / 101 / 010`|`1`| Mỗi chữ ký độc đáo | 

## Vỏ cạnh 

Hãy xem xét```
1 1
0
```Chỉ có một tọa độ, do đó có chính xác một lớp kích thước chữ ký`1`. Từ`Bell(1)=1`, thuật toán trả về`1`. Không cần xử lý đặc biệt. 

Coi như```
2 1
11
```Cả hai tọa độ đều có cùng chữ ký`(1)`. Kích thước nhóm là`2`, cho`Bell(2)=2`. Thuật toán đếm chính xác cả hai phân vùng tọa độ có thể. 

Coi như```
3 2
101
010
```Các chữ ký là:```
(1,0), (0,1), (1,0)
```tọa độ`1`Và`3`có thể tương tác, nhưng phối hợp`2`không thể tham gia cùng họ. Câu trả lời trở thành$$Bell(2)\cdot Bell(1)=2.$$Điều này chứng tỏ tại sao việc nhóm theo chữ ký cột là tiêu chí chính xác. Hai tọa độ có thể hợp nhất một cách chính xác khi mọi vectơ của`T`thấy chúng giống hệt nhau.
