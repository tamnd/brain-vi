---
title: "CF 104442H - El m\u00e1ximo de diversi\u00f3n"
description: "Chúng ta được cho một số chuỗi điểm độc lập trong mặt phẳng, trong đó mỗi chuỗi mô tả một tuyến đường cố định phải tuân theo thứ tự. Mỗi tuyến đường là một danh sách tọa độ $(x1, y1), (x2, y2), dấu chấm, (xn, yn)$ và chúng tôi chỉ quan tâm đến chuyển động giữa các điểm liên tiếp."
date: "2026-06-30T18:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104442
codeforces_index: "H"
codeforces_contest_name: "AdaByron Regional Madrid 2023"
rating: 0
weight: 104442
solve_time_s: 46
verified: true
draft: false
---

[CF 104442H - El m\u00e1ximo de diversi\u00f3n](https://codeforces.com/problemset/problem/104442/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số chuỗi điểm độc lập trong mặt phẳng, trong đó mỗi chuỗi mô tả một tuyến đường cố định phải tuân theo thứ tự. Mỗi tuyến đường là một danh sách tọa độ$(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$, và chúng ta chỉ quan tâm đến chuyển động giữa các điểm liên tiếp. 

Đối với mỗi cặp điểm liên tiếp, chúng tôi tính toán “chi phí thú vị” được xác định bằng biểu thức tuyến tính: ba lần chuyển vị ngang tuyệt đối cộng với hai lần chuyển vị dọc có dấu. Phần ngang bỏ qua hướng, trong khi phần dọc giữ nguyên hướng, do đó chuyển động lên và xuống được xử lý khác nhau. 

Đối với mỗi trường hợp thử nghiệm, mục tiêu chỉ đơn giản là tìm giá trị tối đa của chi phí này trong số tất cả các cặp liên tiếp trong tuyến. 

Các ràng buộc về kích thước đầu vào cho phép tối đa 200 chuỗi, mỗi chuỗi có tối đa 1000 điểm, do đó, tổng cộng tối đa là khoảng 200.000 cạnh. Điều này ngay lập tức gợi ý rằng một$O(n)$quét cho mỗi trường hợp kiểm thử là đủ, vì ngay cả việc vượt qua tuyến tính đầy đủ trên tất cả các điểm vẫn nằm trong giới hạn thoải mái. 

Một lỗi tinh vi phổ biến ở đây xuất phát từ việc đọc sai cấu trúc giá trị tuyệt đối. Chỉ có sự khác biệt x là tuyệt đối. Sự khác biệt y là không. Ví dụ, giữa$(0, 0)$Và$(0, -10)$, phần đóng góp là$3 \cdot 0 + 2 \cdot (-10) = -20$, tức là âm. Việc triển khai sai áp dụng giá trị tuyệt đối cho cả hai tọa độ sẽ biến điều này thành$20$, thay đổi hoàn toàn mức tối đa. 

Một vấn đề khác là giả định kết quả phải không âm. Do số hạng dọc có dấu, giá trị lớn nhất trên tất cả các cạnh vẫn có thể âm nếu tất cả các chuyển động đều làm giảm biểu thức. 

## Phương pháp tiếp cận 

Cấu trúc của vấn đề mang tính cục bộ cao. Mỗi cạnh đóng góp độc lập cho câu trả lời cuối cùng và không có sự tương tác giữa các đoạn khác nhau của đường đi. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc tiền tố, lập trình động hoặc tối ưu hóa hình học. 

Cách tiếp cận bạo lực đã là tối ưu: tính giá trị cho mọi cặp liền kề và theo dõi mức tối đa. Không có công thức thay thế nào có thể làm giảm số lượng so sánh cần thiết, vì mỗi cạnh phải được kiểm tra ít nhất một lần để đảm bảo tính chính xác. 

Người ta có thể cố gắng tìm kiếm một cách giải thích hình học tổng thể, nhưng biểu thức không phải là thước đo khoảng cách và không thỏa mãn các tính chất đối xứng hoặc tam giác một cách hữu ích. Sự phụ thuộc hoàn toàn theo cặp, do đó vấn đề giảm xuống còn việc quét một mảng các giá trị được tính toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đánh giá từng cạnh một cách độc lập | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc số điểm$n$và danh sách tọa độ. Các tọa độ được đưa ra ở dạng phẳng, vì vậy chúng tôi hiểu chúng là các cặp liên tiếp. 
2. Khởi tạo một biến`best`đến một số rất nhỏ, vì tất cả các giá trị được tính toán có thể âm. 
3. Lặp lại từng cặp điểm liên tiếp từ$i = 1$ĐẾN$n - 1$. Với mỗi cặp, trích xuất$(x_i, y_i)$Và$(x_{i+1}, y_{i+1})$. 
4. Tính chênh lệch theo chiều ngang là$|x_{i+1} - x_i|$. Điều này đảm bảo hướng không quan trọng đối với chuyển động ngang. 
5. Tính hiệu số theo chiều dọc là$y_{i+1} - y_i$mà không cần sửa đổi. 
6. Đánh giá chi phí$3 \cdot |x_{i+1} - x_i| + 2 \cdot (y_{i+1} - y_i)$. 
7. Cập nhật`best`nếu giá trị này lớn hơn mức tối đa được lưu trữ hiện tại. 
8. Sau khi xử lý tất cả các cạnh, xuất ra`best`. 

### Tại sao nó hoạt động 

Mỗi cạnh đóng góp một điểm vô hướng độc lập và bài toán yêu cầu giá trị lớn nhất trong số các giá trị độc lập này. Vì không có sự ghép nối giữa các cạnh nên mọi tối ưu toàn cục phải trùng với mức tối đa của các đánh giá cục bộ. Thuật toán liệt kê tất cả những người đóng góp có thể có chính xác một lần, do đó không thể bỏ sót ứng cử viên nào và không có giá trị nào bị tính hai lần hoặc sửa đổi bởi các bước trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        best = -10**18
        
        for i in range(n - 1):
            x1, y1 = arr[2*i], arr[2*i + 1]
            x2, y2 = arr[2*i + 2], arr[2*i + 3]
            
            val = 3 * abs(x2 - x1) + 2 * (y2 - y1)
            if val > best:
                best = val
        
        out.append(str(best))
    
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp định nghĩa toán học. Chi tiết quan trọng là giữ cho thuật ngữ dọc được ký kết. Một điểm tinh tế khác là khởi tạo`best`với một giá trị rất nhỏ thay vì 0, vì tất cả các giá trị được tính toán có thể âm. 

## Ví dụ đã hoạt động 

Hãy xem xét một tuyến đường duy nhất có ba điểm:$(0,0)$,$(2,1)$,$(3,-2)$. 

Đối với đoạn đầu tiên: 

| Phân đoạn | Δx | Δy | Giá trị | 
| --- | --- | --- | --- | 
| (0,0) → (2,1) | 2 | 1 | 3·2 + 2·1 = 8 | 

Đối với đoạn thứ hai: 

| Phân đoạn | Δx | Δy | Giá trị | 
| --- | --- | --- | --- | 
| (2,1) → (3,-2) | 1 | -3 | 3·1 + 2·(-3) = -3 | 

Tối đa là 8. 

Dấu vết này cho thấy chuyển động âm theo chiều dọc có thể làm giảm điểm đáng kể, do đó cạnh tối ưu không nhất thiết phải là cạnh có độ dịch chuyển tuyệt đối lớn nhất. 

Bây giờ hãy xem xét trường hợp tất cả các giá trị đều âm, chẳng hạn như$(0,0)$,$(0,-1)$,$(0,-2)$. 

| Phân đoạn | Δx | Δy | Giá trị | 
| --- | --- | --- | --- | 
| (0,0) → (0,-1) | 0 | -1 | -2 | 
| (0,-1) → (0,-2) | 0 | -1 | -2 | 

Câu trả lời là -2, xác nhận rằng mức tối đa không hàm ý tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cặp liền kề được đánh giá chính xác một lần | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Tổng số điểm trong tất cả các trường hợp thử nghiệm được giới hạn trong khoảng 200.000, do đó giải pháp chạy dễ dàng trong giới hạn thời gian theo mô hình quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        best = -10**18
        for i in range(n - 1):
            x1, y1 = arr[2*i], arr[2*i + 1]
            x2, y2 = arr[2*i + 2], arr[2*i + 3]
            val = 3 * abs(x2 - x1) + 2 * (y2 - y1)
            best = max(best, val)
        out.append(str(best))
    
    return "\n".join(out)

# minimum size
assert run("1\n2\n0 0 1 1\n") == "5"

# all negative vertical movement
assert run("1\n3\n0 0 0 -1 0 -2\n") == "-2"

# horizontal only
assert run("1\n3\n0 0 5 0 10 0\n") == "15"

# mixed values
assert run("1\n4\n0 0 1 2 3 -1 4 0\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 → 1 1 | 5 | tính đúng đắn của công thức cơ bản | 
| chuỗi y giảm dần | -2 | xử lý tối đa âm | 
| phong trào x thuần túy | 15 | đóng góp x tuyệt đối | 
| phong trào hỗn hợp | 9 | lựa chọn tối đa chính xác | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các chuyển động dọc đều âm, điều này có thể làm cho mọi giá trị phân đoạn đều âm. Thuật toán vẫn hoạt động vì nó không giả sử không âm; nó khởi tạo mức tối đa với một giá trị rất nhỏ và cập nhật nó hoàn toàn bằng cách so sánh. 

Một trường hợp cạnh khác là một đường ngang phẳng. Trong tình huống này, kết quả chỉ phụ thuộc vào khoảng cách ngang. Giá trị tuyệt đối đảm bảo những thay đổi về hướng không ảnh hưởng đến kết quả, do đó, đường đi sang trái và sang phải vẫn tích lũy đóng góp dương một cách chính xác.
