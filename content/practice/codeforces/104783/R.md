---
title: "CF 104783R - Sông Cyanua"
description: "Chúng ta có một dãy dài các tháp truyền thông được biểu thị bằng một chuỗi nhị phân. Mỗi ký tự tương ứng với một tòa tháp theo thứ tự dọc theo một dòng. Số 1 có nghĩa là tháp nằm trên đất khô, trên bờ hoặc đảo, trong khi số 0 có nghĩa là tháp nằm bên trong dòng sông xyanua nguy hiểm."
date: "2026-06-28T14:50:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104783
codeforces_index: "R"
codeforces_contest_name: "2021-2022 CTU Open Contest"
rating: 0
weight: 104783
solve_time_s: 51
verified: true
draft: false
---

[CF 104783R - Sông Cyanua](https://codeforces.com/problemset/problem/104783/R) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy dài các tháp truyền thông được biểu thị bằng một chuỗi nhị phân. Mỗi ký tự tương ứng với một tòa tháp theo thứ tự dọc theo một dòng. MỘT`1`có nghĩa là tháp nằm trên đất khô, trên bờ hoặc trên đảo, trong khi`0`có nghĩa là tòa tháp nằm bên trong một dòng sông xyanua nguy hiểm. 

Mục tiêu là chứng nhận tất cả các tòa tháp. Tháp được đánh dấu`1`thật dễ dàng: họ có thể được chứng nhận ngay vào ngày đầu tiên. Tháp được đánh dấu`0`khó hơn: mỗi tòa tháp như vậy phải mất cả ngày để chứng nhận và chỉ có thể được xử lý nếu ít nhất một trong những tòa tháp lân cận của nó đã được chứng nhận trước đó ít nhất một ngày. Nhiều tòa tháp có thể được xử lý song song trong cùng một ngày miễn là tuân thủ quy tắc phụ thuộc. 

Nhiệm vụ là xác định số ngày tối thiểu cần thiết cho đến khi mỗi tòa tháp được chứng nhận. 

Ràng buộc rằng chuỗi có thể lên tới 300.000 ký tự ngụ ý rằng bất kỳ mô phỏng bậc hai nào trong nhiều ngày hoặc quét lặp lại toàn bộ mảng mỗi ngày là không thể. MỘT$O(n^2)$việc truyền bá sẽ yêu cầu theo thứ tự$9 \times 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn thực tế. Điều này thúc đẩy chúng ta hướng tới một phương pháp tuyến tính hoặc gần tuyến tính để tính toán câu trả lời trong một hoặc hai lần. 

Một vấn đề tế nhị là việc nhân giống phụ thuộc vào những người hàng xóm mà bản thân họ có thể được chứng nhận sau này. Một mô phỏng ngây thơ chỉ mở rộng từ bản gốc`1`Một bước mỗi ngày là không đủ trừ khi được cấu trúc cẩn thận, bởi vì các vị trí mới được chứng nhận cũng trở thành nguồn lan truyền trong tương lai. 

Các trường hợp cạnh phát sinh khi chuỗi có thời gian dài`0`S. Ví dụ, hãy xem xét: 

đầu vào:`100001`Đầu ra dự kiến: phụ thuộc vào tốc độ truyền qua chuỗi. 

Một trường hợp cạnh khác là một khối duy nhất: 

đầu vào:`101`Ở đây chính giữa`0`liền kề với một tòa tháp đã được chứng nhận ngay lập tức nên nó sẽ được chứng nhận vào ngày đầu tiên. 

Một trường hợp tế nhị hơn là một đoạn dài các số 0 được giới hạn bởi các số 1: 

đầu vào:`1000001`Câu trả lời phụ thuộc vào việc ảnh hưởng từ cả hai đầu cần di chuyển bao xa và gặp nhau ở giữa. 

Một cách tiếp cận đơn giản chỉ xử lý từ một phía hoặc giả định sự lan truyền độc lập từ phía ban đầu`1`s mà không xem xét việc mở rộng đồng thời sẽ tính toán sai các vị trí trung tâm trong các lần chạy dài bằng 0. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực mô phỏng ngày một cách rõ ràng. Chúng tôi duy trì những tòa tháp nào được chứng nhận và liên tục quét mảng. Vào mỗi ngày, bất kỳ điều gì không chắc chắn`0`tòa tháp có ít nhất một người hàng xóm được chứng nhận từ ngày hôm trước sẽ được chứng nhận. Chúng tôi lặp lại cho đến khi tất cả các tòa tháp được chứng nhận. 

Điều này đúng vì nó phản ánh trực tiếp các quy tắc. Tuy nhiên, mỗi ngày có thể yêu cầu quét toàn bộ chuỗi và trong trường hợp xấu nhất là một chuỗi có độ dài bằng 0$n$yêu cầu$O(n)$ngày. Điều này dẫn đến$O(n^2)$tổng số hoạt động, quá chậm đối với$n = 300000$. 

Quan sát quan trọng là thời gian chứng nhận chỉ phụ thuộc vào khoảng cách đến tòa tháp đã được chứng nhận gần nhất. Mọi`1`bắt đầu ở thời điểm 0. MỘT`0`chỉ có thể được chứng nhận sau khi gần nhất`1`đã được truyền bá qua trung gian`0`S. Vì sự lan truyền lan ra bên ngoài một cách đối xứng và độc lập với tất cả`1`s, vấn đề giảm xuống còn tính toán, đối với mỗi vị trí, nó cách vị trí gần nhất bao xa`1`theo cách tôn trọng thực tế rằng`1`s đang hoạt động ngay từ đầu. 

Điều này biến quá trình thành bài toán khoảng cách ngắn nhất trên một đường: mỗi vị trí cần thời gian bằng khoảng cách của nó tới vị trí gần nhất.`1`, nhưng với ràng buộc rằng`1`bản thân s là nguồn tại thời điểm 0. Câu trả lời là mức tối đa của những khoảng cách này. 

Chúng ta có thể tính toán điều này theo hai lần tuyến tính: một lần từ trái sang phải theo dõi lần nhìn thấy cuối cùng`1`, và một từ phải sang trái. Mỗi`0`đạt được khoảng cách tối thiểu của nó tới một`1`ở hai bên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tính toán khoảng cách hai lượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình này như tính toán xem mỗi tòa tháp phải đợi bao lâu cho đến khi được chứng nhận. 

1. Khởi tạo một mảng`dist`trong đó mỗi vị trí sẽ lưu trữ số ngày tối thiểu cần thiết để tòa tháp đó có thể được chứng nhận. Chúng ta bắt đầu bằng cách coi tất cả các vị trí là vô cùng xa so với một`1`. 
2. Quét từ trái sang phải trong khi vẫn duy trì chỉ mục của trang gần đây nhất`1`. Khi chúng ta gặp phải một`1`, chúng tôi đặt lại trình theo dõi. Khi chúng ta gặp phải một`0`, chúng tôi đo khoảng cách của nó đến lần nhìn thấy cuối cùng này`1`ở bên trái. Điều này cung cấp cho ứng viên thời gian khi việc truyền bá bên trái có thể đạt tới nó. 
3. Quét từ phải sang trái tương tự, theo dõi vị trí gần nhất`1`ở bên phải. Đối với mỗi`0`, chúng tôi tính toán khoảng cách của nó đến gần nhất`1`ở bên phải và kết hợp nó với khoảng cách bên trái đã tính toán trước đó bằng cách lấy giá trị tối thiểu. 
4. Đối với mọi vị trí, giá trị cuối cùng biểu thị tốc độ mà tòa tháp đó có thể được chứng nhận theo cả hai hướng truyền. 
5. Câu trả lời cuối cùng là giá trị tối đa trên tất cả các vị trí, vì chúng ta phải đợi cho đến khi tháp chậm nhất được chứng nhận. 

Lý do điều này có tác dụng là vì mỗi`1`hoạt động như một nguồn chứng nhận đồng thời tại thời điểm 0. Ràng buộc lan truyền cho phép thông tin truyền ra ngoài một bước mỗi ngày, do đó, mỗi vị trí đều có thể được tiếp cận một cách hiệu quả bởi nguồn gần nhất theo nghĩa đường đi ngắn nhất trên biểu đồ đường. Thực hiện cả hai hướng đảm bảo chúng tôi nắm bắt được nguồn gần nhất bất kể hướng nào và sử dụng mức tối đa đảm bảo hoàn thành toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

INF = 10**18
left_dist = [INF] * n

last = -1
for i in range(n):
    if s[i] == '1':
        last = i
        left_dist[i] = 0
    else:
        if last != -1:
            left_dist[i] = i - last

right_dist = [INF] * n
last = -1
for i in range(n - 1, -1, -1):
    if s[i] == '1':
        last = i
        right_dist[i] = 0
    else:
        if last != -1:
            right_dist[i] = last - i

ans = 0
for i in range(n):
    d = min(left_dist[i], right_dist[i])
    ans = max(ans, d)

print(ans)
```Lần đầu tiên tính toán khoảng cách giữa mỗi vị trí với vị trí gần nhất`1`ở bên trái của nó. Vị trí trước vị trí đầu tiên`1`vẫn không thể truy cập được chỉ từ phía bên trái. Đường chuyền thứ hai thực hiện tương tự từ bên phải. 

Bước kết hợp được thực hiện ở mức tối thiểu vì tháp có thể được chứng nhận từ hai phía. Mức tối đa cuối cùng chọn tháp chậm nhất, xác định tổng thời gian hoàn thành. 

Một sai lầm phổ biến là quên rằng sự lan truyền có thể đến từ cả hai hướng. Một cách khác là chỉ điều trị ban đầu`1`s làm nguồn nhưng bỏ qua khoảng cách đó là đối xứng, điều này dẫn đến việc đánh giá quá cao thời gian trong bố cục không đối xứng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`101`Vượt qua trái sang phải: 

| tôi | s[i] | cuối cùng | trái_dist | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 0 | 0 | 1 | 
| 2 | 1 | 2 | 0 | 

Vượt qua phải sang trái: 

| tôi | s[i] | cuối cùng | đúng_dist | 
| --- | --- | --- | --- | 
| 2 | 1 | 2 | 0 | 
| 1 | 0 | 2 | 1 | 
| 0 | 1 | 0 | 0 | 

Kết hợp: 

| tôi | phút(trái,phải) | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 

Câu trả lời là 1. Tháp giữa cần một ngày vì nó cách hàng xóm được chứng nhận một bước chân. 

Điều này xác nhận rằng việc truyền bá nắm bắt chính xác sự phụ thuộc từng bước từ các tháp được chứng nhận liền kề. 

### Ví dụ 2 

đầu vào:`100001`Đường chuyền trái: 

| tôi | s[i] | cuối cùng | trái_dist | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 2 | 
| 3 | 0 | 0 | 3 | 
| 4 | 0 | 0 | 4 | 
| 5 | 1 | 5 | 0 | 

Đường chuyền phải: 

| tôi | s[i] | cuối cùng | đúng_dist | 
| --- | --- | --- | --- | 
| 5 | 1 | 5 | 0 | 
| 4 | 0 | 5 | 1 | 
| 3 | 0 | 5 | 2 | 
| 2 | 0 | 5 | 3 | 
| 1 | 0 | 5 | 4 | 
| 0 | 1 | 0 | 0 | 

Khoảng cách cuối cùng: 

| tôi | phút(trái,phải) | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 0 | 

Câu trả lời là 2. Đạt được tâm từ cả hai phía và đường truyền chậm nhất gặp nhau ở giữa. 

Điều này chứng tỏ rằng giải pháp mô hình hóa chính xác việc mở rộng mặt sóng hai chiều. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Hai lần quét tuyến tính trên chuỗi cộng với một lần quét cuối cùng | 
| Không gian | O(n) | Mảng lưu trữ khoảng cách trái và phải | 

Thuật toán tuyến tính theo độ dài của chuỗi đầu vào, vừa vặn trong giới hạn tối đa 300.000 ký tự. Việc sử dụng bộ nhớ cũng tuyến tính và đủ nhỏ cho các ràng buộc thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    n = len(s)

    INF = 10**18
    left_dist = [INF] * n

    last = -1
    for i in range(n):
        if s[i] == '1':
            last = i
            left_dist[i] = 0
        else:
            if last != -1:
                left_dist[i] = i - last

    right_dist = [INF] * n
    last = -1
    for i in range(n - 1, -1, -1):
        if s[i] == '1':
            last = i
            right_dist[i] = 0
        else:
            if last != -1:
                right_dist[i] = last - i

    ans = 0
    for i in range(n):
        ans = max(ans, min(left_dist[i], right_dist[i]))

    return str(ans)

# provided samples (assumed from statement style)
assert run("101\n") in ["1", "1\n"]
assert run("100001\n") in ["2", "2\n"]

# custom cases
assert run("1\n") in ["0", "0\n"]
assert run("11111\n") in ["0", "0\n"]
assert run("10001\n") in ["2", "2\n"]
assert run("10101\n") in ["1", "1\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| nút đơn đã được chứng nhận | 
|`11111`|`0`| tất cả chứng nhận ngay lập tức | 
|`10001`|`2`| truyền đối xứng vào giữa | 
|`10101`|`1`| sự đúng đắn của cấu trúc xen kẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi chỉ có một`1`ở một đầu của chuỗi chẳng hạn`100000`. Đường chuyền từ trái sang phải giúp khoảng cách tăng dần từ 0 lên 5, trong khi đường chuyền từ phải sang trái không thể cải thiện được gì. Thuật toán trả về chính xác 5 vì tháp xa nhất chỉ phụ thuộc vào một nguồn. 

Một trường hợp khác là khi không có số 0 bên trong ngoại trừ những số 0 bị cô lập như`1110111`. Số 0 ở giữa có khoảng cách 1 từ cả hai phía nên được xác nhận trong một ngày và câu trả lời trở thành 1. 

Trường hợp cạnh thứ ba là một mẫu xen kẽ hoàn toàn như`1010101`. Mỗi số 0 liền kề với một số 1 nên mỗi số 0 đều được chứng nhận trong một ngày. Giá trị tối đa trên tất cả các vị trí là 1, phù hợp với thực tế là không cần truyền dài hơn một bước.
