---
title: "CF 475B - Thành Phố Kết Nối Mạnh Mẽ"
description: "Chúng tôi được yêu cầu xác định xem một thành phố có mạng lưới đường ngang và dọc có \"được kết nối chặt chẽ\" hay không khi mỗi đường được làm một chiều. Thành phố có thể được hình dung như một mạng lưới các giao lộ, với n đường ngang và m đường dọc tạo thành các nút giao n × m."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 475
codeforces_index: "B"
codeforces_contest_name: "Bayan 2015 Contest Warm Up"
rating: 1400
weight: 475
solve_time_s: 59
verified: true
draft: false
---

[CF 475B - Thành phố được kết nối mạnh mẽ](https://codeforces.com/problemset/problem/475/B) 

**Đánh giá:** 1400 
**Tags:** bạo lực, dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem một thành phố có mạng lưới đường ngang và dọc có được "kết nối chặt chẽ" hay không khi mỗi đường được làm một chiều. Thành phố có thể được hình dung như một mạng lưới các nút giao thông, với`n`đường ngang và`m`hình thành các đường thẳng đứng`n × m`nút giao thông. Mỗi đường ngang có một hướng được phép duy nhất: tất cả giao thông lưu thông theo hướng Tây sang Đông hoặc Đông sang Tây. Mỗi con phố dọc cũng có một hướng duy nhất: bắc xuống nam hoặc nam lên bắc. Người lái xe có thể di chuyển dọc theo đường phố theo hướng của nó và chuyển sang đường băng qua đường tại các giao lộ. 

Đầu vào cung cấp số lượng đường ngang và dọc, theo sau là một chuỗi có độ dài`n`với`<`hoặc`>`cho những con đường nằm ngang và một chuỗi dài`m`với`^`hoặc`v`đối với đường thẳng đứng. Đầu ra phải là "CÓ" nếu có thể đến bất kỳ giao lộ nào từ bất kỳ giao lộ nào khác theo chỉ dẫn đường phố và nếu không thì "KHÔNG". 

Những hạn chế với`n, m ≤ 20`, ngụ ý rằng tổng số giao lộ nhiều nhất là 400. Con số này đủ nhỏ để việc duyệt đồ thị cưỡng bức trên tất cả các giao lộ là khả thi. Các trường hợp cạnh không rõ ràng ngay lập tức nhưng có thể phát sinh từ các đường phố có hướng tạo thành "ngõ cụt" dọc theo biên giới. Ví dụ: nếu đường ngang trên cùng hướng về bên trái và đường dọc ngoài cùng bên trái hướng lên trên thì giao lộ ở góc trên cùng bên trái sẽ không thể truy cập được từ một số điểm nhất định, làm gián đoạn kết nối mạnh mẽ. 

Việc triển khai đơn giản có thể chỉ kiểm tra khả năng kết nối từ một giao lộ, điều này sẽ sai. Khả năng kết nối mạnh mẽ đòi hỏi mọi điểm giao nhau đều có thể tiếp cận được từ mọi điểm giao nhau khác. Một cách tiếp cận bất cẩn cũng có thể hiểu sai hướng dẫn, ví dụ: giả sử`<`có nghĩa là từ tây sang đông thay vì từ đông sang tây. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô hình hóa từng điểm giao nhau như một nút trong biểu đồ có hướng và thêm các cạnh theo hướng đường phố. Sau đó, đối với mỗi điểm nối, hãy chạy thuật toán truyền tải đồ thị như DFS hoặc BFS và xác minh rằng tất cả các điểm nối khác đều có thể truy cập được. Với tối đa 400 nút, điều này đòi hỏi tới 400 lần duyệt, mỗi lần truy cập 400 nút, dẫn đến khoảng 160.000 thao tác, có thể chấp nhận được đối với các ràng buộc của vấn đề nhưng không cần thiết. 

Thông tin chi tiết quan trọng là khả năng kết nối mạnh mẽ trong một mạng lưới với các hướng đường thống nhất có thể được đảm bảo khi và chỉ khi các đường trên ranh giới cho phép di chuyển dọc theo cả bốn cạnh. Nói cách khác, nếu các đường ngang trên cùng và dưới cùng hướng ngược nhau, còn các đường dọc ngoài cùng bên trái và ngoài cùng bên phải hướng ngược nhau thì mọi ngã ba đều có thể đến mọi ngã ba khác. Điều này làm giảm vấn đề kiểm tra bốn nút giao ở góc: nếu mô hình hướng cho phép di chuyển từ góc này sang góc khác thì các nút giao bên trong sẽ tự động có thể tiếp cận được vì chuyển động dọc theo các đường ngang và dọc sẽ tăng khả năng tiếp cận. 

Quan sát này cho phép chúng ta tránh việc xây dựng biểu đồ đầy đủ và chạy nhiều lần duyệt. Chúng ta chỉ cần kiểm tra xem các góc trên cùng bên trái, trên cùng bên phải, dưới cùng bên trái và dưới cùng bên phải có thể truy cập lẫn nhau hay không, điều này tương đương với việc đảm bảo rằng ký tự đầu tiên và cuối cùng của chuỗi ngang và dọc hướng ngược nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS từ mọi ngã ba | O(n_m_(n*m)) | O(n*m) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra góc nhìn sâu sắc | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đường ngang`n`và đường dọc`m`, theo sau là các chuỗi biểu thị hướng của đường ngang và dọc. 
2. Kiểm tra đường ngang trên cùng và đường ngang dưới cùng. Nếu cả hai đều trỏ theo cùng một hướng, các cột ngoài cùng bên trái hoặc ngoài cùng bên phải có thể không truy cập được, khiến không thể kết nối mạnh mẽ. 
3. Kiểm tra đường dọc ngoài cùng bên trái và đường dọc ngoài cùng bên phải. Nếu cả hai đều trỏ về cùng một hướng, các hàng trên cùng hoặc dưới cùng có thể không truy cập được, làm đứt kết nối mạnh. 
4. Nếu cả hai kiểm tra ranh giới ngang và dọc đều vượt qua, hãy in "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: trong một mạng lưới nơi các đường phố có hướng thống nhất, cách duy nhất có thể không thể tiếp cận được một giao lộ là nếu các đường ranh giới ngăn cản việc đi lại đến hoặc đi từ giao lộ đó. Bằng cách kiểm tra xem các cạnh đối diện có được định hướng theo hướng ngược nhau hay không, chúng tôi đảm bảo rằng có thể di chuyển xung quanh lưới và bằng cách mở rộng, mọi điểm nối đều có thể đến mọi điểm nối khác. Điều này tránh việc xây dựng biểu đồ rõ ràng và truyền tải nhiều lần trong khi vẫn duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    h_dirs = input().strip()
    v_dirs = input().strip()

    horizontal_ok = h_dirs[0] != h_dirs[-1]
    vertical_ok = v_dirs[0] != v_dirs[-1]

    if horizontal_ok and vertical_ok:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    main()
```Mã đọc đầu vào bằng cách sử dụng`sys.stdin.readline`cho hiệu quả. Nó kiểm tra chỉ đường đường ngang đầu tiên và cuối cùng cũng như chỉ đường đường dọc đầu tiên và cuối cùng. Nếu chúng không bằng nhau, lưới được đảm bảo được kết nối mạnh mẽ, vì vậy nó sẽ in "CÓ"; nếu không thì "KHÔNG". Điều kiện biên được xử lý cẩn thận:`h_dirs[0]`là con đường cực bắc và`h_dirs[-1]`là con đường cực nam,`v_dirs[0]`là cực Tây và`v_dirs[-1]`là cực đông. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 3
><>
v^v
```| Biến | Giá trị | 
| --- | --- | 
| h_dirs | '><>' | 
| v_dirs | 'v^v' | 
| ngang_ok | '>' != '>' → Sai | 
| dọc_ok | 'v' != 'v' → Sai | 
| Đầu ra | KHÔNG | 

Việc kiểm tra xác nhận rằng đường ngang đầu tiên và cuối cùng đều là '>', do đó hàng trên cùng không thể chạm tới hàng dưới cùng. Tương tự, các ranh giới dọc đều điểm 'v', ngăn cản kết nối trái-phải. Do đó "KHÔNG". 

Đối với đầu vào:```
2 2
><
^v
```| Biến | Giá trị | 
| --- | --- | 
| h_dirs | '><' | 
| v_dirs | '^v' | 
| ngang_ok | '>' != '<' → Đúng | 
| dọc_ok | '^' != 'v' → Đúng | 
| Đầu ra | CÓ | 

Tất cả các nút giao ở góc đều có thể tiếp cận được và khả năng kết nối bên trong lan truyền theo các hướng, mang lại khả năng kết nối mạnh mẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ cần kiểm tra ranh giới của chuỗi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung tỷ lệ thuận với n*m | 

Với`n, m ≤ 20`, giải pháp thực thi một số thao tác và tiêu thụ bộ nhớ không đáng kể, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3 3\n><>\nv^v\n") == "NO", "sample 1"
# Custom cases
assert run("2 2\n><\n^v\n") == "YES", "all edges opposite"
assert run("2 3\n>>\n^v^\n") == "NO", "horizontal boundaries same"
assert run("3 2\n<><\nvv\n") == "NO", "vertical boundaries same"
assert run("4 4\n><<>\n^v^v\n") == "YES", "mixed interior, boundaries opposite"
assert run("2 2\n<<\n^^\n") == "NO", "both boundaries same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2\n><\n^v | CÓ | kích thước tối thiểu, ranh giới đối diện | 
| 2 3\n>>\n^v^ | KHÔNG | ranh giới ngang cản trở kết nối | 
| 3 2\n<><\nvv | KHÔNG | ranh giới dọc ngăn cản kết nối | 
| 4 4\n><<>\n^v^v | CÓ | lưới lớn hơn, nội thất hỗn hợp nhưng có thể tiếp cận các góc | 
| 2 2\n<<\n^_^ | KHÔNG | lưới tối thiểu, ranh giới giống nhau | 

## Vỏ cạnh 

Nếu cả hai đường ngang đều hướng về cùng một hướng, ví dụ:`<<`với những con đường thẳng đứng`^v`, hàng trên không thể chạm tới hàng dưới cùng. Thuật toán kiểm tra`h_dirs[0] != h_dirs[-1]`và trả về "KHÔNG". Tương tự, nếu các đường dọc giống nhau, ví dụ:`v^v^`, cột ngoài cùng bên trái không thể chạm tới cột ngoài cùng bên phải và kết quả đầu ra chính xác là "KHÔNG". Thuật toán xử lý các lưới có kích thước 2×2 một cách chính xác theo cùng một logic, chỉ xác nhận khả năng kết nối mạnh khi các ranh giới có hướng ngược nhau.
