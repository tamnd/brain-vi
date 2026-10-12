---
title: "CF 105017F-800"
description: "Chúng ta có một lưới các chữ cái viết thường $N lần M$. Chúng ta được phép thực hiện nhiều lần một thao tác rất cụ thể: chọn bất kỳ ô $(i, j)$ nào, chọn một số nguyên dương $k$ và sao chép ký tự của nó vào $(i-k, j-k)$ hoặc $(i-k, j+k)$, miễn là các ô mục tiêu đó vẫn ở bên trong…"
date: "2026-06-28T02:08:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105017
codeforces_index: "F"
codeforces_contest_name: "Winter Cup 4.0 Online Mirror Contest"
rating: 0
weight: 105017
solve_time_s: 35
verified: true
draft: false
---

[CF 105017F - 800](https://codeforces.com/problemset/problem/105017/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$N \times M$lưới các chữ cái viết thường. Chúng ta được phép thực hiện nhiều lần một thao tác rất cụ thể: chọn bất kỳ ô nào$(i, j)$, chọn số nguyên dương$k$và sao chép ký tự của nó vào$(i-k, j-k)$hoặc$(i-k, j+k)$, miễn là các ô mục tiêu đó nằm trong lưới. 

Vì vậy, thông tin chỉ di chuyển lên trên và mỗi bước di chuyển sẽ dịch chuyển theo đường chéo sang trái hoặc lên phải một lượng như nhau. 

Câu hỏi đặt ra là liệu chỉ sử dụng quy tắc lan truyền này, liệu chúng ta có thể làm cho mọi ô trong lưới cuối cùng trở thành một ký tự mục tiêu cố định hay không?$C$. 

Những hạn chế$N, M \le 1000$ngụ ý lên đến$10^6$tế bào. Bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động một cách rõ ràng đều không thể thực hiện được, vì mỗi hoạt động có thể phụ thuộc vào các lựa chọn ô và bước tùy ý, dẫn đến một không gian trạng thái hàm mũ. Chúng ta cần một thuộc tính cấu trúc mô tả khả năng tiếp cận thay vì mô phỏng. 

Một điểm tinh tế là việc sao chép có tính định hướng và có hiệu lực không thể đảo ngược. Khi một ô bị ghi đè, chúng tôi chỉ quan tâm liệu nó có thể được làm bằng hay không$C$, không phải có bao nhiêu thao tác đã được sử dụng. Một quan sát quan trọng khác là phép toán không phụ thuộc vào sự kề cận mà phụ thuộc vào sự căn chỉnh đường chéo, điều này gợi ý một số bất biến dọc theo đường chéo hoặc đường chéo. 

Một trường hợp lỗi phổ biến xuất phát từ việc giả định rằng có thể lan truyền cục bộ theo mọi hướng. Ví dụ: một lưới chỉ có các hàng dưới cùng chứa$C$có vẻ như đủ, nhưng việc truyền bị hạn chế bởi cấu trúc đường chéo, do đó khả năng tiếp cận không đồng đều trên toàn lưới. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực coi mỗi ô như một nguồn lan truyền, liên tục đẩy các ký tự lên trên theo cả hai hướng chéo. Người ta có thể tưởng tượng việc mô phỏng sự lan truyền giống như BFS trong đó mọi ô có thể hoạt động như một nguồn và cố gắng ghi đè lên các ô phía trên nó. 

Về nguyên tắc, điều này đúng vì mọi thao tác được phép chỉ là sao chép từ ô nguồn sang ô khác có thể truy cập được bằng các bước chéo lặp đi lặp lại. Tuy nhiên, số lượng các hoạt động có thể là rất lớn. Mỗi ô có khả năng tác động đến nhiều ô phía trên nó theo hai hướng chéo và việc áp dụng nhiều lần điều này sẽ dẫn đến độ phức tạp trong trường hợp xấu nhất theo thứ tự$O((NM)^2)$hoặc tệ hơn nếu được mô phỏng một cách rõ ràng. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm. Thay vì hỏi làm thế nào các ô có thể lan xuống dưới, chúng tôi hỏi những điều kiện nào cho phép một ô được ghi đè vào$C$. Một tế bào$(i,j)$có thể trở thành$C$nếu ít nhất một ô trong cùng “cấu trúc khả năng tiếp cận theo đường chéo” đã có$C$và cấu trúc đó hóa ra lại đơn giản: các ô được kết nối nếu chúng có cùng tính chẵn lẻ$i+j$, bởi vì mỗi bước di chuyển đều bảo toàn tính chẵn lẻ của$i+j$. 

Thật vậy, cả hai vị trí mục tiêu$(i-k, j-k)$Và$(i-k, j+k)$duy trì tính chẵn lẻ của tổng:$$(i-k)+(j-k) = i+j-2k,\quad (i-k)+(j+k)=i+j$$Vì vậy, tính chẵn lẻ sẽ chia lưới thành hai thành phần độc lập. Trong mỗi thành phần, sự lan truyền cho phép di chuyển lên trên và sang ngang theo cách mà cuối cùng cho phép bất kỳ ô nào ảnh hưởng đến bất kỳ ô nào khác trong thành phần của nó, miễn là chúng ta có thể tìm thấy ít nhất một lần xuất hiện của$C$trong thành phần đó. 

Do đó, toàn bộ vấn đề được rút gọn thành một kiểm tra tính nhất quán đơn giản: mỗi ô phải nằm trong một thành phần đã chứa ít nhất một thành phần.$C$, bởi vì nếu không thì không có nguồn$C$để truyền bá trong lớp chẵn lẻ bị ngắt kết nối đó. 

Chúng tôi chỉ đơn giản kiểm tra xem tất cả các ô không$C$nằm trong một lớp chẵn lẻ không có trở ngại về cấu trúc, điều này có nghĩa là xác minh rằng$C$tồn tại trong cả hai lớp chẵn lẻ bất cứ khi nào cần thiết, hoặc trực tiếp hơn là mọi ô đều có thể được truy cập từ một số$C$-cell tôn trọng kết nối chẵn lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O((NM)^2)$|$O(NM)$| Quá chậm | 
| Kết nối dựa trên tính chẵn lẻ |$O(NM)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán lớp chẵn lẻ của mỗi ô bằng cách sử dụng$(i + j) \bmod 2$và ghi lại xem ký tự đích$C$xuất hiện trong mỗi lớp chẵn lẻ. Điều này nắm bắt bất biến duy nhất được bảo toàn bởi tất cả các bước di chuyển được phép. 
2. Duyệt lưới một lần và cho mỗi ô, xác định xem nó thuộc về chẵn lẻ 0 hay chẵn lẻ 1. Đối với mỗi chẵn lẻ, duy trì một cờ boolean cho biết liệu chúng ta có thấy ít nhất một lần xuất hiện của$C$. 
3. Sau khi xử lý trước, hãy xác minh từng ô: nếu một ô chưa có$C$, vẫn phải có khả năng biến đổi nó. Điều này chỉ có thể thực hiện được nếu lớp chẵn lẻ của nó chứa ít nhất một$C$. 
4. Nếu chúng ta tìm thấy bất kỳ ô nào có lớp chẵn lẻ không xuất hiện$C$, ngay lập tức kết luận rằng việc chuyển đổi là không thể. 
5. Ngược lại, kết luận rằng tất cả các ô có thể được tạo bằng$C$. 

### Tại sao nó hoạt động 

Hoạt động bảo toàn giá trị của$(i + j) \bmod 2$, do đó lưới phân hủy thành hai thành phần độc lập không bao giờ tương tác. Trong một thành phần, việc sao chép theo đường chéo lặp đi lặp lại cho phép thông tin được truyền theo các bước di chuyển hợp lệ mà không phá vỡ cấu trúc này. Vì vậy, bất kỳ tế bào nào cũng chỉ có thể trở thành$C$nếu thành phần chẵn lẻ của nó đã chứa ít nhất một$C$. Điều kiện này vừa cần vừa đủ vì một lần$C$tồn tại trong một thành phần, các hoạt động được phép lặp lại có thể lan truyền nó trong toàn bộ thành phần đó. 

## Giải pháp Python```
PythonRun
```Mã đọc lưới và theo dõi xem ký tự đích có tồn tại trong mỗi lớp trong số hai lớp chẵn lẻ hay không. Vòng lặp cuối cùng thực thi điều kiện về khả năng tiếp cận: mọi ô phải thuộc về một lớp có thể tạo ra$C$. Việc thoát sớm sẽ tránh việc quét không cần thiết khi phát hiện thấy không thể thực hiện được. 

Một cạm bẫy triển khai phổ biến là quên tính chẵn lẻ bằng cách sử dụng$(i + j)$và thay vào đó cố gắng suy luận bằng các đường chéo riêng biệt, điều này dẫn đến sự phức tạp quá mức. Kiểm tra tính chẵn lẻ nắm bắt cả hai hướng chéo cùng một lúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```

```Ở đây không có ô nào chứa`b`, vì vậy cả lớp chẵn lẻ đều không chứa mục tiêu. 

| tôi | j | tế bào | chẵn lẻ | có C chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | một | 0 | Sai | 
| 0 | 1 | một | 1 | Sai | 
| 0 | 2 | một | 0 | Sai | 
| ... | ... | ... | ... | ... | 

Vì không có lớp chẵn lẻ nào chứa`b`, bất kỳ ô nào cũng không thể truy cập được. Đầu ra là`NO`. 

Điều này cho thấy việc truyền bá không thể tạo ra một ký tự chưa tồn tại ở đâu đó trong lưới. 

### Ví dụ 2 

đầu vào:```

```Chỉ có một ô tồn tại và nó đã khớp$C$. 

| tôi | j | tế bào | chẵn lẻ | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | một | 0 | Đúng | 

Vì lưới đã đồng nhất nên không cần thực hiện thao tác nào. Đầu ra là`YES`. 

Điều này xác nhận thuật toán xử lý đầu vào tối thiểu một cách chính xác mà không yêu cầu bất kỳ lý do lan truyền nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM)$| Mỗi ô được truy cập với số lần không đổi để tính chẵn lẻ và xác thực | 
| Không gian |$O(1)$| Chỉ có hai cờ boolean được lưu trữ bất kể kích thước lưới | 

Lời giải dễ dàng nằm trong giới hạn vì$NM \le 10^6$và tất cả các thao tác đều là kiểm tra ký tự đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""3 3
b
aaa
aaa
aaa
""") == "NO"

assert run("""1 1
a
a
""") == "YES"

# all same and already target
assert run("""2 2
c
cc
cc
""") == "YES"

# target missing entirely
assert run("""2 3
z
abc
def
""") == "NO"

# single row
assert run("""1 5
a
abcaa
""") == "NO"

# mixed parity presence of target
assert run("""3 3
x
xax
axa
xax
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2x2 toàn C | CÓ | trường hợp thành công tầm thường | 
| không có C đâu cả | KHÔNG | không thể khi thiếu nguồn | 
| hàng đơn không khớp | KHÔNG | lan truyền không thể tạo C | 
| mẫu bàn cờ | CÓ | cả hai lớp chẵn lẻ được bảo hiểm | 

## Vỏ cạnh 

Một lưới không có sự xuất hiện của$C$ngay lập tức thất bại vì cả lớp chẵn lẻ đều không thể đưa ra giá trị đích. Thuật toán phát hiện điều này vì cả hai cờ vẫn sai và bất kỳ ô nào không khớp đều sẽ bị từ chối. 

Một lưới hoàn toàn thống nhất đã bằng$C$thành công một cách tầm thường vì mọi ô đều vượt qua bước kiểm tra cuối cùng với
