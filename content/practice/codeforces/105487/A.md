---
title: "CF 105487A - Hộp"
description: "Một hình hộp chữ nhật được đặt trong không gian 3D với mặt đáy nằm phẳng trên một mặt phẳng nằm ngang đã biết. Chiều cao của hộp đã được cho trước nên mặt trên chỉ là bản dịch dọc của mặt dưới."
date: "2026-06-23T19:40:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105487
codeforces_index: "A"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Female Onsite (2024\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a)"
rating: 0
weight: 105487
solve_time_s: 80
verified: true
draft: false
---

[CF 105487A - Hộp](https://codeforces.com/problemset/problem/105487/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một hình hộp chữ nhật được đặt trong không gian 3D với mặt đáy nằm phẳng trên một mặt phẳng nằm ngang đã biết. Chiều cao của hộp đã được cho trước nên mặt trên chỉ là bản dịch dọc của mặt dưới. Hình chữ nhật phía dưới được chỉ định một cách gián tiếp: thay vì cho cả bốn góc, chúng ta được cho hai góc đối diện trên cùng một mặt phẳng ngang. Từ đó, hình chữ nhật đầy đủ được xác định, căn chỉnh với trục x và y. 

Mỗi truy vấn đưa ra một điểm trong không gian và chúng ta phải xác định xem điểm này nằm bên trong hay trên bề mặt của hộp. 

Do đó, cấu trúc đơn giản về mặt hình học: một hộp được căn chỉnh theo trục 3D có đáy là một hình chữ nhật thẳng hàng theo trục trong mặt phẳng xy và có phạm vi dọc được cố định bởi tham số chiều cao. 

Các ràng buộc rất nhỏ, tối đa là một nghìn truy vấn. Điều này ngay lập tức loại trừ bất cứ điều gì ngoài việc xử lý thời gian liên tục cho mỗi truy vấn. Bất kỳ giải pháp nào cố gắng phân tách hình học cho mỗi truy vấn hoặc mô phỏng theo khối lượng sẽ là chi phí không cần thiết. Vấn đề giảm xuống còn việc trích xuất các ranh giới khoảng một lần, sau đó trả lời từng truy vấn với số lượng so sánh cố định. 

Trường hợp cạnh tinh tế xuất phát từ cách xác định hình chữ nhật cơ sở. Hai điểm đã cho là các góc đối diện nhau, nhưng không có gì đảm bảo thứ tự, do đó tọa độ có thể nhỏ hơn hoặc lớn hơn. Việc triển khai ngây thơ giả định các điểm cuối được đặt hàng sẽ không thành công trên các đầu vào được hoán đổi. Một trường hợp góc khác là khi chiều cao bằng 0, hộp sẽ thu gọn thành một hình chữ nhật phẳng trong không gian 3D. Các điểm chính xác trên mặt phẳng đó vẫn phải được xét bên trong. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ cố gắng xây dựng lại tất cả tám đỉnh của hình hộp và sau đó kiểm tra xem một điểm có ở bên trong hay không bằng cách kiểm tra nó trên từng mặt hoặc thực hiện phép kiểm tra điểm trong đa diện tổng quát hơn. Điều này đúng nhưng quá mức cần thiết. Ngay cả khi được thực hiện cẩn thận, nó vẫn đưa ra lý luận hình học không cần thiết về các mặt và pháp tuyến, trong khi cấu trúc của hộp đã đảm bảo sự căn chỉnh trục. 

Quan sát quan trọng là hộp được mô tả đầy đủ bằng các khoảng độc lập dọc theo mỗi trục. Hình chữ nhật phía dưới xác định một khoảng theo x và một khoảng theo y. Chiều cao xác định một khoảng tính bằng z. Khi các phạm vi này được trích xuất, mỗi truy vấn sẽ trở thành một thử nghiệm ngăn chặn đơn giản trong ba phân đoạn một chiều độc lập. 

Cách tiếp cận bạo lực tiêu tốn nhiều công sức để xây dựng lại hình học không cần thiết. Cách tiếp cận được tối ưu hóa nhận ra rằng vấn đề có thể được phân tách dọc theo các trục, giảm mọi thứ xuống còn ba lần kiểm tra theo khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hình học O(q) trên mỗi truy vấn, hiệu quả là O(q) với hằng số lớn | O(1) | Không cần thiết nhưng được chấp nhận | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Trích xuất các giới hạn ngang của hộp từ hai điểm chéo đã cho bằng cách lấy tọa độ x và tọa độ y tối thiểu và tối đa của chúng. Điều này đảm bảo tính chính xác bất kể thứ tự đầu vào. 
2. Tính các giới hạn dọc là khoảng bắt đầu từ mặt phẳng cơ sở z0 đến z0 + h. Điều này xác định chiều cao đầy đủ của hộp. 
3. Đối với mỗi điểm truy vấn, hãy so sánh tọa độ x của nó với khoảng x được tính toán trước. Nếu nó nằm ngoài thì điểm đó không thể ở trong hộp. 
4. Nếu tọa độ x hợp lệ, hãy so sánh tọa độ y với khoảng y. Nếu nó thất bại ở đây thì nó cũng ở bên ngoài. 
5. Nếu cả hai kiểm tra theo chiều ngang đều đạt, hãy so sánh tọa độ z với khoảng dọc. Nếu nó nằm trong phạm vi này thì điểm nằm bên trong hoặc trên ranh giới, nếu không thì nó nằm bên ngoài. 

Thứ tự kiểm tra không quan trọng đối với tính chính xác, nhưng việc kiểm tra theo chiều ngang trước tiên sẽ làm giảm nhẹ công việc trong thực tế vì nhiều điểm bị từ chối sớm. 

### Tại sao nó hoạt động

Hộp được căn chỉnh theo trục, do đó tư cách thành viên trong vùng 3D tương đương với tư cách thành viên đồng thời trong ba khoảng 1D độc lập. Không có sự kết hợp giữa các ràng buộc x, y và z. Bất kỳ điểm nào bên trong phải thỏa mãn cả ba ràng buộc và bất kỳ điểm nào không đáp ứng được dù chỉ một ràng buộc đều nằm ngoài tích Descartes của các khoảng đó, chính xác là hộp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

z0, h, u0, v0, u1, v1 = map(int, input().split())

x_min = min(u0, u1)
x_max = max(u0, u1)
y_min = min(v0, v1)
y_max = max(v0, v1)

z_min = z0
z_max = z0 + h

q = int(input())
out = []

for _ in range(q):
    x, y, z = map(int, input().split())

    if x_min <= x <= x_max and y_min <= y <= y_max and z_min <= z <= z_max:
        out.append("YES")
    else:
        out.append("NO")

print("\n".join(out))
```Việc thực hiện bắt đầu bằng cách chuẩn hóa tọa độ hình chữ nhật, vì hai điểm chéo có thể được sắp xếp theo bất kỳ thứ tự nào. Bước này ngăn việc xây dựng khoảng không chính xác khi u0 lớn hơn u1 hoặc v0 lớn hơn v1. 

Khoảng dọc được xây dựng trực tiếp từ chiều cao cơ sở và chiều cao hộp. Vì hộp kéo dài lên từ mặt phẳng cơ sở nên không có sự mơ hồ trong việc đặt hàng ở đây. 

Mỗi truy vấn được trả lời bằng cách kiểm tra khoảng thời gian ba chiều trực tiếp. Sự so sánh mang tính bao hàm vì các điểm biên được xem xét bên trong hộp. 

Một lỗi phổ biến khi triển khai kiểu này là quên rằng sự bình đẳng được cho phép trên tất cả các mặt, điều này sẽ loại bỏ không chính xác các điểm nằm chính xác trên các cạnh hoặc các mặt. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu: 

đầu vào: 

z0 = 1, h = 1, các góc dưới (u0,v0)=(-1,-1), (u1,v1)=(1,1) 

Vì vậy: 

x trong [-1, 1] 

y trong [-1, 1] 

z trong [1, 2] 

Xử lý truy vấn: 

| Truy vấn (x,y,z) | x kiểm tra | y kiểm tra | z kiểm tra | Kết quả | 
| --- | --- | --- | --- | --- | 
| (-1,-1,1) | bên trong | bên trong | bên trong | CÓ | 
| (0,0,2) | bên trong | bên trong | bên trong | CÓ | 
| (1,2,2) | bên trong | bên ngoài | bên trong | KHÔNG | 

Truy vấn đầu tiên nằm chính xác ở một góc của đế và ở mặt dưới, xác nhận rằng việc bao gồm ranh giới được xử lý chính xác. Truy vấn thứ hai theo chiều dọc ở trung tâm mặt trên, vẫn hợp lệ. Truy vấn thứ ba chỉ thất bại ở tọa độ y, cho thấy tính độc lập của các ràng buộc trục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được trả lời bằng các so sánh liên tục theo thời gian trên ba trục | 
| Không gian | O(1) | Chỉ một số biến biên cố định được lưu trữ | 

Các ràng buộc cho phép tối đa 1000 truy vấn, do đó, việc quét tuyến tính với kiểm tra thời gian liên tục dễ dàng nằm trong giới hạn. Giải pháp chỉ thực hiện so sánh số nguyên đơn giản cho mỗi truy vấn và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    z0, h, u0, v0, u1, v1 = map(int, input().split())

    x_min = min(u0, u1)
    x_max = max(u0, u1)
    y_min = min(v0, v1)
    y_max = max(v0, v1)

    z_min = z0
    z_max = z0 + h

    q = int(input())
    out = []

    for _ in range(q):
        x, y, z = map(int, input().split())
        if x_min <= x <= x_max and y_min <= y <= y_max and z_min <= z <= z_max:
            out.append("YES")
        else:
            out.append("NO")

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("""1 1 -1 -1 1 1
3
-1 -1 1
0 0 2
1 2 2
""") == "YES\nYES\nNO"

assert run("""0 0 5 5 5 5
2
5 5 0
5 5 1
""") == "YES\nNO"

assert run("""10 3 -2 -2 2 2
4
0 0 10
0 0 13
0 0 14
3 0 11
""") == "YES\nYES\nNO\nNO"

assert run("""-5 10 -1 -1 -1 -1
3
-1 -1 -5
-1 -1 5
-1 -1 6
""") == "YES\nYES\nNO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp mẫu | CÓ CÓ KHÔNG | tính đúng đắn cơ bản | 
| hình vuông suy biến | CÓ KHÔNG | hành vi cạnh h = 0 | 
| hộp bình thường | CÓ CÓ KHÔNG KHÔNG | từ chối nội thất và ranh giới | 
| dịch chuyển dọc | CÓ CÓ KHÔNG | xử lý z0 âm | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi hai điểm chéo được cho theo thứ tự đảo ngược. Ví dụ: nếu đầu vào chỉ định (u0, v0) = (5, 5) và (u1, v1) = (-3, -2), hình chữ nhật vẫn trải dài từ -3 đến 5 theo x và -2 đến 5 theo y. Thuật toán xử lý việc này bằng cách lấy mức tối thiểu và tối đa trước khi so sánh. Nếu không có bước này, việc kiểm tra định kỳ sẽ không thành công và các điểm hợp lệ sẽ bị từ chối. 

Một trường hợp quan trọng khác là khi chiều cao bằng 0. Trong trường hợp này, hộp suy biến thành một hình chữ nhật phẳng nằm hoàn toàn trên mặt phẳng z = z0. Một điểm như (x, y, z0) phải được chấp nhận nếu nó nằm trong hình chữ nhật đáy. Thuật toán xử lý điều này một cách tự nhiên vì khoảng dọc trở thành [z0, z0], do đó chỉ có kết quả khớp chính xác mới vượt qua. 

Trường hợp tinh tế cuối cùng là bao gồm ranh giới trên cả ba trục. Các điểm chính xác trên các mặt, các cạnh hoặc các đỉnh phải được xét bên trong. Việc sử dụng các so sánh toàn diện đảm bảo rằng không có điểm biên hợp lệ nào bị loại trừ, duy trì tính chính xác trên tất cả các cấu hình suy biến.
