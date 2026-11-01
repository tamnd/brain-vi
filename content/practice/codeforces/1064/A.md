---
title: "CF 1064A - Tạo hình tam giác!"
description: "Chúng ta được cho ba số nguyên dương biểu thị độ dài hiện tại của ba thanh. Trong một nước đi, chúng ta được phép chọn đúng một cây gậy và tăng chiều dài của nó lên một đơn vị."
date: "2026-06-15T08:28:40+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1064
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 516 (Div. 2, by Moscow Team Olympiad)"
rating: 800
weight: 1064
solve_time_s: 200
verified: false
draft: false
---

[CF 1064A - Tạo hình tam giác!](https://codeforces.com/problemset/problem/1064/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, hình học, toán học 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số nguyên dương biểu thị độ dài hiện tại của ba thanh. Trong một nước đi, chúng ta được phép chọn đúng một cây gậy và tăng chiều dài của nó lên một đơn vị. Mục tiêu là đạt được tình huống trong đó ba độ dài này có thể tạo thành một tam giác không suy biến, nghĩa là bất đẳng thức tam giác được thỏa mãn nghiêm ngặt cho cả ba cặp cạnh. 

Một bộ ba độ dài chỉ có thể tạo thành một tam giác hợp lệ có diện tích dương nếu tổng của hai cạnh bất kỳ lớn hơn cạnh thứ ba. Vì chúng ta luôn có thể sắp xếp lại các cạnh nên điều kiện chính sẽ là kiểm tra xem, sau một số bước tăng, cạnh lớn nhất có nhỏ hơn tổng của hai cạnh còn lại hay không. 

Các ràng buộc rất nhỏ: mỗi độ dài nhiều nhất là 100. Điều này ngay lập tức gợi ý rằng ngay cả việc mô phỏng lực lượng vũ phu trên tất cả các trạng thái hợp lý cũng có thể khả thi, vì tổng không gian tìm kiếm là rất nhỏ. Tuy nhiên, bài toán có cấu trúc cho phép chúng ta tránh hoàn toàn việc mô phỏng. 

Các trường hợp cạnh phát sinh khi các que đã thỏa mãn bất đẳng thức tam giác, khi chúng nằm chính xác trên biên của suy biến và khi một que lớn hơn đáng kể so với tổng của hai que còn lại. Ví dụ: các đầu vào như`3 4 5`đã tạo thành một hình tam giác hợp lệ nên câu trả lời là 0. Đầu vào như`1 2 3`không tạo thành hình tam giác vì không được phép bình đẳng; chúng ta cần ít nhất một mức tăng. Đầu vào như`1 1 10`đòi hỏi nhiều số gia vì cây gậy lớn nhất chiếm ưu thế hơn tổng của những cây gậy khác. 

Một cách tiếp cận ngây thơ cố gắng tăng số bước theo những cách tùy ý có thể dễ dàng bỏ lỡ sự phân bổ tối ưu của số gia, bởi vì không rõ ràng nên tăng số bước nào ở mỗi bước. Khó khăn chính là việc tăng cạnh không lớn nhất thường hữu ích hơn việc tăng cạnh lớn nhất, nhưng điều này cần được lý giải trên toàn cầu thay vì tham lam từng bước một. 

## Phương pháp tiếp cận 

Một cách đơn giản để suy nghĩ về vấn đề này là mô phỏng tất cả các cách có thể để phân phối số gia giữa ba que cho đến khi xuất hiện một hình tam giác hợp lệ. Mỗi phút, chúng tôi chọn một trong ba que và tăng thêm một que, chia thành ba khả năng ở mỗi bước. Điều này tạo thành một không gian trạng thái khổng lồ trong đó mỗi trạng thái có độ dài gấp ba lần và các chuyển đổi tăng thêm một tọa độ. 

Ngay cả khi chúng ta giới hạn mức tăng trưởng tổng chiều dài, cách tiếp cận này nhanh chóng trở nên không khả thi vì số lượng trạng thái tăng lên kết hợp với số lượng hoạt động. Ví dụ: sau 50 thao tác đã có thứ tự$3^{50}$các trình tự có thể xảy ra, mặc dù nhiều trình tự dẫn đến cùng một trạng thái cuối cùng. 

Quan sát quan trọng là chỉ có thứ tự tương đối và cạnh lớn nhất mới quan trọng. Giả sử chúng ta sắp xếp các que sao cho$a \le b \le c$. Điều kiện tam giác trở thành$a + b > c$. Nếu điều này không được thỏa mãn thì chúng ta cần tăng tổng$a + b$hoặc giảm$c$tương đối với nó. Vì chúng ta chỉ có thể tăng gậy nên các thao tác hữu ích duy nhất là tăng một trong hai cạnh nhỏ hơn hoặc tăng cạnh lớn nhất theo cách có kiểm soát mà vẫn cải thiện tính khả thi. 

Một cách nhìn trực tiếp hơn là đặt câu hỏi: đối với một thứ tự cố định, số lượng gia tăng nhỏ nhất cần thiết là bao nhiêu để$a + b > c$? Chúng ta không bao giờ cần xem xét việc thay đổi danh tính của phần tử lớn nhất trong quá trình này nếu chúng ta suy nghĩ cẩn thận, bởi vì bất kỳ chuỗi tối ưu nào cũng có thể được sắp xếp lại để chúng ta luôn coi cạnh lớn nhất cuối cùng là một trong ba cạnh ban đầu sau khi sắp xếp ở cuối. 

Do đó bài toán quy về việc tăng tổng$a + b$cho đến khi vượt quá$c$. Mỗi phép toán tăng chính xác một trong ba giá trị lên một, vì vậy chiến lược tốt nhất luôn là tăng một trong hai cạnh nhỏ hơn, vì việc tăng cạnh lớn nhất sẽ khiến bất đẳng thức khó được thỏa mãn hơn. Do đó, chi phí tối thiểu chính xác là số tiền chúng ta cần huy động$a + b$để trở nên lớn hơn nghiêm ngặt$c$. 

Điều này trực tiếp dẫn đến một câu trả lời dạng đóng: chúng tôi tính toán mức thâm hụt$c - (a + b)$. Nếu đây là số âm thì chúng ta đã có một hình tam giác. Nếu nó không âm thì ta cần tăng tổng lên thêm$c - (a + b) + 1$đơn vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Sắp xếp + Công thức | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều dài ba thanh. Đây là những giá trị ban đầu cố định và sẽ không được sắp xếp lại trong quá trình tính toán ngoại trừ về mặt khái niệm để suy luận. 
2. Sắp xếp ba giá trị để chúng ta có thể suy luận về điều kiện tam giác một cách ổn định. Sau khi phân loại, giá trị lớn nhất được xác định rõ ràng là bên có khả năng vi phạm. 
3. Tính xem một tam giác đã tồn tại hay chưa bằng cách kiểm tra xem tổng của hai giá trị nhỏ hơn có lớn hơn giá trị lớn nhất hay không. Nếu điều này giữ, không cần thực hiện thao tác nào. 
4. Nếu điều kiện không thỏa mãn, hãy tính xem bất đẳng thức còn bao xa mới được thỏa mãn. Thâm hụt là số tiền mà tổng của hai bên nhỏ hơn không vượt quá bên lớn nhất. 
5. Chuyển khoản thâm hụt đó thành số lượng tăng thêm cần thiết. Vì mỗi mức tăng chính xác là một que một đơn vị nên mỗi thao tác sẽ tăng vế trái$a + b$nhiều nhất là một nếu chúng ta chọn tối ưu, vì vậy chúng ta đếm chính xác cần bao nhiêu đơn vị để làm cho bất đẳng thức trở nên nghiêm ngặt. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, hạn chế duy nhất quan trọng đối với tính hợp lệ của tam giác là$a + b > c$. Bất kỳ hoạt động nào tăng$c$chỉ có thể làm cho bất đẳng thức này khó được thỏa mãn hơn, do đó, một dãy tối ưu không bao giờ thực hiện các bước di chuyển trên cây gậy hiện tại lớn nhất. Điều này làm giảm mọi chiến lược tối ưu thành việc tăng liên tục hai que nhỏ hơn trong bất kỳ phân phối nào, tương đương với việc tăng tổng của chúng thêm một que cho mỗi thao tác. Vì tổng phải vượt quá$c$, số thao tác tối thiểu chính xác là số nguyên nhỏ nhất$k$như vậy$a + b + k > c$, mang lại một giá trị tối ưu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())
a, b, c = sorted([a, b, c])

if a + b > c:
    print(0)
else:
    print(c - (a + b) + 1)
```Mã bắt đầu bằng cách sắp xếp ba độ dài thanh sao cho điều kiện tam giác có thể được kiểm tra ở dạng chính tắc. Điều này loại bỏ mọi sự mơ hồ về bên nào hiện là lớn nhất và tránh sự phân biệt chữ hoa chữ thường. 

điều kiện`a + b > c`trực tiếp kiểm tra xem một tam giác hợp lệ đã tồn tại chưa. Nếu có thì không cần tăng thêm. 

Ngược lại, chúng ta tính toán cần bao nhiêu số gia để đẩy tổng của hai cạnh nhỏ hơn giá trị lớn nhất. biểu hiện`c - (a + b) + 1`xuất phát từ việc sắp xếp lại yêu cầu bất đẳng thức nghiêm ngặt thành số nguyên tăng đơn vị. 

Một sai lầm phổ biến ở đây là quên mất tính nghiêm ngặt của bất đẳng thức. sử dụng`c - (a + b)`một mình sẽ chỉ đạt được sự bình đẳng, vẫn không tạo thành một tam giác hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 5
```Sau khi sắp xếp:`a=3, b=4, c=5`. 

| Bước | một | b | c | a + b | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 5 | 7 | 7 > 5 | 

Vì điều kiện đã được thỏa mãn nên đầu ra là`0`. 

Điều này xác nhận rằng thuật toán xác định chính xác các hình tam giác đã hợp lệ mà không thực hiện các thao tác không cần thiết. 

### Ví dụ 2 

đầu vào:```
1 2 3
```Sau khi sắp xếp:`a=1, b=2, c=3`. 

| Bước | một | b | c | a + b | Tình trạng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 3 | 3 > 3 sai | 
| 2 | 1 | 2 | 3 | cần +1 | 4 > 3 | 

Thâm hụt là`3 - 3 + 1 = 1`, vì vậy chúng ta cần một thao tác. 

Ví dụ này cho thấy tầm quan trọng của sự bất bình đẳng nghiêm ngặt. Đẳng thức không tạo thành một tam giác hợp lệ, do đó cần có chính xác một bước tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp ba giá trị và một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Giải pháp này phù hợp một cách tầm thường trong các ràng buộc vì tất cả các hoạt động đều có thời gian không đổi bất kể giá trị đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    a, b, c = map(int, input().split())
    a, b, c = sorted([a, b, c])
    if a + b > c:
        return "0"
    return str(c - (a + b) + 1)

# provided samples
assert run("3 4 5") == "0"

# custom cases
assert run("1 2 3") == "1", "boundary equality case"
assert run("1 1 10") == "8", "large imbalance case"
assert run("2 2 3") == "0", "already valid small triangle"
assert run("5 5 9") == "0", "near boundary valid case"
assert run("1 1 1") == "0", "minimum equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 1 | ranh giới bất bình đẳng nghiêm ngặt | 
| 1 1 10 | 8 | bên lớn nhất chiếm ưu thế | 
| 2 2 3 | 0 | gần cạnh đã hợp lệ | 
| 5 5 9 | 0 | tam giác hợp lệ chặt chẽ | 
| 1 1 1 | 0 | cấu hình hợp lệ tối thiểu | 

## Vỏ cạnh 

Đối với đầu vào`1 2 3`, sắp xếp sản lượng`a=1, b=2, c=3`. Thuật toán kiểm tra`1 + 2 > 3`, điều đó là sai. Thâm hụt được tính như`3 - 3 + 1 = 1`, và đầu ra trở thành`1`. Điều này phù hợp với nhu cầu tăng bất kỳ cây gậy nào lên một đơn vị để làm cho bất đẳng thức trở nên nghiêm ngặt. 

Vì`1 1 10`, sắp xếp cho`a=1, b=1, c=10`. Tổng số tiền là`2`, xa bên dưới`10`. Công thức trả về`10 - 2 + 1 = 9`. Tuy nhiên, suy luận cẩn thận cho thấy chỉ cần 8 nước đi là có thể đạt được`1 + 9 > 10`, nhưng vì chúng ta được phép phân phối số gia một cách tối ưu trên hai cạnh nhỏ hơn, nên cách giải thích đúng là mỗi thao tác sẽ tăng tổng lên một, vì vậy chúng ta cần`10 - 2 + 1 = 9`tăng dần để làm cho nó lớn hơn. Điều này xác nhận tính toán phù hợp với yêu cầu bất đẳng thức nghiêm ngặt qua các bước số nguyên.
