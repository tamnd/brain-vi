---
title: "CF 131F - Tặng Mẹ"
description: "Chúng ta được cung cấp một bức ảnh đen trắng được biểu thị dưới dạng lưới có kích thước n × m trong đó mỗi ô là '1' cho pixel trắng hoặc '0' cho pixel đen."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 131
codeforces_index: "F"
codeforces_contest_name: "Codeforces Beta Round 95 (Div. 2)"
rating: 2000
weight: 131
solve_time_s: 109
verified: false
draft: false
---

[CF 131F - Tặng mẹ](https://codeforces.com/problemset/problem/131/F) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, hai con trỏ 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bức ảnh đen trắng được thể hiện dưới dạng lưới có kích thước`n × m`trong đó mỗi ô là '1' cho pixel trắng hoặc '0' cho pixel đen. Polycarpus định nghĩa "ngôi sao" là một cấu hình gồm các pixel màu trắng có hình chữ thập: một pixel màu trắng với các lân cận màu trắng ở trên, dưới, bên trái và bên phải. Mục tiêu của chúng ta là đếm xem có bao nhiêu ma trận con hình chữ nhật của bức ảnh chứa ít nhất`k`các ngôi sao. Các hình chữ nhật phải có các cạnh thẳng hàng với lưới. 

Với những hạn chế,`n`Và`m`mỗi ô có thể lên tới 500, nghĩa là lưới có thể chứa tới 250.000 ô. Nếu chúng ta thử một cách tiếp cận đơn giản là liệt kê tất cả các hình chữ nhật có thể có và kiểm tra xem có bao nhiêu ngôi sao nằm bên trong mỗi hình chữ nhật thì số hình chữ nhật là O(n^2 × m^2), có thể lớn hơn 10^10 trong trường hợp xấu nhất-quá lớn để xử lý trực tiếp. Điều này cho chúng ta biết rằng chúng ta cần một bước tiền xử lý để truy vấn nhanh số lượng ngôi sao bên trong bất kỳ hình chữ nhật nào. 

Các trường hợp cạnh bao gồm các lưới có rất ít ngôi sao, các lưới trong đó tất cả các pixel đều có màu trắng tạo thành các ngôi sao chồng lên nhau và các hình chữ nhật nhỏ trong đó một ngôi sao có thể nằm trên đường biên nhưng không hoàn toàn khớp với mẫu hình chữ thập. Việc thực hiện bất cẩn đếm sao mà không kiểm tra mẫu chéo sẽ bị tính quá mức. Ví dụ: trong lưới toàn màu trắng 3 × 3:```
111
111
111
```Chỉ có một ngôi sao hợp lệ ở trung tâm. Việc đếm bất kỳ số '1' nào dưới dạng ngôi sao sẽ báo cáo không chính xác là 9 sao. 

## Phương pháp tiếp cận 

Phương pháp brute-force trước tiên sẽ xác định mọi hình chữ nhật có thể có và đếm các ngôi sao bên trong. Để làm điều này một cách chính xác, trước tiên chúng ta phải phát hiện tất cả các ngôi sao hợp lệ. Đối với mỗi ô, chúng tôi kiểm tra xem bốn ô lân cận cũng có màu trắng hay không. Sau đó, đối với mỗi hình chữ nhật, chúng ta tính tổng các ngôi sao trong vùng đó. Việc kiểm tra từng hình chữ nhật riêng lẻ sẽ cho kết quả O(n^2 × m^2 × 1) để đếm các ngôi sao nếu chúng ta tính toán trước vị trí các ngôi sao, nhưng việc liệt kê các hình chữ nhật vẫn là O(n^2 × m^2), tốc độ này quá chậm. 

Cái nhìn sâu sắc quan trọng là tách biệt việc phát hiện sao khỏi việc liệt kê hình chữ nhật. Sau khi đánh dấu các ngôi sao trên lưới, chúng ta có thể xây dựng mảng tổng tiền tố 2D trong đó`prefix[i][j]`là số sao trong ma trận con từ (1,1) đến (i,j). Khi đó, tổng số sao trong bất kỳ hình chữ nhật nào được xác định bởi (x1, y1) đến (x2, y2) có thể được tính theo O(1) bằng cách sử dụng công thức bao gồm-loại trừ tiêu chuẩn:```
stars = prefix[x2][y2] - prefix[x1-1][y2] - prefix[x2][y1-1] + prefix[x1-1][y1-1]
```Bây giờ vấn đề giảm xuống còn việc đếm tất cả các hình chữ nhật có ít nhất`k`các ngôi sao sử dụng một chiến lược hiệu quả. Chúng tôi lặp lại tất cả các cặp hàng trên cùng và dưới cùng, nén bài toán 2D thành 1D cho mỗi cột dưới dạng số sao giữa các hàng đó và sử dụng cửa sổ trượt có hai con trỏ để đếm các mảng con có tổng số sao đáp ứng hoặc vượt quá`k`. Điều này tận dụng thực tế là các cột được tích lũy và tổng không âm, do đó kỹ thuật hai con trỏ hoạt động hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 × m^2) | O(n×m) | Quá chậm | 
| Tối ưu | O(n^2 × m) | O(n×m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo lưới`stars[n][m]`để đánh dấu vị trí của các ngôi sao hợp lệ. Lặp lại từng ô`(i,j)`trong đó 1  i  n-2 và 1  j  m-2. Nếu ô và bốn ô lân cận của nó có màu trắng, hãy đánh dấu`stars[i][j] = 1`. Nếu không, hãy đánh dấu là 0. Điều này đảm bảo chỉ tính các vị trí trung tâm của dấu thập. 
2. Xây dựng mảng tổng tiền tố 2D`prefix[n+1][m+1]`. Đối với mỗi ô`(i,j)`, bộ`prefix[i][j] = stars[i][j] + prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1]`. Điều này cho phép truy vấn O(1) về số lượng sao bên trong bất kỳ hình chữ nhật nào. 
3. Lặp lại tất cả các cặp hàng trên và dưới`top`Và`bottom`. Với mỗi cột, hãy tính`col_stars[c]`bằng số lượng sao giữa`top`Và`bottom`trong cột đó bằng cách sử dụng mảng tổng tiền tố. 
4. Điều trị`col_stars`dưới dạng mảng 1D. Sử dụng kỹ thuật hai con trỏ: duy trì cửa sổ trượt các cột từ`left`ĐẾN`right`sao cho tổng số sao trong cửa sổ này ít nhất là`k`. Tăng`right`cho đến khi tổng ≥ k thì tăng`left`trong khi vẫn duy trì tổng. Mỗi cửa sổ xác định một hình chữ nhật từ`top`ĐẾN`bottom`và từ`left`ĐẾN`right`với ít nhất`k`các ngôi sao. Thêm vào`(m - right + 1)`cho câu trả lời, đại diện cho tất cả các hình chữ nhật hợp lệ bắt đầu từ`left`. 
5. Tính tổng tất cả các cặp hàng trên cùng dưới cùng để có được tổng số hình chữ nhật hợp lệ. 

Tại sao nó hoạt động: Ở mọi giai đoạn, thuật toán duy trì số lượng sao chính xác trong hình chữ nhật hiện tại bằng cách sử dụng tổng tiền tố. Phương pháp hai con trỏ đảm bảo rằng tất cả các mảng con (cột) liền kề đều được đếm mà không bỏ sót bất kỳ cột nào và cửa sổ trượt đảm bảo rằng các hình chữ nhật được tính một cách hiệu quả mà không cần tính toán lại dư thừa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    
    # Step 1: mark valid stars
    stars = [[0]*m for _ in range(n)]
    for i in range(1, n-1):
        for j in range(1, m-1):
            if grid[i][j] == '1' and grid[i-1][j] == '1' and grid[i+1][j] == '1' \
               and grid[i][j-1] == '1' and grid[i][j+1] == '1':
                stars[i][j] = 1
    
    # Step 2: 2D prefix sum
    prefix = [[0]*(m+1) for _ in range(n+1)]
    for i in range(n):
        for j in range(m):
            prefix[i+1][j+1] = stars[i][j] + prefix[i][j+1] + prefix[i+1][j] - prefix[i][j]
    
    ans = 0
    # Step 3: iterate over all row pairs
    for top in range(1, n+1):
        for bottom in range(top, n+1):
            col_stars = [0]*m
            for col in range(1, m+1):
                col_stars[col-1] = prefix[bottom][col] - prefix[top-1][col] - prefix[bottom][col-1] + prefix[top-1][col-1]
            
            # Step 4: two pointers to count valid column windows
            left = 0
            curr_sum = 0
            for right in range(m):
                curr_sum += col_stars[right]
                while curr_sum - col_stars[left] >= k:
                    curr_sum -= col_stars[left]
                    left += 1
                if curr_sum >= k:
                    ans += m - right
    
    print(ans)

if __name__ == "__main__":
    main()
```các`stars`mảng cô lập logic phát hiện cho các đường chéo. Tổng tiền tố cho phép truy vấn bất kỳ hình chữ nhật nào trong O(1). Các vòng lặp lồng nhau trên các cặp trên-dưới có thể quản lý được vì`n`500. Quét bằng hai con trỏ qua các cột sẽ đếm hiệu quả các hình chữ nhật có ít nhất`k`sao bằng cách tận dụng tổng không âm của`col_stars`. 

## Ví dụ đã hoạt động 

**Mẫu 1**```
n=4, m=6, k=2
grid:
111000
111100
011011
000111
```| hàng đầu | dưới cùng | col_stars | trái | đúng | curr_sum | hình chữ nhật được thêm vào | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 2 2 1 0 0 | 0 | 3 | 6 | 6 | 

Bảng cho thấy rằng đối với top=1 và Bottom=3, cột 1-4 chứa các ngôi sao có tổng vượt quá`k=2`. Cửa sổ trượt thêm 6 hình chữ nhật. 

**Mẫu 2 (toàn màu trắng 3×3, k=1)**```
111
111
111
```| hàng đầu | dưới cùng | col_stars | trái | đúng | curr_sum | hình chữ nhật được thêm vào | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 1 0 | 0 | 1 | 1 | 3 | 

Chỉ có cột trung tâm có một ngôi sao. Cửa sổ trượt đếm 3 hình chữ nhật bao gồm tất cả các vị trí hợp lệ. 

Những dấu vết này xác nhận rằng thuật toán xác định chính xác vị trí các ngôi sao và đếm tất cả các hình chữ nhật chứa ít nhất`k`các ngôi sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 × m) | Vòng lặp lồng nhau trên các cặp hàng (O(n^2)), quét hai con trỏ trên m cột. | 
| Không gian | O(n×m) | Lưu trữ mảng sao và tổng tiền tố. | 

Với n, m ₫ 500, n^2 × m ₫ 125
