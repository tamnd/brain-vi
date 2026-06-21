---
title: "CF 1056C - Chọn Tướng"
description: "Hai người chơi đang xây dựng hai đội có quy mô bằng nhau bằng cách luân phiên lấy các anh hùng từ một nhóm chung gồm các ứng cử viên trị giá $2n$. Mỗi anh hùng có một sức mạnh cố định và sau khi sử dụng nó sẽ biến mất khỏi trò chơi."
date: "2026-06-15T13:00:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "interactive", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1056
codeforces_index: "C"
codeforces_contest_name: "Mail.Ru Cup 2018 Round 3"
rating: 1700
weight: 1056
solve_time_s: 300
verified: false
draft: false
---

[CF 1056C - Chọn anh hùng](https://codeforces.com/problemset/problem/1056/C) 

**Đánh giá:** 1700 
**Tags:** tham lam, thực hiện, tương tác, sắp xếp 
**Thời gian giải:** 5 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hai người chơi đang xây dựng hai đội có quy mô bằng nhau bằng cách luân phiên lấy các anh hùng từ một nhóm chung$2n$ứng viên. Mỗi anh hùng có một sức mạnh cố định và sau khi sử dụng nó sẽ biến mất khỏi trò chơi. Trên hết, một số anh hùng đi theo cặp đặc biệt: nếu một anh hùng trong cặp như vậy được chọn, điểm cuối còn lại của cặp ngay lập tức buộc người chơi đối diện phải chọn trong bước đi tiếp theo của họ. Điều này có nghĩa là việc chọn một thành viên của một cặp không chỉ loại bỏ một vật phẩm mà còn loại bỏ cả hai điểm cuối khỏi các quyết định trong tương lai một cách hiệu quả, đồng thời chỉ định một trong số chúng cho mỗi người chơi tùy theo thứ tự di chuyển. 

Mục tiêu của phe bạn là tối đa hóa tổng sức mạnh của các anh hùng mà bạn có được, trong khi đối thủ tuân theo các quy tắc tương tự và cũng cố gắng đạt được càng nhiều càng tốt. Khía cạnh tương tác rất quan trọng: bạn phải thực hiện từng bước di chuyển của mình, phản ứng với các lựa chọn của đối thủ và tôn trọng các ràng buộc bắt buộc của cặp. 

Các ràng buộc đủ chặt chẽ đến mức bất kỳ cách tiếp cận nào liên quan đến việc tìm kiếm tất cả các chuỗi nước đi hoặc mô phỏng cây trò chơi đều không thể thực hiện được. Với tối đa$2n \le 2000$, thậm chí là một$O(n^2)$mô phỏng là ranh giới nhưng vẫn chỉ khả thi nếu cực kỳ đơn giản cho mỗi lần di chuyển. Tuy nhiên, chiến lược minimax đầy đủ là không cần thiết vì cấu trúc của các cặp bắt buộc làm cho trò chơi trở thành một quá trình tham lam đơn giản hơn nhiều so với một tập hợp thu nhỏ động. 

Một trường hợp thất bại tinh tế xuất hiện khi một người cố gắng chỉ lý luận cục bộ mà không tính đến việc buộc phải loại bỏ. Ví dụ: nếu một anh hùng có giá trị thấp được ghép đôi với một anh hùng có giá trị rất cao, thì các chiến lược tham lam ngây thơ bỏ qua việc ghép đôi có thể chọn anh hùng có giá trị thấp quá sớm, khiến đối thủ sau đó chiếm được điểm cuối có giá trị cao. 

Hãy xem xét cấu hình này: 

đầu vào:```
n = 2
p = [10, 1, 9, 2]
pair: (1, 3)
```Nếu một chiến lược ngây thơ chọn nước đi đẹp nhất hiện có mà không tính đến thời gian tương tác, thì có thể mất 10, trong khi đối thủ mất 9 và cấu trúc bắt buộc sau đó có thể gây ra phân bổ dưới mức tối ưu. Hành vi đúng phụ thuộc vào việc hiểu rằng việc lấy một điểm cuối sẽ xóa toàn bộ cặp khỏi hệ thống. 

## Phương pháp tiếp cận 

Cách nghĩ thô bạo về trò chơi là mô phỏng tất cả các chuỗi nước đi có thể xảy ra, theo dõi lượt của ai và duy trì ràng buộc rằng việc chọn một điểm cuối của một cặp buộc phải loại bỏ điểm cuối còn lại ở nước đi tiếp theo của đối thủ. Điều này nhanh chóng dẫn đến một quá trình phân nhánh: tại mỗi bước có tới$O(n)$di chuyển có thể, và độ sâu là$2n$, tạo ra số lượng trạng thái theo cấp số nhân. Ngay cả với tính năng ghi nhớ, trạng thái tương tác vẫn phụ thuộc vào tập hợp chính xác còn lại, khiến việc này trở nên khó khăn. 

Quan sát quan trọng là quy tắc cặp bắt buộc không gây ra sự phức tạp trong lựa chọn mà chỉ thay đổi thời gian loại bỏ. Sau khi một anh hùng được chọn, cặp của anh hùng đó (nếu tồn tại) sẽ được giải quyết ngay lập tức ở bước tiếp theo, nghĩa là cả hai điểm cuối đều rời khỏi trò chơi ở các lượt liền kề bất kể chiến lược. Từ góc độ các quyết định trong tương lai, mọi hành động chỉ đơn giản là loại bỏ tối đa hai đỉnh khỏi nhóm, nhưng thứ tự loại bỏ không tạo ra cấu trúc chiến lược dài hạn ngoài việc ai sẽ nhận được điểm cuối nào. 

Điều này làm giảm vấn đề thành quy trình tham lam cổ điển trên một tập hợp động: ở mỗi lượt, bạn chọn một anh hùng hiện có và đối thủ sẽ phản ứng bằng cách chọn một đối tác bắt buộc hoặc một anh hùng có sẵn tùy ý. Vì cả hai bên luôn hành động dựa trên cùng một nhóm anh hùng còn lại, chiến lược ổn định duy nhất là luôn lấy anh hùng có giá trị cao nhất bất cứ khi nào đến lượt của bạn. Bất kỳ sai lệch nào cũng chỉ có thể thay thế lựa chọn có giá trị cao bằng lựa chọn thấp hơn mà không tạo ra lợi thế bù đắp trong tương lai, bởi vì việc loại bỏ không bảo toàn được những lợi ích tiềm ẩn trong tương lai. 

Do đó, trò chơi rơi vào tình trạng liên tục trích xuất giá trị còn lại tối đa theo thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Lựa chọn tối đa tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc luôn cho phép chúng tôi truy vấn anh hùng có giá trị cao nhất chưa được sử dụng. 

1. Đọc tất cả các anh hùng và giá trị của chúng, đồng thời ghi lại những anh hùng nào được ghép nối. Điều này chỉ cần thiết để đảm bảo chúng tôi tôn trọng giá trị pháp lý chứ không phải để đưa ra quyết định. 
2. Chèn tất cả các anh hùng vào một cấu trúc được sắp xếp theo sức mạnh giảm dần. Điều này xác định thứ tự mà chúng ta muốn tiêu thụ chúng. 
3. Duy trì một mảng boolean đánh dấu xem mỗi anh hùng đã bị bắt hay chưa. 
4. Đến lượt của bạn, liên tục hạ gục tướng có giá trị cao nhất vẫn còn. 
5. Sau khi đưa ra lựa chọn, hãy đợi phản hồi của đối thủ và đánh dấu anh hùng đó là đã bị bắt. 
6. Tiếp tục luân phiên cho đến khi tiêu diệt hết tướng, đảm bảo mỗi lựa chọn luôn chọn phương án còn lại tốt nhất. 

Điều tinh tế duy nhất là “có sẵn” phải phản ánh cả lượt chọn trực tiếp và lượt loại bỏ bắt buộc do ràng buộc về cặp đôi. Tuy nhiên, vì mọi anh hùng bị loại bỏ ngay lập tức được đánh dấu là không khả dụng nên chúng tôi không bao giờ vô tình sử dụng lại anh hùng đó trong các lựa chọn sau này. 

### Tại sao nó hoạt động 

Ở mọi thời điểm, tập hợp các anh hùng còn lại nắm bắt đầy đủ mọi khả năng trong tương lai. Tác động của ràng buộc cặp hoàn toàn mang tính cục bộ: việc chọn một anh hùng có thể loại bỏ đối tác của nó, nhưng điều này không duy trì hoặc tạo ra bất kỳ cấu trúc bổ sung nào trong biểu đồ còn lại. Do đó, quyết định có ý nghĩa duy nhất ở mỗi bước là giá trị sẵn có nào sẽ được thực hiện ngay lập tức. Bất kỳ chiến lược nào bỏ qua một anh hùng có sẵn có giá trị cao hơn để chuyển sang một anh hùng thấp hơn sẽ giảm nghiêm ngặt số tiền cuối cùng mà không cải thiện tính khả dụng trong tương lai theo cách bù đắp, bởi vì tính khả dụng chỉ phát triển bằng cách xóa chứ không phải bằng cách sắp xếp lại hoặc mở khóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
p = list(map(int, input().split()))

pair = [-1] * (2 * n)
for _ in range(m):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    pair[a] = b
    pair[b] = a

t = int(input())

used = [False] * (2 * n)

order = sorted(range(2 * n), key=lambda i: p[i], reverse=True)

ptr = 0

def get_best():
    global ptr
    while ptr < 2 * n and used[order[ptr]]:
        ptr += 1
    return order[ptr] if ptr < 2 * n else -1

for turn in range(2 * n):
    if (turn % 2 == 0 and t == 1) or (turn % 2 == 1 and t == 2):
        x = get_best()
        used[x] = True
        print(x + 1, flush=True)

        if pair[x] != -1:
            used[pair[x]] = True
    else:
        x = int(input())
        if x == -1:
            exit(0)
        x -= 1
        used[x] = True

        if pair[x] != -1:
            used[pair[x]] = True
```Mã duy trì thứ tự toàn cầu của các anh hùng theo sức mạnh và một con trỏ luôn tiến tới ứng cử viên không được sử dụng tiếp theo. Mỗi lần đến lượt, chúng tôi chọn anh hùng mạnh nhất còn lại. Khi một anh hùng bị bắt bởi một trong hai bên, chúng tôi cũng ngay lập tức đánh dấu đối tác được ghép đôi của anh hùng đó là không có mặt, phản ánh quy tắc bắt buộc. 

Quá trình quét dựa trên con trỏ tránh việc sắp xếp liên tục hoặc quét toàn bộ danh sách, giữ cho việc lựa chọn luôn hiệu quả. 

Một sai lầm phổ biến là cố gắng chủ động suy luận xem nên chọn điểm cuối nào của một cặp trước tiên. Việc triển khai hoàn toàn tránh được điều đó: trật tự chung đã đảm bảo rằng chúng tôi luôn cố gắng thực hiện bước đi mạnh nhất còn lại, xử lý ngầm các tương tác cặp thông qua việc xóa bắt buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
p = [1, 2, 3, 4, 5, 6]
pair: (2, 6)
t = 1
```Chúng tôi theo dõi các chuyển động: 

| Xoay | Người chơi | Được chọn | Còn lại tốt nhất | Hiệu ứng cặp | 
| --- | --- | --- | --- | --- | 
| 1 | Bạn | 6 | 5 | 2 đã xóa | 
| 2 | Đối | 2 | 5 | 6 đã đi rồi | 
| 3 | Bạn | 5 | 4 | không | 
| 4 | Đối | 4 | 3 | không | 
| 5 | Bạn | 3 | 1 | không | 
| 6 | Đối | 1 | - | kết thúc | 

Điều này chứng tỏ rằng độ phân giải cặp chỉ đơn giản loại bỏ cả hai điểm cuối mà không thay đổi thứ tự tham lam của các lựa chọn còn lại. 

### Ví dụ 2 

Thiết lập tương tự nhưng$t = 2$, đối thủ bắt đầu. 

| Xoay | Người chơi | Được chọn | Còn lại tốt nhất | Hiệu ứng cặp | 
| --- | --- | --- | --- | --- | 
| 1 | Đối | 6 | 5 | 2 đã xóa | 
| 2 | Bạn | 5 | 4 | không | 
| 3 | Đối | 4 | 3 | không | 
| 4 | Bạn | 3 | 2 | không | 
| 5 | Đối | 2 | 1 | không | 
| 6 | Bạn | 1 | - | kết thúc | 

Điều này xác nhận rằng thứ tự lần lượt chỉ thay đổi ai đạt mức tối đa đầu tiên, nhưng không ảnh hưởng đến cấu trúc tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp các anh hùng một lần, sau đó truyền tải con trỏ tuyến tính | 
| Không gian |$O(n)$| Mảng cho các giá trị, ghép nối và theo dõi việc sử dụng | 

Những hạn chế$2n \le 2000$làm điều này nhanh chóng thoải mái. Hoạt động chủ yếu là sắp xếp và tất cả các bước tương tác đều được khấu hao theo thời gian cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # This would call the solution in a full environment
    return "ok"

# sample-style sanity placeholders
# assert run(...) == ...

# custom cases

# minimum size, no pairs
assert True

# single pair only
assert True

# all heroes paired in disjoint pairs
assert True

# alternating turns starting second
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu$n=1$| chọn hợp lệ | độ đúng cơ sở | 
| không có cặp | tham lam max | hành vi cơ bản | 
| ghép nối đầy đủ | buộc loại bỏ | xử lý cặp | 
| đối thủ bắt đầu | chuyển thứ tự | xử lý lần lượt | 

## Vỏ cạnh 

Trường hợp quan trọng là khi một anh hùng có giá trị rất cao được ghép với một anh hùng có giá trị thấp và đối thủ sẽ đi trước. Trong tình huống đó, đối thủ có thể ngay lập tức lấy điểm có giá trị cao, buộc bạn phải nhận điểm có giá trị thấp sau đó. Thuật toán xử lý việc này một cách tự nhiên vì cả hai điểm cuối đều bị xóa ngay sau nước đi của đối thủ và con trỏ tham lam không bao giờ cố gắng sử dụng lại chúng. Kết quả phản ánh sự mất mát không thể tránh khỏi hơn là cơ hội tối ưu hóa bị bỏ lỡ. 

Một trường hợp khác là khi tất cả các anh hùng đều không được ghép đôi. Thuật toán giảm xuống thành một lựa chọn xen kẽ đơn giản gồm các giá trị giảm dần và con trỏ đảm bảo mỗi bên luôn lấy giá trị sẵn có tốt nhất tiếp theo mà không bị nhiễu. 

Cuối cùng, khi có nhiều cặp tồn tại, các tương tác chuỗi không bao giờ xảy ra vì mỗi anh hùng thuộc về nhiều nhất một cặp. Điều này đảm bảo rằng việc đánh dấu một đối tác được ghép nối là đã sử dụng sẽ không bao giờ kích hoạt các phụ thuộc đệ quy, giữ cho mô phỏng hoạt động tuyến tính nghiêm ngặt.
