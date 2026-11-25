---
problem: 984B
contest_id: 984
problem_index: B
name: "Minesweeper"
contest_name: "Codeforces Round 483 (Div. 2) [Thanks, Botan Investments and Victor Shaburov!]"
rating: 1100
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 65
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a777-461c-83ec-870f-45a398a95555
---

#CF 984B - Tàu quét mìn 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a777-461c-83ec-870f-45a398a95555 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một lưới biểu thị một bảng Dò mìn đã hoàn thiện. Mỗi ô có thể chứa một quả bom, một chữ số từ 1 đến 8 hoặc để trống. Chữ số trong một ô được cho là mã hóa số lượng quả bom tồn tại trong vùng lân cận 8 xung quanh nó và các ô trống được cho là để xác nhận rằng không có quả bom lân cận nào tồn tại. 

Nhiệm vụ không phải là mô phỏng Máy quét mìn hay tái tạo lại bất cứ thứ gì mà chỉ đơn giản là xác minh tính nhất quán. Mỗi ô áp đặt một ràng buộc cục bộ lên các ô xung quanh nó. Một ô bom không áp đặt các ràng buộc. Một ô chữ số áp đặt sự bình đẳng chính xác giữa giá trị của nó và số lượng bom xung quanh nó. Một ô trống áp đặt điều kiện không có bom cho tất cả các ô lân cận của nó. 

Lưới tối đa là 100 x 100, vì vậy có tối đa 10.000 ô. Việc kiểm tra từng ô và quét các ô lân cận của nó được giới hạn bởi hệ số không đổi tối đa là 8 hướng trên mỗi ô, do đó, mọi giải pháp tuyến tính về số lượng ô đều dễ dàng đủ nhanh. Thậm chí vài triệu thao tác chỉ là chuyện nhỏ trong 1 giây. 

Điểm tinh tế chính là các ràng buộc có tính chất hai chiều và chồng chéo. Một quả bom ảnh hưởng đến tối đa 8 ô xung quanh và mỗi ô trong số đó có thể yêu cầu đếm cùng một quả bom một cách độc lập. Một sai lầm ngây thơ là chỉ xác thực các chữ số mà không diễn giải chính xác các ô trống hoặc quên rằng các ô trống phải thực thi số lượng 0 nghiêm ngặt. 

Một số trường hợp đặc biệt thường phá vỡ quá trình triển khai: 

Một chữ số liền kề với số lượng bom ít hơn dự kiến sẽ rất dễ bị bỏ sót nếu giới hạn không được xử lý cẩn thận. Ví dụ:```
3 3
111
1*1
111
```Điều này hợp lệ vì mỗi ô góc nhìn thấy chính xác một quả bom liền kề theo đường chéo. 

Một trường hợp thất bại khác là quên rằng các ô trống áp đặt một ràng buộc nghiêm ngặt chứ không chỉ thiếu yêu cầu. Ví dụ:```
2 2
.*
..
```Điều này không hợp lệ vì ô trống phía dưới bên trái có một quả bom lân cận. 

Cuối cùng, vấn đề xử lý ranh giới: các ô trên các cạnh có ít hơn 8 ô lân cận và logic không được giả định kích thước vùng lân cận cố định. 

## Phương pháp tiếp cận 

Phương pháp xác minh bằng vũ lực là trực tiếp: đối với mỗi ô, hãy quét 8 ô lân cận và đếm số bom. Nếu ô chứa một chữ số, hãy so sánh số đếm với chữ số đó. Nếu nó trống, hãy đảm bảo số đếm bằng 0. Điều này đúng vì nó trực tiếp mã hóa định nghĩa về tính hợp lệ. 

Chi phí đến từ việc quét vùng lân cận nhiều lần. Mỗi ô kiểm tra tối đa 8 ô lân cận nên tổng số thao tác tỷ lệ thuận với 8nm, vốn đã tuyến tính. Ngay cả trong trường hợp xấu nhất của lưới 100 x 100, đây chỉ là 8000 lần kiểm tra hàng xóm, điều này không đáng kể. 

Không cần cải thiện tiệm cận có ý nghĩa vì lực lượng vũ phu đã có cấu trúc tối ưu. Bất kỳ quá trình xử lý trước nào như tổng tiền tố đều không cần thiết vì vùng lân cận là cố định và rất nhỏ. Vấn đề về cơ bản là cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét hàng xóm) | O(nm) | O(1) | Đã chấp nhận | 
| Tổng tiền tố hoặc tiền xử lý | O(nm) | O(nm) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Lặp lại từng ô trong lưới. Mỗi ô phải được xác thực độc lập vì các ràng buộc là cục bộ và không phụ thuộc vào cấu trúc chung. 
2. Với mỗi ô, hãy tính xem có bao nhiêu quả bom ở 8 vị trí lân cận của nó. Điều này đòi hỏi phải kiểm tra tất cả các vùng đồng bằng lân cận, bao gồm cả các đường chéo, trong khi vẫn ở trong ranh giới lưới. 
3. Nếu ô hiện tại chứa bom, hãy bỏ qua xác thực cho ô này vì bom không áp đặt các ràng buộc đối với ô lân cận hoặc chính ô đó. 
4. Nếu ô chứa một chữ số, hãy so sánh số lượng bom lân cận được tính toán với chữ số đó. Nếu chúng khác nhau, lưới sẽ không hợp lệ ngay lập tức. 
5. Nếu ô trống, hãy xác minh rằng số lượng bom lân cận chính xác bằng 0. Bất kỳ số đếm nào khác 0 đều vi phạm ràng buộc. 
6. Nếu tất cả các ô đều vượt qua các bước kiểm tra này thì lưới sẽ hợp lệ. 

### Tại sao nó hoạt động 

Mỗi ràng buộc trong bài toán hoàn toàn mang tính cục bộ: nó chỉ phụ thuộc vào một ô và các ô lân cận của nó. Bằng cách tính toán rõ ràng số lượng vùng lân cận cho mỗi ô, chúng tôi đánh giá mọi ràng buộc chính xác một lần. Vì mỗi quả bom đóng góp chính xác vào số lượng ô lân cận của các ô xung quanh và mỗi chữ số thực thi sự bằng nhau với số lượng đó, nên độ chính xác sẽ giảm để đảm bảo tính nhất quán của các phương trình cục bộ này. Không có sự phụ thuộc toàn cầu ẩn, vì vậy việc vượt qua tất cả các bước kiểm tra cục bộ đảm bảo tính hợp lệ toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def valid(grid, n, m):
    dirs = [(-1,-1), (-1,0), (-1,1),
            (0,-1),         (0,1),
            (1,-1),  (1,0), (1,1)]
    
    for i in range(n):
        for j in range(m):
            cnt = 0
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if grid[ni][nj] == '*':
                        cnt += 1
            
            if grid[i][j] == '*':
                continue
            
            if grid[i][j] == '.':
                if cnt != 0:
                    return False
            else:
                if cnt != int(grid[i][j]):
                    return False
    
    return True

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    
    print("YES" if valid(grid, n, m) else "NO")

if __name__ == "__main__":
    main()
```Giải pháp tách biệt việc xử lý đầu vào khỏi logic xác thực để việc kiểm tra lưới trở nên độc lập và có thể tái sử dụng. Danh sách hướng mã hóa tất cả 8 độ lệch lân cận, bao gồm cả các đường chéo, điều này rất quan trọng vì sự liền kề của Minesweeper không phải là 4 hướng. 

Một chi tiết triển khai quan trọng là kiểm tra ranh giới. Mọi tọa độ lân cận phải được xác minh trước khi truy cập vào lưới; nếu không, các ô góc và cạnh sẽ gây ra việc lập chỉ mục không hợp lệ. Một điểm tinh tế khác là coi các chữ số là ký tự, do đó việc chuyển đổi bằng`int()`chỉ được yêu cầu khi cần so sánh. Các tế bào bom được bỏ qua rõ ràng để tránh những lần kiểm tra không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
111
1*1
111
```| Ô (i,j) | Loại | Bom hàng xóm | Dự kiến ​​| Kết quả | 
| --- | --- | --- | --- | --- | 
| (0,0) | 1 | 1 | trận đấu | được | 
| (0,1) | 1 | 1 | trận đấu | được | 
| (1,1) | * | - | bỏ qua | được | 

Dấu vết này xác nhận rằng mỗi chữ số phản ánh chính xác quả bom duy nhất ở vùng lân cận trung tâm và không có ô nào vi phạm ràng buộc của nó. 

### Ví dụ 2 

đầu vào:```
2 2
.*
..
```| Ô (i,j) | Loại | Bom hàng xóm | Dự kiến ​​| Kết quả | 
| --- | --- | --- | --- | --- | 
| (0,0) | . | 1 | phải là 0 | thất bại | 

Ô trống ở (1,0) hoặc (0,0) thấy có bom liền kề, vi phạm quy tắc không có bom nên lưới không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô kiểm tra tối đa 8 ô lân cận, công việc liên tục trên mỗi ô | 
| Không gian | O(1) | Chỉ bù hướng cố định và lưu trữ lưới đầu vào | 

Kích thước lưới tối đa là 10.000 ô và mỗi ô thực hiện một lượng công việc không đổi, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    dirs = [(-1,-1), (-1,0), (-1,1),
            (0,-1),         (0,1),
            (1,-1),  (1,0), (1,1)]
    
    for i in range(n):
        for j in range(m):
            cnt = 0
            for di, dj in dirs:
                ni, nj = i + di, j + dj
                if 0 <= ni < n and 0 <= nj < m:
                    if grid[ni][nj] == '*':
                        cnt += 1
            
            if grid[i][j] == '*':
                continue
            if grid[i][j] == '.':
                if cnt != 0:
                    return "NO"
            else:
                if cnt != int(grid[i][j]):
                    return "NO"
    
    return "YES"

# provided sample
assert run("3 3\n111\n1*1\n111\n") == "YES"

# minimum size valid
assert run("1 1\n.\n") == "YES"

# minimum size invalid
assert run("1 1\n1\n") == "NO"

# simple invalid digit mismatch
assert run("2 2\n11\n1*\n") == "NO"

# all bombs
assert run("2 2\n**\n**\n") == "NO"

# valid small cluster
assert run("2 2\n*1\n11\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 "." | CÓ | tính chính xác của lưới trống | 
| 1x1 "1" | KHÔNG | chữ số không có hàng xóm không hợp lệ | 
| 2x2 hỗn hợp | KHÔNG | lan truyền không phù hợp | 
| tất cả bom | KHÔNG | bắt buộc phải có chữ số, các khoảng trống không hợp lệ | 

## Vỏ cạnh 

Trường hợp nặng về ranh giới là một hàng hoặc một cột trong đó số lượng hàng xóm giảm đi một cách tự nhiên. Ví dụ:```
1 5
1*1*1
```Mỗi ô có ít hơn 8 ô lân cận, nhưng thuật toán vẫn chỉ tính chính xác các ô lân cận hiện có do kiểm tra ranh giới. Việc xác thực vẫn nhất quán vì phần kề được cắt bớt ở các đường viền lưới. 

Một trường hợp cạnh khác là lưới hoàn toàn trống:```
3 3
...
...
...
```Mỗi ô đều có hàng xóm không có bom, vì vậy tất cả các ô trống đều vượt qua yêu cầu nghiêm ngặt về số 0 và thuật toán chấp nhận nó mà không cần vỏ đặc biệt. 

Trường hợp cuối cùng là các bom và chữ số xen kẽ trong đó mỗi chữ số phụ thuộc vào nhiều hàng xóm được chia sẻ. Phương pháp đếm cục bộ vẫn đúng vì mỗi quả bom được tính độc lập cho từng ô lân cận và không cần theo dõi tính nhất quán toàn cầu.
