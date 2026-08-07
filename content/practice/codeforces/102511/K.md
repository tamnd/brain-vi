---
title: "CF 102511K - Tai nạn giao thông"
description: "Chúng ta cần phân tích một dòng đèn giao thông. Một chiếc ô tô khởi động ở vị trí 0 tại một thời điểm có giá trị thực ngẫu nhiên. Vì nó di chuyển một mét mỗi giây nên thời gian nó chạm tới đèn là thời gian bắt đầu cộng với vị trí của đèn đó."
date: "2026-08-06T19:31:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102511
codeforces_index: "K"
codeforces_contest_name: "2019 ICPC World Finals"
rating: 0
weight: 102511
solve_time_s: 73
verified: true
draft: false
---

[CF 102511K - Tai nạn giao thông](https://codeforces.com/problemset/problem/102511/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần phân tích một dòng đèn giao thông. Một chiếc ô tô khởi động ở vị trí 0 tại một thời điểm có giá trị thực ngẫu nhiên. Vì nó di chuyển một mét mỗi giây nên thời gian nó chạm tới đèn là thời gian bắt đầu cộng với vị trí của đèn đó. Đèn có màu đỏ hoặc xanh theo chu kỳ lặp lại của riêng nó và ô tô chỉ thành công nếu mọi đèn đều có màu xanh đúng thời điểm nó đến. 

Nhiệm vụ là tính toán hai loại xác suất. Đối với mỗi đèn, chúng ta cần xác suất rằng đó là đèn đầu tiên nơi ô tô dừng lại. Chúng ta cũng cần xác suất để chiếc xe vượt qua mọi ánh sáng. 

Khó khăn là khoảng thời gian bắt đầu rất lớn. Giá trị 2019! được chọn vì nó chứa mọi chu kỳ ánh sáng có thể có làm ước số, do đó việc phân bố thời gian bắt đầu giống hệt như việc chọn thời gian ngẫu nhiên trên một chu kỳ kết hợp hoàn chỉnh. Bản thân chu trình kết hợp đã quá lớn để có thể liệt kê hết. 

Số lượng đèn chỉ là 500 và mỗi chu kỳ nhiều nhất là 100. Việc mô phỏng theo thời gian là không thể vì ngay cả sự kết hợp một chu kỳ cũng có thể rất lớn. Giới hạn hữu ích là kích thước khoảng thời gian, không phải độ dài của khoảng thời gian. Chúng ta cần một biểu diễn chỉ giữ lại các mẫu tuần hoàn có thể được tạo bởi các chu kỳ lên tới 100. 

Một sai lầm phổ biến là cho rằng đèn độc lập. Họ không như vậy. Ví dụ, hai đèn có chu kỳ 4 và 6 có mối tương quan với nhau vì thời gian bắt đầu giống nhau sẽ ảnh hưởng đến cả hai. Một sai lầm khác là xử lý sai sự xuất hiện ở cuối khoảng màu đỏ. Đèn xanh bắt đầu vào thời điểm`r`, do đó khoảng là nửa mở. 

Ví dụ: với một đèn ở vị trí 1 với`r = 1, g = 1`, một nửa số lần xuất phát khiến xe đến trong giây màu đỏ và một nửa đến trong giây màu xanh lá cây. Câu trả lời là:```
0.5
0.5
```Ở đây, phương pháp lấy mẫu thời gian bắt đầu là số nguyên không thành công vì thời gian bắt đầu là liên tục. Các điểm biên có xác suất bằng 0 và không được coi là trường hợp rời rạc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tìm chu kỳ lặp lại hoàn chỉnh của tất cả các ánh sáng, sau đó quét mọi khoảnh khắc có thể có trong chu kỳ đó. Nó đúng vì hệ thống giao thông lặp lại chính xác. Tuy nhiên, bội số chung nhỏ nhất của các khoảng thời gian từ 1 đến 100 lớn hơn nhiều so với bất kỳ cấu trúc dữ liệu khả thi nào, do đó phương pháp này thậm chí không thể xây dựng được dòng thời gian. 

Quan sát quan trọng là mặc dù chu kỳ chung rất lớn nhưng mọi hàm tuần hoàn có liên quan chỉ có một số lượng nhỏ các tần số có thể có. Một ánh sáng với thời gian`p`có thể được mô tả bằng hệ số Fourier với tần số`k/p`. Sau khi kết hợp tất cả các khoảng thời gian có thể lên tới 100, số phân số rút gọn riêng biệt`k/p`chỉ khoảng 3000. 

Thay vì lưu trữ toàn bộ dòng thời gian, chúng tôi lưu trữ biểu diễn Fourier của tập hợp thời gian bắt đầu vẫn còn tồn tại. Nhân với chức năng xanh của đèn sẽ loại bỏ những xe dừng ở đèn đó. Nhân với hàm màu đỏ và lấy hệ số Fourier không đổi sẽ cho xác suất rằng đèn hiện tại bị hỏng đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(LCM của các kỳ) | O(LCM của các kỳ) | Quá chậm | 
| Biểu diễn Fourier | O(n*3044*100) | O(3044) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước mọi tần số giảm có thể`a/b`Ở đâu`b <= 100`. Cung cấp cho mỗi tần số như vậy một mã định danh số nguyên. Đồng thời tính toán trước cách hai tần số cộng modulo 1. 
2. Đối với mỗi đèn giao thông, hãy xây dựng hệ số Fourier của khoảng màu đỏ và khoảng màu xanh lá cây. Khoảng thời gian bị dịch chuyển bởi vị trí đèn vì ô tô khởi hành vào thời điểm`t`đạt đến ánh sáng vào thời điểm`t + x`. 
3. Duy trì bản đồ Fourier biểu thị tập hợp thời gian bắt đầu đã vượt qua tất cả các đèn trước đó. Ban đầu đây là hàm hằng số 1. 
4. Trước khi cập nhật đèn hiện tại, hãy nhân chức năng sống sót hiện tại với chức năng màu đỏ của đèn hiện tại. Hệ số của tần số 0 chính xác là phần số lần khởi động không thành công ở ánh sáng này trước tiên. 
5. Nhân hàm sống sót với hàm màu xanh của đèn hiện tại và tiếp tục. Sau tất cả các đèn, hệ số tần số bằng 0 là xác suất đạt đến điểm cuối. 

Điều bất biến là sau khi xử lý lần đầu tiên`i`đèn, biểu diễn Fourier chính xác là chức năng chỉ báo thời gian bắt đầu vượt qua những điều đó`i`đèn. Phép nhân màu đỏ trích xuất tập hợp con bị lỗi ở thời điểm hiện tại, trong khi phép nhân màu xanh lá cây chỉ giữ lại những thời điểm còn tồn tại. Vì phép nhân Fourier thể hiện phép nhân của các hàm tuần hoàn nên mọi cập nhật đều giữ nguyên ý nghĩa này. 

## Giải pháp Python```python
import sys
import math
from array import array

input = sys.stdin.readline

freqs = []
freq_id = {}
for d in range(1, 101):
    for a in range(d):
        g = math.gcd(a, d)
        if g == 1:
            key = (a, d)
            freq_id[key] = len(freqs)
            freqs.append(key)
freq_id[(0, 1)] = len(freqs)
freqs.append((0, 1))

m = len(freqs)

add_table = []
for a, b in freqs:
    row = array('H')
    for c, d in freqs:
        num = a * d + c * b
        den = b * d
        num %= den
        if num == 0:
            row.append(freq_id[(0, 1)])
        else:
            g = math.gcd(num, den)
            row.append(freq_id[(num // g, den // g)])
    add_table.append(row)

TWO_PI = 2.0 * math.pi

def make_fourier(x, r, g, red):
    p = r + g
    length = r if red else g
    start = 0 if red else r
    end = start + length
    res = []
    for k in range(p):
        if k == 0:
            val = length / p
        else:
            a = math.cos(-TWO_PI * k * start / p) + 1j * math.sin(-TWO_PI * k * start / p)
            b = math.cos(-TWO_PI * k * end / p) + 1j * math.sin(-TWO_PI * k * end / p)
            val = (a - b) / (2j * math.pi * k)
        if x and k:
            val *= math.cos(TWO_PI * k * x / p) + 1j * math.sin(TWO_PI * k * x / p)
        if abs(val) > 1e-14:
            if k == 0:
                idx = freq_id[(0, 1)]
            else:
                z = k
                d = p
                gg = math.gcd(z, d)
                idx = freq_id[(z // gg, d // gg)]
            res.append((idx, val))
    return res

def multiply(cur, poly):
    nxt = {}
    add = add_table
    for a, va in cur.items():
        row = add[a]
        for b, vb in poly:
            c = row[b]
            nxt[c] = nxt.get(c, 0j) + va * vb
    return nxt

def solve():
    n = int(input())
    lights = []
    for _ in range(n):
        x, r, g = map(int, input().split())
        lights.append((x, r, g))

    cur = {freq_id[(0, 1)]: 1.0 + 0j}
    ans = []
    zero = freq_id[(0, 1)]

    for x, r, g in lights:
        red = make_fourier(x, r, g, True)
        fail = multiply(cur, red)
        ans.append(fail.get(zero, 0).real)
        green = make_fourier(x, r, g, False)
        cur = multiply(cur, green)

    for v in ans:
        print("{:.12f}".format(v))
    print("{:.12f}".format(cur.get(zero, 0).real))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý tạo ra một vũ trụ nhỏ gọn gồm tất cả các tần số có thể. Bảng cộng được lưu trữ bằng cách sử dụng số nguyên 16 bit vì có ít hơn 65536 tần số có thể có, giúp giữ cho bước tích chập nhanh. 

Cấu trúc Fourier sử dụng tích phân của hàm chỉ báo khoảng. Sự dịch chuyển vị trí nhân mỗi tần số với hệ số pha tương ứng, phù hợp với thực tế là ô tô chạm tới ánh sáng muộn hơn một cách chính xác.`x`giây. 

Thói quen nhân là cốt lõi của giải pháp. Nó kết hợp mọi tần số hiện có với mọi tần số từ ánh sáng hiện tại. Vì toán hạng thứ hai có tối đa 100 mục nên số lượng thao tác vẫn nhỏ. 

Hệ số không đổi là giá trị trung bình của hàm được biểu diễn. Vì các hàm của chúng ta là các chỉ báo nên giá trị trung bình đó chính xác là xác suất được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*3044*100) | Mỗi ánh sáng nhân phổ hiện tại lên tối đa 100 tần số | 
| Không gian | O(3044) | Chỉ các tần số Fourier có thể được lưu trữ | 

Phổ lớn nhất chỉ có vài nghìn mục, vì vậy giải pháp phù hợp thoải mái trong giới hạn nhất định. 

## Vỏ cạnh 

Một đèn không có thời gian màu đỏ có`r = 0`. Hàm Fourier màu đỏ của nó trống nên xác suất thất bại của nó bằng 0. Hàm màu xanh lá cây là hàm hằng số, nghĩa là ánh sáng không ảnh hưởng đến sự phân bố người sống sót. 

Một ngọn đèn không có thời gian xanh có`g = 0`. Mọi chiếc xe còn sống sót khi đến gần đều dừng lại. Hàm màu đỏ trở thành hàm chỉ báo đầy đủ và xác suất sống sót cuối cùng trở thành 0 sau bản cập nhật này. 

Xe đến đúng thời điểm đèn chuyển từ đỏ sang xanh phải đi tiếp. Việc tích hợp khoảng thời gian sử dụng`[0, r)`cho màu đỏ và`[r, r+g)`đối với màu xanh lá cây, do đó các điểm biên được gán chính xác. Những điểm này không ảnh hưởng đến xác suất, nhưng việc sử dụng các khoảng đóng có thể gây ra hệ số Fourier không chính xác.
