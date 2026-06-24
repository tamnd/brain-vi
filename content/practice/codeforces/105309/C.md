---
title: "CF 105309C - Showcase 3D của Shiori Novella"
description: "Lưới có thể được xem như một bảng xếp các ô nhị phân. Mỗi ô chứa 0 hoặc 1 và chúng ta được phép lật một ô, biến 0 thành 1 hoặc 1 thành 0. Mục tiêu là sửa đổi lưới sao cho không có hai ô lân cận, theo chiều ngang hoặc chiều dọc, có cùng giá trị."
date: "2026-06-23T14:52:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105309
codeforces_index: "C"
codeforces_contest_name: "CerealCodes III Novice Division"
rating: 0
weight: 105309
solve_time_s: 91
verified: false
draft: false
---

[CF 105309C - Buổi trình diễn 3D của Shiori Novella](https://codeforces.com/problemset/problem/105309/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lưới có thể được xem như một bảng xếp các ô nhị phân. Mỗi ô chứa 0 hoặc 1 và chúng ta được phép lật một ô, biến 0 thành 1 hoặc 1 thành 0. Mục tiêu là sửa đổi lưới sao cho không có hai ô lân cận, theo chiều ngang hoặc chiều dọc, có cùng giá trị. Nói cách khác, mọi cạnh giữa các ô liền kề phải kết nối các giá trị khác nhau sau khi chuyển đổi. 

Cấu hình cuối cùng hợp lệ chính xác là một mẫu bàn cờ, vì đó là cấu trúc duy nhất đảm bảo mọi cặp liền kề đều khác nhau. Nhiệm vụ là chọn một cấu hình gần nhất với lưới ban đầu, giảm thiểu số lượng ô bị đảo ngược. 

Các ràng buộc cho phép tối đa 10^4 trường hợp thử nghiệm với tổng số lên tới 10^6 ô trên tất cả các lưới. Điều này ngụ ý rằng giải pháp phải tuyến tính theo kích thước của lưới cho mỗi trường hợp thử nghiệm, vì bất kỳ quá trình lặp lại hệ số không đổi bậc hai hoặc thậm chí nặng nào cho mỗi trường hợp thử nghiệm sẽ quá chậm. Một lần duyệt qua mỗi lưới là đủ nếu chúng ta có thể biểu diễn cấu trúc mục tiêu một cách đơn giản. 

Trường hợp khó phát hiện khi lưới rất nhỏ. Đối với lưới 1 x 1, không có ràng buộc kề nào phải thỏa mãn, do đó không cần lật bất kể giá trị. Đối với lưới 1 x m hoặc n x 1, điều kiện bàn cờ giảm xuống các giá trị xen kẽ dọc theo một đường và logic hai mẫu tương tự vẫn được áp dụng. 

Một cạm bẫy tiềm ẩn khác là cố gắng khắc phục các vi phạm cục bộ bằng cách tham lam. Ví dụ: nếu chúng ta thấy hai người hàng xóm ngang nhau, việc lật một người có thể tạo ra vi phạm mới ở nơi khác. Điều này làm cho bất kỳ chiến lược địa phương nào đều không đáng tin cậy. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng gán các giá trị cho từng ô trong khi thực thi các ràng buộc lân cận một cách linh hoạt. Người ta có thể tưởng tượng việc thử tất cả các phép gán 0 và 1 có thể có cho mỗi ô, nhưng điều đó dẫn đến cấu hình 2^(n·m), điều này hoàn toàn không khả thi ngay cả đối với các lưới nhỏ. 

Một cách tiếp cận bạo lực có cấu trúc chặt chẽ hơn một chút sẽ là quyết định từng ô, phân nhánh xem có nên lật hay không trong khi vẫn duy trì tính hợp lệ với các nước láng giềng. Ngay cả với việc cắt tỉa, số lượng trạng thái vẫn tăng theo cấp số nhân vì mỗi quyết định đều ảnh hưởng đến các ràng buộc trong tương lai. Số lượng thao tác tăng lên khoảng 2^(n·m), vượt xa giới hạn. 

Quan sát quan trọng là bất kỳ lưới cuối cùng hợp lệ nào cũng phải thay thế các giá trị giống như bàn cờ. Một khi điều này được nhận ra, vấn đề sẽ giảm xuống còn việc so sánh lưới ban đầu với chỉ hai lưới mục tiêu có thể có. Một mẫu gán giá trị 0 cho tất cả các ô trong đó (i + j) là số chẵn và 1 nếu ngược lại, trong khi mẫu còn lại là phần bù của nó. Chúng tôi chỉ cần tính toán số lần lật cần thiết để phù hợp với từng mẫu và lấy mức tối thiểu. 

Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân trên tất cả các lưới đến số lần chuyển qua đầu vào không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n·m)) | O(n·m) | Quá chậm | 
| Tối ưu | O(n·m) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Chiến lược tối ưu

1. Lưu ý rằng mọi cấu hình hợp lệ đều phải xen kẽ các giá trị giữa các ô liền kề, do đó chỉ có thể có các mẫu bàn cờ. Điều này làm giảm không gian tìm kiếm xuống còn hai ứng viên. 
2. Xác định mẫu ứng viên đầu tiên trong đó ô ở vị trí (i, j) được kỳ vọng là 0 nếu (i + j) chẵn và 1 nếu ngược lại. Điều này tạo ra một cấu trúc xen kẽ cố định. 
3. Xác định mẫu ứng cử viên thứ hai là nghịch đảo chính xác của mẫu đầu tiên, trong đó các ô chẵn lẻ là 1 và các ô chẵn lẻ lẻ là 0. Điều này bao gồm bàn cờ hợp lệ duy nhất còn lại. 
4. Đối với mỗi ô trong lưới, hãy so sánh giá trị hiện tại của nó với cả hai mẫu ứng cử viên và đếm riêng các giá trị không khớp. Một sự không phù hợp tương ứng với một lần lật được yêu cầu. 
5. Số lần lật cần thiết cho mỗi mẫu là tổng số lần không khớp của mẫu đó. 
6. Câu trả lời cho lưới là số lượng tối thiểu của hai số không khớp. 

### Tại sao nó hoạt động 

Bất kỳ cấu hình hợp lệ nào cũng phải gán các giá trị khác nhau cho các ô liền kề, điều này buộc biểu đồ lưới phải có màu lưỡng cực nghiêm ngặt. Một lưới là lưỡng cực với một phân vùng duy nhất dựa trên tính chẵn lẻ của tọa độ, vì vậy mọi giải pháp hợp lệ được xác định hoàn toàn bằng việc lựa chọn giá trị được gán cho một lớp chẵn lẻ. Khi lựa chọn đó được cố định, tất cả các ô khác sẽ bị buộc phải thực hiện. Đây là lý do tại sao có chính xác hai mẫu hợp lệ và tại sao việc so sánh với cả hai mẫu sẽ đảm bảo số lần lật tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        
        cost0 = 0
        cost1 = 0
        
        for i in range(n):
            row = grid[i]
            for j in range(m):
                bit = row[j]
                
                expected0 = '0' if (i + j) % 2 == 0 else '1'
                expected1 = '1' if (i + j) % 2 == 0 else '0'
                
                if bit != expected0:
                    cost0 += 1
                if bit != expected1:
                    cost1 += 1
        
        out.append(str(min(cost0, cost1)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai đọc từng trường hợp thử nghiệm một cách độc lập và duy trì hai bộ đếm tương ứng với hai mẫu bàn cờ. Đối với mỗi ô, nó tính toán giá trị mà ô đó phải có theo mỗi mẫu bằng cách sử dụng tính chẵn lẻ của các chỉ số. Mỗi sự không phù hợp sẽ làm tăng chi phí tương ứng. 

Một chi tiết triển khai phổ biến cần thực hiện đúng là tránh tính toán lại các phép biến đổi lưới. Thay vì xây dựng các lưới dự kiến ​​đầy đủ, giải pháp sẽ tính toán các giá trị dự kiến ​​một cách nhanh chóng bằng cách sử dụng tính chẵn lẻ. Điều này giúp giảm thiểu mức sử dụng bộ nhớ và đảm bảo xử lý thời gian tuyến tính. 

Một sự tinh tế khác là đảm bảo rằng cả hai bộ đếm chi phí đều được duy trì độc lập. Việc trộn lẫn logic hoặc cố gắng sử dụng lại một bộ đếm sẽ dẫn đến kết quả không chính xác vì hai mẫu phân kỳ theo từng ô. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
2 3
010
111
```Chúng tôi tính toán chi phí cho cả hai mẫu. 

| Tế bào | Giá trị | (i+j)%2 | P0 dự kiến ​​| Chi phí P0 | Dự kiến ​​P1 | Chi phí P1 | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,0) | 0 | 0 | 0 | 0 | 1 | 1 | 
| (0,1) | 1 | 1 | 1 | 0 | 0 | 1 | 
| (0,2) | 0 | 0 | 0 | 0 | 1 | 1 | 
| (1,0) | 1 | 1 | 1 | 0 | 0 | 1 | 
| (1,1) | 1 | 0 | 0 | 1 | 1 | 0 | 
| (1,2) | 1 | 1 | 1 | 0 | 0 | 1 | 

Tổng chi phí P0 = 1, tổng chi phí P1 = 5, đáp án là 1. 

Điều này cho thấy một lưới gần bàn cờ chỉ yêu cầu một số chỉnh sửa nhỏ khi căn chỉnh với lựa chọn chẵn lẻ chính xác. 

### Ví dụ 2 

Lưới đầu vào:```
1 1
00
```| Tế bào | Giá trị | (i+j)%2 | P0 dự kiến ​​| Chi phí P0 | Dự kiến ​​P1 | Chi phí P1 | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,0) | 0 | 0 | 0 | 0 | 1 | 1 | 
| (0,1) | 0 | 1 | 1 | 1 | 0 | 0 | 

Tổng chi phí P0 = 1, tổng chi phí P1 = 1, đáp án là 1. 

Điều này chứng tỏ rằng cả hai hướng của bàn cờ đều có thể tối ưu như nhau tùy thuộc vào cách sắp xếp ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) cho mỗi trường hợp thử nghiệm | Mỗi ô được truy cập một lần và so sánh với hai mẫu | 
| Không gian | O(1) thêm | Chỉ các bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Tổng số ô được xử lý trên tất cả các trường hợp thử nghiệm tối đa là 10^6, do đó giải pháp chạy thoải mái trong giới hạn. Thuật toán chỉ thực hiện công việc theo thời gian không đổi trên mỗi ô, khiến nó hoạt động hiệu quả ngay cả trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            g = [input().strip() for _ in range(n)]
            c0 = c1 = 0
            for i in range(n):
                for j in range(m):
                    v = g[i][j]
                    e0 = '0' if (i+j)%2==0 else '1'
                    e1 = '1' if (i+j)%2==0 else '0'
                    c0 += (v != e0)
                    c1 += (v != e1)
            out.append(str(min(c0, c1)))
        return "\n".join(out)

    return solve()

# sample-like checks
assert run("1\n1 1\n0\n") == "0", "1x1 already valid"
assert run("1\n2 2\n00\n00\n") == "2", "all same grid"
assert run("1\n2 3\n010\n101\n") == "0", "perfect checkerboard"
assert run("2\n1 3\n000\n1 3\n111\n") == "1\n1", "line cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 0 | trường hợp cơ sở tối thiểu | 
| tất cả số không 2x2 | 2 | sự không phù hợp đồng phục tồi tệ nhất | 
| bàn cờ hoàn hảo | 0 | trường hợp tối ưu không lật | 
| nhiều hàng 1x3 | 1,1 | tính nhất quán giữa các trường hợp | 

## Vỏ cạnh 

Lưới 1 x 1 không có ràng buộc kề. Thuật toán đánh giá cả hai mẫu và tìm thấy điểm không khớp bằng 0 đối với ít nhất một mẫu, bởi vì cả hai mẫu đều giảm xuống một giá trị mong đợi duy nhất và cả hai lựa chọn đều có thể khớp bằng cách xây dựng giá trị tối thiểu trên hai mẫu. 

Một lưới thống nhất chẳng hạn như tất cả các số 0 trong khối 2 x 2 nêu bật sự cần thiết phải so sánh cả hai hướng của bàn cờ. Một mẫu căn chỉnh chính xác một nửa ô, dẫn đến chính xác hai điểm không khớp, trong khi mẫu còn lại hoạt động tương tự và mẫu tối thiểu nắm bắt chính xác các lần lật tối ưu. 

Một lưới xen kẽ hoàn toàn đã khớp với mẫu bàn cờ sẽ mang lại sự không khớp bằng 0 cho một trong hai phép gán chẵn lẻ. Thuật toán xác định chính xác điều này mà không cần bất kỳ cách viết hoa đặc biệt nào, vì các bộ đếm không khớp vẫn bằng 0 đối với cấu hình khớp.
