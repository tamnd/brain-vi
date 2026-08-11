---
title: "CF 102399J - \u041a\u043e\u043d\u043a\u0443\u0440\u0441 \u043a\u043e\u0442\u0438\u043a\u043e\u0432"
description: "Có (n) cư dân và (n) mèo. Cư dân (i) sở hữu con mèo (i). Mỗi mối quan hệ quen biết được biểu diễn bằng một cạnh có hướng từ cư dân (a) đến mèo (b), nghĩa là cư dân (a) biết mèo (b). Mỗi cư dân đều biết con mèo của mình nên mỗi đỉnh đều có một vòng tự lặp."
date: "2026-08-11T16:00:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "J"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 227
verified: false
draft: false
---

[CF 102399J - \u041a\u043e\u043d\u043a\u0443\u0440\u0441 \u043a\u043e\u0442\u0438\u043a\u043e\u0432](https://codeforces.com/problemset/problem/102399/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Có (n) cư dân và (n) mèo. Cư dân (i) sở hữu con mèo (i). Mỗi mối quan hệ quen biết được biểu diễn bằng một cạnh có hướng từ cư dân (a) đến mèo (b), nghĩa là cư dân (a) biết mèo (b). Mỗi cư dân đều biết con mèo của mình nên mỗi đỉnh đều có một vòng tự lặp. 

Chúng ta cần chia (n) chỉ số thành hai tập hợp khác rỗng. Tập đầu tiên trở thành ban giám khảo, nghĩa là những chỉ số đó được coi là đối tượng cư trú. Nhóm thứ hai trở thành những người tham gia, nghĩa là những chỉ số đó được coi là mèo. Các kích thước phải cộng lại bằng (n), vì vậy mọi chỉ mục phải thuộc chính xác một trong hai bộ. Thành viên bồi thẩm đoàn không được biết bất kỳ con mèo nào tham gia. 

Điều kiện cuối cùng có cách giải thích biểu đồ hữu ích. Giả sử chỉ số (a) được gán cho ban giám khảo và có một cạnh (a \to b). Khi đó mèo (b) không thể là người tham gia nên (b) cũng phải thuộc ban giám khảo. Vì vậy, bất cứ khi nào một đỉnh được đưa vào ban giám khảo, mọi đỉnh có thể tiếp cận được bằng một cạnh đi ra cũng phải được đặt ở đó. Bồi thẩm đoàn là một tập hợp con thực sự khác rỗng được đóng dưới các cạnh đi ra. 

Các ràng buộc gợi ý mạnh mẽ về một thuật toán đồ thị tuyến tính. Có thể có tới (10^6) đỉnh và (10^6) cạnh quen thuộc trong tất cả các bài kiểm tra. Thậm chí (O(n^2)) là quá lớn, trong khi (O(n+m)) là phù hợp. Số lượng lớn các trường hợp thử nghiệm cũng có nghĩa là việc triển khai chỉ nên xử lý mỗi cạnh với số lần không đổi và sử dụng truyền tải biểu đồ lặp thay vì DFS đệ quy, vì biểu đồ có các đỉnh (10^6) có thể dễ dàng vượt quá giới hạn đệ quy của Python và tạo ra chi phí ngăn xếp cuộc gọi quá mức. 

Có một số trường hợp nguy hiểm có thể đánh lừa một công trình xây dựng bất cẩn. 

Đối với (n=1), phân vùng duy nhất có thể có sẽ không có thành viên ban giám khảo hoặc không có người tham gia. Đầu ra cần thiết là`No`.```
1
1 1
1 1
```Việc xây dựng chọn một cách mù quáng một tập hợp có thể tiếp cận hoặc không thể tiếp cận có thể vô tình tạo ra một mặt trống. 

Đối với một đồ thị không liên thông chặt chẽ, câu trả lời vẫn tồn tại ngay cả khi có nhiều cạnh. Ví dụ,```
2
2 3
1 1
2 2
1 2
```Câu trả lời có thể là ban giám khảo ({2}) và con mèo tham gia ({1}). Cư dân 2 chỉ biết cat 2 nên điều kiện chéo được thỏa mãn. Một cách tiếp cận bất cẩn đòi hỏi phải tìm các đỉnh có bậc bằng 0 sẽ bỏ lỡ điều này, bởi vì cả hai đỉnh đều có các cạnh hướng ra ngoài. 

Hướng của biểu đồ cũng có vấn đề. Coi như```
3
3 4
1 1
2 2
3 3
2 1
```Ở đây bồi thẩm đoàn ({1}) hoạt động, bởi vì cư dân 1 chỉ biết cat 1. Tập hợp các đỉnh không thể chạm tới đỉnh 1 là ({2,3}), tập hợp này cũng tạo thành bồi thẩm đoàn hợp lệ. Việc triển khai coi sự quen biết như một mối quan hệ vô hướng sẽ kết luận không chính xác rằng đỉnh 1 và 2 phải ở cùng một phía, mặc dù điều kiện chỉ cấm một cạnh từ thành viên bồi thẩm đoàn đến con mèo tham gia. 

Cuối cùng, nếu mọi đỉnh đều có thể chạm tới mọi đỉnh khác thì không tồn tại nghiệm nào. Ví dụ,```
2
2 4
1 1
1 2
2 1
2 2
```Việc đưa một trong hai đỉnh vào ban giám khảo sẽ buộc đỉnh còn lại vào ban giám khảo thông qua khả năng tiếp cận. Do đó mọi tập đóng khác rỗng là toàn bộ đồ thị, không có phần tử nào. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê mọi tập hợp bồi thẩm đoàn có thể có. Có (2^n) tập hợp con, tập trống và tập đầy đủ không hợp lệ, để lại (2^n-2) ứng cử viên. Đối với mỗi ứng cử viên, chúng ta có thể quét tất cả (m) cạnh quen biết và loại bỏ nó nếu một cạnh nào đó bắt đầu trong ban giám khảo và kết thúc bên ngoài nó. Điều này đúng vì mọi phân vùng có thể đều được kiểm tra, nhưng kết quả trong trường hợp xấu nhất của nó là ((2^n-2)m), vốn đã rất lớn đối với (n=30), chứ đừng nói đến (n=10^6). 

Lực lượng vũ phu hoạt động vì sự lựa chọn thực sự duy nhất là chỉ số nào thuộc về ban giám khảo. Điều quan trọng là phải hiểu điều gì làm cho tập hợp con được chọn hợp lệ. Nếu (a) nằm trong bồi thẩm đoàn và có cạnh (a\to b), thì (b) cũng phải nằm trong bồi thẩm đoàn. Việc lặp lại lập luận này cho thấy bồi thẩm đoàn phải chứa mọi đỉnh có thể tiếp cận được từ bất kỳ đỉnh bồi thẩm đoàn nào. Trong thuật ngữ đồ thị, chúng ta cần một tập hợp đúng khác rỗng được đóng dưới các cạnh bên ngoài. 

Điều này ngay lập tức kết nối vấn đề với khả năng tiếp cận. Bắt đầu từ đỉnh 1. Nếu một số đỉnh không thể đạt tới từ 1, hãy lấy tất cả các đỉnh không thể chạm tới đó làm ban giám khảo. Tập hợp này được đóng dưới các cạnh đi ra: nếu một đỉnh không thể tiếp cận được có cạnh nối với một đỉnh có thể tiếp cận thì chính nó sẽ có thể tiếp cận được. 

Điều gì sẽ xảy ra nếu mọi đỉnh đều có thể truy cập được từ 1? Sau đó chúng tôi thực hiện ý tưởng tương tự trong biểu đồ đảo ngược. Nếu một số đỉnh không thể đạt tới 1 trong biểu đồ đảo ngược, điều đó có nghĩa là đỉnh tương ứng không thể đạt tới 1 trong biểu đồ gốc. Lấy tất cả các đỉnh như bồi thẩm đoàn. Tập hợp này lại được đóng dưới các cạnh đi ra. Nếu mọi đỉnh cũng có thể tới được từ 1 trong đồ thị đảo ngược thì mọi đỉnh đều có thể tới 1 và 1 có thể tới mọi đỉnh, do đó đồ thị được kết nối chặt chẽ. Trong một đồ thị liên thông mạnh, mọi tập đóng-đi ra khác rỗng phải chứa tất cả các đỉnh, khiến cho việc phân vùng hợp lệ là không thể. 

Do đó, hai phép duyệt phân biệt chính xác hai trường hợp chúng ta cần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n·m)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mọi chỉ số (1,\ldots,n) là một đỉnh của đồ thị. Với mỗi cặp quen biết ((a,b)), hãy thêm một cạnh có hướng (a\to b). Hướng thể hiện chính xác tình huống bị cấm: đỉnh bồi thẩm đoàn (a) không thể có cạnh đi ra so với đỉnh người tham gia (b). 
2. Chạy DFS hoặc BFS từ đỉnh (1) trong biểu đồ gốc và đánh dấu mọi đỉnh có thể tiếp cận. Nếu một số đỉnh vẫn chưa được thăm, hãy đặt chính xác các đỉnh đó vào ban giám khảo và đặt mọi đỉnh khác vào tập người tham gia. Đây là trường hợp hữu ích đầu tiên vì một cạnh không thể rời khỏi tập không thể tiếp cận và đi vào tập hợp có thể tiếp cận. 
3. Nếu đạt đến mọi đỉnh, hãy xây dựng đồ thị đảo ngược và chạy đường truyền từ đỉnh (1) tại đó. Việc tiếp cận (v) trong đồ thị đảo ngược tương đương với việc nói rằng (v) có thể đạt tới (1) trong đồ thị ban đầu. 
4. Nếu một số đỉnh vẫn không thể truy cập được trong biểu đồ đảo ngược, hãy đặt các đỉnh đó vào ban giám khảo và tất cả các đỉnh khác vào tập người tham gia. Tập hợp này được đóng lại dưới các cạnh đi ra ban đầu. Giả sử (v) có trong đó và (v\to u). Nếu (u) có thể tới (1), thì (v) có thể tới (u) và sau đó (1), mâu thuẫn với việc (v) không thể tới được từ (1) trong đồ thị đảo ngược. 
5. Nếu lần duyệt thứ hai cũng tới mọi đỉnh, xuất ra`No`. Tại thời điểm này, đỉnh 1 có thể chạm tới mọi đỉnh và mọi đỉnh đều có thể chạm tới đỉnh 1. Do đó, mọi cặp đỉnh đều có thể chạm tới nhau nên đồ thị được liên thông chặt chẽ. 
6. Đối với trường hợp thành công, xuất ra các đỉnh trong tập đóng đã chọn làm thành viên bồi thẩm đoàn và phần bù của nó là mèo tham gia. Hai bộ phân vùng tất cả (n) chỉ mục, do đó kích thước của chúng tự động tổng hợp thành (n). Vì việc truyền tải thành công còn lại ít nhất một đỉnh nằm ngoài tập hợp đã chọn nên cả hai cạnh đều khác rỗng. 

### Tại sao nó hoạt động 

Điều bất biến trung tâm là bồi thẩm đoàn phải khép kín dưới những góc cạnh hướng ngoại. Lần duyệt đầu tiên xây dựng một tập hợp như vậy từ các đỉnh không thể đạt tới từ 1. Lần truyền thứ hai xây dựng một tập hợp như vậy từ các đỉnh không thể đạt tới 1. Trong cả hai trường hợp, một cạnh đi ra từ tập hợp đã chọn không thể nhập phần bù của nó, vì vậy không có thành viên bồi thẩm đoàn nào biết một con mèo tham gia. 

Nếu cả hai đường đi đều đạt tới mọi đỉnh thì đồ thị được liên thông chặt chẽ. Lấy bất kỳ tập bồi thẩm đoàn nào khác trống và chọn một đỉnh (v) trong đó. Vì đồ thị có tính liên thông mạnh nên mọi đỉnh đều có thể đến được từ (v). Do đó, việc đóng cửa dưới các cạnh hướng ra sẽ buộc mọi đỉnh vào trong bồi thẩm đoàn. Ban giám khảo sau đó sẽ chứa tất cả (n) đỉnh và sẽ không có người tham gia, điều này bị cấm. Như vậy`No`đúng trong trường hợp liên thông mạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def traverse(graph, start):
    n = len(graph)
    seen = bytearray(n)
    seen[start] = 1
    stack = [start]

    while stack:
        v = stack.pop()
        for u in graph[v]:
            if not seen[u]:
                seen[u] = 1
                stack.append(u)

    return seen

def solve():
    t = int(input())
    answer = []

    for _ in range(t):
        line = input()
        while not line.strip():
            line = input()

        n, m = map(int, line.split())

        graph = [[] for _ in range(n)]
        rev = [[] for _ in range(n)]

        for _ in range(m):
            a, b = map(int, input().split())
            a -= 1
            b -= 1
            graph[a].append(b)
            rev[b].append(a)

        reachable = traverse(graph, 0)

        if not all(reachable):
            jury = [i + 1 for i in range(n) if not reachable[i]]
            participants = [i + 1 for i in range(n) if reachable[i]]

            answer.append("Yes")
            answer.append(f"{len(jury)} {len(participants)}")
            answer.append(" ".join(map(str, jury)))
            answer.append(" ".join(map(str, participants)))
            continue

        can_reach_one = traverse(rev, 0)

        if not all(can_reach_one):
            jury = [i + 1 for i in range(n) if not can_reach_one[i]]
            participants = [i + 1 for i in range(n) if can_reach_one[i]]

            answer.append("Yes")
            answer.append(f"{len(jury)} {len(participants)}")
            answer.append(" ".join(map(str, jury)))
            answer.append(" ".join(map(str, participants)))
        else:
            answer.append("No")

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```Danh sách lân cận`graph`đại diện cho các khía cạnh quen biết giữa cư dân và mèo. Danh sách đảo ngược`rev`chứa chính xác các cạnh đối diện, cho phép phép duyệt thứ hai trả lời câu hỏi "đỉnh nào có thể đạt tới đỉnh 1?" mà không cần thực hiện tìm kiếm riêng biệt từ mọi đỉnh. 

Việc truyền tải sử dụng một`bytearray`thay vì danh sách boolean của Python. Với tối đa (10^6) đỉnh, điều này làm giảm đáng kể mức tiêu thụ bộ nhớ. Ngăn xếp cũng được lặp đi lặp lại, tránh các vấn đề về độ sâu đệ quy trên chuỗi dài. 

Tìm kiếm đầu tiên kiểm tra xem mọi đỉnh có thể truy cập được từ đỉnh 1 hay không. Khi một số đỉnh không thể truy cập được, mã sẽ chọn chính xác các đỉnh không thể truy cập được làm ban giám khảo. điều kiện`if not reachable[i]`là sự lựa chọn biên quan trọng: bản thân đỉnh 1 thuộc về phần bù trong trường hợp này, trong khi mọi đỉnh không thể chạm tới đều thuộc về ban giám khảo. 

Tìm kiếm thứ hai sử dụng`rev`. Một đỉnh được đánh dấu chính xác ở đó khi nó có thể đạt đến đỉnh 1 trong biểu đồ ban đầu. Theo đó, việc lựa chọn`not can_reach_one[i]`đưa ra tập đóng thứ hai có thể có. 

Không có số nguyên nào có thể tràn vì số nguyên Python có độ chính xác tùy ý và tất cả các chỉ mục chỉ được chuyển đổi sang dạng dựa trên 0 trong quá trình triển khai. Đầu vào đảm bảo rằng các cặp quen biết là duy nhất, do đó không cần phải loại bỏ các cạnh trùng lặp. 

Các ca kiểm thử phân tách dòng trống được xử lý bằng cách bỏ qua các dòng trống trước khi đọc (n) và (m). Đầu ra được tích lũy và ghi một lần, nhanh hơn đáng kể so với việc gọi nhiều lần`print`cho hàng triệu chỉ số tiềm năng. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp thử nghiệm đầu tiên từ mẫu.```
3 4
1 1
2 2
3 3
1 3
```Các cạnh có hướng là (1\to1), (2\to2), (3\to3) và (1\to3). 

| Bước | Đỉnh bắt đầu | Các đỉnh có thể tiếp cận | Các đỉnh không thể tiếp cận | Hành động | 
| --- | --- | --- | --- | --- | 
| DFS gốc | 1 | ({1,3}) | ({2}) | Ban giám khảo = ({2}) | 
| Đầu ra | | Ban giám khảo ({2}) | Người tham gia ({1,3}) |`Yes`| 

Tập hợp ({2}) bị đóng vì cạnh đi ra duy nhất của nó là (2\to2). Những con mèo tham gia là 1 và 3, và cư dân 2 không biết ai trong số chúng. Điều này cung cấp một phân vùng hợp lệ với kích thước (1+2=3). 

Bây giờ hãy xem xét thử nghiệm mẫu thứ hai.```
3 7
1 1
1 2
1 3
2 2
3 1
3 2
3 3
```Đồ thị có các cạnh từ 1 đến mọi đỉnh, từ 3 đến mọi đỉnh ngoại trừ chính nó cũng có 3 và đỉnh 2 chỉ có vòng lặp tự nó. 

| Bước | Đỉnh bắt đầu | Các đỉnh có thể tiếp cận | Các đỉnh không thể tiếp cận | Hành động | 
| --- | --- | --- | --- | --- | 
| DFS gốc | 1 | ({1,2,3}) | (\varnothing) | Tiếp tục | 
| Đảo ngược DFS | 1 | ({1,3}) | ({2}) | Ban giám khảo = ({2}) | 
| Đầu ra | | Ban giám khảo ({2}) | Người tham gia ({1,3}) |`Yes`| 

Trong biểu đồ ban đầu, mọi đỉnh đều có thể tiếp cận được từ 1, vì vậy cách xây dựng đầu tiên không thể đưa ra một ban giám khảo khác trống. Trong đồ thị đảo ngược, đỉnh 2 không thể tới được từ 1, nghĩa là đỉnh 2 không thể tới được 1 trong đồ thị ban đầu. Chọn đỉnh 2 làm ban giám khảo vì cạnh đi ra duy nhất của nó là vòng tự lặp (2\to2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Mỗi cạnh của đồ thị được kiểm tra nhiều nhất một lần trong mỗi hai lần duyệt. | 
| Không gian | (O(n+m)) | Danh sách kề ban đầu và đảo ngược lưu trữ các mục nhập được định hướng (2m), cộng với trạng thái truyền tải. | 

Trên tất cả các trường hợp thử nghiệm, tổng của (n) và (m) nhiều nhất là (10^6). Do đó, tổng số phép toán đồ thị là tuyến tính ở khoảng (2\cdot10^6) đỉnh và (2\cdot10^6) mục nhập cạnh, phù hợp với các giới hạn đã cho. Quá trình truyền tải lặp lại cũng tránh được chi phí đệ quy Python và các lỗi về độ sâu đệ quy. 

## Trường hợp thử nghiệm 

Vì sự cố chấp nhận bất kỳ phân vùng hợp lệ nào nên chuỗi đầu ra chính xác không phải là một xác nhận phù hợp. Thay vào đó, khai thác thử nghiệm bên dưới sẽ xác thực kết quả đầu ra thực tế: nó kiểm tra kích thước, xác minh rằng cả hai bộ tạo thành một phân vùng và kiểm tra mọi cạnh của người quen so với nhóm ban giám khảo và người tham gia.```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        t = int(sys.stdin.readline())
        answer = []

        def traverse(graph, start):
            n = len(graph)
            seen = bytearray(n)
            seen[start] = 1
            stack = [start]

            while stack:
                v = stack.pop()
                for u in graph[v]:
                    if not seen[u]:
                        seen[u] = 1
                        stack.append(u)

            return seen

        for _ in range(t):
            line = sys.stdin.readline()
            while not line.strip():
                line = sys.stdin.readline()

            n, m = map(int, line.split())
            graph = [[] for _ in range(n)]
            rev = [[] for _ in range(n)]

            edges = []

            for _ in range(m):
                a, b = map(int, sys.stdin.readline().split())
                a -= 1
                b -= 1
                graph[a].append(b)
                rev[b].append(a)
                edges.append((a, b))

            first = traverse(graph, 0)

            if not all(first):
                jury = [i + 1 for i in range(n) if not first[i]]
                participants = [i + 1 for i in range(n) if first[i]]

                answer.append("Yes")
                answer.append(f"{len(jury)} {len(participants)}")
                answer.append(" ".join(map(str, jury)))
                answer.append(" ".join(map(str, participants)))
            else:
                second = traverse(rev, 0)

                if not all(second):
                    jury = [i + 1 for i in range(n) if not second[i]]
                    participants = [i + 1 for i in range(n) if second[i]]

                    answer.append("Yes")
                    answer.append(f"{len(jury)} {len(participants)}")
                    answer.append(" ".join(map(str, jury)))
                    answer.append(" ".join(map(str, participants)))
                else:
                    answer.append("No")

        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str) -> bool:
    data = inp.split()
    pos = 0
    t = int(data[pos])
    pos += 1

    cases = []

    for _ in range(t):
        n = int(data[pos])
        m = int(data[pos + 1])
        pos += 2

        edges = []
        for _ in range(m):
            a = int(data[pos])
            b = int(data[pos + 1])
            pos += 2
            edges.append((a, b))

        cases.append((n, edges))

    tokens = out.split()
    pos = 0

    for n, edges in cases:
        if pos >= len(tokens):
            return False

        verdict = tokens[pos]
        pos += 1

        if verdict == "No":
            continue

        if verdict != "Yes" or pos + 2 > len(tokens):
            return False

        j = int(tokens[pos])
        p = int(tokens[pos + 1])
        pos += 2

        if j <= 0 or p <= 0 or j + p != n:
            return False

        if pos + j + p > len(tokens):
            return False

        jury = list(map(int, tokens[pos:pos + j]))
        pos += j

        participants = list(map(int, tokens[pos:pos + p]))
        pos += p

        if len(set(jury)) != j or len(set(participants)) != p:
            return False

        if any(x < 1 or x > n for x in jury + participants):
            return False

        jury_set = set(jury)
        participant_set = set(participants)

        if jury_set & participant_set:
            return False

        if len(jury_set | participant_set) != n:
            return False

        for a, b in edges:
            if a in jury_set and b in participant_set:
                return False

    return pos == len(tokens)

sample = """\
4
3 4
1 1
2 2
3 3
1 3

3 7
1 1
1 2
1 3
2 2
3 1
3 2
3 3

1 1
1 1

2 4
1 1
1 2
2 1
2 2
"""

assert validate(sample, solve_data(sample)), "provided sample"

minimum = """\
1
1 1
1 1
"""

assert solve_data(minimum).strip() == "No", "minimum-size case"

all_self = """\
1
4 4
1 1
2 2
3 3
4 4
"""

assert validate(all_self, solve_data(all_self)), "all-self edges"

chain = """\
1
4 7
1 1
2 2
3 3
4 4
1 2
2 3
3 4
"""

assert validate(chain, solve_data(chain)), "directed chain"

strongly_connected = """\
1
3 6
1 1
2 2
3 3
1 2
2 3
3 1
"""

assert solve_data(strongly_connected).strip() == "No", "strongly connected graph"

boundary = """\
1
2 3
1 1
2 2
1 2
"""

assert validate(boundary, solve_data(boundary)), "two-vertex boundary case"

large_self_loop_case = "1\n100000 100000\n" + "".join(
    f"{i} {i}\n" for i in range(1, 100001)
)

assert validate(
    large_self_loop_case,
    solve_data(large_self_loop_case)
), "large linear-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 1`|`No`| Đồ thị nhỏ nhất có thể không thể chia thành hai cạnh khác rỗng. | 
| Bốn vòng tự |`Yes`| Các đỉnh chỉ có cạnh tự có thể được chia tùy ý thành hai nhóm khác rỗng. | 
| Chuỗi chỉ đạo |`Yes`| Kiểm tra xem tập hợp đã chọn có phải được đóng dưới các cạnh đi ra hay không. | 
| Ba chu kỳ có vòng lặp tự |`No`| Phát hiện chính xác đồ thị được kết nối mạnh. | 
| Hai đỉnh với (1\to2) |`Yes`| Kiểm tra hàm ý có hướng không cần thiết nhỏ nhất và lập chỉ mục ranh giới. | 
| (100000) tự vòng lặp |`Yes`| Thực hiện bộ nhớ tuyến tính và hành vi thời gian trên một đầu vào lớn. | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu,```
1
1 1
1 1
```đường truyền đầu tiên đạt tới đỉnh 1, do đó không có đỉnh nào không thể tới được. Quá trình duyệt ngược cũng đạt tới đỉnh 1. Đồ thị liên thông mạnh nghĩa là thuật toán in ra`No`. Điều này tránh được lỗi phổ biến khi in tập hợp ban giám khảo hoặc người tham gia một thành phần trong khi để trống mặt kia. 

Đối với một đồ thị chỉ bao gồm các vòng lặp tự,```
1
4 4
1 1
2 2
3 3
4 4
```lần duyệt đầu tiên từ đỉnh 1 chỉ đến đỉnh 1. Các đỉnh 2, 3 và 4 không thể truy cập được nên thuật toán chọn ({2,3,4}) làm ban giám khảo và ({1}) làm tập hợp người tham gia. Mỗi đỉnh bồi thẩm đoàn chỉ biết con mèo của chính nó, con mèo này cũng là một chỉ số của bồi thẩm đoàn, do đó không có ranh giới nào bị cấm giữa bồi thẩm đoàn với người tham gia. 

Đối với một chuỗi có hướng,```
1
4 7
1 1
2 2
3 3
4 4
1 2
2 3
3 4
```lần duyệt đầu tiên từ 1 đạt đến cả bốn đỉnh, do đó nó không thể trực tiếp cung cấp một tập hợp đóng không thể truy cập được. Trong biểu đồ đảo ngược, đỉnh 1 có thể tiếp cận được từ mọi đỉnh vì chuỗi ban đầu dẫn về 4 thay vì hướng tới 1. Do đó, quá trình truyền ngược chỉ đến đỉnh 1, để lại ({2,3,4}) là ban giám khảo. Tập hợp này được đóng dưới các cạnh ban đầu vì (2\to3) và (3\to4) nằm bên trong nó. 

Đối với trường hợp liên thông mạnh,```
1
3 6
1 1
2 2
3 3
1 2
2 3
3 1
```đường truyền ban đầu từ 1 đến tất cả các đỉnh. Đồ thị đảo ngược cũng cho phép 1 đạt đến tất cả các đỉnh, vì chu trình ban đầu cho phép mọi đỉnh đạt đến 1. Thuật toán in ra`No`. Bất kỳ bồi thẩm đoàn nào không trống sẽ buộc toàn bộ chu trình vào ban giám khảo, không để lại con mèo tham gia nào. 

Đối với trường hợp ranh giới hai đỉnh,```
1
2 3
1 1
2 2
1 2
```đường truyền đầu tiên đạt đến cả hai đỉnh. Trong đồ thị đảo ngược, đỉnh 1 chỉ chạm đến chính nó vì cạnh ban đầu (1\to2) trở thành (2\to1). Do đó, thuật toán chọn đỉnh 2 làm ban giám khảo và đỉnh 1 làm người tham gia. Cư dân 2 chỉ biết cat 2 nên việc xây dựng là hợp lệ. Trường hợp này đặc biệt hữu ích để phát hiện lỗi từng lỗi một trong quá trình chuyển đổi giữa các nhãn đầu vào dựa trên một và mảng Python dựa trên 0.
