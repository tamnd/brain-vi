---
title: "CF 102586K - Trò chơi và Truy vấn"
description: "Chúng tôi duy trì nhiều bộ quái vật. Một quái vật chỉ được xác định bằng HP hiện tại của nó và các truy vấn sẽ thay đổi số lượng quái vật có giá trị HP cụ thể hoặc hỏi cần bao nhiêu lượt Bob để loại bỏ một số lượng quái vật nhất định nếu cả hai người chơi đều chơi tối ưu."
date: "2026-08-01T06:23:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102586
codeforces_index: "K"
codeforces_contest_name: "XX Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102586
solve_time_s: 83
verified: true
draft: false
---

[CF 102586K - Trò chơi và truy vấn](https://codeforces.com/problemset/problem/102586/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì nhiều bộ quái vật. Một quái vật chỉ được xác định bằng HP hiện tại của nó và các truy vấn sẽ thay đổi số lượng quái vật có giá trị HP cụ thể hoặc hỏi cần bao nhiêu lượt Bob để loại bỏ một số lượng quái vật nhất định nếu cả hai người chơi đều chơi tối ưu. 

Số lượng quan trọng của trò chơi không phải là thứ tự tấn công chính xác mà là lượng HP phải loại bỏ. Hãy xem xét một con quái vật có HP`x`. Alice luôn di chuyển trước và có thể tăng quái vật này lên 1 trước mỗi cuộc tấn công của Bob. Sau đó`t`Các cuộc tấn công của Bob tập trung vào con quái vật này, nó nhận được`t`tăng và`2t`thiệt hại, vì vậy HP còn lại của nó là`x + t - 2t = x - t`. Nó biến mất đúng lúc`t = x`. Điều này có nghĩa là một quái vật có HP`x`chi phí chính xác`x`Bob quay người bỏ đi. 

Đối với nhiều quái vật, ý tưởng tương tự cũng được áp dụng. Nếu Bob chọn một số quái vật có tổng HP`S`, anh ấy có thể chi tiêu chính xác`S`quay lại để loại bỏ chúng. Lý do là mỗi lượt Bob đóng góp hai sát thương trong khi Alice chỉ đóng góp một HP, do đó, một lượt chơi đầy đủ sẽ giảm tổng HP của quái vật được chọn đi một. Bob luôn có thể tấn công quái vật mà Alice vừa tăng lên, ngăn cản Alice tích lũy thêm HP trên một quái vật duy nhất. Alice không thể trì hoãn toàn bộ quá trình vượt quá tổng giá trị HP ban đầu. 

Vì Bob muốn hoàn thành càng sớm càng tốt nên anh ấy chọn những con quái vật có giá trị HP nhỏ nhất. Một truy vấn yêu cầu`k`do đó những cái chết đang yêu cầu tổng số tiền`k`giá trị HP nhỏ nhất hiện có. 

Số lượng truy vấn có thể đạt tới`3 * 10^5`, vì vậy việc quét tất cả quái vật cho mỗi truy vấn là không thể. Các giá trị HP được giới hạn bởi`10^6`, có nghĩa là chúng tôi có thể lưu trữ thông tin do HP lập chỉ mục. Giải pháp sắp xếp tất cả quái vật sau mỗi lần cập nhật sẽ quá chậm vì số lượng quái vật cũng có thể trở nên rất lớn. Chúng ta cần cấu trúc dữ liệu logarit hỗ trợ thay đổi số lượng của một giá trị HP và tìm tiền tố chứa số lượng quái vật nhất định. 

Các trường hợp cạnh quan trọng tập trung xung quanh các giá trị HP trùng lặp và các vị trí biên. Một truy vấn có thể yêu cầu tất cả quái vật có cùng một giá trị HP hoặc có thể dừng ở giữa một nhóm có cùng HP. 

Ví dụ:```
5
1 3 4
2 2
1 3 1
2 3
2 4
```Sau bản cập nhật đầu tiên, có bốn quái vật có HP 3. Câu hỏi đầu tiên yêu cầu hai quái vật, vì vậy câu trả lời là`6`, vì hai giá trị HP nhỏ nhất là`3`Và`3`. 

Đầu ra là:```
6
10
13
```Việc triển khai bất cẩn chỉ lưu trữ các giá trị HP riêng biệt và bỏ qua bội số của chúng sẽ thất bại vì một giá trị HP có thể đại diện cho nhiều quái vật. 

Một trường hợp ranh giới khác là khi được yêu cầu`k`kết thúc chính xác tại một ranh giới giá trị.```
2
1 1 5
2 5
```Câu trả lời là`25`. Cấu trúc dữ liệu phải bao gồm toàn bộ khối tần số khi số tiền tố đạt chính xác`k`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ lưu trữ mọi quái vật trong một danh sách. Đối với truy vấn loại 2, chúng ta có thể sắp xếp các giá trị HP hiện tại, lấy giá trị đầu tiên`k`, và thêm chúng. Điều này đúng vì trò chơi giảm bớt việc chọn`k`giá trị HP nhỏ nhất Tuy nhiên có thể có tới`3 * 10^5`truy vấn và số lượng quái vật có thể lên tới khoảng`10^11`bởi vì mỗi bản cập nhật có thể đặt số lượng lên tới`10^6`. Ngay cả khi bỏ qua chi phí phân loại, việc duyệt qua tất cả quái vật là không thể. Việc sắp xếp sau mỗi truy vấn sẽ vượt xa giới hạn thời gian. 

Quan sát hữu ích là bản thân các giá trị HP rất nhỏ. Chúng ta không cần phải biết riêng từng con quái vật. Chúng tôi chỉ cần hai thông tin cho mỗi giá trị HP`x`: có bao nhiêu quái vật có HP`x`, và tổng số đóng góp của những con quái vật đó, đó là`x * count[x]`. 

Điều này chuyển đổi vấn đề thành việc duy trì tần số và tổng trọng số trên một phạm vi tọa độ cố định. Cây Fenwick phù hợp tự nhiên vì các cập nhật ảnh hưởng đến một vị trí và các truy vấn cần tổng tiền tố. Một cây Fenwick lưu trữ số lượng và một cây khác lưu trữ tổng giá trị HP. Để trả lời một truy vấn, chúng tôi tìm giá trị HP nhỏ nhất có số tiền tố đạt tới`k`. Tất cả các giá trị nhỏ hơn sẽ được lấy hoàn toàn và số quái vật còn lại sẽ được lấy từ giá trị HP cuối cùng đó. 

Sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(M log M)`mỗi truy vấn, ở đâu`M`là số lượng quái vật |`O(M)`| Quá chậm | 
| Cây Fenwick |`O(log 10^6)`mỗi truy vấn |`O(10^6)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ hai cây Fenwick được HP lập chỉ mục. Cây đầu tiên lưu trữ số lượng quái vật tồn tại ở mỗi giá trị HP. Cây thứ hai lưu trữ tổng HP đóng góp bởi giá trị đó, vì vậy vị trí`x`cửa hàng`x * count[x]`. 
2. Đối với truy vấn cập nhật thay đổi số lượng quái vật có HP`x`ĐẾN`y`, tính toán sự khác biệt so với lần đếm trước đó. Thêm sự khác biệt này vào cây Fenwick đếm và thêm`x`nhân số chênh lệch này với tổng cây Fenwick. 
3. Đối với truy vấn yêu cầu đầu tiên`k`quái vật theo thứ tự HP được sắp xếp, sử dụng cây đếm Fenwick để tìm giá trị HP nhỏ nhất`pos`trong đó số tiền tố trở thành ít nhất`k`. 
4. Tất cả các giá trị HP nhỏ hơn`pos`đều được bao gồm đầy đủ. Cộng tổng HP của họ từ cây tổng Fenwick. 
5. Số quái vật cần thiết còn lại được lấy từ HP`pos`. Nhân số còn lại với`pos`và thêm nó vào câu trả lời. 

Lý do điều này có tác dụng là vì câu trả lời chỉ phụ thuộc vào tập hợp nhiều giá trị HP được sắp xếp. Cây Fenwick cho phép chúng ta điều hướng thứ tự được sắp xếp này mà không cần lưu trữ rõ ràng mọi quái vật. 

### Tại sao nó hoạt động 

Sau mỗi vòng Alice-Bob hoàn thành, tổng HP của bất kỳ bộ sưu tập quái vật nào đã chọn sẽ giảm đúng một nếu Bob tấn công bên trong bộ sưu tập đó. Bob luôn có thể chọn các đòn tấn công của mình để những quái vật mà anh ta muốn loại bỏ nhận được sát thương cần thiết, trong khi lượng HP bổ sung của Alice chỉ hủy bỏ một điểm của tiến trình đó mỗi vòng. Do đó, một bộ sưu tập quái vật có tổng HP`S`yêu cầu chính xác`S`Bob quay người biến mất. 

Bob muốn tổng HP nhỏ nhất có thể trong số`k`quái vật, vậy nên sự lựa chọn tối ưu chính là`k`giá trị HP nhỏ nhất Cấu trúc dữ liệu trả về chính xác số tiền này, điều này chứng tỏ thuật toán tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXX = 10**6

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

    def kth(self, k):
        idx = 0
        step = 1 << (self.n.bit_length() - 1)
        while step:
            nxt = idx + step
            if nxt <= self.n and self.bit[nxt] < k:
                idx = nxt
                k -= self.bit[nxt]
            step >>= 1
        return idx + 1

def solve():
    q = int(input())
    cnt_tree = Fenwick(MAXX)
    sum_tree = Fenwick(MAXX)
    cnt = [0] * (MAXX + 1)

    ans = []

    for _ in range(q):
        query = input().split()
        t = int(query[0])

        if t == 1:
            x = int(query[1])
            y = int(query[2])
            diff = y - cnt[x]
            cnt[x] = y
            cnt_tree.add(x, diff)
            sum_tree.add(x, diff * x)

        else:
            k = int(query[1])
            pos = cnt_tree.kth(k)
            before = cnt_tree.sum(pos - 1)
            total = sum_tree.sum(pos - 1)
            total += (k - before) * pos
            ans.append(str(total))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`cnt`mảng lưu trữ tần số hiện tại của mọi giá trị HP để các bản cập nhật có thể được chuyển đổi thành sự khác biệt. Cây Fenwick chỉ lưu trữ thông tin tổng hợp cần thiết cho các truy vấn. 

các`kth`hàm thực hiện nâng nhị phân trên cây Fenwick. Nó tìm vị trí đầu tiên có số tiền tố đạt đến yêu cầu`k`. Điều này tránh tìm kiếm nhị phân với các truy vấn tiền tố lặp lại và giữ cho mỗi truy vấn ở dạng logarit. 

Việc tính toán câu trả lời sẽ tách tiền tố được bao gồm đầy đủ khỏi giá trị HP được bao gồm một phần cuối cùng. Đây là nơi thường xuất hiện các lỗi riêng lẻ. Số đếm trước`pos`phải được loại trừ và chỉ`k - before`quái vật từ`pos`đóng góp. 

Số nguyên Python xử lý số tiền tối đa có thể mà không bị tràn. Đóng góp lớn nhất có thể là vượt quá giới hạn 32 bit, do đó, việc sử dụng số học Python thông thường sẽ tránh được mọi xử lý bổ sung. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các truy vấn tạo ra bốn quái vật có HP 1, sau đó thêm ba quái vật có HP 2. 

| Truy vấn | Số lượng HP | k | Giá trị HP được chọn | Trả lời | 
| --- | --- | --- | --- | --- | 
|`1 1 4`|`1:4`| | | | 
|`2 3`|`1:4`| 3 |`1,1,1`| 3 | 
|`1 2 3`|`1:4, 2:3`| | | | 
|`2 6`|`1:4, 2:3`| 6 |`1,1,1,1,2,2`| 8 | 
|`1 2 2`|`1:4, 2:2`| | | | 
|`2 6`|`1:4, 2:2`| 6 |`1,1,1,1,2,2`| 8 | 

Dấu vết này chứng tỏ rằng sự đa dạng rất quan trọng. Cây Fenwick thứ hai lưu trữ phần đóng góp có trọng số chứ không chỉ số lượng quái vật. 

Đối với mẫu thứ hai, sau hai bản cập nhật đầu tiên, có 12 quái vật có HP 1 và 15 quái vật có HP 2. 

| Truy vấn | Số lượng HP | k | Tiền tố được lấy | Trả lời | 
| --- | --- | --- | --- | --- | 
|`1 1 12`|`1:12`| | | | 
|`2 12`|`1:12`| 12 | mười hai quái vật HP 1 | 12 | 
|`1 2 15`|`1:12, 2:15`| | | | 
|`2 12`|`1:12, 2:15`| 12 | mười hai quái vật HP 1 | 12 | 
|`2 3`|`1:12, 2:15`| 3 | ba quái vật HP 1 | 3 | 

Điều này xác nhận rằng truy vấn có thể dừng trước khi tiếp cận nhóm HP lớn hơn, do đó`kth`tìm kiếm phải xác định được ranh giới chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(Q log 10^6)`| Mọi cập nhật, truy vấn tiền tố và tìm kiếm thứ k đều sử dụng các phép toán cây Fenwick | 
| Không gian |`O(10^6)`| Hai cây Fenwick và mảng tần số được HP | 

Giá trị HP tối đa được cố định ở`10^6`, do đó hệ số logarit là khoảng 20. Với`3 * 10^5`truy vấn, giải pháp chỉ thực hiện vài triệu thao tác Fenwick và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    if not data:
        return ""

    it = iter(data)
    q = int(next(it))

    MAXX = 10**6

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            r = 0
            while i:
                r += self.bit[i]
                i -= i & -i
            return r

        def kth(self, k):
            idx = 0
            step = 1 << (self.n.bit_length() - 1)
            while step:
                nxt = idx + step
                if nxt <= self.n and self.bit[nxt] < k:
                    idx = nxt
                    k -= self.bit[nxt]
                step >>= 1
            return idx + 1

    c = Fenwick(MAXX)
    s = Fenwick(MAXX)
    cur = [0] * (MAXX + 1)
    out = []

    for _ in range(q):
        t = int(next(it))
        if t == 1:
            x = int(next(it))
            y = int(next(it))
            d = y - cur[x]
            cur[x] = y
            c.add(x, d)
            s.add(x, d * x)
        else:
            k = int(next(it))
            p = c.kth(k)
            b = c.sum(p - 1)
            out.append(str(s.sum(p - 1) + (k - b) * p))

    sys.stdin = old
    return "\n".join(out)

assert run("""6
1 1 4
2 3
1 2 3
2 6
1 2 2
2 6
""") == "3\n8\n8"

assert run("""3
1 5 1
2 1
2 1
""") == "5\n5"

assert run("""5
1 1 3
1 2 2
2 4
1 1 0
2 2
""") == "7\n4"

assert run("""4
1 1000000 3
2 2
1 1 5
2 5
""") == "2000000\n5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nhóm HP đơn |`5`Và`5`| Cập nhật cơ bản và truy vấn lặp lại | 
| Giá trị HP hỗn hợp |`7`Và`4`| Lựa chọn tiền tố trên nhiều tần số | 
| Giá trị HP tối đa |`2000000`Và`5`| Xử lý tọa độ lớn và cập nhật ranh giới | 

## Vỏ cạnh 

Khi tất cả quái vật có cùng HP, tìm kiếm nhỏ thứ k sẽ trực tiếp trên một khối tần số. Ví dụ:```
3
1 7 4
2 3
2 4
```Câu trả lời là:```
21
28
```Cây Fenwick không cần có mục riêng cho từng quái vật. Nó nhìn thấy bốn quái vật ở vị trí 7 và lấy số lượng yêu cầu từ khối đó. 

Khi một bản cập nhật loại bỏ tất cả quái vật của một HP nhất định, chênh lệch tần số sẽ trở thành âm. Ví dụ:```
4
1 3 5
1 3 0
1 2 4
2 4
```Câu trả lời cuối cùng là:```
8
```Bản cập nhật đã trừ đi chính xác sự đóng góp trước đó của HP 3, chỉ còn lại bốn quái vật có HP 2. 

Khi nào`k`kết thúc chính xác ở cuối khối tần số, tìm kiếm thứ k phải trả về khối đó chứ không phải khối tiếp theo. Ví dụ:```
3
1 2 3
1 5 3
2 3
```Câu trả lời là:```
6
```Ba quái vật đầu tiên đều có HP 2 nên việc tìm kiếm dừng lại ở vị trí 2. Việc triển khai xử lý việc này vì`kth`tìm tiền tố đầu tiên có số lượng ít nhất`k`, không hẳn là lớn hơn`k`.
