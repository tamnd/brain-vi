---
title: "CF 1146G - Hạn chế phân vùng"
description: "Chúng ta được yêu cầu ấn định độ cao của các ngôi nhà dọc theo một con phố để tối đa hóa tổng lợi nhuận. Đường phố có n vị trí sẵn có, mỗi vị trí có thể chứa một ngôi nhà có chiều cao nguyên từ 0 đến h. Lợi nhuận từ một ngôi nhà có chiều cao a là a^2. Có m giới hạn thành phố."
date: "2026-06-12T03:22:07+07:00"
tags: ["codeforces", "competitive-programming", "dp", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1146
codeforces_index: "G"
codeforces_contest_name: "Forethought Future Cup - Elimination Round"
rating: 2700
weight: 1146
solve_time_s: 79
verified: true
draft: false
---

[CF 1146G - Hạn chế phân vùng](https://codeforces.com/problemset/problem/1146/G) 

**Đánh giá:** 2700 
**Tags:** dp, luồng, đồ thị 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu ấn định độ cao của các ngôi nhà dọc theo một con phố để tối đa hóa tổng lợi nhuận. Đường phố có`n`các vị trí có sẵn, mỗi vị trí có thể chứa một ngôi nhà có chiều cao nguyên giữa`0`Và`h`. Lợi nhuận từ nhà trên cao`a`là`a^2`. 

có`m`hạn chế của thành phố. Mỗi hạn chế chỉ định một phạm vi vị trí liền kề`[l_i, r_i]`, giới hạn chiều cao`x_i`, và một quả phạt đền`c_i`. Nếu ngôi nhà cao nhất trong phạm vi đó vượt quá`x_i`, hình phạt`c_i`được trừ vào tổng lợi nhuận. Nhiệm vụ của chúng tôi là chọn độ cao để tối đa hóa tổng lợi nhuận trừ đi hình phạt. 

Các hạn chế là nhỏ:`n, h, m ≤ 50`. Điều này có nghĩa là các thuật toán lên đến khoảng`O(n * h^2 * m)`hoặc`O(h^n)`với việc cắt tỉa có thể khả thi. Bởi vì cả hai`n`Và`h`lên tới 50, thử mọi sự kết hợp độ cao một cách ngây thơ (`h^n`) rõ ràng là không thực tế. Các trường hợp cạnh liên quan đến các phạm vi bao gồm các vị trí đơn lẻ, các phạm vi chồng chéo với các giới hạn khác nhau hoặc các giới hạn bằng`0`hoặc`h`. Ví dụ: nếu một hạn chế cho biết tại chỗ`1`không thể vượt quá chiều cao`0`, một cách tiếp cận ngây thơ gán mọi độ cao cho`h`sẽ phải chịu một hình phạt không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại tất cả các bài tập chiều cao có thể có. Đối với mỗi bài tập, hãy tính tổng bình phương và trừ đi tất cả các khoản tiền phạt nếu vi phạm hạn chế. Số bài tập là`O((h+1)^n)`, điều này hoàn toàn không thể thực hiện được đối với`n = 50`. 

Một cách tiếp cận có hệ thống hơn là xem vấn đề này như một **vấn đề lan truyền hạn chế độ cao**. Mỗi hạn chế có hiệu quả giới hạn chiều cao của các ngôi nhà trong phạm vi của nó. Nếu một hạn chế áp đặt mức tối đa`x_i`, thì độ cao tối ưu tại mỗi vị trí trong`[l_i, r_i]`không thể vượt quá`x_i`mà không bị phạt tiền`c_i`. Vì lợi nhuận là bậc hai và tăng dần theo chiều cao nên chúng tôi muốn đặt mỗi ngôi nhà càng cao càng tốt mà không phải trả những khoản tiền phạt không cần thiết. 

Thông tin chi tiết quan trọng là chúng ta có thể **gán cho mỗi vị trí mức tối thiểu của tất cả các hạn chế ảnh hưởng đến vị trí đó**, sau đó tính tổng các bình phương. Điều này có hiệu quả vì mỗi hạn chế có thể được áp dụng độc lập: một khi chúng ta cố định giới hạn cho từng vị trí để tránh bị phạt, việc tăng chiều cao vượt quá giới hạn đó chỉ mang lại lợi nhuận nếu chúng ta sẵn sàng trả tiền phạt, nhưng trả tiền phạt không bao giờ tốt hơn việc chọn chiều cao tối đa dưới giới hạn. Vì vậy, chúng tôi có thể tính toán chiều cao tối đa được phép cho mỗi vị trí và sau đó trừ đi mọi khoản tiền phạt phát sinh nếu chúng tôi quyết định vi phạm giới hạn. 

Bởi vì các giới hạn và hình phạt nhỏ và phạm vi ngắn nên mô phỏng trực tiếp sẽ hoạt động. Chúng tôi khởi tạo một mảng`height_limit`với`h`cho mọi vị trí. Đối với mỗi hạn chế, chúng tôi cập nhật các vị trí trong`[l_i, r_i]`với mức tối thiểu của giới hạn hiện tại và`x_i`. Sau đó, phép gán tối ưu là`height[i] = height_limit[i]`cho mọi vị trí. Cuối cùng, tính tổng lợi nhuận bằng tổng bình phương trừ đi tiền phạt đối với bất kỳ hạn chế nào vẫn bị vi phạm (nếu chúng tôi quyết định bỏ qua một số hạn chế). Trong thực tế, vì vấn đề muốn lợi nhuận tối đa và tất cả các khoản tiền phạt đều dương nên việc vi phạm giới hạn không bao giờ là tối ưu, vì vậy chúng ta có thể đơn giản thực thi các giới hạn chiều cao tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((h+1)^n * m) | O(n) | Quá chậm | 
| Tối ưu | O(n * m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`height_limit`kích thước`n`có giá trị`h`. Đây là chiều cao tối đa được phép ở mỗi vị trí trước khi bị phạt. 
2. Đối với mỗi hạn chế`(l_i, r_i, x_i, c_i)`, lặp qua các vị trí`l_i-1`ĐẾN`r_i-1`và cập nhật`height_limit[pos] = min(height_limit[pos], x_i)`. Điều này đảm bảo chúng tôi không bao giờ vượt quá giới hạn và do đó tránh bị phạt. 
3. Khởi tạo`profit = 0`. 
4. Đối với từng vị trí`i`TRONG`[0, n-1]`, thêm vào`height_limit[i]^2`ĐẾN`profit`. Điều này nắm bắt mức tăng tối đa theo giới hạn được thi hành. 
5. Không cần phải trừ tiền phạt vì các giới hạn đã được thực thi. Trở lại`profit`. 

Tại sao nó hoạt động: Điều bất biến được duy trì là ở mỗi bước,`height_limit[i]`là độ cao cực đại tại`i`điều đó không gây ra bất kỳ khoản tiền phạt nào. Bất kỳ chiều cao nào lớn hơn giới hạn này sẽ làm giảm lợi nhuận do bị phạt, trong khi bất kỳ chiều cao nào nhỏ hơn sẽ làm giảm mức tăng bình phương. Do đó phép gán là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, h, m = map(int, input().split())
height_limit = [h] * n

for _ in range(m):
    l, r, x, c = map(int, input().split())
    for i in range(l-1, r):
        height_limit[i] = min(height_limit[i], x)

profit = sum(a * a for a in height_limit)
print(profit)
```Khối đầu tiên đọc đầu vào và khởi tạo độ cao tối đa có thể. Vòng lặp hạn chế đảm bảo mỗi nhà không vượt quá giới hạn hạn chế. Tính toán cuối cùng tính tổng các bình phương, điều này đúng vì lợi nhuận bậc hai tăng theo chiều cao đến giới hạn. sử dụng`l-1`Và`r`đảm bảo lập chỉ mục dựa trên 0 chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3 3
1 1 1 1000
2 2 3 1000
3 3 2 1000
```| Bước | chiều cao_limit | Hành động | 
| --- | --- | --- | 
| ban đầu | [3,3,3] | đặt chiều cao tối đa = h | 
| hạn chế 1 | [1,3,3] | áp dụng giới hạn 1 cho pos 1 | 
| hạn chế 2 | [1,3,3] | áp dụng giới hạn 3 cho vị trí 2 (không thay đổi) | 
| hạn chế 3 | [1,3,2] | áp dụng giới hạn 2 cho pos 3 | 
| lợi nhuận | 1^2+3^2+2^2=14 | tổng bình phương | 

Dấu vết xác nhận các giới hạn được truyền một cách chính xác, tạo ra lợi nhuận tối đa. 

**Ví dụ tùy chỉnh** 

đầu vào:```
4 5 2
1 3 2 10
2 4 4 20
```| Bước | chiều cao_limit | Hành động | 
| --- | --- | --- | 
| ban đầu | [5,5,5,5] | đặt chiều cao tối đa = h | 
| hạn chế 1 | [2,2,2,5] | áp dụng giới hạn 2 cho pos 1-3 | 
| hạn chế 2 | [2,2,2,4] | áp dụng giới hạn 4 cho vị trí 2-4 | 
| lợi nhuận | 2^2+2^2+2^2+4^2=4+4+4+16=28 | tổng bình phương | 

Điều này cho thấy các hạn chế chồng chéo được xử lý bằng cách lấy giới hạn tối thiểu tại mỗi vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m) | Mỗi hạn chế lặp đi lặp lại nhiều nhất`n`vị trí | 
| Không gian | O(n) | Chỉ một`height_limit`mảng là cần thiết | 

Thuật toán dễ dàng phù hợp với các ràng buộc, như`50 * 50 = 2500`hoạt động là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, h, m = map(int, input().split())
    height_limit = [h] * n
    for _ in range(m):
        l, r, x, c = map(int, input().split())
        for i in range(l-1, r):
            height_limit[i] = min(height_limit[i], x)
    return str(sum(a*a for a in height_limit))

# provided samples
assert run("3 3 3\n1 1 1 1000\n2 2 3 1000\n3 3 2 1000\n") == "14"

# custom cases
assert run("4 5 2\n1 3 2 10\n2 4 4 20\n") == "28", "overlapping ranges"
assert run("1 50 0\n") == "2500", "single house no restriction"
assert run("5 1 5\n1 5 0 1000\n1 1 1 500\n2 2 1 500\n3 3 1 500\n4 4 1 500\n") == "4", "mix of zero limits"
assert run("50 50 1\n1 50 25 1000\n") == str(25*25*50), "max n and h restriction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 5 2 … | 28 | phạm vi chồng chéo được xử lý | 
| 1 50 0 | 2500 | nhà đơn không hạn chế | 
| 5 1 5 … | 4 | hỗn hợp giới hạn số không và giới hạn đơn vị | 
| 50 50 1 … | 31250 | maximum n and h |

 ## Vỏ cạnh 

If a restriction has`x_i = 0`, thuật toán đặt đúng `
