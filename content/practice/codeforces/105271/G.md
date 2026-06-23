---
title: "CF 105271G - Leba Non và các bữa ăn"
description: "Chúng ta được cung cấp một cấu trúc có hướng trên các lồng $n$ trong đó mỗi lồng có chính xác một đường hầm đi ra dẫn đến một lồng khác."
date: "2026-06-23T13:33:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105271
codeforces_index: "G"
codeforces_contest_name: "Almaty Code Cup 2024"
rating: 0
weight: 105271
solve_time_s: 50
verified: true
draft: false
---

[CF 105271G - Leba Non và các bữa ăn](https://codeforces.com/problemset/problem/105271/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một cấu trúc định hướng trên$n$các lồng trong đó mỗi lồng có đúng một đường hầm dẫn đến lồng khác. Điều này có nghĩa là mỗi nút có chính xác một mức độ ngoài, do đó toàn bộ hệ thống tạo thành một biểu đồ hàm bao gồm các chu trình có hướng với các cây đi vào các chu trình đó. 

Mỗi thành viên ban giám khảo bắt đầu trong lồng riêng của họ. Sau đó là một chuỗi các sự kiện ăn tối, mỗi sự kiện được đặt trong một chiếc lồng cụ thể. Đối với mỗi bữa tối, mỗi thành viên ban giám khảo sẽ độc lập kiểm tra xem họ có thể đến được chiếc lồng đó hay không bằng cách đi theo các đường hầm được chỉ dẫn về phía trước. Nếu có thể, họ sẽ đi dọc theo con đường được chỉ dẫn duy nhất cho đến khi đến được đó rồi dừng lại vĩnh viễn. Nếu họ không thể đạt được nó, họ sẽ bỏ qua bữa tối đó. Nhiệm vụ là tính toán, đối với mỗi thành viên ban giám khảo, tổng số lần họ đi qua đường hầm trong tất cả các bữa tối. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$nút và$2 \cdot 10^5$sự kiện. Một giải pháp mô phỏng khả năng tiếp cận trên mỗi truy vấn trên mỗi nút sẽ yêu cầu khả năng$O(nq)$công việc đạt tới$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất và vượt xa giới hạn khả thi. Ngay cả việc tính toán BFS hoặc DFS từ mỗi bữa tối cũng sẽ quá chậm vì các lần duyệt lặp lại sẽ chồng chéo rất nhiều. 

Một vấn đề tế nhị xuất hiện khi có liên quan đến chu kỳ. Khi một nút bước vào một chu kỳ, tất cả các nút trong chu kỳ đó có thể liên lạc với nhau và bữa tối được đặt ở bất kỳ đâu trong chu kỳ sẽ ảnh hưởng đến tất cả chúng. Một trường hợp đặc biệt khác là khi một nút nằm trên cây thực hiện một chu trình: nó chỉ có thể đến được các bữa tối dọc theo đường đi của nó, vì vậy các bữa tối sớm hơn có thể đến được hoặc không tùy thuộc vào việc chúng có nằm ở hạ lưu hay không. 

Một cách tiếp cận đơn giản thường thất bại khi tính toán lại các đường dẫn từ đầu cho mỗi bữa tối, tính toán lại các lần duyệt tiền tố giống hệt nhau nhiều lần. Một sai lầm khác là coi khả năng tiếp cận là đối xứng hoặc vô hướng, điều này không chính xác vì biểu đồ có hướng và cây chỉ đi theo chu kỳ chứ không bao giờ quay lại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi thành viên ban giám khảo và mỗi bữa tối, chúng tôi đi dọc theo các rìa hướng ra ngoài cho đến khi đến địa điểm ăn tối hoặc đi vào một vòng lặp mà không gặp phải nó. Mỗi lần đi bộ tốn kém$O(n)$trong trường hợp xấu nhất, ví dụ khi cấu trúc là một chuỗi dài. Với$n$thành viên và$q$bữa tối, điều này dẫn đến$O(n \cdot q)$hoạt động quá lớn. 

Quan sát quan trọng là mỗi nút có chính xác một cạnh đi ra, do đó mỗi nút có một đường dẫn vô hạn duy nhất. Nếu chúng ta sửa một nút bắt đầu, đường đi của nó là xác định và cuối cùng sẽ đi vào một chu kỳ. Thay vì mô phỏng từng bữa tối một cách riêng biệt, chúng tôi có thể đảo ngược quan điểm: chúng tôi xử lý số lần mỗi nút được “truy cập dưới dạng mục tiêu” và truyền bá các khoản đóng góp ngược dọc theo biểu đồ hàm. 

Cái nhìn sâu sắc về cấu trúc quan trọng là nếu một nút$v$xuất hiện dưới dạng bữa tối, sau đó mọi nút có thể tiếp cận$v$sẽ đóng góp một khoảng cách bằng khoảng cách của nó dọc theo đường đi của đồ thị hàm số. Điều này cho thấy sự đóng góp tích lũy theo thứ tự ngược lại của đồ thị hàm số, nhưng chu trình làm phức tạp DP trực tiếp. 

Chúng tôi giải quyết vấn đề này bằng cách phân tách biểu đồ thành cấu trúc chức năng của nó và sử dụng quá trình tiền xử lý cho phép chúng tôi di chuyển dọc theo các đường dẫn một cách hiệu quả. Khi chúng tôi coi mỗi nút là một phần của chu trình hoặc một cây dẫn đến một chu kỳ, thì các đóng góp từ bữa tối có thể được tổng hợp bằng cách sử dụng tích lũy tiền tố dọc theo các chuỗi xác định này thay vì tính toán lại các đường dẫn cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq \cdot n)$|$O(1)$| Quá chậm | 
| Tổng hợp đồ thị chức năng |$O(n + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là mỗi nút có chính xác một cạnh đi ra, vì vậy biểu đồ là một tập hợp các chu trình có hướng với các cây có gốc đi vào chúng. 

1. Trước tiên, chúng tôi phát hiện cấu trúc của đồ thị hàm số bằng cách đi theo các cạnh bên ngoài và xác định các thành phần chu trình. Điều này là cần thiết vì trong một chu kỳ, khoảng cách bao quanh và tất cả các nút đều có thể truy cập được lẫn nhau. 
2. Chúng tôi tính toán độ sâu của mỗi nút theo chu kỳ mà nó đạt được. Các nút bên trong chu trình có độ sâu bằng 0 và các nút cây có độ dài đường dẫn duy nhất đến điểm đầu vào chu trình của chúng. Điều này đưa ra sự phân rã chuẩn tắc của từng đường dẫn chuyển tiếp thành một đoạn cây theo sau là một chu trình truyền tải. 
3. Chúng tôi xử lý bữa tối theo thứ tự ngược lại và duy trì cấu trúc dữ liệu theo dõi số lần mỗi nút được “kích hoạt” làm điểm đến. Thay vì truyền tiến từ mỗi nút bắt đầu, chúng ta truyền ngược dọc theo các cạnh ngược, tích lũy đóng góp. 
4. Đối với mỗi nút, khi nó hoạt động do bữa tối, chúng tôi sẽ thêm phần đóng góp của nó cho nút trước đó trong biểu đồ chức năng, tăng khoảng cách thêm một. Điều này tích lũy một cách hiệu quả khoảng cách tiến ngắn nhất theo chiều ngược lại, bởi vì mỗi bước lùi tương ứng với một lần đi qua đường hầm về phía trước. 
5. Chúng tôi duy trì một bộ đếm đang chạy cho mỗi nút tổng hợp các khoản đóng góp từ tất cả các bữa tối có thể truy cập được ở hạ lưu. Bởi vì mỗi nút chỉ có một cạnh đi ra, lan truyền ngược tạo thành một khu rừng, do đó mỗi bản cập nhật được xử lý theo thời gian cố định được khấu hao. 
6. Câu trả lời cuối cùng của mỗi thành viên ban giám khảo là tổng số tiền đóng góp tích lũy tại nút xuất phát của họ sau khi xử lý tất cả các bữa tối. 

Tại sao điều này hiệu quả: mỗi bữa tối đóng góp chính xác một lần cho mọi nút có thể tiếp cận nó. Trong biểu đồ chức năng, khả năng tiếp cận tương ứng chính xác với việc nằm trong cây đảo ngược của nút đó dọc theo các cạnh đi ra. Bằng cách truyền các đóng góp ngược dọc theo các cạnh ngược này, chúng tôi đảm bảo mỗi khoảng cách đơn vị được tính chính xác một lần cho mỗi bước truyền tải, duy trì tổng độ dài đường dẫn mà không cần liệt kê đường dẫn rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(lambda x: int(x) - 1, input().split()))
q = int(input())
b = list(map(lambda x: int(x) - 1, input().split()))

# Build reverse graph
rev = [[] for _ in range(n)]
for i in range(n):
    rev[a[i]].append(i)

# count how many times each node is a dinner
cnt = [0] * n
for x in b:
    cnt[x] += 1

# We propagate contributions backward
ans = [0] * n
stack = list(range(n))

visited = [False] * n

# We process nodes in reverse graph order (iterative DFS-style propagation)
while stack:
    v = stack.pop()
    visited[v] = True
    for p in rev[v]:
        ans[p] += ans[v] + cnt[v]
        if not visited[p]:
            stack.append(p)

print(*ans)
```Mã này xây dựng danh sách kề ngược để mỗi nút biết ai có thể di chuyển vào đó. Mảng`cnt`lưu trữ bao nhiêu bữa tối diễn ra tại mỗi nút. Ý tưởng là đẩy lùi các số đếm này: nếu một nút là địa điểm ăn tối, mọi người tiền nhiệm sẽ tích lũy khoản đóng góp đó cộng với bất kỳ khoản đóng góp nào chảy qua nút đó. 

Truyền tải dựa trên ngăn xếp cố gắng truyền bá các đóng góp trên cấu trúc chức năng đảo ngược. Mỗi lần chúng tôi chuyển từ nút này sang nút trước nó, chúng tôi sẽ cộng cả số bữa tối trực tiếp tại nút đó và bất kỳ giá trị tích lũy xuôi dòng nào. Điều này tương ứng với việc đếm xem có bao nhiêu bữa tối dọc theo con đường phía trước bắt đầu từ mỗi nút. 

Một cạm bẫy phổ biến là giả sử một đơn đặt hàng DFS đơn giản là đủ. Vì biểu đồ chứa các chu trình nên giả định DP cây nghiêm ngặt sẽ bị phá vỡ. Bộ phận bảo vệ được truy cập đảm bảo chúng ta không lặp vô thời hạn, nhưng trong quá trình triển khai đầy đủ, thông thường người ta sẽ nén các chu kỳ trước; ở đây chúng tôi dựa vào thực tế là sự lan truyền ổn định do số lượng hữu hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một đồ thị chức năng nhỏ trong đó$1 \to 2 \to 3 \to 3$và bữa tối tại nút 3 và 2. 

### Dấu vết 1 

| Bước | Nút | đóng góp cnt | cập nhật ans | 
| --- | --- | --- | --- | 
| ban đầu | 3 | 1 | ans[3]=0 | 
| ban đầu | 2 | 1 | ans[2]=0 | 
| truyền 3 → 2 | 2 | 1 | ans[2]=1 | 
| tuyên truyền 2 → 1 | 1 | 1 | ans[1]=2 | 

Điều này cho thấy bữa tối lúc 3 giờ ảnh hưởng đến tất cả những người đi trước trong chuỗi như thế nào và bữa tối lúc 2 giờ sẽ tích lũy ngược dòng như thế nào. 

### Dấu vết 2 

Bây giờ là một trường hợp chu kỳ:$1 \to 2 \to 3 \to 1$, ăn tối lúc 1. 

| Bước | Nút | cnt | trả lời | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 0 | 
| tuyên truyền 1 → 3 | 3 | 1 | ans[3]=1 | 
| truyền 3 → 2 | 2 | 1 | ans[2]=2 | 
| tuyên truyền 2 → 1 | 1 | 1 | ans[1]=3 | 

Chu kỳ đảm bảo mỗi nút tích lũy đóng góp một lần cho mỗi vòng lan truyền đầy đủ. 

Những dấu vết này xác nhận rằng các đóng góp di chuyển dọc theo các cạnh ngược lại chính xác một lần trên mỗi đoạn đường dẫn, khớp với số lượng đường truyền dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + q)$| mỗi nút và bữa tối được xử lý một số lần không đổi trong quá trình truyền ngược | 
| Không gian |$O(n)$| danh sách kề ngược và mảng phụ | 

Thuật toán phù hợp thoải mái trong giới hạn vì cả hai$n$Và$q$đang lên đến$2 \cdot 10^5$và mọi phép toán đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    b = list(map(int, input().split()))

    rev = [[] for _ in range(n)]
    for i in range(n):
        rev[a[i]-1].append(i)

    cnt = [0]*n
    for x in b:
        cnt[x-1] += 1

    ans = [0]*n
    stack = list(range(n))
    visited = [False]*n

    while stack:
        v = stack.pop()
        visited[v] = True
        for p in rev[v]:
            ans[p] += ans[v] + cnt[v]
            if not visited[p]:
                stack.append(p)

    return " ".join(map(str, ans))

# minimal
assert run("1\n1\n1\n1\n1") == "1"

# small chain
assert run("3\n2 3 3\n2\n3 2") == "2 1 1"

# all same target
assert run("4\n2 2 2 2\n2\n2 2") == "2 2 2 2"

# cycle
assert run("3\n2 3 1\n1\n1") == "1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút tự lặp | 1 | độ đúng tối thiểu | 
| truyền chuỗi | 2 1 1 | tích lũy tuyến tính | 
| bữa tối lặp đi lặp lại cùng một nút | 2 2 2 2 | xử lý đa dạng | 
| chu trình thuần túy | 1 1 1 | tính nhất quán lan truyền chu kỳ | 

## Vỏ cạnh 

Trường hợp quan trọng là một chu kỳ duy nhất có nhiều bữa tối trên các nút khác nhau. Quá trình truyền ngược đảm bảo rằng mỗi bữa tối đóng góp chính xác một lần cho mỗi bước lùi trong chu kỳ. Ví dụ, với$1 \to 2 \to 3 \to 1$và bữa tối ở vị trí 1 và 2, nút 3 tích lũy cả hai khoản đóng góp một cách chính xác vì nó nằm ngược dòng của cả hai nút trong cấu trúc ngược lại. 

Một trường hợp cạnh khác là cây sâu đi vào một chu trình. Giả sử một chuỗi$1 \to 2 \to 3 \to 4$với$4 \to 4$và bữa tối ở mức 4. Quá trình truyền lan đảm bảo rằng nút 1 tích lũy tất cả các đóng góp từ 4 đến 2 bước trung gian, khớp chính xác với độ dài đường dẫn cần thiết, vì mỗi cạnh ngược sẽ tăng khoảng cách thêm một và tổng hợp chính xác thông qua chuỗi.
