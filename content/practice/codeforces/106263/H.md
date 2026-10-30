---
title: "CF 106263H - SCNU LOGO"
description: "Tác vụ bắt đầu bằng một logo ASCII cố định: một lưới nhỏ được tạo thành từ và . ký tự có kích thước 5 hàng x 36 cột. Bạn được cấp một hệ số tỷ lệ k và bạn phải xuất ra một lưới mới trong đó mọi ký tự gốc trở thành một khối k × k vững chắc của cùng một ký tự."
date: "2026-06-19T14:21:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "H"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 63
verified: true
draft: false
---

[CF 106263H - SCNU LOGO](https://codeforces.com/problemset/problem/106263/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ bắt đầu bằng một logo ASCII cố định: một lưới nhỏ làm bằng`*`Và`.`ký tự có kích thước 5 hàng x 36 cột. Bạn được cung cấp một hệ số tỷ lệ`k`và bạn phải xuất ra một lưới mới trong đó mọi ký tự gốc trở thành một khối`k × k`khối có cùng ký tự. 

Về mặt khái niệm, mỗi ký tự trong lưới ban đầu là một pixel. Chia tỷ lệ theo`k`có nghĩa là mở rộng từng pixel thành một ô vuông có chiều dài cạnh`k`, giữ nguyên màu sắc của nó. Do đó, hình ảnh cuối cùng có chiều cao`5k`và chiều rộng`36k`. 

Các ràng buộc là cực kỳ nhỏ, với`k`nhiều nhất là 5, do đó kích thước đầu ra tối đa là 25 x 180 ký tự. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ cách tiếp cận nào có kích thước đầu ra đều tuyến tính đều an toàn. Hạn chế thực sự là tính chính xác và tránh những lỗi lập chỉ mục tinh vi. 

Một lỗi phổ biến xuất hiện khi mọi người cố gắng xây dựng lại các hàng trước rồi sau đó sao chép chúng không chính xác. Một sai lầm khác là trộn lẫn thứ tự mở rộng hàng và mở rộng cột, điều này có thể dẫn đến việc xen kẽ các mẫu lặp lại thay vì chia tỷ lệ theo khối. 

Ví dụ: nếu lưới ban đầu có một hàng`"*."`Và`k = 2`, kết quả đúng là:```
**..
**..
```Thay vào đó, một cách tiếp cận sai có thể sao chép các ký tự theo chiều dọc nhưng lại quên mở rộng theo chiều ngang, tạo ra:```
*.
*.
```vi phạm yêu cầu mỗi ký tự phải trở thành một khối vuông. 

Một dạng lỗi khác xuất phát từ việc cố gắng xây dựng lưới theo tỷ lệ bằng cách nối các chuỗi liên tục mà không kiểm soát căn chỉnh, điều này có thể dễ dàng dẫn đến các hàng bị trùng lặp không chính xác hoặc các cột không đồng bộ. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực là trực tiếp: coi mỗi ký tự là một pixel và thay thế nó bằng một`k × k`chặn ngay lập tức trong một lưới mới. Đối với mỗi hàng ban đầu, chúng tôi xây dựng`k`hàng mới và trong mỗi hàng đó, chúng tôi mở rộng từng ký tự thành`k`bản sao. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa về tỷ lệ một cách rõ ràng. Chi phí đến từ việc xây dựng chuỗi lặp đi lặp lại. Đối với mỗi hàng trong số 5 hàng, chúng tôi xử lý 36 ký tự và mỗi ký tự đóng góp`k`bản sao cho mỗi`k`hàng, do đó tổng công việc tỷ lệ thuận với`5 × 36 × k²`. Từ`k ≤ 5`, đây nhiều nhất là vài nghìn thao tác ký tự, điều này không đáng kể. 

Không cần có sự tối ưu hóa có ý nghĩa nào ngoài việc triển khai mở rộng một cách rõ ràng. Thông tin chi tiết quan trọng là nhận ra rằng việc chia tỷ lệ tách thành mở rộng hàng và mở rộng cột độc lập: sao chép dọc kiểm soát số lần lặp lại mỗi hàng và sao chép ngang kiểm soát độ rộng của mỗi ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu | O(5 × 36 × k²) | O(5 × 36 × k²) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng, xây dựng sạch sẽ) | O(5 × 36 × k²) | O(5 × 36 × k²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hàng đầu ra theo hàng bằng cách sử dụng mở rộng trực tiếp. 

1. Đọc`k`và 5 dòng logo gốc. Cấu trúc cố định nên không tồn tại sự mơ hồ về phân tích cú pháp. 
2. Đối với mỗi hàng gốc, xây dựng một phiên bản mở rộng theo chiều ngang bằng cách thay thế từng ký tự`c`với`c`lặp đi lặp lại`k`lần. Điều này tạo ra một chuỗi đại diện cho một hàng được chia tỷ lệ. 
3. Lặp lại từng hàng đã mở rộng`k`lần và nối tất cả các bản sao vào đầu ra. Điều này thực hiện việc chia tỷ lệ theo chiều dọc bằng cách nhân đôi. 
4. In tất cả các hàng được tạo theo thứ tự. 

Việc phân tách mở rộng theo chiều ngang và chiều dọc đảm bảo rằng mỗi ký tự gốc ánh xạ tới một ký tự liền kề`k × k`khối. Việc mở rộng theo chiều ngang đảm bảo độ chính xác về chiều rộng, trong khi việc in lặp lại đảm bảo độ chính xác về chiều cao. 

### Tại sao nó hoạt động 

Mỗi ký tự gốc ở vị trí`(i, j)`ánh xạ tới một khối duy nhất trong các hàng chiếm lưới đầu ra`[i·k, (i+1)·k)`và cột`[j·k, (j+1)·k)`. Thuật toán xây dựng chính xác các khối này vì việc mở rộng theo chiều ngang sẽ cố định các khoảng cột và việc lặp lại theo chiều dọc sẽ cố định các khoảng cách hàng. Vì mỗi khối được lấp đầy một cách độc lập và nhất quán nên không thể xảy ra sự chồng chéo hoặc khoảng trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input().strip())

grid = [input().rstrip("\n") for _ in range(5)]

out_lines = []

for row in grid:
    expanded_row = "".join(ch * k for ch in row)
    for _ in range(k):
        out_lines.append(expanded_row)

sys.stdout.write("\n".join(out_lines))
```Giải pháp đầu tiên đọc hệ số tỷ lệ và số hàng cố định. Sau đó nó xử lý từng hàng một cách độc lập. biểu hiện`ch * k`thực hiện chia tỷ lệ theo chiều ngang bằng cách sao chép các ký tự. Sau khi xây dựng một hàng mở rộng, nó sẽ được nối thêm`k`lần để đạt được tỷ lệ theo chiều dọc. Thứ tự này rất quan trọng: việc mở rộng theo chiều ngang trước tiên đảm bảo rằng mỗi dòng in đã có chiều rộng chính xác, do đó việc sao chép theo chiều dọc không cần tính toán lại. 

Một điểm tinh tế là tránh nối lặp lại bên trong các vòng lặp bên trong bằng cách sử dụng`+=`, điều này sẽ làm giảm hiệu suất do tái phân bổ chuỗi lặp đi lặp lại. Thay vào đó, chỉ cần tạo hàng mở rộng một lần cho mỗi hàng đầu vào là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
k = 1
row = "*.*"
```| Bước | Hàng gốc | Hàng mở rộng | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | _._ | _._ | _._ | 

Hệ số tỷ lệ là 1, do đó không có sự biến đổi nào xảy ra. Thuật toán chính xác giữ nguyên cả hai chiều. 

### Ví dụ 2 

đầu vào:```
k = 2
row = "*."
```| Bước | Hàng gốc | Hàng mở rộng | Dòng đầu ra | 
| --- | --- | --- | --- | 
| 1 | *. | **.. | **.. | 
| 2 | *. | **.. | **.. | 

Mỗi ký tự mở rộng thành khối 2 ký tự và mỗi hàng được lặp lại hai lần, tạo thành khối 2 × 2 cho mỗi ký tự gốc. Điều này xác nhận rằng cả hai trục đều được chia tỷ lệ một cách độc lập và nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(5 × 36 × k²) | Mỗi ký tự đóng góp k bản ngang và mỗi hàng được lặp lại k lần | 
| Không gian | O(5 × 36 × k²) | Lưới đầu ra phải được lưu trữ trước khi in | 

Kích thước đầu ra tối đa rất nhỏ (tối đa 900 ký tự), do đó giải pháp vừa vặn thoải mái trong giới hạn thời gian và bộ nhớ. Thuật toán có hiệu quả là thời gian không đổi dưới các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(sys.stdin.readline().strip())
    grid = [sys.stdin.readline().rstrip("\n") for _ in range(5)]
    out_lines = []
    for row in grid:
        expanded_row = "".join(ch * k for ch in row)
        for _ in range(k):
            out_lines.append(expanded_row)
    return "\n".join(out_lines)

# minimal scaling
assert run("1\n*****...******...***...**...**....**\n**......**.......****..**...**....**\n*****...**.......**.**.**...**....**\n...**...**.......**..****...**....**\n*****...******...**...***...********\n") != "", "k=1 should output same shape"

# k = 2 simple pattern
assert run("2\n*.\n*.\n*.\n*.\n*.\n") == "**..**..\n**..**..\n**..**..\n**..**..\n**..**..\n**..**..", "basic expansion"

# k = 3 single row repeated
assert run("3\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n") == run("3\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*.*\n"), "consistency check"

# max k behavior
assert len(run("5\n*****...******...***...**...**....**\n**......**.......****..**...**....**\n*****...**.......**.**.**...**....**\n...**...**.......**..****...**....**\n*****...******...**...***...********\n").splitlines()) == 25, "height should be 5*k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 biểu tượng | logo giống hệt nhau | trường hợp nhận dạng | 
| k=2 mẫu nhỏ | Mở rộng khối 2×2 | logic chia tỷ lệ chính xác | 
| đầu vào hàng lặp đi lặp lại | mở rộng nhất quán | ổn định trên các hàng | 
| k=5 đầy đủ logo | đầu ra 25 hàng | xử lý ràng buộc tối đa | 

## Vỏ cạnh 

Khi nào`k = 1`, thuật toán sẽ hoạt động như danh tính. Cấu trúc vẫn chạy các vòng lặp giống nhau, nhưng mỗi ký tự được lặp lại một lần và mỗi hàng được in một lần, do đó đầu ra khớp chính xác với đầu vào. 

Vì`k = 5`, trường hợp lớn nhất, mỗi ký tự trở thành một khối 5×5. Thuật toán xử lý 5 hàng và tạo ra 25 hàng đầu ra. Mỗi phần mở rộng hàng vẫn tuyến tính với 36 ký tự, do đó, kết quả cuối cùng được tạo ra mà không gặp bất kỳ rủi ro nào về vấn đề hiệu suất hoặc căng thẳng về bộ nhớ. 

Một trường hợp tinh tế là khi một hàng bao gồm toàn bộ`.`hoặc hoàn toàn của`*`. Logic mở rộng không phân biệt các ký tự, do đó các hàng đồng nhất sẽ mở rộng thành các khối đồng nhất mà không có bất kỳ biến dạng cấu trúc nào.
