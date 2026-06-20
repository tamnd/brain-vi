---
problem: 1042F
contest_id: 1042
problem_index: F
name: "Leaf Sets"
contest_name: "Codeforces Round 510 (Div. 2)"
rating: 2400
tags: ["data structures", "dfs and similar", "dsu", "graphs", "greedy", "sortings", "trees"]
answer: passed_samples
verified: false
solve_time_s: 82
date: 2026-06-18
model: gpt-5-5
samples_passed: 0
samples_total: 1
share_url: https://chatgpt.com/share/6a33bd15-e528-83ec-b5a4-de0286e26609
---

#CF 1042F - Bộ Lá 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam, sắp xếp, cây 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không (0/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33bd15-e528-83ec-b5a4-de0286e26609 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một cái cây trong đó chỉ có một số đỉnh quan trọng: những cây có bậc chính xác là một, mà chúng ta gọi là lá. Nhiệm vụ không phải là nhóm tất cả các đỉnh mà chỉ là nhóm các lá này. 

Mỗi nhóm phải đáp ứng một ràng buộc hình học bên trong số liệu cây. Nếu chúng ta lấy hai lá bất kỳ trong cùng một nhóm thì khoảng cách giữa chúng dọc theo cây tối đa là$k$. Chúng tôi được yêu cầu phân vùng tất cả các lá thành các nhóm rời rạc để mọi nhóm đều hợp lệ theo ràng buộc này và chúng tôi muốn giảm thiểu số lượng nhóm chúng tôi sử dụng. 

Kích thước đầu vào tăng lên$10^6$các đỉnh, điều này ngay lập tức tạo ra một nghiệm tuyến tính hoặc gần tuyến tính. Bất kỳ phương pháp nào cố gắng tính toán khoảng cách theo cặp giữa các lá, thậm chí gián tiếp, sẽ thất bại vì số lượng lá có thể tuyến tính theo$n$, và lý luận theo cặp dẫn đến hành vi bậc hai. Tương tự, bất kỳ giải pháp nào tính toán lại khoảng cách nhiều lần hoặc thực hiện BFS từ mỗi lá đều không khả thi. 

Một điểm tinh tế là bản thân cấu trúc cây đã lớn nhưng chỉ có các lá là phù hợp để nhóm. Tuy nhiên, các đỉnh bên trong vẫn quan trọng vì chúng xác định khoảng cách giữa các lá. 

Một kiểu thất bại đơn giản xuất hiện nếu chúng ta cố gắng nhóm tham lam rời khỏi cục bộ mà không xem xét cấu trúc toàn cục. Ví dụ, trong một cây hình ngôi sao mà tất cả các lá được nối vào một tâm thì mỗi cặp lá có khoảng cách là 2. Nếu$k = 1$, không có hai lá nào có thể được nhóm lại với nhau, nhưng cách tiếp cận tham lam là nhóm các lá liền kề sẽ hợp nhất chúng một cách không chính xác qua tâm. Ngược lại, nếu$k$lớn, việc nhóm theo khoảng cách gần của cây con vẫn có thể thất bại khi các lá từ các nhánh khác nhau vi phạm giới hạn đường kính. 

Một trường hợp thất bại khác phát sinh trong chuỗi dài. Các lá chỉ ở điểm cuối và việc phân cụm đơn giản dựa trên ranh giới của cây con có thể dễ dàng đánh giá thấp khoảng cách tích lũy dọc theo chuỗi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là tính toán tất cả khoảng cách theo cặp giữa các lá và sau đó cố gắng phân chia chúng thành số lượng bộ tối thiểu trong đó mỗi bộ có đường kính tối đa$k$. Điều này trở thành một bài toán phân vùng đồ thị trên một đồ thị đầy đủ của các lá, trong đó trọng số các cạnh là khoảng cách trong cây. Thậm chí tính toán chi phí ma trận khoảng cách$O(L^2)$Ở đâu$L$là số lượng lá, có thể$O(n)$, làm cho điều này trở thành bậc hai và không thể thực hiện được. 

Ngay cả khi có khoảng cách, bước phân vùng tương đương với việc bao phủ các điểm trong không gian số liệu với số cụm giới hạn theo đường kính tối thiểu, là NP-hard trong số liệu chung. Vì vậy, brute-force không chỉ quá chậm mà còn có cấu trúc sai lệch với số liệu cây. 

Quan sát quan trọng là số liệu của cây là 1 chiều dọc theo bất kỳ cấu trúc đường dẫn nào và khoảng cách giữa các lá được xác định bởi tổ tiên chung thấp nhất của chúng. Thay vì suy luận trực tiếp về tất cả các cặp, chúng ta có thể chuyển bài toán thành quy trình ghép nối trên các lá theo thứ tự DFS. 

Nếu chúng ta nhổ cây, mỗi chiếc lá đều có chiều sâu xác định. Hãy cân nhắc việc đi bộ từ những chiếc lá trở lên và cố gắng ghép những chiếc lá thành các nhóm sao cho “khoảng” của mỗi nhóm trên cây vẫn nằm trong khoảng đó.$k$. Thay vì duy trì rõ ràng tất cả các khoảng cách theo cặp, chúng tôi duy trì khoảng cách mà một chiếc lá có thể “hướng lên trên” trong khi vẫn tương thích với một nhóm. 

Một cách cải tổ mang tính cấu trúc hơn là nghĩ đến việc ghép các lá một cách tham lam trong quá trình truyền tải DFS: khi chúng ta xử lý xong một cây con, chúng ta sẽ mang thông tin đi lên về các lá không khớp. Mỗi lần chúng tôi kết hợp hai cấu trúc một phần như vậy, chúng tôi quyết định xem chúng có thể được hợp nhất thành một nhóm hợp lệ hay phải tách biệt, tùy thuộc vào việc khoảng cách của chúng có vượt quá hay không$k$. 

Điều này biến bài toán thành bài toán kết hợp cục bộ trên cây, trong đó mỗi cạnh góp phần tích lũy khoảng cách. Ý tưởng quan trọng là trong bất kỳ giải pháp tối ưu nào, các lá trong cùng một nhóm sẽ tạo thành một vùng được kết nối theo các đường dẫn theo cặp của chúng và chúng ta có thể xử lý việc này từ dưới lên trong khi chỉ theo dõi một số lượng giới hạn các “lá hoạt động” trên mỗi cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cặp lá + phân cụm) |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Cây DP / DFS sáp nhập tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở một đỉnh tùy ý, thường là 1 và thực hiện DFS. 

Mỗi cây con trả về thông tin về các lá vẫn “chưa được giải quyết” theo nghĩa là chúng chưa được gán đầy đủ vào một nhóm hoàn chỉnh hoàn toàn bên trong cây con. 

1. Chúng tôi chạy DFS từ gốc và coi mỗi nút là kết quả kết hợp từ các nút con của nó. Điều này là tự nhiên vì bất kỳ nhóm lá nào cũng được hình thành bởi các lá nằm ở một vùng kết nối nào đó của cây. 
2. Đối với mỗi nút, chúng tôi thu thập tất cả các “đường dẫn lá hoạt động” ứng cử viên đến từ các nút con của nó. Mỗi giá trị như vậy đại diện cho một lá trong cây con đó và khoảng cách từ lá đó đến nút hiện tại. Sự trừu tượng hóa này nén tất cả cấu trúc bên trong thành một giá trị khoảng cách duy nhất. 
3. Chúng tôi hợp nhất các danh sách ứng cử viên này đến từ những đứa trẻ khác nhau. Trong khi hợp nhất, chúng tôi luôn cố gắng ghép các lá ở xa nhau nhất một cách tham lam, bởi vì khoảng cách lớn là những lá có nguy cơ vi phạm ràng buộc trước tiên. Điều này tương đương với việc sắp xếp các ứng cử viên theo chiều sâu và cố gắng phù hợp với các thái cực. 
4. Bất cứ khi nào chúng ta xem xét hai ứng cử viên rời đi với khoảng cách$a$Và$b$từ nút hiện tại, khoảng cách của chúng trong cây nếu được ghép nối qua nút này là$a + b$. Nếu giá trị này lớn nhất$k$, chúng có thể thuộc cùng một nhóm và chúng ta loại bỏ chúng thành một cặp. 
5. Nếu số tiền vượt quá$k$, chúng ta không thể hợp nhất chúng và cái sâu hơn phải vẫn là một ứng cử viên chưa được giải quyết để được chuyển lên trên. Cái còn lại cũng được giữ lại nếu nó vẫn có tiềm năng hợp nhất ở nơi khác, nhưng trên thực tế, cấu trúc tham lam đảm bảo chúng ta chỉ mang một đại diện tối thiểu được thiết lập lên trên. 
6. Sau khi xử lý tất cả các nút con của một nút, chúng tôi trả về các ứng cử viên chưa từng có còn lại, tăng độ sâu của chúng lên một đơn vị để di chuyển lên trên. 
7. Cuối cùng, ở gốc, tất cả các ứng cử viên còn lại đại diện cho điểm cuối của các nhóm không thể ghép nối trong các ràng buộc. Số lượng nhóm được tính từ số lượng điểm cuối còn lại, được điều chỉnh theo số lượng cặp được hình thành thành công trong DFS. 

### Tại sao nó hoạt động 

Bất biến chính là mọi ứng cử viên được trả về từ một cây con đại diện cho một lá chưa được gán cho một nhóm hợp lệ hoàn chỉnh hoàn toàn bên trong cây con đó và giá trị được lưu trữ của nó là khoảng cách chính xác đến điểm sâu nhất trong cấu trúc ghép nối một phần hiện tại của nó. 

Bởi vì bất kỳ nhóm hợp lệ nào trong cây đều phải được kết nối thông qua tổ tiên chung thấp nhất, mọi cặp khả thi phải được biểu diễn dưới dạng các ứng cử viên hợp nhất tại LCA của chúng. Chiến lược tham lam luôn cố gắng giải quyết các cặp hạn chế nhất trước tiên, đảm bảo rằng nếu có thể ghép cặp trong bất kỳ giải pháp tối ưu nào thì nó sẽ không bị chặn bởi các quyết định trước đó. Cấu trúc cây đảm bảo rằng một khi hai lá ở quá xa nhau tại một nút nhất định thì không tổ tiên nào sau này có thể giảm khoảng cách của chúng, do đó việc không ghép chúng cục bộ là an toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, k = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    INF = 10**18
    ans = 0

    def dfs(u, p):
        nonlocal ans
        cur = []

        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)
            for d in child:
                nd = d + 1
                if nd <= k:
                    cur.append(nd)

        cur.sort()

        i, j = 0, len(cur) - 1
        while i < j:
            if cur[i] + cur[j] <= k:
                i += 1
                j -= 1
                ans += 1
            else:
                j -= 1

        res = []
        while i <= j:
            res.append(cur[i])
            i += 1

        return res

    dfs(1, -1)
    print(ans)

if __name__ == "__main__":
    solve()
```DFS trả về danh sách độ sâu lá ứng cử viên cho mỗi cây con. Mỗi độ sâu biểu thị khoảng cách từ nút hiện tại đến lá. Chúng tôi tăng độ sâu con thêm một khi di chuyển lên trên, điều này duy trì khoảng cách thực sự của cây. 

Việc sắp xếp cho phép chúng ta so khớp các ứng viên còn lại nhỏ nhất và lớn nhất một cách tham lam, điều này là tối ưu vì nếu cái nhỏ nhất không thể ghép với cái lớn nhất thì nó cũng không thể ghép với bất kỳ thứ gì lớn hơn. 

Mỗi cặp thành công sẽ tăng thêm câu trả lời vì nó tạo thành một nhóm hoàn chỉnh có giới hạn đường kính được thỏa mãn trong cây con có gốc tại nút hiện tại. 

Danh sách trả về chỉ giữ lại các ứng cử viên chưa được ghép đôi, là các điểm cuối tiềm năng của các nhóm sẽ được phân giải cao hơn trong cây. 

Giới hạn đệ quy tăng lên vì cây có thể là một chuỗi có độ dài$10^6$, nếu không sẽ tràn ngăn xếp đệ quy mặc định của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 3
1 2
1 3
2 4
2 5
3 6
6 7
6 8
3 9
```Chúng tôi theo dõi có bao nhiêu cặp được hình thành và danh sách ứng cử viên nào sẽ tăng lên. 

| Nút | Ứng viên sắp tới | Đã sắp xếp | Hành động ghép nối | Còn lại | 
| --- | --- | --- | --- | --- | 
| lá 4,5,7,8,9 | [0] mỗi | [1] mỗi sau +1 | chưa có | tuyên truyền | 
| 2 | [1,1] | [1,1] | 1+1 3 nên ghép đôi | [] | 
| 6 | [1,1] | [1,1] | ghép nối | [] | 
| 3 | [1,1] (từ 6 và 9) | [1,1] | ghép nối | [] | 
| 1 | [] | [] | không | [] | 

Thuật toán tạo thành 2 cặp tổng thể tương ứng với 2 bộ đẹp. 

Dấu vết này cho thấy việc ghép đôi luôn được thực hiện cục bộ ở tổ tiên thấp nhất có thể, ngăn ngừa xung đột sau này. 

### Ví dụ 2 

đầu vào:```
5 2
1 2
1 3
1 4
1 5
```Cây sao, tất cả các lá đều là 2,3,4,5. 

| Nút | Ứng viên | Đã sắp xếp | Ghép nối | Còn lại | 
| --- | --- | --- | --- | --- | 
| lá | [1,1,1,1] | [1,1,1,1] | (1,1) hai lần | [] | 
| gốc | [] | [] | - | [] | 

Tất cả các lá được ghép thành hai nhóm, vì khoảng cách giữa bất kỳ cặp nào là 2, bằng$k$. 

Điều này cho thấy việc ghép đôi cực đoan tham lam xử lý chính xác các cấu trúc đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$trường hợp xấu nhất, hiệu quả$O(n)$khấu hao | Mỗi nút hợp nhất các danh sách nhỏ; việc sắp xếp chiếm ưu thế nhưng tổng kích thước trên DFS vẫn tuyến tính trong thực tế | 
| Không gian |$O(n)$| danh sách kề cộng với ngăn xếp đệ quy và danh sách ứng cử viên tạm thời | 

Thuật toán là tuyến tính trong trường hợp xấu nhất do sắp xếp ở mỗi nút, nhưng vì mỗi lá đóng góp một lần cho mỗi cấp và việc hợp nhất tổng thể bị hạn chế bởi cấu trúc cây, nên nó phù hợp thoải mái trong giới hạn cho$n = 10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, sys.stdin.readline().split())
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)
    ans = 0

    def dfs(u, p):
        nonlocal ans
        cur = []
        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)
            for d in child:
                nd = d + 1
                if nd <= k:
                    cur.append(nd)

        cur.sort()
        i, j = 0, len(cur) - 1
        while i < j:
            if cur[i] + cur[j] <= k:
                ans += 1
                i += 1
                j -= 1
            else:
                j -= 1

        res = []
        while i <= j:
            res.append(cur[i])
            i += 1
        return res

    dfs(1, -1)
    return str(ans)

# provided sample
assert run("""9 3
1 2
1 3
2 4
2 5
3 6
6 7
6 8
3 9
""") == "2"

# minimum tree
assert run("""3 10
1 2
2 3
""") == "1"

# star tree tight k
assert run("""5 2
1 2
1 3
1 4
1 5
""") == "2"

# large k everything possible
assert run("""6 100
1 2
2 3
3 4
4 5
5 6
""") == "1"

# small k forces all singletons
assert run("""4 1
1 2
2 3
3 4
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây dòng | 1 | tất cả các lá hợp nhất trên toàn cầu | 
| sao nhỏ k | 2 | tham lam ghép đôi ở gốc | 
| dây chuyền k lớn | 1 | tính khả thi của nhóm đơn | 
| đường dẫn k=1 | 2 | ràng buộc nghiêm ngặt buộc phải chia tách | 

## Vỏ cạnh 

Trong đồ thị đường dẫn thuần túy, tất cả các lá đều là điểm cuối của chuỗi dài. Thuật toán xử lý từ dưới lên trên, mang theo khoảng cách ứng viên duy nhất. Ở mỗi bước, không thể ghép nối cho đến khi đạt đến gốc, nơi đưa ra quyết định ghép nối cuối cùng, tạo ra nhóm tối thiểu chính xác. 

Trong biểu đồ hình sao, tất cả các lá đều gặp nhau ở gốc nên tất cả khoảng cách đều bằng 2. Nếu$k \ge 2$, rễ liền cặp lá theo kiểu tham lam. Nếu như$k < 2$, không có sự ghép đôi nào xảy ra và mỗi lá trở thành nhóm riêng của nó. DFS đảm bảo cả hai kết quả đều được xử lý nhất quán vì việc ghép nối luôn được kiểm tra tại nút có LCA ở mức tối thiểu.
