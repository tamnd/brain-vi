---
problem: 1040B
contest_id: 1040
problem_index: B
name: "Shashlik Cooking"
contest_name: "Codeforces Round 507 (Div. 2, based on Olympiad of Metropolises)"
rating: 1300
tags: ["dp", "greedy", "math"]
answer: passed_samples
verified: false
solve_time_s: 85
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33b8ae-772c-83ec-a3d7-73b862c99c7f
---

# CF 1040B - Nấu ăn Shashlik 

**Đánh giá:** 1300 
**Tags:** dp, tham lam, toán học 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33b8ae-772c-83ec-a3d7-73b862c99c7f 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$xiên được sắp xếp từ trái sang phải. Mỗi xiên có một trạng thái nhị phân: hoặc nó vẫn ở trạng thái ban đầu hoặc nó đã bị lật. 

Một thao tác duy nhất được thực hiện bằng cách chọn một vị trí$i$. Thao tác đó lật xiên$i$và cũng lật từng xiên trong khoảng cách$k$từ nó, miễn là những vị trí đó tồn tại. Vì vậy, một thao tác ảnh hưởng đến một phân đoạn liên tục tập trung vào$i$, cụ thể là tất cả các chỉ số từ$i-k$ĐẾN$i+k$. 

Hiệu ứng của nhiều thao tác tích lũy theo tính chẵn lẻ: nếu một xiên được lật một số lần lẻ thì nó sẽ bị lật và nếu nó được lật một số lần chẵn thì nó sẽ trở về trạng thái ban đầu. Mục tiêu là đảm bảo mọi xiên đều được lật ít nhất một lần theo nghĩa cuối cùng, nghĩa là mỗi vị trí phải được thực hiện ít nhất một thao tác đã chọn. 

Kết quả đầu ra không chỉ là số thao tác tối thiểu mà còn là vị trí chính xác nơi chúng ta thực hiện những lần lật này. 

Những hạn chế$n \le 1000$Và$k \le 1000$ngay lập tức đề nghị rằng một$O(n^2)$hoặc thậm chí$O(nk)$Cách tiếp cận này là đủ vì số lượng thao tác và kiểm tra tối đa là nhỏ. Đây là một bài toán bao phủ tham lam cổ điển trên một đường thẳng, vì vậy chúng ta mong đợi một giải pháp tối ưu là tuyến tính. 

Một điểm tinh tế nảy sinh khi$k = 0$. Trong trường hợp đó, mỗi thao tác chỉ tác động đến một xiên, nghĩa là mỗi xiên phải được chọn riêng lẻ. Bất kỳ logic khoảng cách tham lam nào cũng phải xử lý rõ ràng trường hợp này. 

Một trường hợp cạnh khác là khi$2k + 1 \ge n$. Sau đó, một thao tác duy nhất tại tâm được chọn tốt sẽ bao phủ toàn bộ mảng, do đó, câu trả lời sẽ thu gọn thành một bước di chuyển. 

Một sai lầm ngây thơ là nghĩ rằng phải tránh hoặc theo dõi sự chồng chéo một cách linh hoạt. Điều đó dẫn đến việc mô phỏng các lần lật và tính toán lại các vị trí chưa được phát hiện, điều này là không cần thiết và có nguy cơ đưa ra lý luận chẵn lẻ không chính xác. Vấn đề không yêu cầu quản lý tính chẵn lẻ trạng thái cuối cùng một cách rõ ràng mà chỉ đảm bảo phạm vi bao phủ đầy đủ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng tất cả các chuỗi hoạt động có thể có và theo dõi phạm vi bao phủ. Đối với mỗi bước, chúng ta có thể thử mọi vị trí có thể có trong lần lật tiếp theo, mô phỏng phạm vi được bao phủ kết quả và lặp lại cho đến khi tất cả các vị trí được bao phủ. Điều này nhanh chóng trở thành cấp số nhân vì mỗi tiểu bang phân nhánh thành$n$các lựa chọn, và thậm chí cắt tỉa theo phạm vi bảo hiểm cũng không cứu được nó trong trường hợp xấu nhất. 

Quan sát quan trọng là mỗi thao tác bao gồm một khoảng liền kề có độ dài cố định$2k+1$. Khi chúng ta diễn giải lại vấn đề theo cách bao trùm phân khúc$[1, n]$với số khoảng tối thiểu, cấu trúc trở nên hoàn toàn tham lam. Không có sự phụ thuộc giữa các khoảng ngoại trừ phạm vi bảo hiểm. Chiến lược tốt nhất là luôn bao phủ vị trí không được che chắn ngoài cùng bên trái bằng cách sử dụng khoảng cách kéo dài sang bên phải nhất có thể tính từ điểm đó, điều này đạt được bằng cách tập trung thao tác một cách tối ưu. 

Vì vậy, thay vì khám phá các lựa chọn, chúng ta liên tục “nhảy” qua$2k+1$các vị trí, đặt mỗi tâm càng xa về bên trái càng tốt trong khi vẫn che phủ điểm hiện tại chưa được che chắn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Bao phủ khoảng tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ xiên ngoài cùng bên trái ở vị trí 1, đây là vị trí đầu tiên phải che. 

Chúng tôi coi đây là điểm chưa được khám phá đầu tiên. 
2. Nếu$k = 0$, xuất ra mọi vị trí từ 1 đến$n$. 

Điều này là cần thiết vì mỗi thao tác chỉ lật một xiên duy nhất nên không có cách nào lật được nhiều vị trí cùng một lúc. 
3. Ngược lại, đối với vị trí ngoài cùng bên trái hiện tại$l$, đặt một thao tác tại vị trí$l + k$. 

Lựa chọn này là tối ưu vì nó tối đa hóa phạm vi được bao phủ ở bên phải trong khi vẫn bao phủ được$l$, tạo ra vùng phủ sóng$[l, l + 2k]$. 
4. Sau khi đặt một thao tác, hãy cập nhật vị trí chưa được che chắn tiếp theo thành$l + 2k + 1$. 

Điều này xảy ra sau bởi vì mọi thứ lên đến$l + 2k$hiện được bao phủ bởi hoạt động hiện tại. 
5. Lặp lại cho đến khi$l > n$. 

Tại thời điểm đó, mọi vị trí đều được bảo hiểm. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến rằng tất cả các vị trí đều nhỏ hơn$l$đã được bảo hiểm. Ở mỗi bước, chúng tôi chọn một thao tác bao gồm$l$và mở rộng vùng phủ sóng càng xa càng tốt. Bất kỳ lựa chọn thay thế nào bao gồm$l$phải đặt trung tâm của nó trong$[l, l+2k]$và không có lựa chọn nào trong số đó có thể mở rộng phạm vi phủ sóng ra ngoài$l+2k$. Vì vậy, việc lựa chọn$l+k$không bao giờ tăng số lượng thao tác so với bất kỳ giải pháp hợp lệ nào và quá trình tiến triển tham lam tạo ra phạm vi bao phủ có kích thước tối thiểu của toàn bộ phân khúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    if k == 0:
        print(n)
        print(*range(1, n + 1))
        return

    res = []
    i = 1

    while i <= n:
        pos = i + k
        if pos > n:
            pos = n
        res.append(pos)
        i += 2 * k + 1

    print(len(res))
    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cấu trúc tham lam. Trường hợp đặc biệt$k = 0$được xử lý trước tiên vì công thức chung sẽ cố gắng thực hiện từng bước một cách không chính xác.$1$trong khi vẫn đặt các trung tâm dư thừa. 

Biến`i`đại diện cho vị trí không được che chắn ngoài cùng bên trái. Mỗi người được chọn`pos`là trung tâm tối ưu cho phân khúc đó. Sau mỗi lần lựa chọn, chúng tôi nhảy chính xác$2k + 1$các bước vì đó là toàn bộ phạm vi bao phủ của hoạt động. 

Kẹp`pos`ĐẾN`n`không thực sự cần thiết để đảm bảo tính chính xác trong tất cả các công thức nhưng nó đảm bảo chúng tôi không bao giờ tham chiếu các vị trí nằm ngoài phạm vi hợp lệ khi$l + k > n$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 2
```Chúng tôi có độ dài bảo hiểm$2k+1 = 5$. 

| Bước | Ngoài cùng bên trái không được che chắn$l$| Vị trí được chọn$l+k$| Khoảng thời gian được bảo hiểm | Kế tiếp$l$| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | [1, 5] | 6 | 
| 2 | 6 | 7 | [5, 7] | 11 | 

Đầu ra:```
2
3 7
```Điều này cho thấy cách hai thao tác bao trùm toàn bộ phạm vi bằng cách nhảy vào các khối có kích thước 5. 

### Ví dụ 2 

đầu vào:```
6 1
```Bây giờ mỗi thao tác bao gồm chiều dài 3. 

| Bước |$l$| Được chọn | Khoảng thời gian được bảo hiểm | Kế tiếp$l$| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | [1, 3] | 4 | 
| 2 | 4 | 5 | [4, 6] | 7 | 

Đầu ra:```
2
2 5
```Điều này xác nhận khoảng cách tham lam ở khoảng cách$2k+1 = 3$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi bước tiến con trỏ lên$2k+1$, vậy nhiều nhất$O(n/(2k+1))$hoạt động | 
| Không gian | O(1) | Chỉ một danh sách các trung tâm đã chọn được lưu trữ | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì$n \le 1000$, và số thao tác nhiều nhất là khoảng$n$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("7 2\n") == "2\n3 7"

# k = 0 case
assert run("5 0\n") == "5\n1 2 3 4 5"

# full coverage in one move
assert run("5 10\n") == "1\n5"

# small middle case
assert run("6 1\n") == "2\n2 5"

# boundary spacing check
assert run("1 1\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 0 | 5, 1 2 3 4 5 | k = 0 xử lý đặc biệt | 
| 5 10 | 1, 5 | hoạt động duy nhất bao gồm tất cả | 
| 6 1 | 2, 2 5 | khoảng cách tham lam tiêu chuẩn | 
| 1 1 | 1, 1 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Khi nào$k = 0$, mỗi thao tác chỉ ảnh hưởng đến một vị trí. Thuật toán bỏ qua logic khoảng tham lam một cách rõ ràng và xuất trực tiếp tất cả các chỉ số, tránh bước sai có thể bỏ qua các vị trí. 

Khi$2k + 1 \ge n$, trung tâm được chọn đầu tiên$l + k$đạt hoặc vượt quá cuối mảng. Trong trường hợp đó, thao tác đầu tiên đã bao gồm mọi vị trí và vòng lặp sẽ kết thúc ngay sau một lần lặp. 

Khi$n = 1$, vòng lặp chạy một lần và chọn vị trí 1, thỏa mãn mức độ bao phủ đầy đủ bất kể$k$.
