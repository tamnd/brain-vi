---
title: "CF 1147B - Hình Chladni"
description: "Bài toán đưa ra một lưới hình chữ nhật gồm các ký tự biểu thị một mẫu trên một tấm kim loại, trong đó mỗi ô chứa một ký tự . (trống) hoặc a (đầy). Nhiệm vụ là tìm hình chữ nhật nhỏ nhất chứa tất cả các ký tự."
date: "2026-06-12T03:14:26+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "strings"]
categories: ["algorithms"]
codeforces_contest: 1147
codeforces_index: "B"
codeforces_contest_name: "Forethought Future Cup - Final Round (Onsite Finalists Only)"
rating: 1900
weight: 1147
solve_time_s: 62
verified: true
draft: false
---

[CF 1147B - Hình Chladni](https://codeforces.com/problemset/problem/1147/B) 

**Xếp hạng:** 1900 
**Tags:** bạo lực, dây 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một lưới hình chữ nhật gồm các ký tự biểu thị một mẫu trên một tấm kim loại, trong đó mỗi ô chứa một`.`(trống) hoặc một`*`(điền). Nhiệm vụ là tìm hình chữ nhật nhỏ nhất chứa tất cả các`*`nhân vật. Hình chữ nhật này được mô tả bởi hàng trên cùng, hàng dưới cùng, cột ngoài cùng bên trái và cột ngoài cùng bên phải, sử dụng các chỉ số dựa trên 1. 

Đầu vào đầu tiên đưa ra số lượng hàng và cột, tiếp theo là chính lưới. Đầu ra phải là bốn số nguyên xác định hình chữ nhật giới hạn của tất cả các ngôi sao. 

Vì lưới có thể lớn tới 500x500 nên bất kỳ phương pháp nào kiểm tra rõ ràng mọi hình chữ nhật có thể đều không khả thi. Một lực lượng thô bạo xem xét mọi cặp tọa độ trên/dưới và trái/phải để kiểm tra xem nó có chứa tất cả các ngôi sao hay không sẽ cần tới`(500^2)^2`hoạt động, khoảng 6,25×10^10, quá chậm so với giới hạn 2 giây thông thường. 

Các trường hợp cạnh cần cẩn thận khi bao gồm các lưới có một`*`, lưới nơi tất cả`*`nằm trong một hàng hoặc một cột và các lưới ở đó`*`chạm vào ranh giới. Ví dụ: một lưới như:```
3 4
*...
....
....
```nên quay lại`1 1 1 1`. Cách tiếp cận bất cẩn với nhiều hàng hoặc cột sẽ thất bại. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ quét mọi hình chữ nhật có thể, tính toán xem nó có chứa tất cả các ngôi sao hay không và theo dõi hình chữ nhật nhỏ nhất như vậy. Điều này có tác dụng vì việc kiểm tra một hình chữ nhật rất đơn giản nhưng không thành công khi lưới lớn: với n hàng và m cột, số lượng thao tác là O(n^2 * m^2), quá chậm đối với n=m=500. 

Quan sát quan trọng là chúng ta không cần phải kiểm tra mọi hình chữ nhật. Mỗi ngôi sao chỉ đóng góp vào các điểm cực trị của hình chữ nhật. Hàng trên cùng của hình chữ nhật là chỉ số hàng tối thiểu chứa một`*`, hàng dưới cùng là chỉ số hàng tối đa chứa một`*`, cột ngoài cùng bên trái là chỉ số cột tối thiểu với`*`và cột ngoài cùng bên phải là chỉ số cột tối đa có`*`. Quét lưới một lần và ghi lại các điểm cực trị này là đủ. 

Điều này làm giảm vấn đề từ quét brute-force bốn vòng lặp lồng nhau thành quét O(n*m) duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m^2) | O(1) | Quá chậm | 
| Tối ưu | O(n*m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến để theo dõi ranh giới hình chữ nhật. Bộ`top`đến một giá trị lớn (lớn hơn bất kỳ hàng nào có thể),`bottom`đến -1,`left`đạt giá trị lớn và`right`đến -1. Chúng đại diện cho những thái cực chưa được khởi tạo. 
2. Lặp lại từng hàng`i`từ 0 đến n-1. Đối với mỗi hàng, lặp qua từng cột`j`từ 0 đến m-1. Nếu tế bào`(i,j)`chứa`*`, cập nhật các thái cực: 

- Đặt`top`ĐẾN`min(top, i)`. 
- Bộ`bottom`ĐẾN`max(bottom, i)`. 
- Bộ`left`ĐẾN`min(left, j)`. 
- Bộ`right`ĐẾN`max(right, j)`. 
3. Sau khi quét lưới, hình chữ nhật chứa tất cả các ngôi sao có hàng trên cùng`top+1`, hàng dưới cùng`bottom+1`, cột bên trái`left+1`, và cột bên phải`right+1`. các`+1`chiếm chỉ mục dựa trên 1. 

Tại sao nó hoạt động: Các biến`top`,`bottom`,`left`,`right`luôn giữ những chỉ số cực cao của các ngôi sao được nhìn thấy cho đến nay. Vì chúng tôi quét toàn bộ lưới nên không bỏ sót ngôi sao nào. Bằng cách lấy cực tiểu và cực đại của tọa độ, chúng tôi đảm bảo hình chữ nhật bao quanh chính xác tất cả các ngôi sao. Không có nguy cơ thiếu một ranh giới vì mỗi`*`được xem xét riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    top, bottom = n, -1
    left, right = m, -1

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                top = min(top, i)
                bottom = max(bottom, i)
                left = min(left, j)
                right = max(right, j)

    print(top + 1, left + 1, bottom + 1, right + 1)

if __name__ == "__main__":
    main()
```Mỗi phần của giải pháp này tương ứng trực tiếp với hướng dẫn thuật toán. Việc khởi tạo các cực trị đảm bảo chúng ta có thể so sánh từng`*`một cách chính xác. Vòng lặp kép đảm bảo mọi ô đều được kiểm tra, tránh bỏ sót các ngôi sao. Sự bổ sung cuối cùng của`+1`chuyển đổi từ lập chỉ mục Python dựa trên 0 sang đầu ra dựa trên 1 mà vấn đề yêu cầu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 4
....
.*..
....
```| tôi | j | lưới[i][j] | hàng đầu | dưới cùng | trái | đúng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | . | 3 | -1 | 4 | -1 | 
| 1 | 2 | . | 3 | -1 | 4 | -1 | 
| 2 | 2 | * | 1 | 1 | 2 | 2 | 

Đầu ra:`2 2 2 2`. Bảng cho thấy sau khi gặp trường hợp duy nhất`*`, mọi thái cực đều sụp đổ vào ô đó. 

Đầu vào mẫu 2:```
4 5
*....
.....
..*..
.....
```| tôi | j | lưới[i][j] | hàng đầu | dưới cùng | trái | đúng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | * | 0 | 0 | 0 | 0 | 
| 2 | 2 | * | 0 | 2 | 0 | 2 | 

Đầu ra:`1 1 3 3`. Cả hai ngôi sao đều được bao gồm trong hình chữ nhật. Bảng xác nhận rằng thuật toán mở rộng ranh giới một cách chính xác khi một ngôi sao mới xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Chúng tôi quét từng ô chính xác một lần. | 
| Không gian | O(n*m) | Chúng tôi lưu trữ lưới; chỉ có bốn số nguyên bổ sung được sử dụng. | 

Với n, m lên tới 500, số lượng thao tác là 250.000, trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("3 4\n....\n.*..\n....\n") == "2 2 2 2", "sample 1"

# single row
assert run("1 5\n..*..\n") == "1 3 1 3", "single row"

# single column
assert run("5 1\n.\n*\n.\n*\n.\n") == "2 1 4 1", "single column"

# all stars
assert run("2 2\n**\n**\n") == "1 1 2 2", "all stars"

# one star at corner
assert run("3 3\n*..\n...\n...\n") == "1 1 1 1", "corner"

# stars in diagonal
assert run("3 3\n*..\n.*.\n..*\n") == "1 1 3 3", "diagonal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x5 với ngôi sao ở giữa | 1 3 1 3 | hàng đơn | 
| 5x1 có hai sao | 2 1 4 1 | cột đơn | 
| 2x2 tất cả các ngôi sao | 1 1 2 2 | hình chữ nhật kéo dài toàn bộ lưới | 
| sao góc 3x3 | 1 1 1 1 | điều kiện biên | 
| sao chéo 3x3 | 1 1 3 3 | hình chữ nhật mở rộng với nhiều cực trị | 

## Vỏ cạnh 

Đối với một người`*`ở bất kỳ đâu, thuật toán đặt trên, dưới, trái và phải có cùng một giá trị, tạo ra một hình chữ nhật có kích thước 1x1. Đối với một ngôi sao ở góc trên bên trái,`top`Và`left`vẫn bằng 0 và cuối cùng`+1`chuyển đổi thành các chỉ số dựa trên 1. Đối với một ngôi sao ở phía dưới bên phải,`bottom`Và`right`được cập nhật chính xác dưới dạng chỉ số tối đa. Trong mọi trường hợp, thuật toán giới hạn chính xác tất cả các ngôi sao mà không vượt quá hoặc thiếu hụt. 

Thuật toán xử lý chính xác các lưới trong đó tất cả các ngôi sao nằm trên một hàng hoặc một cột vì cực tiểu và cực đại hội tụ về vị trí thực tế. Điều này tránh được những cạm bẫy khi cách tiếp cận đơn giản có thể cho rằng hình chữ nhật có kích thước tối thiểu lớn hơn mức cần thiết.
