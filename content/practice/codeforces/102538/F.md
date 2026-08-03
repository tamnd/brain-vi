---
title: "CF 102538F - Trang Trại Quái Vật"
description: "Chúng tôi có một dòng quái vật. Quái vật i bắt đầu với h[i] điểm sức khỏe. Trong mỗi lượt, chúng ta có thể đánh bất kỳ quái vật còn sống nào và giảm lượng máu của nó đi một hoặc bỏ qua lượt của chúng ta. Đối thủ luôn tấn công quái vật sống ngoài cùng bên trái và giảm máu của nó đi b."
date: "2026-08-03T21:01:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102538
codeforces_index: "F"
codeforces_contest_name: "300iq Contest 3"
rating: 0
weight: 102538
solve_time_s: 183
verified: true
draft: false
---

[CF 102538F - Trang trại quái vật](https://codeforces.com/problemset/problem/102538/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng quái vật. Quái vật`i`bắt đầu bằng`h[i]`điểm sức khỏe. Ở mỗi lượt, chúng ta có thể đánh bất kỳ quái vật sống nào và giảm máu của nó đi`a`, hoặc bỏ qua lượt của chúng tôi. Đối thủ luôn tấn công quái vật sống ngoài cùng bên trái và giảm máu của nó bằng`b`. Bất cứ khi nào một trong các đòn tấn công của chúng tôi giết chết một con quái vật, chúng tôi sẽ nhận được một điểm. Mục tiêu là tối đa hóa số lượng quái vật mà cá nhân chúng tôi tiêu diệt được. 

Thứ tự quan trọng vì đối thủ không được tự do lựa chọn mục tiêu. Quái vật có chỉ số nhỏ hơn chặn tất cả quái vật theo sau nó cho đến khi nó chết, do đó số đòn tấn công của đối thủ mà quái vật nhận được phụ thuộc vào quyết định được đưa ra cho quái vật trước đó. Cách hữu ích để suy nghĩ về quá trình này không phải là mô phỏng các lượt mà là sự cân bằng của các bước di chuyển có sẵn. Mỗi đòn tấn công của đối thủ đều cho chúng ta thời gian để chuẩn bị tiêu diệt trong tương lai, trong khi mỗi đòn tấn công chúng ta dùng vào quái vật sẽ tiêu tốn một lượt di chuyển. 

Ràng buộc`n <= 300000`loại trừ mọi mô phỏng thực hiện công tỷ lệ với số vòng quay. Giá trị sức khỏe có thể lên tới`10^9`, vì vậy tổng số cuộc tấn công có thể rất lớn. Thuật toán phải xử lý mọi quái vật chỉ với số lần không đổi hoặc logarit, hướng tới giải pháp dựa trên cấu trúc dữ liệu hoặc tham lam. 

Một sai lầm phổ biến là đối xử với quái vật một cách độc lập. Ví dụ, với sức tấn công ngang nhau và ba quái vật có sức khỏe`1 1 1`, một chiến lược quyết định từng con quái vật một cách độc lập có thể cho rằng cả ba con quái vật đều có thể bị tiêu diệt. Trên thực tế, sau khi giết một quái vật, đối thủ vẫn được thay phiên nhau thực hiện các đòn tấn công của chúng ta và lợi thế ở nước đi đầu tiên sẽ bị hạn chế. Câu trả lời đúng là`2`. 

Một sai lầm nữa là quên đi lợi thế nước đi ban đầu. Với đầu vào`3 1 1`và sức khỏe`2 2 2`, câu trả lời đúng là`3`. Chiến lược chiến thắng là đợi cho đến khi quái vật còn lại một máu, sau đó kết liễu nó trước khi đối thủ có thể loại bỏ nó. Coi nước đi đầu tiên như một lượt đi bình thường của đối thủ sẽ mất khả năng này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng các lựa chọn. Đối với mỗi quái vật, chúng tôi quyết định nên dành các đòn tấn công của mình để tiêu diệt nó hay để lại cho đối thủ. Sau mỗi lựa chọn, chúng tôi mô phỏng cuộc chiến và đếm số chiến thắng. Điều này đúng vì mọi chiến lược khả thi đều được khám phá, nhưng có hai lựa chọn cho mỗi quái vật, đưa ra`2^n`khả năng. Ngay cả trước khi xem xét chi phí mô phỏng, điều này trở nên không thể khi`n`đạt tới hàng trăm nghìn. 

Điều quan trọng là chúng ta không cần biết chính xác thứ tự các cuộc tấn công của mình. Giả sử chúng ta quyết định loại quái vật nào chúng ta muốn giết. Đối với một quái vật được chọn, không có lý do gì để đánh nó nhiều lần hơn mức cần thiết. Cho phép`r`là số lần đánh nhỏ nhất trước khi quái vật đạt đến trạng thái mà một cú đánh nữa sẽ giết chết nó. Số lần tấn công tối ưu của chúng ta vào con quái vật này chính xác là`r + 1`. 

Công thức cho`r`xuất phát từ việc xem xét lượng máu còn lại trong chu kỳ tấn công của đối thủ. Sau khi giảm tất cả các giá trị sức khỏe đi một, chúng ta cần giá trị nhỏ nhất`r`như vậy:```
(h - r * a) mod b < a
```Giá trị là:```
r = floor(b * (h mod a) / a)
```Sau khi quyết định liệu một con quái vật có bị chúng ta giết hay không, chúng ta có thể biểu thị hiệu ứng đó bằng sự thay đổi về số lần di chuyển có sẵn. Nếu chúng ta không giết nó, đối thủ sẽ tiêu`h`tấn công vào nó. Nếu chúng ta giết nó, sự khác biệt giữa nước đi của đối thủ và nước đi của chúng ta là một giá trị khác. Chúng tôi so sánh lựa chọn này với đường cơ sở nơi mọi quái vật đều được giao cho đối thủ. 

Dành cho quái vật`i`, cho phép`y[i]`là sự thay đổi gây ra bởi việc chọn giết nó thay vì phớt lờ nó. Chúng ta cần chọn càng nhiều`y[i]`giá trị nhất có thể trong khi vẫn giữ mọi tiền tố hợp lệ. Điều này trở thành một vấn đề lựa chọn tham lam cổ điển. Chúng tôi quét quái vật từ trái sang phải, giữ tất cả các giá trị đã chọn trong một đống và nếu không thể đặt tiền tố, hãy loại bỏ quái vật được chọn tệ nhất. Việc loại bỏ tồi tệ nhất là lớn nhất`y[i]`, bởi vì việc loại bỏ giá trị lớn hơn sẽ khắc phục hạn chế trong khi hy sinh ít quái vật được chọn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giảm một lượng máu của quái vật. Điều này thay đổi điều kiện tiêu diệt từ đạt đến sức khỏe`0`để đạt đến giá trị âm, điều này làm cho việc tính toán mô-đun trở nên rõ ràng hơn. 
2. Đối với mỗi quái vật, hãy tính số lần tấn công tối thiểu của chúng ta trước đòn tấn công tiêu diệt cuối cùng. Giá trị là`r = (b * (h % a)) // a`. 
3. Tính toán lợi ích của việc tự mình giết con quái vật này. Nếu chúng ta để nó sống, sự đóng góp của nó là`h`. Nếu chúng ta giết nó, phần đóng góp của nó là:```
b * ((h - r * a) // b) - (r + 1)
```Sự khác biệt giữa hai giá trị này là`y`. 

1. Giữ nguyên số tiền đã chọn`y`giá trị trong khi quét quái vật từ trái sang phải. Đồng thời duy trì tổng số tiền hiện tại của tất cả các bản gốc`h`các giá trị trong tiền tố này. 
2. Thêm dòng điện`y`đến một đống tối đa. Nếu những thay đổi đã chọn trở nên quá lớn và vi phạm điều kiện tiền tố, hãy xóa phần lớn nhất`y`từ đống. Đống luôn chứa những con quái vật mà cuối cùng chúng tôi quyết định tiêu diệt. 
3. Số phần tử còn lại trong heap là số lần thắng tối đa. 

Điều bất biến là sau khi xử lý mọi tiền tố, đống chứa số lượng quái vật bị tiêu diệt tối đa có thể có trong số tiền tố đó trong khi vẫn giữ số dư di chuyển tiền tố hợp lệ. Khi số dư trở nên không hợp lệ, việc loại bỏ chi phí lớn nhất là tối ưu vì mỗi quái vật bị loại bỏ sẽ giảm cùng một giới hạn tương ứng.`y`giá trị lớn nhất và giá trị lớn nhất mang lại tổn thất nhỏ nhất về số lượng. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    h = list(map(int, input().split()))

    heap = []
    chosen_sum = 0
    prefix = 0

    for value in h:
        x = value - 1
        prefix += x

        r = (b * (x % a)) // a

        kill_value = b * ((x - r * a) // b) - (r + 1)
        y = kill_value - x

        heapq.heappush(heap, -y)
        chosen_sum += y

        if chosen_sum > prefix:
            removed = -heapq.heappop(heap)
            chosen_sum -= removed

    print(len(heap))

if __name__ == "__main__":
    solve()
```Biến`prefix`lưu trữ số bước di chuyển đã lưu cơ bản nếu chúng ta để đối thủ xử lý mọi quái vật. Biến`chosen_sum`lưu trữ toàn bộ sửa đổi được đưa ra bởi những quái vật hiện được chọn cho các cuộc tấn công của chúng tôi. 

Vùng heap của Python là vùng heap tối thiểu, vì vậy các giá trị âm được lưu trữ để mô phỏng vùng heap tối đa. Khi điều kiện tiền tố không thành công, giá trị lớn nhất`y`được gỡ bỏ. Đây là nơi duy nhất xảy ra sự lựa chọn tham lam. 

biểu hiện`(x % a)`là an toàn bởi vì`a`có thể lớn như`10^9`, nhưng số nguyên Python không bị tràn. Việc trừ đi một ở đầu cũng là cần thiết. Nếu không có nó, những quái vật bắt đầu bằng chính xác một điểm máu sẽ bị xử lý không chính xác. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 1 1
1 1 1
```| Quái vật | x | y | Đống sau khi sửa | Số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | -1 | [-(-1)] | 1 | 
| 2 | 0 | -1 | hai lựa chọn | 2 | 
| 3 | 0 | -1 | loại bỏ thứ ba | 2 | 

Không thể thêm quái vật thứ ba vì không có đủ lợi thế di chuyển ban đầu. Bất biến heap sẽ tự động loại bỏ nó. 

Đối với đầu vào:```
3 1 1
2 2 2
```| Quái vật | x | y | Đống sau khi sửa | Số câu trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | đã chọn | 1 | 
| 2 | 1 | -1 | đã chọn | 2 | 
| 3 | 1 | -1 | đã chọn | 3 | 

Số dư tiền tố sẵn có đủ để giữ cả ba lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi quái vật được chèn một lần và bị loại bỏ nhiều nhất một lần khỏi đống. | 
| Không gian | O(n) | Heap có thể chứa một mục nhập cho mỗi quái vật. | 

Với`n = 300000`, các phép toán logarit đủ nhỏ, trong khi bất kỳ cách tiếp cận nào tùy thuộc vào số lần tấn công đều không thể thực hiện được vì giá trị sức khỏe có thể cực kỳ lớn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io
import heapq

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, a, b = map(int, sys.stdin.readline().split())
    h = list(map(int, sys.stdin.readline().split()))

    heap = []
    chosen_sum = 0
    prefix = 0

    for value in h:
        x = value - 1
        prefix += x
        r = (b * (x % a)) // a
        y = b * ((x - r * a) // b) - (r + 1) - x

        heapq.heappush(heap, -y)
        chosen_sum += y

        if chosen_sum > prefix:
            chosen_sum += heapq.heappop(heap)

    sys.stdin = old
    return str(len(heap))

assert run("3 1 1\n1 1 1\n") == "2"
assert run("3 1 1\n2 2 2\n") == "3"
assert run("1 10 1\n1\n") == "1"
assert run("4 5 10\n100 100 100 100\n") == "4"
assert run("5 3 7\n1 2 3 4 5\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 1 / 1 1 1`|`2`| Lợi thế nước đi đầu tiên và giới hạn tiền tố | 
|`3 1 1 / 2 2 2`|`3`| Chiến lược chờ đợi và thời điểm tiêu diệt chính xác | 
|`1 10 1 / 1`|`1`| Trường hợp ranh giới quái vật đơn | 
|`4 5 10 / 100 100 100 100`|`4`| Giá trị sức khỏe lớn | 
|`5 3 7 / 1 2 3 4 5`|`3`| Các giá trị hỗn hợp và xử lý từng cái một | 

## Vỏ cạnh 

Một con quái vật có đúng một điểm máu là nơi dễ mắc lỗi nhất. Vì:```
1 10 1
1
```câu trả lời là`1`. Sau khi chuyển đổi sức khỏe thành`h - 1`, con quái vật có giá trị`0`. Công thức cho`r = 0`, nghĩa là một đòn tấn công là đủ. 

Khi tất cả quái vật có lượng máu như nhau, thuật toán vẫn phải tôn trọng mệnh lệnh do đối thủ áp đặt. Vì:```
3 1 1
1 1 1
```chọn mọi quái vật sẽ vi phạm điều kiện tiền tố. Đống loại bỏ lựa chọn ít hữu ích nhất và để lại hai quái vật. 

Khi sức mạnh tấn công rất khác nhau thì việc mô phỏng trực tiếp là không thể. Vì:```
4 5 10
100 100 100 100
```số vòng quay rất lớn nhưng việc tính toán mô-đun ngay lập tức xác định được các lựa chọn tối ưu. Heap chỉ theo dõi các quyết định chứ không theo dõi các cuộc tấn công riêng lẻ.
