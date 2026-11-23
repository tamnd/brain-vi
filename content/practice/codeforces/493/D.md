---
title: "CF 493D - Vasya và cờ vua"
description: "Chúng ta có một bảng $n nhân n$. Hậu trắng bắt đầu ở góc trên bên trái $(1,1)$ và quân hậu đen bắt đầu ở góc trên bên phải $(1,n)$. Mỗi ô vuông khác đều chứa một con tốt màu xanh lá cây. Việc di chuyển là bắt buộc. Ở mỗi lượt, người chơi phải bắt được một số quân với quân hậu của mình."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "games", "math"]
categories: ["algorithms"]
codeforces_contest: 493
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 281 (Div. 2)"
rating: 1700
weight: 493
solve_time_s: 85
verified: true
draft: false
---

[CF 493D - Vasya và Cờ vua](https://codeforces.com/problemset/problem/493/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, trò chơi, toán học 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một$n \times n$Cái bảng. Hậu trắng bắt đầu ở góc trên bên trái$(1,1)$, và quân hậu đen bắt đầu ở góc trên bên phải$(1,n)$. Mỗi ô vuông khác đều chứa một con tốt màu xanh lá cây. 

Việc di chuyển là bắt buộc. Ở mỗi lượt, người chơi phải bắt được một số quân với quân hậu của mình. Quân bị bắt có thể là quân tốt màu xanh lá cây hoặc quân hậu đối phương. Quân hậu di chuyển chính xác như trong cờ vua, dọc theo hàng, cột hoặc đường chéo và không thể nhảy qua quân cờ. 

Một người chơi thua trong hai trường hợp. Đầu tiên là khi họ bắt đầu lượt của mình mà không có quyền bắt giữ hợp pháp. Thứ hai là khi quân hậu của họ bị bắt ở nước đi trước đó của đối thủ. 

Đầu vào chỉ chứa kích thước bảng$n$. Chúng ta phải xác định người chơi nào thắng khi chơi hoàn hảo. Nếu Trắng thắng thì chúng ta cũng phải thực hiện nước đi đầu tiên của Trắng. Trong số tất cả các nước đi đầu tiên giành chiến thắng, chúng ta cần nước có hàng nhỏ nhất và sau đó là cột nhỏ nhất. 

Ràng buộc là phần rõ ràng nhất của vấn đề. Kích thước bảng có thể lớn bằng$10^9$. Việc xây dựng bảng là không thể. Ngay cả việc lưu trữ một bit trên mỗi ô vuông cũng sẽ cần quá nhiều bộ nhớ. Bất kỳ giải pháp nào liên quan đến từng ô riêng lẻ hoặc mô phỏng từng bước di chuyển của trò chơi đều bị loại trừ ngay lập tức. 

Điều này cho thấy trò chơi phải có cấu trúc toán học rất đơn giản. Toàn bộ giải pháp cần phải được bắt nguồn từ một mẫu chỉ phụ thuộc vào$n$. 

Trường hợp cạnh không rõ ràng đầu tiên là$n=2$. 

đầu vào:```
2
```Các quân hậu tấn công nhau trực tiếp dọc theo hàng đầu tiên. Trắng có thể bắt được hậu đen ngay lập tức. 

Đầu ra:```
white
1 2
```Một giải pháp bất cẩn cho rằng cả hai quân hậu phải tiếp tục bắt những con tốt sẽ bỏ lỡ chiến thắng ngay lập tức này. 

Một trường hợp quan trọng khác là$n=3$. 

đầu vào:```
3
```Đầu ra:```
black
```Thoạt nhìn Trắng có vẻ có nhiều động thái hợp pháp. Trên thực tế, mọi nước đi an toàn đều bị ép buộc. Cả hai quân hậu đều di chuyển xuống dọc theo các cột tương ứng của mình cho đến khi cuối cùng Trắng phải bước vào cột giữa và bị bắt. Một phân tích ngây thơ chỉ dựa trên số lần di chuyển có thể dễ dàng dự đoán sai người chiến thắng. 

Trường hợp quan trọng cuối cùng là bất kỳ bảng chẵn nào có kích thước lớn hơn hai. 

đầu vào:```
4
```Đầu ra:```
white
2 2
```Nước đi chiến thắng không phải là nước đi góc. Hình vuông chính xác rất quan trọng vì tuyên bố yêu cầu nước đi đầu tiên giành chiến thắng nhỏ nhất về mặt từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa trạng thái trò chơi một cách rõ ràng. Chúng ta có thể đại diện cho vị trí của cả quân hậu và tất cả những con tốt còn lại, tạo ra mọi nước đi hợp pháp và tính toán người chiến thắng với điểm tối thiểu. 

Đối với một bảng có kích thước$n$, có$n^2$hình vuông và hầu như tất cả ban đầu đều chứa các mảnh. Số lượng các trạng thái tăng theo cấp số nhân với số lần bắt giữ. Ngay cả đối với những bảng rất nhỏ, điều này trở nên không khả thi. Với$n$lên tới$10^9$, việc xây dựng vị trí ban đầu một mình là không thể. 

Quan sát quan trọng là bảng được lấp đầy hoàn toàn bằng các quân cờ. Bởi vì quân hậu không thể nhảy qua các quân cờ, nên ban đầu mỗi quân hậu chỉ có thể chiếm được các quân liền kề dọc theo một trong các hướng tấn công của nó. 

Chúng ta hãy xem xét cấu trúc trò chơi thay vì các vị trí riêng lẻ. 

Đối với số lẻ$n$, có một cột trung tâm. Bất kỳ quân hậu nào bước vào đó đều trở nên dễ bị quân hậu đối phương tấn công. Lối chơi tối ưu buộc cả hai quân hậu phải ở trong cột bên mình càng lâu càng tốt. Họ dần dần di chuyển xuống phía dưới. Cuối cùng Trắng là người chơi đầu tiên buộc phải vào cột giữa, sau đó Đen bắt được hậu trắng và giành chiến thắng. 

Thậm chí$n$, không có cột trung tâm. Trắng có thể ngay lập tức di chuyển đến hình vuông$(2,2)$. Sau nước đi này, thế cờ trở nên cân xứng nhưng Đen phải đáp trả trước. Trắng chuyển nước đi cho Đen một cách hiệu quả trong thế thua. Đây là chiến lược chiến thắng tiêu chuẩn cho các quy mô bảng chẵn. 

Hệ quả đáng chú ý là người chiến thắng chỉ phụ thuộc vào tính chẵn lẻ của$n$. 

Nếu như$n$thật kỳ lạ, Đen thắng. 

Nếu như$n$chẵn, Trắng thắng và nước đi bắt buộc là$(2,2)$. Vì$n=2$, đây cũng là$(1,2)$, vì Trắng có thể bắt được hậu đen ngay lập tức và nước đi đó có hàng nhỏ nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của$n$. 
2. Nếu$n$thật kỳ lạ, hãy in`"black"`. 

Đối với kích thước bàn cờ lẻ, cách chơi tối ưu sẽ dẫn đến chiến thắng bắt buộc cho người chơi thứ hai. 
3. Nếu không, hãy in`"white"`. 

Ngay cả kích thước bàn cờ cũng là vị trí chiến thắng cho người chơi đầu tiên. 
4. Nếu$n=2$, in nước đi`(1, 2)`. 

Trắng có thể bắt được hậu đen ngay lập tức. 
5. Ngược lại, in nước đi`(2, 2)`. 

Đây là nước đi thắng đầu tiên nhỏ nhất về mặt từ điển đối với mỗi nước chẵn.$n \ge 4$. 

### Tại sao nó hoạt động 

Trò chơi giảm hoàn toàn về mức ngang bằng. 

Đối với kích thước bàn cờ lẻ, bàn cờ đầy sẽ buộc cả hai quân hậu vào một chuỗi bắt đối xứng. Trắng là người chơi đầu tiên phải rời khỏi cột bên an toàn và tiến vào ô bị Đen tấn công. Vì Trắng đi trước nên Trắng đạt đến thời điểm thua này trước. 

Đối với kích thước bàn chẵn, Trắng có thể di chuyển đến$(2,2)$, tạo ra cấu trúc thua có kích thước lẻ cho Đen trong khi vẫn giữ được tính đối xứng. Đen trở thành người chơi cuối cùng phải đối mặt với nước đi buộc phải thua. Vì vị trí kết quả là người chơi thua khi di chuyển nên Trắng thắng. 

Bởi vì câu lệnh yêu cầu nước đi thắng nhỏ nhất,$(2,2)$được chọn cho tất cả các bảng chẵn lớn hơn hai, trong khi$(1,2)$là việc nắm bắt chiến thắng ngay lập tức khi$n=2$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

if n % 2 == 1:
    print("black")
else:
    print("white")
    if n == 2:
        print("1 2")
    else:
        print("2 2")
```Quyết định đầu tiên kiểm tra tính chẵn lẻ của quy mô bảng. Toàn bộ phân tích lý thuyết trò chơi đều tập trung vào điều kiện duy nhất này. 

Khi$n$Thật kỳ lạ, câu trả lời chỉ bao gồm một dòng vì Đen thắng và không có nước đi thắng nào cho Trắng. 

Khi$n$chẵn thì chúng ta cũng phải có nước đi đầu tiên thắng lợi. Trường hợp đặc biệt$n=2$được xử lý riêng vì Trắng có thể trực tiếp bắt được hậu đen. Đối với mọi bảng chẵn lớn hơn,$(2,2)$là bước đi cần thiết. 

Không cần vòng lặp, đệ quy hoặc cấu trúc dữ liệu lớn. Giải pháp hoạt động hoàn toàn với giá trị của$n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
```| Bước | n | Chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 2 | Thậm chí | Trắng thắng | 
| Trường hợp đặc biệt | 2 | Thậm chí | Di chuyển = (1,2) | 

Đầu ra:```
white
1 2
```Điều này thể hiện bảng nhỏ nhất. Các quân hậu tấn công trực tiếp lẫn nhau nên Trắng thắng ngay khi bắt được hậu đen. 

### Ví dụ 2 

đầu vào:```
3
```| Bước | n | Chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 3 | lẻ | Đen thắng | 
| Đầu ra | 3 | lẻ | "đen" | 

Đầu ra:```
black
```Điều này chứng tỏ trường hợp có kích thước lẻ. Trò chơi cuối cùng buộc Trắng vào cột trung tâm trước, giúp Đen giành chiến thắng. 

### Ví dụ 3 

đầu vào:```
4
```| Bước | n | Chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| Đọc đầu vào | 4 | Thậm chí | Trắng thắng | 
| Nước đi chiến thắng | 4 | Thậm chí | (2,2) | 

Đầu ra:```
white
2 2
```Điều này thể hiện mô hình có kích thước chẵn chung. Trắng sử dụng nước đi chiến thắng kinh điển$(2,2)$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ kiểm tra tính chẵn lẻ và đầu ra theo thời gian không đổi | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài một vài biến | 

Kích thước bảng có thể đạt$10^9$, nhưng thuật toán không bao giờ xây dựng bảng hoặc mô phỏng trò chơi. Thời gian chạy và mức sử dụng bộ nhớ của nó là không đổi, vì vậy nó dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    n = int(input())

    if n % 2 == 1:
        print("black")
    else:
        print("white")
        if n == 2:
            print("1 2")
        else:
            print("2 2")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n") == "white\n1 2\n", "sample 1"

# custom cases
assert run("3\n") == "black\n", "small odd board"
assert run("4\n") == "white\n2 2\n", "small even board"
assert run("1000000000\n") == "white\n2 2\n", "maximum even value"
assert run("999999999\n") == "black\n", "large odd value"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`|`white / 1 2`| Bắt nữ hoàng ngay lập tức | 
|`3`|`black`| Bảng lẻ nhỏ nhất | 
|`4`|`white / 2 2`| Quy tắc chung về bảng chẵn | 
|`1000000000`|`white / 2 2`| Đầu vào chẵn có kích thước tối đa | 
|`999999999`|`black`| Đầu vào lẻ ​​lớn | 

## Vỏ cạnh 

Hãy xem xét bảng nhỏ nhất có thể. 

đầu vào:```
2
```Thuật toán phát hiện bảng chẵn và nhập trường hợp đặc biệt. Nó xuất ra:```
white
1 2
```Điều này đúng vì quân hậu đen chiếm giữ$(1,2)$, và Trắng bắt được nó ngay lập tức. Một giải pháp luôn mang lại kết quả$(2,2)$vì ngay cả các bảng cũng sẽ thất bại ở đây vì hình vuông đó không tồn tại. 

Hãy xem xét bảng lẻ nhỏ nhất. 

đầu vào:```
3
```Thuật toán thấy rằng$n$là số lẻ và kết quả đầu ra:```
black
```Đối số bắt buộc chơi đối với các bảng lẻ được áp dụng ngay lập tức. Cuối cùng, quân trắng trở thành người chơi đầu tiên bị ép vào cột giữa và thua cuộc. 

Hãy xem xét một bảng lẻ rất lớn. 

đầu vào:```
999999999
```Thuật toán lại chỉ sử dụng tính chẵn lẻ và kết quả đầu ra:```
black
```Không cần mô phỏng. Đối số cấu trúc tương tự chứng minh kết quả cho các bảng lẻ nhỏ vẫn có giá trị bất kể kích thước bảng. 

Hãy xem xét bảng chẵn lớn nhất được phép. 

đầu vào:```
1000000000
```Các đầu ra thuật toán:```
white
2 2
```việc di chuyển$(2,2)$tồn tại bởi vì$n \ge 4$và đối số chẵn lẻ đảm bảo đây là nước đi mở đầu thắng lợi. Kích thước bảng không ảnh hưởng đến việc tính toán.
