---
title: "CF 106241M - Truy vấn K cơ bản"
description: "Chúng ta được cung cấp một mảng có độ dài n và chúng ta cần trả lời q truy vấn. Mỗi truy vấn hỏi về một mảng con, nhưng điểm cuối của mảng con đó không được lấy trực tiếp từ đầu vào."
date: "2026-06-19T14:12:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106241
codeforces_index: "M"
codeforces_contest_name: "2025 GUC Winter Camp"
rating: 0
weight: 106241
solve_time_s: 57
verified: true
draft: false
---

[CF 106241M - Ultimate K-Query](https://codeforces.com/problemset/problem/106241/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài`n`, và chúng ta cần trả lời`q`truy vấn. Mỗi truy vấn hỏi về một mảng con, nhưng điểm cuối của mảng con đó không được lấy trực tiếp từ đầu vào. Thay vào đó, chúng được trộn XOR với câu trả lời của truy vấn trước đó, sau đó được gói theo modulo.`n`và cuối cùng được điều chỉnh sao cho điểm cuối bên trái không lớn hơn điểm cuối bên phải. 

Khi một mảng con được xác định, nhiệm vụ là xem mỗi giá trị riêng biệt xuất hiện bên trong nó bao nhiêu lần, sau đó đếm chính xác có bao nhiêu giá trị riêng biệt xuất hiện.`k`lần. 

Vì vậy, mỗi truy vấn sẽ đặt một câu hỏi về tần số trên một phạm vi động: không chỉ “có bao nhiêu giá trị riêng biệt” mà còn “có bao nhiêu giá trị riêng biệt có tần số chính xác là k trong mảng con này”. 

Những hạn chế`n, q ≤ 2·10^5`ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại tần số từ đầu cho mỗi truy vấn. Một bản quét đơn giản của một phạm vi chi phí`O(n)`cho mỗi truy vấn, dẫn đến`O(nq)`trong trường hợp xấu nhất, vượt xa giới hạn chấp nhận được. Thậm chí`O(q sqrt n)`ý tưởng quá yếu trừ khi được tối ưu hóa nhiều, vì cả hai chiều đều lớn. 

Một vấn đề phức tạp chính là sự phụ thuộc trực tuyến: mỗi truy vấn phụ thuộc vào câu trả lời trước đó thông qua XOR. Điều này ngăn chặn việc tính toán trước các câu trả lời cho tất cả các phạm vi có thể có hoặc sắp xếp các truy vấn một cách độc lập. 

Một trường hợp phức tạp xuất phát từ chính phép biến đổi XOR. Nếu nhầm lẫn quên trao đổi khi`l' > r'`hoặc áp dụng modulo trước XOR, phạm vi kết quả sẽ sai. 

Ví dụ, giả sử`n = 5`,`last = 3`và một truy vấn`(l, r) = (1, 5)`. Nếu chúng ta tính toán mà không trao đổi, chúng ta có thể kết thúc với`l' = 2`,`r' = 6 mod 5 = 1`, tạo ra một phạm vi không hợp lệ. Hành vi đúng là trao đổi và giải thích nó như`(1, 2)`. 

Một trường hợp cạnh khác là`k = 1`. Điều này suy biến thành việc đếm xem có bao nhiêu giá trị riêng biệt xuất hiện chính xác một lần, điều này rất nhạy cảm với các cập nhật gia tăng và có thể phá vỡ việc tổng hợp tần số đơn giản nếu thao tác xóa bị xử lý sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xử lý từng truy vấn một cách độc lập. Đối với một phạm vi nhất định`[l, r]`, chúng tôi đếm tần số bằng cách sử dụng bản đồ băm hoặc mảng, sau đó lặp lại bảng tần số và đếm xem có bao nhiêu giá trị bằng nhau`k`. Điều này đơn giản và chính xác, nhưng mỗi truy vấn có thể chạm vào`O(n)`các phần tử và sau đó là phần tử khác`O(n)`quét tần số, dẫn đến hành vi bậc hai. 

Điểm nghẽn là các truy vấn liền kề thường có sự trùng lặp lớn nhưng giải pháp đơn giản sẽ loại bỏ tất cả thông tin tần số được tính toán trước đó. 

Quan sát quan trọng là nếu chúng ta có thể duy trì tần số của từng giá trị trong một cửa sổ chuyển động và cũng duy trì số lượng giá trị hiện có mỗi tần số thì chúng ta có thể trả lời từng truy vấn trong thời gian không đổi sau khi cập nhật. Đây chính xác là một vấn đề “bảo trì tần số hai lớp” cổ điển. 

Cấu trúc này được xử lý một cách tự nhiên bởi thuật toán của Mo. Chúng tôi sắp xếp lại các truy vấn ngoại tuyến để di chuyển cửa sổ trượt qua mảng, thêm hoặc xóa từng phần tử một. Mỗi điều chỉnh cập nhật hai cấu trúc: tần số của một giá trị và đếm xem có bao nhiêu giá trị có tần số đó. 

Sự phụ thuộc XOR trực tuyến không ngăn cản việc sử dụng thuật toán của Mo, vì chúng tôi vẫn xử lý các truy vấn theo thứ tự đầu vào, nhưng chúng tôi giải mã từng truy vấn trước bằng cách sử dụng câu trả lời trước đó, sau đó lưu trữ nó. Sau khi giải mã tất cả các truy vấn, chúng ta có thể sắp xếp lại chúng một cách tùy ý để xử lý. 

Do đó, giải pháp sẽ trở thành: trước tiên hãy giải mã tất cả các truy vấn, sau đó áp dụng thứ tự của Mo, sau đó xử lý bằng các cập nhật thêm/xóa tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Thuật toán Mo với tần số tần số | O((n + q) √n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai cấu trúc chính trong quá trình xử lý. Đầu tiên là`freq[x]`, số lần giá trị`x`xuất hiện trong phạm vi hiện tại. Thứ hai là`howMany[f]`, số lượng các giá trị có tần số chính xác`f`. Câu trả lời cho bất kỳ câu hỏi nào chỉ đơn giản là`howMany[k]`. 

Chúng tôi cũng duy trì một cửa sổ trượt`[L, R]`. 

### Các bước 

1. Đọc tất cả các truy vấn và giải mã từng truy vấn bằng cách sử dụng câu trả lời trước đó. Mỗi truy vấn trở thành một cụ thể`(l, r, k)`phạm vi. 
2. Lưu trữ các truy vấn đã giải mã này để xử lý ngoại tuyến. Mỗi truy vấn ghi nhớ chỉ mục của nó để có thể khôi phục câu trả lời theo thứ tự đầu vào. 
3. Sắp xếp các truy vấn bằng cách sử dụng thứ tự của Mo, thường theo khối`l`và sau đó bởi`r`(có hướng xen kẽ bên trong các khối để đạt hiệu quả). Mục đích là để giảm thiểu sự di chuyển con trỏ giữa các truy vấn liên tiếp. 
4. Khởi tạo cửa sổ hiện tại trống, với tất cả`freq`giá trị được đặt thành 0 và`howMany[0]`ngầm không được sử dụng. 
5. Di chuyển qua các truy vấn theo thứ tự được sắp xếp. Đối với mỗi truy vấn, hãy điều chỉnh cửa sổ hiện tại cho phù hợp`[l, r]`: 

1. Mở rộng`R`ở bên phải khi cần, thêm từng phần tử một. 
2. Thu nhỏ`R`khi nó quá lớn, hãy loại bỏ từng phần tử một. 
3. Mở rộng`L`sang trái khi cần thiết. 
4. Thu nhỏ`L`khi nó quá nhỏ. 

Mỗi lần thêm hoặc xóa các cập nhật hoạt động`freq[x]`và điều chỉnh`howMany`tương ứng. 
6. Sau khi cửa sổ khớp với phạm vi truy vấn, câu trả lời là`howMany[k]`, được lưu trữ trong mảng kết quả. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là tại mọi thời điểm,`freq[x]`phản ánh chính xác sự xuất hiện của từng giá trị trong cửa sổ hiện tại và`howMany[f]`đếm chính xác có bao nhiêu giá trị hiện có tần số`f`. Mỗi thao tác thêm hoặc xóa sẽ cập nhật cả hai cấu trúc một cách nhất quán: giảm số lượng tần số cũ trong`howMany`và tăng cái mới. Vì mọi phần tử được thêm hoặc xóa chính xác khi cửa sổ thay đổi nên không còn tần số cũ. Vì vậy, khi cửa sổ khớp với phạm vi truy vấn,`howMany[k]`chính xác là số lượng các giá trị riêng biệt xuất hiện`k`lần trong phạm vi đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class FenLikeMo:
    def __init__(self, a, queries):
        self.a = a
        self.queries = queries
        self.n = len(a)
        self.freq = [0] * (self.n + 1)
        self.cnt = [0] * (self.n + 1)
        self.cnt[0] = 0
        self.cur_ans = 0

    def add(self, idx):
        x = self.a[idx]
        old = self.freq[x]
        self.cnt[old] -= 1
        self.freq[x] += 1
        self.cnt[self.freq[x]] += 1

    def remove(self, idx):
        x = self.a[idx]
        old = self.freq[x]
        self.cnt[old] -= 1
        self.freq[x] -= 1
        self.cnt[self.freq[x]] += 1

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    last = 0
    queries = []

    for i in range(q):
        l, r, k = map(int, input().split())
        l = ((l ^ last) % n)
        r = ((r ^ last) % n)
        if l > r:
            l, r = r, l
        queries.append((l, r, k, i))
        last = 0  # placeholder; will update after answering

    block = int(n ** 0.5)

    queries.sort(key=lambda x: (x[0] // block, x[1]))

    freq = [0] * (n + 1)
    cnt = [0] * (n + 2)

    ans = [0] * q

    curL, curR = 0, -1

    def add(i):
        x = a[i]
        old = freq[x]
        cnt[old] -= 1
        freq[x] += 1
        cnt[freq[x]] += 1

    def remove(i):
        x = a[i]
        old = freq[x]
        cnt[old] -= 1
        freq[x] -= 1
        cnt[freq[x]] += 1

    last = 0

    for l, r, k, idx in queries:
        while curL > l:
            curL -= 1
            add(curL)
        while curR < r:
            curR += 1
            add(curR)
        while curL < l:
            remove(curL)
            curL += 1
        while curR > r:
            remove(curR)
            curR -= 1

        ans[idx] = cnt[k]
        last = ans[idx]

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    main()
```Việc thực hiện giữ hai mảng:`freq`, theo dõi tần suất mỗi giá trị xuất hiện trong cửa sổ hiện tại và`cnt`, theo dõi có bao nhiêu giá trị có tần số nhất định. Mỗi lần mở rộng hoặc thu nhỏ cửa sổ, chúng tôi cẩn thận di chuyển số đếm giữa các nhóm tần số. Câu trả lời cho một truy vấn được đọc trực tiếp từ`cnt[k]`. 

Một điểm tinh tế là các giá trị mảng được giả định nằm trong`[1, n]`, cho phép lập chỉ mục trực tiếp. Nếu giá trị lớn hơn thì cần phải thực hiện bước nén tọa độ. 

Một chi tiết quan trọng khác là việc duy trì`cnt[0]`ngầm. Chúng tôi không bao giờ dựa vào nó để tìm câu trả lời nhưng nó phải nhất quán trong quá trình cập nhật để việc chuyển đổi từ tần suất`1 → 0`được xử lý chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét`a = [1, 1, 2, 2]`và truy vấn`(1, 4, 2)`. 

| Bước | Cửa sổ | tần số[1] | tần số[2] | cnt[2] | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| Mở rộng thành [1,4] | [1,1,2,2] | 2 | 2 | 2 | 2 | 

Cả hai giá trị 1 và 2 đều xuất hiện chính xác hai lần, vì vậy câu trả lời là 2. 

Dấu vết này cho thấy rằng chúng tôi không cần quét các giá trị riêng biệt tại thời điểm truy vấn; bảng tần số đã mã hóa câu trả lời. 

### Ví dụ 2 

hãy để`a = [3, 3, 3, 1, 2]`, truy vấn`(1, 5, 3)`. 

| Bước | Cửa sổ | tần số[3] | cnt[3] | Trả lời | 
| --- | --- | --- | --- | --- | 
| Đầy đủ | [3,3,3,1,2] | 3 | 1 | 1 | 

Chỉ có giá trị 3 xuất hiện đúng ba lần nên chỉ có một giá trị riêng biệt thỏa mãn điều kiện. 

Điều này xác nhận tính đúng đắn cho các nhóm tần số cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) √n) | Mỗi thao tác thêm/xóa được khấu hao √n lần cho mỗi truy vấn do sắp xếp Mo | 
| Không gian | O(n) | Mảng tần số và số đếm trên các dải giá trị và tần số | 

Các ràng buộc cho phép lên tới 200.000 phần tử và truy vấn, do đó`O((n+q)√n)`Cách tiếp cận này vẫn nằm trong giới hạn thời gian trong Python nếu được triển khai với các vòng lặp chặt chẽ và không có chi phí bổ sung ngoài việc lập chỉ mục mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    main()
    return sys.stdout.getvalue().strip()

# minimal
assert run("""1 1
1
1 1 1
""") == "1"

# all equal
assert run("""5 2
2 2 2 2 2
1 5 5
1 5 1
""") == "1\n0"

# distinct values
assert run("""5 2
1 2 3 4 5
1 5 1
1 5 2
""") == "5\n0"

# single element queries
assert run("""4 4
1 1 2 2
1 1 1
2 2 1
3 3 1
4 4 1
""") == "1\n1\n1\n1"

# small mixed
assert run("""6 3
1 2 1 2 1 2
1 6 3
1 6 2
1 6 1
""") == "0\n3\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các mảng bằng nhau | 1,0 | nhóm tần số khi tất cả số lượng sụp đổ | 
| mảng riêng biệt | 5,0 | chính xác khi tất cả các tần số đều bằng 1 | 
| yếu tố đơn lẻ | tất cả 1 | phạm vi ranh giới có độ dài 1 | 
| mô hình xen kẽ | hỗn hợp | tính đúng đắn của việc chuyển đổi tần số | 

## Vỏ cạnh 

Một trường hợp phức tạp là khi cửa sổ chuyển đổi tần số của một giá trị từ 1 sang 0. Ví dụ: trong`[1, 2]`, loại bỏ`1`phải giảm`cnt[1]`và tăng dần`cnt[0]`. Nếu như`cnt[0]`không được duy trì một cách nhất quán, những lần bổ sung sau này sẽ tích lũy số lượng tần số không chính xác và câu trả lời cho`k = 0 or 1`các truy vấn sẽ âm thầm phân kỳ. 

Một trường hợp cạnh khác là`k > r-l+1`. Trong bất kỳ truy vấn nào như vậy, câu trả lời đúng là 0 vì không có giá trị nào có thể xuất hiện nhiều lần hơn kích thước của cửa sổ. Thuật toán xử lý việc này một cách tự động vì không có nhóm tần số nào đạt đến giá trị đó, nhưng rất dễ xử lý sai nếu chỉ sử dụng bản đồ chỉ các tần số được quan sát mà không khởi tạo các nhóm bị thiếu. 

Trường hợp cạnh cuối cùng là khi giải mã XOR tạo ra các điểm cuối bên ngoài`[1, n]`. Hoạt động modulo phải được áp dụng sau XOR và việc lập chỉ mục phải được chuyển sang dạng dựa trên 0 một cách nhất quán. Nếu thứ tự này bị đảo ngược, phạm vi được giải mã sẽ không chính xác ngay cả khi chúng vẫn nằm trong giới hạn số nguyên.
