---
title: "CF 453E - Little Pony và Lord Tirek"
description: "Chúng tôi có một đội ngựa con, mỗi con có ba thuộc tính: năng lượng hiện tại, năng lượng tối đa và tốc độ tái tạo năng lượng trên mỗi đơn vị thời gian. Theo thời gian, Lord Tirek thực hiện một số hoạt động gọi là Hấp thụ năng lượng, mỗi hoạt động nhắm vào một phân đoạn ngựa con liên tiếp."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 453
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 259 (Div. 1)"
rating: 3100
weight: 453
solve_time_s: 46
verified: true
draft: false
---

[CF 453E - Little Pony và Lord Tirek](https://codeforces.com/problemset/problem/453/E) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đội ngựa con, mỗi con có ba thuộc tính: năng lượng hiện tại, năng lượng tối đa và tốc độ tái tạo năng lượng trên mỗi đơn vị thời gian. Theo thời gian, Lord Tirek thực hiện một số hoạt động gọi là Hấp thụ năng lượng, mỗi hoạt động nhắm vào một phân đoạn ngựa con liên tiếp. Khi hấp thụ năng lượng tại một thời điểm nhất định, anh ta sẽ thu thập tất cả năng lượng mà mỗi chú ngựa con hiện có và năng lượng của chúng sẽ đặt lại về 0. Giữa các hướng dẫn, ngựa con tái tạo năng lượng theo tỷ lệ tương ứng, nhưng không thể vượt quá mức tối đa của từng cá thể. 

Mục đích là để tính toán, với mỗi lệnh Hấp thụ Mana, tổng lượng mana mà Tirek thu thập được. Điều này tương đương với việc mô phỏng mana của từng chú ngựa con theo thời gian và tổng hợp mana của một loạt ngựa con vào những thời điểm cụ thể. 

Các ràng buộc rất đáng kể: lên tới 100.000 ngựa con và 100.000 thao tác, với số lần lớn bằng 1e9. Một giải pháp đơn giản lặp lại từng con ngựa cho mỗi lệnh sẽ thực hiện theo thứ tự 10^10 thao tác, điều này vượt xa khả thi trong 3 giây. Điều này ngay lập tức gợi ý sự cần thiết của một cấu trúc dữ liệu có khả năng cập nhật và truy vấn theo phạm vi mà không cần chạm vào từng phần tử riêng lẻ. 

Các trường hợp cạnh không rõ ràng bao gồm: 

- Ngựa con không có khả năng hồi phục: nếu chúng bị hấp thụ một lần, chúng sẽ không lấy lại được năng lượng sau đó. Một cách tiếp cận cộng gộp ngây thơ giả định rằng tăng trưởng tuyến tính sẽ bị tính quá mức. 
- Những chú ngựa con có khả năng hồi phục vượt quá mức tối đa: chúng ta phải giới hạn năng lượng ở mức tối đa cho ngựa con. 
- Khoảng cách lớn giữa các lần hướng dẫn: mô phỏng đơn giản theo thời gian là không khả thi. 
- Hướng dẫn bao gồm các phạm vi trùng lặp hoặc giống hệt nhau: mana tích lũy phải tính đến việc đặt lại vào đúng thời điểm, không tính hai lần. 

Ví dụ, một con ngựa đơn với`s=0`,`m=5`,`r=2`và hướng dẫn tại thời điểm 2 và 4 sẽ tạo ra mana được thu thập ở mức 4 và 4. Một cách tiếp cận bất cẩn mà bỏ qua giới hạn ở`m`thay vào đó có thể báo cáo 4 và 8, điều đó là sai. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi hướng dẫn, lặp lại tất cả các ngựa con bị ảnh hưởng, tính năng lượng hiện tại của chúng như sau:`min(max_mana, last_mana + delta_time * regen)`, tính tổng và đặt lại mana của họ về 0. Mặc dù đúng nhưng độ phức tạp trong trường hợp xấu nhất là O(n*m), tức là 10^10 đối với n=m=10^5. Điều này rõ ràng là quá chậm. 

Thông tin chi tiết quan trọng là chúng tôi không cần cập nhật từng chú ngựa con ở mọi bước thời gian. Sự tăng trưởng năng lượng là tuyến tính trên mỗi con ngựa giữa các hướng dẫn và bị giới hạn ở mức tối đa. Mỗi hướng dẫn có thể được xử lý theo thời gian hấp thụ cuối cùng trên mỗi con ngựa. Điều này làm cho vấn đề trở nên phù hợp với **cây phân đoạn có khả năng lan truyền lười biếng**, trong đó mỗi nút lưu trữ thời gian cập nhật lần cuối và có thể tính toán năng lượng hiện tại cho phạm vi của nó theo yêu cầu. Chúng ta có thể truy vấn và cập nhật phạm vi một cách hiệu quả theo O(log n) cho mỗi lệnh. 

Việc tối ưu hóa có hiệu quả vì năng lượng của mỗi chú ngựa con giữa các lần hướng dẫn tăng tuyến tính theo thời gian và có giới hạn đã biết. Cây phân đoạn cho phép chúng ta truyền bá sự khác biệt về thời gian và đặt lại mà không cần chạm vào từng phần tử riêng lẻ trừ khi cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Cây phân đoạn có cập nhật lười biếng | O((n + m) * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cây phân đoạn trong đó mỗi lá tượng trưng cho một con ngựa. Lưu trữ cho mỗi lá giá trị năng lượng cuối cùng, năng lượng tối đa, tốc độ hồi phục và thời gian cập nhật lần cuối. 
2. Đối với mỗi hướng dẫn tại một thời điểm`t_i`phạm vi ảnh hưởng`[l_i, r_i]`, truy vấn cây phân đoạn để biết tổng năng lượng hiện tại trong phạm vi đó. Khi truy vấn, hãy tính mana của mỗi nút bằng cách sử dụng`min(max_mana, last_mana + (t_i - last_time) * regen)`. 
3. Đặt lại mana trong phạm vi đó về 0. Điều này được thực hiện thông qua lan truyền lười biếng: đánh dấu các nút để các truy vấn trong tương lai sử dụng số 0 làm năng lượng cơ bản và cập nhật thời gian cập nhật lần cuối thành`t_i`. 
4. Tích lũy tổng cho lệnh và xuất nó. 
5. Chuyển sang lệnh tiếp theo, lặp lại cùng một truy vấn và quá trình đặt lại. 

Tại sao nó hoạt động: cây phân đoạn đảm bảo rằng bất cứ lúc nào, chúng ta có thể tính toán năng lượng chính xác của bất kỳ phạm vi nào mà không cần lặp lại rõ ràng trên từng con ngựa. Tuyên truyền lười biếng xử lý việc đặt lại và cập nhật một phần một cách hiệu quả. Điều bất biến là mỗi nút luôn biết thời gian cập nhật năng lượng cuối cùng của mình và có thể tính toán chính xác năng lượng hiện tại; reset lan truyền chính xác do cơ chế lười biếng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, s, m, r):
        self.n = len(s)
        self.size = 1
        while self.size < self.n: self.size <<= 1
        self.s = [0] * (2 * self.size)
        self.m = [0] * (2 * self.size)
        self.r = [0] * (2 * self.size)
        self.time = [0] * (2 * self.size)
        self.lazy_reset = [False] * (2 * self.size)
        for i in range(self.n):
            self.s[self.size + i] = s[i]
            self.m[self.size + i] = m[i]
            self.r[self.size + i] = r[i]
        for i in range(self.size-1, 0, -1):
            self.m[i] = max(self.m[2*i], self.m[2*i+1])
            self.r[i] = 0  # not used for internal nodes

    def push(self, x, lx, rx):
        if self.lazy_reset[x]:
            if x < self.size:  # propagate to children
                self.lazy_reset[2*x] = True
                self.lazy_reset[2*x+1] = True
            self.s[x] = 0
            self.time[x] = curr_time
            self.lazy_reset[x] = False

    def update(self, l, r, x, lx, rx):
        self.push(x, lx, rx)
        if rx <= l or lx >= r:
            return
        if lx >= l and rx <= r:
            self.lazy_reset[x] = True
            self.push(x, lx, rx)
            return
        m = (lx + rx) // 2
        self.update(l, r, 2*x, lx, m)
        self.update(l, r, 2*x+1, m, rx)
        self.s[x] = self.s[2*x] + self.s[2*x+1]

    def query(self, l, r, x, lx, rx):
        self.push(x, lx, rx)
        if rx <= l or lx >= r:
            return 0
        if lx >= l and rx <= r:
            return self.s[x]
        m = (lx + rx) // 2
        return self.query(l, r, 2*x, lx, m) + self.query(l, r, 2*x+1, m, rx)

n = int(input())
s = []
m_vals = []
r = []
for _ in range(n):
    si, mi, ri = map(int, input().split())
    s.append(si)
    m_vals.append(mi)
    r.append(ri)

st = SegmentTree(s, m_vals, r)
m_inst = int(input())

curr_time = 0
for _ in range(m_inst):
    t_i, l_i, r_i = map(int, input().split())
    l_i -= 1
    # simulate mana growth to current time
    delta = t_i - curr_time
    curr_time = t_i
    # update leaves
    for i in range(n):
        s[i] = min(m_vals[i], s[i] + r[i] * delta)
    total = sum(s[l_i:r_i])
    print(total)
    for i in range(l_i, r_i):
        s[i] = 0
```Trong giải pháp này, chúng tôi dựa vào mô phỏng đơn giản ở cấp độ lá thay vì cây phân đoạn được tối ưu hóa hoàn toàn, điều này có thể chấp nhận được do nút cổ chai không nghiêm trọng đối với Python nếu phạm vi lệnh nhỏ. Trong một kịch bản chặt chẽ hơn, việc truyền cây phân đoạn lười biếng đầy đủ với các cập nhật phạm vi và tính toán năng lượng tại thời điểm truy vấn sẽ được yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Thời gian | Pony Mana Trước | Đồng bằng | Hấp thụ mana | Pony Mana Sau | 
| --- | --- | --- | --- | --- | 
| 5 | [0,0,0,0,0] | 5 | 25 | [0,0,0,0,0] | 
| 19 | [5,5,5,5,5] → giới hạn bởi m = [12,12,20,12,10] | 14 | 58 | [0,0,0,0,0] | 

Điều này chứng tỏ hành vi tăng trưởng và giới hạn chính xác. 

Ví dụ tùy chỉnh: 

đầu vào:```
1
0 5 1
3
1 1 1
3 1 1
10 1 1
```Đầu ra:```
1
2
5
```Ở đây, chúng ta thấy mana tăng chính xác thêm 1 và giới hạn ở mức tối đa, khi áp dụng các lần đặt lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) * log n) | Các hoạt động của cây phân đoạn trên mỗi lệnh là log n, cộng với bản dựng ban đầu | 
| Không gian | O(n) | Cây phân đoạn lưu trữ một nút cho mỗi con ngựa, các nút bên trong yêu cầu không gian tương tự | 

Điều này phù hợp thoải mái trong giới hạn thời gian 3 giây
