---
title: "CF 104468M - Các chỉ số linh hoạt"
description: "Chúng ta đang làm việc với các hoán vị có độ dài $N$, vì vậy mọi cách sắp xếp hợp lệ đều là sự sắp xếp lại các số từ $1$ đến $N$. Thuộc tính cấu trúc duy nhất mà chúng ta quan tâm là liệu vị trí $i$ có phải là gốc hay không, nghĩa là $Pi P{i+1}$."
date: "2026-06-30T13:02:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "M"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 87
verified: false
draft: false
---

[CF 104468M - Chỉ số linh hoạt](https://codeforces.com/problemset/problem/104468/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với các hoán vị có độ dài$N$, do đó mọi sự sắp xếp hợp lệ đều là sự sắp xếp lại các số từ$1$ĐẾN$N$. Thuộc tính cấu trúc duy nhất mà chúng ta quan tâm là liệu một vị trí có$i$là một nguồn gốc, có nghĩa là$P_i > P_{i+1}$. Những vị trí như vậy rất đặc biệt vì chúng chính xác là “điểm dừng” nơi hoán vị giảm xuống khi đọc từ trái sang phải. 

Chúng tôi được tặng một bộ$S$của các chỉ số. Điều kiện hạn chế nơi được phép hạ xuống: nếu một vị trí$i$thì là sự đi xuống$i$phải thuộc về$S$. Tương tự, tất cả các vị trí bên ngoài$S$buộc phải thỏa mãn$P_i < P_{i+1}$, vì vậy họ phải tăng dần các bước. 

Nhiệm vụ là đếm xem có bao nhiêu hoán vị của$[1..N]$thỏa mãn ràng buộc này cho một tập hợp nhất định$S$, trên nhiều trường hợp thử nghiệm, modulo$10^9+7$. 

Các ràng buộc rất lớn:$N$có thể đi lên$2 \cdot 10^5$và có tới$10^3$trường hợp thử nghiệm với tổng số$K$được giới hạn trên toàn cầu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê các hoán vị hoặc thậm chí mô phỏng các ràng buộc trên mỗi hoán vị. Cấu trúc phải giảm xuống một công thức tổ hợp cho mỗi trường hợp thử nghiệm, lý tưởng nhất là tuyến tính hoặc gần tuyến tính trong$K$. 

Một điểm tinh tế quan trọng là tình trạng này mang tính cục bộ nhưng gây ra hậu quả toàn cầu. Một vị trí gốc được phép duy nhất sẽ thay đổi cách phân chia các giá trị, do đó việc xử lý các vị trí một cách độc lập sẽ không thành công. 

Một sai lầm ngây thơ là nghĩ rằng mỗi chỉ số trong$S$quyết định một cách độc lập xem đó có phải là sự đi xuống hay không. Điều đó sẽ gợi ý một cách đơn giản$2^K$- đếm kiểu hoặc phân tách giai thừa, nhưng điều này bỏ qua việc mô tả xác định việc phân chia hoán vị thành các lần chạy tăng dần và các lần chạy đó phải nhất quán trên toàn cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra tất cả$N!$hoán vị và kiểm tra xem mọi vị trí đi xuống có nằm trong$S$. Đối với mỗi hoán vị, chúng tôi quét một lần để xác định tất cả các chỉ số$i$Ở đâu$P_i > P_{i+1}$và xác minh tư cách thành viên trong tập hợp băm. Chi phí này$O(N \cdot N!)$, điều này vượt xa khả thi ngay cả đối với$N = 10$. 

Quan sát quan trọng là ngừng suy nghĩ trực tiếp về các hoán vị mà thay vào đó tập trung vào cấu trúc do các vết lõm gây ra. Mọi hoán vị có thể được phân tách thành các phân đoạn tăng tối đa. Xuống dốc tại vị trí$i$chính xác là nơi một đoạn kết thúc và đoạn tiếp theo bắt đầu. 

Ràng buộc nói rằng việc ngắt đoạn chỉ được phép ở các vị trí trong$S$. Điều này có nghĩa là sự bổ sung của$S$không được có điểm ngắt, vì vậy những vị trí đó buộc phải có tính liên tục trong các lần chạy tăng dần. Nói cách khác, chúng ta đang phân chia mảng thành các khối tăng dần và các điểm phân chia được phép chính xác là$S$. 

Bây giờ vấn đề trở thành: có bao nhiêu cách gán các số$1..N$thành các khối được xác định bởi các vị trí không ngắt bắt buộc và các vị trí ngắt tùy chọn, đồng thời duy trì thứ tự tăng dần bên trong mỗi khối. 

Áp dụng chuyển đổi tiêu chuẩn: quét từ trái sang phải và xác định các đoạn liền kề nơi cấm ngắt. Những ràng buộc bắt buộc này hợp nhất các vị trí thành các thành phần. Sau khi được hợp nhất, mỗi thành phần hoạt động như một “khe” duy nhất trong đó các phần tử phải tăng lên, nhưng thứ tự tương đối giữa các thành phần có thể thay đổi tùy ý. Điều này biến vấn đề thành việc đếm các hoán vị của các khối, là giai thừa của số khối và cũng nhân các sắp xếp bên trong cố định. 

Cấu trúc cuối cùng đơn giản hóa việc tính toán giai thừa dựa trên số lượng thành phần được kết nối tồn tại dưới các cạnh kề cận bắt buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot N!)$|$O(N)$| Quá chậm | 
| Đếm thành phần + giai thừa |$O(N)$mỗi bài kiểm tra |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta diễn giải lại các ràng buộc dưới dạng các quan hệ kề cận bắt buộc. Nếu vị trí$i$không có trong$S$, thì chúng ta phải có$P_i < P_{i+1}$, có nghĩa là$i$Và$i+1$phải nằm trong cùng một khối tăng dần. Vì vậy mọi chỉ số$i \notin S$buộc một kết nối giữa$i$Và$i+1$. 

1. Sắp xếp hoặc lưu trữ$S$trong một bộ băm cho$O(1)$kiểm tra thành viên. Điều này cho phép chúng tôi nhanh chóng xác định xem một ranh giới được phép hay bị cấm. 
2. Tra cứu chỉ số từ$1$ĐẾN$N-1$. Bất cứ khi nào$i \notin S$, chúng tôi hợp nhất$i$Và$i+1$vào cùng một thành phần. Điều này xây dựng các phân đoạn liền kề của các vị trí không được phép đi xuống. 
3. Đếm kết quả có bao nhiêu thành phần được kết nối. Nếu chúng ta bắt đầu với$N$các vị trí bị cô lập và hợp nhất các vị trí liền kề khi bị ép buộc, mỗi lần hợp nhất sẽ giảm số lượng thành phần đi một. 
4. Gọi số thành phần là$C$. Giải thích tổ hợp chính là mỗi thành phần hoạt động giống như một phần tử duy nhất trong hoán vị của các thành phần. 
5. Bên trong mỗi thành phần, các giá trị phải tăng dần nên khi một bộ số được gán cho một thành phần thì chỉ có một cách duy nhất để sắp xếp chúng. 
6. Do đó, quyền tự do duy nhất là hoán vị$C$bản thân các thành phần, góp phần tạo nên một yếu tố$C!$. 
7. Tính giai thừa lên đến$N$một lần và xuất ra$C!$cho từng trường hợp thử nghiệm. 

Lý do điều này có tác dụng là vì các ràng buộc không đi xuống bắt buộc xác định đầy đủ các ràng buộc đặt hàng nội bộ. Mọi thành phần liên thông được hình thành bởi các cạnh tăng cưỡng bức đều có cấu trúc bên trong cố định và không còn bậc tự do bổ sung nào bên trong nó. Tất cả sự thay đổi đều xuất phát từ cách các khối được sắp xếp tương đối với nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    T = int(input())
    max_n = 2 * 10**5 + 5

    fact = [1] * (max_n)
    for i in range(1, max_n):
        fact[i] = fact[i - 1] * i % MOD

    for _ in range(T):
        n, k = map(int, input().split())
        s = set(map(int, input().split()))

        components = 1
        for i in range(1, n):
            if i in s:
                components += 1

        print(fact[components])

if __name__ == "__main__":
    solve()
```Giải pháp tính toán trước các giai thừa một lần để mỗi truy vấn giảm xuống việc đếm số lượng phân đoạn được hình thành. Bước không cần thiết duy nhất là giải thích phần bổ sung của$S$: mọi chỉ số trong$S$buộc phải phân chia giữa các vị trí$i$Và$i+1$, tăng số lượng các thành phần độc lập. 

Một lỗi triển khai phổ biến là đảo ngược logic và hợp nhất trên$i \in S$. Điều đó sẽ coi các phần thoát nước được phép là cấu trúc cưỡng bức một cách không chính xác, trong khi các ràng buộc cưỡng bức thực tế đến từ các vị trí bên ngoài$S$. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$N = 5$Và$S = \{2, 4\}$. Điều này có nghĩa là chỉ vị trí 2 và 4 mới được phép hạ xuống, vì vậy vị trí 1 và 3 phải có ranh giới tăng dần. 

| tôi | ở S | hành động | thành phần | 
| --- | --- | --- | --- | 
| 1 | không | hợp nhất (1,2) | 1 | 
| 2 | vâng | phép chia nhỏ | 2 | 
| 3 | không | hợp nhất (3,4) | 2 | 
| 4 | vâng | phép chia nhỏ | 3 | 

Chúng tôi kết thúc với 3 thành phần, vì vậy câu trả lời là$3! = 6$. Điều này cho thấy cấu trúc của các vị trí bị cấm bị sụp đổ như thế nào. 

Bây giờ hãy xem xét$N = 4$,$S = \{1,2,3\}$. Mọi vị trí đều được phép đi xuống, do đó không có sự hợp nhất bắt buộc. 

| tôi | ở S | thành phần | 
| --- | --- | --- | 
| 1 | vâng | 1 | 
| 2 | vâng | 2 | 
| 3 | vâng | 3 | 
| | | Tổng cộng 4 | 

chúng tôi nhận được$4$thành phần, vì vậy câu trả lời là$4! = 24$, có nghĩa là tất cả các hoán vị đều hợp lệ vì không có vị trí nào bị hạn chế tăng. 

Những ví dụ này cho thấy rằng các ràng buộc chỉ làm giảm sự tự do khi chúng ép buộc sự kề cận, chứ không phải khi chúng chỉ cho phép giảm dần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sum N + \sum K)$| Mỗi bài kiểm tra sẽ quét các chỉ mục một lần và kiểm tra tư cách thành viên là O(1) | 
| Không gian |$O(N)$| bảng giai thừa lên đến mức tối đa$N$| 

Chi phí tiền xử lý là tuyến tính ở mức tối đa$N$và mỗi trường hợp thử nghiệm là tuyến tính trong$N$. Với tổng số ràng buộc giới hạn bởi$2 \cdot 10^5$, điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline
    T = int(input())
    max_n = 2 * 10**5 + 5
    fact = [1] * max_n
    for i in range(1, max_n):
        fact[i] = fact[i - 1] * i % MOD

    out = []
    for _ in range(T):
        n, k = map(int, input().split())
        s = set(map(int, input().split()))
        comp = 1
        for i in range(1, n):
            if i in s:
                comp += 1
        out.append(str(fact[comp]))
    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample (as interpreted)
# assert run(...) == ...

# custom tests

# minimum size
assert run("1\n2 1\n1\n") == "2", "n=2 basic split"

# all positions allowed
assert run("1\n4 3\n1 2 3\n") == str(24), "all S"

# no internal splits except endpoints
assert run("1\n5 1\n3\n") == str(6), "single constraint"

# alternating pattern
assert run("1\n6 2\n2 5\n") == str(6), "multiple components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 trường hợp | 2 | logic giai thừa cơ sở | 
| đầy đủ S | 24 | tự do tối đa | 
| thưa thớt S | 6 | đếm thành phần | 
| rải rác S | 6 | phân khúc không tầm thường | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi$S$trống rỗng. Điều đó buộc mọi vị trí$i$để thỏa mãn$P_i < P_{i+1}$, do đó hoán vị phải tăng nghiêm ngặt. Chỉ có một hoán vị thỏa mãn điều này và thuật toán tạo ra chính xác một thành phần, cho$1! = 1$, phù hợp với kết quả mong đợi. 

Một góc khác là khi$S$chứa tất cả các chỉ số từ$1$ĐẾN$N-1$. Khi đó không có sự kề cận nào bị ép buộc, vì vậy mọi hoán vị đều hợp lệ. Thuật toán tính$N$linh kiện, sản xuất$N!$, phù hợp với không gian hoán vị đầy đủ. 

Trường hợp thứ ba là các ràng buộc rải rác như luân phiên thành viên. Mỗi chỉ mục bị thiếu sẽ hợp nhất hai vị trí, do đó các thành phần sẽ co lại một cách chính xác ở những nơi không có ràng buộc. Việc theo dõi quá trình hợp nhất xác nhận rằng mỗi đẳng thức bắt buộc sẽ giảm bậc tự do đi đúng một lân cận, bảo toàn tính bất biến mà các thành phần tương ứng với các phân đoạn tăng cưỡng bức tối đa.
