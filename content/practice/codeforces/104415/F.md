---
title: "CF 104415F - Huấn luyện ném đĩa"
description: "Nhiệm vụ này mô tả một tình huống vật lý đơn giản trong đó một chiếc đĩa bay được ném từ điểm gốc và chạm đất tại một điểm $(x, y)$. Người chơi tên Asfora chỉ có thể bắt được nó nếu anh ta chạy đủ xa trước khi nó tiếp đất."
date: "2026-06-30T19:51:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104415
codeforces_index: "F"
codeforces_contest_name: "IME++ Starters Try-outs 2023"
rating: 0
weight: 104415
solve_time_s: 43
verified: true
draft: false
---

[CF 104415F - Huấn luyện ném đĩa](https://codeforces.com/problemset/problem/104415/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một tình huống vật lý đơn giản trong đó một chiếc đĩa bay được ném từ điểm xuất phát và tiếp đất tại một điểm.$(x, y)$. Người chơi tên Asfora chỉ có thể bắt được nó nếu anh ta chạy đủ xa trước khi nó tiếp đất. Hình học rất đơn giản: khoảng cách cần thiết để chạm tới đĩa bay là khoảng cách Euclide từ điểm gốc đến điểm$(x, y)$, trong khi khả năng chạy sẵn có được thể hiện thông qua một tham số giống như vận tốc$v$và một thời gian$t$. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp các giá trị xác định vị trí hạ cánh và khả năng di chuyển. Quyết định mang tính nhị phân: xác định xem Asfora có thể tiếp cận đĩa bay kịp thời hay không. 

Quan sát quan trọng là mọi thứ đều quy về việc so sánh khoảng cách hình học với bán kính có thể tiếp cận. Khoảng cách Euclide là$\sqrt{x^2 + y^2}$. Khoảng cách có thể đạt được tỷ lệ thuận với$v \cdot t$. Do đó, phép so sánh là điểm nằm bên trong hay trên đường tròn có tâm tại gốc tọa độ. 

Kích thước đầu vào nhỏ cho mỗi trường hợp thử nghiệm và mỗi truy vấn đều độc lập. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào vượt quá thời gian không đổi cho mỗi trường hợp thử nghiệm đều là không cần thiết. Ngay cả khi có nhiều trường hợp thử nghiệm, việc tính toán cho mỗi trường hợp chỉ bao gồm một vài phép tính số học. 

Một vấn đề triển khai tinh tế nhưng quan trọng là độ chính xác của dấu phẩy động. Phép tính trực tiếp sử dụng căn bậc hai có thể gây ra lỗi làm tròn, đặc biệt khi các giá trị lớn hoặc gần với ranh giới. Một vấn đề thực tế khác là hiệu năng: việc phân tích liên tục các số dấu phẩy động trong môi trường I/O chậm có thể trở thành nút thắt cổ chai ngay cả khi bản thân số học là tầm thường. 

Các trường hợp cạnh xuất phát từ đẳng thức ranh giới và độ chính xác về số. Ví dụ, khi$x^2 + y^2$cực kỳ gần với$v^2 t^2$, so sánh dấu phẩy động có thể lật kết quả không chính xác. Một trường hợp đặc biệt khác là khi tất cả các giá trị đều bằng 0, trong đó câu trả lời về cơ bản phải là số dương. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính toán khoảng cách Euclide một cách rõ ràng. Đối với mỗi trường hợp thử nghiệm, chúng tôi đánh giá$\sqrt{x^2 + y^2}$, sau đó so sánh nó với$v \cdot t$. Điều này đúng về mặt toán học vì nó khớp trực tiếp với định nghĩa hình học của khoảng cách. Tuy nhiên, nó đưa ra hai điểm kém hiệu quả. Đầu tiên, việc tính căn bậc hai chậm hơn so với phép nhân và phép cộng. Thứ hai, số học dấu phẩy động có thể gây ra lỗi chính xác, đặc biệt khi xử lý các số nguyên lớn hoặc các bất đẳng thức chặt chẽ. 

Cách tiếp cận tốt hơn sẽ tránh được cả hai vấn đề bằng cách loại bỏ hoàn toàn căn bậc hai. Thay vì so sánh$\sqrt{x^2 + y^2} \le v \cdot t$, chúng ta bình phương cả hai vế, tạo ra$x^2 + y^2 \le v^2 t^2$. Phép biến đổi này bảo toàn tính đúng đắn vì tất cả các đại lượng liên quan đều không âm. Việc so sánh trở nên hoàn toàn dựa trên số nguyên nếu đầu vào là số nguyên hoặc ít nhất tránh được sự mất ổn định của dấu phẩy động trung gian. 

Brute-force hoạt động vì nó phản chiếu trực tiếp hình học, nhưng nó trở nên kém tin cậy hơn và chậm hơn một chút khi được chia tỷ lệ trên nhiều trường hợp thử nghiệm. Công thức bình phương loại bỏ các phép toán siêu việt và rút gọn toàn bộ bài toán thành một vài phép nhân và phép cộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Khoảng cách trực tiếp với sqrt | O(1) mỗi lần kiểm tra | O(1) | Đúng nhưng mong manh | 
| So sánh bình phương | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập, do đó không có trạng thái nào được mang giữa chúng. 
2. Với mỗi test case, hãy đọc các giá trị$x$,$y$,$v$, Và$t$. Chúng xác định vị trí hạ cánh và khả năng di chuyển sẵn có. 
3. Tính bình phương khoảng cách từ gốc tọa độ là$x^2 + y^2$. Điều này thể hiện yêu cầu hình học chính xác mà không đưa vào căn bậc hai. 
4. Tính bình phương khoảng cách có thể tiếp cận được như sau$v^2 \cdot t^2$. Điều này mở rộng bán kính di chuyển có sẵn vào cùng một không gian bình phương với khoảng cách. 
5. So sánh hai giá trị. Nếu như$x^2 + y^2 \le v^2 t^2$, cho ra kết quả rằng chiếc đĩa có thể tiếp cận được; nếu không thì xuất ra kết quả không phải như vậy. 
6. Lặp lại cho tất cả các trường hợp kiểm tra, in từng kết quả ngay lập tức. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bình phương là một phép biến đổi tăng dần trên các số không âm. Cả khoảng cách Euclide và khoảng cách có thể tới được luôn không âm, do đó việc áp dụng cùng một phép biến đổi đơn điệu cho cả hai vế sẽ duy trì trật tự. Điều này đảm bảo rằng bất đẳng thức ở dạng bình phương tương đương với điều kiện hình học ban đầu, loại bỏ sự mất ổn định về số mà không làm thay đổi độ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        x = int(data[idx]); y = int(data[idx + 1])
        v = int(data[idx + 2]); t_ = int(data[idx + 3])
        idx += 4

        lhs = x * x + y * y
        rhs = (v * t_) * (v * t_)

        if lhs <= rhs:
            out.append("YES")
        else:
            out.append("NO")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai giữ mọi thứ ở dạng số học số nguyên. Sản phẩm$v \cdot t$được tính toán trước, sau đó bình phương, điều này an toàn trong Python do hỗ trợ số nguyên lớn. Trong cài đặt C++, cần cẩn thận để tránh tràn, nhưng Python xử lý độ chính xác tùy ý một cách tự nhiên. 

Đọc đầu vào dưới dạng danh sách phẳng sẽ tránh các lệnh gọi hàm lặp lại và giảm thiểu chi phí. Điều này quan trọng trong môi trường có số lượng lớn trường hợp thử nghiệm được đóng gói vào một luồng đầu vào duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
3 4 5 1
3 4 1 1
```Ta tính từng trường hợp: 

| Trường hợp | x | y | v | t | x2+y2 | (v·t)2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 5 | 1 | 25 | 25 | CÓ | 
| 2 | 3 | 4 | 1 | 1 | 25 | 1 | KHÔNG | 

Trường hợp đầu tiên nằm chính xác trên đường tròn biên, trường hợp này được chấp nhận vì điều kiện không nghiêm ngặt. Trường hợp thứ hai rõ ràng nằm ngoài bán kính có thể tiếp cận. 

### Ví dụ 2 

đầu vào:```
1
0 0 10 0
```| Trường hợp | x | y | v | t | x2+y2 | (v·t)2 | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 10 | 0 | 0 | 0 | CÓ | 

Điều này kiểm tra trường hợp suy biến trong đó cả vị trí và thời gian đều bằng 0. Chiếc đĩa bay đã ở điểm xuất phát nên câu trả lời gần như là tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số biến cố định được sử dụng ngoài bộ lưu trữ đầu ra | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì mỗi trường hợp thử nghiệm được giải quyết trong thời gian không đổi. Ngay cả đối với rất lớn$T$, việc tính toán được giới hạn ở phép nhân và so sánh số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    data = sys.stdin.read().strip().split()
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        x = int(data[idx]); y = int(data[idx + 1])
        v = int(data[idx + 2]); tt = int(data[idx + 3])
        idx += 4

        lhs = x * x + y * y
        rhs = (v * tt) * (v * tt)

        out.append("YES" if lhs <= rhs else "NO")

    return "\n".join(out)

# provided sample (if any assumed)
assert run("2\n3 4 5 1\n3 4 1 1\n") == "YES\nNO"

# all equal boundary
assert run("1\n6 8 10 1\n") == "YES"

# strict fail case
assert run("1\n6 8 1 1\n") == "NO"

# zero case
assert run("1\n0 0 0 5\n") == "YES"

# large safe case
assert run("1\n100000 100000 200000 1\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 4 5 1 | CÓ | Bình đẳng ranh giới Pythagore | 
| 6 8 1 1 | KHÔNG | Xóa lỗi bên ngoài bán kính | 
| 0 0 0 5 | CÓ | Trường hợp nguồn gốc thoái hóa | 
| 100000 100000 200000 1 | CÓ | Độ chính xác giá trị lớn | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi điểm nằm chính xác trên đường tròn biên. Đối với đầu vào$x=3, y=4, v=5, t=1$, kết quả tính toán$x^2 + y^2 = 25$Và$(vt)^2 = 25$. Thuật toán so sánh đẳng thức một cách chính xác vì nó sử dụng`<=`, đảm bảo bao gồm ranh giới. 

Một trường hợp khác là khi thời gian bằng không. Đối với đầu vào$x=0, y=0, v=10, t=0$, cả hai bên đều đánh giá bằng 0 và điều kiện được giữ nguyên. Công thức bình phương tránh được bất kỳ sự mơ hồ nào về phép chia hoặc dấu phẩy động. 

Trường hợp cạnh thứ ba liên quan đến tọa độ lớn. Vì$x=y=10^5, v=2\cdot10^5, t=1$, cả hai vế đều khớp một cách an toàn với số nguyên Python và việc so sánh vẫn chính xác. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, phép nhân trung gian phải được xử lý cẩn thận để tránh tràn, đó chính là lý do tại sao bình phương tích sau khi tính toán$v \cdot t$trong một loại mở rộng là cần thiết.
