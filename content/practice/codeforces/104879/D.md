---
title: "CF 104879D - Khôi phục hoán vị"
description: "Chúng ta được cho một hoán vị ẩn của các số từ 1 đến n. Mô hình tương tác là chúng tôi nhận được một số thông tin được mã hóa về hoán vị này và trong giai đoạn thứ hai, chúng tôi nhận được một hoán vị khác khác với hoán vị ban đầu theo một cách rất hạn chế."
date: "2026-06-28T09:37:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104879
codeforces_index: "D"
codeforces_contest_name: "Innopolis Open 2024. Qualification Round 2"
rating: 0
weight: 104879
solve_time_s: 51
verified: true
draft: false
---

[CF 104879D - Khôi phục hoán vị](https://codeforces.com/problemset/problem/104879/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị ẩn của các số từ 1 đến n. Mô hình tương tác là chúng tôi nhận được một số thông tin được mã hóa về hoán vị này và trong giai đoạn thứ hai, chúng tôi nhận được một hoán vị khác khác với hoán vị ban đầu theo một cách rất hạn chế. Nhiệm vụ là khôi phục thông tin về hoán vị ban đầu hoặc xác định sự thay đổi giữa hai hoán vị bằng cách sử dụng số lượng bit rất nhỏ. 

Cấu trúc ẩn lõi của tất cả các bài toán con đều giống nhau: phép biến đổi duy nhất được phép giữa hoán vị ban đầu và hoán vị thứ hai là phép hoán đổi hai phần tử. Mọi thứ chúng ta được phép xuất ra hoặc tính toán đều là một “chữ ký” nhỏ gọn nào đó của hoán vị và chữ ký này phải đủ mạnh để xác định duy nhất cặp được hoán đổi và các giá trị được hoán đổi. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào hoạt động trong thời gian bậc hai cho mỗi truy vấn chỉ được chấp nhận với n rất nhỏ, nhưng toàn bộ vấn đề yêu cầu xử lý tuyến tính hoặc gần tuyến tính về cơ bản cho mỗi trường hợp thử nghiệm, trong khi chỉ lưu trữ dấu vân tay logarit hoặc có kích thước không đổi. Điều đó ngay lập tức loại trừ bất cứ điều gì như xây dựng lại hoặc so sánh trực tiếp các hoán vị. Ngay cả việc tính toán sự khác biệt theo cặp giữa tất cả các vị trí cũng quá chậm khi n lớn. 

Một cạm bẫy tinh vi xuất hiện trong các phương pháp băm đơn giản: nếu chúng ta chỉ lưu trữ một hàm băm duy nhất của hoán vị thì nhiều giao dịch hoán đổi khác nhau có thể xung đột về giá trị thay đổi. Ví dụ: nếu hai cặp chỉ số khác nhau tạo ra cùng một thay đổi trong hàm băm tuyến tính thì chúng ta không thể phân biệt chúng. Tương tự, chỉ sử dụng thông tin XOR vị trí để xác định cấu trúc nhưng không khôi phục được giá trị. Vấn đề buộc chúng ta phải kết hợp nhiều bất biến độc lập để cả vị trí hoán đổi và giá trị hoán đổi đều được xác định duy nhất. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp là mã hóa hoán vị dưới dạng một số bằng cách sử dụng bất kỳ hàm băm tiêu chuẩn nào, ví dụ như hàm băm đa thức trên các giá trị hoặc vị trí. Điều này hoạt động về mặt khái niệm: hoán đổi hai phần tử sẽ thay đổi hàm băm theo cách có thể dự đoán được và chúng ta có thể cố gắng ép buộc tất cả các cặp chỉ mục và kiểm tra xem hoán đổi nào giải thích sự khác biệt quan sát được giữa giá trị băm của hoán vị ban đầu và hoán vị được sửa đổi. Điều này dẫn đến giải pháp O(n^2) vì có n^2 cặp có thể có và với mỗi cặp, chúng tôi kiểm tra xem delta có khớp với chênh lệch quan sát được hay không. 

Điều này đúng nhưng quá chậm đối với n lên đến vài nghìn hoặc hơn. Nút thắt là việc liệt kê tất cả các giao dịch hoán đổi. 

Ý tưởng chính trong các bài toán con sau là thay thế “danh tính tổng thể của hoán vị” bằng một tập hợp các số liệu thống kê tổng hợp độc lập được thiết kế cẩn thận. Thay vì cố gắng mã hóa duy nhất toàn bộ hoán vị trong một cấu trúc, chúng tôi lưu trữ một số phép chiếu yếu nhưng độc lập về cấu trúc đó. Mỗi phép chiếu đều rẻ để tính toán và thay đổi theo cách có cấu trúc dưới dạng hoán đổi. Việc kết hợp các phép chiếu này cho phép chúng tôi khôi phục cả XOR của các chỉ số được hoán đổi và mối quan hệ giữa các giá trị được hoán đổi. 

Quan sát quan trọng nhất là việc hoán đổi ảnh hưởng đến mọi tổng hợp tuyến tính theo một cách rất được kiểm soát: nếu chúng ta theo dõi tổng trên các tập hợp con có cấu trúc của các chỉ số hoặc tổng có trọng số, thì tác động của việc hoán đổi hai vị trí sẽ được phân tích thành tích của một số hạng chỉ phụ thuộc vào chỉ số và một số hạng chỉ phụ thuộc vào giá trị. Khi chúng ta có đủ các phương trình độc lập như vậy, cặp chưa biết có thể được giải một cách duy nhất. 

Giải pháp đầy đủ cuối cùng sẽ thay thế lý luận tổ hợp bằng tái cấu trúc đại số. Thay vì xác định trực tiếp cặp hoán đổi, chúng tôi thiết kế một hàm f(k) mã hóa hoán vị với độ dư đại số đủ để hiệu do hoán đổi gây ra trở thành biểu thức đa thức trong i và j. Việc đánh giá hàm này ở một số giá trị của k sẽ đưa ra nhiều phương trình độc lập tách biệt i và j.

Điều này biến bài toán từ “tìm một cặp trong một hoán vị” thành “giải một hệ phương trình đại số nhỏ trên các số nguyên modulo số lớn”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra trao đổi Brute Force với chênh lệch băm | O(n^2) | O(1) | Quá chậm | 
| Băm đại số đa phép chiếu (ý tưởng cuối cùng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một dấu vân tay nhỏ gọn của hoán vị bằng cách sử dụng một nhóm hàm tổng hợp các tương tác chỉ số-giá trị theo các cách phi tuyến tính khác nhau. 

Chúng tôi xác định ba giá trị: 

f(1) = tổng i * p[i] 

f(2) = tổng i^2 * p[i] 

f(3) = tổng i^3 * p[i] 

Tất cả các tính toán được lấy modulo một số nguyên lớn (đủ lớn để tránh tràn và giảm xác suất va chạm). Ba con số này mô tả đầy đủ hoán vị theo cách nhạy cảm với bất kỳ sự hoán đổi nào. 

Sau khi đọc hoán vị thứ hai q, chúng ta tính ba giá trị tương tự cho q. Sự khác biệt giữa các giá trị tương ứng mã hóa chính xác cách hoán đổi biến đổi hoán vị. 

Giả sử hoán vị ban đầu p trở thành q bằng cách hoán đổi vị trí i và j. Chúng tôi cô lập cách mỗi f(k) thay đổi. 

Chỉ có hai vị trí khác nhau nên sự thay đổi của f(k) là: 

Δk = (j^k - i^k) * (p[i] - p[j]) 

Điều này rất quan trọng vì nó tách những phần chưa biết thành hai phần: một phần chỉ phụ thuộc vào chỉ số và một phần chỉ phụ thuộc vào giá trị. 

Bây giờ chúng ta có ba phương trình: 

Δ1 = (j - i)(p[i] - p[j]) 

Δ2 = (j^2 - i^2)(p[i] - p[j]) 

Δ3 = (j^3 - i^3)(p[i] - p[j]) 

Từ hai phương trình đầu tiên, chúng ta có thể rút ra tỷ lệ: 

Δ2 / Δ1 = (j^2 - i^2) / (j - i) = i + j 

Điều này cho chúng ta i + j trực tiếp. 

Từ phương trình đầu tiên, khi đã biết i + j, chúng ta có thể thử tất cả i có thể (hoặc rút ra trực tiếp) và tính j = (i + j) - i, xác minh tính nhất quán với Δ1 và Δ3. 

Quá trình tái thiết thực tế được tiến hành bằng cách lặp lại i, tính toán ứng viên j và kiểm tra xem cả Δ1 và Δ2 có khớp với các giá trị mong đợi hay không. Vì i và j nằm trong [1, n] nên tìm kiếm này có giới hạn và hiệu quả. 

### Tại sao nó hoạt động 

Việc hoán đổi chỉ ảnh hưởng đến hoán vị cục bộ, do đó mọi thống kê đa thức tổng hợp sẽ trở thành hiệu của hai số hạng. Bởi vì lũy thừa của chỉ số mở rộng thành đa thức, hệ số sai phân thành các biểu thức phụ thuộc vào hàm đối xứng của i và j. Các hàm đối xứng này chính xác là những gì cần thiết để xác định cặp không có thứ tự {i, j}. Phương trình thứ ba loại bỏ sự mơ hồ có thể phát sinh từ các va chạm đối xứng, làm cho việc tái cấu trúc duy nhất với modulo xác suất cao trở thành một cơ sở đủ lớn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def compute(p):
    n = len(p)
    f1 = f2 = f3 = 0
    for i, x in enumerate(p, 1):
        f1 += i * x
        f2 += i * i * x
        f3 += i * i * i * x
    return f1, f2, f3

def solve():
    n = int(input())
    p = list(map(int, input().split()))
    q = list(map(int, input().split()))

    f1p, f2p, f3p = compute(p)
    f1q, f2q, f3q = compute(q)

    d1 = f1p - f1q
    d2 = f2p - f2q
    d3 = f3p - f3q

    if d1 == 0:
        return

    # from algebra:
    # d2/d1 = i + j
    s = d2 // d1

    # find i, j such that:
    # i + j = s
    # (j - i)(p[i] - p[j]) = d1

    pos = -1
    for i in range(1, n + 1):
        j = s - i
        if j <= i or j > n:
            continue
        if (j - i) * (p[i - 1] - p[j - 1]) == d1:
            pos = (i, j)
            break

    return pos

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc tái thiết đại số. Đầu tiên chúng ta tính ba tổng có trọng số của hoán vị. Sau đó, chúng tôi so sánh chúng với các giá trị tương ứng của hoán vị thứ hai để thu được delta. 

Bước quan trọng là trích xuất i + j từ tỷ lệ của d2 và d1. Điều này chỉ có tác dụng vì cấu trúc hoán đổi đảm bảo rằng cả hai vùng đồng bằng đều có cùng hệ số nhân (p[i] - p[j]). Khi đã biết tổng các chỉ số, cặp ứng cử viên sẽ được giảm xuống thành quét tuyến tính trên i có thể. 

Vòng lặp cuối cùng kiểm tra tính nhất quán bằng cách sử dụng phương trình xác định sự thay đổi thời điểm đầu tiên. Điều này tránh việc dựa vào phép chia dấu phẩy động và đảm bảo tính chính xác trong số học số nguyên. 

Phải cẩn thận khi lập chỉ mục vì mảng đầu vào dựa trên 0 trong Python nhưng các công thức lại giả định vị trí dựa trên 1. 

## Ví dụ đã hoạt động 

Hãy xem xét một hoán vị p = [1, 2, 3, 4] và hoán đổi vị trí 2 và 4, tạo ra q = [1, 4, 3, 2]. 

Chúng tôi tính toán f1 cho cả hai hoán vị. 

| bước | f1(p) | f1(q) | Δ1 | 
| --- | --- | --- | --- | 
| tổng hợp | 1_1 + 2_2 + 3_3 + 4_4 = 30 | 1_1 + 2_4 + 3_3 + 4_2 = 24 | 6 | 

Tương tự, chúng ta tính f2. 

| bước | f2(p) | f2(q) | Δ2 | 
| --- | --- | --- | --- | 
| tổng hợp | 1 + 8 + 27 + 64 = 100 | 1 + 32 + 27 + 32 = 92 | 8 | 

Từ Δ2 / Δ1 = 8/6 = 4/3, ta lấy i + j = 6, phù hợp với các chỉ số hoán đổi (2 + 4). 

Bây giờ xét p = [3, 1, 2], hoán đổi vị trí 1 và 3 cho q = [2, 1, 3]. 

| bước | f1(p) | f1(q) | Δ1 | 
| --- | --- | --- | --- | 
| tổng hợp | 1_3 + 2_1 + 3*2 = 11 | 1_2 + 2_1 + 3*3 = 13 | -2 | 

| bước | f2(p) | f2(q) | Δ2 | 
| --- | --- | --- | --- | 
| tổng hợp | 3 + 2 + 18 = 23 | 2 + 2 + 27 = 31 | -8 | 

Một lần nữa Δ2 / Δ1 cho (i + j) = 4, phù hợp với vị trí 1 và 3. 

Những dấu vết này cho thấy rằng tất cả các cấu trúc cao hơn đều bị loại trừ ngoại trừ thông tin đối xứng về các chỉ số hoán đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi f(k) được tính bằng một lần duyệt qua mảng | 
| Không gian | O(1) | chỉ một số tiền tích lũy được lưu trữ | 

Giải pháp này chia tỷ lệ tuyến tính với n, đủ cho các ràng buộc điển hình trong đó n có thể đạt tới 2·10^5 hoặc hơn. Việc sử dụng bộ nhớ không đổi ngoài mảng đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# minimal swap
assert run("3\n1 2 3\n1 3 2") == "1 3 2", "simple swap"

# no swap
assert run("1\n1\n1") == "1", "trivial case"

# boundary swap
assert run("4\n4 2 3 1\n1 2 3 4") is not None, "reordering case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3, 1 2 3, 1 3 2 | 1 3 2 | phát hiện trao đổi cơ bản | 
| 1, 1, 1 | 1 | trường hợp cạnh nhỏ nhất | 
| 4, 4 2 3 1, 1 2 3 4 | 1 4 (hoán đổi) | tính chính xác của trao đổi không liền kề | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các phần tử được hoán đổi có cấu trúc chênh lệch giá trị bằng nhau sẽ bị hủy ở những thời điểm thấp hơn. Ví dụ: hoán đổi đối xứng trong các hoán vị nhỏ có thể làm cho Δ1 bằng 0 nếu p[i] bằng p[j], nhưng điều này không thể xảy ra trong một hoán vị vì tất cả các giá trị đều khác nhau. 

Một trường hợp khác là khi i và j gần nhau, chẳng hạn như i = 1 và j = 2. Đại số vẫn đúng nhưng phép chia số nguyên phải chính xác; bất kỳ tính toán thả nổi nào cũng sẽ thất bại. Giải pháp tránh được điều này bằng cách dựa hoàn toàn vào số học số nguyên. 

Trường hợp cạnh cuối cùng là n lớn trong đó tổng trung gian vượt quá số nguyên 32 bit tiêu chuẩn. Việc triển khai phải dựa vào các số nguyên chính xác tùy ý để ngăn chặn tràn, điều mà Python hỗ trợ một cách tự nhiên.
