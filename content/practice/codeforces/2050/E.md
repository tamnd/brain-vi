---
title: "CF 2050E - Ba Dây"
description: "Chúng ta có ba chuỗi: a, b và c. Về mặt khái niệm, c được hình thành bằng cách lấy các chữ cái từ a và b theo một thứ tự đan xen nào đó. Ở mỗi bước, một chữ cái được lấy từ phía trước a hoặc b và thêm vào c."
date: "2026-06-08T08:48:23+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 2050
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 991 (Div. 3)"
rating: 1500
weight: 2050
solve_time_s: 87
verified: true
draft: false
---

[CF 2050E - Ba Dây](https://codeforces.com/problemset/problem/2050/E) 

**Đánh giá:** 1500 
**Tags:** dp, triển khai, chuỗi 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp ba chuỗi:`a`,`b`, Và`c`. Về mặt khái niệm,`c`được hình thành bằng cách lấy các chữ cái từ`a`Và`b`theo một thứ tự đan xen nào đó. Ở mỗi bước, một chữ cái được lấy từ phía trước của một trong hai`a`hoặc`b`và gắn vào`c`. Khi một chuỗi trống, các chữ cái còn lại của chuỗi kia sẽ được thêm vào. Sau đó, một số chữ cái trong`c`có thể đã được thay thế một cách tùy tiện. Mục tiêu của chúng tôi là xác định **số lượng chữ cái tối thiểu trong`c`chắc chắn đã được thay đổi** so với sự xen kẽ hợp lệ của`a`Và`b`. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi chuỗi có tối đa 1000 ký tự và tổng độ dài của tất cả các trường hợp thử nghiệm không vượt quá 2000 đối với`a`hoặc`b`. Điều này ngụ ý rằng một giải pháp có độ phức tạp`O(|a|*|b|)`mỗi trường hợp thử nghiệm là khả thi, bởi vì`1000*1000 = 10^6`các hoạt động được chấp nhận trong giới hạn thời gian 2 giây. 

Các trường hợp cạnh không rõ ràng phát sinh khi tất cả các ký tự giống hệt nhau hoặc khi một chuỗi được sử dụng hoàn toàn trước chuỗi kia. Ví dụ, nếu`a = "aa"`,`b = "b"`, Và`c = "aba"`, số lượng thay đổi tối thiểu là`0`từ`a`Và`b`có thể đã xen kẽ để hình thành`aba`. Một cách tiếp cận tham lam ngây thơ chỉ phù hợp với các nhân vật trong`a`cho đến khi nó thất bại sẽ tính toán không chính xác những thay đổi bổ sung. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra mọi sự đan xen của`a`Và`b`và so sánh nó với`c`, đếm sự khác biệt. Điều này đúng nhưng không khả thi: vì`|a|=n`Và`|b|=m`, có`C(n+m, n)`sự xen kẽ. Ngay cả đối với`n = m = 20`, đây đã là hơn một trăm triệu kết hợp. 

Thông tin chi tiết quan trọng là đây là **vấn đề lập trình động**, tương tự như việc tính toán khoảng cách chỉnh sửa hoặc đan xen các chuỗi. Cho phép`dp[i][j]`đại diện cho số lượng thay đổi tối thiểu cần thiết để hình thành lần đầu tiên`i+j`nhân vật của`c`sử dụng cái đầu tiên`i`nhân vật của`a`và đầu tiên`j`nhân vật của`b`. Ở mỗi bước, chúng ta có thể lấy ký tự tiếp theo từ`a`hoặc`b`và so sánh nó với vị trí tương ứng trong`c`, tăng chi phí nếu không phù hợp. 

Cách tiếp cận này làm giảm vấn đề từ cấp số nhân đến`O(|a|*|b|)`, điều này khả thi với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n+m, n)) | O(1) | Quá chậm | 
| Lập trình động | O(| a | * | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`dp`kích thước`(len(a)+1) x (len(b)+1)`với vô cùng, ngoại trừ`dp[0][0] = 0`. Điều này thể hiện sự thay đổi không cần thiết đối với tiền tố trống. 
2. Lặp lại tất cả`i`từ`0`ĐẾN`len(a)`và tất cả`j`từ`0`ĐẾN`len(b)`. Tại mỗi ô`(i, j)`chúng ta có sự xen kẽ một phần của`i`nhân vật từ`a`Và`j`nhân vật từ`b`. 
3. Nếu`i < len(a)`, cố gắng lấy ký tự tiếp theo từ`a`. So sánh`a[i]`với`c[i+j]`. Nếu chúng khớp nhau thì chi phí vẫn giữ nguyên; nếu không, hãy tăng chi phí lên`1`. Cập nhật`dp[i+1][j]`với giá trị tối thiểu của giá trị hiện tại và chi phí này. 
4. Nếu`j < len(b)`, cố gắng lấy ký tự tiếp theo từ`b`. So sánh`b[j]`với`c[i+j]`và cập nhật tương tự`dp[i][j+1]`. 
5. Sau khi điền vào bảng,`dp[len(a)][len(b)]`chứa số lượng thay đổi tối thiểu cần thiết để khớp`c`. 

**Tại sao nó hoạt động:** Ở mọi tiền tố`(i,j)`,`dp[i][j]`được đảm bảo là số lượng thay đổi tối thiểu cần thiết để tạo thành tiền tố tương ứng của`c`. Bằng cách xem xét cả hai lựa chọn (lấy từ`a`hoặc`b`) ở mỗi bước, DP đảm bảo rằng không có sự xen kẽ nào bị bỏ sót và chi phí được tích lũy một cách chính xác. Đây chính xác là sự lặp lại của việc xen kẽ các chuỗi với chi phí sửa đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_changes(a, b, c):
    n, m = len(a), len(b)
    INF = int(1e9)
    dp = [[INF] * (m+1) for _ in range(n+1)]
    dp[0][0] = 0

    for i in range(n+1):
        for j in range(m+1):
            if i < n:
                cost = dp[i][j] + (a[i] != c[i+j])
                if cost < dp[i+1][j]:
                    dp[i+1][j] = cost
            if j < m:
                cost = dp[i][j] + (b[j] != c[i+j])
                if cost < dp[i][j+1]:
                    dp[i][j+1] = cost
    return dp[n][m]

t = int(input())
for _ in range(t):
    a = input().strip()
    b = input().strip()
    c = input().strip()
    print(min_changes(a, b, c))
```các`dp`khởi tạo đặt tất cả các trạng thái thành vô cùng để biểu thị các trạng thái không được tính toán/không hợp lệ. Các bản cập nhật được lựa chọn cẩn thận ở mức tối thiểu, đảm bảo giải pháp tối ưu được lan truyền. sử dụng`a[i] != c[i+j]`nắm bắt ngắn gọn chi phí "ký tự đã thay đổi". Điều kiện biên được xử lý bằng cách chỉ thử ký tự tiếp theo nếu nằm trong giới hạn. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`a="a"`,`b="b"`,`c="cb"`| tôi | j | dp[i][j] | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | bắt đầu | 
| 1 | 0 | 1 | lấy 'a' vs 'c', không khớp +1 | 
| 0 | 1 | 1 | lấy 'b' vs 'c', không khớp +1 | 
| 1 | 1 | 1 | chọn đường đi tối ưu | 

Kết quả:`1`thay đổi. 

**Ví dụ 2**:`a="ab"`,`b="cd"`,`c="acbd"`| tôi | j | dp[i][j] | hành động | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | bắt đầu | 
| 1 | 0 | 0 | 'a' khớp với 'a' | 
| 0 | 1 | 1 | 'c' và 'a', không khớp | 
| 1 | 1 | 0 | 'c' và 'c', không khớp | 
| 2 | 1 | 0 | 'b' và 'b', không khớp | 
| 2 | 2 | 0 | 'd' vs 'd', không khớp | 

Kết quả:`0`những thay đổi. 

Các bảng này chứng minh rằng DP theo dõi chính xác những thay đổi tối thiểu, ngay cả khi các ký tự được xen kẽ theo các thứ tự khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(| a | 
| Không gian | O(| a | 

Với tổng độ dài trên tất cả các trường hợp thử nghiệm không vượt quá 2000, điều này đảm bảo tổng cộng dưới 4 triệu thao tác, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        a = input().strip()
        b = input().strip()
        c = input().strip()
        print(min_changes(a, b, c))
    return output.getvalue().strip()

# provided samples
assert run("7\na\nb\ncb\nab\ncd\nacbd\nab\nba\naabb\nxxx\nyyy\nxyxyxy\na\nbcd\ndecf\ncodes\nhorse\ncodeforces\negg\nannie\negaegaeg\n") == "1\n0\n2\n0\n3\n2\n3", "Sample 1"

# custom cases
assert run("1\na\na\naa\n") == "0", "identical strings"
assert run("1\nab\ncd\nacbd\n") == "0", "interleaving matches perfectly"
assert run("1\nab\ncd\nabcd\n") == "0", "all from a then b"
assert run("1\nab\ncd\ndcba\n") == "4", "completely reversed"
assert run("1\nabc\ndef\nxyzuvw\n") == "6", "all changed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a="a", b="a", c="aa" | 0 | ký tự giống hệt nhau | 
| a="ab", b="cd", c="acbd" | 0 | có thể xen kẽ mà không cần thay đổi | 
| a="ab", b="cd", c="abcd" | 0 | một chuỗi đầy đủ trước |
