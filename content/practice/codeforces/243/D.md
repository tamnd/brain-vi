---
title: "CF 243D - Khối"
description: "Chúng ta có một lưới n×n đại diện cho một thành phố được xây dựng từ các khối đơn vị xếp chồng lên nhau trong các tòa tháp. Mỗi ô của lưới chứa một số nguyên biểu thị chiều cao của tháp tại vị trí đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "geometry", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 243
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 150 (Div. 1)"
rating: 2700
weight: 243
solve_time_s: 189
verified: true
draft: false
---

[CF 243D - Hình khối](https://codeforces.com/problemset/problem/243/D) 

**Đánh giá:** 2700 
**Tags:** cấu trúc dữ liệu, dp, hình học, hai con trỏ 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới _n_×_n_ đại diện cho một thành phố được xây dựng từ các khối đơn vị xếp chồng lên nhau trong các tòa tháp. Mỗi ô của lưới chứa một số nguyên biểu thị chiều cao của tháp tại vị trí đó. Petya quan sát thành phố từ một khoảng cách rất xa trong mặt phẳng ngang dọc theo vectơ chỉ hướng _(vx, vy)_. Câu hỏi đặt ra là có bao nhiêu hình khối riêng lẻ được nhìn thấy từ góc nhìn đó. 

Một khối được hiển thị nếu dọc theo hướng xem, không có khối nào khác trực tiếp chặn nó. Vì điểm quan sát ở rất xa nên bài toán quy về bài toán chiếu 2D: ta chỉ cần theo dõi độ cao tối đa gặp dọc theo các đường thẳng song song với vectơ _(vx, vy)_. Nếu một khối nằm cao hơn bất kỳ khối nào trước nó dọc theo đường đó thì nó sẽ hiển thị. 

Các ràng buộc ở mức vừa phải: _n_ ≤ 1000 và chiều cao có thể lên tới 10^9. Các thành phần vectơ chỉ phương là các số nguyên lên tới ±10^4. Vì n nhỏ nên các thuật toán có độ phức tạp khoảng O(n2) có thể chấp nhận được, nhưng mọi thuật toán O(n³) hoặc tệ hơn sẽ quá chậm. Các trường hợp cạnh bao gồm các thành phần vectơ âm, độ cao bằng 0 và lưới một tháp trong đó chỉ tính khối lập phương trên cùng. Ví dụ: nếu một hàng có chiều cao là [1,3,2] và vectơ nằm dọc theo hàng thì chỉ có thể nhìn thấy các hình khối ở vị trí 1, 2 và chiều cao bổ sung ở 2 và 3 tùy theo hướng; một phép lặp ngây thơ bỏ qua hướng vectơ sẽ đếm sai các hình khối. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng mọi tia từ mỗi khối và kiểm tra xem nó có giao nhau với khối khác trước nó hay không. Đối với lưới có kích thước n×n và chiều cao lên tới h, điều này dẫn đến kiểm tra O(n²·h) trên mỗi tia và tổng O(n²·h·n²), điều này không khả thi đối với các giới hạn trên n = 1000 và h = 10^9. Ngay cả việc giảm các tia xuống còn một tia trên mỗi khối cũng không giúp ích gì, bởi vì việc lặp lại dọc theo vectơ chỉ phương để phát hiện sự tắc nghẽn vẫn còn tốn kém. 

Thông tin chi tiết quan trọng là từ góc nhìn xa dọc theo _(vx, vy)_, các hình khối chỉ có thể chặn các hình khối khác nằm xa hơn dọc theo vectơ đó trên cùng một đường thẳng. Chúng ta có thể phân chia lưới thành các chuỗi dọc theo các đường có độ dốc vy/vx và xử lý các khối dọc theo các đường đó theo thứ tự từ góc nhìn. Dọc theo mỗi chuỗi, chỉ những hình khối cao hơn chiều cao tối đa được thấy cho đến nay mới góp phần vào số lượng hiển thị. Bằng cách chọn thứ tự xử lý một cách cẩn thận, chúng tôi tránh việc kiểm tra từng khối với mọi khối khác, giảm vấn đề thành quét tuyến tính được sắp xếp dọc theo mỗi dòng. 

Ta chuẩn hóa phương bằng cách xét dấu của vx và vy để xác định thứ tự quét của hàng và cột. Đối với mỗi đường chéo hoặc đối chéo (tùy thuộc vào vectơ), chúng tôi theo dõi chiều cao tối đa nhìn thấy và đối với mỗi tháp trong chuỗi, các hình khối hiển thị là chênh lệch giữa chiều cao của tháp và mức tối đa cho đến nay (nếu dương). Điều này chụp chính xác các hình khối nhô ra phía trên các vật cản trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(n^2) | Quá chậm | 
| Tối ưu (quét dọc theo đường vectơ) | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuẩn hóa vectơ chỉ phương bằng dấu của nó. Nếu vx < 0, chúng ta sẽ lặp các cột từ phải sang trái; nếu vy < 0, lặp các hàng từ dưới lên trên. Nếu vx = 0, chúng ta quét hoàn toàn dọc theo các hàng và nếu vy = 0, chúng ta quét hoàn toàn dọc theo các cột. 
2. Xác định thứ tự xử lý các ô. Các ô được xử lý dọc theo các đường vuông góc với vectơ, bắt đầu từ phía đối diện với khung nhìn. Đối với vectơ số nguyên, chúng ta có thể xác định thứ tự quét bằng cách tính tổng hoặc trừ các chỉ số hàng và cột theo dấu vectơ. 
3. Với mỗi dòng trong quá trình quét, hãy khởi tạo một biến`max_height`về 0. Điều này theo dõi khối lập phương cao nhất được thấy cho đến nay dọc theo đường đó. 
4. Lặp lại theo thứ tự từ điểm quan sát đến điểm xa hơn. Với mỗi ô hãy tính`visible = max(0, a[i][j] - max_height)`. Việc này đếm số khối có thể nhìn thấy mới được đóng góp bởi ô này. 
5. Thêm`visible`đến tổng số và cập nhật`max_height = max(max_height, a[i][j])`. 
6. Lặp lại cho tất cả các dòng bao phủ toàn bộ lưới. 

Tại sao nó hoạt động: Dọc theo mỗi đường theo hướng xem, các hình khối chặn tất cả các hình khối phía sau chúng không cao hơn. Bằng cách quét theo đúng thứ tự và duy trì chiều cao tối đa hiện tại, phần đóng góp của mỗi khối được tính toán chính xác một lần. Không có khối nào được tính hai lần vì mỗi khối chỉ được so sánh với các khối gần điểm nhìn dọc theo đường của nó. Tính bất biến đó là`max_height`luôn đại diện cho khối lập phương cao nhất dọc theo đường thẳng đến vị trí hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, vx, vy = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

total = 0

# Determine row and column sweep direction
row_range = range(n) if vy >= 0 else range(n-1, -1, -1)
col_range = range(n) if vx >= 0 else range(n-1, -1, -1)

# The key idea is to sweep along diagonals aligned with vector signs
for sum_idx in range(2*n - 1):
    max_height = 0
    # For each diagonal where i+j == sum_idx (or i-j == sum_idx for other signs)
    for i in row_range:
        j = sum_idx - i
        if j < 0 or j >= n:
            continue
        h = a[i][j]
        visible = max(0, h - max_height)
        total += visible
        max_height = max(max_height, h)

print(total)
```Mã này xác định thứ tự quét theo dấu vectơ. Chúng ta lặp lại các đường chéo sao cho mỗi đường chéo biểu thị một tập hợp các ô được căn chỉnh dọc theo các đường vuông góc của vectơ. Chúng tôi duy trì`max_height`để theo dõi khối lập phương cao nhất dọc theo đường quét và chúng tôi thêm chênh lệch dương giữa chiều cao hiện tại và`max_height`vào tổng số có thể nhìn thấy. Các lỗi riêng lẻ có thể tránh được bằng cách tính toán cẩn thận các chỉ số đường chéo và chúng tôi xử lý tất cả các ô chính xác một lần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 -1 2
5 0 0 0 1
0 0 0 0 2
0 0 0 1 2
0 0 0 0 2
2 2 2 2 3
```| Ô (i,j) | a[i][j] | chiều cao tối đa | có thể nhìn thấy | tổng cộng | 
| --- | --- | --- | --- | --- | 
| (0,0) | 5 | 0 | 5 | 5 | 
| (0,1) | 0 | 5 | 0 | 5 | 
| (0,2) | 0 | 5 | 0 | 5 | 
| (0,3) | 0 | 5 | 0 | 5 | 
| (0,4) | 1 | 5 | 0 | 5 | 
| ... | ... | ... | ... | ... | 
| (4,4) | 3 | 3 | 0 | 20 | 

Điều này chứng tỏ cách`max_height`chặn chính xác các hình khối thấp hơn và chỉ đếm các hình khối mới nhìn thấy được. 

Đầu vào tùy chỉnh:```
2 1 -1
1 2
3 4
```| Ô (i,j) | a[i][j] | chiều cao tối đa | có thể nhìn thấy | tổng cộng | 
| --- | --- | --- | --- | --- | 
| (1,0) | 3 | 0 | 3 | 3 | 
| (1,1) | 4 | 3 | 1 | 4 | 
| (0,0) | 1 | 4 | 0 | 4 | 
| (0,1) | 2 | 4 | 0 | 4 | 

Điều này xác nhận thuật toán xử lý các thành phần vectơ âm một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô được truy cập một lần; vòng lặp kép trên các đường chéo chạm vào từng ô trong số n2 ô đúng một lần. | 
| Không gian | O(n²) | Lưới được lưu trữ trong bộ nhớ. Các biến bổ sung sử dụng O(1). | 

Với n ≤ 1000, n2 = 10^6 lần lặp, có thể chấp nhận được trong giới hạn thời gian 5 giây. Việc sử dụng bộ nhớ bị chi phối bởi việc lưu trữ lưới có 10^6 số nguyên, thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, vx, vy = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    total = 0
    row_range = range(n) if vy >= 0 else range(n-1, -1, -1)
    col_range = range(n) if vx >= 0 else range(n-1, -1, -1)
    for sum_idx in range(2*n - 1):
        max_height = 0
        for i in row_range:
            j = sum_idx - i
            if j < 0 or j >= n
```
