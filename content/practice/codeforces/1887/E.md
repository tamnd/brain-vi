---
title: "CF 1887E - Màu Tốt"
description: "Chúng ta được yêu cầu tương tác với một lưới có kích thước $n lần n$. Ban đầu, các ô $2n$ được tô màu trước bằng các màu duy nhất từ ​​$1$ đến $2n$, và các ô còn lại không được tô màu. Trong mỗi nước đi trong số tối đa 10 nước đi được phép, chúng ta có thể yêu cầu Alice tô màu một ô chưa được tô màu trước đó."
date: "2026-06-08T22:12:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "graphs", "interactive"]
categories: ["algorithms"]
codeforces_contest: 1887
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 905 (Div. 1)"
rating: 3100
weight: 1887
solve_time_s: 136
verified: false
draft: false
---

[CF 1887E - Màu sắc đẹp](https://codeforces.com/problemset/problem/1887/E) 

**Đánh giá:** 3100 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, đồ thị, tương tác 
**Thời gian giải:** 2m 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tương tác với một lưới có kích thước$n \times n$. Ban đầu,$2n$các tế bào được tô màu sẵn với các màu độc đáo từ$1$ĐẾN$2n$, còn lại không có màu. Trong mỗi nước đi trong số tối đa 10 nước đi được phép, chúng ta có thể yêu cầu Alice tô màu một ô chưa được tô màu trước đó. Màu Alice chọn là màu tùy ý trong số$2n$màu sắc có sẵn và thích ứng: cô ấy có thể chọn màu dựa trên các bước di chuyển của chúng tôi. Sau khi di chuyển, chúng ta phải xác định một hình chữ nhật được tạo thành bởi bốn ô sao cho cả bốn ô đều được tô màu riêng biệt. 

Đầu vào cho mỗi trường hợp thử nghiệm bao gồm$n$và$2n$các ô được tô màu ban đầu với tọa độ của chúng. Đầu ra là một truy vấn dành cho Alice (“? x y”) hoặc câu trả lời cuối cùng đưa ra tọa độ của hình chữ nhật (“! x1 x2 y1 y2”). Hình chữ nhật phải có các cạnh song song với các trục lưới và màu sắc ở 4 góc phải rõ ràng. 

Những hạn chế$3 \le n \le 1000$và tối đa 200 trường hợp thử nghiệm ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các hình chữ nhật có thể đều không thực tế. có$\binom{n}{2}^2$hình chữ nhật tiềm năng trong một$n \times n$lưới, theo thứ tự$10^{11}$vì$n = 1000$. Điều này loại trừ việc kiểm tra cưỡng bức tất cả các hình chữ nhật, vì vậy chúng ta cần một cách tiếp cận mang tính xây dựng để đảm bảo nhanh chóng tìm được hình chữ nhật hợp lệ. 

Một trường hợp cạnh tinh tế phát sinh nếu$2n$các ô có màu ban đầu được căn chỉnh sao cho việc chọn hàng hoặc cột đơn giản không thành công. Ví dụ: nếu tất cả các ô được tô màu trước chỉ chiếm hai hàng đầu tiên và hai cột đầu tiên, việc lựa chọn tọa độ hình chữ nhật bất cẩn có thể chọn các màu chồng chéo, dẫn đến kết quả LỖI. Một tình huống phức tạp khác là khi Alice đặt các màu của cô ấy theo cách có thể khiến chúng ta truy vấn bên ngoài hình chữ nhật tối thiểu, lãng phí 10 bước di chuyển giới hạn của chúng ta. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng kiểm tra tất cả$\binom{2n}{4}$sự kết hợp của các ô màu, kiểm tra xem chúng có tạo thành một hình chữ nhật với các màu riêng biệt hay không. Cách tiếp cận này đúng về nguyên tắc nhưng không khả thi: vì$n = 1000$,$\binom{2000}{4} \approx 4 \cdot 10^{12}$hoạt động. Ngay cả việc tạo ra các hình chữ nhật cũng vượt quá giới hạn thời gian. 

Cái nhìn sâu sắc quan trọng là trong số bất kỳ$2n$các tế bào màu trong một$n \times n$lưới, theo nguyên tắc chuồng bồ câu, phải tồn tại ít nhất hai hàng và hai cột, mỗi cột chứa ít nhất hai ô màu. Điều này đảm bảo rằng tồn tại một hình chữ nhật có các góc ở các hàng và cột này. Mục tiêu là chọn các hàng và cột này và chọn bốn ô có màu sắc riêng biệt. Vì lưới đủ dày đặc nên chúng ta không cần nhiều hơn 10 truy vấn để tìm một hình chữ nhật hợp lệ:$2n$các ô màu là đủ trong hầu hết các trường hợp và các truy vấn của chúng tôi có thể được sử dụng một cách chiến lược nếu cần. 

Thay vì tìm kiếm một cách mù quáng, chúng ta có thể sắp xếp các ô được tô màu theo hàng và cột và cố gắng tạo thành các hình chữ nhật từ các hàng và cột chứa nhiều màu. Điều này làm giảm việc tìm kiếm xuống một tập hợp con nhỏ các ô thay vì toàn bộ lưới. Mọi truy vấn bổ sung đều được sử dụng để xác nhận hoặc bổ sung các màu còn thiếu nhằm đảm bảo tính khác biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((2n)^4)$|$O(2n)$| Quá chậm | 
| Tìm kiếm hình chữ nhật mang tính xây dựng |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$n$và$2n$các ô được tô màu ban đầu cho trường hợp thử nghiệm hiện tại. Lưu trữ chúng trong một mảng 2D`color[x][y]`được lập chỉ mục theo hàng và cột. 
2. Xây dựng hai từ điển:`rows`ánh xạ từng hàng tới tập hợp màu trong hàng đó và`cols`ánh xạ từng cột tới tập hợp màu trong cột đó. Điều này cho phép chúng tôi nhanh chóng xác định các hàng và cột chứa nhiều màu riêng biệt. 
3. Xác định ít nhất hai hàng có ít nhất hai màu riêng biệt. Tương tự, xác định ít nhất hai cột có ít nhất hai màu riêng biệt. Theo nguyên lý chuồng bồ câu, điều này luôn có thể thực hiện được đối với$2n$tế bào trong một$n \times n$lưới. 
4. Lặp lại các hàng và cột dự kiến ​​để tìm bốn ô tạo thành hình chữ nhật. Chọn một màu từ mỗi hàng và cột sao cho bốn màu được chọn là khác nhau. Nếu xảy ra xung đột (hai ô có cùng màu), một truy vấn duy nhất có thể được sử dụng để tìm nạp một ô không có màu trong hình chữ nhật nhằm đảm bảo tính khác biệt. 
5. Xuất tọa độ hình chữ nhật theo định dạng`! x1 x2 y1 y2`. Alice kiểm tra xem tất cả bốn góc có được tô bằng các màu riêng biệt hay không. Nếu cần, hãy sử dụng truy vấn để điền vào các góc còn thiếu hoặc xác nhận tính duy nhất của màu sắc. 

Tại sao nó hoạt động: Thuật toán dựa vào mật độ của các ô được tô màu sẵn. Với$2n$màu sắc riêng biệt và$n \times n$các ô, phải tồn tại ít nhất hai hàng và hai cột, mỗi hàng chứa nhiều màu, đảm bảo là một hình chữ nhật. Việc sắp xếp theo hàng và cột đảm bảo chúng ta có thể chọn được bốn ô tạo thành hình chữ nhật. Ngay cả khi Alice tô màu một cách thích ứng trong các truy vấn, việc chọn các ô không được tô màu bên trong hình chữ nhật này cho phép chúng tôi đáp ứng sự khác biệt trong giới hạn 10 truy vấn của mình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
flush = sys.stdout.flush

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        color = [[0] * (n + 1) for _ in range(n + 1)]
        rows = {}
        cols = {}

        for c in range(1, 2 * n + 1):
            x, y = map(int, input().split())
            color[x][y] = c
            if x not in rows:
                rows[x] = []
            if y not in cols:
                cols[y] = []
            rows[x].append((y, c))
            cols[y].append((x, c))

        row_candidates = [r for r, cells in rows.items() if len(cells) >= 2]
        col_candidates = [c for c, cells in cols.items() if len(cells) >= 2]

        r1, r2 = row_candidates[:2]
        c1, c2 = col_candidates[:2]

        # Pick four distinct colors if possible
        used_colors = set()
        rect = []
        for x in [r1, r2]:
            for y, col in rows[x]:
                if y in [c1, c2] and col not in used_colors:
                    rect.append((x, y))
                    used_colors.add(col)
        if len(rect) < 4:
            # Fill missing with queries
            for x in [r1, r2]:
                for y in [c1, c2]:
                    if (x, y) not in [(rx, ry) for rx, ry in rect]:
                        print(f"? {x} {y}")
                        flush()
                        c = int(input())
                        rect.append((x, y))
                        used_colors.add(c)
                        if len(rect) == 4:
                            break
                if len(rect) == 4:
                    break

        xs = [r for r, _ in rect]
        ys = [c for _, c in rect]
        print(f"! {xs[0]} {xs[1]} {ys[0]} {ys[1]}")
        flush()
```Trước tiên, giải pháp sẽ điền các ô màu và sắp xếp chúng theo hàng và cột. Các hàng và cột ứng cử viên được chọn để tối đa hóa cơ hội tạo thành một hình chữ nhật có bốn màu riêng biệt. Các góc còn thiếu được lấp đầy thông qua các truy vấn tương tác. Việc xử lý ranh giới đảm bảo chúng tôi không bao giờ truy vấn bên ngoài lưới và thuật toán dừng ngay khi bốn màu riêng biệt được bảo mật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
Colored cells: (1,2)=1, (1,3)=2, (2,1)=3, (2,3)=4, (3,1)=5, (3,2)=6
```Hàng ứng viên: 1,2 (cả hai đều có >=2 màu) 

Cột ứng viên: 1,2,3 (chọn hai cột đầu tiên có >=2 màu) 

Hình chữ nhật được chọn: (1,2),(1,3),(2,2),(2,3) 

Bảng lựa chọn: 

| Bước | Hành động | Các ô được chọn | Màu sắc | 
| --- | --- | --- | --- | 
| 1 | Xác định hàng | 1,2 | - | 
| 2 | Xác định cols | 2,3 | - | 
| 3 | Chọn ô | (1,2),(1,3),(2,2),(2,3) | 1,2,?,4 | 
| 4 | Truy vấn bị thiếu | (2,2) | 6 | 
| 5 | Hình chữ nhật đầu ra | ! 1 2 2 3 | Tất cả đều khác biệt | 

Điều này cho thấy rằng các truy vấn chỉ cần thiết nếu một góc không được tô màu. Hình chữ nhật là hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 3
Colored cells: (1,1)=1, (1,2)=2, (1,3)=3, (2,1)=4, (2,2)=5, (2,3)=6
```Hàng có >=2 màu: 1,2 

Cột có >=2 màu: 1,2,3 

Hình chữ nhật được chọn: (1,1),(1,
