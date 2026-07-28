---
title: "CF 102798E - Rất Nhiều Khả Năng..."
description: "Chúng ta có n tay sai của kẻ thù. Lính thứ i bắt đầu với [i] máu. Chúng tôi thực hiện chính xác m đòn tấn công một sát thương. Mỗi đòn tấn công sẽ chọn một cách thống nhất trong số các tay sai vẫn còn sống và làm giảm máu của tay sai đó đi một. Một minion biến mất khi máu của nó về 0."
date: "2026-07-27T17:49:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102798
codeforces_index: "E"
codeforces_contest_name: "2020 China Collegiate Programming Contest, Weihai Site"
rating: 0
weight: 102798
solve_time_s: 41
verified: true
draft: false
---

[CF 102798E - Rất nhiều khả năng...](https://codeforces.com/problemset/problem/102798/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`tay sai của kẻ thù. các`i`-quân thứ bắt đầu bằng`a[i]`sức khỏe. Chúng tôi thực hiện chính xác`m`đòn tấn công một sát thương. Mỗi đòn tấn công sẽ chọn một cách thống nhất trong số các tay sai vẫn còn sống và làm giảm máu của tay sai đó đi một. Một minion biến mất khi máu của nó về 0. 

Nhiệm vụ là tính toán số lượng tay sai dự kiến ​​sẽ chết`m`các cuộc tấn công. 

Những hạn chế quan trọng là`n <= 15`Và`m <= 100`. Giá trị nhỏ của`n`đề xuất mạnh mẽ lập trình động tập hợp con vì chỉ có`2^15 = 32768`có thể có những nhóm tay sai đã chết. Tuy nhiên, các giá trị tình trạng khiến việc lưu trữ trực tiếp mọi cấu hình tình trạng có thể là không thể. Chúng ta chỉ cần lưu trữ thông tin ảnh hưởng đến câu trả lời. 

Một sai lầm phổ biến là cho rằng nếu tổng sát thương ít nhất bằng tổng máu của một số lính thì những lính đó phải chết. Các cuộc tấn công là ngẫu nhiên, vì vậy sát thương có thể bị lãng phí lên các tay sai khác nhau. 

Ví dụ:```
1 1
5
```Câu trả lời là`0`, bởi vì một sát thương không thể giết chết một lính có năm máu. 

Một trường hợp khác là khi một số tay sai đã yêu cầu chính xác tất cả sát thương còn lại.```
2 3
1 100
```Lính đầu tiên chắc chắn sẽ chết, nhưng lính thứ hai không thể nhận đủ sát thương. Câu trả lời là`1`. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp tất cả các chuỗi tấn công có thể xảy ra là không thể. có`n^m`những lựa chọn mục tiêu có thể có, và thậm chí với`n = 15`,`m = 100`, con số này vượt xa những gì có thể khám phá được. 

Quan sát quan trọng là câu trả lời cuối cùng chỉ phụ thuộc vào việc tay sai nào đã chết. Chúng ta không cần lượng máu chính xác còn lại của mọi lính còn sống. Đối với một bộ lính chết cố định`S`, mọi tay sai trong`S`đã tiêu thụ chính xác toàn bộ lượng sát thương máu của nó. Các đòn tấn công còn lại chỉ được phân bổ cho các tay sai khác. 

Chúng tôi sử dụng hai chương trình động. 

DP đầu tiên tính toán xác suất để tập hợp lính chết chính xác`S`sau một số lần tấn công nhất định. Quá trình chuyển đổi coi như tay sai cuối cùng đã chết. Nếu là một tay sai`x`tham gia vào nhóm chết tại thời điểm này, trạng thái trước đó phải là`S`không có`x`và số đòn tấn công trước đó trúng vào lính còn sống sót được xác định bằng tổng lượng máu đã tiêu tốn. 

DP thứ hai đếm xem có bao nhiêu cách thực hiện các đòn tấn công còn lại giữa một nhóm tay sai mà không giết chết bất kỳ ai trong số chúng. Việc kết hợp hai giá trị này sẽ mang lại sự đóng góp xác suất của mọi tập hợp chết cuối cùng có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^m) | O(m) | Quá chậm | 
| Tối ưu | O(m * n * 2^n) | O(m * 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các hệ số nhị thức lên tới`m`. Quá trình chuyển đổi cần có sự kết hợp vì chúng tôi đếm có bao nhiêu vị trí tấn công thuộc về một nhóm tay sai cụ thể. 
2. Tính toán trước tổng sức khỏe của từng tập hợp con. Đối với một tập hợp con`S`,`sum[S]`là số đòn tấn công cần thiết để tiêu diệt chính xác tất cả lính trong`S`. 
3. Tính toán`f[mask]`, xác suất sau số lần tấn công hiện tại, số lính chết chính xác là`mask`. 

Để có mặt nạ không trống, hãy chọn một minion đã chết`x`là người đã chết trong đợt tấn công mới nhất. Bộ chết trước đó là`mask`không có`x`. Số cách sắp xếp các đòn tấn công kết thúc`x`được tính bằng sự kết hợp. 

Ngoài ra còn có một quá trình chuyển đổi trong đó đòn tấn công tiếp theo đánh vào một lính đã sống sót sau trạng thái hiện tại và không tạo ra cái chết mới. 
4. Tính toán`g[k][mask]`, số cách phân phối`k`các cuộc tấn công giữa các tay sai trong`mask`trong khi vẫn giữ cho mọi tay sai còn sống. 

Chọn một tay sai`x`từ mặt nạ. Nó có thể nhận được bất cứ nơi nào từ 0 đến`health[x]-1`các cuộc tấn công. Các đòn tấn công còn lại được xử lý đệ quy bởi các tay sai khác. 
5. Với mọi tập chết cuối cùng có thể`S`, nhân: 

xác suất đó`S`là cái chết được đặt sau quá trình giết chóc thực sự, 

theo số cách hợp lệ mà các đòn tấn công không sử dụng có thể được áp dụng lên những tay sai còn sống sót, 

bởi`|S|`, số lượng tay sai chết. 

Tổng của tất cả những đóng góp này là câu trả lời được mong đợi. 

Tại sao nó hoạt động: 

DP đầu tiên tách quá trình ngẫu nhiên thành những thời điểm mà tay sai thực sự chết. DP thứ hai chịu trách nhiệm về các cuộc tấn công nhằm vào những tay sai còn sống sót và không bao giờ giết được chúng. Mỗi chuỗi tấn công có thể có chính xác một cặp trạng thái tương ứng, do đó tổng cuối cùng sẽ tính mọi kết quả với xác suất chính xác của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    N = 1 << n

    comb = [[0] * (m + 1) for _ in range(m + 1)]
    for i in range(m + 1):
        comb[i][0] = comb[i][i] = 1
        for j in range(1, i):
            comb[i][j] = comb[i - 1][j - 1] + comb[i - 1][j]

    sm = [0] * N
    cnt = [0] * N
    for mask in range(1, N):
        b = mask & -mask
        idx = b.bit_length() - 1
        sm[mask] = sm[mask ^ b] + a[idx]
        cnt[mask] = cnt[mask ^ b] + 1

    f = [0.0] * N
    f[0] = 1.0

    for step in range(1, m + 1):
        nf = [0.0] * N
        for mask in range(N):
            c = cnt[mask]
            if mask:
                x = mask
                while x:
                    b = x & -x
                    i = b.bit_length() - 1
                    prev = mask ^ b
                    need = step - 1 - sm[prev]
                    if need >= a[i] - 1:
                        nf[mask] += f[prev] * comb[need][a[i] - 1]
                    x ^= b
                nf[mask] /= (n - c + 1)
            if n > c:
                nf[mask] += f[mask] / (n - c)
        f = nf

    g = [[0.0] * N for _ in range(m + 1)]
    for mask in range(N):
        g[0][mask] = 1.0

    for step in range(1, m + 1):
        for mask in range(1, N):
            b = mask & -mask
            i = b.bit_length() - 1
            rest = mask ^ b
            for take in range(min(step, a[i] - 1) + 1):
                g[step][mask] += comb[step][take] * g[step - take][rest]

    ans = 0.0
    full = N - 1
    for mask in range(N):
        rem = m - sm[mask]
        if rem >= 0:
            ans += f[mask] * g[rem][full ^ mask] * cnt[mask]

    print("{:.12f}".format(ans))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ thông tin tập hợp con trong mặt nạ số nguyên.`sm[mask]`tránh việc tổng hợp nhiều lần các giá trị sức khỏe, nếu không sẽ thêm một yếu tố khác của`n`bên trong các chuyển tiếp. 

Xác suất DP được cập nhật từng lớp vì chỉ cần số lần tấn công trước đó. DP thứ hai giữ tất cả các lớp vì truy vấn cuối cùng có thể yêu cầu bất kỳ số lần tấn công còn lại nào. 

Tất cả các phép tính đều sử dụng dấu phẩy động vì kết quả là kỳ vọng. Các giá trị vẫn đủ nhỏ để có độ chính xác gấp đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * n * 2^n) | Mỗi trạng thái DP kiểm tra tối đa`n`chuyển tiếp tập hợp con | 
| Không gian | O(m * 2^n) | Các trạng thái được lưu trữ cho DP thứ hai | 

Với`n = 15`Và`m = 100`, số lượng thao tác khoảng vài triệu, vừa vặn thoải mái. 

## Vỏ cạnh 

Khi một quân lính cần nhiều sát thương hơn tất cả các đòn tấn công hiện có, nó không bao giờ có thể xuất hiện trong nhóm chết. Tập hợp con DP đương nhiên không đóng góp cho nó vì không thể đạt được tổng sức khỏe cần thiết. 

Khi một số tay sai có một máu, chúng sẽ được xử lý chính xác vì mỗi tay sai có thể chết sau một đòn duy nhất. Xác suất DP phân biệt mọi tập hợp con có thể xảy ra thay vì giả định số ca tử vong xảy ra theo một thứ tự cố định. 

Khi tất cả tay sai chết ngay sau đó`m`các cuộc tấn công, DP lính còn sống sót được gọi mà không có đòn tấn công nào còn lại và phân phối hợp lệ duy nhất là phân phối trống. Đây là lý do tại sao`g[0][mask] = 1`đối với mọi mặt nạ là cần thiết.
