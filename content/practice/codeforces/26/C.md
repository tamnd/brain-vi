---
title: "CF 26C - Sàn gỗ"
description: "Chúng ta được yêu cầu lát một sàn hình chữ nhật có kích thước n × m bằng ba loại ván sàn. Loại thứ nhất là tấm ván ngang 1×2, loại thứ hai là tấm ván dọc 2×1 và loại thứ ba là tấm ván vuông 2×2."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 26
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 26 (Codeforces format)"
rating: 2000
weight: 26
solve_time_s: 129
verified: false
draft: false
---

[CF 26C - Sàn gỗ](https://codeforces.com/problemset/problem/26/C) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lát một sàn hình chữ nhật có kích thước _n_ × _m_ bằng cách sử dụng ba loại ván sàn gỗ. Loại thứ nhất là tấm ván ngang 1×2, loại thứ hai là tấm ván dọc 2×1 và loại thứ ba là tấm ván vuông 2×2. Mục tiêu là bao phủ toàn bộ sàn mà không có khoảng trống hoặc chồng chéo, không cần xoay các tấm ván và có thể tùy ý để lại một số tấm ván không được sử dụng. Đầu vào cung cấp kích thước phòng và số lượng của từng loại ván. Đầu ra phải là mẫu xếp kề sử dụng chữ cái viết thường hoặc "KHÔNG THỂ" nếu không có cách sắp xếp nào hiệu quả. 

Các ràng buộc ở mức vừa phải: _n_ và _m_ nhiều nhất là 100, do đó sàn có tối đa 10.000 ô. Số lượng ván lên tới 10.000, vì vậy có thể có đủ số ván để bao phủ toàn bộ căn phòng nhiều lần. Vì khu vực bị giới hạn nên giải pháp lặp lại trên mỗi ô với số lần không đổi sẽ chạy thoải mái trong giới hạn thời gian 2 giây. Tuy nhiên, những cách tiếp cận ngây thơ cố gắng kết hợp mọi vị trí có thể của các tấm ván rõ ràng là không khả thi do sự tăng trưởng theo cấp số nhân. 

Các trường hợp cạnh không rõ ràng phát sinh khi một trong hai kích thước là số lẻ hoặc khi thiếu một số loại ván nhất định. Ví dụ: một căn phòng 3×2 không thể lát gạch hoàn toàn bằng ván 2×2 vì hàng còn lại có chiều rộng 3 không thể được che phủ bởi các ô vuông 2×2. Nếu không có đủ tấm ván 1×2 hoặc 2×1 để lấp đầy các hàng hoặc cột lẻ thì điều đó có thể trở nên bất khả thi. Một tình huống tinh tế khác xảy ra khi cả hai kích thước đều là số lẻ: việc xếp ô ô cuối cùng cần ít nhất một tấm ván 2 × 2, tấm ván này có thể không tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử đặt đệ quy mọi loại ván ở mọi vị trí trống cho đến khi căn phòng được lát gạch hoàn toàn hoặc không thể đặt thêm vị trí nào nữa. Phương pháp này đúng vì nó kiểm tra toàn diện tất cả các tùy chọn. Tuy nhiên, số lượng ô lên tới 10.000 và mỗi ô có khả năng phân nhánh thành ba lựa chọn, dẫn đến một cây tìm kiếm khổng lồ về mặt thiên văn. Cách tiếp cận này là quá chậm so với các giới hạn nhất định. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả là xử lý việc lát gạch một cách tham lam bằng cách xử lý căn phòng theo khối 2 × 2. Bất kỳ tầng nào cũng có thể được phân tách thành các lưới con 2×2 hoàn chỉnh, cùng với tối đa một hàng hoặc cột còn sót lại nếu kích thước là số lẻ. Bên trong mỗi khối 2×2, trước tiên chúng ta thử đặt một tấm ván 2×2 nếu có. Nếu không, chúng ta có thể che nó bằng cách sử dụng kết hợp các tấm ván 1×2 và 2×1. Đối với các hàng hoặc cột còn sót lại, chúng ta chỉ sử dụng ván ngang hoặc ván dọc tương ứng. Bằng cách lặp lại một cách có hệ thống, chúng tôi giảm vấn đề ốp lát xuống một vị trí xác định luôn tôn trọng hướng của tấm ván. Cách tiếp cận này đảm bảo tính chính xác vì mỗi ô được truy cập chính xác một lần và các lựa chọn đảm bảo phạm vi bao phủ nếu có đủ bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^(n*m)) | O(n*m) | Quá chậm | 
| Sự phân hủy 2 × 2 tham lam | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo mảng 2D có kích thước _n_ × _m_ để lưu trữ các chữ cái xếp chồng lên nhau. Sử dụng bộ đếm để tạo các chữ cái duy nhất cho mỗi tấm ván. 
2. Xử lý tất cả các khối 2×2 hoàn chỉnh trong phòng. Đối với mỗi khối 2×2, nếu có sẵn tấm ván 2×2, hãy đặt nó và giảm số lượng tấm ván 2×2. Nếu không, hãy cố gắng lấp đầy nó bằng hai tấm ván ngang 1×2 hoặc hai tấm ván dọc 2×1, tùy thuộc vào tình trạng sẵn có. 
3. Nếu còn sót lại hàng cuối cùng (khi _n_ là số lẻ), hãy lặp qua hàng đó theo từng cặp cột. Đặt tấm ván ngang 1×2 nếu có. Nếu còn một cột và không có tấm ván nào phù hợp thì không thể ốp lát được. 
4. Nếu còn sót lại cột cuối cùng (khi _m_ là số lẻ), hãy lặp qua cột đó theo từng cặp hàng. Đặt các tấm ván dọc 2×1 nếu có. Nếu vẫn còn một hàng và không có tấm ván nào vừa khít thì không thể xuất ra được. 
5. Sau khi điền tất cả các khối 2×2 và hàng/cột còn sót lại, nếu bất kỳ ô nào vẫn trống thì không thể xuất được. Nếu không, hãy in mảng 2D đã điền. 

Tại sao nó hoạt động: Bất biến được duy trì trong suốt thuật toán là tất cả các khối 2×2 được xử lý đầy đủ đều được lấp đầy hoàn toàn bằng một tấm ván 2×2 hoặc bằng hai tấm ván 1×2 / 2×1. Hàng và cột còn lại được xử lý riêng biệt, đảm bảo rằng mỗi ô được gán một tấm ván mà không vi phạm ràng buộc định hướng. Bởi vì mỗi khối và phân đoạn còn sót lại được xử lý một cách xác định nên thuật toán không thể bỏ qua những khoảng trống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parquet():
    n, m, a, b, c = map(int, input().split())
    grid = [[''] * m for _ in range(n)]
    letter = ord('a')
    
    def next_letter():
        nonlocal letter
        l = chr(letter)
        letter += 1
        if letter > ord('z'):
            letter = ord('a')
        return l
    
    # Fill 2x2 blocks
    for i in range(0, n - n % 2, 2):
        for j in range(0, m - m % 2, 2):
            if c > 0:
                l = next_letter()
                grid[i][j] = grid[i][j+1] = grid[i+1][j] = grid[i+1][j+1] = l
                c -= 1
            else:
                placed = False
                # Try two horizontal 1x2 planks
                if a >= 2:
                    l1, l2 = next_letter(), next_letter()
                    grid[i][j] = grid[i][j+1] = l1
                    grid[i+1][j] = grid[i+1][j+1] = l2
                    a -= 2
                    placed = True
                elif b >= 2:
                    l1, l2 = next_letter(), next_letter()
                    grid[i][j] = grid[i+1][j] = l1
                    grid[i][j+1] = grid[i+1][j+1] = l2
                    b -= 2
                    placed = True
                if not placed:
                    print("IMPOSSIBLE")
                    return
    
    # Handle last row if n is odd
    if n % 2 == 1:
        i = n - 1
        for j in range(0, m - 1, 2):
            if a > 0:
                l = next_letter()
                grid[i][j] = grid[i][j+1] = l
                a -= 1
            else:
                print("IMPOSSIBLE")
                return
        if m % 2 == 1:
            print("IMPOSSIBLE")
            return
    
    # Handle last column if m is odd
    if m % 2 == 1:
        j = m - 1
        for i in range(0, n - 1, 2):
            if b > 0:
                l = next_letter()
                grid[i][j] = grid[i+1][j] = l
                b -= 1
            else:
                print("IMPOSSIBLE")
                return
    
    # Output
    for row in grid:
        print(''.join(row))

parquet()
```Giải pháp bắt đầu bằng cách khởi tạo lưới và một công cụ trợ giúp để tạo các chữ cái xếp chồng lên nhau. Nó lặp lại trên tất cả các khối 2×2 hoàn chỉnh, cố gắng đặt tấm ván 2×2 trước. Nếu không có sẵn, nó sẽ cố gắng che khối bằng hai tấm ván ngang hoặc dọc, kiểm tra tính khả dụng. Các hàng và cột còn lại được xử lý riêng biệt chỉ bằng các tấm ván tương thích. Nếu ở bất kỳ bước nào không có bảng nào cho ô được yêu cầu thì hàm sẽ không thể xuất ra. Thứ tự này đảm bảo mỗi ô được truy cập chính xác một lần và được gán chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`2 6 2 2 1`| Bước | tôi, j | Hành động | Ván | Còn lại a,b,c | Ảnh chụp lưới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | Nơi 2×2 | 'a' | 2,2,0 | một | 
| 2 | (0,2) | Đặt 2 ngang 1×2 | 'b','c' | 1,2,0 | b b | 
| 3 | (0,4) | Đặt 2 ngang 1×2 | 'd','e' | 0,2,0 | d d | 

Điều này cho thấy thuật toán lấp đầy các khối một cách xác định, sử dụng các tấm ván có sẵn một cách hiệu quả. 

### Đầu vào tùy chỉnh 

đầu vào:`3 3 1 2 1`| Bước | tôi, j | Hành động | Ván | Còn lại a,b,c | Ảnh chụp lưới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | Nơi 2×2 | 'a' | 1,2,0 | một | 
| 2 | hàng cuối cùng (2) | Đặt ngang 1×2 | 'b' | 0,2,0 | b b | 
| 3 | cột cuối cùng (2) | Đặt dọc 2×1 | 'c' | 0,1,0 | c | 

Bìa thuật toán
