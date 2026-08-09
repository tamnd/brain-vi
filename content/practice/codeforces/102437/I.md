---
title: "CF 102437I - Xây dựng đường bộ"
description: "Chúng ta có một lưới ban đầu trống (n lần m). Một bước di chuyển bao gồm việc chọn một hình chữ nhật thẳng hàng với trục có tất cả các ô vẫn trống và có diện tích tối đa (các) ô, sau đó đánh dấu mọi ô của hình chữ nhật đó là đã được xây dựng."
date: "2026-08-09T00:28:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 131
verified: true
draft: false
---

[CF 102437I - Xây dựng đường bộ](https://codeforces.com/problemset/problem/102437/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới ban đầu trống (n \times m). Một bước di chuyển bao gồm việc chọn một hình chữ nhật thẳng hàng với trục có tất cả các ô vẫn trống và có diện tích tối đa (các) ô, sau đó đánh dấu mọi ô của hình chữ nhật đó là đã được xây dựng. Những người chơi luân phiên nhau di chuyển và người chơi thực hiện nước đi hợp pháp cuối cùng sẽ thắng. 

Nhiệm vụ chỉ là xác định xem người chơi đầu tiên, Sam, có chiến lược chiến thắng hay không. Chúng tôi không phải xây dựng các bước di chuyển. Các ràng buộc và câu lệnh được đưa ra bởi trang vấn đề của Codeforces. 

Kích thước tối đa là (1000), vì vậy bản thân bảng có thể chứa tối đa (10^6) ô. Điều đó loại trừ bất cứ điều gì duy trì rõ ràng tất cả các trạng thái trò chơi có thể có. Trò chơi không chỉ đơn thuần là chọn một số ô, vì các ô được chọn phải tạo thành một hình chữ nhật trống. Lời giải phải khai thác tính đối xứng hình học của toàn bộ bàn cờ hơn là mô phỏng trò chơi. 

Các trường hợp cạnh hữu ích nhất chính xác là những trường hợp trong đó tính chẵn lẻ thay đổi hình dạng của tâm bảng. 

Vì```
1 4 1
```nước đi duy nhất có thể có diện tích (1), vì vậy mỗi nước đi sẽ tạo chính xác một ô. Có bốn ô, do đó có bốn nước đi được thực hiện và người chơi thứ hai thắng. Câu trả lời đúng là`NO`. Việc triển khai coi sự tồn tại của các ô liền kề là đủ cho chiến lược người chơi đầu tiên sẽ mắc sai lầm này. 

Vì```
2 2 3
```bảng có bốn ô, nhưng hình chữ nhật lớn nhất được phép có diện tích (3). Hình chữ nhật nhỏ nhất đối xứng qua một góc quay (180^\circ) có kích thước (2 \times 2), do đó có diện tích (4). Vì không có nước đi hợp pháp nào có thể chiếm được một hình chữ nhật như vậy nên người chơi thứ hai có thể sử dụng phép quay đối xứng và thắng. Câu trả lời là`NO`. 

Vì```
2 3 2
```hình chữ nhật đối xứng ở giữa nhỏ nhất là cột ở giữa, có kích thước (2 \times 1) và diện tích (2). Sam có thể xây cột này trước. Hai bên còn lại là các góc quay chính xác của nhau nên mọi chuyển động sau này đều có thể được phản ánh. Câu trả lời là`YES`. 

Vì```
3 3 1
```trung tâm là một tế bào duy nhất. Sam xây ô đó trước, sau đó phản chiếu mọi bước di chuyển của đối thủ qua trung tâm. Câu trả lời là`YES`. Trường hợp này cho thấy tại sao một chiều lẻ chỉ đóng góp một ô vào hình chữ nhật đối xứng nhỏ nhất. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ coi tình huống đó là một trò chơi công bằng nói chung. Từ tập hợp các ô được xây dựng hiện tại, chúng ta có thể liệt kê tối đa (các) hình chữ nhật trống có diện tích, giải đệ quy vị trí kết quả và tuyên bố vị trí thắng nếu có ít nhất một nước đi đến vị trí thua. 

Điều này đúng vì mọi nước đi hợp pháp đều được xem xét và trò chơi là hữu hạn. Vấn đề là số lượng trạng thái. Với (nm) ô, có thể có tới (2^{nm}) cấu hình ô tích hợp khác nhau. Ngay cả khi mọi hình chữ nhật có thể được kiểm tra trong thời gian không đổi thì số hình chữ nhật có thể có trong một bảng (n \times m) là 

[ 
\frac{n(n+1)m(m+1)}{4}, 
] 

vì vậy một cách tiếp cận tối đa hóa toàn diện có thể yêu cầu 

[ 
O\left(2^{nm} n^2m^2\right) 
] 

làm việc trong trường hợp xấu nhất. Với (nm) đạt tới (10^6), điều này không khả thi chút nào. 

Brute-force hoạt động vì trò chơi hoàn toàn được xác định bởi tập hợp các ô đã được xây dựng sẵn, nhưng cách biểu diễn đó chứa nhiều thông tin hơn chúng ta cần đối với hình chữ nhật trống ban đầu. Quan sát quan trọng là bảng có sự đối xứng quay tự nhiên (180^\circ). 

Lấy bất kỳ hình chữ nhật nào và xoay nó theo (180^\circ) xung quanh tâm bảng. Nếu hình chữ nhật ban đầu và bản sao xoay của nó không khớp với nhau thì sau khi một người chơi xây dựng hình chữ nhật ban đầu, người chơi khác có thể tạo bản sao xoay của nó. Vì phép quay bảo toàn cả hình dạng và diện tích nên đáp án luôn là một bước đi hợp pháp miễn là bước ban đầu hợp lệ. 

Điều này mang lại cho người chơi thứ hai một chiến lược chiến thắng bất cứ khi nào mọi hình chữ nhật hợp lệ tách rời khỏi bản sao được xoay của nó. Cách duy nhất để một hình chữ nhật có thể cắt góc quay (180^\circ) của nó là để hình chữ nhật đi qua tâm bảng theo cả hai chiều. 

Điều đó ngay lập tức làm giảm vấn đề tìm diện tích nhỏ nhất có thể của hình chữ nhật có thể giao với bản sao được xoay của chính nó. Đối với kích thước lẻ, một hàng hoặc cột ở giữa là đủ. Đối với kích thước chẵn, tâm nằm giữa hai hàng hoặc hai cột, do đó cần có ít nhất hai hàng hoặc cột. 

Xác định 

[ 
c(x)= 
\bắt đầu{trường hợp} 
1,&x\text{ là số lẻ},\ 
2,&x\text{ là số chẵn}. 
\end{trường hợp} 
] 

Hình chữ nhật đối xứng tâm nhỏ nhất có kích thước (c(n)\times c(m)), nên diện tích của nó là 

[ 
c(n)c(m). 
] 

Nếu (s) nhỏ hơn giá trị này thì không có nước đi hợp lệ nào có thể đi qua tâm theo cả hai hướng. Người chơi thứ hai có thể phản chiếu mọi nước đi nên Sam thua. 

Nếu (s) ít nhất là giá trị này, Sam có thể tạo hình chữ nhật đối xứng ở giữa làm bước đi đầu tiên của mình. Sau khi loại bỏ nó, không có ô nào được cố định bằng cách xoay, vì vậy mọi hình chữ nhật hợp lệ còn lại đều có một đối tác xoay riêng biệt, rời rạc. Sau đó Sam phản chiếu mọi nước đi của đối thủ và thực hiện nước đi cuối cùng. 

Do đó, toàn bộ trò chơi giảm xuống còn một so sánh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{nm}n^2m^2)) | (O(2^{nm})) | Quá chậm | 
| Tối ưu | (O(1)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định số hàng tối thiểu mà một hình chữ nhật phải chiếm để vượt qua tâm thẳng đứng của bảng. Đó là (1) khi (n) lẻ và (2) khi (n) chẵn. 
2. Xác định số cột tối thiểu mà hình chữ nhật phải chiếm để vượt qua tâm ngang. Đó là (1) khi (m) lẻ và (2) khi (m) chẵn. 
3. Nhân hai giá trị này. Gọi kết quả`need`. Đây là diện tích của hình chữ nhật nhỏ nhất có thể đáp ứng được vòng quay (180^\circ) của chính nó. 
4. So sánh (các) với`need`. Nếu (s \geq \text{need}), Sam sẽ xây hình chữ nhật ở giữa đó trước và giành chiến thắng nhờ tính đối xứng xoay. 
5. Nếu (s < \text{need}), không có nước đi đầu tiên hợp lệ nào có thể giao với bản sao được xoay của nó. Người chơi thứ hai có thể xoay mỗi nước đi (180^\circ) và luôn có được một nước đi hợp pháp khác, vì vậy Sam thua. 

### Tại sao nó hoạt động 

Xét chuyển động quay (180^\circ) của bảng. Một hình chữ nhật không thể có một đối tượng xoay riêng biệt chỉ khi nó giao với hình ảnh được xoay của nó. Để hai hình chữ nhật giao nhau, hình chữ nhật ban đầu phải cắt qua tâm của bảng dọc theo cả hai chiều. Hình chữ nhật nhỏ nhất có thể có như vậy có một hàng ở giữa khi kích thước tương ứng là số lẻ và hai hàng ở giữa khi kích thước tương ứng là số chẵn, với cùng một quy tắc cho các cột. Như vậy`need`chính xác là diện tích nhỏ nhất của hình chữ nhật có thể cản trở việc ghép cặp quay. 

Khi (s < \text{need}), mọi hình chữ nhật hợp lệ sẽ rời khỏi hình ảnh được xoay của nó. Sau khi người chơi thứ hai phản hồi bằng hình chữ nhật được xoay, phản hồi không thể chồng lên bất kỳ thứ gì đã được tạo vì hai hình chữ nhật rời nhau. Xoay cũng bảo toàn diện tích, vì vậy phản ứng là hợp pháp. Mỗi nước đi của Sam đều được ghép nối với chính xác một phản ứng, nghĩa là Sam không thể thực hiện nước đi cuối cùng. 

Khi (s \geq \text{need}), đầu tiên Sam sẽ tạo hình chữ nhật ở giữa. Nó bất biến khi xoay, vì vậy sau khi loại bỏ nó, mỗi ô còn lại thuộc về một cặp ô riêng biệt khi xoay. Bất kỳ hình chữ nhật hợp pháp nào sau này không thể giao nhau với hình ảnh được xoay của chính nó, bởi vì bất kỳ hình chữ nhật nào như vậy sẽ phải chạm vào khu vực trung tâm đã được chiếm giữ. Do đó, Sam có thể phản chiếu mọi nước đi của đối thủ và cuối cùng thực hiện nước đi cuối cùng. 

Sự so sánh với`need`như vậy là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s = map(int, input().split())

    need_n = 1 if n % 2 else 2
    need_m = 1 if m % 2 else 2

    need = need_n * need_m

    print("YES" if s >= need else "NO")

if __name__ == "__main__":
    solve()
```Đầu vào chứa chính xác một trường hợp thử nghiệm, vì vậy`solve()`đọc ba số nguyên một lần. Các giá trị`need_n`Và`need_m`biểu thị số lượng hàng và cột tối thiểu cần thiết để một hình chữ nhật có thể vươn qua tâm bảng. 

Phép nhân cho diện tích nhỏ nhất có thể có của một hình chữ nhật có thể chồng lên góc quay (180^\circ) của nó. Không cần kiểm tra xem hình chữ nhật đó có phù hợp về mặt vật lý hay không, bởi vì (1) hoặc (2) các hàng và cột trung tâm luôn tồn tại với các chiều dương. 

Sự so sánh mang tính bao hàm. Khi`s == need`, hình chữ nhật ở giữa là hợp lệ, vì vậy câu trả lời phải là`YES`. sử dụng`>`thay vì`>=`sẽ từ chối không chính xác tất cả các trường hợp có ranh giới chính xác như (2\times3) với (s=2). 

Số nguyên Python có độ chính xác tùy ý, mặc dù ở đây không yêu cầu số học lớn. Giá trị liên quan lớn nhất chỉ là (4). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đã cho là```
1 4 2
```Kích thước đầu tiên là số lẻ nên một hàng là đủ để chạm tới tâm. Chiều thứ hai là số chẵn nên cần có hai cột. Do đó, hình chữ nhật đối xứng tâm nhỏ nhất có diện tích (1\cdot2=2). 

| (n) | (m) | (các) |`need_n`|`need_m`|`need`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 2 | 1 | 2 | 2 | CÓ | 

Vì (s=2) đạt đến ranh giới chính xác nên Sam có thể lấy hai ô trung tâm. Hai ô còn lại bên trái và hai ô còn lại bên phải được ghép luân phiên nhau nên Sam có thể trả lời mọi nước đi bằng gương của nó. Điều này mang lại yêu cầu`YES`. 

### Ví dụ 2 

Hãy xem xét```
2 2 3
```Cả hai kích thước đều bằng nhau nên cần có hai hàng và hai cột để đến tâm. Hình chữ nhật đối xứng ở tâm nhỏ nhất là toàn bộ bảng (2\times2). 

| (n) | (m) | (các) |`need_n`|`need_m`|`need`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 3 | 2 | 2 | 4 | KHÔNG | 

Diện tích cho phép tối đa chỉ là (3), nên Sam không thể chiếm hình chữ nhật ở giữa (2\times2). Mỗi hình chữ nhật hợp pháp có một bản sao được xoay riêng biệt. Người chơi thứ hai luôn có thể lấy bản sao đó, vì vậy người chơi thứ nhất thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) | Chỉ có hai lần kiểm tra tính chẵn lẻ và một lần so sánh được thực hiện. | 
| Không gian | (O(1)) | Chỉ có một số lượng biến số nguyên không đổi được lưu trữ. | 

Bảng có thể chứa tối đa (10^6) ô, nhưng thuật toán không bao giờ xây dựng bảng. Thời gian chạy và mức sử dụng bộ nhớ của nó độc lập với (n), (m) và (s), vì vậy nó dễ dàng phù hợp với các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n, m, s = map(int, input().split())

    need_n = 1 if n % 2 else 2
    need_m = 1 if m % 2 else 2

    need = need_n * need_m

    print("YES" if s >= need else "NO")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("1 4 2\n") == "YES\n", "sample 1"

assert run("1 1 1\n") == "YES\n", "minimum board"

assert run("1 4 1\n") == "NO\n", "one-dimensional even board with s=1"

assert run("2 2 3\n") == "NO\n", "even-by-even boundary"

assert run("2 3 2\n") == "YES\n", "even-by-odd boundary"

assert run("3 3 1\n") == "YES\n", "odd-by-odd with only single cells"

assert run("1000 1000 1000000\n") == "YES\n", "maximum-size board"

assert run("1000 1000 3\n") == "NO\n", "maximum-size board below threshold"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`YES`| Bảng nhỏ nhất có thể và trung tâm lẻ | 
|`1 4 1`|`NO`| Bảng chẵn một chiều và trường hợp chẵn lẻ (s=1) | 
|`2 2 3`|`NO`| Bảng chẵn trong đó (các) bảng nằm dưới ngưỡng | 
|`2 3 2`|`YES`| Bảng chẵn lẻ trong đó (các) bảng đạt chính xác ngưỡng | 
|`3 3 1`|`YES`| Bảng lẻ chỉ có các bước di chuyển một ô | 
|`1000 1000 1000000`|`YES`| Kích thước tối đa và diện tích tối đa cho phép | 
|`1000 1000 3`|`NO`| Bảng lớn đều nhau nhưng không đủ diện tích | 

## Vỏ cạnh 

cho```
1 4 1
```chúng tôi có`need_n = 1`Và`need_m = 2`, cho`need = 2`. Vì (s=1<2) nên không có hình chữ nhật hợp lệ nào có thể vượt qua tâm bàn cờ. Trò chơi chỉ đơn giản là bốn bước đi độc lập của một ô, vì vậy người chơi thứ hai sẽ đi ô thứ tư. Thuật toán trả về`NO`. 

Vì```
2 2 3
```cả hai chiều đều bằng nhau, cho`need_n = 2`,`need_m = 2`, Và`need = 4`. Một hình chữ nhật có thể cản trở việc sao chép quay của nó phải bao phủ hai hàng trung tâm và hai cột trung tâm, nghĩa là toàn bộ bảng (2\times2). Vì (s=3) nên hình chữ nhật đó không có sẵn. Người chơi thứ hai phản chiếu mọi nước đi, đưa ra`NO`. 

Vì```
2 3 2
```kích thước cho`need_n = 2`Và`need_m = 1`, Vì thế`need = 2`. Cột trung tâm (2\times1) là hợp lệ. Sam xây dựng nó trước tiên, để lại các vùng bên trái và bên phải (2\times1) được ghép nối bằng cách xoay. Bất kỳ hình chữ nhật nào của đối thủ đều nằm ở bên này hoặc bên kia và có một đối tượng xoay rời rạc. Sam có thể phản chiếu mọi nước đi nên kết quả là`YES`. 

Vì```
3 3 1
```cả hai chiều đều lẻ, vì vậy`need = 1`. Bản thân tế bào trung tâm duy nhất được cố định xoay. Sam lấy nó trước, sau đó mỗi ô còn lại có một ô đối diện riêng biệt. Bất kỳ hình chữ nhật hợp pháp nào cũng có một bản sao được xoay rời rạc và Sam có thể phản hồi một cách đối xứng. Kết quả là`YES`. 

Trường hợp tối đa```
1000 1000 1000000
```có`need = 4`vì cả hai chiều đều chẵn. Vì toàn bộ bàn cờ là hợp pháp nên Sam cũng có thể chỉ cần xây dựng toàn bộ bàn cờ chỉ trong một nước đi. Thuật toán trả về`YES`mà không cần xây dựng bất kỳ ô nào trong số (10^6). 

Cùng một bảng với```
1000 1000 3
```có cùng ngưỡng là (4), nhưng (s=3) là không đủ. Việc ghép luân phiên thuộc về người chơi thứ 2 nên thuật toán trả về đúng`NO`.
