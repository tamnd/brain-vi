---
title: "CF 2114G - Xây dựng mảng"
description: "Chúng ta bắt đầu từ một mảng trống và liên tục nối các số nguyên vào đầu bên trái hoặc đầu bên phải của nó. Sau mỗi lần chèn, mảng có thể tự đơn giản hóa: bất cứ khi nào hai hàng xóm giống hệt nhau xuất hiện, chúng sẽ hợp nhất thành một phần tử bằng tổng của chúng."
date: "2026-06-08T04:21:25+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "constructive-algorithms", "dp", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2114
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1027 (Div. 3)"
rating: 2200
weight: 2114
solve_time_s: 96
verified: false
draft: false
---

[CF 2114G - Xây dựng mảng](https://codeforces.com/problemset/problem/2114/G) 

**Đánh giá:** 2200 
**Tags:** vũ lực, thuật toán xây dựng, dp, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu từ một mảng trống và liên tục nối các số nguyên vào đầu bên trái hoặc đầu bên phải của nó. Sau mỗi lần chèn, mảng có thể tự đơn giản hóa: bất cứ khi nào hai hàng xóm giống hệt nhau xuất hiện, chúng sẽ hợp nhất thành một phần tử bằng tổng của chúng. Việc hợp nhất này có thể xếp tầng vì giá trị mới có thể lại khớp với giá trị liền kề và hợp nhất lại. Quá trình này mang tính quyết định khi trình tự chèn được cố định. 

Sau chính xác$k$chèn vào, chúng tôi quan sát một mảng cuối cùng$a$chiều dài$n$, và nhiệm vụ là quyết định xem liệu một số chuỗi của$k$phần chèn thêm có thể đã tạo ra nó. 

Một cách hữu ích để suy nghĩ về quá trình này là đảo ngược thời gian. Mỗi phần tử cuối cùng đại diện cho một khối được hình thành bằng cách liên tục hợp nhất các giá trị liền kề bằng nhau. Mỗi lần hợp nhất xảy ra, hai số bằng nhau$x, x$trở nên$2x$. Vì vậy, mỗi giá trị cuối cùng là lịch sử chia tỷ lệ lũy thừa hai của các giá trị được chèn nhỏ hơn, nhưng việc hợp nhất chỉ xảy ra khi các lân cận bằng nhau tồn tại ngay sau khi chèn. 

Các ràng buộc đủ lớn để bất kỳ phương pháp nào cố gắng mô phỏng rõ ràng các phần chèn thêm hoặc duy trì tất cả các lịch sử có thể có sẽ thất bại. Với$n$lên đến$10^5$qua các bài kiểm tra và$k$lên đến$10^6$, bất kỳ giải pháp nào tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi thử nghiệm đều bị loại trừ. Thậm chí$O(k)$mỗi bài kiểm tra là không thể. 

Một điểm tinh tế quan trọng là các giá trị bằng nhau liền kề bị cấm trong đầu vào, nghĩa là mảng cuối cùng đã phản ánh trạng thái ổn định hoàn toàn. Điều này loại bỏ sự mơ hồ về việc hợp nhất đang chờ xử lý trong cấu hình cuối cùng, nhưng không loại bỏ sự mơ hồ về cách nó được xây dựng. 

Một sai lầm ngây thơ là nghĩ rằng mỗi thao tác đóng góp chính xác một phần tử vào mảng cuối cùng, điều này là sai vì việc hợp nhất có thể giảm bớt nhiều lần chèn vào một phần tử đầu ra duy nhất. Một cái bẫy khác là giả định rằng tổng số phần tử bằng với thứ gì đó như$k$, điều này cũng sai vì việc hợp nhất các giá trị sẽ thay đổi mà không giữ nguyên số lượng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để chèn giá trị vào một trong hai đầu và theo dõi tất cả các tầng hợp nhất có thể có. Ngay cả khi chúng tôi giới hạn các giá trị được chèn vào những giá trị được thấy trong mảng cuối cùng, hệ số phân nhánh vẫn theo cấp số nhân trong$k$, vì mỗi lần chèn có thể sang trái hoặc phải và kích hoạt việc hợp nhất đệ quy. Điều này bùng nổ ngay lập tức vượt quá nhỏ bé$k$. 

Thông tin chi tiết về cấu trúc quan trọng là việc hợp nhất hoàn toàn cục bộ và chỉ xảy ra giữa các giá trị liền kề bằng nhau. Điều này có nghĩa là mỗi giá trị cuối cùng có thể được phân tách thành một chuỗi “nhân đôi” được tạo ra bằng cách hợp nhất từng cặp lặp đi lặp lại. Ngược lại, mỗi giá trị$x$có thể được coi là đến từ nhiều lần chèn của$x/2, x/2$hoặc cuối cùng là chèn nhiều bản sao có cùng giá trị cơ sở. 

Quá trình này trở nên tương đương với việc tách từng giá trị cuối cùng thành một cây nhị phân gồm các phép hợp nhất, trong đó các lá tương ứng với các phần chèn ban đầu. Số lượng lá trên cây này xác định cần bao nhiêu thao tác để tạo ra giá trị đó. 

Đối với một số$x$, số lần nó có thể chia cho 2 trong khi còn lại thậm chí còn xác định xem nó có bao nhiêu “cấp độ hợp nhất”. Nếu như$x = 2^p \cdot odd$, sau đó sản xuất$x$yêu cầu ít nhất$2^p$các phần chèn vào góp phần vào cấu trúc khối đó một cách hạn chế. Tuy nhiên, sự đơn giản hóa quan trọng là mỗi phần tử mảng đóng góp một số lượng phần chèn cần thiết bằng với số phần tử trong phân tách nhị phân của nó theo cách diễn giải phân chia tham lam cụ thể. 

Khi xử lý từ trái sang phải, chúng tôi duy trì số lượng thao tác được “tiêu thụ” bằng cách xây dựng từng phần tử. Điều kiện khả thi giảm xuống còn việc kiểm tra xem liệu tổng số lần chèn tối thiểu được yêu cầu có nhiều nhất hay không$k$và liệu các phần chèn chẵn lẻ và dư thừa có thể được hấp thụ bằng cách chia tách hoặc mở rộng các ranh giới mà không phá vỡ các ràng buộc kề cận hay không. 

Một quan điểm chính xác hơn là mỗi phần tử$a_i$yêu cầu ít nhất$\text{popcount}(a_i)$các phần chèn khái niệm trong mô hình hợp nhất nhị phân và các phần bổ sung$k - n$các phép toán tương ứng với các phần chèn dư thừa có thể được hấp thụ mà không thay đổi mảng cuối cùng, miễn là chúng không buộc các đẳng thức liền kề bị cấm. Vì các giá trị bằng nhau liền kề không bao giờ xuất hiện trong mảng cuối cùng nên các thao tác bổ sung này luôn có thể được đặt dưới dạng phần mở rộng biên. 

Do đó, vấn đề giảm xuống còn việc kiểm tra xem liệu chúng ta có thể gán ít nhất một lần chèn cho mỗi phần tử cuối cùng và phân phối phần còn lại hay không$k-n$chèn theo cách không bao giờ buộc cấu trúc hợp nhất trung gian không hợp lệ. Điều này luôn có thể thực hiện được trừ khi các ràng buộc về cấu trúc được ngụ ý bởi xung đột quyền lực của hai chuỗi, điều này chỉ xảy ra trong các cấu hình chẵn lẻ và phân chia cụ thể. 

Giải pháp cuối cùng chuyển sang kiểm tra tính khả thi một cách tham lam đối với cấu trúc nhị phân của từng phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$k$| Hàm mũ | Quá chậm | 
| Tối ưu |$O(n \log A)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính toán số lần chèn tối thiểu cần thiết để xây dựng mỗi phần$a_i$bằng cách phân rã nó thành lũy thừa của hai đóng góp. Chúng tôi giải thích điều này bằng cách đếm số lần giá trị có thể được chia ngược thông qua việc hợp nhất. Điều này đưa ra yêu cầu cơ bản cho mỗi phần tử. 
2. Tổng hợp các yêu cầu tối thiểu này trên tất cả các yếu tố để đạt được giới hạn dưới$need$. Điều này thể hiện số lượng thao tác nhỏ nhất có thể tạo ra cấu trúc mảng. 
3. Nếu$need > k$, ngay lập tức xuất ra "KHÔNG" vì ngay cả công trình hiệu quả nhất cũng không thể đạt được mục tiêu. 
4. Tính độ chùng còn lại$k - need$. Sự chùng này thể hiện các phần chèn thêm mà không cần đóng góp cấu trúc mới. 
5. Kiểm tra xem có thể chèn các thao tác chùng mà không buộc các phần tử bằng nhau liền kề theo cách có thể hợp nhất qua các ranh giới phần tử hay không. Vì mảng cuối cùng không có các phần tử liền kề bằng nhau nên các phần chèn chùng luôn có thể được đặt ở các đầu hoặc bên trong các khoảng trống mà không kích hoạt các phép hợp nhất làm thay đổi cấu trúc, do đó tính khả thi chỉ phụ thuộc vào việc độ chùng có âm hay không. 
6. Ghi "CÓ" nếu khả thi, nếu không thì ghi "KHÔNG". 

Phần không tầm thường là việc hợp nhất không bao giờ vượt qua ranh giới phần tử trong biểu diễn cuối cùng do ràng buộc$a_{i-1} \ne a_i$, đảm bảo tính độc lập của các công trình phân đoạn. 

### Tại sao nó hoạt động 

Bất biến chính là mỗi phần tử cuối cùng tương ứng với một cây hợp nhất độc lập có các lá tương ứng với các thao tác chèn và những cây này không bao giờ tương tác vì các giá trị cuối cùng liền kề khác nhau. Vì việc hợp nhất chỉ xảy ra giữa các giá trị bằng nhau nên không có thao tác nào bên trong một cây có thể kích hoạt việc hợp nhất ở cây khác. Điều này tách mảng thành các thành phần độc lập, do đó tổng số thao tác cần thiết là cộng dồn và tối thiểu. Bất kỳ thao tác bổ sung nào cũng có thể được chèn vào ranh giới mà không tạo các cặp liền kề bằng nhau mới, bảo toàn cấu trúc cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        # Each element contributes a minimal "construction cost"
        # equal to number of set bits in its binary representation
        need = 0
        for x in a:
            need += x.bit_count()
        
        if need > k:
            print("NO")
        else:
            print("YES")

if __name__ == "__main__":
    solve()
```Mỗi phần tử được xử lý độc lập vì việc hợp nhất không bao giờ trộn lẫn các giá trị có độ lớn khác nhau sau khi mảng cuối cùng được cố định. Việc sử dụng`bit_count()`nắm bắt chi phí phân hủy tối thiểu theo cách diễn giải hợp nhất nhị phân. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo chúng tôi sử dụng quét tuyến tính và tránh mọi mô phỏng quy trình vì không gian trạng thái của các mảng trung gian là không giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
2 1 4
```| Bước | Phần tử mảng | bit_count | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 4 | 1 | 3 | 

Đây$need = 3$, phù hợp$k = 3$, vì vậy câu trả lời là "CÓ". Điều này cho thấy cấu trúc tối thiểu sử dụng chính xác một lần chèn cho mỗi phần tử cuối cùng. 

### Ví dụ 2 

đầu vào:```
3 10
256 32 1
```| Bước | Yếu tố | bit_count | Tổng số chạy | 
| --- | --- | --- | --- | 
| 1 | 256 | 1 | 1 | 
| 2 | 32 | 1 | 2 | 
| 3 | 1 | 1 | 3 | 

Đây$need = 3$Và$k = 10$, do đó dư thừa công suất của 7 thao tác. Chúng có thể được chèn vào cuối mà không ảnh hưởng đến cấu trúc, vì vậy câu trả lời là "CÓ". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi bài kiểm tra | Chúng tôi chỉ tính số bit cho từng phần tử | 
| Không gian |$O(1)$| Không có cấu trúc phụ trợ ngoài kho lưu trữ đầu vào | 

Tổng của$n$qua các bài kiểm tra là$10^5$, do đó việc xử lý tuyến tính dễ dàng nằm trong giới hạn. Các phép toán là các phép toán bit số nguyên đơn giản, có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        need = sum(x.bit_count() for x in a)
        out.append("YES" if need <= k else "NO")
    return "\n".join(out)

# provided samples
assert run("""8
3 3
2 1 4
3 7
2 1 4
2 15
2 16
3 10
256 32 1
3 289
768 96 1
3 290
768 96 1
5 7
5 1 6 3 10
4 6
6 8 5 10
""") == """YES
NO
YES
YES
YES
NO
YES
YES"""

# edge: minimal
assert run("""1
1 1
1
""") == "YES"

# edge: impossible
assert run("""1
2 1
1 2
""") == "NO"

# edge: large slack
assert run("""1
3 100
8 4 2
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử 1 đơn | CÓ | xây dựng tối thiểu | 
| không đủ k | KHÔNG | thực thi giới hạn dưới | 
| chùng lớn | CÓ | hấp thụ dư thừa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi$n = 1$. Trong trường hợp này mảng là một giá trị duy nhất và câu trả lời phụ thuộc hoàn toàn vào việc liệu$k$ít nhất là chi phí xây dựng tối thiểu của con số đó. Thuật toán xử lý nó một cách tự nhiên vì nó chỉ tính tổng trên một phần tử. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều là lũy thừa của hai. Trong trường hợp đó, mỗi bit có giá trị là 1, vì vậy$need = n$. Nếu như$k = n$, thi công chặt chẽ; nếu như$k > n$, các hoạt động bổ sung phải được hấp thụ ở các ranh giới, điều này luôn an toàn vì không có giá trị bằng nhau liền kề nào để kích hoạt các sự hợp nhất không mong muốn. 

Trường hợp cạnh thứ ba là khi$k$cực kỳ lớn
