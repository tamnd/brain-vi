---
title: "CF 64H - Bowling trên bàn"
description: "Chúng tôi được trao bảng xếp hạng cuối cùng của một giải đấu bowling trên bàn. Mỗi người tham gia có một tên duy nhất và số điểm nguyên. Nhiệm vụ không chỉ là sắp xếp người chơi mà còn gán nhãn xếp hạng theo kiểu được sử dụng trong các giải đấu thực tế."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "sortings"]
categories: ["algorithms"]
codeforces_contest: 64
codeforces_index: "H"
codeforces_contest_name: "Unknown Language Round 1"
rating: 2300
weight: 64
solve_time_s: 112
verified: true
draft: false
---

[CF 64H - Bowling trên bàn](https://codeforces.com/problemset/problem/64/H) 

**Đánh giá:** 2300 
**Thẻ:** *đặc biệt, phân loại 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao bảng xếp hạng cuối cùng của một giải đấu bowling trên bàn. Mỗi người tham gia có một tên duy nhất và số điểm nguyên. Nhiệm vụ không chỉ là sắp xếp người chơi mà còn gán nhãn xếp hạng theo kiểu được sử dụng trong các giải đấu thực tế. 

Người chơi được sắp xếp chủ yếu bằng cách giảm điểm. Nếu hai người chơi có cùng số điểm, tên của họ sẽ quyết định thứ tự từ điển. Sau khi sắp xếp, chúng ta phải xuất ra từng người tham gia cùng với vị trí của họ. 

Phần khó khăn là xử lý các mối quan hệ một cách chính xác. Nếu một số người chơi có cùng số điểm thì họ cũng có cùng một phạm vi địa điểm. Ví dụ: nếu ba người chơi chiếm các vị trí 5, 6 và 7 vì họ đều có số điểm 100 thì mỗi người trong số họ phải nhận được nhãn`5-7`. 

Kích thước đầu vào rất nhỏ. Có tối đa 100 người tham gia, vì vậy ngay cả các giải pháp bậc hai cũng hoàn toàn an toàn. Một cách sắp xếp đơn giản đã giải quyết được gần như toàn bộ vấn đề. Thách thức thực sự là định dạng chính xác các khoảng xếp hạng. 

Những trường hợp nguy hiểm đều liên quan đến ràng buộc. 

Hãy xem xét đầu vào này:```
3
alice 10
bob 10
carol 10
```Cả ba người chơi đều có chung vị trí từ 1 đến 3, vì vậy kết quả đúng là:```
1-3 alice
1-3 bob
1-3 carol
```Thay vào đó, việc triển khai bất cẩn có thể in:```
1 alice
2 bob
3 carol
```Điều đó sẽ chỉ định không chính xác các thứ hạng khác nhau cho các điểm bằng nhau. 

Một trường hợp tinh vi khác xuất hiện khi có sự hòa giữa bảng:```
5
anna 100
bella 90
claire 90
diana 80
eva 70
```Đầu ra đúng là:```
1 anna
2-3 bella
2-3 claire
4 diana
5 eva
```Thứ hạng tiếp theo sau khi hòa phải tiếp tục từ vị trí đã chiếm giữ thực tế. Sau khi chia sẻ vị trí 2 và 3, người tham gia tiếp theo sẽ nhận được vị trí 4 chứ không phải 3. 

Một trường hợp nữa thường gây ra lỗi sai lầm là hòa về cuối:```
4
a 5
b 4
c 1
d 1
```Đầu ra đúng là:```
1 a
2 b
3-4 c
3-4 d
```Khi xử lý các điểm được nhóm, chúng ta phải tính toán ranh giới khoảng một cách cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đầu tiên sắp xếp tất cả người tham gia theo điểm giảm dần và tên tăng dần. Sau đó, đối với mỗi người tham gia, hãy quét toàn bộ mảng để đếm xem có bao nhiêu người chơi có cùng số điểm và xác định vị trí đầu tiên và cuối cùng của nhóm điểm đó. 

Điều này hoạt động vì các ràng buộc rất nhỏ. Với tối đa 100 người chơi,`O(n^2)`quét chỉ thực hiện khoảng 10.000 so sánh trong trường hợp xấu nhất. 

Tuy nhiên, việc quét lại nhiều lần là không cần thiết. Sau khi sắp xếp, điểm bằng nhau sẽ trở thành liên tiếp. Quan sát đó thay đổi vấn đề từ "tìm kiếm trên toàn cầu để tìm điểm phù hợp" thành "xử lý các nhóm liền kề". 

Sau khi mảng được sắp xếp, chúng ta có thể duyệt qua nó từ trái sang phải. Giả sử chỉ số`i ... j`đều có chung số điểm. Sau đó những người chơi này chiếm giữ vị trí`i + 1 ... j + 1`trong bảng xếp hạng vì bảng xếp hạng dựa trên 1. 

Nếu như`i == j`, người chơi có thứ hạng duy nhất nên chúng tôi in một số duy nhất. Ngược lại, cả nhóm chia sẻ khoảng thời gian`i+1 - j+1`. 

Lý do nhóm này hoạt động là vì việc sắp xếp đã đảm bảo hai thuộc tính cùng một lúc. Điểm xuất hiện theo thứ tự xếp hạng và điểm bằng nhau tạo thành một khối liên tục. Điều đó có nghĩa là mọi nhóm liên kết có thể được xử lý độc lập trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả những người tham gia vào một mảng`(name, score)`cặp. 
2. Sắp xếp mảng bằng hai phím: 

- Điểm theo thứ tự giảm dần 
- tên theo thứ tự từ điển tăng dần 

Điều này tạo ra thứ tự đầu ra chính xác theo yêu cầu của câu lệnh. 
3. Khởi tạo chỉ mục`i = 0`. 
4. Trong khi`i < n`, tìm phân khúc người chơi tối đa có cùng số điểm. 

Bắt đầu một con trỏ khác`j = i`, sau đó di chuyển`j`về phía trước trong khi người tham gia tiếp theo có cùng số điểm. 
5. Người chơi trong chỉ số`i ... j`chiếm giữ vị trí xếp hạng`i + 1 ... j + 1`. 

Thứ hạng dựa trên 1, do đó chỉ số mảng 0 tương ứng với vị trí 1. 
6. Nếu`i == j`, in:```
place name
```Nếu không, hãy in:```
start-end name
```cho mỗi người tham gia trong nhóm. 
7. Đặt`i = j + 1`và tiếp tục xử lý nhóm điểm tiếp theo. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi người tham gia có điểm cao hơn sẽ xuất hiện sớm hơn trong mảng. Những người tham gia có số điểm bằng nhau sẽ xuất hiện liên tiếp vì họ so sánh ngang nhau về phím sắp xếp chính. 

Đối với một nhóm chiếm chỉ số`i ... j`, chính xác`i`người tham gia có điểm số cao hơn, vì vậy vị trí hợp lệ đầu tiên là`i + 1`. Chính xác`j + 1`người tham gia có điểm ít nhất là cao, vì vậy vị trí chiếm giữ cuối cùng là`j + 1`. 

Mọi thành viên trong nhóm đều nhận được khoảng thời gian như nhau, phù hợp với định nghĩa xếp hạng của giải đấu. Vì mỗi người tham gia đều thuộc chính xác một nhóm điểm liền kề nên thuật toán sẽ chỉ định vị trí chính xác cho tất cả người chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    players = []
    
    for _ in range(n):
        name, score = input().split()
        players.append((name, int(score)))
    
    players.sort(key=lambda x: (-x[1], x[0]))
    
    ans = []
    
    i = 0
    
    while i < n:
        j = i
        
        while j + 1 < n and players[j + 1][1] == players[i][1]:
            j += 1
        
        left = i + 1
        right = j + 1
        
        for k in range(i, j + 1):
            name = players[k][0]
            
            if left == right:
                ans.append(f"{left} {name}")
            else:
                ans.append(f"{left}-{right} {name}")
        
        i = j + 1
    
    print("\n".join(ans))

solve()
```Giai đoạn đầu tiên đọc tất cả những người tham gia vào danh sách các cặp. Việc sử dụng bộ dữ liệu sẽ giữ cả tên và điểm cùng nhau trong quá trình sắp xếp và xử lý. 

Phím sắp xếp đáng được quan tâm. Chúng ta muốn điểm cao hơn trước nên điểm bị phủ nhận. Tên vẫn theo thứ tự tăng dần bình thường, tự động giải quyết các mối quan hệ theo từ điển. 

Vòng lặp chính xử lý một nhóm điểm tại một thời điểm. Con trỏ`i`đánh dấu sự bắt đầu của nhóm hiện tại, trong khi`j`mở rộng đến người tham gia cuối cùng có cùng điểm số. 

Khoảng xếp hạng đến trực tiếp từ các chỉ số. Vì thứ hạng bắt đầu từ 1 thay vì 0 nên cả hai ranh giới đều thêm một vào chỉ số. 

điều kiện`left == right`phân biệt các cấp bậc duy nhất với các phạm vi được chia sẻ. Nếu không có kiểm tra này, một người chơi sẽ nhận được kết quả đầu ra không chính xác như`3-3 alice`. 

Cuối cùng,`i = j + 1`nhảy trực tiếp đến nhóm điểm chưa được xử lý tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
vasya 10
ted 11
petya 10
katya 33
mike 44
```Sau khi sắp xếp: 

| Chỉ mục | Tên | Điểm | 
| --- | --- | --- | 
| 0 | mike | 44 | 
| 1 | katya | 33 | 
| 2 | ted | 11 | 
| 3 | petya | 10 | 
| 4 | vasya | 10 | 

Xử lý dấu vết: 

| tôi | j | Nhóm Điểm | Khoảng xếp hạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | mike | 1 | 1 micro | 
| 1 | 1 | katya | 2 | 2 katya | 
| 2 | 2 | ted | 3 | 3 ngày | 
| 3 | 4 | petya, vasya | 4-5 | 4-5 petya, 4-5 vasya | 

Đầu ra cuối cùng:```
1 mike
2 katya
3 ted
4-5 petya
4-5 vasya
```Dấu vết này cho thấy các điểm bằng nhau sẽ tự nhiên trở thành một khối liền kề như thế nào sau khi sắp xếp. Hai người chơi cuối cùng chia sẻ vị trí 4 và 5. 

### Ví dụ 2 

đầu vào:```
6
anna 50
bella 50
claire 50
diana 20
eva 10
flora 10
```Sau khi sắp xếp: 

| Chỉ mục | Tên | Điểm | 
| --- | --- | --- | 
| 0 | anna | 50 | 
| 1 | đẹp | 50 | 
| 2 | clare | 50 | 
| 3 | diana | 20 | 
| 4 | eva | 10 | 
| 5 | hệ thực vật | 10 | 

Xử lý dấu vết: 

| tôi | j | Nhóm Điểm | Khoảng xếp hạng | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | Anna, Bella, Claire | 1-3 | chia sẻ | 
| 3 | 3 | diana | 4 | độc đáo | 
| 4 | 5 | eva, hệ thực vật | 5-6 | chia sẻ | 

Đầu ra cuối cùng:```
1-3 anna
1-3 bella
1-3 claire
4 diana
5-6 eva
5-6 flora
```Ví dụ này thực hiện hai nhóm hòa khác nhau và xác nhận rằng thứ hạng tiếp theo sẽ tiếp tục chính xác sau một khoảng thời gian chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Danh sách người tham gia và danh sách đầu ra lưu trữ tất cả người chơi | 

Chỉ với 100 người tham gia, giải pháp dễ dàng phù hợp với giới hạn. Ngay cả những cách tiếp cận chậm hơn nhiều cũng có thể vượt qua một cách thoải mái, nhưng quá trình quét tuyến tính được nhóm sau khi sắp xếp sẽ rõ ràng và có thể mở rộng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    
    def solve():
        n = int(input())
        
        players = []
        
        for _ in range(n):
            name, score = input().split()
            players.append((name, int(score)))
        
        players.sort(key=lambda x: (-x[1], x[0]))
        
        ans = []
        
        i = 0
        
        while i < n:
            j = i
            
            while j + 1 < n and players[j + 1][1] == players[i][1]:
                j += 1
            
            left = i + 1
            right = j + 1
            
            for k in range(i, j + 1):
                name = players[k][0]
                
                if left == right:
                    ans.append(f"{left} {name}")
                else:
                    ans.append(f"{left}-{right} {name}")
            
            i = j + 1
        
        return "\n".join(ans)
    
    return solve()

# provided sample
assert run(
"""5
vasya 10
ted 11
petya 10
katya 33
mike 44
"""
) == (
"""1 mike
2 katya
3 ted
4-5 petya
4-5 vasya"""
), "sample 1"

# minimum size
assert run(
"""1
alice 100
"""
) == (
"""1 alice"""
), "single participant"

# all equal scores
assert run(
"""3
c 5
a 5
b 5
"""
) == (
"""1-3 a
1-3 b
1-3 c"""
), "all tied"

# tie in the middle
assert run(
"""5
anna 100
bella 90
claire 90
diana 80
eva 70
"""
) == (
"""1 anna
2-3 bella
2-3 claire
4 diana
5 eva"""
), "middle tie"

# tie at the end
assert run(
"""4
a 5
b 4
c 1
d 1
"""
) == (
"""1 a
2 b
3-4 c
3-4 d"""
), "ending tie"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người tham gia đơn lẻ |`1 alice`| Kích thước đầu vào tối thiểu | 
| Tất cả đều có điểm bằng nhau |`1-3 ...`| Toàn bộ bảng chia sẻ một phạm vi xếp hạng | 
| Buộc ở giữa |`2-3 ...`| Tiếp tục đúng đắn sau khi hòa | 
| Buộc ở cuối |`3-4 ...`| Sự đúng đắn từng cái một cho nhóm cuối cùng | 

## Vỏ cạnh 

Hãy xem xét trường hợp mọi người tham gia đều có cùng số điểm:```
4
dan 50
anna 50
mike 50
bob 50
```Sau khi sắp xếp theo từ điển với số điểm bằng nhau:```
anna 50
bob 50
dan 50
mike 50
```Thuật toán bắt đầu với`i = 0`và mở rộng`j`cho đến khi`j = 3`bởi vì mọi điểm số đều phù hợp. Khoảng thời gian trở thành`1-4`, được in cho mọi người tham gia:```
1-4 anna
1-4 bob
1-4 dan
1-4 mike
```Điều này xác nhận rằng một khối điểm lớn được xử lý chính xác. 

Bây giờ hãy xem xét tỷ số hòa ngay từ đầu:```
4
alice 100
bob 100
carol 90
dave 80
```Nhóm đầu tiên chiếm chỉ số`0..1`, vì vậy cả hai người chơi đều nhận được`1-2`. Người tham gia tiếp theo bắt đầu tại chỉ mục`2`, tương ứng với vị trí`3`. 

Đầu ra:```
1-2 alice
1-2 bob
3 carol
4 dave
```Điều này xác nhận rằng thứ hạng tiếp tục sau khi có đủ số lượng nhóm ngang nhau, chứ không phải sau vị trí đầu tiên của nhóm đó. 

Cuối cùng, hãy xem xét thứ tự từ điển bên trong các mối quan hệ:```
3
zack 10
anna 10
mike 10
```Tất cả các điểm đều bằng nhau, vì vậy tên xác định thứ tự:```
anna
mike
zack
```Các đầu ra thuật toán:```
1-3 anna
1-3 mike
1-3 zack
```Nếu không có sự sắp xếp thứ cấp theo tên, thứ tự đầu ra sẽ không chính xác mặc dù bản thân các khoảng xếp hạng là chính xác.
