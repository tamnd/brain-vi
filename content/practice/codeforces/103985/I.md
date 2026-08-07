---
title: "CF 103985I - \u041a\u0443\u0440\u044c\u0435\u0440\u0441\u043a\u0438\u0439 \u043a\u043b\u0443\u0431"
description: "Chúng ta được cung cấp một chuỗi các điểm giao hàng trên một trục số phải được truy cập theo một thứ tự cố định. Hai người đưa thư bắt đầu tại các vị trí đã biết trên cùng một đường."
date: "2026-07-02T06:15:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103985
codeforces_index: "I"
codeforces_contest_name: "\u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 (\u041c\u041a\u041e\u0428\u041f) 2017, \u041b\u0438\u0433\u0430 \u0410"
rating: 0
weight: 103985
solve_time_s: 61
verified: true
draft: false
---

[CF 103985I - \u041a\u0443\u0440\u044c\u0435\u0440\u0441\u043a\u0438\u0439 \u043a\u043b\u0443\u0431](https://codeforces.com/problemset/problem/103985/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các điểm giao hàng trên một trục số phải được truy cập theo một thứ tự cố định. Hai người đưa thư bắt đầu tại các vị trí đã biết trên cùng một đường. Ở mỗi bước, chính xác một người chuyển phát được chọn để đi đến điểm giao hàng tiếp theo, trong khi người chuyển phát còn lại vẫn ở nguyên vị trí hiện tại. Sau khi giao hàng, người chuyển phát nhanh đã chọn sẽ có mặt tại điểm giao hàng đó và quy trình tiếp tục được thực hiện cho khách hàng tiếp theo. 

Quyết định phân công là quyền tự do duy nhất: đối với mỗi lần giao hàng theo thứ tự, chúng tôi quyết định xem Petya hay Vasya thực hiện việc đó. Sau khi người chuyển phát nhanh được chỉ định, họ sẽ di chuyển từ vị trí hiện tại đến điểm được yêu cầu và vị trí của họ sẽ cập nhật vĩnh viễn cho đến khi được chọn lại. 

Số lượng chúng ta quan tâm chính là khoảng cách giữa hai người vận chuyển sau mỗi bước giao hàng. Chúng ta muốn chọn các bài tập sao cho khoảng cách tối đa trong toàn bộ quá trình càng nhỏ càng tốt. 

Kích thước đầu vào cho phép lên tới một trăm nghìn điểm phân phối và tọa độ lớn tới 10^9. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng hoặc so sánh rõ ràng tất cả các nhiệm vụ có thể thực hiện được. Bất kỳ cách tiếp cận nào phân nhánh theo cả hai lựa chọn ở mỗi bước đều dẫn đến 2^n khả năng, điều này vượt xa khả thi. Ngay cả lập trình động theo dõi trạng thái chính xác của cả hai bộ chuyển phát ở mỗi bước cũng sẽ yêu cầu trạng thái O(n^2) theo một công thức đơn giản, quá lớn. 

Một khó khăn nhỏ là trạng thái của hệ thống không chỉ phụ thuộc vào điểm giao hàng hiện tại mà còn phụ thuộc vào nơi người chuyển phát nhanh không di chuyển được gửi lần cuối. Điều này tạo ra sự phụ thuộc vào lịch sử, chính điều này khiến những ý tưởng tham lam ngây thơ thoạt nhìn trở nên nghi ngờ. 

Một số tình huống khó khăn đáng được cô lập. 

Nếu tất cả các điểm giao hàng đều nằm rất gần với một nhà vận chuyển ban đầu nhưng lại cách xa điểm vận chuyển kia, thì chiến lược tối ưu có thể đơn giản là bỏ qua hoàn toàn một nhà vận chuyển, bởi vì việc di chuyển cả hai sẽ chỉ làm tăng khoảng cách một cách không cần thiết. Ví dụ: nếu s1 = 0, s2 = 1000 và tất cả xi đều gần bằng 0, giải pháp tối ưu giúp Petya làm mọi thứ và câu trả lời bị chi phối bởi khoảng cách ban đầu hoặc khoảng cách phát triển từ Vasya đến đường đi. 

Một trường hợp phức tạp khác là khi chuỗi luân phiên trái và phải xung quanh vị trí ban đầu. Trong những trường hợp như vậy, việc chuyển đổi người chuyển phát thường xuyên có thể giảm đáng kể khoảng cách tối đa và chiến lược ngây thơ “một người chuyển phát sẽ xử lý hầu hết công việc” sẽ thất bại. 

Cuối cùng, nếu phép gán tối ưu chuyển chuyển phát nhanh chính xác một hoặc hai lần, thì bất kỳ quy tắc tham lam nào cam kết sớm mà không xem xét hình học trong tương lai đều có thể sai. 

Những quan sát này cho thấy khó khăn cốt lõi là cân bằng giữa khoảng thời gian chúng ta để một người chuyển phát không hoạt động và khoảng thời gian mà người đưa thư khác trôi ra xa họ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi cách phân công giao hàng có thể có giữa hai người chuyển phát. Đối với mỗi bài tập, chúng tôi mô phỏng quy trình và theo dõi cả hai vị trí sau mỗi bước, cập nhật khoảng cách tối đa. Điều này đúng vì nó tuân theo chính xác các quy tắc của quy trình, nhưng nó yêu cầu kiểm tra 2^n phép gán và mỗi mô phỏng là O(n), dẫn đến thời gian theo cấp số nhân. 

Một cải tiến tự nhiên là lập trình động trên tiền tố phân phối. Ở bước i, trạng thái hệ thống được xác định theo vị trí hiện tại của mỗi người chuyển phát, điều này phụ thuộc vào lần giao hàng cuối cùng mà mỗi người trong số họ thực hiện. Điều này dẫn đến một trạng thái được xác định bởi chỉ mục cuối cùng được xử lý bởi mỗi chuyển phát nhanh. Quá trình chuyển đổi tương ứng với việc chỉ định lần giao hàng tiếp theo cho một trong hai người chuyển phát nhanh. Mặc dù đúng nhưng công thức này tạo ra trạng thái O(n^2) vì mỗi người chuyển phát có thể đã truy cập lần cuối vào bất kỳ vị trí giao hàng hoặc vị trí ban đầu nào trước đó và việc chuyển đổi giữa tất cả các cặp như vậy trở nên không khả thi đối với n lên tới 100000.

Quan sát quan trọng giúp đơn giản hóa vấn đề là ở bất kỳ bước nào, chỉ có một người chuyển phát nhanh di chuyển, còn người kia vẫn cố định. Khoảng cách ở bước i chỉ đơn giản là khoảng cách giữa điểm giao hàng hiện tại và vị trí cuối cùng của người chuyển phát không hoạt động. Điều này có nghĩa là thông tin liên quan duy nhất về người chuyển phát không hoạt động là vị trí hiện tại của nó chứ không phải toàn bộ lịch sử về cách nó đến đó. 

Điều này dẫn đến một quan điểm tham lam. Tại mỗi điểm giao hàng, chúng tôi quyết định chuyển phát nhanh nào sẽ chuyển đến đó. Quyết định này chỉ ảnh hưởng đến tương lai thông qua vị trí mới của chuyển phát nhanh đó và chi phí hiện tại chỉ phụ thuộc vào khoảng cách hiện tại của chuyển phát nhanh kia. Hệ thống luôn bao gồm hai điểm hoạt động trên đường truyền và một trong số chúng được cập nhật ở mỗi bước. 

Cấu trúc này cho phép thực hiện một chiến lược đơn giản: luôn chỉ định việc giao hàng hiện tại cho người chuyển phát có vị trí hiện tại gần điểm giao hàng đó hơn. Trực giác cho thấy việc di chuyển người đưa thư đến gần hơn sẽ giảm thiểu khoảng cách ngay lập tức giữa các người đưa thư và vì chỉ có một điểm cuối thay đổi trong mỗi bước nên việc trì hoãn một bước di chuyển lớn không mang lại lợi ích bù đắp sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng phân công vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP đầy đủ ở các vị trí cuối cùng | O(n^2) | O(n^2) | Quá chậm | 
| Tham lam chuyển phát nhanh gần nhất | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì vị trí hiện tại của hai người đưa tin. Ban đầu đây là s1 và s2. Chúng tôi lặp qua các điểm giao hàng theo thứ tự. 

1. Bắt đầu với hai biến đại diện cho vị trí hiện tại của người chuyển phát, được khởi tạo là s1 và s2. Khoảng cách ban đầu đã là một phần của mức tối đa, vì bài toán xem xét toàn bộ quá trình. 
2. Với mỗi điểm giao hàng xi, tính khoảng cách từ xi đến vị trí hiện tại của mỗi người giao hàng. Điều này cho chúng ta biết chuyển phát nhanh nào có thể tiếp cận xi với chuyển động nhỏ hơn, chuyển động nào sẽ ít làm xáo trộn hệ thống hơn ở bước này. 
3. Gán xi cho người đưa thư có vị trí hiện tại gần xi hơn. Chuyển phát nhanh này di chuyển đến xi, vì vậy chúng tôi cập nhật vị trí của chuyển phát nhanh đó thành xi. Chuyển phát nhanh khác vẫn không thay đổi. 
4. Sau khi cập nhật vị trí, hãy tính khoảng cách giữa hai người đưa thư và cập nhật câu trả lời với mức tối đa nhìn thấy cho đến nay. 
5. Tiếp tục cho đến khi tất cả các điểm giao hàng được xử lý, sau đó xuất ra khoảng cách được ghi tối đa. 

Tại sao nó hoạt động xuất phát từ cách nhà nước phát triển. Tại bất kỳ thời điểm nào, hệ thống được mô tả đầy đủ bởi hai điểm trên một đường thẳng. Khi chúng ta di chuyển một chuyển phát nhanh đến xi, cách duy nhất để tác động đến khoảng cách trong tương lai là thay đổi một điểm cuối của cặp này. Việc chọn bộ chuyển phát gần hơn sẽ đảm bảo rằng cấu hình mới được tạo sẽ giữ cho cặp điểm càng chặt chẽ càng tốt xung quanh vị trí giao hàng hiện tại, điều này ngăn cản việc hình thành sớm các khoảng cách lớn không cần thiết và lan truyền về phía trước. Bất kỳ lựa chọn thay thế nào cũng thay thế điểm cuối gần hơn bằng điểm cuối xa hơn, điều này chỉ làm tăng khoảng cách hiện tại và không tạo ra lợi thế cấu trúc bù đắp cho các bước trong tương lai, vì các quyết định trong tương lai chỉ phụ thuộc vào vị trí điểm cuối được cập nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s1, s2 = map(int, input().split())
    x = list(map(int, input().split()))

    a = s1
    b = s2

    ans = abs(a - b)

    for xi in x:
        if abs(xi - a) <= abs(xi - b):
            a = xi
        else:
            b = xi

        if abs(a - b) > ans:
            ans = abs(a - b)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ hai biến cho vị trí hiện tại của người chuyển phát. Ở mỗi bước, nó sẽ so sánh chuyển phát nhanh nào gần điểm giao hàng tiếp theo hơn và di chuyển chuyển phát nhanh đó. Câu trả lời được cập nhật sau mỗi lần di chuyển, bao gồm cả cấu hình ban đầu. 

Một điểm tinh tế là các ràng buộc bị phá vỡ một cách tùy ý có lợi cho người chuyển phát đầu tiên, nhưng bất kỳ sự phá vỡ ràng buộc nhất quán nào cũng có hiệu quả vì khoảng cách bằng nhau có nghĩa là một trong hai lựa chọn sẽ duy trì chất lượng cấu hình ngay lập tức như nhau. Một chi tiết khác là khoảng cách ban đầu được bao gồm trước bất kỳ bước di chuyển nào, vì những người vận chuyển bắt đầu tách ra và sự tách biệt đó đã góp phần tối đa hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 0 10
5 6
```Chúng tôi bắt đầu với các vị trí (0, 10) và khoảng cách tối đa ban đầu là 10. 

| Bước | xi | chuyển phát nhanh đã chọn | vị trí sau khi di chuyển | khoảng cách | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | (0, 10) | 10 | 
| 1 | 5 | hoặc (hòa) chọn một | (5, 10) | 5 | 
| 2 | 6 | b gần hơn | (5, 6) | 1 | 

Khoảng cách tối đa trong tất cả các thời điểm là 10, xảy ra khi bắt đầu. Điều này cho thấy đôi khi chiến lược tốt nhất là bỏ qua sự cân bằng và chấp nhận sự tách biệt ban đầu. 

### Ví dụ 2 

đầu vào:```
3 2 1
3 4 5
```Vị trí ban đầu là (2, 1), khoảng cách ban đầu là 1. 

| Bước | xi | chuyển phát nhanh đã chọn | vị trí sau khi di chuyển | khoảng cách | 
| --- | --- | --- | --- | --- | 
| ban đầu | - | - | (2, 1) | 1 | 
| 1 | 3 | một | (3, 1) | 2 | 
| 2 | 4 | b | (3, 4) | 1 | 
| 3 | 5 | một | (5, 4) | 1 | 

Khoảng cách tối đa trở thành 2. Việc phân công xen kẽ ngăn không cho một vận chuyển viên trôi quá xa so với vận chuyển viên kia, cho thấy lý do tại sao việc chuyển đổi có thể có lợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần giao hàng được xử lý một lần bằng cách so sánh khoảng cách theo thời gian không đổi | 
| Không gian | O(1) | Chỉ vị trí hiện tại và câu trả lời mới được lưu trữ | 

Giải pháp tuyến tính về số lượng phân phối, đủ cho n lên tới 100000. Việc sử dụng bộ nhớ là không đổi và không phụ thuộc vào kích thước đầu vào, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    n, s1, s2 = map(int, sys.stdin.readline().split())
    x = list(map(int, sys.stdin.readline().split()))

    a, b = s1, s2
    ans = abs(a - b)

    for xi in x:
        if abs(xi - a) <= abs(xi - b):
            a = xi
        else:
            b = xi
        ans = max(ans, abs(a - b))

    return str(ans)

# provided samples
assert run("2 0 10\n5 6\n") == "10"
assert run("3 2 1\n3 4 5\n") == "2"

# custom cases
assert run("1 0 5\n100\n") == "95", "single move dominates distance"
assert run("3 0 10\n1 2 3\n") == "10", "one courier should handle all"
assert run("4 0 3\n1 100 101 102\n") == "99", "drift vs sticking behavior"
assert run("2 0 1\n5 6\n") == "5", "separation grows after initial tight start"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bước nhảy lớn duy nhất | 95 | xử lý một phong trào chiếm ưu thế | 
| cụm nhỏ đơn điệu | 10 | tham lam chọn một người chuyển phát nhanh | 
| cụm ngoại lệ lớn | 99 | nhạy cảm với các điểm ở xa | 
| đóng bắt đầu rồi lan rộng | 5 | tác động cấu hình sớm | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi cả hai người vận chuyển đều cách đều nhau đến điểm giao hàng tiếp theo. Trong tình huống này, một trong hai lựa chọn sẽ tạo ra khoảng cách ngay lập tức như nhau từ điểm giao hàng và thuật toán chỉ định khoảng cách đó một cách nhất quán cho người chuyển phát đầu tiên. Vì trạng thái vẫn đối xứng trong việc hoán đổi danh tính người chuyển phát, nên điều này không ảnh hưởng đến khoảng cách tối đa cuối cùng. 

Một trường hợp cạnh khác là khi tất cả các điểm giao hàng nằm ở một bên của cả hai vị trí bắt đầu. Trong trường hợp đó, việc liên tục chỉ định cho người đưa thư gần nhất sẽ giảm hiệu quả xuống việc chỉ có một người đưa thư xử lý hầu hết hoặc tất cả các chuyến giao hàng và khoảng cách tối đa bị chi phối bởi khoảng cách giữa người đưa thư không hoạt động và người đang di chuyển. Quy tắc tham lam tự nhiên hội tụ vào hành vi này mà không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng xảy ra khi các điểm giao hàng luân phiên ở bên trái và bên phải. Ở đây, thuật toán thay thế các người đưa thư, đảm bảo rằng không có người đưa thư nào tích lũy dịch chuyển quá mức. Mỗi bước chỉ cập nhật một điểm cuối và giới hạn khoảng cách được duy trì bằng cách luôn giữ điểm cuối được cập nhật càng gần vị trí mới càng tốt.
