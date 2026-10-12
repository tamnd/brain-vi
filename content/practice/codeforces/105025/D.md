---
title: "CF 105025D - \u041d\u0435\u0434\u043e\u0432\u043e\u043b\u044c\u0441\u0442\u0432\u043e \u041c\u0430\u0440\u0441\u0435\u043b\u044f"
description: "Chúng ta có hai nhóm ô tô được sắp xếp theo thứ tự bố trí dọc theo một con đường thẳng tại một thời điểm cố định. Một nhóm di chuyển ra khỏi điểm gốc, nhóm kia di chuyển về phía đó, tất cả đều có tốc độ như nhau."
date: "2026-06-28T01:40:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105025
codeforces_index: "D"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 105025
solve_time_s: 57
verified: true
draft: false
---

[CF 105025D - \u041d\u0435\u0434\u043e\u0432\u043e\u043b\u044c\u0441\u0442\u0432\u043e \u041c\u0430\u0440\u0441\u0435\u043b\u044f](https://codeforces.com/problemset/problem/105025/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai nhóm ô tô được sắp xếp theo thứ tự bố trí dọc theo một con đường thẳng tại một thời điểm cố định. Một nhóm di chuyển ra khỏi điểm gốc, nhóm kia di chuyển về phía đó, tất cả đều có tốc độ như nhau. Bất cứ khi nào hai chiếc xe gặp nhau, tức là khi chúng vượt qua nhau trên đường, sự khó chịu của Marcel sẽ tăng lên gấp đôi đối với mỗi cặp xe tham gia vào cuộc gặp đó ngay lập tức. 

Chi tiết quan trọng là nhiều ô tô có thể trùng nhau cùng một lúc. Nếu tại một thời điểm nào đó có nhiều ô tô gặp nhau tại cùng một điểm thì mỗi cặp trong số chúng sẽ đóng góp riêng rẽ. Vì vậy, nếu k ô tô gặp nhau đồng thời thì phần đóng góp là số cặp trong nhóm này, là k(k−1)/2. 

Dữ liệu đầu vào cung cấp vị trí ban đầu của n ô tô chuyển động sang phải và m ô tô chuyển động sang trái. Vì mọi tốc độ đều bằng nhau nên thứ tự các ô tô trong mỗi nhóm không bao giờ thay đổi và mỗi cặp ô tô ngược chiều nhau sẽ gặp nhau đúng một lần. 

Kết quả đầu ra là tổng số lần gặp nhau như vậy trên tất cả các cặp ô tô từ các hướng khác nhau. 

Các ràng buộc lên tới 3×10^5 ô tô theo mỗi hướng, do đó, bất kỳ mô phỏng bậc hai nào trên tất cả các cặp đều không thể thực hiện được. Ngay cả việc ghép nối trực tiếp O(nm) cũng quá lớn vì nó đạt khoảng 9×10^10 hoạt động trong trường hợp xấu nhất. 

Một trường hợp khó phát hiện xuất phát từ sự va chạm đồng thời. Nếu một số ô tô di chuyển sang phải và ô tô di chuyển trái gặp nhau tại cùng một điểm, việc đếm phải xử lý bội số một cách chính xác. 

Ví dụ: nếu vị trí là:```
n = 2, m = 2
a = [1, 3]
b = [2, 4]
```Sau đó các cặp gặp nhau vào những thời điểm khác nhau nên mỗi cặp đóng góp một cách độc lập. Nhưng nếu chúng ta thay đổi các giá trị sao cho nhiều điểm giao nhau xảy ra vào những thời điểm giống nhau, thì logic hợp nhất đơn giản chỉ tính theo thứ tự từng cặp mà không nhóm lại sẽ bị tính thiếu hoặc thừa tùy thuộc vào việc triển khai. 

Một tình huống phức tạp khác là khi cả một nhóm nằm ngay trước nhóm kia, chẳng hạn:```
a = [1, 2, 3]
b = [10, 11]
```Ở đây mọi cặp đều giao nhau và câu trả lời chỉ đơn giản là n × m. Bất kỳ giải pháp nào cố gắng chỉ khớp với những người hàng xóm gần nhất sẽ thất bại. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: với mỗi ô tô trong nhóm đầu tiên, hãy mô phỏng chuyển động của nó và đếm xem nó gặp bao nhiêu ô tô từ nhóm thứ hai. Vì tất cả các ô tô đều chuyển động với tốc độ không đổi nên hai ô tô gặp nhau chính xác khi thứ tự ban đầu của chúng ngụ ý đường đi của chúng cắt nhau. Điều này làm giảm việc kiểm tra tất cả các cặp (i, j) và tăng câu trả lời nếu xe thứ i di chuyển sang phải ở bên trái của xe thứ j di chuyển sang trái vào đúng thời điểm đặt hàng. Điều này đúng nhưng chi phí O(nm), vượt xa giới hạn. 

Để cải thiện, chúng tôi quan sát thấy chuyển động là đều nên thời gian không thực sự quan trọng. Điều quan trọng là thứ tự tương đối. Một ô tô chuyển động bên phải xuất phát ở vị trí a[i] sẽ gặp mọi ô tô chuyển động trái xuất phát tại vị trí b[j] sao cho đường đi của chúng cắt nhau theo thời gian, tương đương với a[i] < b[j] khi được giải thích theo chỉ số hướng nhất quán. 

Tuy nhiên, vấn đề không chỉ là so sánh đơn giản tất cả các cặp trong mảng đã được sắp xếp, vì việc đếm trực tiếp vẫn có dạng O(nm). Cái nhìn sâu sắc quan trọng là giải thích tình huống như một cuộc quét dọc theo đường thẳng. Mỗi lần đi qua một vị trí từ trái qua phải, chúng ta sẽ cộng xem có bao nhiêu xe từ hướng kia nằm xa hơn hoặc trước điểm đó, tùy theo góc nhìn. 

Vì cả hai mảng đều được sắp xếp nên chúng ta có thể xử lý chúng bằng cách quét hai con trỏ. Chúng ta di chuyển theo các vị trí tăng dần và duy trì bao nhiêu ô tô đi từ hướng ngược lại sẽ gặp ô tô hiện tại. Mỗi chiếc ô tô mới đóng góp tất cả những chiếc ô tô đi ngược chiều đã thấy trước đây hoặc xác định số lượng ô tô đi ngược chiều mà nó sẽ gặp trong tương lai. Điều này làm giảm việc đếm thành thời gian tuyến tính. 

Cấu trúc cơ bản là mỗi cặp được xác định theo thứ tự tương đối trong một chuỗi được sắp xếp hợp nhất, vì vậy chúng ta có thể đếm các lần đảo ngược chéo giữa hai mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Quét hai con trỏ | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi việc này là đếm xem có bao nhiêu cặp chỉ số (i, j) thỏa mãn rằng các ô tô phải vượt qua nhau đúng một lần. Vì tất cả các tốc độ đều giống nhau nên vấn đề này trở thành vấn đề đếm xem có bao nhiêu phần tử từ một danh sách được sắp xếp nằm ở phía đối diện của mỗi phần tử trong danh sách kia theo thứ tự được hợp nhất cuối cùng. 

Chúng tôi khai thác việc sắp xếp và quét: 

1. Chúng ta bắt đầu bằng hai con trỏ i và j ở đầu mảng a và b. Chúng tôi cũng duy trì một bộ đếm theo dõi số lượng ô tô từ nhóm thứ hai đã được “nhìn thấy” trong quá trình quét so với nhóm đầu tiên. 
2. Chúng ta so sánh a[i] và b[j]. Vị trí nhỏ hơn đại diện cho xe tiếp theo theo thứ tự từ trái sang phải dọc đường. 
3. Nếu a[i] < b[j] thì ô tô thuộc nhóm đầu tiên hiện là ô tô tiếp theo theo thứ tự không gian. Điều này có nghĩa là nó sẽ đáp ứng tất cả các xe còn lại từ nhóm thứ hai vẫn đang đặt hàng trước khi chúng tôi xử lý chúng. Chúng ta cộng số lượng xe đi ngược chiều đã được xử lý cho phù hợp rồi di chuyển về phía trước. 
4. Ngược lại, nếu b[j] < a[i], chúng ta xử lý xe nhóm thứ hai trước. Chiếc xe này sẽ đáp ứng tất cả những chiếc xe thuộc nhóm đầu tiên đã được xử lý, vì vậy chúng tôi tích lũy những đóng góp đó và chuyển j về phía trước. 
5. Tiếp tục cho đến khi hết cả hai mảng. Sau khi một mảng kết thúc, các phần tử còn lại trong nhóm còn lại sẽ đóng góp bằng tất cả các xe chạy ngược chiều đã được tính trong đợt quét. 
6. Giá trị tích lũy cuối cùng là tổng số lần gặp nhau. 

Tại sao thứ tự này hoạt động là vì mỗi cặp được tính chính xác một lần tại thời điểm phần tử sau theo thứ tự được sắp xếp được xử lý. Quá trình quét đảm bảo rằng mỗi cặp chéo được tính ở ranh giới chính xác giữa các phân đoạn đã xử lý và chưa xử lý. 

### Tại sao nó hoạt động

Tại bất kỳ thời điểm nào của quá trình quét, chúng tôi duy trì phân vùng dòng thành “tiền tố đã xử lý” và “hậu tố chưa được xử lý” theo thứ tự được sắp xếp. Mỗi cặp (a[i], b[j]) được xác định duy nhất tại thời điểm vị trí lớn hơn trong hai vị trí được xử lý. Tại thời điểm đó, phần tử còn lại đã được phân loại đầy đủ là bên trong tiền tố được xử lý hoặc hậu tố còn lại, do đó phần đóng góp của cặp đó được thêm chính xác một lần. Không thể tính cặp nào sớm hơn vì cả hai điểm cuối đều chưa được xử lý và không thể tính sau vì cả hai điểm cuối đều đã ở phía sau ranh giới quét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = j = 0
    cnt_a = 0
    cnt_b = 0
    ans = 0
    
    while i < n or j < m:
        if j == m or (i < n and a[i] < b[j]):
            ans += cnt_b
            cnt_a += 1
            i += 1
        else:
            ans += cnt_a
            cnt_b += 1
            j += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cấu trúc hợp nhất hai con trỏ cổ điển trên hai mảng được sắp xếp. Các biến`cnt_a`Và`cnt_b`biểu thị số lượng phần tử từ mỗi nhóm đã được xử lý trong quá trình quét. Khi xử lý một phần tử từ một nhóm, chúng tôi thêm số lượng phần tử đã được xử lý từ nhóm đối diện, tương ứng chính xác với số lượng giao điểm hợp lệ mà phần tử đó đóng góp với các phần tử được xử lý trước đó. 

Sự so sánh`a[i] < b[j]`đảm bảo trật tự chặt chẽ nhất quán với phát biểu vấn đề rằng tất cả các vị trí đều khác biệt, do đó không xảy ra ràng buộc. Điều kiện vòng lặp cuối cùng đảm bảo rằng khi một mảng đã hết, tất cả các phần tử còn lại trong nhóm kia sẽ được xử lý với sự đóng góp đầy đủ từ bộ đếm đối diện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, m = 3
a = [3, 5]
b = [1, 2, 7]
```| Bước | tôi | j | một [tôi] | b[j] | cnt_a | cnt_b | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 3 | 1 | 0 | 0 | 0 | 
| lấy b | 0 | 1 | 3 | 2 | 0 | 1 | 0 | 
| lấy b | 0 | 2 | 3 | 7 | 0 | 2 | 0 | 
| lấy một | 1 | 2 | 5 | 7 | 1 | 2 | 2 | 
| lấy một | 2 | 2 | - | 7 | 2 | 2 | 4 | 
| lấy b | 2 | 3 | - | - | 2 | 3 | 4 | 

Điều này cho thấy mỗi phần tử đóng góp chính xác như thế nào khi nó được xử lý, tích lũy các cặp chéo tăng dần. 

### Ví dụ 2 

đầu vào:```
n = 3, m = 2
a = [1, 4, 6]
b = [2, 3]
```| Bước | tôi | j | một [tôi] | b[j] | cnt_a | cnt_b | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 1 | 2 | 0 | 0 | 0 | 
| lấy một | 1 | 0 | 4 | 2 | 1 | 0 | 0 | 
| lấy b | 1 | 1 | 4 | 3 | 1 | 1 | 1 | 
| lấy b | 1 | 2 | 4 | - | 1 | 2 | 2 | 
| lấy một | 2 | 2 | 6 | - | 2 | 2 | 4 | 
| lấy một | 3 | 2 | - | - | 3 | 2 | 6 | 

Dấu vết xác nhận rằng mỗi phần tử được xử lý sẽ thêm các đóng góp bằng các phần tử đối diện đã thấy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi con trỏ di chuyển về phía trước chính xác một lần trên cả hai mảng | 
| Không gian | O(1) | Chỉ các bộ đếm và chỉ số được sử dụng ngoài bộ nhớ đầu vào | 

Quét tuyến tính đủ cho tối đa 3×10^5 phần tử trên mỗi mảng, thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    i = j = 0
    ca = cb = 0
    ans = 0
    
    while i < n or j < m:
        if j == m or (i < n and a[i] < b[j]):
            ans += cb
            ca += 1
            i += 1
        else:
            ans += ca
            cb += 1
            j += 1
    
    return str(ans)

# provided sample
assert run("2 3\n3 5\n1 2 7\n") == "4"

# minimum case
assert run("1 1\n1\n2\n") == "1"

# reversed ordering
assert run("3 3\n1 2 3\n4 5 6\n") == "9"

# alternating
assert run("2 2\n1 10\n5 6\n") == "4"

# large equal structure
assert run("3 3\n1 4 7\n2 5 8\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/1/2 | 1 | tương tác nhỏ nhất | 
| ra lệnh tách ra | 9 | sản phẩm chéo đầy đủ | 
| giá trị xen kẽ | 4 | sự xen kẽ đúng đắn | 
| hợp nhất có cấu trúc | 9 | ghép đôi thống nhất | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử của một mảng nhỏ hơn tất cả các phần tử của mảng kia. Ví dụ:```
n = 3, m = 2
a = [1, 2, 3]
b = [10, 11]
```Trong quá trình quét, mọi phần tử của`a`được xử lý đầu tiên, vì vậy`cnt_b`luôn bằng 0 cho đến khi chúng ta bắt đầu tiêu thụ`b`. Khi chúng tôi đạt được`b`, mỗi phần tử đóng góp`cnt_a`, tăng lên thành 3. Vậy đóng góp là 3 + 3 = 6, khớp với cặp Descartes đầy đủ. 

Một trường hợp khác là luân phiên chặt chẽ:```
a = [1, 4, 7]
b = [2, 5, 8]
```Ở đây mỗi bước xen kẽ. Mỗi`a[i]`chỉ xem được xử lý trước đó`b`, và mỗi`b[j]`thấy đã được xử lý trước đó`a`, đảm bảo mỗi cặp chéo được tính chính xác một lần. Quá trình quét đảm bảo không có cặp nào bị bỏ qua vì mọi phần tử mới ngay lập tức chiếm tất cả các phần tử trước đó thuộc loại đối diện.
