---
title: "CF 35B - Kho hàng"
description: "Chúng tôi được yêu cầu mô phỏng một nhà kho có hệ thống giá đỡ dạng lưới. Mỗi kệ có m phần và có n kệ xếp từ trên xuống dưới. Mỗi phần có thể chứa tối đa một hộp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 35
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 35 (Div. 2)"
rating: 1700
weight: 35
solve_time_s: 81
verified: true
draft: false
---
[CF 35B - Kho hàng](https://codeforces.com/problemset/problem/35/B) 

**Đánh giá:** 1700 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một nhà kho có hệ thống giá đỡ dạng lưới. Mỗi kệ có`m`các phần, và có`n`kệ xếp chồng lên nhau từ trên xuống dưới. Mỗi phần có thể chứa tối đa một hộp. Chúng tôi nhận được một chuỗi các thao tác: một số thêm các hộp vào một giá và phần nhất định, còn một số khác thì loại bỏ các hộp theo mã định danh. Các hộp không thể chồng lên nhau. Nếu phần dự định đã đầy, hộp sẽ được đặt vào phần có sẵn tiếp theo ở bên phải trên cùng một giá. Nếu không còn phần nào, chúng ta tiếp tục đến kệ tiếp theo bên dưới, bắt đầu từ phần ngoài cùng bên trái. Nếu không có phần nào còn trống trên tất cả các giá thì hộp sẽ bị loại bỏ. 

Đối với mỗi thao tác lấy ra, chúng ta phải xuất ra chính xác kệ và phần của hộp đã chiếm hoặc`-1 -1`nếu nó không có trong kho. 

Các hạn chế là nhỏ:`n`Và`m`tối đa là 30 và số lượng hoạt động`k`lên đến 2000. Điều này cho phép mô phỏng trực tiếp. Trong trường hợp xấu nhất, mỗi vị trí có thể yêu cầu quét tất cả`n * m`các vị trí. Điều đó mang lại`2000 * 900 = 1.8 * 10^6`hoạt động, có thể chấp nhận được trong giới hạn 2 giây. Các trường hợp khó khăn không rõ ràng liên quan đến việc cố gắng đặt các hộp vào toàn bộ kệ hoặc loại bỏ một hộp chưa bao giờ được lắp vào. 

Ví dụ: nếu chúng ta cố gắng đặt một hộp trong phần`(1, 2)`nhưng nó đầy và toàn bộ kệ 1 đầy thì hộp nên vào phần`(2, 1)`. Nếu tất cả các kệ đã đầy thì hộp sẽ bị bỏ qua. Một cách tiếp cận ngây thơ mà quên tiếp tục ở kệ tiếp theo sẽ tạo ra vị trí không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì lưới 2D đại diện cho nhà kho. Mỗi ô lưu trữ mã định danh hộp hoặc trống. Để chèn`+1 x y id`, cố gắng đặt hộp ở`(x, y)`. Nếu bị chiếm dụng, hãy quét phần còn lại của hàng hiện tại và sau đó là các hàng tiếp theo theo thứ tự cho đến khi tìm thấy một ô trống hoặc tất cả các giá đã hết. Để loại bỏ`-1 id`, tìm kiếm toàn bộ lưới để xác định vị trí hộp và xóa ô của nó. 

Điều này đúng nhưng không hiệu quả đối với`-1`truy vấn vì mỗi lần tìm kiếm có thể mất`O(n*m)`thời gian, dẫn đến`O(k * n * m)`tổng thể. Với những hạn chế, tìm kiếm đơn giản này vẫn có thể hoạt động nhưng có thể tránh được. 

Thông tin chi tiết quan trọng cho một giải pháp hiệu quả là mỗi hộp đều có một mã định danh duy nhất và chúng tôi chỉ cần trả lời các yêu cầu xóa một cách hiệu quả. Bằng cách duy trì các mã nhận dạng hộp ánh xạ từ điển tới vị trí của chúng, chúng ta có thể truy xuất các vị trí theo thời gian không đổi. Logic vị trí vẫn sử dụng lưới nhưng thao tác loại bỏ không còn yêu cầu quét nữa. Điều này làm giảm thời gian dành cho việc xóa khỏi`O(n*m)`ĐẾN`O(1)`mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * n * m) | O(n*m) | Hoạt động nhưng chậm ở giới hạn trên | 
| Tối ưu | O(k * n * m) | O(n*m + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`warehouse`kích thước`n x m`để lưu trữ số nhận dạng hộp hoặc`None`cho các phần trống. Tạo một từ điển`positions`ánh xạ số nhận dạng hộp tới hiện tại của chúng`(shelf, section)`tọa độ. 
2. Đối với mỗi truy vấn, hãy phân tích thao tác. Nếu nó là`+1 x y id`, cố gắng đặt hộp bắt đầu từ kệ`x-1`và phần`y-1`. Kiểm tra xem ô có trống không. Nếu vậy, hãy đặt hộp, lưu tọa độ của nó vào`positions`và tiếp tục truy vấn tiếp theo. Nếu bị chiếm đóng, hãy di chuyển ngay dọc theo cùng một kệ. Nếu đã dùng hết giá, hãy chuyển sang giá tiếp theo bên dưới, quét từ phần ngoài cùng bên trái. Lặp lại cho đến khi tìm thấy một phần trống hoặc tất cả các kệ đã hết. Nếu không tìm thấy khoảng trống, hãy bỏ qua ô này. 
3. Đối với`-1 id`, kiểm tra xem`id`tồn tại ở`positions`. Nếu vậy hãy lấy lại`(shelf, section)`, xuất ra các chỉ số dựa trên 1, xóa mục nhập khỏi`positions`và đánh dấu ô trong`warehouse`trống rỗng. Nếu không có thì xuất ra`-1 -1`. 
4. Lặp lại cho tất cả các truy vấn. 

Lý do tính năng này hoạt động là vì lưới kho thể hiện chính xác tình trạng sử dụng theo thời gian thực và từ điển đảm bảo truy xuất liên tục theo thời gian để loại bỏ. Thứ tự quét tuân thủ nghiêm ngặt các quy tắc sắp xếp của bài toán, đảm bảo đúng vị trí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
warehouse = [[None for _ in range(m)] for _ in range(n)]
positions = {}

for _ in range(k):
    parts = input().split()
    if parts[0] == '+1':
        x, y, box_id = int(parts[1]) - 1, int(parts[2]) - 1, parts[3]
        placed = False
        for i in range(x, n):
            start_j = y if i == x else 0
            for j in range(start_j, m):
                if warehouse[i][j] is None:
                    warehouse[i][j] = box_id
                    positions[box_id] = (i, j)
                    placed = True
                    break
            if placed:
                break
    else:
        box_id = parts[1]
        if box_id in positions:
            i, j = positions.pop(box_id)
            print(i + 1, j + 1)
            warehouse[i][j] = None
        else:
            print(-1, -1)
```Chúng tôi duy trì lưới cho vị trí vì quá trình quét rất đơn giản và an toàn. Từ điển đảm bảo việc loại bỏ là O(1). Chúng tôi xử lý cẩn thận các chỉ số dựa trên 0 và 1 để phù hợp với kỳ vọng đầu ra. Chúng tôi cũng đặt lại quá trình quét cột khi chuyển sang giá mới. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 2 9
+1 1 1 cola
+1 1 1 fanta
+1 1 1 sevenup
+1 1 1 whitekey
-1 cola
-1 fanta
-1 sevenup
-1 whitekey
-1 cola
```| Bước | Hoạt động | Trạng thái kho hàng | Vị trí | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | +1 1 1 cola | [[cola, Không có],[Không có,Không có]] | {'cola':(0,0)} | | 
| 2 | +1 1 1 tuyệt vời | [[cola, fanta],[Không có,Không có]] | {'cola':(0,0),'fanta':(0,1)} | | 
| 3 | +1 1 1 bảyup | [[cola,fanta],[bảyup,Không có]] | ... | | 
| 4 | +1 1 1 phím trắng | [[cola,fanta],[sevenup,whitekey]] | ... | | 
| 5 | -1 cola | [[Không có,fanta],[bảyup,phím trắng]] | {'fanta':(0,1),...} | 1 1 | 
| 6 | -1 tưởng tượng | [[Không,Không có],[bảyup,phím trắng]] | {'sevenup':(1,0),'whitekey':(1,1)} | 1 2 | 
| 7 | -1 bảy lần | [[Không có,Không có],[Không có,phím trắng]] | {'whitekey':(1,1)} | 2 1 | 
| 8 | -1 phím trắng | [[Không,Không],[Không,Không]] | {} | 2 2 | 
| 9 | -1 cola | [[Không,Không],[Không,Không]] | {} | -1 -1 | 

Dấu vết này xác nhận thứ tự vị trí và tra cứu từ điển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * n * m) | Mỗi vị trí có thể quét tất cả các phần trong trường hợp xấu nhất, số lần loại bỏ là O(1). | 
| Không gian | O(n*m + k) | Lưới kho + từ điển cho tối đa k số nhận dạng hộp. | 

Với`n, m <= 30`Và`k <= 2000`, điều này thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n, m, k = map(int, input().split())
    warehouse = [[None]*m for _ in range(n)]
    positions = {}
    for _ in range(k):
        parts = input().split()
        if parts[0] == '+1':
            x, y, box_id = int(parts[1]) - 1, int(parts[2]) - 1, parts[3]
            placed = False
            for i in range(x, n):
                start_j = y if i == x else 0
                for j in range(start_j, m):
                    if warehouse[i][j] is None:
                        warehouse[i][j] = box_id
                        positions[box_id] = (i,j)
                        placed = True
                        break
                if placed:
                    break
        else:
            box_id = parts[1]
            if box_id in positions:
                i,j = positions.pop(box_id)
                print(i+1,j+1)
                warehouse[i][j] = None
            else:
                print(-1,-1)
    return output.getvalue().strip()
```
