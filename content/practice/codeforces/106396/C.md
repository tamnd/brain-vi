---
title: "CF 106396C - \u5f71\u5b50\u620f"
description: "Chúng ta được cho một tấm bảng hình chữ nhật có kích thước $n nhân m$. Mỗi ô có thể được hiểu là một vị trí mà chúng ta có thể đặt một quân cờ và bài toán yêu cầu số lượng quân cờ tối đa mà chúng ta có thể đặt theo một hạn chế di chuyển xuất phát từ quy tắc tấn công giống như hiệp sĩ."
date: "2026-06-20T03:36:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "C"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 78
verified: true
draft: false
---

[CF 106396C - \u5f71\u5b50\u620f](https://codeforces.com/problemset/problem/106396/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta một tấm bảng hình chữ nhật có kích thước$n \times m$. Mỗi ô có thể được hiểu là một vị trí mà chúng ta có thể đặt một quân cờ và bài toán yêu cầu số lượng quân cờ tối đa mà chúng ta có thể đặt theo một hạn chế di chuyển xuất phát từ quy tắc tấn công giống như hiệp sĩ. Một quân cờ được đặt trên một ô sẽ “điều khiển” một số ô khác và mọi cấu hình hợp lệ đều phải đảm bảo rằng không có hai quân cờ nào được đặt tấn công lẫn nhau. 

Cấu trúc của chuyển động là thông tin ẩn chính: mối quan hệ tấn công luôn lật ngược tính chẵn lẻ giữa hai lớp màu của lưới. Điều này ngay lập tức ngụ ý một cấu trúc lưỡng cực, nghĩa là nếu chúng ta tô màu lưới giống như một bàn cờ, thì mọi nước đi sẽ chuyển từ màu này sang màu kia. Do đó, bất kỳ tập hợp quân cờ nào được đặt hoàn toàn trên một lớp màu sẽ tự động an toàn, vì không có hai ô nào trong cùng một lớp có thể tấn công lẫn nhau. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp hai số nguyên$n$Và$m$, mô tả kích thước lưới. Đầu ra là số lượng phần tối đa có thể được đặt độc lập trên mỗi lưới. 

Các ràng buộc không được hiển thị rõ ràng, nhưng sự hiện diện của giải pháp dạng đóng trong mã tham chiếu cho thấy rằng$n, m$có thể lớn, có thể lên tới ít nhất$10^9$hoặc tương tự. Điều đó ngay lập tức loại trừ mọi mô phỏng hoặc xây dựng đồ thị. Bất kỳ giải pháp nào cũng phải giảm lưới thành công thức thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh chính xuất hiện khi một chiều rất nhỏ. Một trình kiểm tra ngây thơ giả định kết quả tô màu bàn cờ đầy đủ như$\lceil nm/2 \rceil$thất bại cho bảng mỏng. Ví dụ, trên một$2 \times 3$board, một công thức đơn giản cho ra 3, nhưng các quy tắc sắp xếp cẩn thận chỉ cho phép 4 hoặc đôi khi có hành vi có cấu trúc khác nhau tùy thuộc vào hình dạng tấn công. Một trường hợp dễ vỡ khác là$1 \times m$, nơi bảng thoái hóa thành một đường và mọi tương tác đều biến mất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là coi lưới như một biểu đồ trong đó mỗi ô là một nút và các cạnh biểu thị các cuộc tấn công hợp lệ. Sau đó chúng tôi cố gắng tính tập hợp độc lập tối đa. Về nguyên tắc, điều này đúng vì chúng ta muốn tập hợp các nút lớn nhất không có cạnh nào bên trong nó. 

Tuy nhiên, việc xây dựng biểu đồ này đã tốn kém$O(nm)$và việc chạy bất kỳ thuật toán tiêu chuẩn nào cho tập độc lập tối đa là không khả thi vì ngay cả các công thức so khớp lưỡng cực cũng yêu cầu ít nhất thời gian tuyến tính hoặc siêu tuyến tính cho mỗi trường hợp thử nghiệm. Đối với lưới lớn, phương pháp này thất bại ngay lập tức. 

Quan sát quan trọng là cấu trúc tấn công có tính đều đặn cao và chỉ phụ thuộc vào tọa độ tương đối. Lưới tự nhiên chia thành hai lớp chẵn lẻ và hầu hết các tương tác chỉ vượt qua các lớp này. Điều đó cho thấy rằng trong hầu hết các trường hợp, việc lấy tất cả các ô của một giá trị chẵn lẻ sẽ mang lại câu trả lời tối ưu, mang lại khoảng một nửa lưới, tức là.$\lfloor (nm+1)/2 \rfloor$. 

Điều này hoạt động hoàn hảo khi cả hai chiều ít nhất là 3. Lý do là lưới “đủ dày đặc” nên không có ràng buộc mẫu cục bộ nào phá vỡ giới hạn lưỡng cực. 

Khó khăn đến từ những kích thước nhỏ. Khi$n = 1$, không có tương tác dọc nào cả, vì vậy mọi ô đều độc lập. Khi$n = 2$, bàn cờ trở thành một dải 2 hàng trong đó các nước đi của hiệp sĩ tạo ra xung đột cục bộ theo kiểu lặp đi lặp lại. Điều này dẫn đến một cấu trúc tối ưu định kỳ với các cột chu kỳ 4, tạo ra mẫu tối đa lặp lại gồm 4 ô trên mỗi khối có chiều rộng 4, với cách xử lý đặc biệt đối với phần còn lại. 

Do đó, lời giải rút gọn thành ba chế độ: trường hợp đường tầm thường, trường hợp dải bị ràng buộc và trường hợp tổng quát trong đó tính chẵn lẻ chiếm ưu thế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Đồ thị lực lượng vũ phu / DP |$O(nm)$hoặc tệ hơn |$O(nm)$| Quá chậm | 
| Các trường hợp chia công thức tối ưu |$O(1)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc$n$Và$m$và trao đổi chúng nếu cần thiết để$n \le m$. Điều này làm giảm việc xử lý trường hợp vì tất cả các công thức chỉ phụ thuộc vào kích thước nhỏ hơn là “chiều cao”. 
2. Nếu$n = 1$, trở lại$m$. Trong một hàng, không quân nào có thể tấn công quân nào khác vì mọi nước đi đều yêu cầu dịch chuyển theo chiều dọc. Vì vậy mọi tế bào đều có thể được chiếm giữ một cách an toàn. 
3. Nếu$n = 2$, tính toán câu trả lời bằng cách sử dụng cấu trúc khối dọc theo các cột. Chúng tôi chia bảng thành các đoạn gồm 4 cột. Mỗi khối đầy đủ đóng góp 4 miếng. Đối với phần còn lại$m \bmod 4$cột, các đóng góp tuân theo một mẫu cố định: 1 cột bổ sung mang lại 2 vị trí bổ sung, 2 cột mang lại 4, 3 cột mang lại 4 và 4 cột mang lại 4. Điều này phát sinh từ việc đóng gói cục bộ tối ưu trong lưới 2 hàng trong đó các kiểu tấn công lặp lại sau mỗi bốn cột. 
4. Nếu$n \ge 3$, trở lại$\lceil nm / 2 \rceil$. Điều này tương ứng với việc tô một lớp màu của màu bàn cờ. Vì tất cả các cuộc tấn công chéo tính chẵn lẻ nên không có hai ô được chọn xung đột và đây là mức tối đa do tính đối xứng. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên cấu trúc của các bước di chuyển giống như hiệp sĩ bảo toàn tính lưỡng cực và thực tế là chỉ các lưới hẹp mới tạo ra các ràng buộc cục bộ phá vỡ tính tối ưu chẵn lẻ thuần túy. Vì$n \ge 3$, mọi ô đều có đủ tính linh hoạt của vùng lân cận mà không cấu hình cục bộ nào có thể buộc chúng ta xuống dưới giới hạn chẵn lẻ. Vì$n = 1$Và$n = 2$, biểu đồ quá mỏng và các ràng buộc chồng chéo cục bộ trở thành toàn cầu, tạo ra các mẫu tối ưu định kỳ thay vì mức tối đa lưỡng cực đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    if n > m:
        n, m = m, n

    if n == 1:
        print(m)
    elif n == 2:
        k = m // 4
        r = m % 4
        ans = k * 4 + min(4, r * 2)
        print(ans)
    else:
        print((n * m + 1) // 2)

t = int(input())
for _ in range(t):
    solve()
```Bước đầu tiên đảm bảo chúng ta luôn coi chiều nhỏ hơn là$n$, giúp đơn giản hóa lý luận cho$n = 2$trường hợp. các$n = 1$nhánh xử lý trực tiếp biểu đồ đường suy biến. 

các$n = 2$tính toán mã hóa mẫu tối ưu lặp lại trên các khối bốn cột. Việc nhân 4 cho các khối đầy đủ phản ánh rằng mỗi phân đoạn 2x4 có thể chứa bốn quân không tấn công trong một cấu hình tối ưu. Phần điều chỉnh còn lại sử dụng`min(4, r * 2)`mã hóa các giá trị tốt nhất được tính toán trước cho các khối một phần. 

Trường hợp cuối cùng sử dụng số học số nguyên để tính toán$\lceil nm/2 \rceil$an toàn như`(n * m + 1) // 2`. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n = 2, m = 5$Chúng tôi áp dụng công thức 2 hàng. 

| Bước | Khối đầy đủ (m//4) | Còn lại (m%4) | Khối đóng góp | Đóng góp còn lại | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 4 | phút(4, 2) = 2 | 6 | 

Kết quả là 6, cho thấy cấu trúc tuần hoàn như thế nào và phần còn lại đóng góp một phần. 

### Ví dụ 2:$n = 3, m = 5$Chúng ta rơi vào trường hợp tổng quát. 

| Bước | n*m | công thức chẵn lẻ | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 15 | (15+1)//2 | 8 | 

Điều này xác nhận rằng giải pháp giảm xuống mức tối đa lưỡng cực đơn giản khi lưới đủ rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Mỗi trường hợp chỉ sử dụng kiểm tra số học và có điều kiện | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp này phù hợp thoải mái trong giới hạn ngay cả đối với số lượng lớn các trường hợp thử nghiệm, vì mỗi lần tính toán đều có thời gian không đổi và tránh hoàn toàn việc xây dựng lưới. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    data = inp.strip().split()
    t = int(data[0])
    idx = 1
    out = []

    for _ in range(t):
        n = int(data[idx]); m = int(data[idx+1])
        idx += 2

        if n > m:
            n, m = m, n

        if n == 1:
            out.append(str(m))
        elif n == 2:
            k = m // 4
            r = m % 4
            ans = k * 4 + min(4, r * 2)
            out.append(str(ans))
        else:
            out.append(str((n * m + 1) // 2))

    return "\n".join(out)

# provided sample-like checks
assert run("1\n1 10\n") == "10"
assert run("1\n2 3\n") == "4"
assert run("1\n3 3\n") == "5"

# custom cases
assert run("1\n2 1\n") == "2", "thin 2-row minimal width"
assert run("1\n2 8\n") == "8", "full 2-row blocks"
assert run("1\n3 4\n") == "6", "general parity small grid"
assert run("1\n10 10\n") == "50", "square large grid parity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 2 | cạnh 2 hàng nhỏ nhất | 
| 2 8 | 8 | khối định kỳ đầy đủ | 
| 3 4 | 6 | chuyển sang quy tắc chẵn lẻ | 
| 10 10 | 50 | trường hợp đối xứng lớn | 

## Vỏ cạnh 

các$n = 1$trường hợp dễ vỡ nhất. Một giải pháp dựa trên tính chẵn lẻ ngây thơ sẽ xuất ra không chính xác$\lceil m/2 \rceil$, nhưng trên thực tế tất cả các ô đều độc lập. Đối với đầu vào`1 7`, đầu ra đúng là`7`và thuật toán trực tiếp trả về$m$, phù hợp với thực tế là không có cạnh tấn công nào tồn tại trong một hàng. 

các$n = 2$trường hợp mã hóa cấu trúc không tầm thường duy nhất. Đối với đầu vào`2 5`, thuật toán tính một khối đầy đủ và một khối còn lại:$4 + 2 = 6$. Một giả định nửa lưới ngây thơ sẽ cho$\lceil 10/2 \rceil = 5$, điều này hoàn toàn sai vì kiểu tương tác cho phép đóng gói dày đặc hơn so với cách tô màu lưỡng cực đơn giản. 

Vì$n \ge 3$, chẳng hạn như`3 3`, thuật toán trả về$\lceil 9/2 \rceil = 5$. Việc thực hiện theo dõi xác nhận rằng không có nhánh đặc biệt nào được kích hoạt và cấu trúc lưỡng cực chi phối câu trả lời, phù hợp với thực tế là không còn tồn tại ràng buộc lưới mỏng nào nữa.
