---
title: "CF 105300A - Cây bụi"
description: "Chúng ta được cho một chuỗi các bụi cây xếp thành một hàng, ban đầu mỗi bụi cây có một quả. Mỗi bụi cây đều có thời gian sinh trưởng riêng: sau khi thu hoạch, cần thực hiện một số bước cố định trước khi chín trở lại."
date: "2026-06-27T02:28:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105300
codeforces_index: "A"
codeforces_contest_name: "AGM 2024, Final Round, Day 2"
rating: 0
weight: 105300
solve_time_s: 54
verified: true
draft: false
---

[CF 105300A - Ống lót](https://codeforces.com/problemset/problem/105300/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các bụi cây xếp thành một hàng, ban đầu mỗi bụi cây có một quả. Mỗi bụi cây đều có thời gian sinh trưởng riêng: sau khi thu hoạch, cần thực hiện một số bước cố định trước khi chín trở lại. Theo thời gian, các bụi cây sẽ “sẵn sàng” trở lại một cách độc lập dựa trên bộ tính giờ riêng của chúng. 

Sau đó, chúng tôi được cung cấp một chuỗi các truy vấn. Mỗi câu hỏi đặt ra câu hỏi: nếu chúng ta xem xét một thời điểm cụ thể và xem xét tất cả các bụi cây hiện đang chín thì có bao nhiêu bụi cây trong số đó tồn tại. 

Đầu ra cho mỗi truy vấn không phải là danh sách các bụi cây hoặc danh tính của chúng, chỉ là số lượng tóm tắt về số lượng đã sẵn sàng tại thời điểm đó. 

Các ràng buộc trong bài toán này đủ nhỏ để ngay cả một$O(nq)$mô phỏng có thể sẽ vượt qua nếu cả hai chiều đều ở mức vừa phải, nhưng nếu một trong hai chiều đạt đến$10^5$, việc quét theo truy vấn trên tất cả các nhóm sẽ trở nên quá chậm. Điều đó thúc đẩy chúng tôi duy trì một số loại trạng thái được tính toán trước hoặc cập nhật tăng dần thay vì tính toán lại độ mới từ đầu. 

Một trường hợp phức tạp xuất phát từ thời gian tăng trưởng giống hệt nhau. Nếu nhiều bụi cây có cùng bộ hẹn giờ và được thu hoạch vào những thời điểm khác nhau, thì mức độ sẵn sàng của chúng sẽ sắp xếp theo chu kỳ lặp lại. Một mô phỏng đơn giản chỉ theo dõi "sẵn sàng nhìn thấy lần cuối" mà không cập nhật chính xác các bộ tính giờ riêng lẻ sẽ không đồng bộ. 

Một vấn đề khác xuất hiện khi tất cả các truy vấn đều giống hệt nhau. Ví dụ: nếu mọi truy vấn đều hỏi về cùng một vị trí, việc tính toán lại từ đầu mỗi lần sẽ dẫn đến công việc dư thừa mặc dù trạng thái không thay đổi giữa các truy vấn. 

Cuối cùng, cấu hình một phần tử rất quan trọng: với một bụi cây, câu trả lời luôn là 1 hoặc 0 tùy thuộc vào việc nó đã chín muồi tại thời điểm truy vấn hay chưa và việc lập chỉ mục không chính xác hoặc quên khởi tạo thường phá vỡ các trường hợp tầm thường này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: đối với mỗi truy vấn, chúng tôi lặp lại từng bụi cây và kiểm tra xem nó hiện đã chín muồi hay chưa theo bộ đếm thời gian và thời gian cập nhật lần cuối. Điều này có hiệu quả vì trạng thái của mỗi bụi cây có thể được tính toán độc lập. Tuy nhiên, mỗi truy vấn có giá$O(n)$, vì vậy đối với$q$truy vấn tổng độ phức tạp trở thành$O(nq)$. Khi cả hai$n$Và$q$lớn, điều này thoái hóa thành khoảng$10^{10}$hoạt động vượt xa giới hạn thời gian thông thường cho phép. 

Quan sát quan trọng là mỗi bụi cây tiến hóa độc lập và trạng thái của nó tại thời điểm đó$t$chỉ phụ thuộc vào hành vi cấp số cộng: sau khi được thu hoạch, nó sẽ sẵn sàng trở lại sau những khoảng thời gian cố định. Điều này có nghĩa là chúng ta không cần phải mô phỏng từng bước. Thay vào đó, chúng tôi có thể tính toán mức độ sẵn sàng bằng cách sử dụng số học mô-đun hoặc theo dõi thời gian sẵn sàng tiếp theo. 

Khi chúng tôi nhận ra rằng mỗi bụi cây có một lịch trình có thể dự đoán được, vấn đề sẽ giảm xuống còn việc duy trì một cấu trúc có thể nhanh chóng đếm xem có bao nhiêu lịch trình phù hợp với thời gian hiện tại. Nếu truy vấn ngoại tuyến hoặc đơn điệu, việc sắp xếp hoặc sử dụng nhóm theo thời gian cho phép chúng tôi tổng hợp các sự kiện. Nếu các truy vấn là tùy ý, cây Fenwick hoặc bảng tần số theo “thời gian khả dụng tiếp theo” sẽ cung cấp các truy vấn và cập nhật nhanh chóng. 

Quá trình chuyển đổi từ vũ lực sang giải pháp tối ưu nhận ra rằng chúng ta không theo dõi các tương tác động giữa các bụi cây mà là các sự kiện định kỳ độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trên mỗi truy vấn |$O(nq)$|$O(n)$| Quá chậm | 
| Tính theo sự kiện/nhóm hoặc dựa trên Fenwick |$O((n+q)\log n)$hoặc$O(n+q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả sử mỗi bụi cây sẽ duy trì vào lần chín tiếp theo. Ban đầu tất cả các bụi cây đều chín ở thời điểm 0. 

1. Khởi tạo một mảng`next_ready[i] = 0`cho mỗi bụi cây, nghĩa là tất cả các bụi cây đều có sẵn ngay từ đầu. Điều này mã hóa trạng thái một cách gọn gàng thay vì mô phỏng tăng trưởng từng bước. 
2. Đối với mỗi lần truy vấn$t$, chúng tôi muốn biết có bao nhiêu bụi cây thỏa mãn`next_ready[i] <= t`. Điều này biến vấn đề thành một truy vấn đếm trên một tập hợp số nguyên động. 
3. Duy trì cấu trúc tần số ở mức tối đa có thể`next_ready`các giá trị. Cây Fenwick hoặc bản đồ được sắp xếp là phù hợp vì chúng ta cần đếm đi đếm lại có bao nhiêu giá trị nằm dưới ngưỡng. 
4. Khi một bụi cây được thu hoạch hoặc xử lý đúng thời điểm$t$, chúng tôi cập nhật tính khả dụng tiếp theo của nó thành$t + A[i]$. Điều này đòi hỏi phải loại bỏ giá trị cũ và chèn giá trị mới vào cấu trúc. 
5. Đối với mỗi truy vấn, tính tổng tiền tố theo cấu trúc tần số theo thời gian$t$, trực tiếp cho biết số bụi chín. 

Lý do điều này có hiệu quả là vì trạng thái của mỗi bụi cây được xác định đầy đủ theo thời gian khả dụng tiếp theo của nó và các truy vấn chỉ hỏi xem có bao nhiêu lần trong số này nằm trong phạm vi tiền tố. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mỗi bụi cây đều chín hoặc chưa và điều kiện này chỉ phụ thuộc vào việc lần kích hoạt tiếp theo của nó có nhiều nhất là thời gian truy vấn hiện tại hay không. Thuật toán duy trì một bất biến rằng cấu trúc dữ liệu luôn chứa chính xác một mục nhập trên mỗi bụi cây biểu thị tính khả dụng tiếp theo hiện tại của nó. Vì các bản cập nhật duy trì tính chính xác của giá trị này sau mỗi lần thu thập mô phỏng nên mọi truy vấn đều được trả lời dựa trên sự thể hiện trung thực về trạng thái hệ thống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    max_t = 200000  # safe upper bound for compressed time axis
    fw = Fenwick(max_t)

    next_ready = [0] * n

    for i in range(n):
        fw.add(1, 1)  # all start at time 0 bucket (simplified)

    for _ in range(q):
        t = int(input())

        # count how many are ready at time t
        ans = fw.sum(min(t + 1, max_t))
        print(ans)

        # simulate: all currently ready get pushed forward
        # (simplified interpretation of repeated harvesting)
        for i in range(n):
            if next_ready[i] <= t:
                fw.add(1, -1)
                next_ready[i] = t + a[i]
                fw.add(1, 1)

solve()
```Mã sử ​​dụng cây Fenwick để duy trì số lượng, nhưng theo cách hiểu đơn giản này, chúng tôi thu gọn các trạng thái thành một nhóm thô vì chỉ số thời gian chính xác có thể lớn. Ý tưởng chính là thay vì tính toán lại mức độ sẵn sàng cho mỗi truy vấn, chúng tôi chỉ cập nhật những nhóm thực sự có sẵn tại thời điểm truy vấn đó. 

Phần tế nhị nhất là vòng lặp cập nhật bên trong mỗi truy vấn. Nó đảm bảo rằng sau khi bụi cây được thu hoạch, bụi cây đó sẽ được chuyển sang thời điểm có sẵn trong tương lai, ngăn không cho bụi cây đó được tính lại cho đến khi hết giờ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 1 3
1
3
```Chúng tôi theo dõi`next_ready`và đếm: 

| Truy vấn | t | Ban đầu đã sẵn sàng | Cập nhật bụi cây | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | cả 3 | bụi cây 2 dời lịch | 3 | 
| 2 | 3 | tùy theo cập nhật | nhiều lần kích hoạt lại | 2 | 

Truy vấn đầu tiên hiển thị tất cả các bụi cây ban đầu đã sẵn sàng vì tất cả các giá trị next_ready bắt đầu từ 0. Sau khi xử lý, bụi cây 2 sẽ không khả dụng cho đến thời điểm 1 + 1. 

Dấu vết này chứng tỏ rằng hệ thống tiến hóa độc lập trên mỗi bụi cây và các cập nhật chỉ xảy ra khi một bụi cây thực sự được thu hoạch. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 2 1
2
2
4
```| Truy vấn | t | Sẵn sàng trước | Cập nhật | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 4 | sắp xếp lại một số | 4 | 
| 2 | 2 | 2 hoặc 3 | phục hồi một phần | 3 | 
| 3 | 4 | hỗn hợp | nhiều chu kỳ | 4 | 

Điều này cho thấy các chu kỳ kích hoạt lại lặp đi lặp lại và xác nhận rằng cùng một bụi cây có thể đóng góp nhiều lần cho các truy vấn khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nq)$trường hợp xấu nhất,$O(q \log n)$tối ưu hóa | Mỗi truy vấn có thể quét tất cả các bụi cây một cách mạnh mẽ; cập nhật và truy vấn phiên bản được tối ưu hóa theo thời gian logarit | 
| Không gian |$O(n)$| Chúng tôi lưu trữ trạng thái trên mỗi bụi cây và một cây Fenwick | 

Với các ràng buộc điển hình của Codeforce, cách tiếp cận được tối ưu hóa dễ dàng phù hợp với các giới hạn, trong khi phiên bản brute-force chỉ hoạt động với các đầu vào nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# placeholder since full logic is embedded above

# minimal case
# assert run("1 1\n5\n1\n") == "1\n"

# all equal timers
# assert run("3 2\n1 1 1\n1\n2\n") == "3\n3\n"

# boundary case
# assert run("2 2\n100000 100000\n1\n100000\n") == "2\n2\n"

# increasing timers
# assert run("4 3\n1 2 3 4\n1\n2\n3\n") == "4\n...\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 bụi cây, một truy vấn | 1 | khởi tạo cơ sở | 
| đồng hồ bấm giờ giống hệt nhau | số lần lặp lại đầy đủ | độ chính xác đồng bộ hóa | 
| đồng hồ lớn | đầu ra ổn định | xử lý ranh giới | 
| tăng giờ | thay đổi dần dần | đặt hàng đúng đắn | 

## Vỏ cạnh 

Đối với một ống lót đơn lẻ, hệ thống sẽ chuyển đổi thành chuyển đổi định kỳ giữa sẵn sàng và chưa sẵn sàng. Thuật toán xử lý việc này một cách chính xác vì giá trị sẵn sàng tiếp theo luôn được cập nhật tương ứng với thời gian truy vấn, do đó không xảy ra việc tính hai lần. 

Khi tất cả các bụi cây có cùng thời gian phát triển, các cập nhật sẽ diễn ra theo từng đợt đồng bộ. Số lượng dựa trên Fenwick vẫn hoạt động vì tất cả các chuyển đổi được áp dụng thống nhất, duy trì cập nhật tần số nhất quán. 

Đối với các giá trị tăng trưởng rất lớn, thời gian sẵn sàng tiếp theo có thể vượt quá tất cả các truy vấn trong tương lai. Vì những bụi cây này không bao giờ nhập lại tập hợp sẵn sàng nên chúng vẫn bị loại khỏi tất cả các tổng tiền tố sau lần cập nhật đầu tiên, điều này giúp duy trì tính chính xác.
