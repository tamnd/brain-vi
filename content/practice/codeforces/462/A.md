---
title: "CF 462A - Appleman và nhiệm vụ dễ dàng"
description: "Chúng ta có một bảng n×n, trong đó mỗi ô chứa một chữ 'x' hoặc một chữ 'o'. Nhiệm vụ là kiểm tra thuộc tính cục bộ cho mỗi ô: liệu số ô liền kề trực giao chứa 'o' có phải là số chẵn hay không."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 462
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 263 (Div. 2)"
rating: 1000
weight: 462
solve_time_s: 62
verified: true
draft: false
---

[CF 462A - Appleman và nhiệm vụ dễ dàng](https://codeforces.com/problemset/problem/462/A) 

**Đánh giá:** 1000 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng _n_×_n_, trong đó mỗi ô chứa 'x' hoặc 'o'. Nhiệm vụ là kiểm tra thuộc tính cục bộ cho mỗi ô: liệu số ô liền kề trực giao chứa 'o' có phải là số chẵn hay không. Hai ô liền kề nhau nếu chúng có chung một cạnh, vì vậy mỗi ô bên trong có tối đa bốn ô lân cận, các ô cạnh có ba hoặc hai ô và các ô góc có hai ô. Đầu ra là "CÓ" nếu tất cả các ô đáp ứng thuộc tính này và "KHÔNG" nếu ngược lại. 

Ràng buộc kích thước đầu vào nhỏ, _n_   100. Điều này rất quan trọng vì nó ngụ ý rằng chúng ta có đủ khả năng để kiểm tra trực tiếp mọi ô và các ô lân cận mà không phải lo lắng về hiệu suất. Ngay cả trong trường hợp xấu nhất là bảng 100×100, chúng tôi sẽ thực hiện tối đa 100×100×4 = 40.000 thao tác, không đáng kể trong giới hạn thời gian 1 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các bảng rất nhỏ (n=1) trong đó không có hàng xóm và cấu hình trong đó chỉ các ô ranh giới có 'o'. Ví dụ: bảng 1×1 có một chữ 'o' duy nhất sẽ xuất ra "CÓ" vì nó không có hàng xóm nào, tức là số chẵn. Một trường hợp cạnh khác là khi tất cả các ô đều chứa 'o'; mỗi ô bên trong có bốn ô lân cận, chẵn, nhưng các ô ở góc chỉ có hai ô lân cận, vẫn chẵn nên bảng hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua từng ô và đếm số chữ 'o' trong số các ô lân cận của nó. Nếu bất kỳ ô nào có số lẻ, chúng tôi trả về ngay "KHÔNG". Nếu không, sau khi kiểm tra tất cả các ô, chúng tôi trả về "CÓ". Điều này hiệu quả vì bo mạch đủ nhỏ để cho phép hàng xóm kiểm tra trực tiếp. 

Chúng ta có thể chính thức hóa brute-force: đối với ô (i,j), kiểm tra (i-1,j), (i+1,j), (i,j-1), (i,j+1) nếu chúng tồn tại. Đếm những người hàng xóm cho mỗi ô là O(1) và có n2 ô, nên tổng thời gian là O(n2). Điều này có thể chấp nhận được ở đây. Không cần có cách tiếp cận tiệm cận nhanh hơn; những hạn chế của vấn đề làm cho lực lượng vũ phu vừa chính xác vừa hiệu quả. Thông tin chi tiết quan trọng là nhận ra rằng tính kề cận hoàn toàn mang tính cục bộ, vì vậy chúng ta không bao giờ cần thực hiện bất kỳ thao tác toàn cục nào như truyền tải đồ thị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n rồi đọc n dòng n ký tự vào danh sách 2D biểu diễn bảng. Điều này cho phép truy cập trực tiếp vào bất kỳ ô nào theo chỉ số hàng và cột. 
2. Lặp lại qua từng ô trong bảng bằng cách sử dụng hai vòng lặp lồng nhau cho hàng và cột. 
3. Đối với mỗi ô ở vị trí (i,j), khởi tạo bộ đếm về 0. Bộ đếm này sẽ theo dõi xem có bao nhiêu trong số bốn hàng xóm có thể chứa 'o'. 
4. Kiểm tra từng hàng xóm: phía trên (i-1,j), phía dưới (i+1,j), bên trái (i,j-1) và bên phải (i,j+1). Chỉ kiểm tra hàng xóm nếu các chỉ số nằm trong giới hạn của bảng. Nếu hàng xóm chứa 'o', hãy tăng bộ đếm. 
5. Sau khi đếm các ô lân cận cho ô hiện tại, hãy kiểm tra xem bộ đếm có lẻ không. Nếu lẻ thì in ra "NO" và kết thúc. Điều này có tác dụng vì một vi phạm duy nhất cũng đủ để vô hiệu hóa toàn bộ bảng. 
6. Nếu vòng lặp hoàn tất mà không tìm thấy ô nào có số lẻ, hãy in "CÓ". Mọi ô đều thỏa mãn điều kiện kề nhau. 

Tại sao nó hoạt động: Thuật toán kiểm tra từng ô chính xác một lần và đánh giá thuộc tính quan tâm chỉ bằng cách sử dụng các ô lân cận của nó. Vì vấn đề là cục bộ và tất cả thông tin liên quan đều nằm ở các vị trí liền kề của ô nên không có tương tác ẩn nào. Bất kỳ ô nào có số lượng ô lân cận 'o' lẻ sẽ được phát hiện ngay lập tức, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
board = [input().strip() for _ in range(n)]

for i in range(n):
    for j in range(n):
        count = 0
        if i > 0 and board[i-1][j] == 'o':
            count += 1
        if i < n-1 and board[i+1][j] == 'o':
            count += 1
        if j > 0 and board[i][j-1] == 'o':
            count += 1
        if j < n-1 and board[i][j+1] == 'o':
            count += 1
        if count % 2 != 0:
            print("NO")
            sys.exit()
print("YES")
```Giải pháp đọc bảng hiệu quả với`sys.stdin.readline`để tránh chi phí từ`input()`. Nó kiểm tra rõ ràng các giới hạn trước khi truy cập hàng xóm để ngăn ngừa lỗi chỉ mục. sử dụng`sys.exit()`dừng tính toán ngay khi phát hiện thấy vi phạm, điều này hiệu quả hơn một chút so với việc kiểm tra cờ sau vòng lặp. Việc đếm trực tiếp các hàng xóm sẽ tránh được các cấu trúc dữ liệu bổ sung, giữ độ phức tạp về không gian ở mức tối thiểu. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3
xxo
xox
oxx
```| tôi | j | Hàng xóm với 'o' | đếm % 2 | 
| --- | --- | --- | --- | 
| 0 | 0 | (0,1),(1,0) không có, (1,0) 'x' | 0 | 
| 0 | 1 | (0,0)x,(0,2)o,(1,1)x | 1 -> có? kiểm tra | 
| Trên thực tế: | | | | 
| (0,1): lên n/a, xuống (1,1)x, trái (0,0)x, phải (0,2)o → 1 hàng xóm 'o' → lẻ → kích hoạt NO? | | | | 

Đợi đầu ra mẫu là CÓ, vì vậy hãy kiểm tra lại. 

Ban: 

hàng0: x x o 

hàng1: x o x 

hàng2: o x x 

Tính toán hàng xóm: 

(0,0): lên n/a, xuống (1,0)x, trái n/a, phải (0,1)x → 0 → chẵn → OK 

(0,1): lên n/a, xuống (1,1)o, trái (0,0)x, phải (0,2)o → 2 → chẵn → OK 

(0,2): lên n/a, xuống (1,2)x, trái (0,1)x, phải n/a → 0 → chẵn → OK 

(1,0): lên (0,0)x, xuống (2,0)o, trái n/a, phải (1,1)o → 2 → chẵn → OK 

(1,1): lên (0,1)x, xuống (2,1)x, trái (1,0)x, phải (1,2)x → 0 → chẵn → OK 

(1,2): lên (0,2)o, xuống (2,2)x, trái (1,1)x, phải n/a → 1 → lẻ? Đợi mẫu đầu ra là CÓ nên phải tính toán lại: 

lên (0,2)o →1, xuống (2,2)x →0, trái (1,1)o →1, tổng 2 → chẵn → OK 

(2,0): lên (1,0)x, xuống n/a, trái n/a, phải (2,1)x → 0 → chẵn → OK 

(2,1): lên (1,1)o, xuống n/a, trái (2,0)o, phải (2,2)x → 2 → chẵn → OK 

(2,2): lên (1,2)x, xuống n/a, trái (2,1)x, phải n/a → 0 → chẵn → OK 

Tất cả đều → CÓ 

Điều này chứng tỏ việc kiểm tra ranh giới cẩn thận là rất quan trọng để tránh các lỗi riêng lẻ. 

**Đầu vào mẫu 2**```
1
o
```| tôi | j | Hàng xóm với 'o' | đếm % 2 | 
| --- | --- | --- | --- | 
| 0 | 0 | không → 0 | 0 → chẵn → OK | 

Đầu ra: CÓ. Ngay cả với bảng một ô, thuật toán vẫn xử lý nó một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô trong số n² được truy cập một lần, mỗi ô đếm tối đa 4 ô lân cận, thời gian không đổi. | 
| Không gian | O(n²) | Bảng được lưu trong bộ nhớ dưới dạng danh sách n chuỗi có độ dài n. | 

Thuật toán hoạt động tốt trong giới hạn thời gian 1 giây ngay cả với mức tối đa n=100 và mức sử dụng bộ nhớ ở mức tối thiểu so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n = int(input())
    board = [input().strip() for _ in range(n)]
    for i in range(n):
        for j in range(n):
            count = 0
            if i > 0 and board[i-1][j] == 'o':
                count += 1
            if i < n-1 and board[i+1][j] == 'o':
                count += 1
            if j > 0 and board[i][j-1] == 'o':
                count += 1
            if j < n-1 and board[i][j+1] == 'o':
                count += 1
            if count % 2 != 0:
                return "NO"
    return "YES"

# provided samples
assert run("3\nxxo\nxox\noxx\n") == "
```
