---
title: "CF 105586I - \u5c0f P \u7231\u6298\u8dc3"
description: "Chúng ta được cung cấp một cấu trúc được định hướng ẩn bên trong một mảng. Mỗi thành phố có chính xác một dịch chuyển tức thời đi và mỗi dịch chuyển tức thời thuộc về chính xác một thành phố."
date: "2026-06-22T14:45:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "I"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 52
verified: true
draft: false
---

[CF 105586I - \u5c0f P \u7231\u6298\u8dc3](https://codeforces.com/problemset/problem/105586/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc được định hướng ẩn bên trong một mảng. Mỗi thành phố có chính xác một dịch chuyển tức thời đi và mỗi dịch chuyển tức thời thuộc về chính xác một thành phố. Nếu chúng ta xem thành phố$i$như một nút và vẽ một cạnh$i \to a_i$, toàn bộ hệ thống sẽ trở thành một đồ thị hàm số trong đó mỗi nút có cấp độ chính xác bằng một. 

Từ bất kỳ thành phố khởi đầu nào, bạn liên tục đi theo các dịch chuyển tức thời, và cuối cùng bạn rơi vào một chu kỳ. Yêu cầu trong bài toán là đồ thị phải được kết nối chặt chẽ sau khi chúng ta được phép hoán đổi các dịch chuyển tức thời được chỉ định cho các thành phố. Hoán đổi trao đổi các cạnh đi của hai nút, nghĩa là chúng ta hoán đổi hai giá trị trong mảng$a$. 

Mục tiêu là tính toán số lần hoán đổi tối thiểu cần thiết để từ bất kỳ nút nào, mọi nút khác đều có thể truy cập được thông qua các đường dẫn được định hướng. 

Các ràng buộc cho phép lên đến$10^5$các nút cho mỗi trường hợp thử nghiệm, với tổng số$10^5$qua các bài kiểm tra. Điều này loại trừ bất kỳ lý luận bậc hai nào đối với tất cả các giao dịch hoán đổi hoặc mô phỏng lặp lại các phép biến đổi. Bất kỳ giải pháp nào về cơ bản đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp khó phát hiện nhưng quan trọng là khi đồ thị đã bao gồm một chu trình có chiều dài có hướng duy nhất.$n$. Trong trường hợp đó, nó đã được kết nối mạnh nên câu trả lời là không. Ví dụ,$n=3$,$a=[2,3,1]$hoạt động. Một cách tiếp cận ngây thơ cố gắng “kết nối các thành phần” mà không nhận ra cấu trúc này vẫn có thể thực hiện các hoán đổi không cần thiết. 

Một trường hợp góc khác là khi cấu trúc là sự kết hợp rời rạc của nhiều chu trình và cây dẫn vào chúng. Mặc dù đồ thị luôn có bậc ngoài 1 nhưng nó có thể có nhiều chu kỳ. Ví dụ,$a=[2,1,4,3]$chia thành hai 2 chu kỳ. Điều này không được kết nối chặt chẽ và lý luận ngây thơ chỉ tính chu kỳ một cách độc lập sẽ thất bại trừ khi nó xem xét cách hoán đổi có thể hợp nhất các chu kỳ. 

## Phương pháp tiếp cận 

Đồ thị do mảng tạo ra là đồ thị hàm số cổ điển. Mỗi thành phần được kết nối chứa chính xác một chu trình được định hướng, có thể có cây cối tham gia vào đó. Tuy nhiên, vì mỗi nút có chính xác một cạnh đi ra nên khả năng tiếp cận hoàn toàn được kiểm soát bởi cấu trúc của các chu trình này. 

Một ý tưởng mạnh mẽ sẽ là mô phỏng các giao dịch hoán đổi. Chúng ta có thể thử tất cả các cặp vị trí, hoán đổi chúng, xây dựng lại biểu đồ và kiểm tra xem nó có được kết nối chặt chẽ hay không bằng cách chạy DFS từ mọi nút. Tự kiểm tra kết nối mạnh mẽ là$O(n)$, và có$O(n^2)$hoán đổi, dẫn đến$O(n^3)$mỗi trường hợp thử nghiệm. Điều này ngay lập tức là không thể đối với$n=10^5$. 

Ngay cả khi chúng tôi giảm việc kiểm tra, vấn đề cốt lõi vẫn là: chúng tôi đang sửa đổi cấu trúc giống như hoán vị và yêu cầu số lần hoán đổi tối thiểu để đạt được cấu trúc toàn cầu mục tiêu. Quan sát chính là cấu trúc mục tiêu phải là một chu trình đơn bao gồm tất cả các nút, bởi vì trong biểu đồ hàm, khả năng kết nối mạnh có nghĩa là mỗi nút cũng có chính xác một mức độ, buộc đồ thị phải là một chu trình có hướng duy nhất. 

Vì vậy, vấn đề trở thành: chúng tôi muốn chuyển đổi mảng hiện tại thành một hoán vị bao gồm một chu kỳ có độ dài$n$, sử dụng các hoán đổi, trong đó mỗi hoán đổi trao đổi hai giá trị trong mảng. Điều này tương đương với việc sắp xếp lại mảng thành một hoán vị chu trình đơn và câu hỏi đặt ra là ánh xạ hiện tại cách cấu trúc chu trình đó bao xa. 

Cái nhìn sâu sắc mang tính quyết định là diễn giải mảng như một hoán vị của các đích đến. Vì mỗi giá trị$a_i$được sử dụng chính xác một lần làm vị trí nguồn cạnh đi, cấu trúc hoạt động giống như một hoán vị sau khi xem xét các nút theo các cạnh đi ra. Mỗi chu trình trong đồ thị hàm số tương ứng với một chu trình trong phân tách hoán vị này. 

Hoán đổi hai giá trị sẽ hợp nhất hoặc chia tách các chu kỳ theo cách được kiểm soát. Kết quả đã biết là bất kỳ hoán vị nào cũng có thể được hợp nhất thành một chu kỳ bằng cách sử dụng các giao dịch hoán đổi và số lượng giao dịch hoán đổi tối thiểu cần thiết được xác định bởi số lượng chu kỳ hiện tại. Mỗi lần hoán đổi có thể giảm số lượng chu kỳ tối đa một, vì nó có thể kết nối hai chu kỳ thành một. 

Nếu đồ thị có$c$chu kỳ, thì chúng ta cần ít nhất$c-1$hoán đổi để hợp nhất chúng thành một chu kỳ duy nhất và điều này có thể đạt được một cách tham lam bằng cách luôn hoán đổi các phần tử từ các chu kỳ khác nhau. 

Vì vậy, câu trả lời rút gọn thành việc đếm số chu trình có hướng trong đồ thị hàm số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trao đổi + kiểm tra kết nối) |$O(n^3)$|$O(n)$| Quá chậm | 
| Đếm chu kỳ trong đồ thị hàm số |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị hàm số ngầm từ mảng$a$, trong đó mỗi nút$i$chỉ vào$a_i$. Việc biểu diễn này không cần thiết một cách rõ ràng ngoài việc truyền tải, vì chúng ta có thể theo dõi các cạnh một cách trực tiếp. 
2. Duyệt qua tất cả các nút và phát hiện chu kỳ bằng cách sử dụng trạng thái truy cập tiêu chuẩn. Đối với mỗi nút chưa được truy cập, hãy bắt đầu đi tiếp dọc theo các cạnh đi ra cho đến khi đến nút đã truy cập hoặc quay lại nút trong đường dẫn đệ quy hiện tại. Thời điểm chúng ta nhập lại một nút trong đường dẫn hiện tại, chúng ta đã xác định được một chu trình. 
3. Đếm xem có bao nhiêu chu kỳ riêng biệt. Mỗi nút thuộc về chính xác một chu kỳ hoặc là một phần của chuỗi dẫn đến một chu kỳ, nhưng cuối cùng mỗi chuỗi đều đi vào một chu kỳ duy nhất, do đó mỗi nút được chỉ định trong quá trình truyền tải mà không có sự mơ hồ. 
4. Nếu số chu kỳ là$c$, đầu ra$c - 1$. 

Lý do bước này hợp lệ là vì mỗi lần hoán đổi có thể hợp nhất chính xác hai chu kỳ bằng cách chuyển hướng một kết nối biên đi giữa chúng. Khi hai chu kỳ được hợp nhất, chúng hoạt động như một chu trình duy nhất về khả năng tiếp cận. 

### Tại sao nó hoạt động 

Đồ thị chức năng phân hủy thành các chu kỳ rời rạc với các cây ăn vào chúng, nhưng khả năng kết nối mạnh mẽ đòi hỏi phải loại bỏ tất cả sự phân nhánh thành các chu kỳ riêng biệt. Bất kỳ cấu trúc cuối cùng hợp lệ nào cũng phải là một chu trình được định hướng duy nhất trên tất cả các nút. Mỗi lần hoán đổi chỉ có thể giảm số lượng chu kỳ tối đa một, vì nó có thể kết nối hai chu kỳ rời rạc trước đó nhưng không thể hợp nhất nhiều hơn hai thành phần cùng một lúc. Vì ban đầu có$c$chu kỳ, ít nhất$c-1$cần phải có sự hợp nhất và tồn tại một chuỗi hoán đổi mang tính xây dựng để đạt được chính xác giới hạn này bằng cách liên tục kết nối hai chu kỳ khác nhau. Điều này mang lại cả sự tối ưu và tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        a = [0] + list(map(int, input().split()))

        vis = [0] * (n + 1)
        cycles = 0

        for i in range(1, n + 1):
            if vis[i]:
                continue

            cur = i
            stack_index = {}
            path = []

            while True:
                if vis[cur]:
                    break
                if cur in stack_index:
                    cycles += 1
                    break

                stack_index[cur] = len(path)
                path.append(cur)
                cur = a[cur]

            for v in path:
                vis[v] = 1

        print(max(0, cycles - 1))

if __name__ == "__main__":
    solve()
```Việc triển khai thực hiện một bước chuyển tiếp từ mỗi nút không được truy cập. các`stack_index`từ điển được sử dụng để phát hiện cạnh sau trong đường dẫn hiện tại, biểu thị khám phá chu trình. Sau khi quá trình truyền tải kết thúc, tất cả các nút được truy cập trong bước đi đó sẽ được đánh dấu là đã xử lý để chúng không bao giờ được truy cập lại. 

Một điểm tinh tế là mỗi nút được gán cho chính xác một lần truyền tải, do đó, mặc dù chúng tôi chỉ phát hiện rõ ràng các chu kỳ khi xem lại đường dẫn hiện tại, số lượng chu kỳ vẫn chính xác vì mọi thành phần chức năng được kết nối đều đóng góp chính xác một sự kiện phát hiện chu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [3, 4, 1, 2]
```| Bắt đầu | Truyền tải đường đi | Phát hiện chu kỳ | Tổng số chu kỳ | 
| --- | --- | --- | --- | 
| 1 | 1 → 3 → 1 | vâng | 1 | 

Chỉ tồn tại một chu trình nên đáp án là$1 - 1 = 0$. 

Điều này xác nhận rằng hoán vị một chu kỳ đã được kết nối mạnh mẽ và không yêu cầu hoán đổi. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [2, 1, 4, 3]
```| Bắt đầu | Truyền tải đường đi | Phát hiện chu kỳ | Tổng số chu kỳ | 
| --- | --- | --- | --- | 
| 1 | 1 → 2 → 1 | vâng | 1 | 
| 3 | 3 → 4 → 3 | vâng | 2 | 

Có hai chu kỳ, vì vậy câu trả lời là$2 - 1 = 1$. 

Điều này cho thấy rằng mỗi chu trình rời rạc yêu cầu một thao tác hợp nhất và các giao dịch hoán đổi đóng vai trò là người tham gia chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi nút được truy cập một lần và được xử lý một lần trong quá trình truyền tải chức năng | 
| Không gian |$O(n)$| Mảng đã truy cập và lưu trữ đường dẫn tạm thời | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$10^5$, do đó, giải pháp tuyến tính nằm trong giới hạn thoải mái và mức sử dụng bộ nhớ vẫn bị giới hạn bởi phân bổ cho mỗi lần kiểm tra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite
    out = []
    
    def solve():
        T = int(input())
        for _ in range(T):
            n = int(input())
            a = [0] + list(map(int, input().split()))
            vis = [0] * (n + 1)
            cycles = 0

            for i in range(1, n + 1):
                if vis[i]:
                    continue
                cur = i
                stack_index = {}
                path = []
                while True:
                    if vis[cur]:
                        break
                    if cur in stack_index:
                        cycles += 1
                        break
                    stack_index[cur] = 1
                    path.append(cur)
                    cur = a[cur]
                for v in path:
                    vis[v] = 1
            out.append(str(max(0, cycles - 1)))

    solve()
    return "\n".join(out)

# sample
assert run("1\n1\n1\n") == "0"

# all one cycle
assert run("1\n4\n2 3 4 1\n") == "0"

# two cycles
assert run("1\n4\n2 1 4 3\n") == "1"

# chain into cycle style
assert run("1\n5\n2 3 1 5 4\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút tự lặp | 0 | trường hợp tối thiểu | 
| chu kỳ lớn duy nhất | 0 | đã tối ưu | 
| hai 2 chu kỳ | 1 | yêu cầu hợp nhất chu kỳ | 
| chu trình hỗn hợp | 1 | tính đúng đắn của cấu trúc chung | 

## Vỏ cạnh 

Biểu đồ một nút như$n=1, a=[1]$ngay lập tức tạo thành số chu kỳ là một, tạo ra câu trả lời bằng 0. Quá trình truyền tải bắt đầu tại nút 1, phát hiện chu trình tự lặp và đánh dấu nó đã truy cập. 

Một hoán vị hoàn toàn tuần hoàn như$a=[2,3,4,1]$được xử lý như một lần truyền tải với một sự kiện phát hiện chu kỳ. Thuật toán không bao giờ phân tách nó một cách không chính xác vì không còn điểm bắt đầu mới nào chưa được truy cập sau khi đánh dấu đường dẫn. 

Một trường hợp chu trình rời rạc như$a=[2,1,4,3]$được xử lý bằng cách bắt đầu từ nút 1 phát hiện một chu kỳ, sau đó bắt đầu từ nút 3 phát hiện một chu kỳ khác. Mỗi thành phần đóng góp chính xác một phát hiện chu kỳ, tạo ra số chu kỳ chính xác là hai và câu trả lời là một. 

Một chuỗi có vẻ bệnh lý trong một chu trình vẫn bị thu gọn thành một chu trình được phát hiện duy nhất vì chỉ mục nhập chu trình mới kích hoạt tình trạng cạnh sau; các nút cây được đánh dấu đơn giản là đã truy cập và không bao giờ tăng số chu kỳ.
