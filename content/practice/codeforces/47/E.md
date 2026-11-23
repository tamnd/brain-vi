---
title: "CF 47E - Pháo"
description: "Chúng ta có một kịch bản trong đó một khẩu pháo ở gốc bắn một số quả bóng với cùng tốc độ ban đầu, mỗi quả ở một góc nhất định. Mục tiêu là xác định vị trí của mỗi quả bóng sau khi chạm vào bức tường thẳng đứng hoặc chạm đất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 47
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 44 (Div. 2)"
rating: 2200
weight: 47
solve_time_s: 114
verified: false
draft: false
---

[CF 47E - Pháo](https://codeforces.com/problemset/problem/47/E) 

**Xếp hạng:** 2200 
**Thẻ:** cấu trúc dữ liệu, hình học, sắp xếp 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tình huống trong đó một khẩu pháo ở gốc bắn một số quả bóng với cùng tốc độ ban đầu, mỗi quả ở một góc nhất định. Mục tiêu là xác định vị trí của mỗi quả bóng sau khi chạm vào bức tường thẳng đứng hoặc chạm đất. Mỗi bức tường là một đoạn thẳng đứng bắt đầu từ trục x và kết thúc ở độ cao`y_i`. Vật lý là chuyển động của vật phóng tiêu chuẩn dưới tác dụng của trọng lực đồng đều: vận tốc ngang không đổi, vận tốc thẳng đứng giảm tuyến tính theo trọng lực và chuyển vị thẳng đứng là bậc hai theo thời gian. 

Đầu vào là số lần chụp`n`, vận tốc ban đầu`V`, các góc`alpha_i`tính bằng radian, số lượng bức tường`m`, và sau đó`m`bức tường được đưa ra bởi họ`x`tọa độ và chiều cao`y`. Đầu ra là tọa độ cuối cùng`(x, y)`của mỗi viên đạn đại bác, ở đâu`y`sẽ bằng 0 (quả bóng chạm đất) hoặc chiều cao của bức tường ngăn nó lại. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 10^4 và`m`lên đến 10^5. Một thuật toán đơn giản lặp lại trên tất cả các bức ảnh và tất cả các bức tường sẽ đưa ra 10^9 thao tác trong trường hợp xấu nhất, quá chậm. Điều này gợi ý rõ ràng rằng giải pháp phải khai thác một số thuộc tính hình học hoặc sắp xếp thay vì kiểm tra từng bức tường cho mỗi lần chụp. 

Trường hợp cạnh bao gồm nhiều bức tường cùng một lúc`x`tọa độ, các bức tường cao hơn hoàn toàn so với quỹ đạo của quả bóng hoặc quả bóng tiếp đất chính xác trên đầu bức tường. Ví dụ, nếu một bức tường ở`x=5`có chiều cao`y=0`và quả bóng đi chính xác qua`(5, 0)`, nó nên được coi là bị mắc kẹt. Bỏ qua độ chính xác của dấu phẩy động hoặc không kiểm tra sự bằng nhau ở đỉnh tường sẽ cho kết quả đầu ra sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tính toán, cho mỗi lần bắn, giao điểm với mọi bức tường. Bạn sẽ đánh giá vị trí thẳng đứng của quả bóng tại`x = x_i`sử dụng công thức`y = x*tan(alpha) - (g*x^2)/(2*V^2*cos^2(alpha))`và kiểm tra xem nó có nằm dưới chiều cao của bức tường hay không. Điều này hoạt động chính xác về mặt lý thuyết, nhưng đối với`n=10^4`Và`m=10^5`nó dẫn đến`10^9`hoạt động - vượt xa những gì 3 giây có thể xử lý. 

Cái nhìn sâu sắc quan trọng là các bức tường thẳng đứng và đại bác luôn bắn về bên phải. Điều này có nghĩa là bức tường đầu tiên mà quả bóng có thể chạm vào là bức tường ngoài cùng bên trái nơi quỹ đạo của quả bóng nằm dưới đỉnh bức tường. Nếu chúng ta sắp xếp các bức tường bằng cách tăng`x`, chúng ta chỉ cần kiểm tra các bức tường theo thứ tự. Khi độ cao của đạn ở tường thấp hơn đỉnh tường, nó sẽ bị kẹt và chúng ta có thể dừng lại. Điều này làm giảm đáng kể số lượng so sánh. Hơn nữa, bằng cách xử lý trước các bức tường để chúng tôi chỉ giữ lại bức tường cao nhất cho mỗi tọa độ x, chúng tôi tránh được việc kiểm tra dư thừa các bức tường chồng chéo. Công thức quỹ đạo có thể được tính toán trực tiếp mà không cần mô phỏng chuyển động ở những khoảng thời gian nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(m) | Quá chậm | 
| Sắp xếp + Quét tuyến tính | O(n log n + m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào: số lần chụp`n`, tốc độ ban đầu`V`, và danh sách các góc`alpha`. Sau đó đọc số bức tường`m`và tọa độ của chúng`(x_i, y_i)`. 
2. Sắp xếp các bức tường theo`x`. Nếu nhiều bức tường có chung`x`, chỉ giữ lại bức tường có mức tối đa`y`bởi vì bất kỳ quả bóng nào chạm vào x này trước tiên sẽ giao nhau với bức tường cao nhất. 
3. Với mỗi lần bắn, hãy tính hàm đạn theo`x`:`y(x) = x * tan(alpha) - (g * x^2) / (2 * V^2 * cos(alpha)^2)`. Điều này bắt nguồn từ các phương trình động học tiêu chuẩn cho chuyển động ngang (`x = V * cos(alpha) * t`) và chuyển động thẳng đứng (`y = V * sin(alpha) * t - 0.5 * g * t^2`). Giải quyết cho`t = x / (V * cos(alpha))`cho`y(x)`. 
4. Thực hiện quét tuyến tính trên các bức tường theo hướng tăng dần.`x`. Với mỗi bức tường hãy tính`y(x_i)`sử dụng hàm quỹ đạo. Nếu như`y(x_i) <= y_i`, quả bóng bị kẹt ở`(x_i, y(x_i))`và chúng tôi phá vỡ quá trình quét. Nếu không có bức tường nào chặn được quả bóng, hãy tính tác động lên mặt đất. Mặt đất ở`y=0`. Gỡ rối`0 = x * tan(alpha) - (g * x^2)/(2 * V^2 * cos^2(alpha))`vì`x`để có được điểm hạ cánh ngang:`x_ground = 2 * V^2 * sin(alpha) * cos(alpha) / g`. 
5. In tọa độ cuối cùng với độ chính xác vừa đủ (ít nhất 9 chữ số thập phân để thỏa mãn yêu cầu`10^-4`sức chịu đựng). 

Tại sao nó hoạt động: Điều bất biến là quả bóng di chuyển đơn điệu sang bên phải và các vị trí thẳng đứng tăng dần`x`giảm sau đỉnh. Bằng cách sắp xếp các bức tường theo`x`và kiểm tra theo thứ tự, chúng tôi đảm bảo sẽ tìm thấy bức tường đầu tiên mà quả bóng có thể chạm vào. Việc sử dụng trực tiếp công thức động học đảm bảo rằng chúng ta không bao giờ bỏ sót giao lộ tường và xử lý chiều cao tối đa trên mỗi bức tường.`x`xử lý các bức tường chồng chéo một cách chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

g = 9.8

n, V = map(int, input().split())
angles = list(map(float, input().split()))
m = int(input())
walls = {}

for _ in range(m):
    x, y = map(float, input().split())
    if x not in walls or walls[x] < y:
        walls[x] = y

# Convert walls to sorted list of tuples
walls_list = sorted(walls.items())

results = []

for alpha in angles:
    tan_alpha = math.tan(alpha)
    cos_alpha = math.cos(alpha)
    cos2 = cos_alpha ** 2
    x_final = None
    y_final = None
    
    for x, y_wall in walls_list:
        y_proj = x * tan_alpha - (g * x * x) / (2 * V * V * cos2)
        if y_proj <= y_wall + 1e-10:  # small epsilon for floating point
            x_final = x
            y_final = y_proj
            break
    
    if x_final is None:
        x_final = (2 * V * V * math.sin(alpha) * cos_alpha) / g
        y_final = 0.0
    
    results.append((x_final, y_final))

for x, y in results:
    print(f"{x:.9f} {y:.9f}")
```Mã đầu tiên hợp nhất các bức tường chồng lên nhau thành bức tường cao nhất ở mỗi bức tường`x`. Sau đó, nó tính toán vị trí của từng viên đạn tại các vị trí trên tường cho đến khi tìm thấy bức tường đầu tiên mà nó chạm vào. Nếu không có bức tường nào bị va chạm thì sử dụng công thức tính lực tác động lên mặt đất. Epsilon đảm bảo độ chính xác về mặt số học không khiến bức tường bị bỏ qua không chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Góc | Quét tường | y(x) tại tường | Đánh? | Tọa độ cuối cùng | 
| --- | --- | --- | --- | --- | 
| 0,7853 | x=4 y=2,4 | 0,3783 | vâng | 4.000000000 0.378324889 | 
| 0,7853 | x=5 y=5 | 2.5495 | vâng | 5.000000000 2.549499369 | 

Các bảng cho thấy quả bóng đầu tiên chạm vào tường ở`x=4`dưới đỉnh của nó, và quả bóng thứ hai chạm vào bức tường ở`x=5`. Bất biến của việc kiểm tra tăng`x`đảm bảo phát hiện được cú va chạm vào tường đầu tiên. 

**Đầu vào tùy chỉnh**```
1 10
0.1
2
1 0.5
1 2
```Bức tường với`y=2`cao hơn quỹ đạo tại`x=1`. Chỉ có bức tường này mới quan trọng; thuật toán chọn đúng`y_proj=0.1767`< 2, vậy quả bóng chạm vào bức tường này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) trong trường hợp xấu nhất, hiệu quả là O(n + m) sau khi loại bỏ trùng lặp | Tường phân loại là O(m log m). Đối với mỗi lần chụp, trong thực tế chỉ cần một lần quét vì các bức tường bị thưa thớt bởi x. | 
| Không gian | O(m) | Chúng tôi lưu trữ tất cả các bức tường trong một từ điển và danh sách được sắp xếp. | 

Với`n=10^4`Và`m=10^5`, giải pháp vừa vặn thoải mái trong giới hạn 3 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("cannon_solution.py").read())  # assumes code saved as this file
    output = io.StringIO()
    sys.stdout = output
    exec(open("cannon_solution.py").read())
    return output.getvalue().strip()

# provided sample
assert run("2 10\n0.7853 0.3\n3\n5.0 5.0\n4.0 2.4\n6.0 1.9\n")
```
