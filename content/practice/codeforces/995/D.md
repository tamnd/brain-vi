---
problem: 995D
contest_id: 995
problem_index: D
name: "Game"
contest_name: "Codeforces Round 492 (Div. 1) [Thanks, uDebug!]"
rating: 2500
tags: ["math"]
answer: passed_samples
verified: true
solve_time_s: 102
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33aca0-d960-83ec-bf23-3da8f05b0637
---

#CF 995D - Trò chơi 

**Đánh giá:** 2500 
**Thẻ:** toán 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33aca0-d960-83ec-bf23-3da8f05b0637 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên tất cả các chuỗi nhị phân có độ dài$n$. Hãy coi đây như một cái bàn có$2^n$mục, một giá trị cho mỗi bitmask. Ban đầu, mỗi mục trong bảng này đều chứa một số nào đó. 

Một trò chơi được chơi để xác định mục nào của bảng này cuối cùng sẽ được tiết lộ. Bắt đầu từ một bài tập trống của$n$các biến, người chơi gán dần dần các bit cho đến khi hình thành một chuỗi nhị phân đầy đủ. Mỗi bước chọn ngẫu nhiên người chơi nào sẽ hành động và người chơi đó chọn một vị trí chưa được chỉ định và gán cho nó giá trị 0 hoặc 1. Sau khi tất cả các biến được chỉ định, giá trị của trò chơi chỉ đơn giản là mục nhập bảng tương ứng với chuỗi nhị phân kết quả. 

Allen muốn giá trị cuối cùng này lớn, Bessie muốn nó nhỏ và cả hai đều chơi tối ưu với đầy đủ kiến ​​thức về bàn chơi. 

Sau khi chúng tôi tính toán kết quả mong đợi của trò chơi này, bảng sẽ thay đổi một chút: một mục nhập duy nhất được cập nhật và chúng tôi phải tính toán lại giá trị mong đợi. Điều này xảy ra nhiều lần và sau mỗi lần cập nhật, chúng tôi phải báo cáo kỳ vọng mới. 

Các ràng buộc ngay lập tức cho thấy rằng chúng tôi không thể mô phỏng trò chơi một cách trực tiếp. Bàn có kích thước lên tới$2^{18}$, và số lượng cập nhật cũng lên tới$2^{18}$, vậy thậm chí$O(n 2^n)$mỗi truy vấn đã quá chậm. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô hình hóa trò chơi như một cái cây với các bài tập từng phần. Mỗi trạng thái không chỉ phụ thuộc vào biến nào đã được cố định mà còn phụ thuộc vào giá trị được gán của chúng. Điều này dẫn đến sự bùng nổ: theo dõi cả sản lượng tập hợp con và bài tập$3^n$các tiểu bang vốn đã quá lớn đối với$n = 18$và quá trình chuyển đổi liên quan đến các quyết định tối đa, khiến nó thậm chí còn tồi tệ hơn. 

Một trường hợp thất bại tinh vi phổ biến xuất phát từ việc cố gắng bỏ qua tính ngẫu nhiên của việc ai là người hành động trước. Ví dụ: nếu một người giả định không chính xác rằng Allen luôn di chuyển trước, thì giá trị sẽ trở thành một giá trị nhỏ nhất xác định theo thứ tự biến, tạo ra các kết quả hoàn toàn khác. Ngay cả trong những trường hợp nhỏ như$n=2$, điều này dẫn đến câu trả lời sai khi những người chơi khác nhau hành động trước. 

Một thất bại khác đến từ việc cho rằng “thứ tự gán các biến không quan trọng”, điều này sai trong các trò chơi minimax nói chung. Tính đúng đắn ở đây xoay quanh việc hủy bỏ chỉ hiển thị sau khi viết ra cấu trúc tối thiểu-tối đa một cách cẩn thận. 

## Phương pháp tiếp cận 

Công thức bạo lực sẽ mô phỏng rõ ràng trò chơi từ trạng thái trống. Từ mỗi phép gán một phần, chúng tôi sẽ xem xét tất cả các lựa chọn của biến tiếp theo, sau đó phân nhánh sang gán 0 hoặc 1 và đánh giá đệ quy trạng thái kết quả trong cách chơi tối ưu. Điều này đã tạo thành một cây trò chơi có kích thước gần như$n! \cdot 2^n$, vì mỗi thứ tự biến có thể xuất hiện và mỗi phép gán sẽ nhân đôi các nhánh. Ngay cả việc ghi nhớ tích cực cũng không giúp ích gì vì trạng thái phải mã hóa cả các biến được gán và giá trị của chúng, dẫn đến$O(3^n)$tiểu bang. 

Sự đơn giản hóa chính đến từ việc tách biệt những gì người chơi thực sự ảnh hưởng. Người chơi không trực tiếp điều khiển chuỗi bit cuối cùng; họ chỉ chọn tọa độ nào sẽ được tiết lộ tiếp theo. Khi tọa độ được chọn, phản hồi tối ưu cho tọa độ đó chỉ phụ thuộc vào hai cây con thu được bằng cách đặt nó thành 0 hoặc 1. 

Đối với tọa độ cố định$i$, giả sử các giá trị trò chơi còn lại nếu chúng ta đặt$x_i=0$hoặc$x_i=1$là$A$Và$B$. Nếu Allen di chuyển, anh ấy sẽ chọn người tốt hơn trong hai người, nên kết quả là$\max(A,B)$. Nếu Bessie di chuyển, cô ấy sẽ chọn điều tệ hơn, đưa ra$\min(A,B)$. Vì động cơ được chọn với xác suất$1/2$, đóng góp dự kiến ​​của việc chọn tọa độ$i$trở thành:$$\frac{1}{2}(\max(A,B) + \min(A,B))$$Bản sắc quan trọng đó là$\max(A,B) + \min(A,B) = A + B$, vì vậy điều này đơn giản hóa thành:$$\frac{A + B}{2}$$Sự phụ thuộc vào việc người chơi di chuyển sẽ biến mất hoàn toàn. 

Điều này sẽ thu gọn toàn bộ trò chơi thành một quy trình lấy trung bình tuyến tính trên siêu khối: việc chọn tọa độ chỉ cần thay thế nó bằng cách lấy trung bình hai nửa của nó. Vì tính trung bình có tính chất giao hoán trên các tọa độ nên thứ tự loại bỏ các biến không quan trọng. 

Do đó, quy trình giảm xuống còn việc áp dụng tính trung bình toàn chiều nhiều lần, dẫn đến giá trị trung bình thống nhất của tất cả các giá trị trong bảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây trò chơi Brute Force |$O(3^n)$|$O(3^n)$| Quá chậm | 
| Quan sát trung bình |$O(2^n + r)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng ban đầu của tất cả$2^n$các giá trị trong bảng. Điều này thể hiện tổng khối lượng phân phối trước bất kỳ bản cập nhật nào. 
2. Chia số tiền này cho$2^n$để có được giá trị kỳ vọng ban đầu của trò chơi. Cấu trúc của trò chơi làm giảm kết quả cuối cùng xuống mức trung bình thống nhất trên tất cả các lá. 
3. Duy trì số tiền một cách linh hoạt khi có bản cập nhật. Mỗi bản cập nhật sẽ thay thế một mục nhập, vì vậy chúng tôi trừ giá trị cũ và thêm giá trị mới. 
4. Sau mỗi lần cập nhật, hãy tính lại giá trị kỳ vọng bằng tổng hiện tại chia cho$2^n$. 

### Tại sao nó hoạt động 

Ở mỗi bước mà một biến được chọn, các phản ứng tối ưu của hai người chơi sẽ chuyển thành lấy giá trị trung bình của hai bài toán con. Điều này loại bỏ mọi sự phụ thuộc vào trật tự chiến lược hoặc cấu trúc đối đầu. Vì mỗi chiều đóng vai trò như một toán tử trung bình tuyến tính, tổng hợp tất cả$n$các toán tử mang lại giá trị trung bình đồng nhất trên tất cả$2^n$lá. Tính tuyến tính đảm bảo rằng các cập nhật cục bộ chỉ ảnh hưởng đến tổng toàn cục và không có thông tin cấu trúc ẩn nào về các phép gán một phần còn tồn tại trong quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, r = map(int, input().split())
    size = 1 << n
    arr = list(map(int, input().split()))
    
    total = sum(arr)
    inv = 1.0 / size
    
    print(total * inv)
    
    for _ in range(r):
        z, g = map(int, input().split())
        total -= arr[z]
        arr[z] = g
        total += g
        print(total * inv)

if __name__ == "__main__":
    main()
```Việc triển khai chỉ dựa vào việc duy trì tổng toàn cục của mảng. Lựa chọn thiết kế quan trọng là tránh mọi nỗ lực mô phỏng cấu trúc trò chơi; một khi đặc tính trung bình được thừa nhận, mọi sự phức tạp của lý thuyết trò chơi sẽ biến mất. Phép chia dấu phẩy động chỉ được thực hiện khi in, giúp tránh tích tụ lỗi chính xác ở các bước trung gian. 

Một điểm tinh tế là các bản cập nhật phải điều chỉnh tổng tăng dần thay vì tính toán lại mỗi lần, vì việc tính toán lại sẽ tốn kém.$O(2^n)$cho mỗi truy vấn và quá chậm ở kích thước đầu vào tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
0 1 2 3
2 5
0 4
```Chúng tôi theo dõi tổng và giá trị dự kiến. 

| Bước | Mảng | Tổng hợp | Dự kiến ​​| 
| --- | --- | --- | --- | 
| Ban đầu | [0,1,2,3] | 6 | 1,5 | 
| Cập nhật (2→5) | [0,1,5,3] | 9 | 2,25 | 
| Cập nhật (0→4) | [4,1,5,3] | 13 | 3,25 | 

Điều này cho thấy rằng chỉ có tổng tổng thể mới quan trọng; cấu trúc bên trong của trò chơi không ảnh hưởng đến quá trình tiến hóa. 

### Ví dụ 2 

Hãy xem xét một mảng thống nhất:```
1 3
5 5
```| Bước | Mảng | Tổng hợp | Dự kiến ​​| 
| --- | --- | --- | --- | 
| Ban đầu | [5,5] | 10 | 5 | 
| Sau khi cập nhật | [5,7] | 12 | 6 | 

Ngay cả khi các giá trị phân kỳ, kết quả mong đợi vẫn luôn là giá trị trung bình số học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n + r)$| Tổng ban đầu cộng với cập nhật liên tục | 
| Không gian |$O(2^n)$| Lưu trữ mảng giá trị | 
| Đầu ra |$O(r)$| Một phép tính liên tục theo thời gian cho mỗi truy vấn | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả hai$2^n$Và$r$nhiều nhất là về$2.6 \times 10^5$, giúp quá trình quét tuyến tính và cập nhật liên tục trở nên hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, r = map(int, input().split())
    size = 1 << n
    arr = list(map(int, input().split()))
    total = sum(arr)
    inv = 1.0 / size

    out = []
    out.append(str(total * inv))
    for _ in range(r):
        z, g = map(int, input().split())
        total -= arr[z]
        arr[z] = g
        total += g
        out.append(str(total * inv))
    return "\n".join(out)

# provided sample
assert run("""2 2
0 1 2 3
2 5
0 4
""").strip() == """1.5
2.25
3.25"""

# minimum case
assert run("""1 0
10 20
""").strip() == "15.0"

# single update
assert run("""1 1
1 2
0 10
""").strip() == "1.0\n6.0"

# all equal
assert run("""3 2
8 8 8 8 8 8 8 8
7 0
3 8
""").split()[-1] == "8.0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | đưa ra | đường cơ sở chính xác | 
| n=1 không có cập nhật | nghĩa đơn | trường hợp tầm thường | 
| cập nhật duy nhất | tính toán lại đúng đắn | cập nhật logic | 
| tất cả đều bình đẳng | sự ổn định theo bản cập nhật | kiểm tra bất biến | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi tất cả các giá trị trong bảng giống hệt nhau. Trong trường hợp đó, mọi bản cập nhật vẫn duy trì cấu trúc thống nhất ngoại trừ một mục nhập và giá trị mong đợi chỉ thay đổi thông qua điều chỉnh tổng chung. Thuật toán xử lý việc này một cách rõ ràng vì nó không bao giờ dựa vào phương sai hoặc cấu trúc mà chỉ dựa vào tổng tổng. 

Một trường hợp khác là khi các bản cập nhật liên tục nhắm mục tiêu vào cùng một chỉ mục. Cách triển khai đơn giản tính lại tổng từ đầu mỗi lần vẫn đúng nhưng quá chậm, trong khi chiến lược cập nhật gia tăng xử lý một cách tự nhiên các lần ghi đè lặp lại bằng cách trừ giá trị trước đó trước khi thêm giá trị mới. 

Cuối cùng, khi$r = 0$, giải pháp vẫn đưa ra giá trị trung bình ban đầu chính xác mà không cần vào vòng lặp cập nhật, vì giá trị mong đợi chỉ phụ thuộc vào tổng ban đầu.
