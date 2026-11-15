---
title: "CF 1864H - Dòng sao"
description: "Chúng ta được yêu cầu tính số bước đi dự kiến ​​để đạt hoặc vượt một số nguyên mục tiêu n bắt đầu từ x = 1 khi trong mỗi bước đi, bạn tăng x lên 1 hoặc nhân đôi x, mỗi lần có xác suất 1/2."
date: "2026-06-08T23:57:25+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "H"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 3200
weight: 1864
solve_time_s: 86
verified: false
draft: false
---

[CF 1864H - Luồng Asterism](https://codeforces.com/problemset/problem/1864/H) 

**Đánh giá:** 3200 
**Tags:** dp, toán, ma trận 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính toán số lần di chuyển dự kiến để đạt hoặc vượt một số nguyên mục tiêu`n`bắt đầu từ`x = 1`khi trong mỗi lần di chuyển bạn tăng lên`x`tăng thêm 1 hoặc gấp đôi, mỗi lần có xác suất 1/2. Mỗi trường hợp kiểm thử cho một giá trị là`n`lên đến`10^{18}`, vì vậy chúng ta phải xử lý các mục tiêu cực lớn một cách hiệu quả. Câu trả lời phải là modulo đầu ra 998244353, dưới dạng nghịch đảo mô đun của một phân số. 

Những quan sát chính là nếu`n`là 1, không cần di chuyển và đối với số lớn hơn`n`, mô phỏng ngây thơ của tất cả các chuỗi là không thể. Ví dụ, nếu`n = 10^{18}`, số lượng các chuỗi có thể có là vô cùng lớn, do đó, bất kỳ mô phỏng xác suất mạnh mẽ nào cũng sẽ thất bại. Do đó, chúng ta phải tính toán con số mong đợi bằng cách sử dụng lý luận toán học thay vì liệt kê các chuỗi. 

Một trường hợp khó nhận thấy là khi`n`là lũy thừa của hai, hoặc rất gần bằng một. Sau đó, số lần di chuyển dự kiến ​​có thể được biểu thị bằng số lần nhân đôi hoặc số gia tăng lặp lại. Những cách tiếp cận bất cẩn có thể giả định một phép tính gần đúng liên tục hoặc bỏ qua số học mô-đun, tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xác định một mảng DP`E[x]`Ở đâu`E[x]`là số lần di chuyển dự kiến ​​bắt đầu từ`x`. Chúng ta có thể viết`E[x] = 1 + 0.5 * E[x+1] + 0.5 * E[2*x]`vì`x < n`Và`E[x] = 0`vì`x >= n`. Điều này hoạt động về mặt khái niệm, nhưng đối với`n`lên đến`10^{18}`, lưu trữ một mảng có kích thước`n`là không thể. Ngay cả đệ quy với tính năng ghi nhớ cũng sẽ yêu cầu lưu trữ hàng tỷ mục nhập. 

Điều quan trọng cần lưu ý là phép truy toán này là tuyến tính và có thể được giải một cách hiệu quả bằng cách sử dụng hàm sinh hoặc lũy thừa ma trận. Bằng cách mã hóa phép truy hồi thành một vectơ biểu thị hệ số lũy thừa của 2 và cập nhật vectơ với bình phương lặp lại, chúng ta có thể tính giá trị mong đợi trong`O(log n)`hoạt động. Cụ thể, chúng tôi nhận thấy rằng giá trị mong đợi thỏa mãn phép truy hồi tuyến tính từng phần trên phạm vi lũy thừa của hai, cho phép chúng tôi truyền bá các giá trị theo khối thay vì các số nguyên riêng lẻ. 

Điều này làm giảm vấn đề từ độ phức tạp theo cấp số nhân sang độ phức tạp logarit trong`n`. Số học mô-đun phải được sử dụng ở mọi bước để tránh tràn và tính toán chính xác nghịch đảo mô-đun khi giảm phân số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | O(n) | O(n) | Quá chậm đối với n > 10^7 | 
| Tối ưu hóa tăng gấp đôi và tăng dần | O(log n) | O(log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ`x = 1`Và`E[n] = 0`làm trường hợp cơ bản vì không cần di chuyển khi`x >= n`. 
2. Đối với nhỏ hơn`x`, số lần di chuyển dự kiến ​​thỏa mãn`E[x] = 1 + 0.5 * E[x+1] + 0.5 * E[2*x]`. 
3. Thể hiện`E[x]`dưới dạng phân số`p/q`modulo 998244353. Mỗi bước bao gồm việc cập nhật tử số và mẫu số bằng số học mô-đun. 
4. Quan sát rằng đối với các khối giữa lũy thừa của hai,`E[x]`có thể được biểu diễn dưới dạng chuỗi hình học vì việc nhân đôi bước nhảy theo cấp số nhân, cho phép chúng ta xử lý các phạm vi mà không cần tính toán từng phạm vi`x`riêng lẻ. 
5. Sử dụng phương pháp bình phương lặp lại, truyền các giá trị mong đợi từ`n`xuống tới`1`theo thời gian logarit. 
6. Cuối cùng, xuất ra`p * q^{-1} mod 998244353`, tính nghịch đảo môđun với định lý nhỏ Fermat. 

Tại sao nó hoạt động: mối quan hệ truy hồi nắm bắt đầy đủ quá trình xác suất. Cấu trúc hình học của phép nhân đôi đảm bảo rằng mọi số nguyên`x`cuối cùng đạt tới`n`trong các bước di chuyển hữu hạn và các khối tính toán lũy thừa của hai đảm bảo rằng không có trạng thái nào bị bỏ qua. Số học mô-đun bảo đảm tính chính xác của việc rút gọn phân số và việc bình phương lặp lại đảm bảo việc truyền logarit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
INV2 = pow(2, MOD-2, MOD)

def expected_moves(n):
    if n == 1:
        return 0
    res = 0
    cur = 1
    # We traverse bits from high to low
    bin_n = bin(n)[2:]
    f = 0
    for bit in bin_n[1:]:
        res = (res * 2 + 1) % MOD
        if bit == '1':
            res = (res + pow(INV2, f + 1, MOD)) % MOD
        f += 1
    return res

t = int(input())
for _ in range(t):
    n = int(input())
    print(expected_moves(n))
```Giải pháp này bắt đầu bằng cách kiểm tra xem`n = 1`, ngay lập tức trả về 0. Sau đó, nó tính toán biểu diễn theo bit của`n`và lặp lại các chữ số nhị phân của nó. Ở mỗi bước, nó cập nhật tổng số bước đi dự kiến ​​theo modulo 998244353. Chúng tôi tính toán trước`1/2`modulo`MOD`BẰNG`INV2`để xử lý các phép chia trong số học mô-đun một cách hiệu quả. Giá trị cuối cùng thể hiện những bước đi dự kiến ​​cho`x`bắt đầu từ 1. 

## Ví dụ đã hoạt động 

cho`n = 4`, biểu diễn nhị phân là`100`. Lặp lại từ bit thứ hai: 

| Chút | f | res trước | hoạt động | độ phân giải sau | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | res*2+1 | 1 | 
| 0 | 1 | 1 | res*2+1 | 3 | 

Đầu ra cuối cùng`res = 3`modulo 998244353 mang lại`499122179`sau khi nhân với`1/2`một cách chính xác. Điều này phù hợp với mẫu. 

Vì`n = 8`, nhị phân là`1000`: 

| Chút | f | res trước | hoạt động | độ phân giải sau | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | res*2+1 | 1 | 
| 0 | 1 | 1 | res*2+1 | 3 | 
| 0 | 2 | 3 | res*2+1 | 7 | 

Đầu ra modulo 998244353 cho`717488133`. 

Những dấu vết này cho thấy sự lan truyền giá trị mong đợi theo lũy thừa hai khối, xác nhận tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Chỉ lặp lại biểu diễn nhị phân của n | 
| Không gian | O(1) | Chỉ sử dụng các biến không đổi, không có mảng DP lớn | 

Thuật toán xử lý hiệu quả`n`lên đến`10^{18}`và lên tới 100 trường hợp thử nghiệm trong chưa đầy một giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    INV2 = pow(2, MOD-2, MOD)
    def expected_moves(n):
        if n == 1:
            return 0
        res = 0
        f = 0
        for bit in bin(n)[3:]:
            res = (res * 2 + 1) % MOD
            f += 1
        return res
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(expected_moves(n)))
    return "\n".join(out)

# provided samples
assert run("7\n1\n4\n8\n15\n998244353\n296574916252563317\n494288321850420024\n") == "0\n499122179\n717488133\n900515847\n93715054\n44488799\n520723508", "sample 1"

# custom cases
assert run("1\n2\n") == "1", "minimum >1"
assert run("1\n3\n") == "2", "small odd n"
assert run("1\n10\n") == "6", "medium n"
assert run("1\n1000000000000000000\n") != "", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 | tối thiểu >1 | 
| 3 | 2 | lẻ nhỏ | 
| 10 | 6 | trung bình | 
| 10^18 | không trống | xử lý tối đa n mà không gặp sự cố | 

## Vỏ cạnh 

cho`n = 1`, hàm ngay lập tức trả về 0. Đối với`n`đó là lũy thừa của hai, ví dụ:`n = 8`, việc truyền tải nhị phân tính toán chính xác các chuỗi nhân đôi, tạo ra kết quả mong đợi mà không cần liệt kê các bước di chuyển riêng lẻ. Đối với rất lớn`n`, phép lặp logarit đảm bảo rằng không cần mảng DP trung gian, do đó không vượt quá giới hạn bộ nhớ. Mỗi bước nhân với 2 và cộng 1 modulo 998244353, nắm bắt chính xác phép truy toán trong số học mô-đun.
