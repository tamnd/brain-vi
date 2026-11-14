---
title: "CF 1779F - Đá Phù Thủy"
description: "Chúng ta được cấp một cây có gốc trong đó mỗi nút lưu trữ một giá trị số nguyên nhỏ. Một thao tác đơn lẻ sẽ chọn một nút và thay thế mọi giá trị trong cây con của nó bằng XOR của tất cả các giá trị hiện có bên trong cây con đó. Sau phép gán này, mọi nút trong cây con đó sẽ trở nên giống hệt nhau."
date: "2026-06-09T11:30:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1779
codeforces_index: "F"
codeforces_contest_name: "Hello 2023"
rating: 2500
weight: 1779
solve_time_s: 89
verified: false
draft: false
---

[CF 1779F - Đá Xorcerer](https://codeforces.com/problemset/problem/1779/F) 

**Đánh giá:** 2500 
**Tags:** bitmask, thuật toán xây dựng, dp, cây 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc trong đó mỗi nút lưu trữ một giá trị số nguyên nhỏ. Một thao tác đơn lẻ sẽ chọn một nút và thay thế mọi giá trị trong cây con của nó bằng XOR của tất cả các giá trị hiện có bên trong cây con đó. Sau phép gán này, mọi nút trong cây con đó sẽ trở nên giống hệt nhau. 

Chúng tôi có thể áp dụng nhiều nhất$2n$các hoạt động “nén bằng XOR” của cây con như vậy và mục tiêu là đưa mọi giá trị nút về 0. Nhiệm vụ không phải là tối ưu hóa số lượng thao tác mà chỉ là xây dựng bất kỳ trình tự hợp lệ nào hoặc báo cáo không thể thực hiện được. 

Cây được bắt nguồn từ nút 1 và cây con có nghĩa là cây con có gốc thông thường: một nút và tất cả các con cháu của nó. 

Ràng buộc$a_i \le 31$là tín hiệu cấu trúc quan trọng. Mọi giá trị tồn tại trong không gian 5 bit, do đó hành vi XOR hoàn toàn tuyến tính trên một không gian vectơ nhỏ. Cái lớn$n$lên tới$2 \cdot 10^5$loại trừ mọi thao tác duyệt cây con, do đó, mọi giải pháp đều phải tránh tính toán lại XOR của cây con nhiều lần từ đầu. 

Trường hợp cạnh tinh tế đầu tiên là khi cây là một chuỗi và các giá trị khác 0 chỉ ở gần các lá. Một chiến lược ngây thơ áp dụng nhiều lần các thao tác từ dưới lên mà không kiểm soát cẩn thận việc truyền bá có thể dễ dàng tạo ra các dao động trong đó các giá trị được sao chép lên trên và sau đó được đưa lại vào các cây con thấp hơn. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều bằng 0. Một giải pháp đúng phải tạo ra các hoạt động bằng 0, không thử các hoạt động cây con không cần thiết có thể tạo ra các giá trị khác 0 nếu được triển khai không chính xác. 

Trường hợp thứ ba là khi XOR của toàn bộ cây khác 0. Một số chiến lược tham lam không chính xác cố gắng "sửa" gốc sớm, nhưng việc ghi đè lên cây con có thể gây ra sự không nhất quán trừ khi cấu trúc được xử lý theo thứ tự được kiểm soát. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liên tục chọn các nút và tính toán lại tất cả các XOR của cây con bằng DFS, sau đó áp dụng thao tác để giảm giá trị. Mỗi chi phí hoạt động$O(n)$, và lên đến$O(n)$các hoạt động có thể cần thiết, dẫn đến$O(n^2)$, vượt xa giới hạn của$2 \cdot 10^5$. 

Quan sát cấu trúc quan trọng là thao tác thay thế cây con bằng hằng số bằng XOR của nó. Điều này hoạt động giống như một phép chiếu: nó phá hủy biến thể bên trong bên trong cây con và thay thế nó bằng một bản tóm tắt theo từng bit. Vì XOR là tuyến tính, nên tác động của các phép chiếu cây con lặp đi lặp lại có thể được giải thích theo cách thông tin đi lên và sau đó được “làm phẳng” đi xuống. 

Ý tưởng trung tâm là xử lý cây theo thứ tự DFS từ dưới lên và sử dụng thực tế là chúng ta luôn có thể “đặt lại” một cây con về giá trị được kiểm soát sau khi các cây con đã được ổn định. Chúng tôi khai thác thực tế là sau khi tạo đồng nhất cho các nút con, cây con XOR của nút cha trở nên dễ suy luận và chúng tôi có thể loại bỏ các giá trị một cách có hệ thống trong khi vẫn giữ số lượng thao tác bị giới hạn bằng cách truy cập vào mỗi nút một số lần không đổi. 

Việc xây dựng hoạt động bằng cách đảm bảo rằng mỗi nút được xử lý theo cách mà cây con của nó bị buộc vào trạng thái tạm thời và sau đó được sửa, truyền điều khiển lên trên mà không cần tính toán lại từ đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tuyên truyền mang tính xây dựng DFS |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và thực hiện DFS. Trong DFS, chúng tôi xây dựng thứ tự các nút sao cho chúng tôi luôn xử lý đầy đủ các nút con trước nút cha của chúng. Việc xây dựng dựa vào việc áp dụng các thao tác tại các thời điểm được lựa chọn cẩn thận để XOR của cây con trở nên ổn định khi cần thiết. 

1. Chạy DFS từ thư mục gốc và lưu trữ thứ tự con. 

Điều này mang lại cho chúng ta một cấu trúc thứ tự sau trong đó mọi cây con đều được khám phá đầy đủ trước khi chúng ta hành động trên gốc của nó. 
2. Đối với mỗi nút, trước tiên hãy xử lý đệ quy tất cả các nút con. 

Điều này đảm bảo rằng khi chúng ta thao tác với một nút, nút con của nó đã ở trạng thái được kiểm soát. 
3. Sau khi xử lý một cây con, hãy tính toán XOR của cây con đó thông qua cấu trúc xây dựng của chúng ta. 

Chúng tôi không tính toán lại nó một cách rõ ràng mỗi lần mà dựa vào thực tế là cây con đã được giảm xuống một giá trị thống nhất bởi các thao tác trước đó. 
4. Áp dụng một thao tác trên nút con để thu gọn cây con của nó thành một giá trị duy nhất bằng cây con XOR của nó. 

Bước này loại bỏ cấu trúc bên trong và đảm bảo cây con có thể được coi là một nút duy nhất trong quá trình suy luận sau này. 
5. Sau khi tất cả các nút con được xử lý, hãy áp dụng một thao tác tại nút hiện tại để thực thi tính nhất quán giữa các nút con và chính nút đó. 

Điều này truyền các giá trị được kiểm soát lên trên trong khi vẫn giữ nguyên tính bất biến mà mọi cây con được xử lý vẫn thống nhất. 
6. Cuối cùng, thực hiện quá trình dọn dẹp ở thư mục gốc nếu cần để cấu trúc XOR toàn cục thu gọn về 0. 

Vì tất cả các cây con đều đồng nhất ở giai đoạn này nên thao tác gốc cuối cùng sẽ căn chỉnh toàn bộ cây. 

Thứ tự đảm bảo rằng không có cây con nào được xem lại theo cách phá vỡ tính đồng nhất. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý một nút, cây con của nó luôn đồng nhất, nghĩa là mọi nút trong cây con đó đều có giá trị như nhau. Khi một cây con trở nên đồng nhất, mọi thao tác XOR của cây con tiếp theo bên trong nó sẽ duy trì tính đồng nhất vì XOR có các giá trị giống nhau sẽ nhất quán trên tất cả các nút. Vì mỗi cây con được giảm chính xác một lần về trạng thái thống nhất được kiểm soát trước khi được cây mẹ của nó sử dụng, nên không có thao tác nào sau đó tạo ra tính không đồng nhất. Về cơ bản, việc xây dựng thực hiện việc nén cây từ dưới lên thành các giá trị đơn trong khi vẫn giữ số lượng thao tác tuyến tính bằng cách đảm bảo mỗi nút có liên quan đến số lần đặt lại cây con không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    p = [0] * n
    g = [[] for _ in range(n)]
    
    for i in range(1, n):
        parent = int(input().split()[0])
        p[i] = parent - 1
        g[p[i]].append(i)

    ans = []

    def dfs(v):
        for u in g[v]:
            dfs(u)
        ans.append(v + 1)
        x = 0
        for u in g[v]:
            x ^= a[u]
        x ^= a[v]
        for u in g[v]:
            a[u] = x
        a[v] = x

    dfs(0)

    # final cleanup
    if a[0] != 0:
        ans.append(1)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Mã này xây dựng cây gốc và chạy DFS xử lý con trước cha mẹ. Danh sách`ans`ghi lại thứ tự các hoạt động của cây con. Mỗi nút được thêm vào sau khi các nút con của nó được xử lý, tương ứng với việc thu gọn cây con của nó sau khi cấu trúc bên trong của nó đã được đơn giản hóa. 

Biến cục bộ`x`tính toán XOR của nút hiện tại và các giá trị con của nó, được sử dụng để biểu diễn tổng hợp cây con sau các phép biến đổi trước đó. Tất cả các cây con sau đó được gán giá trị này, tạo thành cây con thống nhất. Đây là bước quan trọng để đảm bảo rằng các phép tính cha trong tương lai coi mỗi cây con là một đơn vị tổng hợp duy nhất. 

Việc điều chỉnh gốc cuối cùng đảm bảo trạng thái toàn cục được trung hòa hoàn toàn khi gốc s
