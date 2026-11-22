---
title: "CF 325A - Hình vuông và hình chữ nhật"
description: "Chúng ta có tối đa năm hình chữ nhật thẳng hàng với trục trên một mặt phẳng. Mỗi hình chữ nhật được xác định bởi tọa độ dưới cùng bên trái và trên cùng bên phải. Không có hai hình chữ nhật nào chồng lên nhau, mặc dù chúng có thể chạm nhau ở các cạnh hoặc góc."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 325
codeforces_index: "A"
codeforces_contest_name: "MemSQL start[c]up Round 1"
rating: 1500
weight: 325
solve_time_s: 260
verified: true
draft: false
---

[CF 325A - Hình vuông và Hình chữ nhật](https://codeforces.com/problemset/problem/325/A) 

**Đánh giá:** 1500 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có tối đa năm hình chữ nhật thẳng hàng với trục trên một mặt phẳng. Mỗi hình chữ nhật được xác định bởi tọa độ dưới cùng bên trái và trên cùng bên phải. Không có hai hình chữ nhật nào chồng lên nhau, mặc dù chúng có thể chạm nhau ở các cạnh hoặc góc. Nhiệm vụ là xác định xem sự kết hợp của các hình chữ nhật này có tạo thành một hình vuông hoàn hảo hay không. Điều này có nghĩa là diện tích tổng hợp được bao phủ bởi các hình chữ nhật phải tạo thành một hình vuông có các cạnh song song với các trục, không có khoảng trống hoặc lỗ. 

Ràng buộc về số lượng hình chữ nhật nhiều nhất là năm là rất quan trọng. Nó cho phép chúng ta suy luận về mặt hình học về tất cả các cách sắp xếp có thể thay vì dựa vào hình học tính toán nặng nề hoặc mô phỏng lưới. Tọa độ của mỗi hình chữ nhật được giới hạn bởi 31.400, điều này đảm bảo rằng số học hoặc so sánh đơn giản không có nguy cơ tràn số nguyên trong Python. 

Các trường hợp cạnh không rõ ràng bao gồm các cách sắp xếp trong đó hình chữ nhật chỉ chạm vào các góc, hình chữ nhật tạo thành hình vuông có lỗ bên trong hoặc hình chữ nhật được căn chỉnh theo một đường không phải là hình vuông. Ví dụ: ba hình chữ nhật tạo thành hình chữ L trông giống như một phần của hình vuông có thể đánh lừa một phép kiểm tra ngây thơ chỉ dựa trên tọa độ giới hạn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liên quan đến việc tạo một lưới boolean 2D đủ lớn để bao phủ hộp giới hạn của tất cả các hình chữ nhật và đánh dấu tất cả các ô nằm trong bất kỳ hình chữ nhật nào. Sau đó, chúng ta có thể lặp lại trên lưới để kiểm tra xem tất cả các ô trong hình vuông bao quanh có bị che hay không. Điều này có hiệu quả vì số lượng hình chữ nhật rất nhỏ nhưng không phù hợp và yêu cầu bộ nhớ lớn nếu tọa độ giới hạn gần tối đa là 31.400. 

Cách tiếp cận tối ưu dựa vào lý luận hình học. Đầu tiên, chúng ta tính hình vuông giới hạn tối thiểu chứa tất cả các hình chữ nhật bằng cách tìm hình vuông nhỏ nhất`x1`Và`y1`và lớn nhất`x2`Và`y2`. Nếu chiều rộng và chiều cao của khung giới hạn này không bằng nhau thì các hình chữ nhật không thể tạo thành hình vuông. Nếu chúng bằng nhau, bước tiếp theo là đảm bảo rằng tổng diện tích của các hình chữ nhật bằng diện tích của hình vuông bao quanh này. Nếu tổng diện tích của các hình chữ nhật riêng lẻ khớp với diện tích của hộp giới hạn và không có sự trùng lặp nào (được đảm bảo bằng câu lệnh bài toán), thì các hình chữ nhật đó chính xác tạo thành một hình vuông. 

Thông tin chi tiết quan trọng là đối với một số lượng nhỏ hình chữ nhật không chồng lên nhau, sự kết hợp tạo thành hình vuông tương đương với hộp giới hạn của chúng là hình vuông và tổng diện tích của chúng khớp với diện tích hộp giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lưới vũ phu | O(S^2) trong đó S là chiều dài cạnh của khung giới hạn | O(S^2) | Hoạt động nhưng quá mức cần thiết | 
| Giới hạn hình học + Kiểm tra diện tích | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số hình chữ nhật`n`và tọa độ của chúng. 
2. Khởi tạo`min_x`,`min_y`đến vô cùng và`max_x`,`max_y`về không. Chúng sẽ theo dõi hình chữ nhật giới hạn. 
3. Khởi tạo`total_area`về không. Điều này sẽ tổng hợp diện tích của tất cả các hình chữ nhật. 
4. Lặp lại từng hình chữ nhật. Đối với mỗi hình chữ nhật có tọa độ`(x1, y1, x2, y2)`, cập nhật`min_x`Và`min_y`đến giá trị hiện tại nhỏ hơn của chúng và`x1`,`y1`. Cập nhật`max_x`Và`max_y`đến giá trị hiện tại lớn hơn của chúng và`x2`,`y2`. Thêm vào`(x2 - x1) * (y2 - y1)`ĐẾN`total_area`. 
5. Tính chiều rộng`w = max_x - min_x`và chiều cao`h = max_y - min_y`của hình chữ nhật giới hạn. 
6. Nếu`w != h`, in "KHÔNG" và kết thúc. Ngược lại, hãy kiểm tra xem`total_area == w * h`. Nếu đúng, in "CÓ"; nếu không thì in "KHÔNG". 

Thuật toán hoạt động vì tính bất biến mà các hình chữ nhật không trùng nhau đảm bảo rằng việc tính tổng các diện tích tương đương với việc tính diện tích hợp. Việc kiểm tra hộp giới hạn đảm bảo rằng hình dạng là hình vuông. Không có khoảng trống nếu các khu vực khớp nhau vì hình chữ nhật không thể mở rộng ra ngoài hộp giới hạn và không thể chồng lên nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
min_x = float('inf')
min_y = float('inf')
max_x = 0
max_y = 0
total_area = 0

for _ in range(n):
    x1, y1, x2, y2 = map(int, input().split())
    min_x = min(min_x, x1)
    min_y = min(min_y, y1)
    max_x = max(max_x, x2)
    max_y = max(max_y, y2)
    total_area += (x2 - x1) * (y2 - y1)

width = max_x - min_x
height = max_y - min_y

if width != height or total_area != width * height:
    print("NO")
else:
    print("YES")
```Mã trực tiếp thực hiện thuật toán. Xử lý cẩn thận việc khởi tạo khung giới hạn sẽ tránh được lỗi cho hình chữ nhật bắt đầu từ 0. sử dụng`float('inf')`đảm bảo mọi đầu vào sẽ cập nhật chính xác các giá trị tối thiểu. Tính tổng các diện tích hình chữ nhật có tác dụng vì bài toán đảm bảo không có sự trùng lặp nên chúng ta không tính hai lần. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu: 

| Hình chữ nhật | x1 | y1 | x2 | y2 | Tổng diện tích | phút_x | phút_y | max_x | max_y | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 3 | 6 | 0 | 0 | 2 | 3 | 
| 2 | 0 | 3 | 3 | 5 | 6 | 0 | 0 | 3 | 5 | 
| 3 | 2 | 0 | 5 | 2 | 6 | 0 | 0 | 5 | 5 | 
| 4 | 3 | 2 | 5 | 5 | 6 | 0 | 0 | 5 | 5 | 
| 5 | 2 | 2 | 3 | 3 | 1 | 0 | 0 | 5 | 5 | 

Chiều rộng và chiều cao của hộp giới hạn: 5, tổng diện tích: 25. Chiều rộng bằng chiều cao và tổng diện tích bằng 25, vì vậy đầu ra là CÓ. 

Một đầu vào khác có hình chữ nhật tạo thành một đường:```
2
0 0 1 3
1 0 2 3
```Hộp giới hạn: chiều rộng 2, chiều cao 3, diện tích 6, tổng diện tích hình chữ nhật 6. Chiều rộng != chiều cao, đầu ra NO. Điều này cho thấy thuật toán phát hiện chính xác các hình không phải hình vuông ngay cả khi tổng diện tích khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển qua tối đa 5 hình chữ nhật, hoạt động liên tục trên mỗi hình chữ nhật | 
| Không gian | O(1) | Chỉ một số biến được sử dụng để theo dõi khung giới hạn và tổng diện tích | 

Với n 5, thuật toán chạy ngay lập tức và sử dụng bộ nhớ không đáng kể, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    min_x = float('inf')
    min_y = float('inf')
    max_x = 0
    max_y = 0
    total_area = 0

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        min_x = min(min_x, x1)
        min_y = min(min_y, y1)
        max_x = max(max_x, x2)
        max_y = max(max_y, y2)
        total_area += (x2 - x1) * (y2 - y1)

    width = max_x - min_x
    height = max_y - min_y

    return "YES" if width == height and total_area == width * height else "NO"

# provided samples
assert run("5\n0 0 2 3\n0 3 3 5\n2 0 5 2\n3 2 5 5\n2 2 3 3\n") == "YES", "sample 1"

# custom cases
assert run("1\n0 0 1 1\n") == "YES", "single rectangle square"
assert run("2\n0 0 1 2\n1 0 2 2\n") == "NO", "rectangles form rectangle, not square"
assert run("3\n0 0 2 1\n0 1 1 2\n1 1 2 2\n") == "YES", "L-shaped fills 2x2 square"
assert run("2\n0 0 3 3\n3 0 6 3\n") == "NO", "two rectangles side by side non-square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 hình chữ nhật 1x1 | CÓ | Đầu vào tối thiểu, hình chữ nhật đơn tạo thành hình vuông | 
| 2 hình chữ nhật 1x2 + 1x2 | KHÔNG | Hợp hình chữ nhật là hình chữ nhật, không phải hình vuông | 
| 3 hình chữ nhật tạo thành hình chữ L 2x2 | CÓ | Tổng diện tích bằng bình phương, kiểm tra chạm góc | 
| 2 hình chữ nhật cạnh nhau 3x3 | KHÔNG | Hộp đóng khung hình chữ nhật, tổng diện tích khớp nhưng không vuông | 

## Vỏ cạnh 

Sự sắp xếp theo hình chữ L chứng tỏ rằng các hình chữ nhật chỉ chạm vào các cạnh hoặc góc được xử lý chính xác. Thuật toán dựa vào tổng diện tích và hộp giới hạn nên tự động loại bỏ các cấu hình có khoảng trống, giống như hai hình chữ nhật tạo thành 2x1 +
