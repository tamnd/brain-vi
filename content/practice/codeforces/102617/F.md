---
title: "CF 102617F - Chảo Nướng"
description: "Vấn đề mô tả một bộ bánh quy hình tròn phải vừa với một chiếc chảo nướng hình chữ nhật. Mỗi cookie có tâm trên mặt phẳng tọa độ và bán kính."
date: "2026-08-01T07:11:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102617
codeforces_index: "F"
codeforces_contest_name: "mBIT Rookie November 2019"
rating: 0
weight: 102617
solve_time_s: 57
verified: true
draft: false
---

[CF 102617F - Chảo nướng](https://codeforces.com/problemset/problem/102617/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một bộ bánh quy hình tròn phải vừa với một chiếc chảo nướng hình chữ nhật. Mỗi cookie có tâm trên mặt phẳng tọa độ và bán kính. Cái chảo phải có các cạnh song song với trục tọa độ và chúng ta cần diện tích nhỏ nhất có thể có của hình chữ nhật chứa hoàn toàn mọi cookie. 

Một chiếc bánh quy có trung tâm`(x, y)`và bán kính`r`đạt tới`r`các đơn vị trái, phải, lên và xuống từ trung tâm của nó. Điều đó có nghĩa là hình chữ nhật nhỏ nhất chứa một cookie có phạm vi ngang`[x - r, x + r]`và phạm vi dọc`[y - r, y + r]`. Câu trả lời là diện tích hình chữ nhật bao phủ tất cả các phạm vi riêng lẻ này. 

Kích thước đầu vào có thể đạt tới`100000`cookie, do đó thuật toán phải xử lý mỗi cookie một số lần không đổi. Phương pháp bậc hai so sánh từng cặp cookie sẽ hoạt động khoảng`10^10`hoạt động trong trường hợp lớn nhất, vượt xa giới hạn thời gian cuộc thi thông thường cho phép. Các ràng buộc hướng tới việc quét tuyến tính. 

Các trường hợp chính xảy ra do quên rằng chảo phải chứa toàn bộ chiếc bánh quy chứ không chỉ phần giữa của nó. Một cookie duy nhất có thể xác định câu trả lời. Ví dụ:```
1
0 0 5
```Đầu ra đúng là:```
100
```Chảo phải có chiều rộng`10`và chiều cao`10`. Cách tiếp cận chỉ theo dõi trung tâm sẽ trả về khu vực không chính xác`0`. 

Một lỗi phổ biến khác là xử lý tọa độ âm không chính xác. Coi như:```
2
-10 -5 2
4 3 1
```Các giới hạn theo chiều ngang là`[-12, -8]`Và`[3, 5]`, vậy tổng chiều rộng là`17`. Các giới hạn theo chiều dọc là`[-7, -3]`Và`[2, 4]`, vậy chiều cao là`11`. Đầu ra đúng là:```
187
```Việc triển khai bất cẩn chỉ sử dụng các giá trị dương hoặc áp dụng các giá trị tuyệt đối cho tọa độ có thể làm mất vị trí tối thiểu và tối đa thực sự. 

Trường hợp ranh giới cuối cùng là khi tất cả các cookie trùng nhau:```
3
0 0 1
0 0 1
0 0 1
```Đầu ra đúng là:```
4
```Chảo không cần phải nở ra để làm bánh quy lặp đi lặp lại. Chỉ có những biên giới cực đoan mới quan trọng. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử các hình chữ nhật có thể có và kiểm tra xem mọi cookie có vừa với mỗi hình chữ nhật hay không. Điều này đúng vì cuối cùng hình chữ nhật tối ưu sẽ được xem xét, nhưng không có cách nào hữu ích để liệt kê tất cả các hình chữ nhật có thể có. Số lượng tọa độ có thể tăng lên theo giá trị đầu vào, khiến cách tiếp cận này không thể thực hiện được ngay cả đối với số lượng cookie vừa phải. 

Một cách giải thích bạo lực tự nhiên hơn là tìm ranh giới cookie ngoài cùng bên trái, ngoài cùng bên phải, thấp nhất và cao nhất bằng cách so sánh liên tục từng cookie với mọi cookie khác. Cách tiếp cận đó có hiệu quả vì câu trả lời chỉ phụ thuộc vào các giá trị cực trị. Tuy nhiên, nếu có`n`cookie và chúng tôi liên tục tìm kiếm các thái cực, công việc trở nên`O(n^2)`, có nghĩa là về`10^10`so sánh cho`n = 100000`. 

Quan sát quan trọng là hình chữ nhật cuối cùng được xác định hoàn toàn bởi bốn con số. Vế trái là giá trị nhỏ nhất của`x - r`, vế phải là giá trị lớn nhất của`x + r`, cạnh dưới là giá trị nhỏ nhất của`y - r`và cạnh trên là giá trị lớn nhất của`y + r`. Mỗi cookie có thể cập nhật bốn giá trị này một cách độc lập. 

Phương pháp vũ lực có hiệu quả vì cuối cùng nó tìm thấy bốn ranh giới này, nhưng nó lặp lại những so sánh tương tự nhiều lần. Nhận xét rằng mỗi cookie chỉ đóng góp bốn đường viền ứng cử viên cho phép chúng tôi thay thế việc tìm kiếm lặp đi lặp lại bằng một lượt duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến đại diện cho ranh giới pan hiện tại. Ranh giới bên trái và dưới cùng bắt đầu ở vô cực dương vì chúng ta đang tìm kiếm các giá trị nhỏ hơn. Ranh giới bên phải và trên cùng bắt đầu ở âm vô cực vì chúng ta đang tìm kiếm các giá trị lớn hơn. 
2. Đọc từng cookie và tính bốn tọa độ cực trị của nó:`x - r`,`x + r`,`y - r`, Và`y + r`. Đây là những giá trị duy nhất có thể ảnh hưởng đến chảo cuối cùng. 
3. Cập nhật ranh giới hiện tại bằng bốn giá trị này. Cạnh trái nhỏ nhất và cạnh dưới trở thành giới hạn dưới mới, trong khi cạnh phải lớn nhất và cạnh trên trở thành giới hạn trên mới. 
4. Sau khi tất cả cookie được xử lý, hãy tính chiều rộng như sau`right - left`và chiều cao như`top - bottom`. Sản phẩm của họ là diện tích chảo nhỏ nhất có thể. 

Tại sao nó hoạt động: sau khi xử lý bất kỳ tiền tố nào của cookie, bốn ranh giới được lưu trữ mô tả chính xác hình chữ nhật nhỏ nhất chứa tất cả các cookie được thấy cho đến nay. Khi một cookie mới được thêm vào, thay đổi duy nhất có thể xảy ra là nó mở rộng một trong bốn mặt này. Vì mọi cookie đều được xử lý và tất cả các phần mở rộng có thể có đều được xem xét nên ranh giới cuối cùng sẽ chứa mọi cookie và không thể giảm thêm nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    
    left = float('inf')
    right = -float('inf')
    bottom = float('inf')
    top = -float('inf')
    
    for _ in range(n):
        x, y, r = map(int, input().split())
        
        left = min(left, x - r)
        right = max(right, x + r)
        bottom = min(bottom, y - r)
        top = max(top, y + r)
    
    width = right - left
    height = top - bottom
    
    print(width * height)

if __name__ == "__main__":
    solve()
```Mã chỉ giữ lại bốn đường viền của câu trả lời hiện tại. Các giá trị`x - r`Và`x + r`mô tả các điểm bên trái và bên phải của cookie, trong khi`y - r`Và`y + r`mô tả các điểm dưới cùng và trên cùng. 

sử dụng`float('inf')`và vô cực âm tránh cần xử lý đặc biệt cho cookie đầu tiên. Sau đó, mọi giá trị đầu vào sẽ được xử lý thông qua cùng một logic cập nhật. 

Số nguyên Python có độ chính xác tùy ý, do đó phép nhân cuối cùng không yêu cầu xử lý tràn đặc biệt. Việc tính toán chiều rộng và chiều cao phải được thực hiện sau tất cả các lần cập nhật vì diện tích phụ thuộc vào bộ cookie hoàn chỉnh chứ không phải bất kỳ cookie riêng lẻ nào. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
4
1 1 5
2 -4 3
-5 2 6
-8 -1 4
```Dấu vết là: 

| Bánh quy | Trái | Đúng | Dưới cùng | Đầu trang | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | thông tin | -inf | thông tin | -inf | 
| (1, 1, 5) | -4 | 6 | -4 | 6 | 
| (2, -4, 3) | -4 | 6 | -7 | 6 | 
| (-5, 2, 6) | -11 | 6 | -7 | 8 | 
| (-8, -1, 4) | -12 | 6 | -7 | 8 | 

Chiều rộng cuối cùng là`18`và chiều cao cuối cùng là`15`, cho diện tích`270`. Ví dụ này cho thấy các loại bánh quy khác nhau có thể xác định các mặt khác nhau của chảo như thế nào. 

Một trường hợp cookie duy nhất:```
1
0 0 5
```| Bánh quy | Trái | Đúng | Dưới cùng | Đầu trang | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | thông tin | -inf | thông tin | -inf | 
| (0, 0, 5) | -5 | 5 | -5 | 5 | 

Chiều rộng và chiều cao đều là`10`, vậy câu trả lời là`100`. Điều này xác nhận rằng thuật toán xử lý đầu vào nhỏ nhất có thể mà không có trường hợp đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cookie cập nhật bốn ranh giới một lần. | 
| Không gian | O(1) | Chỉ có bốn biến biên được lưu trữ. | 

Thuật toán chia tỷ lệ trực tiếp với số lượng cookie. Vì`100000`cookie, nó chỉ thực hiện vài trăm nghìn phép tính số học, dễ dàng phù hợp với giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    data = inp.strip().split()
    if not data:
        return ""
    
    it = iter(data)
    n = int(next(it))
    
    left = float('inf')
    right = -float('inf')
    bottom = float('inf')
    top = -float('inf')
    
    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        r = int(next(it))
        left = min(left, x - r)
        right = max(right, x + r)
        bottom = min(bottom, y - r)
        top = max(top, y + r)
    
    return str((right - left) * (top - bottom))

assert solution("""4
1 1 5
2 -4 3
-5 2 6
-8 -1 4
""") == "270", "sample"

assert solution("""1
0 0 5
""") == "100", "single cookie"

assert solution("""3
0 0 1
0 0 1
0 0 1
""") == "4", "all equal cookies"

assert solution("""2
-10 -5 2
4 3 1
""") == "187", "negative coordinates"

assert solution("""2
10000000 10000000 10000000
-10000000 -10000000 10000000
""") == "1600000000000000", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một chiếc bánh quy có bán kính 5 | 100 | Hình chữ nhật phải bao gồm toàn bộ cookie. | 
| Ba cookie giống hệt nhau | 4 | Cookie lặp đi lặp lại không thay đổi đường viền. | 
| Cookie có tọa độ âm | 187 | Xử lý đúng các giá trị tối thiểu và tối đa. | 
| Tọa độ và bán kính rất lớn | 1600000000000000 | Giá trị số học lớn và an toàn tràn. | 

## Vỏ cạnh 

Đối với trường hợp cookie đơn:```
1
0 0 5
```thuật toán bắt đầu với các ranh giới trống. Chiếc bánh quy duy nhất thay đổi cả bốn mặt thành`-5`Và`5`. Hình chữ nhật thu được có diện tích`100`, đó là chảo nhỏ nhất có thể. 

Đối với tọa độ âm:```
2
-10 -5 2
4 3 1
```cookie đầu tiên đóng góp cạnh trái`-12`và cạnh dưới`-7`. Cookie thứ hai đóng góp cạnh phải`5`và cạnh trên`4`. Hình chữ nhật cuối cùng là`17`đơn vị rộng và`11`cao đơn vị, vậy câu trả lời là`187`. Thuật toán không xử lý các vị trí phủ định một cách đặc biệt vì phép so sánh xử lý chúng một cách tự nhiên. 

Đối với các cookie chồng chéo:```
3
0 0 1
0 0 1
0 0 1
```mọi cookie đều tạo ra các đường viền giống nhau. Các phép toán tối thiểu và tối đa không thay đổi hình chữ nhật sau cookie đầu tiên, đưa ra câu trả lời đúng về`4`. Điều này cho thấy tại sao giải pháp phụ thuộc vào các mức cực đoan hơn là số lượng cookie.
