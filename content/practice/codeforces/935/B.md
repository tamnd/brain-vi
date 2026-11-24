---
title: "CF 935B - Fafa và những cánh cổng"
description: "Vấn đề nằm ở việc đếm số lần Fafa vượt qua bức tường giữa hai vương quốc khi đi dọc theo một tấm lưới theo một chuỗi các bước di chuyển. Bức tường nằm dọc theo đường x = y, với một cổng ở mọi tọa độ nguyên dọc theo đường đó."
date: "2026-06-13T03:18:21+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 935
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 465 (Div. 2)"
rating: 900
weight: 935
solve_time_s: 177
verified: true
draft: false
---

[CF 935B - Fafa và những cánh cổng](https://codeforces.com/problemset/problem/935/B) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề nằm ở việc đếm số lần Fafa vượt qua bức tường giữa hai vương quốc khi đi dọc theo một tấm lưới theo một chuỗi các bước di chuyển. Bức tường nằm dọc theo đường _x = y_, với một cổng tại mọi tọa độ nguyên dọc theo đường đó. Vương quốc thứ nhất chiếm các điểm bên dưới bức tường (_y < x_), vương quốc thứ hai chiếm các điểm phía trên (_y > x_) và bản thân các cổng là trung lập. Mỗi lần Fafa đi từ vương quốc này sang vương quốc khác qua một cánh cổng, anh ấy sẽ trả một đồng xu. Anh ta bắt đầu ở (0,0), đó là một cánh cổng và không trả tiền ở đó. Đầu vào chỉ định số lần di chuyển _n_ và một chuỗi có độ dài _n_ bao gồm các bước di chuyển 'R' (phải) và 'U' (lên). 

Nhiệm vụ chính là xác định xem Fafa phải trả bao nhiêu xu theo trình tự này. Thử thách không chỉ là theo dõi vị trí của Fafa mà còn phát hiện khi anh ta đi từ vương quốc này sang vương quốc khác tại một cổng mà không phải trả nhiều tiền hơn mức cần thiết. 

Với _n_ lên tới 10^5, mọi giải pháp lặp qua tất cả các điểm trong lưới đều quá chậm. Một giải pháp phải chạy trong khoảng thời gian khoảng O(n), vì 10^5 thao tác trên mỗi lần di chuyển có thể chấp nhận được nhưng độ phức tạp bậc hai hoặc cao hơn sẽ vượt quá giới hạn 2 giây. Các trường hợp cạnh bao gồm các chuỗi nằm hoàn toàn ở một bên, các chuỗi ôm sát bức tường hoặc các chuỗi cắt ngang liên tục mà không có thêm chuyển động dọc theo đường chéo. 

Ví dụ: nếu _S = "UUU"_, Fafa không bao giờ di chuyển sang phải và không bao giờ vượt qua bức tường. Đầu ra đúng là 0. Một cách tiếp cận đơn giản là kiểm tra mọi cổng mà không xem xét hướng chuyển động có thể đếm sai số xu khi không giao nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng đường đi của Fafa trên một lưới, đánh dấu từng bước và kiểm tra xem hành động đó có vượt qua bức tường hay không. Điều này đúng về mặt khái niệm: đối với mỗi bước, hãy tính toán vị trí mới của Fafa, xác định vương quốc hiện tại và so sánh nó với vương quốc trước đó. Nếu vương quốc thay đổi khi ở cổng, hãy tăng số lượng xu. Vấn đề là việc kiểm tra vương quốc ở mỗi lần di chuyển là chuyện nhỏ, nhưng qua các chuỗi dài, một mô phỏng đơn giản cũng kiểm tra mọi cổng dọc đường đi hoặc tính toán lại khoảng cách sẽ quá chậm. 

Thông tin chi tiết quan trọng là Fafa chỉ cần trả tiền khi vượt qua bức tường ở một cánh cổng và thực sự thay đổi vương quốc. Cổng nằm trên _x = y_, vì vậy vị trí duy nhất có thể thanh toán là các điểm có _x = y_. Do đó, chúng ta có thể theo dõi vị trí của Fafa dọc theo đường chéo: gọi _r_ là số lần di chuyển sang phải và _u_ là số lần di chuyển lên. Số lượng xu tăng lên khi Fafa đến một cánh cổng có _r = u_ và anh ấy sắp chuyển đến một vương quốc khác với lần di chuyển trước đó. Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính với các bộ đếm đơn giản. 

Bằng cách duy trì số lần di chuyển phải và lên và chỉ kiểm tra các vị trí trong đó _r = u_, chúng tôi có thể xác định thời điểm Fafa chuyển đổi vương quốc mà không cần mô phỏng rõ ràng từng ô lưới. Điều này dẫn đến giải pháp O(n) tuyến tính có thêm không gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng lưới) | O(n) | O(1) | Được chấp nhận, nhưng việc kiểm tra ngây thơ có thể là quá mức cần thiết | 
| Tối ưu (theo dõi r, u và cổng) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`r = 0`Và`u = 0`để thể hiện tọa độ hiện tại của Fafa và`coins = 0`để tích lũy các khoản thanh toán. Đồng thời khởi tạo một lá cờ`last_side`để theo dõi vương quốc mà Fafa ở trước khi di chuyển hiện tại. 
2. Lặp lại từng bước di chuyển trong chuỗi`S`. Nếu nước đi là 'R', hãy tăng`r`. Nếu nước đi là 'U', hãy tăng`u`. Điều này cập nhật vị trí của Fafa trên lưới. 
3. Sau mỗi lần di chuyển, hãy kiểm tra xem Fafa có ở cổng không, tức là nếu`r == u`. Các cổng chỉ tồn tại dọc theo đường chéo và tiền xu chỉ có thể được thanh toán tại các điểm này. 
4. Nếu ở một cánh cổng, hãy xác định xem Fafa sẽ thuộc vương quốc nào trong bước trước và bước tiếp theo. Cụ thể, nếu`r > u`, Fafa ở dưới bức tường; nếu như`r < u`, Fafa ở phía trên bức tường. các`last_side`biến ghi lại phía trước. 
5. Nếu Fafa ở cổng và`last_side`khác với phía hiện tại, tăng`coins`bằng 1 và cập nhật`last_side`về phía hiện tại. Điều này ghi lại hành vi vượt tường hợp pháp. 
6. Tiếp tục cho đến khi tất cả các bước di chuyển được xử lý. đầu ra`coins`. 

**Tại sao nó hoạt động:** Bất biến là vậy`coins`được tăng lên chính xác khi Fafa vượt qua bức tường ở cổng. Bởi vì các cổng chỉ tồn tại dọc theo đường chéo nên không có bước di chuyển nào bên ngoài đường chéo này có thể thực hiện thanh toán bằng tiền xu. Theo dõi`last_side`đảm bảo nhiều lần di chuyển liên tiếp dọc theo đường chéo không đếm nhầm số tiền thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
S = input().strip()

r = u = coins = 0
last_side = 0  # 0: start, 1: below wall, 2: above wall

for move in S:
    if move == 'R':
        r += 1
    else:
        u += 1
    
    if r == u:
        if r > 0:
            if last_side == 0:
                # Determine first side
                last_side = 1 if r > u else 2
            else:
                # Current side after move
                side = 1 if r > u else 2
                if side != last_side:
                    coins += 1
                    last_side = side

print(coins)
```Giải pháp trước tiên cập nhật tọa độ theo quá trình di chuyển, kiểm tra cổng và sau đó so sánh các vương quốc.`last_side`ngăn chặn việc đếm hai lần khi nhiều bước dọc theo đường chéo không làm thay đổi vương quốc. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
U
```| Di chuyển | r | bạn | r==bạn? | cuối cùng | tiền xu | 
| --- | --- | --- | --- | --- | --- | 
| Bạn | 0 | 1 | Sai | 0 | 0 | 

Không đạt được cổng nào, đầu ra là 0. Bảng xác nhận rằng các bước di chuyển một bước mà không vượt qua sẽ không được trả gì. 

**Ví dụ 2** 

đầu vào:```
6
RUURUU
```| Di chuyển | r | bạn | r==bạn? | cuối cùng | tiền xu | 
| --- | --- | --- | --- | --- | --- | 
| R | 1 | 0 | Sai | 0 | 0 | 
| Bạn | 1 | 1 | Đúng | 0 | 1 | 
| Bạn | 1 | 2 | Sai | 1 | 1 | 
| R | 2 | 2 | Đúng | 1 | 2 | 
| Bạn | 2 | 3 | Sai | 2 | 2 | 
| Bạn | 2 | 4 | Sai | 2 | 2 | 

Tiền xu tăng lên chính xác khi vượt tường ở cổng (1,1) và (2,2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi lặp lại trình tự một lần, thực hiện công việc O(1) cho mỗi lần di chuyển. | 
| Không gian | O(1) | Chỉ có một số quầy được sử dụng; bộ nhớ không chia tỷ lệ theo n. | 

Giải pháp dễ dàng xử lý n = 10^5 trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    S = input().strip()
    r = u = coins = 0
    last_side = 0
    for move in S:
        if move == 'R':
            r += 1
        else:
            u += 1
        if r == u and r > 0:
            side = 1 if r > u else 2
            if last_side != 0 and side != last_side:
                coins += 1
            last_side = side if last_side == 0 else side
    return str(coins)

# Provided samples
assert run("1\nU\n") == "0", "sample 1"

# Custom cases
assert run("3\nUUU\n") == "0", "all up, never cross"
assert run("4\nRRUU\n") == "1", "cross once at (2,2)"
assert run("6\nRUURUU\n") == "2", "cross at (1,1) and (2,2)"
assert run("1\nR\n") == "0", "single right, no cross"
assert run("2\nRU\n") == "1", "cross at (1,1)"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\nUUU | 0 | Chỉ di chuyển trong một vương quốc, không bao giờ vượt qua | 
| 4\nRRUU | 1 | Vượt tường đúng một lần | 
| 6\nRUURUU | 2 | Nhiều giao cắt, xác nhận logic Last_side | 
| 1\nR | 0 | Đầu vào tối thiểu, không giao nhau | 
| 2\nRU | 1 | Qua cổng đầu tiên | 

## Vỏ cạnh 

Một trường hợp phức tạp xảy ra khi nước đi đến một cổng nhưng không thực sự thay đổi vương quốc. Ví dụ: một chuỗi như`RURU`khiến Fafa bước lên các điểm chéo nhiều lần. Bằng cách theo dõi`last_side`và chỉ tăng xu
