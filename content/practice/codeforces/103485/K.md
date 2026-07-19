---
title: "CF 103485K - Tri ân các Pharaoh"
description: "Chúng ta có mô hình một năm với lịch tròn có độ dài k, trong đó các ngày lặp lại sau mỗi k bước. Có n pharaoh và mỗi pharaoh được chỉ định một khu vực với hai giai đoạn: trồng trọt mất p[i] đơn vị thời gian và thu hoạch mất c[i] đơn vị thời gian."
date: "2026-07-03T06:26:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103485
codeforces_index: "K"
codeforces_contest_name: "Copa Do Mat\u00e3o, University Of S\u00e3o Paulo Programming Contest"
rating: 0
weight: 103485
solve_time_s: 52
verified: true
draft: false
---

[CF 103485K - Cống hiến cho các Pharaoh](https://codeforces.com/problemset/problem/103485/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp mô hình một năm với lịch tròn có độ dài`k`, nơi ngày lặp lại mỗi ngày`k`các bước. có`n`các pharaoh, và mỗi pharaoh được giao một khu vực với hai giai đoạn: quá trình trồng trọt`p[i]`đơn vị thời gian và việc thu hoạch mất`c[i]`đơn vị thời gian. Sau khi cả hai giai đoạn hoàn thành, một lễ hội duy nhất dành cho vị pharaoh đó sẽ diễn ra vào đúng thời điểm.`p[i] + c[i]`. 

Vì thời gian có tính tuần hoàn với chu kỳ`k`, điều quan trọng đối với việc lập kế hoạch không phải là thời gian tuyệt đối mà là vị trí ngày trong năm, tức là`(p[i] + c[i]) mod k`. 

Chúng tôi được thông báo rằng mỗi lễ hội phải diễn ra vào một ngày trong vòng ngày đầu tiên.`n`ngày của một năm nào đó, nghĩa là ngày trong năm của nó phải nằm trong khoảng`[0, n-1]`. Vì việc dịch chuyển cả năm chỉ cộng bội số của`k`, nó không làm thay đổi modulo dư`k`, vì vậy ngày lễ hội của mỗi pharaoh hoàn toàn được quyết định bởi`t[i] = (p[i] + c[i]) mod k`. 

Hai lễ hội không được phép diễn ra trong cùng một ngày dương lịch, kể cả vào những năm khác nhau. Điều đó loại bỏ mọi quyền tự do phân tách các va chạm bằng cách sử dụng dịch chuyển năm, bởi vì các va chạm chỉ phụ thuộc vào modulo lớp dư lượng`k`. 

Vì vậy, nhiệm vụ giảm xuống còn việc kiểm tra xem những`n`dư lượng đều khác biệt và mỗi nằm trong`[0, n-1]`. 

Từ góc độ hạn chế,`n`đi lên`10^6`, vì vậy mọi nghiệm đều phải tuyến tính hoặc gần tuyến tính. Sắp xếp cũng được, băm cũng được, nhưng mọi thứ bậc hai đều không thể thực hiện được ngay lập tức. Điểm mấu chốt là một khi bài toán được quy giản một cách chính xác về các phần dư mô-đun thì không còn tìm kiếm tổ hợp nào nữa. 

Một vài trường hợp khó phát hiện nếu không được giảm thiểu cẩn thận: 

Nếu hai pharaoh sản xuất giống nhau`(p[i] + c[i]) % k`, chúng sẽ va chạm ngay cả khi tổng của chúng khác nhau đáng kể, chẳng hạn`k = 5`,`t = [1, 6]`cả hai bản đồ tới`1`, gây xung đột. 

Nếu phần dư có giá trị modulo`k`nhưng vượt quá`n-1`, Ví dụ`n = 3`,`k = 10`, và số dư bằng`7`, thì nó vi phạm yêu cầu “n ngày đầu tiên” mặc dù đó là ngày hợp lệ trong năm. 

Cuối cùng, nếu người ta cố gắng gán sai các năm khác nhau để tránh xung đột, điều đó sẽ thất bại vì sự thay đổi năm không thay đổi ngày trong năm. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng gán mỗi pharaoh vào một ngày cụ thể trong một năm nào đó và mô phỏng rõ ràng các vị trí, kiểm tra tất cả các va chạm trong tất cả các ca có thể xảy ra trong năm. Điều đó nhanh chóng trở nên không khả thi vì mỗi lựa chọn đều tương tác với tất cả những lựa chọn khác, dẫn đến suy luận theo cấp số nhân hoặc ít nhất là bậc hai về các vị trí. 

Sự đơn giản hóa cấu trúc quan trọng là thời gian có tính tuần hoàn với mô đun cố định`k`, do đó mỗi sự kiện sẽ sụp đổ thành một dư lượng bất biến duy nhất`(p[i] + c[i]) mod k`. Một khi điều này được quan sát thấy, tất cả sự tự do sẽ biến mất: chúng ta chỉ đang kiểm tra xem liệu các phần dư này có tạo thành một ánh xạ nội xạ vào tập hợp hay không`{0, 1, ..., n-1}`. 

Điều này làm giảm vấn đề xuống còn việc kiểm tra tần số và xác thực phạm vi đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập kế hoạch Brute Force qua nhiều năm | Hàm mũ / không khả thi | Cao | Quá chậm | 
| Giảm mô-đun + băm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính ngày lễ hội hiệu quả 

Với mỗi pharaoh, hãy tính`t[i] = (p[i] + c[i]) % k`. Đây là ngày duy nhất trong năm có thể diễn ra lễ hội, vì việc dịch chuyển từng năm không làm thay đổi giá trị này. 

### 2. Xác thực giới hạn phạm vi ngày 

Kiểm tra xem mỗi`t[i]`nằm ở`[0, n-1]`. Nếu bất kỳ giá trị nào lớn hơn hoặc bằng`n`, không thể cấu hình được vì lễ hội đó không thể đặt trong phạm vi cho phép trước`n`ngày. 

### 3. Kiểm tra tính duy nhất 

Chèn từng cái`t[i]`thành một tập băm. Nếu bất kỳ giá trị nào đã có sẵn thì hai lễ hội có cùng một ngày dương lịch, khiến lịch trình không hợp lệ. 

### 4. Kết quả trả về 

Nếu cả hai điều kiện đều đúng cho tất cả các pharaoh, xuất ra`"S"`, nếu không thì xuất ra`"N"`. 

### Tại sao nó hoạt động 

Thời gian lễ hội của mỗi pharaoh được cố định theo modulo`k`, do đó việc tự do lập kế hoạch qua các năm không ảnh hưởng đến việc đặt hàng trong vòng một năm. Hạn chế duy nhất quan trọng là liệu những chất cặn cố định này có thể được đưa vào nguồn nguyên liệu sẵn có hay không.`n`khe không có va chạm. Thuật toán trực tiếp xác minh khả năng xâm nhập vào miền được phép, do đó, bất kỳ lỗi nào đều tương ứng chính xác với sự chồng chéo bị cấm hoặc phân công ngoài phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    seen = set()
    
    for i in range(n):
        t = (p[i] + c[i]) % k
        if t >= n:
            print("N")
            return
        if t in seen:
            print("N")
            return
        seen.add(t)
    
    print("S")

if __name__ == "__main__":
    solve()
```Mã tuân theo mức giảm chính xác. Sự tính toán`(p[i] + c[i]) % k`là vị trí thời gian chuẩn. các`t >= n`kiểm tra thực thi hạn chế “n ngày đầu tiên”. Bộ này quy định rằng không có hai pharaoh nào ở cùng một ngày. 

Một cạm bẫy triển khai phổ biến là quên rằng cả hai ràng buộc phải được kiểm tra độc lập: chỉ tính duy nhất là không đủ nếu một giá trị nằm ngoài phạm vi ngày được phép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
0 1
1 0
```| tôi | p[i] | c[i] | t = (p[i]+c[i])%k | đã thấy trước đây | phạm vi hợp lệ | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | không | 1 < 2 | được | 
| 1 | 1 | 0 | 1 | vâng | 1 < 2 | xung đột | 

Đầu ra là`"N"`vì cả hai vị pharaoh đều lập bản đồ về ngày thứ nhất, tạo ra một vụ va chạm. 

### Ví dụ 2 

đầu vào:```
3 3
2 0 1
1 2 0
```| tôi | p[i] | c[i] | t | đã thấy | hợp lệ | quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 0 | không | vâng | được | 
| 1 | 0 | 2 | 2 | không | vâng | được | 
| 2 | 1 | 0 | 1 | không | vâng | được | 

Tất cả các dư lượng đều khác biệt và nằm trong`[0,2]`, vậy đầu ra là`"S"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi pharaoh được xử lý một lần với các phép toán băm O(1) | 
| Không gian | O(n) | Đặt nhiều nhất cửa hàng`n`dư lượng | 

Giải pháp thoải mái phù hợp với các ràng buộc lên đến`10^6`các phần tử vì cả bộ nhớ và thời gian đều có quy mô tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    out = io.StringIO()
    sys.stdout = out
    
    import sys as _sys
    input = _sys.stdin.readline
    
    n, k = map(int, input().split())
    p = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    seen = set()
    for i in range(n):
        t = (p[i] + c[i]) % k
        if t >= n or t in seen:
            return "N"
        seen.add(t)
    return "S"

assert run("2 2\n0 1\n1 0\n") == "N"
assert run("3 3\n2 0 1\n1 2 0\n") == "S"

assert run("1 10\n5\n5\n") == "S"
assert run("3 5\n0 1 2\n0 0 0\n") == "N"
assert run("4 10\n1 2 3 4\n0 0 0 0\n") == "S"
assert run("3 3\n0 3 6\n0 0 0\n") == "N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | S | trường hợp tối thiểu | 
| tất cả thời gian thu hoạch bằng không | N | dư lượng trùng lặp | 
| giá trị hỗn hợp | S | sự đúng đắn bình thường | 
| dư lượng lặp đi lặp lại | N | phát hiện va chạm | 
| hành vi modulo ranh giới | N | xử lý bọc mod | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị đều có modulo riêng biệt.`k`nhưng một cái vượt quá phạm vi cho phép. Ví dụ, nếu`n = 3`,`k = 10`, và dư lượng tính toán là`7`, nó vẫn không hợp lệ vì không thể đặt trong vòng ba ngày đầu tiên. Thuật toán từ chối điều này ngay lập tức thông qua`t >= n`điều kiện trước bất kỳ vấn đề lý luận duy nhất nào. 

Một trường hợp cạnh khác là va chạm hoàn toàn khi giảm modulo. Ví dụ, nếu nhiều pharaoh khác nhau về`(p[i] + c[i])`nhưng chia sẻ cùng một phần dư modulo`k`, chúng chắc chắn sẽ xung đột bất kể cách giải thích nào về việc lập kế hoạch trong nhiều năm. Kiểm tra dựa trên tập hợp nắm bắt điều này trực tiếp trong quá trình lặp. 

Một trường hợp tế nhị cuối cùng là khi`k < n`không thể thực hiện được do những hạn chế (`n ≤ k`), nhưng ngay cả khi đó, các phần dư vẫn có thể va chạm nhau do quá trình khử modulo thu gọn một không gian số nguyên lớn hơn thành một miền tuần hoàn nhỏ hơn.
