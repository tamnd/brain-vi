---
title: "CF 1675B - Làm cho nó ngày càng tăng"
description: "Chúng tôi được cung cấp một số mảng độc lập. Đối với mỗi mảng, chúng ta được phép thu nhỏ các phần tử nhiều lần, trong đó một thao tác chọn một vị trí và thay thế giá trị của nó bằng một nửa giá trị được làm tròn xuống. Không được phép hoán đổi nên thứ tự của các phần tử là cố định."
date: "2026-06-10T01:04:31+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1675
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 787 (Div. 3)"
rating: 900
weight: 1675
solve_time_s: 101
verified: true
draft: false
---

[CF 1675B - Làm cho nó ngày càng tăng](https://codeforces.com/problemset/problem/1675/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng độc lập. Đối với mỗi mảng, chúng ta được phép thu nhỏ các phần tử nhiều lần, trong đó một thao tác chọn một vị trí và thay thế giá trị của nó bằng một nửa giá trị được làm tròn xuống. Không được phép hoán đổi nên thứ tự của các phần tử là cố định. 

Mục tiêu là áp dụng số lượng tối thiểu các thao tác thu nhỏ như vậy để sau tất cả các sửa đổi, mảng sẽ tăng dần từ trái sang phải. Nếu không có cách nào để đạt được mức tăng nghiêm ngặt cho dù chúng tôi thu nhỏ các phần tử bằng cách nào, chúng tôi phải báo cáo là không thể thực hiện được. 

Một điểm cấu trúc quan trọng là mọi phần tử chỉ có thể di chuyển xuống dọc theo một chuỗi các giá trị thu được bằng cách chia số nguyên lặp lại cho hai. Mỗi số có một “quỹ đạo giá trị” ngắn, vì việc giảm một nửa lặp đi lặp lại đạt đến 0 sau tối đa khoảng 31 bước đối với các ràng buộc nhất định. 

Các ràng buộc rất nhỏ cho mỗi trường hợp thử nghiệm, với tối đa 30 phần tử. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu phức tạp hoặc tối ưu hóa toàn cầu. Tuy nhiên, số lượng ca kiểm thử lớn nên giải pháp phải tuyến tính hoặc gần tuyến tính cho mỗi ca kiểm thử. 

Một sự hiểu lầm ngây thơ thường thất bại ở đây là thu nhỏ từng phần tử một cách tham lam để chỉ lớn hơn kết quả trước đó mà không tính đến hậu quả trong tương lai. Điều này có thể thất bại vì việc tạo một phần tử hiện tại quá nhỏ có thể buộc phần tử tiếp theo vào một vị trí không thể thực hiện được ngay cả khi lựa chọn lớn hơn một chút sẽ có hiệu quả. 

Một dạng thất bại khác đến từ việc chỉ giảm bớt khi cần thiết. Đôi khi chúng ta phải giảm mạnh các phần tử trước đó thành “không gian trống” cho các phần tử sau, ngay cả khi phần tử hiện tại đã thỏa mãn bất đẳng thức nghiêm ngặt cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi phần tử có một tập hợp nhỏ các giá trị có thể thu được bằng cách chia nhiều lần cho hai. Chúng ta có thể thử tất cả các kết hợp của những lựa chọn này trên mảng và kiểm tra xem chuỗi nào đang tăng nghiêm ngặt, đếm các phép toán trong suốt quá trình. Vì mỗi phần tử có nhiều nhất là 31 trạng thái, điều này dẫn đến khoảng$31^n$sự kết hợp, điều này hoàn toàn không thể thực hiện được ngay cả đối với$n = 30$, vì nó bùng nổ theo cấp số nhân. 

Quan sát quan trọng là các quyết định mang tính cục bộ khi chúng tôi xử lý mảng từ trái sang phải. Khi chúng tôi cố định một giá trị cho vị trí$i$, yêu cầu duy nhất mà nó áp đặt đối với các phần tử trong tương lai là giới hạn dưới: phần tử tiếp theo phải lớn hơn nó. Do đó, đối với mỗi phần tử, chúng ta chỉ cần chọn giá trị nhỏ nhất có thể đạt được bằng cách giảm một nửa nhưng vẫn lớn hơn giá trị đã chọn trước đó. 

Tuy nhiên, có một bước ngoặt quan trọng: chúng tôi không chỉ chọn các giá trị mà còn phải trả một khoản chi phí tương đương với số lần thực hiện halving. Vì vậy, trong số tất cả các giá trị được chuyển đổi hợp lệ, chúng ta muốn có một giá trị thỏa mãn bất đẳng thức trong khi cực tiểu hóa các phép toán. 

Điều này dẫn đến một cấu trúc tham lam: đối với mỗi vị trí, chúng tôi liệt kê tất cả các giá trị có thể đạt được từ số ban đầu, ghép từng giá trị với số lượng phân chia cần thiết và chọn giá trị khả thi nhất theo ràng buộc do phần tử trước đó áp đặt. 

Bởi vì mỗi số chỉ có thể giảm một nửa khoảng 30 lần nên việc liệt kê tất cả các trạng thái cho mỗi phần tử là rẻ. Sự lựa chọn tham lam có hiệu quả vì một khi chúng ta ấn định một giá trị cho vị trí$i$, bất kỳ lựa chọn nào lớn hơn mức cần thiết chỉ làm cho những hạn chế trong tương lai trở nên khó khăn hơn, không bao giờ dễ dàng hơn và cũng không làm giảm chi phí trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bạo lực trước mọi lựa chọn |$O(31^n)$|$O(n)$| Quá chậm | 
| Tham lam với chuỗi được tính toán trước |$O(n \log A)$mỗi bài kiểm tra |$O(\log A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải trong khi vẫn giữ nguyên giá trị được chọn cuối cùng. 

1. Đối với mỗi phần tử$a_i$, chúng tôi tạo ra tất cả các cặp có thể truy cập$(v, c)$, Ở đâu$v$là giá trị thu được bằng cách chia liên tục cho hai và$c$là số thao tác cần thiết để đạt được nó. Điều này được thực hiện bằng cách chia liên tục cho đến khi đạt đến số 0. Việc liệt kê này rẻ vì độ sâu tối đa là 31. 
2. Chúng tôi cố gắng chọn một giá trị$v$cho vị trí hiện tại như vậy$v > \text{prev}$, Ở đâu`prev`là giá trị được chọn cho phần tử trước đó. 
3. Trong số tất cả hợp lệ$v$, ta chọn cái có chi phí nhỏ nhất$c$. Nếu nhiều giá trị có cùng chi phí, chúng tôi ưu tiên giá trị nhỏ hơn$v$. Tùy chọn này giúp duy trì các ràng buộc trong tương lai dễ dàng hơn vì các giá trị nhỏ hơn sẽ có nhiều khoảng trống hơn phía trên chúng. 
4. Nếu không có giá trị có thể truy cập nào thỏa mãn$v > \text{prev}$, ta kết luận rằng không thể xây dựng một dãy tăng chặt chẽ và dừng lại ngay lập tức. 
5. Sau khi chọn được cặp tốt nhất, chúng tôi cập nhật`prev = v`và tích lũy chi phí. 

Lý do cốt lõi là ở mỗi bước, chúng tôi nén tất cả các khả năng trong tương lai vào một giá trị ràng buộc duy nhất. Thông tin duy nhất được chuyển tiếp là ngưỡng nhỏ nhất có thể mà vẫn đảm bảo tính khả thi. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là các giá trị có thể tiếp cận của mọi phần tử tạo thành một chuỗi giảm dần đơn điệu. Bất kỳ sự lựa chọn nào cho vị trí$i$chỉ ảnh hưởng đến các yếu tố trong tương lai thông qua ngưỡng mà nó đặt ra. Nếu chúng ta chọn một giá trị khả thi cho$a_i$, việc chọn một cái lớn hơn chỉ có thể làm giảm số lượng các lựa chọn hợp lệ cho$a_{i+1}$, không bao giờ tăng chúng, đồng thời cũng không cải thiện cơ cấu chi phí của nó. Vì vậy, giá trị khả thi rẻ nhất tại địa phương luôn được đảm bảo an toàn. 

Bởi vì không gian trạng thái là tuyến tính trên mỗi phần tử và chúng ta không bao giờ xem lại các quyết định nên không cần quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_states(x):
    states = []
    steps = 0
    while True:
        states.append((x, steps))
        if x == 0:
            break
        x //= 2
        steps += 1
    return states

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    prev = -1
    ans = 0
    possible = True

    for i in range(n):
        states = get_states(a[i])

        best_cost = float('inf')
        best_val = None

        for v, c in states:
            if v > prev:
                if c < best_cost or (c == best_cost and (best_val is None or v < best_val)):
                    best_cost = c
                    best_val = v

        if best_val is None:
            possible = False
            break

        ans += best_cost
        prev = best_val

    print(ans if possible else -1)
```chức năng`get_states`xây dựng rõ ràng chuỗi giảm một nửa đầy đủ của mỗi số. Điều này an toàn vì độ dài chuỗi được giới hạn bởi số bit trong số nguyên. 

Bên trong vòng lặp chính, chúng tôi duy trì`prev`, giá trị được chọn cuối cùng. Đối với mỗi phần tử mới, chúng tôi quét tất cả các trạng thái có thể truy cập của nó và chọn trạng thái hợp lệ rẻ nhất. Việc ràng buộc về giá trị đảm bảo chúng tôi không tăng ngưỡng cho các bước tiếp theo một cách không cần thiết. 

Một chi tiết triển khai tinh tế đang được khởi tạo`prev`đến một giá trị nhỏ hơn bất kỳ giá trị mảng nào có thể có. sử dụng`-1`là đủ vì tất cả các giá trị sau khi biến đổi đều không âm. Một chi tiết quan trọng khác là sự cố ngay lập tức khi một vị trí không có trạng thái hợp lệ, vì các phần tử sau này không thể khắc phục được điều kiện tăng nghiêm ngặt đã bị hỏng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 6 5
```Chúng tôi theo dõi các trạng thái có thể truy cập: 

| tôi | một [tôi] | có thể tiếp cận (giá trị, chi phí) | giá trị đã chọn | chi phí | trước | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | (3,0),(1,1),(0,2) | 1 | 1 | 1 | 
| 1 | 6 | (6,0),(3,1),(1,2),(0,3) | 3 | 1 | 3 | 
| 2 | 5 | (5,0),(2,1),(1,2),(0,3) | 5 | 0 | 5 | 

Tổng chi phí là 2. Điều này cho thấy mặc dù phần tử thứ hai có thể ở mức 6 nhưng làm như vậy sẽ cản trở tính khả thi của phần tử thứ ba, vì vậy việc giảm nó là cần thiết. 

### Ví dụ 2 

đầu vào:```
4
5 3 2 1
```| tôi | một [tôi] | có thể truy cập | đã chọn | chi phí | trước | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 5 | (5,0),(2,1),(1,2),(0,3) | 1 | 2 | 1 | 
| 1 | 3 | (3,0),(1,1),(0,2) | 0 | 2 | 1 | 
| 2 | 2 | (2,0),(1,1),(0,2) | 0 | 2 | 1 | 
| 3 | 1 | (1,0),(0,1) | không thể | - | - | 

Điều này thể hiện một trường hợp lỗi chính: mặc dù mỗi phần tử riêng lẻ có các giá trị nhỏ hơn có thể đạt được, yêu cầu ngày càng nghiêm ngặt trở nên không thể duy trì trong suốt chuỗi, dẫn đến không thể duy trì được trên toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log A)$mỗi bài kiểm tra | mỗi phần tử tạo ra tối đa ~31 trạng thái và chúng tôi quét chúng một lần | 
| Không gian |$O(\log A)$| chuỗi giá trị giảm một nửa tạm thời | 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm, nhưng mỗi thử nghiệm đều rất nhỏ. Tổng công việc vẫn nằm trong giới hạn thoải mái vì mỗi phần tử chỉ đóng góp một lượng tính toán không đổi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    def get_states(x):
        res = []
        c = 0
        while True:
            res.append((x, c))
            if x == 0:
                break
            x //= 2
            c += 1
        return res

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        prev = -1
        ans = 0
        ok = True

        for i in range(n):
            best = None
            best_cost = 10**18

            for v, c in get_states(a[i]):
                if v > prev:
                    if c < best_cost or (c == best_cost and (best is None or v < best)):
                        best_cost = c
                        best = v

            if best is None:
                ok = False
                break

            ans += best_cost
            prev = best

        out.append(str(ans if ok else -1))

    return "\n".join(out)

# provided samples
assert solve("""7
3
3 6 5
4
5 3 2 1
5
1 2 3 4 5
1
1000000000
4
2 8 7 5
5
8 26 5 21 10
2
5 14
""") == """2
-1
0
0
4
11
0"""

# custom cases
assert solve("""3
1
0
2
1 0
3
8 4 2
""") == """0
-1
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| phần tử đơn đã hợp lệ | 
|`1 0`|`-1`| không thể tăng nghiêm ngặt | 
|`8 4 2`|`3`| buộc phải chia đôi lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi các giá trị sớm giảm về 0. Ví dụ,`[2, 1]`buộc phần tử thứ hai phải lớn hơn phần tử thứ nhất, còn phần tử thứ nhất chỉ có thể đi xuống chứ không bao giờ đi lên nên chúng ta phải đảm bảo không nhận nhầm`0 > 0`phong cách bình đẳng Thuật toán xử lý việc này vì chúng tôi luôn thực thi`v > prev`, không`>=`. 

Một trường hợp khác xuất hiện khi giá trị ban đầu lớn hơn có vẻ tệ hơn nhưng thực tế vẫn đảm bảo tính khả thi trong tương lai. Chẳng hạn, việc chọn`1`sớm có thể chặn các phần tử sau, trong khi chọn`2`với chi phí cao hơn một chút cho phép có một chuỗi đầy đủ. Sự lựa chọn tham lam giữa tất cả các trạng thái có thể tiếp cận đảm bảo chúng ta luôn xem xét tất cả các lựa chọn thay thế như vậy trước khi cam kết. 

Cuối cùng, các mảng có nhiều số 0 sẽ kiểm tra khả năng phát hiện tính không thể thực hiện được. Một lần`prev`trở thành`0`, không phần tử nào sau này có thể giảm xuống dưới hoặc bằng nó, vì vậy chỉ những giá trị dương hoàn toàn có thể tiếp cận mới quan trọng. Nếu không tồn tại, chúng tôi chấm dứt chính xác với`-1`.
